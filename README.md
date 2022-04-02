# dvm
> developer virtual machine command line interface

## purpose
prevent catastrophic supply chain vulnerability attacks

## background
On March 18, 2022, I read the news about a [big supply chain attack](https://arstechnica.com/information-technology/2022/03/sabotage-code-added-to-popular-npm-package-wiped-files-in-russia-and-belarus/) from an open-source JavaScript package that would delete documents from a computer if it detected an IP address in Belarus or Russia.  It was a wake up call for me to improve my security, because what would have happened if a similar attack had been performed looking for an IP addresses inside my country?  This is the motivation for creating this library.  So you can develop in confidence without worrying that a dependency of a dependency might delete or mess with your files.

## summary
The dvm library basically uses [multipass](https://multipass.run/) to create a virtual machine pre-configured for your current git repository.

## install
- install [multipass](https://multipass.run/) from Canonical
- download [the dvm file](https://github.com/DanielJDufour/dvm/blob/main/dvm)
- make the file executable: `chmod +x dvm`
- copy the file to some place on your path: `cp dvm /usr/local/bin/dvm` 

## usage
```bash
# inside /home/user/repos/project

# get the vm name
dvm name
> code-project-0e486d1716

# start vm
# if the vm doesn't exist, create it
dvm start

# stop vm
dvm stop

# stop, delete and purge vm
dvm clean

# get ip address of the vm
dvm ip
> 192.168.64.6

# run ls command in the vm
dvm run ls

# install node project dependencies
dvm run npm install

# clean and start a fresh new vm
dvm reset

# open up a shell to the vm
dvm shell

# list virtual machines
# filter by current vm name and names that start with "dvm"
dvm list
> code-project-0e486d1716  Running           192.168.64.10     Ubuntu 20.04 LTS
```

## advanced usage
### environment file
By default, dvm will look for a `.dvm` file in the root of the git repository.  It will then source that file each time you run a dvm command.
Here's an example of a .dvm for a small JavaScript library:
```
DMV_NAME="random-project-name"

# lower the memory to make sure the library works in low memory environments
DVM_MEM=4GB

# make sure there's enough room for all the dependencies
DMV_DISK=50GB

# we aren't using Docker for this repo
DVM_DOCKER=false

# turn off Python support
# because it is a JavaScript project
DVM_PYTHON=false
```

### shared virtual machines
A virtual machine is automatically created by using your repo name and hash of the path in order to avoid conflicts when two repos on your computer have the same name.  If you would like multiple repos to share the same virtual machine, you can set the `DVM_NAME` environmental variable.  For example, to launch a virtual machine with the name "python-project":
```bash
DVM_NAME="python-project" dvm launch
```
### port access
If you are running a dev server for an app at port 8080 in your virtual machine and want to view it in your web browser, get the ip address using ```dvm ip``` (e.g. `192.168.64.6`) and then navigate to `http://{IP}:{PORT}` (e.g. `http://192.168.64.1:8080`) in your web browser.
### configuring specs
You can configure the specs of the vm only when you start it for the first time.  We basically pass this configuration to 
a call like `multipass launch --cpus $DVM_CPUS --mem $DVM_MEM --disk $DVM_DISK`.
```
# use 2 cpus for the machine
# default is 1
DVM_CPUS=2 dvm start

# set the memory used by the VM
# default is 16GB
DVM_MEM=24GB dvm start

# set the disk space used by the VM
# default is 20GB
DVM_DISK=100GB dvm start
```
### configurating install
By default, DVM installs some common libraries.  You can turn those on or off:
```
# don't install nodejs, npm, n, np, pnpm and yarn
DVM_NODE=false dvm start

# don't install docker and docker-compose
DVM_DOCKER=false dvm start

# don't install Python and pip
DVM_PYTHON=false dvm start
```

### support
If you have any questions or security concerns, please email the author of dvm at daniel.j.dufour@gmail.com.
