# My DotFiles settings

## Setup ZSH
Install Instructions at:
https://github.com/sorin-ionescu/prezto

Add the following lines to your `.zshrc` file:
```
autoload -Uz promptinit
promptinit
prompt pure
```

## Setup Z script for path jumper, by saving z file to the home directory:
`wget https://raw.githubusercontent.com/rupa/z/master/z.sh -O ~/z.sh`

And than, add Z script as source at your `.zshrc` file. Open it and add the
following line at the end of the file.
`source ~/z.sh`

## Install SCM BREEZE:
```
git clone git://github.com/ndbroadbent/scm_breeze.git ~/.scm_breeze
```
```
~/.scm_breeze/install.sh
```

Add the following line to your `.zshrc`:
```
[ -s "$HOME/.scm_breeze/scm_breeze.sh" ] && source "$HOME/.scm_breeze/scm_breeze.sh"
```

## Setup FZF (https://github.com/junegunn/fzf):
```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
```
```
~/.fzf/install
```

Add the following lines to the `.zshrc` file:
```
#
# FZF Options
#

export FZF_DEFAULT_OPTS='--reverse --extended --tabstop=2 --margin 1'
export FZF_DEFAULT_COMMAND='ag --hidden --follow --ignore .git -g ""'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_CTRL_T_OPTS='--preview "[ -f "{}" ] && pygmentize -g {}" --bind ?:toggle-preview'
```

## Setup SILVER SEARCHER (https://github.com/ggreer/the_silver_searcher)
```
apt-get install silversearcher-ag
```

## Install Pygments:
```
pip install Pygments
```

Add the following lines to the `.zshrc` file:
```
fco() {
  local tags branches target
  tags=$(
    git tag | awk '{print "\x1b[31;1mtag\x1b[m\t" $1}') || return
  branches=$(
    git branch --all | grep -v HEAD             |
    sed "s/.* //"    | sed "s#remotes/[^/]*/##" |
    sort -u          | awk '{print "\x1b[34;1mbranch\x1b[m\t" $1}') || return
  target=$(
    (echo "$tags"; echo "$branches") |
    fzf-tmux -- --no-hscroll --ansi +m -d "\t" -n 2) || return
  git checkout $(echo "$target" | awk '{print $2}')
}
```
