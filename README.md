# Vimspector_configuration

Would you like to debug code from your Vim editor? Vimspector is the response:
https://github.com/puremourning/vimspector

I know you'd like to hit the 'start-debugging' button and forget about configuration files.
This is the spirit of this repo...although I encourage you, at some point, to understand the
configuration mechanics..just in case you need something different.

**How to debug?**

Prerequisites:
- Vim 8.2+ with python3.6 support
- Python 3.6+
- Tools for compile/debug C code: gcc/gdb/gdbserver
- Vimspector adapters (debugpy for python and vscode-cpptools for C)


**Steps**
- Copy '.vimspector.json' file to your project root folder (it is mandatory to use this filename)
  - Note: this file contains debug configuration profiles for local and remote debugging (both C and python)
- Open your C or python code with Vim... and hit 'F5'... thats all!

<img src="https://user-images.githubusercontent.com/63365742/131861406-8bdc0632-7060-46f8-abf7-30fae03faa77.png" width="50%">

![image](https://user-images.githubusercontent.com/63365742/131861594-bca5ce07-464d-4626-8010-7407cb3268ff.png)

**Too many debug options?**

Much better if Vimspector only shows you the ones related to your file type (python or C).
In this case, you have to create language-specific Vimspector configuration files and place them in the Vimspector configuration files folder: YOUR_VIMSPECTOR_PATH/configurations/\<OS\>/\<filetype\>/your_config_file

Note: You can use any filename, but it can not begin with a '.' symbol (so .vimspector.json is not a valid filename here)

Let's try it out! Let's say you're using linux. You should copy:
- 'python.json' file to 'YOUR_VIMSPECTOR_PATH/configurations/linux/python/python.json
- 'c.json' file to 'YOUR_VIMSPECTOR_PATH/configurations/linux/c/c.json
  
Files located in the Vimspector folder are common for all projects, so there is no need to create a specific debug configuration file for each project, unless you need to customize something for it. So great!, I'll probably only have to hit 'F5' in my next project to start debugging (no more configuration files).

Note: Vimspector takes the configuration file/s from your project folder or its own folder (not from both of them, and your project folder takes precedence).

**Do you want to know more about Vimspector configuration files?**
Read comments in the repo configuration files and, of course, read official Vimspector github info.
