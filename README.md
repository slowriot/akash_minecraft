This repository contains scripts and configurations to launch a Minecraft server on the Akash blockchain.  It can be easily configured with only changes to deploy.yaml, and supports any Minecraft version, including multiple modded server types.

[Akash](https://akash.network/) is a blockchain-based distributed cloud computing platform that aims to cut the cost of on-demand cloud computing.  This repository was created for a hackathon to showcase Akash's capabilities in a practical way.

The included scripts run on Linux and probably MacOS.  Windows users can use the attached deploy.yaml file with 

# Dependencies
- [akash](https://docs.akash.network/guides/deploy#part-1-install-akash)
- curl: `apt install curl`
- jq: `apt install jq`
- yq: `pip install yq`

# Setup
Prior to using the scripts, you need to [create a wallet](https://docs.akash.network/guides/wallet).

If you haven't ordered any deployments before, you will also need to [create a certificate](https://docs.akash.network/guides/deployment#create-a-certificate).

# Payment
Deployments are paid for with the AKT token.  You can buy it on [various exchanges](https://akash.network/token).

# Scripts
## deploy.sh

## close.sh

# Configuration
For more detailed information on configuring the Minecraft docker image, including how to enable mod support, see the documentation at https://github.com/slowriot/docker-minecraft-server#readme.
