+++
title = "Example Blog Post"
date = 2017-03-03T14:15:59-06:00
authors = ["stanleynguyen"]
cover = "/posts/example/img/cover.png"
tags = ["example", "blog"]
keywords = ["example", "blog"]
description = "This is an example blog post"
showFullContent = false
+++

### Section 1

This is some example content

##### Sub section 1

This is some content for subsection 1

##### Test Code highlight

Go code with hugo highlight template

{{< highlight go >}}
a := "some string"
fmt.Println("lol")
{{</ highlight >}}

Go code with triple backtick

```go
a := "some string"
fmt.Println("lol")
```

JS code with hugo highlight template

{{< highlight js >}}
const a = "some string"
console.log("lol")
const b = () => { return 1; };
{{</ highlight >}}

JS code with triple backtick

```js
const a = 'some string';
console.log('lol');
const b = () => {
  return 1;
};
```

Let's agree to use triple backtick unless line number is needed
