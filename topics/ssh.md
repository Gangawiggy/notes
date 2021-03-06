# Basics

`sudo apt-get install openssh-server` - Install SSH.  

`ssh user@SERVER-IP-ADDRESS` - SSH to server.  
`ssh -p PORT user@SERVER-IP-ADDRESS` - Via port.  

SSH in Virtual Machine needs a port forwarding rule in network settings for the VM. Name `ssh`, host port `3022`, guest port `22`.

`ssh -p 3022 user@127.0.0.1` - SSH to VM locally.  

`ssh -p 3022 user@127.0.0.1 ls -a` - Execute one command and exit.  

# SSH with RSA key
This is used to log in without writing the password each time. Also, it is much more secure.  

This works by using a private `id_rsa` and public `id_rsa.pub` key pair. These keys can be generated by running `ssh-keygen`, and they are located in `~/.ssh` i.e. `/home/user/.ssh`.  

The `id_rsa` private key stays in the host machine in `~/.ssh`. The `id_rsa.pub` public key goes on the server in a `~/.ssh/authorized_keys` file. Simple as that, the next ssh is password-less.  

To transfer a public key, use this from `~`:  
```bash
rsync -av -e 'ssh -p 3022' ./.ssh/id_rsa.pub user@127.0.0.1:~/.ssh/
```

# Transfer files over SSH with rsync
Only trasnfer the files that have changes or are missing. Much faster and secure than FTP.  

`-a` - Recursion and preserve everything.  
`-v` - Transfer log.  
`-e` - Specify remote shell.  

```bash
# Transfer everything from the current directory to a new folder in the remote home directory.
rsync -av -e 'ssh' . user@255.255.255.255:~/folder/

# Transfer folder1 and its content to remote home directory
rsync -av -e 'ssh' ./folder1 user@255.255.255.255:~/

# Transfer everything from folder1 without the folder itself to remote folder2 in remote home directory.
rsync -av -e 'ssh' ./folder1/ user@255.255.255.255:~/folder2/

# Transfer multiple files and folders.
rsync -av -e 'ssh' ./file1 ./file2 ./folder1/ user@255.255.255.255:~/folder2/
```

### To Virtual Machine
```bash
rsync -av -e "ssh -p PORT_NUMBER" <SOURCE> <DESTINATION>:<PATH>  

rsync -av -e 'ssh -p 3022' . user@127.0.0.1:~/make-this_folder
```
