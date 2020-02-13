# Run Observer on a remote node with SSH

remote_observe runs an Observer on a remote node deployed
with as release with Distillery, manual with iex or any other
way. The script automates the steps to do port discovery and 
redirection. Also it uses erlang DNS to support `--sname` deployed
instances. 

You only have to ensure that in your mix.exs you've added the runtime_tools in your deployment: 

```
  # Run "mix help compile.app" to learn about applications.
  def application do
    [
      mod: {My.Application, []},
      extra_applications: [:runtime_tools]
    ]
  end
```

![remote_observe demo](https://raw.githubusercontent.com/dominicletz/remote_observe/master/demo.gif)

## SYNOPSIS

```
Usage: remote_observe <ssh_server_url> (-options) (<node_name>)

  <name_name> is only required if there is more than one node
              running on the remote machine.

Options:

  -e                Start an erlang shell instead of an elixir shell
  -l                Use long names instead of default short names
  -c <cookie>       Set the cookie value
  -h <cookie_home>  Looks for the cookie in <cookie_home>/.erlang.cookie
                    on the remote node
  -p <cookie_path>  Looks for the cookie in <cookie_path> on the remote
                    node

Examples:
  remote_observe -c secret dev.myserver.com
  remote_observe -p /opt/app/releases/COOKIE dev.myserver.com
```

### Optional Environment Variables
  
* `ERL_EPMD_PORT` - Define an alternative EPMD port


## Installation
Mark the script `remote_observe` as executable and move it somewhere in your $PATH for convinience:

```
wget https://raw.githubusercontent.com/dominicletz/remote_observe/master/remote_observe
chmod +x ./remote_observe
sudo mv ./remote_observe /usr/local/bin
```

## Notes

- [Releases with Runtime tools](#releases-with-runtime-tools)
- [Compatible VM](#compatible-vm)
- [Access to the Cookie](#access-to-the-cookie)
- [Short and long names](#short-and-long-names)
- [EPMD Port](#epmd-port)
- [MacOS](#macos)

### Releases with Runtime tools
In order to run the observer tool locally, observing the remote as in the demo your release needs to include the runtime tools. You can [read more about this here](https://tkowal.wordpress.com/2016/04/23/observer-in-erlangelixir-release/) and about [enabling distribution in releases here](https://elixirforum.com/t/remote-observer-connection-issues/26315/3)

### Compatible VM
You need to have a local installed Elixir VM that is compatible with the remote Elixir VM that you have 

### Access to the Cookie
To connect to a remote VM `remote_observe` tries to determine the secret cookie. It assumes to be in the home directory of the SSH user.

1) If you are running a release the cookie can be in the releases/COOKIE subdirectory of your app. In that case provide the full cookie path using the -p <cookie_path> option.

1) If the cookie is in a different home directory you can use the parameter -h <cookie_home> to provide an alternative home directory. 

1) If you know the cookie you can provide the cookie directly via the option -c <cookie_value>

### Short and long names
Currently the script is only using `-sname`. Even though there is an -l option this is currently not functional. __TODO__ 

### EPMD Port
The default epmd port is 4369. Unfortunately the remote epmd port and locally mapped epmd port need to be identical. So in the default setup `remote_observe` will kill any locally running epmd service before creating the SSH tunnel, so that the Elixir/Erlang remote shell is opened against the remote node and not locally.

### MacOS
Haven't tested there. 
