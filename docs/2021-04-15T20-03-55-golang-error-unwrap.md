---
date: 2021-04-15T20:03:55-07:00
title: Golang Error Unwrap
tags: Go
---

# Golang Error Unwrap

During a code review, my colleague mentioned about `%w` format directive in `fmt.Errorf()` . Googled about that and reliazed this thing called `error.Unwrap() `

```go
package main

import (
	"errors"
	"fmt"
	"log"
	"strings"
)

type parentError struct{}

func (e parentError) Error() string {
	return "Error parentError"
}

type child1Error struct{}

func (e child1Error) Error() string {
	return "Error major child1"
}

type child2Error struct{}

func (e child2Error) Error() string {
	return "Error minor child2"
}

func main() {
	child2 := child2Error{}
	parent := fmt.Errorf("parent: %w", child2)
	grandparent := fmt.Errorf("grandparent: %w", parent)
	if err := checkErrorLevel(grandparent); err != nil {
		log.Printf("Kid, please don't repeat it %v", err)
	}

	child1 := child1Error{}
	parent = fmt.Errorf("parent: %w", child1)
	grandparent = fmt.Errorf("grandparent: %w", parent)
	checkErrorLevel(grandparent)
	if err := checkErrorLevel(grandparent); err != nil {
		log.Printf("Kid, please don't repeat it %v", err)
	}
}

func checkErrorLevel(err error) error {
	if unWrapedErr := errors.Unwrap(err); unWrapedErr != nil {
		if strings.Contains(unWrapedErr.Error(), "minor") {
			return nil
		} else {
			return err
		}
	}
	return nil
}
```

parentError{} wraps child1Error{} and child2Error{} which in turn wrapped by grandparent error. 

In checkErrorLevel() we Unwrap it to examine the error to check whether its `minor` or not. If its minor we return nil, if not the error itself. `Unwrap` is possible only if the error is wrapped in the first place using `%w` in fmt.Errorf(). 

```
vigneshravichandran@localhost go-til % go run error-wrapping/main.go
2021/04/15 20:47:22 Kid, please don't repeat it grandparent: parent: Error major child1
```

changing the format directive to %v and running the same returns no output. Because, unwrapping isn't happening.

Source code: https://github.com/viggy28/go-til/blob/main/error-wrapping/main.go

