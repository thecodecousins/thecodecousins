---
title: 'Contribution Guide to TheCodeCousins'
description: 'Contribution Guide to TheCodeCousins'
keywords: ['blog', 'software', 'software-engineering', 'technology', 'code']
---

First of all, thank you for sharing your ideas, knowledge, opinions 🎉🎉 You are awesome and we love you 😍😍

❤️🧡💛💚💙💜🖤

The following is a set of guidelines for contributing to our blog.
We tried to be as comprehensive as possible, but if you feel that there is more to this, please feel free to propose changes to this in a pull request.

### Table of Content

> [Contribute as an author](#author)
>
> [Contribute as a translator](#translator)
>
> [Bonus: Fancy author field](#bonus)
>
> [Doubts and questions](#question)

### <a name="author" id="author"></a> I. Contribute as an author

Step-by-step guide for your new post to be on [TheCodeCousins](https://thecodecousins.com)

##### Setting up local env

Start by forking [this repository](https://github.com/thecodecousins/thecodecousins) and then clone it recursively to your local workspace

```bash
git clone https://github.com/YOUR-USERNAME/thecodecousins --recursive
cd thecodecousins/
```

Check if you have [hugo](https://gohugo.io) installed by running `hugo version`.
If you haven't already installed [hugo](https://gohugo.io), please follow the instruction on [their site](https://gohugo.io/getting-started/installing/).

##### Start a new post

Generate a new post from our theme template by running

```bash
hugo new posts/YOUR-POST-NAME/index.md # we will explain why index.md in the next section
```

Fill in all the necessary fields:

- title: try to keep it short and captivating.
- date: must be in [ISO-8601 format](https://en.wikipedia.org/wiki/ISO_8601), [this timestamp generator](https://timestampgenerator.com/) might come handy for such.
- author: your full/pen name. For a fancier post author, head down to [bonus section](#bonus)
- cover: image that will be shown in preview and at the top of your post
- tags: topics of your post
- keywords: same as tags but can be more specific
- description: a short summary of your post to be shown in preview
- showFullContent: please keep this as false to share blog's estate with other authors

Write your blog post in markdown with an enhanced flavor from [hugo](https://gohugo.io/).
If you are intending to do more than just standard mardown (md), checkout [hugo docs](https://gohugo.io/content-management/) to learn more.

##### Static assets

We intend to use leaf bundle organisation for all posts on [TheCodeCousins](https://thecodecousins.com), which is the reason why you were asked to create a folder with your post name and an `index.md` for post content.
Your final product with some static assets would look something like this

![Example source tree](/example-tree.png)

Then your assets can be included in the post using the full path with `content` folder as root
(i.e. `/posts/test-posts/img/cover.jpg` is the needed path to include the cover image).

##### Letting us know 🎉🎉

After you are done with your post, please open a [Pull Request](https://github.com/thecodecousins/thecodecousins/compare) labelled `post` on our repository and your post will be up in no time. 🥳🥳

### <a name="translator" id="translator"></a> II. Contribute as a translator

Step-by-step to make your contribution by translating someone else's post on [TheCodeCousins](https://thecodecousins.com)

##### Seeking permission from original author

Open [an issue](https://github.com/thecodecousins/thecodecousins/issues/new) labelled `translation` with a link in description to the original post on [our repo](https://github.com/thecodecousins/thecodecousins).
We will connect with the original author and help you obtain their permission 👌👌.

##### Setting up local env

Start by forking this repository and then clone it recursively to your local workspace

```bash
git clone https://github.com/YOUR-USERNAME/thecodecousins --recursive
cd thecodecousins/
```

Check if you have [hugo](https://gohugo.io) installed by running `hugo version`.
If you haven't already installed [hugo](https://gohugo.io), please follow the instruction on [their site](https://gohugo.io/getting-started/installing/).

##### Create the post in target language

Identify the post's ID by locating what the containing folder name is (i.e. if the path to markdown file is `/content/posts/example/index.md` then the post's ID is `example`).
Create the post in target language with the following command after identifying target language's key from [our config file](https://github.com/thecodecousins/thecodecousins/blob/master/config.yaml) (i.e. `vi` for Viet)

```bash
# For English as target language, there's no need for language key
# as English is the default language
hugo new posts/<POST-ID>/index.<LANGUAGE-KEY>.md
```

Fill in all the necessary fields:

- title: translation from original post
- date: must be in [ISO-8601 format](https://en.wikipedia.org/wiki/ISO_8601), [this timestamp generator](https://timestampgenerator.com/) might come handy for such.
- author: your full/pen name. For a fancier post author, head down to [bonus section](#bonus)
- cover: same as original post's
- tags: same as or translated from original post's
- keywords: same as or translated from original post's
- description: translation from original post
- showFullContent: please keep this as false to share blog's estate with other authors

Start the post with link to original post and attribution to the original author.
For example, your translated post can be started with "This post was translated from [our example post](/posts/example) by Stanley Nguyen".

Write your blog post in markdown with an enhanced flavor from [hugo](https://gohugo.io/).
If you are intending to do more than just standard mardown (md), checkout [hugo docs](https://gohugo.io/content-management/) to learn more.

##### Letting us know 🎉🎉

After you are done with your post, please open a [Pull Request](https://github.com/thecodecousins/thecodecousins/compare) labelled `post` and `translation` on our repository and your post will be up in no time. 🥳🥳

### <a name="bonus" id="bonus"></a> III. Bonus: Author field with metadata

This section is for anyone who's looking for a fancier `Author` field at the top of their post instead of just a plain string.

Create an author data for yourself, we recommend using your Github handle as an identifier for consistency and to avoid collision among our contributors

```bash
touch data/authors/YOUR-USERNAME.yml
```

There are 3 fields that you can provide in your author data file:

- name: self-explanatory 😁😆
- url: link to your online profile
- bio: short intro of yourself, please keep it under 50 characters

Use the corresponding identifier as the `author` field for your post. (i.e., my author data file is `stanleynguyen.yml` then my post's `author` field should be `stanleynguyen`)

### <a name="question" id="question"></a> IV. Doubts and questions

Please shoot us [a new issue](https://github.com/thecodecousins/thecodecousins/issues/new) labelled `question` on [our repo](https://github.com/thecodecousins/thecodecousins).
