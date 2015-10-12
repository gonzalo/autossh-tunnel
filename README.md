autossh init.d script
=====================

Script to generate ssh tunnel at debian/raspian boot

Usage
=====

Be sure you have autossh installed

```
$apt-get install autossh
```

Autossh doesn't allow to set a password so you have to upload your rsa key to remote server.
As we don't configure an specific user for autossh generate keys as root

```
$ssh-keygen -t rsa
```
...and let default options without passphrase, then copy public key to remote server...
```
$ssh-copy-id user@remotehost
```
...or alternatively using just ssh...
```
$cat ~/.ssh/id_rsa.pub | ssh user@remotehost "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
Now test that you can access remote server WITHOUT password
```
$ssh user@remotehost
```
If it's ok you can now configure autossh script
First edit autossh-tunnel to adjust your need. If you want to create multiple tunnels you have to create
different autossh-tunnel files, just copy and rename them.
Key parameters are:
* MPORT=54321 # autossh monitoring port (unique, change only if you create multiple tunnels)
* TUNNEL="-L 1234:localhost:1234" # the ssh tunnel to setup
* RUSER="user" # remote user
* RSERVER="remotehost.tld -p 22" # remote server (change standard port 22 if you are using another one)

Let's install this script to run it at boot

```
$cp autossh-tunnel /etc/init.d/
$chmod +x /etc/init.d/autossh-tunnel
$update-rc.d autossh-tunnel defaults
```

And test it!
```
$/etc/init.d/autossh-tunnel start #to run
$/etc/init.d/autossh-tunnel stop #to stop
$/etc/init.d/autossh-tunnel restart #to (guess it!) 
```
