# Visual Studio Code

Working with VSC is highly recommended due to its extensions and easy integration.

## Windows

After downloading VSC, for working in windows is highly recommended to install a linux subsystem. In this case Ubuntu.

[Microsoft subsystem](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

For integrating VSC with Ubuntu, we need to open VSC and install: `Remote - WSL` from **extensions**.

### Python

From Ubuntu, we can now install python.

```bash
sudo apt-get update
sudo install python3.7
```

#### pip and virtual environments

When working with pip, is always recommended to work with virtual environments.

For this we will have to install pip (pip3 for python3.x) and virtualenv packages.

```bash
sudo apt-get install python3-pip
sudo pip3 install virtualenv
```

Now we can create our virtual environment at the location we want to do it with

```bash
virtualenv <your-virtual-environment-name>
```

It is highly recommended to your own python interpreter and to set the path of where you want to create the virtual environment

```bash
virtualenv <your-virtual-environment-name> -p /usr/bin/python3.7
```

If needed, we can check the python interpreter location with: `which python3.7`

**Useful**

For activating and deactivating the environment

```bash
source venv/bin/activate
deactivate
```

**IMPORTANT**

For Visual Studio Code to fetch all the components needed to run WSL, we need to run `code .` from the terminal.

[Visual Studio Code Configuration](https://code.visualstudio.com/docs/remote/wsl)

### Setting the Visual Studio Code configuration

If we want VSC to automatically open our virtual environment, we need to add the following lines to the `settings.json`

If I am not wrong, there are 3 setting files, a **default** one, a **local** one and one that belongs to the **project**

For finding the file you can search somewhere on File/... or do as I do by pressing `Ctrl + Shift + p` and searching for `settings.json`.

Once settings.json file is open, we need to add the following keys and values:

```json
"python.venvPath": "/mnt/c/GitEjar/", # So VSC knows where the environment is.
"python.pythonPath": "/mnt/c/GitEjar/env/bin/python", # For the python interpreter of the virtual environment.
```

### VS Code Workspace
When working with different projects it is recommended to create different workspaces.
For creating one we just need go to `File > Save WorkSpace As. > Select the folder and choose a name`.
a `.code-workpace` will be created.
Inside this file we can configure our own settings as we did in `Settings.json`

```json
{
	"folders": [
		{
			"path": "."
		}
	],
	"settings": {
		"python.venvPath": <Your_path>,
		"python.pythonPath": "env/bin/python3.7"
	}
}
```


## Bonus: Coding format

When coding in python I highly recommend to use the black format which is a subset of pep8.
For this we can add the following line to our `settings.json`

```json
"python.formatting.provider": "black"
```

You will get a prompt to install the Black formatter

## Bonus: Customizing the Linux prompt

In my case I like having the name of the branch and virtual environment I am currently working on being shown at the linux terminal and with different colors.

For this we need to find the `.bashrc` file. In my case I don't remember how I found it but it was not shown when I tried to enter to its reference `~/.bashrc`

Once there we will add 2 functions, one for getting the information of the virtual environment and another one for getting the git branch. After this we will modify the PS1 prompt

```bash
## Add at the end of the file the following:

#Function for getting the venv name

virtualenv_info(){
    # Get Virtual Env
    if [[ -n "$VIRTUAL_ENV" ]]; then
        # Strip out the path and just leave the env name
        venv="${VIRTUAL_ENV##*/}"
    else
        # In case you don't have one activated
        venv=''
    fi
    [[ -n "$venv" ]] && echo "(virtualenv:$venv) "
}

#Getting the git branch name
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

# disable the default virtualenv prompt change
export VIRTUAL_ENV_DISABLE_PROMPT=1

# Setting the new colors
export PS1="\$(virtualenv_info)\[\033[34m\]\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "

```










