## Tmux快捷键

### Server

* 启动Server

`tmux start-server`

### Session

* 创建Session

`tmux new -t {session_name}`

* 退出Session

`Ctrl + b d`

* 删除Session

`Ctrl + b:kill-session`

* 删除所有Session

`Ctrl + b:kill-Server`
`tmux kill-session -a`

* 列出Session

`tmux ls`

### Windows

* 创建Window

`Ctrl + b c`

* 删除Window

`Ctrl + b x`

* 下一个Window

`Ctrl + b n`

* 上一个Window

`Ctrl + b p`

* 重命名Window

`Ctrl + b ,`

* 在多个Window里搜索

`Ctrl + b f`



