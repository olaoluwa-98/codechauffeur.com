# Code Chauffeur
A website containing articles authored by Emmanuel Awotunde. Visit [codechauffeur.com](http://codechauffeur.com).

## Setup
- Install [Hugo](https://gohugo.io/getting-started/installing).
- Clone repo `git clone --recurse-submodules https://github.com/olaoluwa-98/codechauffeur.com`.
- Run Hugo server `hugo server -D`.

## Deploy on GitHub Pages
- Run `git worktree add -B gh-pages public origin/gh-pages` once.
- Run `chmod +x ./deploy.sh && ./deploy.sh`.

## Update theme to latest
- Run `git submodule update --recursive --remote`.