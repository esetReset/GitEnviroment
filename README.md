# Git
* Open Git bash
* Start generate keys
```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
* Start ssh agent
```
$ eval "$(ssh-agent -s)"
```
* Add private key
```
$ ssh-add ~/.ssh/name of key
```
* For automatic start of agent open ~/.ssh/.bash_profile and add following lines
```
#/bin/bash 
unset TZ 
env=~/.ssh/agent.env
agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env 
```
* To store your keys permanetly in the agent create ~/.ssh/config and put this line for each key (for example key with name git)
```
IdentityFile ~/.ssh/git
```