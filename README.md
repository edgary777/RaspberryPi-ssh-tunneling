# SSH Tunneling with a client raspberry pi

## Prerequisites
-You need to have access to a public ssh server (Digital Ocean, AWS, etc...) and a raspberry pi.
-install autossh ``sudo apt install autossh``


## Setup
First we open the ports we want to expose on the external server:

1. enable the firewall.
   ````
   sudo ufw enable
   ````
1. allow external access to whatever port you want, change the 22 for whatever you need.
   ````
   sudo ufw allow 2222
   ````

If you want the connection to be persistent, even after a reboot, you need to be able to authenticate without a password.

To do that you just have to create an rsa key and share it with the server:

1. generate your keys.
   ````
   ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ""
   ````
1. share your keys with the server.
   ````
   ssh-copy-id -i ~/.ssh/id_rsa.pub user@serverAddress
   ````
   
# SSH Tunneling

After setting up the basics you just need to run a simple command to star the ssh tunnel.

1. First you should do a test to verify everything is working correctly
 ````
 autossh -N -R 22:localhost:2222 user@serverAddress
 ````
   - The 22 is the port you want to forward to the server, change it to whatever you want.
   - The 2222 is the port that you will access when connecting to the server, also change it to whatever you want.
   if everything is working correctly you should be able to access your pi by using the server address.
1. If everything is working correctly we now will set the ssh connection to begin on startup to make it persistent.
   - Open crontab (Don't use sudo).
     ````
     crontab -e
     ````
   - add this line at the end
     ````
     @reboot nohup autossh -f -N -R 22:localhost:2222 user@serverAddress &
     ````
