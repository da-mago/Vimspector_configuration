# Debug in Vim (Vimspector)

Would you like to debug code from your Vim editor? Vimspector is the response:
https://github.com/puremourning/vimspector

Would you like it to be as easy as hitting the 'start-debugging-button'? Follow this repo instructions.


## Prerequisites
<details>
<summary>Vim 8.2 with python3.6+ support</summary>

Does your Vim support Vimspector? Run `vim --version` and check both Vim version and python3 support.
  
Otherwise, install it or compile it from sources (out of scope of this README)
</details>

<details>
<summary>Python 3.6+</summary>
</details>

<details>
<summary>Tools for compile/debug C code: gcc/gdb/gdbserver</summary>
  'gdb' in the client machine and gdbserver/OCD' in the remote machine. 'gcc' wherever you compile the code.
</details>

<details>
<summary>Vimspector adapters ('debugpy' for python, 'vscode-cpptools' for C and 'vscode-bash-debug' for bash)</summary>
  
  Actually, when starting debugging, if the debug adapter needed by Vimspector is not present, Vimspector will ask the user if it should install it (automatically) for you. So, you just accept...and that's all 
</details>


## Try it out!
- Clone the repository
- Copy '.vimspector.json' to 'src_examples' folder
- Go to 'src_example' folder and open any example source code file with Vim.
- Hit 'F5' ...  that's all! (try only local debug configurations at this point)

<img src="https://user-images.githubusercontent.com/63365742/131861406-8bdc0632-7060-46f8-abf7-30fae03faa77.png" width="50%">

![image](https://user-images.githubusercontent.com/63365742/131861594-bca5ce07-464d-4626-8010-7407cb3268ff.png)

## Vimspector overview

I promise I won't deep in too much on this... just the basics.

This is the typical local debugging chain when using Vimspector:
```
   Vim -> Vimspector plugin -> adapter -> native debugger -> binary
```

Vimspector is a Vim plugin that adds support for DAP (Debugger Adapter Protocol) and visual debugging (separated windows for code, variables, stack, ...).
DAP protocol comes from Microsoft and it is a language-agnostic debugger protocol: https://microsoft.github.io/debug-adapter-protocol/ \
The point is that it is "unrealistic to assume that existing debuggers or runtimes adopt this protocol any time soon" (words from the main DAP website). So, the solution is to use a bridge between the DAP protocol and the regular debugger used for any language (ie: gdb for C).\
This bridge is called 'adapter', and Microsoft itself is providing some of these adapters (ie: debugpy for python, vscode-cpptools for C/C++, ...).

OK... so in the end, we'll end up using the regular native debugger under the hood. In fact, you can even send native debugger commands in the Vimspector window console (ie: (for gdb) -exec p i) \
In some cases, like debugpy, the adapter and the debuger come in a single piece (it makes sense since this debugger comes directly from Microsoft).

So, what do we need to configure for all these pieces (Vimspector, adapter, native debugger)?
- Vimspector will need common (regardless the adapter type) configuration:
  - request type (launch or attach to the target)
  - program (binary to debug)
  - whether to stop on entry or not
  - Host/port (for remote debugging)
  - etc ...
- Adapter will need specific adapter configuration:
  - set a command-line to execute the specific adapter
  - automate the launching of the debugger server in the remote machine
  - etc ...
- Native debugger may require some initialization (ie: run some native gdb commands before starting debugging)

You can set the whole configuration in a single file, but it is advisable to split it in two files: Vimspector config and adapter config.
As the number of debug configurations and its complexity increase, it helps to better organize the information (in fact, several Vimspector configurations could use the same adapter configuration).

Take-away: if you Vimspector configuration is simple, just use a single file. This repo follows this single file configuration approach.

## Let's improve the setup

Great! It is working... but ... do you have any question/comments?
- Do I need to copy the json file/s to every project? -> No
- I would prefer Vimspector to show me only debug configurations belonging to the file opened in Vim -> No problem
- What about debugging my current project (not those simple examples)? -> You're almost ready

### Common configuration files

You can store Vimspector and adapter configuration files either in your project folder or in your Vimspector path.
Configuration files present in your project folder will take precedence over the ones found in the Vimspector path.
Take-away: do not add any Vimspector/adapter config file to your project (use the common ones), unless you need to customize something for it.

Filenames and locations:
- Specific for a project:
  - ${PROJECT_FOLDER)/.vimspector.json (for debug configuration profiles)
  - ${PROJECT_FOLDER)/.gadgets.json (for adapters configuration)
- Common for all projects:
  - ${VIMSPECTOR_PATH}/configurations/\<OS\>/\<filetype\>/any_filename.json (there may be more than one)
  - ${VIMSPECTOR_PATH}/gadgets/\<OS\>/.gadgets.json (for adapter config file)

Maybe, at this point, you're asking yourself about the 'filetype' folder. Good question. Vimspector allows to split configuration profiles in several files (one per language). This way, it will only show you the ones related to the opened file.

### Let's try this new setup out

- Remove '.vimspector,json' from 'src_examples' folder
- Copy 'python.json' to ${VIMSPECTOR_PATH}/configurations/linux/python/python.json (let's assume you are working on Linux)
- Copy 'c.json' to ${VIMSPECTOR_PATH}/configurations/linux/c/c.json
- Copy 'bash.json' to ${VIMSPECTOR_PATH}/configurations/linux/sh\bash.json
- Again, from 'src_example' folder, open any example source code file with Vim and hit 'F5'.

Note: Maybe you need first to create those 'python', 'c' and 'sh' folders.\

This time, if you open 'prueba.py' file and hit 'F5', you'll see only valid python debug options:
![image](https://user-images.githubusercontent.com/63365742/131924732-d8dcdb13-7a80-41f7-871b-165d21c98349.png)

Note: If there is only one debug configuration available, debug will start immediately (no need to choose an option).

And the best is that you won't probaly need to do anything else for your next project....just hit 'F5'.

## Remote debugging

At this point, you've succesfully debugged the source code examples in your local machine... but we need to do something else to try remote-debugging out.\
We need, of course, to run a debugger server in the remote machine.\
Vimspector allows you to automate this task in the adapter configuration (connecting by **passwordless** ssh to the remote machine and launching the server).\
This repo is not using this approach, and it is assuming that you manually enter the remote machine and launch the debugger server before start debugging.


### How to run debugger server in the remote machine?
Issue this command after connecting by ssh:
- Python: python3 -m debugpy --listen ${client_host}:${port} --wait-for-client ${PYTHON_FILE}
- C: gdbserver --once --no-startup-with-shell ${client_host}:${port} ${C_BINARY}

Note:
- client_host can be ignored (meaning localhost). '0.0.0.0' value means any client IP.

Examples:
- python3 -m debugpy --listen 0.0.0.0:5678 --wait-for-client prueba.py
- gdbserver --once --no-startup-with-shell 5678 prueba

### SSH-tunneling

You can create a ssh tunnel between your local and remote machine and then configure both debugger client and server to use localhost:\
https://github.com/microsoft/debugpy/wiki/Debugging-over-SSH

I've just tested quickly by issuing this command (without keys argument):\
```
sh  -L 5678:127.0.0.1:5678 -l ${USER} ${REMOTE_HOSTNAME}
```

## Do you want to know more about Vimspector configuration files?
Read comments in the repo configuration files and, of course, read official Vimspector github info.
