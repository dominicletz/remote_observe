# remote_observe -- Remote connect to your Elixir/Erlang VM 

![remote_observe demo](https://raw.githubusercontent.com/dominicletz/remote_observe/master/demo.gif)

## SYNOPSIS

remote_observe is a bash script to do the neccesary
setup steps to connect a local Elixir/Erlang instance to
a remote machine to which you have SSH access to.

```remote_observe <ssh_server_url> (node_name)```

### Optional Environment Variables
  
* `COOKIE_HOME` - Set the path to the erlang cookie if not in the home directory
* `COOKIE` - Set the remote cookie  directly
* `ERL_EPMD_PORT` - Define an alternative EPMD port


## Installation
Mark the script `remote_observe` as executable and move it somewhere in your $PATH for convinience:

```
wget https://raw.githubusercontent.com/dominicletz/remote_observe/master/remote_observe
chmod +x ./remote_observe
sudo mv ./remote_observe /usr/local/bin
```

## Notes

- [Compatible VM](#compatible-vm)
- [Access to the Cookie](#access-to-the-cookie)
- [Elixir / Erlang](#elixir-/-erlang)
- [EPMD Port](#epmd-port)

### Compatible VM
You need to have a local installed Elixir VM that is compatible with the remote Elixir VM that you have 

### Access to the Cookie
To connect to a remote VM `remote_observe` tries to determine the secret cookie. It assumes to be in the home directory of the SSH user.

1) If the cookie is in a different directory you can use the environment variable `COOKIE_HOME` to provide an alternative path as in the demo.

1) If you have/know the cooke you can provide the cookie directly via the environment variable `COOKIE`

Example: `COOKIE=secret remote_observer myserver.com`


### Elixir / Erlang
If you're not an Elixir user you can just comment in the bash script the iex line and instead uncomment the erl line. Pull request to make this configurable are welcome 

### EPMD Port
The default epmd port is 4369. Unfortunately the remote epmd port and locally mapped epmd port need to be identical. So in the default setup `remote_observe` will kill any locally running epmd service before creating the SSH tunnel, so that the Elixir/Erlang remote shell is opened against the remote node and not locally.
This 