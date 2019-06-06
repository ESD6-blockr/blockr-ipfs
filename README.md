# blockr-ipfs
Private IPFS network setup used within the Blockr project

# Private IPFS network
This repository contains the necessary files to create a private IPFS node. This IPFS node is used within the Blockr project to upload files and save the IPFS hash in a smart contract. For more information on IPFS, visit https://docs.ipfs.io/.

The IPFS network is made private by the swarm-key, located in the `.ipfs/swarm.key` file. The swarm of peers need to connect to each other. By supplying each node with the same swarm-key, it will enforce the network to be private instead of public (connecting to all known peers in the public IPFS network).

Another element that is used to create a private network, is the bootstrap list (https://docs.ipfs.io/guides/examples/bootstrap/). The bootstrap list is a list of peers, from which the IPFS daemon learns where it should look for files. A bootstrap node is a kind of 'main node'. For the first setup of the private network, the bootstrap node is the first created node. 

We enforce the private network by removing all default bootstrap nodes (`ipfs bootstrap rm --all`) and adding the bootstrap to the daemon. More information on that below. 

# Steps required to start IPFS node
This image is not yet posted to the public DockerHub repository, so you will have to build this image yourself. 

- Run `docker build -t ipfs .`
  This will tag the image so you can run it later. 
- Run `docker run --name ipfs -p $PORT:4001 -p $PORT:5001 -p $PORT:8080 ipfs` 
  Substitute the `$PORT` variables with the ports you wish to open. This command will spin up a docker container with all the necessary modifications. 

# Bootstrapping the IPFS node
The IPFS node will be configured with no bootstrap nodes. The final step is to configure     the bootstrap node. If you run the command `docker exec -it ipfs ipfs config show` and scroll down, you will see that the 'Bootstrap' value is `null`. Now we will add the _ip address_ and the _peer ID_ of the bootstrap node to both the bootstrap and the 'normal' nodes. 

- _IP address:_ result of `curl ifconfig.me` to get the outside IP of your machine. Execute this on your host (not your Docker container).
- _Peer ID:_ was created on initialisation of the IPFS node, result of `docker exec -it ipfs ipfs config show | grep "PeerID"`.
- Execute the following command on the IPFS daemon: `docker exec -it ipfs bootstrap add /ip4/$IP_ADDRESS/tcp/4001/ipfs/$PEER_ID`. Replace `$IP_ADDRESS` and `$PEER_ID` with the previously mentioned results. 
- If you run `docker exec -it ipfs config show` again, it should now show the Bootstrap node configured.

# Finalizing
Now you have run your own IPFS node! You can use this for your own development, or with the Blockr IPFS API, especially made for the Blockr project; see https://github.com/ESD6-blockr/blockr-ipfs-api.