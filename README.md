# Debug in Vim

Would you like to debug from your Vim editor? **Vimspector** is the response:
https://github.com/puremourning/vimspector

Would you like it to be as easy as hitting the 'start-debugging-button'? Follow this repo instructions.


## Prerequisites

<details>
<summary>Python 3.6+</summary>
</details>   

<details>
<summary>Vim 8.2 with python3.6+ support</summary>
<br>
Run `vim --version` and check both Vim version and python3 support.
</details>

<details>
<summary>Tools for compile/debug C code</summary>
<br>   
'gdb' (client machine), 'gdbserver/OCD' (remote machine) and 'gcc' (wherever you compile the code)
</details>

<details>
<summary>Vimspector adapters</summary>
<br>
- 'debugpy' for python<br>
- 'vscode-cpptools' for C<br>
- 'vscode-bash-debug' for bash)<br>
- ...
</summary>
<br>
<br>
Actually, when starting debugging, if the debug adapter needed by Vimspector is not present, Vimspector will ask you to install it (automatically) for you. So, you just accept...and that's all 
</details>


## Try it out!
Clone the repository, copy the Vimspector configuration files to the appropriate Vimspector folders and that's all

```
git clone https://github.com/da-mago/Vimspector_configuration.git

# Yes.. We're assuming that you're using Linux
# Replace VIMSPECTOR_PATH with your Vimspector path
# Create 'configurations/linux/<filetype> folders if they are not present
cp Vimspector_cfg/python.json ${VIMSPECTOR_PATH}/configurations/linux/python/python.json
cp Vimspector_cfg/c.json ${VIMSPECTOR_PATH}/configurations/linux/c/c.json
cp Vimspector_cfg/bash.json ${VIMSPECTOR_PATH}/configurations/linux/sh\bash.json
```

Open an example source code file and hit 'F5' to start debugging!

<img src="https://user-images.githubusercontent.com/63365742/132125037-0a59864e-aa98-4cf3-a5c3-74c7412962e9.png" width="50%">

![image](https://user-images.githubusercontent.com/63365742/131861594-bca5ce07-464d-4626-8010-7407cb3268ff.png)

---
**NOTE**

In order 'F5' (and others) is configured for Vimspector, you need to open your `.vimrc` config file and add this line: \
`let g:vimspector_enable_mappings = 'HUMAN'`
<br><br>HUMAN mapping: https://github.com/puremourning/vimspector#human-mode

---
### ... and try remote debugging

Before starting remote debugging, you need to launch the debugger server in the remote machine
- (Debugging python ?) python3 -m debugpy --listen ${client_host}:${port} --wait-for-client ${PYTHON_FILE}
- (Debugging c ?) gdbserver --once --no-startup-with-shell ${client_host}:${port} ${C_BINARY}

~~~
Examples:
- python3 -m debugpy --listen 0.0.0.0:5678 --wait-for-client prueba.py
- gdbserver --once --no-startup-with-shell 5678 prueba

client_host can be ignored (meaning localhost). '0.0.0.0' value means any client IP.
~~~
---
> **_NOTE:_** Vimspector allows you to automate this task in the adapter configuration (connecting by **passwordless** ssh to the remote machine and launching the server). Read official Vimspector documentation.
---

## Vimspector overview

I promise I won't deep in too much on this... just the basics.

This is the typical local debugging chain when using Vimspector:
```
   Vim -> Vimspector plugin -> adapter -> native debugger -> binary
```

Vimspector is a Vim plugin that adds support for DAP (Debugger Adapter Protocol) and visual debugging (separated windows for code, variables, stack, ...).
DAP protocol comes from Microsoft and it is a language-agnostic debugger protocol: https://microsoft.github.io/debug-adapter-protocol/ \
The point is that it is "unrealistic to assume that existing debuggers or runtimes adopt this protocol any time soon" (words from the own DAP website). So, the solution is to use a bridge between the DAP protocol and the regular debugger (ie: gdb for C).\
This bridge is called 'adapter', and Microsoft itself is providing some of these adapters (ie: debugpy for python, vscode-cpptools for C/C++, ...).

OK... so in the end, we'll end up using the regular native debugger under the hood. In fact, you can even send native debugger commands in the Vimspector window console (ie: (for gdb) -exec p i) \
In some cases, like debugpy, the adapter and the debuger come in a single piece (it makes sense since this debugger comes directly from Microsoft).

## Specific configuration for my project

The configuration found in the Vimspector folder is not good for my next project.<br>
Don'r worry... You can create a new configuration file for this project and store it in the project root folder (not in Vimspector path). Vimspector will look first at your project folder, and then in its own folder.

> **NOTE:_** Mandatory filename: .vimspector.json

## Other useful links
<details>
<summary>I want to know more about this</summary>
<br>https://github.com/puremourning/vimspector
</details>   

<details>
<summary>You're not sudo. How to install a newer python package?</summary>
<br>https://ernie55ernie.github.io/python/2016/11/11/install-python-packages-for-local-user-without-sudo.html
</details>

<details>
<summary>You're not sudo. How to install a newer vim package with python3 support?</summary>
<br>https://github.com/AllanChain/blog/issues/151
</details>

<details>
<summary>Debugging over SSH (link from Vimspector doc)</summary>
<br>https://github.com/microsoft/debugpy/wiki/Debugging-over-SSH
</details>
