# Gambits and exploits in software engineering

Because I keep forgetting them, I decided to write them down.

## How to write new content locally

- Clone this repo
- `git submodule init && git submodule update`
- Install [Hugo](https://gohugo.io/getting-started/quick-start/)
- `hugo server -D`

## Content cheat sheet

- Public static content goes into `static/`
- Post specific content goes into `content/posts/`

## Custom theme

- I forked the excellent [hugo-coder](https://github.com/luizdepra/hugo-coder) theme, as I wanted to get rid of all the bloat I didn't need.
- Updating the theme can be done by
  - cloning the fork repo
  - making the needed changes
  - commit & push
  - update the submodule in this repo

## Hosting

- Content is hosted on Github pages, and Github actions will automatically build and deploy the site on every push to the `master` branch, or manually.
- The integration likes to break every so often, when in doubt, check breaking changes to the CI 
