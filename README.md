# WineWrap readme

## What's WineWrap
WineWrap is a simple helper script for users of Wine (https://www.winehq.org/). WineWrap is a shell script and is currently a WIP (work in progress) project...

It's goal is to make it easier/more convenient to use multiple Wine prefixes and various programs installed across multiple prefixes.

## Features
### v0.3 (first release)
- GUI (yad) driven operation
- User selectable Wine prefix upon program execution with optional new Wine prefix creation and saving the selection
- Automatic Wine prefix recognition based on executable path (if an executable is run that is stored inside one of the WineWrap prefixes it will automatically be run in that Wine prefix bypassing the selection screen)
- Automatic Wine prefix selection based on per executable saved value

## Requirements
No real hard requirements at this point, just couple of expectations. I assume you have:
- Wine installed
- `yad` installed
- Any linux distro with bash or compatible shell environment
- Some command line experience for setting it up

## Limitations
Currently only the `wine` binary is wrapped, wine64, wineboot, winetricks etc are not. For all others you will have manually specify the Wine prefix (for now).

## Installing
1. Download the `winewrap` shell script into a suitable location of your choice, make it executable and ensure the directory it is saved in is part of the PATH environment variable for any users who will want to use it.
2. Locate the real Wine script (`which wine`), I assume here that this will give you a location with a symlink pointing to the real Wine binary. Check and take note of the path pointing to the real Wine binary (`ls -l /path/to/wine-symlink`).
3. If the path is different from `/opt/wine-stable/bin/wine` you will need to edit the winewrap script and update the path on the `realwine=...` line.
4. Modify the wine symlink found by the `which` command in step 2 to point to the winewrap script in the location you downloaded it to.
5. WineWrap is ready to be used!

### Example (debian and derivates)
We are assuming that WineWrap will be installed for a single user `bob`; Wine is installed; real Wine binary is located at `/opt/wine-stable/bin/wine`; wine symlink is `/usr/bin/wine`; you have opened a shell as the user `bob`; you have `.local/bin` set up in your Home directory and it is already included in the PATH environment variable for user `bob`.
```
bob@bob:~$ pwd
/home/bob

bob@bob:~$ which wine
/usr/bin/wine

bob@bob:~$ ls -l /usr/bin/wine
lrwxrwxrwx 1 root root 23 Jan 17 23:36 /usr/bin/wine -> /opt/wine-stable/bin/wine

bob@bob:~$ sudo apt install yad
bob@bob:~$ wget 'https://github.com/gurrsoft/winewrap/raw/master/winewrap' -O ~/.local/bin/winewrap
bob@bob:~$ chmod +x ~/.local/bin/winewrap
bob@bob:~$ sudo ln -f -s ~/.local/bin/winewrap /usr/bin/wine

bob@bob:~$ ls -l /usr/bin/wine
lrwxrwxrwx 1 root root 23 Jan 17 23:36 /usr/bin/wine -> /home/bob/.local/bin/winewrap
```

## Planned features
- Easier/semi-automatic install and Wine update survivability
- More WineWrap prefix management options (such as prefix cloning, backup/restore, delete)
- Wrap more Wine binaries and winetricks too
- Provide configuration options via GUI (such as WineWrap prefix path for example)?

## FAQ
Q: Where are the WineWrap prefixes stored?  
A: WineWrap prefix are in a directory called `.wine_pfxs` in your Home directory.  

Q: Where are the saved prefix selections stored? How can I remove/make WineWrap 'forget' a saved selection?  
A: Each WineWrap prefix directory will have a file named `always.list` created the first time a selection is saved for that prefix. It is a simple text file, you can use you favorite text editor to open the file and remove the corresponding line referencing the executable in question.  

Q: I have updated Wine and WineWrap is no longer working, what do I do?  
A: You have to repeat the installation steps to reinstate the symlinks.  

Q: WineWrap is not working at all, how can I figure out what is wrong?  
A: Try running WineWrap and your target Windows executable from command-line as `DEBUG=1 wine path/to/your/target.exe` and observe the output for any errors. You can check `winewrap-debug.log` in your home directory after a DEBUG=1 run.  

