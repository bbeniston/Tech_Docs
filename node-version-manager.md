This article will show you how to manage multiple node versions in the local machine.

A developer may be working in different projects which are dependent on different NodeJs versions. NVM comes to the help to manage different NodeJs versions in the same machine. The Nodejs versions could be switched as per the current need of the developer.

Remove the existing Nodejs installation:
Follow the steps to remove the existing Nodejs installation.

Uninstall Nodejs from the uninstall option in the control panel.
Remove the Nodejs installation folder (Example: C:\Program Files\nodejs)
Remove the npm folder (Example: C:\Users\<user>\AppData\Roaming\npm)


You can skip the above steps if Nodejs is not installed in your system.
Install NVM
Download the latest version of NVM from https://github.com/coreybutler/nvm-windows/releases (You can download the nvm-setup.zip file, unzip it and install the setup it in your system.)

Use NVM
Here are the useful commands to manage multiple Nodejs versions.

nvm list available (to get the list of node versions available in the NodeJs repository)
nvm install <version-number> (to install a specific version available in the Nodejs repository)
nvm list (to get the list of node versions installed in our machine)
nvm use <version-number> (to switch to a different node version)
node -v (to check the current node version)


Depends on the required version of the project, you can switch the Nodejs version.
