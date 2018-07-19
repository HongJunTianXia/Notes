iterm2

oh my zsh  https://github.com/robbyrussell/oh-my-zsh

https://github.com/ahmetb/kubectx

https://github.com/jonmosco/kube-ps1

https://github.com/powerline/fonts

vim ~/.zshrc
```
ZSH_THEME=“agnoster”
DEFAULT_USER=“wtihstars”     # 增加这一项，可以隐藏掉路径前面那串用户名

plugins=(git brew node npm scp git cp extract kubectl)   # 自己按需把要用的 plugin 写上

source <(kubectl completion zsh)
source /usr/local/Cellar/kube-ps1/0.6.0/share/kube-ps1.sh
PROMPT=‘$(kube_ps1)’$PROMPT
```


步骤：https://www.jianshu.com/p/9c3439cc3bdb
