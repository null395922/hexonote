# Publish GitHub pages with GitHub Actions | itsopensource
Publish GitHub pages with GitHub Actions | itsopensource

### [itsopensource](/)

# Publish GitHub pages with GitHub Actions

December 12, 2020

[automation](/tags/automation)[ci](/tags/ci)[github-actions](/tags/github-actions)

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAKCAYAAAC0VX7mAAAACXBIWXMAAAsSAAALEgHS3X78AAAA7ElEQVQoz52RTU/CQBCG+f9H/wMXL0aO3riYcFETDOCBhEDbpcXtx66C0jKPWxAhpTTgJJuZnXnzzLvZFmdCZJc3crjvz/G8Gq0mWGCg8waz7ACXyoKLgHtXTwHc9OBZ1S+82uG6gHYf7p3LngcPYxgsTt1eDCzzyxzuRtCduDyEfngFUH4VZd5sa2EQCbevwuNUGLo6MLt+OT/Wn3UoNWvHKXjug9Iv+Fw362sd5kWB1jFxkqLmEVmiyb9XrveOtRYdp26WsFyumh3+AfN8C9JxwtTzmfkKYz/wVUhmLCqMCFxtHPxfT26Kqv4H/RMQxJsK1cIAAAAASUVORK5CYII=)
![](https://itsopensource.com/static/42143b9a1f8f163ccc6f87d27cc83674/ee604/github-actions.png)

GitHub pages are the best way to host static blogs like `Gatsby`. One of the most common ways to do this is, maintain your code in main/master branch, build it, and then push the code to `gh-pages` branch. There are various CI that easily automate this process like Travis CI, CircleCI, etc. With GitHub actions, this would be a piece of cake, and without depending on any third-party provider. From the docs:

> Automate, customize, and execute your software development workflows right in your repository with GitHub Actions

If you are not sure what are GitHub actions please visit [here](https://github.com/features/actions).

Workflow

* * *

#### Prerequisites

1.  You have a static blog(let’s say gatsby) setup, the code for your blog is in master branch.
2.  You have a build script in your package.json
3.  You have setup your GitHub pages (or pointed to your custom domain) in `gh-pages` branch.

#### Process

**Step 1**

Install [gh-pages npm package](https://www.npmjs.com/package/gh-pages) in your project, this is a small utility package which helps to publish your code to gh-pages branch. You can skip this step in case you want to push all contents from master to `gh-pages` branch.

**Step 2**

Add a deploy script in your `package.json`. This script should do 2 jobs

1.  Build the project, make it ready for being published.
2.  Push the changes to the `gh-pages` branch.

For example, this pushes the code to `gh-pages` via npm package.

    "deploy": "gatsby build && gh-pages -d public -r https://$GH_TOKEN@github.com/<username>/<repository_name>.git"

**Step 3**

Generate an access token and add it to secret of your repository.

1.  Create an access token: goto [https://github.com/settings/tokens](https://github.com/settings/tokens), create a new token, give it access for repo and workflow.

    [![](https://itsopensource.com/static/30aebc71ae3bcb4ee8e555ba47aa0951/fcda8/action1.png)](/static/30aebc71ae3bcb4ee8e555ba47aa0951/7527b/action1.png) 

2.  Add this token to secret of your repository: goto `https://github.com/<username>/<repository_name>/settings/secrets/actions`, click `new repository secret`.

    [![](https://itsopensource.com/static/9109d16686926974e4becbfa4d63c8cd/fcda8/action2.png)](/static/9109d16686926974e4becbfa4d63c8cd/bf433/action2.png) 

**Step 4**

1.  Create a workflow file, you need to create a file in following path: `.github/workflows/<some-name-you-like>.yml`, its important to have `.yml` extension and have exact same path.
2.  Following action file is complete enough to publish `gh-pages`, every time a new commit is merged in master branch. ✅

        name: gh-pages publisher 🚀

        on:
        push:
          branches: [ master ]

        jobs:
        publish-gh-pages:
          runs-on: ubuntu-latest
          steps:
             - uses: actions/checkout@v2
             - uses: actions/setup-node@v1
             with:
                node-version: 12
             - run: npm ci
             - run: git config user.name "<user name>" && git config user.email "user email"
             - run: npm run deploy
             env:
                GH_TOKEN: ${{secrets.SECRET_GITHUB_TOKEN}}

This script is pretty self-explanatory, it performs the following tasks.

-   It specifies to work only on `push` on `master`
-   Sets up a `node 12` environment
-   Installs dependencies via `npm ci`
-   Sets up git requirements for username and email (required to do a push to branch)
-   Run the deploy script
-   In case you don’t use gh-pages npm package, you can write another step for git push to gh-pages branch.
-   At last, we set up env variable `GH_TOKEN` from our action secret (which you set up in step 3), this env variable would be available in `package.json`

**Step 5**

Commit this file and see your first `action` in action (sorry for the pun 🙈)

* * *

Hope this helps 🙏🙏🙏

* * *

-   You can see the actions running here: `https://github.com/<username>/<repository_name>/actions`.
-   Workflow file for [itsopensource.com](https://itsopensource.com) can be viewed [here](https://github.com/tsl143/itsopensource/blob/master/.github/workflows/gh-pages-publish.yml).

* * *

[![](https://itsopensource.com/trishul.jpg)
](/author/trishul)

**Trishul Goel** is a professional frontend developer; writes React code for living and volunteers for Mozilla to justify his existence.  
[@trishulgoel](https://twitter.com/trishulgoel)

-   [← Creating accordions with native HTML](/creating-accordions-with-native-html/)
-   [ReadabilityJS - adding Reader View Mode to websites →](/readabilityjs-adding-reader-view/) 
    [https://itsopensource.com/publish-github-pages-with-github-actions/](https://itsopensource.com/publish-github-pages-with-github-actions/)
