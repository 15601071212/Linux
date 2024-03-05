## 使用Crontab定时执行任务
### 1. 修改用户路径下的.bashrc文件，指定用vim作为crontab的编辑器
```bash
root@zdh-web-00:~# cd ~
root@zdh-web-00:~# vi .bashrc
```
```bash
# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias rs2='service nginx restart'
alias rs='systemctl restart server_uwsgi.service'
alias sp='netstat -lnp| grep 8001'
alias sp2='netstat -lnp| grep 7070'
alias rsc='cd /var/www/LRM && ./reload_service.sh'
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
#if [ -f /etc/bash_completion ] && ! shopt -oq posix; then
#    . /etc/bash_completion
#fi
alias redis-server=/usr/bin/redis-server
```
> 添加下面这行内容
```bash
export EDITOR="/usr/bin/vim"
```
> 执行source命令使修改生效
```bash
root@zdh-web-00:~# source .bashrc
```
### 2. 编辑crontab添加任务及执行时间
> 用下面的命令编辑crontab的内容:
```bash
root@zdh-web-00:~# crontab -e
```
> 用下面的命令检查crontab的内容，*/10 * * * * 表示每10分钟执行一次后面指定的任务
```bash
root@zdh-web-00:~# crontab -l
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
*/10 * * * * cd /var/www/LRM && python3 manage.py shell < /var/www/LRM/import_keyword_and_update_doc_html.py >> /var/www/LRM/import_keyword_and_update_doc_html.log
*/10 * * * * cd /var/www/LRM && python3 manage.py shell < /var/www/LRM/page_view_chart.py
#*/5 * * * * /usr/bin/python3 /var/www/LRM/get_keywords_test.py >> /var/www/LRM/get_keywords_test.log
root@zdh-web-00:~#
```
