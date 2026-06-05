# jefflinse.io

Hugo sources (content, styles, etc.) for my personal website, [jefflinse.io](https://jefflinse.io).

Built with [Hugo](https://gohugo.io/) (extended) and the [hugo-coder](https://github.com/luizdepra/hugo-coder) theme (a git submodule).

## Setup

```sh
brew install hugo
git submodule update --init --recursive   # pulls the theme
```

## Local development

```sh
hugo server -D        # preview at http://localhost:1313 (-D includes drafts)
```

## Writing a post

```sh
hugo new content posts/my-post-title.md   # title auto-fills from the filename
```

Edit the file, then set `draft = false` (or delete the line) when it's ready. See
`archetypes/posts.md` for the front-matter template.

## Deploy

**Push to `main` and it deploys itself.** The [`Deploy`](.github/workflows/deploy.yml)
GitHub Action builds the site and pushes the generated output to the
[jefflinse.github.io](https://github.com/jefflinse/jefflinse.github.io) Pages
repo, which serves `jefflinse.io`. You can also trigger it manually from the
Actions tab (`workflow_dispatch`).

How the auth works: an SSH **deploy key** has its public half registered (with
write access) on the Pages repo, and its private half stored as the
`ACTIONS_DEPLOY_KEY` secret on this repo.

The custom domain is kept alive by `static/CNAME` (copied into every build), so a
deploy can never strip it.

### Manual deploy (rarely needed)

`public/` is a git submodule pointing at the Pages repo, for building locally:

```sh
hugo --gc --minify -d public
git -C public add -A && git -C public commit -m "Publish" && git -C public push
```
