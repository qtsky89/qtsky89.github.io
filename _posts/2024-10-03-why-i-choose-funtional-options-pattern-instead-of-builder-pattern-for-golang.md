---
title: Why I choose Funtional options pattern instead of Builder pattern for Golang
date: 2024-10-03 15:00:00 +0900
categories: [design pattern]
tags: [functional options pattern, builder pattern, golang, design pattern]
author: wonhee
toc: true
comments: true
---

## Summary

When I need to make complex types in any programming language,

I typically turn to the Builder pattern to simplify the process.

However, when trying to implement the Builder pattern in Golang,

I face some error handling challenges due to Go’s lack of a try-catch mechanism.

In this document, I’ll explain why I chose the Functional options pattern over the Builder pattern for Golang.

## Simple struct

Let’s imagine a simple Golang struct.

```go
type Document struct {
    Id string
    Title string
    Body string
}
```

I can create an instance of Document like this.

```go
func newDocument(id int, title string, body string) Document {
    return Document {
        Id: id,
        Title: title,
        Body: body,
    }
}

func main() {
    d := newDocument(1, "test title", "test body")
    fmt.Printf("%+v\n", d)
}
```

```shell
$ go run main.go
{Id:1 Title:test title Body: test body}
```

I don't need to use complex design pattern.

I believe that simple code is the best code, and I apply design patterns when they're necessary.

Here's the full code at this point.

```go
package main

import (
    "fmt"
)

type Document struct {
    Id string
    Title string
    Body string
}

func newDocument(id int, title string, body string) Document {
    return Document {
        Id: id,
        Title: title,
        Body: body,
    }
}

func main() {
    d := newDocument(1, "test title", "test body")
    fmt.Printf("%+v\n", d)
}
```

## Complex struct

But what if I need to add a new field to the struct? Like this.

```diff
type Document struct {
    Id string
    Title string
    Body string

+   LikeCount int
}
```

It's not practical to use what I created at a certain point.

## Builder pattern

I could use the Builder pattern to create the Document struct.

First, let's define the builder interface.

A builder interface typically consists of multiple Set functions and a Build function.

```go
type DocumentBuilder interface {
	SetId(id int) DocumentBuilder
	SetTitle(title string) DocumentBuilder
	SetBody(body string) DocumentBuilder
	SetLikeCount(likeCount int) DocumentBuilder

	Build() Document
}
```

Next, I need to create a struct that implements this Builder interface.

```go
type docBuilder struct {
	doc Document
}

func (b *docBuilder) SetId(id int) DocumentBuilder {
	b.doc.Id = id
	return b
}

func (b *docBuilder) SetTitle(title string) DocumentBuilder {
	b.doc.Title = title
	return b
}

func (b *docBuilder) SetBody(body string) DocumentBuilder {
	b.doc.Body = body
	return b
}

func (b *docBuilder) SetLikeCount(likeCount int) DocumentBuilder {
	b.doc.LikeCount = likeCount
	return b
}

func (b *docBuilder) Build() Document {
	return b.doc
}
```

then, I can make interface factory function.

```go
func NewDocumentBuilder() DocumentBuilder {
	return &docBuilder{}
}
```

Now I can use all the code like this.

```go
func main() {
	b := NewDocumentBuilder()
	b.SetId(1).SetTitle("test title").SetBody("test body").SetLikeCount(10)
	d := b.Build()
	fmt.Printf("%+v\n", d)
}
```

Let's run the code.

```shell
$ go run main.go
{Id:1 Title:test title Body:test body LikeCount:10}
```

It's working like a charm.

## Builder pattern error handling

The Builder pattern works great in Golang,

but what if I need to handle errors in each Set function?

Let's add an error-handling feature, like ensuring the id isn't a negative number.

```diff
type DocumentBuilder interface {
	SetId(id int) DocumentBuilder
	SetTitle(title string) DocumentBuilder
	SetBody(body string) DocumentBuilder
	SetLikeCount(likeCount int) DocumentBuilder

+	Error() error
	Build() Document
}

type docBuilder struct {
	doc Document
+	err error
}

+func (b *docBuilder) Error() error {
+	return b.err
+}

func (b *docBuilder) SetId(id int) DocumentBuilder {
+	if id < 0 {
+		b.err = fmt.Errorf("id shouldn't be negative. got=%d", id)
+		return b
+	}

	b.doc.Id = id

	return b
}
```

Then, I can use it like this.

```diff
func main() {
	b := NewDocumentBuilder()
-	b.SetId(1).SetTitle("test title").SetBody("test body").SetLikeCount(10)
+	b.SetId(-1).SetTitle("test title").SetBody("test body").SetLikeCount(10)

+	if b.Error() != nil {
+		fmt.Printf("%+v\n", b.Error())
+		return
+	}

	d := b.Build()
	fmt.Printf("%+v\n", d)
}
```

```shell
$ go run main.go
id shouldn't be negative. got=-1
```

It works, but the only issue is that I can't stop the other Set functions, even if b.SetId encounters an error.

Languages like JavaScript, Java, and Python don't have this problem 

because they use try-catch blocks, which immediately stop execution when an error occurs.

I could add error-handling code to each setter function like this.

```diff
func (b *docBuilder) SetId(id int) DocumentBuilder {
+	if b.err != nil {
+		return b
+	}

	if id < 0 {
		b.err = fmt.Errorf("id shouldn't be negative. got=%d", id)
		return b
	}

	b.doc.Id = id

	return b
}

func (b *docBuilder) SetTitle(title string) DocumentBuilder {
+	if b.err != nil {
+		return b
+	}

	b.doc.Title = title
	return b
}

func (b *docBuilder) SetBody(body string) DocumentBuilder {
+	if b.err != nil {
+		return b
+	}

	b.doc.Body = body
	return b
}
```

