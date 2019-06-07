# HandyMiner GUI Setup Guide

0. [**DOWNLOAD COMPILED GUI BUILD**](https://github.com/HandshakeAlliance/HandyMiner-GUI/releases)

1. **Housekeeping**

If you have run a Beta HandyMiner-GUI build or Handshake HSD on this machine previously (and are not actively using) like for a testnet: remove your hsd data.

Mac:
```rm -rf ~/.hsd/testnet```

Windows:
```docker rm earthlabHSD```

2. Install [nodejs ~v10](https://nodejs.org/en/)

2a. (Windows Only) Install [docker](https://docs.docker.com/docker-for-windows/install/)

## Miner Configuration:

***It is critical to choose the right GPU, especially on a laptop!***

If you decide to mine on your "Intel HD Graphics" GPU: Your performance will grind to a halt. Why? Because that particular GPU is actually responsible for rendering graphics...like the pixels on the screen you're looking at now. 
Furthermore: The hashrate on an Intel-HD is probably comparable to CPU mining with HSD directly so please just use your AMD or NVIDIA dedicated GPUs if you desire using your laptop for simple tasks while mining. 
Finally: Dont go expecting to do any crazy graphics rendering or gaming on your laptop while mining.

## How do I find my GPUs?

Within the Miner GUI is a configuration panel specific to the GPUs you will use.
Click the **Query GPU IDs** button to ask the system which cards are connected. 

Note: Most modern Laptops have 2 platforms. 0 or 1. We set to 0 by default. 
If you dont see your AMD or NVIDIA GPU listed in the Query GPU response: Try the other platform by entering a different platform value in the 'platform' field and click **Query GPU IDs** again.

## FAQ/GOTCHAS

1. **I changed my wallet or stratum password and now there are errors or my shares all fail.**
The miner stratum doesnt deal with password (ultimately wallet) changes. 

**Easy fix:**
change the username of the miner in the GUI.

**Technical fix:**
If you change your miner user/password just remove the local stratum directory and restart the miner. 
Mac: ```rm -rf ~/.hsd/testnet/stratum``` 
Windows folks: ```docker rm earthlabHSD```

2. **I'd like to run a Handshake HSD node myself locally instead**
Awesome! If GUI sees an hsd node running locally we just use it. Make sure to have [hstratum](https://github.com/HandshakeAlliance/hstratum) installed and configured properly into your HSD, and run like::
```
./bin/hsd --network=testnet --cors=true --api-key=earthlab --http-host=0.0.0.0 --coinbase-address=ts1q59rxjegn030vwe0z3jjgx76j6ql44tpfwkjv5g --index-address=true --index-tx=true --listen --plugins hstratum --stratum-host 0.0.0.0 --stratum-port 3008 --stratum-public-host 0.0.0.0 --stratum-public-port 3008 --stratum-max-inbound 1000 --stratum-difficulty 8 --stratum-dynamic --stratum-password=earthlab
```
^^ Notice the **--coinbase-address** field: that's your wallet you'd like to mine to.
Also note that we run with the **--index-tx=true** and **--index-address=true** flags for GUI. This allows us to show timelines and update the GUI when we win blocks.

3. **How Do I access the Windows Docker Machine?**
You can ssh in via your terminal (i use git bash).
```winpty docker exec -it earthlabHSD bash```
will ssh you into the docker machine's hsd folder (/usr/hsd). 

4. **Can I run my dockerized HSD node outside the GUI?**
You sure can. On your terminal::
```winpty docker exec -i earthlabHSD sh -c "./run.sh" ts1q59...wkjv5g``` where ```ts1.....v5g``` is your wallet you'd like to mine to.
