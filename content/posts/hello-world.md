+++
title = "Hello World"
date = 2026-05-31T17:12:45-07:00
draft = true
description = "Back to writing."
tags = ["meta"]
categories = []
series = []
+++

It's been a while. This is a starter draft — edit or delete it.

## Writing a new post

```sh
hugo new content posts/my-post-title.md   # title auto-fills from the filename
hugo server -D                            # preview, including drafts, at localhost:1313
```

Set `draft = false` (or remove the line) in the front matter when it's ready to publish.

## Markdown reference

Inline `code`, **bold**, *italic*, and [links](https://jefflinse.io).

```go
func main() {
    fmt.Println("syntax highlighting works")
}
```

> Blockquotes look like this.

That's it — happy writing.
