---
title: "How I escaped from Microsoft Office tools as a Computer Science student"
date: 2019-10-09T00:35:20+07:00
draft: true
author: "Do Hoang"
tags: ["pandoc", "presentation", "blog"]
cover: "/posts/pandoc/images/test.png"
keywords: ["pandoc"]
description: "The Easiest Way to Make Presentations! (Pandoc + Markdown)"
showFullContent: false
---

## Introduction
need smt here

## Why Pandoc For Presentation?

**Presentation shouldn’t get in the way of content.**

- With a word processor, you spend valuable time agonizing over
what font size to make the section headings.

    &rarr; With *Pandoc*, you just tell it to start a new section.

- With a word processor, changing the formatting means you have to
change each instance individually.

    &rarr; With *Pandoc*, you just redefine the relevant commands

- With a word processor, you have to carefully match any provided
templates.

    &rarr; With *Pandoc*, you can be sure you’ve fit the template, and switch
templates easily.

## Installation Guide

**Integrated Software**

Thus we want to make our slide looks like *Latex* typesets so a Basic Latex packages is required. \
I strongly recommend not just installing the TeX compiler, but *all* TeX packages for your own convenience, so you have everything offline already. This will be a big download at first, but nowadays the storage space should be minimal.

**On GNU/Linux:**

On Debian-based distros (Ubuntu/Linux Mint):

```bash
sudo apt-get install texlive-full
```

For Arch-based distros (Manjaro, Parabola, Antergos):

```bash
sudo pacman -S texlive-most texlive-lang
```

**On Windows**

Download and install the packages *[here](https://miktex.org/download/#collapse264)*.

**On MacOS**

Download and install the packages *[here](https://tug.org/mactex/)*. Or via Brew:
```bash
brew cask install mactex
```

After installed Latex, the next step is to get Pandoc - a universal document converter.

**On GNU/Linux**

For each linux distribution, you might have the different pandoc package's name in your package manager. For example in ArchLinux, we can install pandoc with:
```bash
sudo pacman -S pandoc
```
But if you want to get the latest version ( you might want to check whether the pandoc version in your package manager is not outdated ), we can get the binary package for amd64 architecture on the *[download page](https://github.com/jgm/pandoc/releases/tag/2.7.3)*.

**On MacOS**
```bash
brew install pandoc
```

**On Windows**

You can get the latest version on the *[dowload page](https://github.com/jgm/pandoc/releases/tag/2.7.3)*.

## Getting started 

![test](/posts/pandoc/images/test.png)


