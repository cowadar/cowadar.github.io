## Handige commando's
### Multiple Git pull
Indien je in meerdere onderliggende folders git repo's hebt zitten, kan je met dit commando alle git repos laten pullen.

Dankzij [[parallel]] zal dit niet tegelijkertijd uitgevoerd worden, maar op meerdere tasks in parallel.

```bash
find . -maxdepth 8 -name '.git' -prune -type d -printf '%h\n' | parallel --eta 'echo {} && git -C {} pull'
```

## Git alias
```bash
function gc () {
	git commit -m "$*"
}

alias ga='git add'
alias gp='git push'
alias gl='git log'
alias gs='git status'
alias gd='git diff'
alias gdc='git diff --cached'
alias gb='git branch'
alias gra='git remote add'
alias grr='git remote rm'
alias gpu='git pull'
alias gcl='git clone'
```

## Submodules
## Git clone
```bash
git clone --recurse-submodules git@github.com:havenstadrp/resources
```

### Git pull alle subfolders
```bash
find . -maxdepth 8 -name '.git' -prune -type d -printf '%h\n' | parallel --eta 'echo {} && git -C {} pull'
```

## Config
### File permissions
```bash
git config core.filemode false

```
### User "login"
```bash
git config --global user.email "bedar89@gmail.com"
git config --global user.name "bedar89"
```
