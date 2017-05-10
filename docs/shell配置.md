# Mac 系统Shell的配置

### 查看系统支持哪几种shell

```cat /etc/Shells```

![Shell种类](https://cvws.icloud-content.com/B/AS0zZCamm1h2pIpwzl-8yHQ-BRoIAZQ_mxJpwVevgYeYYYIJj74djixi/1.png?o=AoA2K51YCCRdwf5EcZX5vOelV9xbRonxqHU8cDJDIs9q&v=1&x=3&a=BS9Lf4IrufjCA7sSOQ&e=1486724806&k=viyJyFSYaHLU1I2gjjMDLw&fl=&r=9fba02e7-6798-46bf-803c-ba068da89bce-1&ckc=com.apple.clouddocs&ckz=com.apple.CloudDocs&p=26&s=pZXECr-QTwM3FBi3lsH8PP26jg8&cd=i)

### 安装zsh

**Red Hat Linux*

```sudo yum install zsh```

**Ubuntu Linux**

```sudo apt-get install zsh```

**Mac系统自带了ZSH**

### 安装oh-my-zsh

* Via Curl

```sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"```

* Via wget

```sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"```

### 修改配置文件

``` cd ~/.zshrc

配置用到的插件，主题和环境变量


### 牛b的自动补全

[官网](http://mimosa-pudica.net/zsh-incremental.html)

```
# Incremental completion for zsh
# by y.fujii <y-fujii at mimosa-pudica.net>, public domain


autoload -U compinit
zle -N self-insert self-insert-incr
zle -N vi-cmd-mode-incr
zle -N vi-backward-delete-char-incr
zle -N backward-delete-char-incr
zle -N expand-or-complete-prefix-incr
compinit

bindkey -M viins '^[' vi-cmd-mode-incr
bindkey -M viins '^h' vi-backward-delete-char-incr
bindkey -M viins '^?' vi-backward-delete-char-incr
bindkey -M viins '^i' expand-or-complete-prefix-incr
bindkey -M emacs '^h' backward-delete-char-incr
bindkey -M emacs '^?' backward-delete-char-incr
bindkey -M emacs '^i' expand-or-complete-prefix-incr

unsetopt automenu
compdef -d scp
compdef -d tar
compdef -d make
compdef -d java
compdef -d svn
compdef -d cvs

# TODO:
#     cp dir/

now_predict=0

function limit-completion
{
	if ((compstate[nmatches] <= 1)); then
		zle -M ""
	elif ((compstate[list_lines] > 6)); then
		compstate[list]=""
		zle -M "too many matches."
	fi
}

function correct-prediction
{
	if ((now_predict == 1)); then
		if [[ "$BUFFER" != "$buffer_prd" ]] || ((CURSOR != cursor_org)); then
			now_predict=0
		fi
	fi
}

function remove-prediction
{
	if ((now_predict == 1)); then
		BUFFER="$buffer_org"
		now_predict=0
	fi
}

function show-prediction
{
	# assert(now_predict == 0)
	if
		((PENDING == 0)) &&
		((CURSOR > 1)) &&
		[[ "$PREBUFFER" == "" ]] &&
		[[ "$BUFFER[CURSOR]" != " " ]]
	then
		cursor_org="$CURSOR"
		buffer_org="$BUFFER"
		comppostfuncs=(limit-completion)
		zle complete-word
		cursor_prd="$CURSOR"
		buffer_prd="$BUFFER"
		if [[ "$buffer_org[1,cursor_org]" == "$buffer_prd[1,cursor_org]" ]]; then
			CURSOR="$cursor_org"
			if [[ "$buffer_org" != "$buffer_prd" ]] || ((cursor_org != cursor_prd)); then
				now_predict=1
			fi
		else
			BUFFER="$buffer_org"
			CURSOR="$cursor_org"
		fi
		echo -n "\e[32m"
	else
		zle -M ""
	fi
}

function preexec
{
	echo -n "\e[39m"
}

function vi-cmd-mode-incr
{
	correct-prediction
	remove-prediction
	zle vi-cmd-mode
}

function self-insert-incr
{
	correct-prediction
	remove-prediction
	if zle .self-insert; then
		show-prediction
	fi
}

function vi-backward-delete-char-incr
{
	correct-prediction
	remove-prediction
	if zle vi-backward-delete-char; then
		show-prediction
	fi
}

function backward-delete-char-incr
{
	correct-prediction
	remove-prediction
	if zle backward-delete-char; then
		show-prediction
	fi
}

function expand-or-complete-prefix-incr
{
	correct-prediction
	if ((now_predict == 1)); then
		CURSOR="$cursor_prd"
		now_predict=0
		comppostfuncs=(limit-completion)
		zle list-choices
	else
		remove-prediction
		zle expand-or-complete-prefix
	fi
}
```

保存脚本内容到本地

```source incr.zsh```
