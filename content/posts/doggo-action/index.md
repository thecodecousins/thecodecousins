+++
title = 'Building my Github Action to deliver doggoes to PRs'
date = 2020-03-07T16:08:42.903Z
authors = ["stanleynguyen"]
tags = ["doggo", "dog", "doge", "Github", "action", "continuous delivery", "software", "devops"]
cover = '/posts/doggo-action/images/dog.gif'
keywords = ["dog", "doggo", "doge", "github", "actions", "continuous delivery"]
description = "Because who doesn't want good boys to show up whenever they push?"
showFullContent = false
+++

The long awaited GitHub Action feature is finally out of beta and ready to be present
in every repositories. GitHub even organized [a Hackathon](https://githubhackathon.com)
through out March to encourage folks to create more awesome and useful actions.
While browsing through the submissions, I found [a cool GitHub Action](https://github.com/ruairidhwm/action-cats)
that posts cat gifs on pull requests. Shout out to [Ruairidh](https://ruairidh.dev) with his cool
idea 👏👏👏.

It'll only do justice to dogs if there's an action that deliver them
good boys to our PRs. That moment I knew exactly what my next Github Action project would be. Time to get to work.

![Doggo get to work](/posts/doggo-action/images/get2work.gif)

### The gist of creating a GitHub Action

[GitHub actions](https://help.github.com/en/actions/getting-started-with-github-actions/about-github-actions)
are basically premade (with ❤️) units of work to be used GitHub workflows (think [Travis](https://travis-ci.org/)'s builds).
Github actions can be either built with [Docker containers](https://help.github.com/en/actions/building-actions/creating-a-docker-container-action)
or [JS/TS scripts](https://help.github.com/en/actions/building-actions/creating-a-javascript-action).
An advantage to creating a GitHub action with JS/TS is readily available modules
from [GitHub toolkit](https://github.com/actions/toolkit). With such integration
support, it's much easier to connect with GitHub services (C'mon 🙄 Who wanna write
`curl` commands to make API calls). It's obvious to go with [TypeScript action template](https://github.com/actions/typescript-action).

With that decision made, let's get down to writing [action-dogs](https://github.com/stanleynguyen/action-dogs).

### The main run file

In a JS/TS GitHub action, workload will be started out from a main entrypoint (think
running `node src/index.js` to start a Node process for web applications, etc).
For action-dogs, this is my basic setup for the main program

```typescript
import * as core from "@actions/core";
import * as github from "@actions/github";
import { generate } from "./doggo/generator";

(async function run(): Promise<void> {
  try {
    const ctx = github.context;
    if (!ctx.payload.pull_request) {
      throw new Error("Not in the context of a PR!");
    }

    const ghCli = new github.GitHub(core.getInput("github-token"));
    const doggo = generate();
    ghCli.issues.createComment({
      ...ctx.repo,
      issue_number: ctx.payload.pull_request.number,
      body: `![Doggo](${doggo})`
    });
  } catch (e) {
    core.setFailed(e.message);
  }
})();
```

During an event that can trigger GitHub workflows, we are provided with a context
object that can be accessed through `@actions/github` module. Using that, I'm able
to check whether my payload is coming from a `pull_request` and reject otherwise.
Next up, I need to post a comment to the corresponding pull request with content
of a doggo gif. Given that my doggo generator (of which I will explain in the
next section) works properly, I can retrieve an URL of a doggo gif, creating a
comment on pull requests is super simple as I just need to pass in the repo's information
from our context object and the PR's number. Also, in case we get any errors during
these operations, calling `core.setFailed(e.message)` will mark the build as failed
with the error message.

### The doggo generator

After much research with the intention to use one of the public APIs to get random
doggo gifs, I couldn't find one that's public (like [dog.ceo](https://dog.ceo))
and also serves gifs (like [GIPHY](https://giphy.com)). Since there's no avenue
for me to store my GIPHY API key securely to be used in action-dogs, I fell back
to the good-ol-way of static JSON array.

Wanna know how I obtained my array full of doggo awesomeness (from ❤️ [GIPHY](https://giphy.com) ❤️) without any API key generated in the process?
I actually went to GIPHY site, searched for doggoes, and scrolled down util a good
amount of "giffy-boys" generated before pulling up my console

![Inspect GIPHY](/posts/doggo-action/images/inspect.png)

And with these few lines of JS

```js
const dogsData = [];
document
  .querySelectorAll("a._2SwDiFPqIlZmUDkxHNOeqU")
  .forEach(e => dogsData.push(e.href));
var dataStr =
  "data:text/json;charset=utf-8," +
  encodeURIComponent(JSON.stringify(dogsData));
var dlAnchorElem = document.createElement("a");
dlAnchorElem.setAttribute("href", dataStr);
dlAnchorElem.setAttribute("download", "dogs.json");
dlAnchorElem.click();
```

which basically grab `href` values from all "copy link" elements on the search
results page, stream them to a JSON array and fill in a file for me to "download", "generating" is simply picking a random URL from the array.

```typescript
import dogs from "./dogs.json";

export function generate(): string {
  return dogs[Math.floor(Math.random() * dogs.length)];
}
```

### Testing

I wrote a unit test for my doggo generator using jest (but actually mainly as an avenue for
funny descriptions).

```ts
import { generate } from "../../src/doggo/generator";

describe("doggo generator", () => {
  test("to return a good boy", () => {
    Math.random = jest.fn().mockReturnValue(0);
    const good = "https://media3.giphy.com/media/mCRJDo24UvJMA/giphy.gif";
    const boy = generate();
    expect(boy).toBe(good);
  });
});
```

But the real test is a workflow using `action-dogs` itself (Yes, you can use
a GitHub action on its own repo 🤯).

```yaml
name: "doggo"

on: pull_request

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: stanleynguyen/action-dogs@master
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

Now I can see `action-dogs` in action with [a sample PR](https://github.com/stanleynguyen/action-dogs/pull/1). Hurray 🙌🙌🙌!! Now I can safely [publish it on GitHub Marketplace](https://help.github.com/en/actions/building-actions/publishing-actions-in-github-marketplace).

### Outtro

So that's my story of creating `action-dogs` for fun and learning. You can find
source code right on [GitHub](https://github.com/stanleynguyen/action-dogs) (well,
cos where else could it be 🤷‍♂️) and `action-dogs` on [Marketplace](https://github.com/marketplace/actions/action-dogs).
