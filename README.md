This repository contains scripts and configurations to launch a Minecraft server on the Akash blockchain.  It can be easily configured with only changes to deploy.yaml, and supports any Minecraft version, including multiple modded server types.

[Akash](https://akash.network/) is a blockchain-based distributed cloud computing platform that aims to cut the cost of on-demand cloud computing.  This repository was created for a hackathon to showcase Akash's capabilities in a practical way.

The included scripts should run anywhere Bash is available and the below dependencies are met.  Users can also use the deploy.yaml file directly with other Akash deployment clients as they become available, or [deploy manually](https://docs.akash.network/guides/deployment) following the user guide.

# Dependencies
- [akash CLI](https://docs.akash.network/guides/deploy#part-1-install-akash)
- curl: `apt install curl`
- jq: `apt install jq`
- yq: `pip install yq`

# Setup
Prior to using the scripts, you need to [create a wallet](https://docs.akash.network/guides/wallet).

If you haven't ordered any Akash deployments before, you will also need to [create a certificate](https://docs.akash.network/guides/deployment#create-a-certificate).

# Payment
Deployments are paid for with the AKT token.  You can buy the token on [various exchanges](https://akash.network/token).

At the time of writing, you must have a minimum of 5AKT in your wallet - this is [held in escrow](https://docs.akash.network/glossary/escrow] for the duration of the deployment, and will be returned to you, minus the hosting costs you accrue.

Typical costs for running this Minecraft server with default settings, at the time of writing, are 8uAKT per block (uAKT = 1/1,000,000th of an AKT).

# Scripts
![](https://raw.githubusercontent.com/slowriot/akash_minecraft/master/docs/deploy_anim.gif)

## deploy.sh
Execute with `./deploy.sh`.

The script will verify all dependencies are present, and check that a suitable wallet exists; if more than one wallet is present on the system, it will prompt you to select the one you want to use.  It does not attempt to verify that you have enough funds, so please make sure you have sufficient funding for the escrow process (see above), as well as gas fees for blockchain transactions and the deployment fees themselves..

The script downloads the latest list of nodes, and automatically selects the best one based on its ping time relative to you.

The script sends the deployment request to the blockchain, and gives you a `dseq` number - you can use this to manage the deployment manually without the script, or to close it down later.

The script automatically chooses the best bidder for the deployment - it waits a pre-defined amount of time, collects bids, and chooses the lowest bidder.  If there is more than one bidder at the lowest price, it selects randomly between them.

The script then reports the address of the provider it's chosen, and sends the manifest.  Once the manifest has been accepted, you get a report of the lease status.

The script then gives you a URL you can connect to with your Minecraft client, and share with your friends.  Bear in mind it can take a minute or two for the server to spin up, once the lease has been created, so be patient if you can't connect immediately.

The script also generates `akash` commands for you to run if you want to get the Minecraft server logs, or the Kubernetes logs for the container.

The result of all transactions is verified by query commands after, so even if a transaction response gets lost due to a timeout or RPC error, the script (and the deployment) will continue and succeed.

## close.sh
Execute with `./close.sh`.

A convenience wrapper to automatically find the last deployment you launched, and tell akash to close it (i.e. shut it down) immediately.

You do not need to use this to close a deployment - it is provided just as a convenience function.  You can always just close down deployments manually as per the user guide, using the `dseq` number which was given to you by the `deploy.sh` script.  Guidance for how to close a deployment manually: https://docs.akash.network/guides/deployment#close-your-deployment

Be careful - if you have launched other deployments since this Minecraft deployment, it will simply attempt to close the last one you launched.

## Debugging
The scripts can be run with various environment variables set for debugging purposes:
- `debug=true` - will print out every `akash` command the scripts are about to run, allowing you to duplicate the workflow or debug errors.
- `dry_run=true` - will only execute query commands, and will not commit anything to the blockchain.  Use in conjunction with `debug=true` to see a dry run of a deployment - however, bear in mind that as the new deployment won't be committed to the blockchain, subsequent commands querying that deployment will fail - this is to be expected, and the script handles this gracefully.  This setting is useful to enable to repeat a deployment attempt, after one has failed, to find out what went wrong (as it will dry-run against the last deployment of yours it finds).

Example commandline usage: `debug=true dry_run=true ./deploy.sh`

# Configuration
## deploy.yaml

# Minecraft configuration
For more detailed information on configuring the Minecraft docker image, including how to enable mod support, see the documentation at https://github.com/slowriot/docker-minecraft-server#readme.
