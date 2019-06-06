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
  Substitute the `$PORT` variables with the required. This command will spin up a docker container with all the necessary modifications. 

# Bootstrapping the IPFS node
The IPFS node will be configured with no bootstrap nodes. The final step is to configure     the bootstrap node. If you run the command `docker exec -it ipfs ipfs config show` and scroll down, you will see that the 'Bootstrap' value is `null`. 
  