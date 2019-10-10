+++
title = "Test Blog Post"
date = 2017-03-03T14:15:59-06:00
author = "stanleynguyen"
cover = "/posts/test-post/img/cover.jpg"
tags = ["test", "blog"]
keywords = ["test", "blog"]
description = "This is a test blog post"
showFullContent = false
+++

### Section 1

This is some test content

##### Sub section 1

This is some content for subsection 1

##### Test Code highlight

{{< highlight go >}}
a := "some string"
fmt.Println("lol")
{{</ highlight >}}

```go
a := "some string"
fmt.Println("lol")
```

{{< highlight js >}}
const a = "some string"
console.log("lol")
const b = () => { return 1; };
{{</ highlight >}}

```js
const a = 'some string';
console.log('lol');
const b = () => {
  return 1;
};
```

Let's agree to use triple backtick unless line number is needed
