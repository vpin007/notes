We created our VM and supplied our ssh public key.
We don't want our services running as root so we will create a non root user and add that user to the Sudo group.

````bash
    adduser graft
````


````bash
    usermod -aG sudo graft
````


I'll copy over the ssh file from the root folder into the graft users folder and set the appropriate permissions. 
````bash
    rsync --archive --chown=graft:graft ~/.ssh /home/graft
````
Then exit terminal

'change putty login from root to graft user and login
I'll log out and log back in as the graft user.


I'll use sudo apt-get update and upgrade to update the system packages. 

````bash
    sudo apt-get update
    sudo apt-get upgrade
````


Use instructions from here to install community edition.
https://deb.graft.community/

````bash
    curl -s https://deb.graft.community/public.gpg | sudo apt-key add -
````
````bash
    sudo  echo "deb [arch=amd64] https://deb.graft.community bionic main" | sudo tee /etc/apt/sources.list.d/graft.community.list
````
Then 


````bash
    sudo apt update
    sudo apt install graftnoded
    sudo apt install graft-supernode
    sudo apt install graft-blockchain-tools
````

I Didn't install cli-wallet or rpc-wallet but the command is

````bash
    sudo apt install graft-wallet
````


'Create a directory for each supernode you plan on running
'since user is graft full path will be /home/graft/snx
I'll be running 2 on this server so I'll create 2 directories. 
````bash
    mkdir -p ~/sn1/logs
    mkdir -p ~/sn2/logs
````

Run graftnoded and let it run for a few seconds to create directories.
type exit to stop. Then Change to the .graft/lmdb/ directory.

````bash
    cd .graft/lmdb/
    ls
````
Should see 2 files in directory. We will delete the data.mdb and lock.mdb.

````bash
    rm data.mdb
    rm lock.mdb
````
Directory should be empty
````bash
    ls
````
Now use wget to pull a snapshot of the blockchain file. 

````bash
    wget https://graft.observer/lmdb/data.mdb
````
After it finishes type cd to change back to our home directory.

````bash
    cd
````


Run graftnoded --log-level 0 again and let it fully sync. 

````bash
    graftnoded --log-level 0
````

Open another ssh connection to VPS. List the directory contents with ls.
````bash
    ls
````

Should see all the supernode directories, example sn1, sn2, etc

Edit config.ini for each supernode. Change to each directory or include full path to the config.ini file.

````bash
    cd sn1
    nano config.ini
````
or 
````bash
    nano sn1/config.ini
````

http-address port for testnet is 28690

http-address port for mainnet is 18690

On sn1 I'll change http-address port to 18690 then increment this number for each additional supernode after that.
Actually you can put this to whatever number as long as it's not an already used port.
Mainly used for checking status at this point, but in the future it may need to be standard.
Change data-dir to point to each supernode directory, don't forget the trailing slash /.
wallet-public-address=Paste in your wallet address you plan on using for this supernode. Make sure there are no spaces or line continuations. Do not use these addresses they are just for example, use your own wallet addresses. 

* Changes for sn1

change http-address to 18690

data-dir=/home/graft/sn1/

wallet-public-address=GCYjfdEqmC4c2rFxtCCRihhog2T9YLEwfQsZvkgyya4UGSvWajUDK7QWhUn6ByJPxzJh9CBoThf4aPtkpEVNM6ugSNDvL48

* Changes for sn2

change http-address to 18691

data-dir=/home/graft/sn2/  

wallet-public-address=G7MgdHQkf84MtRH55b1jDCDzTjRFeMTTJJHXXrKcMxXqaZAJmwVu1Cw28J87gJnoK24EP2HhoYdZrLEcXMSVuooR1c9PGvQ

If you run graftnoded and or graft-supernode in an ssh session and exit out, it will kill the process.
We need a way to keep these processes running and not have to keep the ssh session\windows open all the time.

Some options are: 
Running the processes in "screen" sessions.
Using TMUX.
Using systemd.


I'll show how to use "screen" and provide links for setting up systemd and maybe put it in a seperate video.
We'll create a screen session for graftnoded.

````bash
    screen -S graftnoded
````
run graftnoded and let it fully sync.

````bash
     graftnoded --log-level 0 
````

Hold  Ctrl+a  then press d, this will drop us back to original session.

Create a screen session for supernode1
````bash
   screen -S supernode1 
````
Change to the sn1 directory and run graft-supernode

````bash
    ls
    cd sn1
    ls
    graft-supernode
````


Hold  Ctrl+a  then press d, this will drop us back to original session.

Create a screen session for supernode2
````bash
    screen -S supernode2
````
Change to the sn2 directory and run graft-supernode

````bash
    ls
    cd sn2
    ls
    graft-supernode
````
Hold  Ctrl+a  then press d, this will drop us back to original session.
 
To get list of screen sessions

````bash
    screen -list
````

To reattach to a session

````bash
    screen -r graftnoded
````


Hold  Ctrl+a  then press d, this will drop us back to original session.


````bash
    screen -r supernode1
````
Hold  Ctrl+a  then press d, this will drop us back to original session.


Now in order to stake your coins, you need to get the SN ID and signature of each one of your supernodes.
**The signature changes every time you query it.

You can get this information using the command line by issuing a curl command or putting the address into a web browswer.
It may be easier to copy from the web browser.
When checking from a web browser change the 127.0.0.1 to the public ip address of the computer\vps.
Or check out the program I made to query it and create the stake command for you.
If you are running multiple supernodes on 1 server increse the port number for each supernode.



````bash
    curl -s http://127.0.0.1:18690/dapi/v2.0/cryptonode/getwalletaddress
    curl -s http://127.0.0.1:18691/dapi/v2.0/cryptonode/getwalletaddress
````

Or in a web browser:

http://supernodeip:18690/dapi/v2.0/cryptonode/getwalletaddress

http://supernodeip:18691/dapi/v2.0/cryptonode/getwalletaddress	
