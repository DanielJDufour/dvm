# dvm
> developer virtual machine command line interface

## purpose
prevent catastrophic supply chain vulnerability attacks

## usage
```bash
# inside /home/user/repos/project.git

dvm name
> code-project-0e486d1716

# start vm
dvm start

# stop vm
dvm stop

# stop, delete and purge vm
dvm clean

# get ip address of the virtual machine
dvm ip
> 192.168.64.6

# run ls command in the vm
dvm run ls

# install node project dependencies
dvm run npm install
```

## advanced usage
### shared virtual machines
A virtual machine is automatically created by using your repo name and hash of the path in order to avoid conflicts when two repos on your computer have the same name.  If you would like multiple repos to share the same virtual machine, you can set the `DVM_NAME` environmental variable.  For example, to launch a virtual machine with the name "python-projects":
```bash
DVM_NAME="python-project" dvm launch
```
### port access
If you are running a dev server for an app at port 8080 in your virtual machine and want to view it in your web browser, get the ip address using ```dvm ip``` (e.g. `192.168.64.6`) and then navigate to `http://{IP}:{PORT}` (e.g. `http://192.168.64.1:8080`) in your web browser.