It works, but it doesn't look clean in the code.

Here’s the full code at this point.

```go
package main

import (
	"fmt"
)

type DocumentBuilder interface {
	SetId(id int) DocumentBuilder
	SetTitle(title string) DocumentBuilder
	SetBody(body string) DocumentBuilder
	SetLikeCount(likeCount int) DocumentBuilder

	Error() error
	Build() Document
}

type docBuilder struct {
	doc Document
	err error
}

func (b *docBuilder) Error() error {
	return b.err
}

func (b *docBuilder) SetId(id int) DocumentBuilder {
	if b.err != nil {
		return b
	}

	if id < 0 {
		b.err = fmt.Errorf("id shouldn't be negative. got=%d", id)
		return b
	}

	b.doc.Id = id

	return b
}

func (b *docBuilder) SetTitle(title string) DocumentBuilder {
	if b.err != nil {
		return b
	}

	b.doc.Title = title
	return b
}

func (b *docBuilder) SetBody(body string) DocumentBuilder {
	if b.err != nil {
		return b
	}

	b.doc.Body = body
	return b
}

func (b *docBuilder) SetLikeCount(likeCount int) DocumentBuilder {
	if b.err != nil {
		return b
	}

	b.doc.LikeCount = likeCount
	return b
}

func (b *docBuilder) Build() Document {
	return b.doc
}

func NewDocumentBuilder() DocumentBuilder {
	return &docBuilder{}
}

type Document struct {
	Id        int
	Title     string
	Body      string
	LikeCount int
}

func main() {
	b := NewDocumentBuilder()
	b.SetId(-1).SetTitle("test title").SetBody("test body").SetLikeCount(10)

	if b.Error() != nil {
		fmt.Printf("%+v\n", b.Error())
		return
	}

	d := b.Build()
	fmt.Printf("%+v\n", d)
}
```

## Functional options pattern

Instead of using the Builder pattern to create a complex struct,

I can use the Functional options pattern instead.

I can pass a bunch of function types as parameters to set the internal fields of the Document struct.

First, let's create a custom Option interface and a simple factory function.

```go
type Option func(*Document) error

func NewDocument(opts ...Option) (*Document, error) {
	ret := Document{}
	for _, opt := range opts {
		err := opt(&ret)
		if err != nil {
			return nil, err
		}
	}
	return &ret, nil
}
```

All I need to do is pass a bunch of Functional options as parameters.

Let’s create the Functional options first.

I’ll add error handling only in the WithId function for comparison.

```go
func WithId(id int) Option {
	return func(d *Document) error {
		if id < 0 {
			return fmt.Errorf("id shouldn't be negative. got=%d", id)
		}
		d.Id = id
		return nil
	}
}

func WithTitle(title string) Option {
	return func(d *Document) error {
		d.Title = title
		return nil
	}
}

func WithBody(body string) Option {
	return func(d *Document) error {
		d.Body = body
		return nil
	}
}

func WithLikeCount(likeCount int) Option {
	return func(d *Document) error {
		d.LikeCount = likeCount
		return nil
	}
}
```

Then, I can use it like this.

```go
func main() {
	d, err := NewDocument(WithId(-1), WithTitle("test title"), WithBody("test body"), WithLikeCount(10))

	if err != nil {
		fmt.Printf("%+v\n", err)
		return
	}

	fmt.Printf("%+v\n", d)
}
```

Let's run this.

```shell
$ go run main.go
id shouldn't be negative. got=-1
```

Even though I pass multiple Functional options,

the execution stops immediately and returns an error when one occurs, thanks to this logic.

```diff
func NewDocument(opts ...Option) (*Document, error) {
	ret := Document{}
	for _, opt := range opts {
		err := opt(&ret)
+		if err != nil {
+			return nil, err
+		}
	}
	return &ret, nil
}
```

Here's the full code at this point.

```go
package main

import (
	"fmt"
)

type Document struct {
	Id        int
	Title     string
	Body      string
	LikeCount int
}

type Option func(*Document) error

func NewDocument(opts ...Option) (*Document, error) {
	ret := Document{}
	for _, opt := range opts {
		err := opt(&ret)
		if err != nil {
			return nil, err
		}
	}
	return &ret, nil
}

func WithId(id int) Option {
	return func(d *Document) error {
		if id < 0 {
			return fmt.Errorf("id shouldn't be negative. got=%d", id)
		}
		d.Id = id
		return nil
	}
}

func WithTitle(title string) Option {
	return func(d *Document) error {
		d.Title = title
		return nil
	}
}

func WithBody(body string) Option {
	return func(d *Document) error {
		d.Body = body
		return nil
	}
}

func WithLikeCount(likeCount int) Option {
	return func(d *Document) error {
		d.LikeCount = likeCount
		return nil
	}
}

func main() {
	d, err := NewDocument(WithId(-1), WithTitle("test title"), WithBody("test body"), WithLikeCount(10))

	if err != nil {
		fmt.Printf("%+v\n", err)
		return
	}

	fmt.Printf("%+v\n", d)
}
```

## Conclusion
Both the Builder pattern and the Functional options pattern are great for creating complex types.

However, with the Builder pattern, due to Go's lack of a try-catch mechanism, you either have to go through unwanted setter functions or add extra error-handling code to avoid this.

On the other hand, the Functional options pattern can immediately return an error, which is why I prefer using it for creating complex types in Go.