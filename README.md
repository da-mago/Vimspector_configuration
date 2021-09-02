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
- Create a new project folder
- Copy '.gadgets.json' and '.vimspector.json' to it
- Copy the bash, C or python source code files to it
- Open any of those source code files with Vim... and hit 'F5'... thats all! (try only local debug configurations at this point)

<img src="https://user-images.githubusercontent.com/63365742/131861406-8bdc0632-7060-46f8-abf7-30fae03faa77.png" width="50%">

![image](https://user-images.githubusercontent.com/63365742/131861594-bca5ce07-464d-4626-8010-7407cb3268ff.png)


## Moving things to your own projects

No surprises. Just copying the json configuration files to your project root folder should be enough.

## Do i need to copy json configuration files to every project?

No. You can make json configuration files common to all projects by storing them in the appropriate Vimspector folders.
- debug configuration profiles: 
- debug adapter configuration:
You can also place these files in the vimspector folder, so they are common for all projects (no more Vimspector config files in your project).
But maybe you are interested in some special customization in a aprojec
Depends on your needs
Much better if Vimspector only shows you the ones related to your file type (python or C).
In this case, you have to create language-specific Vimspector configuration files and place them in the Vimspector configuration files folder: YOUR_VIMSPECTOR_PATH/configurations/\<OS\>/\<filetype\>/your_config_file

Note: You can use any filename, but it can not begin with a '.' symbol (so .vimspector.json is not a valid filename here)

Let's try it out! Let's say you're using linux. You should copy:
- 'python.json' file to 'YOUR_VIMSPECTOR_PATH/configurations/linux/python/python.json
- 'c.json' file to 'YOUR_VIMSPECTOR_PATH/configurations/linux/c/c.json
- 'bash.json' file to 'YOUR_VIMSPECTOR_PATH/configurations/linux/sh/bash.json (only local debug configuration)
  
Files located in the Vimspector folder are common for all projects, so there is no need to create a specific debug configuration file for each project, unless you need to customize something for it. So great!, I'll probably only have to hit 'F5' in my next project to start debugging (no more configuration files).

Note: Vimspector takes the configuration file/s from your project folder or its own folder (not from both of them, and your project folder takes precedence).

**Do you want to know more about Vimspector configuration files?**
Read comments in the repo configuration files and, of course, read official Vimspector github info.
