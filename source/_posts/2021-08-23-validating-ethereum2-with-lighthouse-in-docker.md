---
title: Validating Ethereum2 with Lighthouse in Docker
tags:
  - cryptocurrency
  - docker
  - ethereum
  - lighthouse
  - linux
  - staking
id: '1912'
categories:
  - - uncategorized
alias: /2021/08/23/validating-ethereum2-with-lighthouse-in-docker/
date: 2021-08-23 13:15:45
---

Ethereum is shifting from Proof of Work "mining" to a Proof of Stake "staking" blockchain. This stake requires 32 ETH per validator to validate directly, but there are many custodial pooling services that allows those who have less to still participate with little investment or effort. However, I enjoy participating directly. I learned quite a lot in the process running my [Ethereum GPU mining rig](https://www.benchodroff.com/2018/01/31/building-six-gpu-ethereum-mining-rig/) since 2017, and wished to do the same with staking. These are my quick notes how how I set up docker containers on Ubuntu Linux 20.04 LTS for Geth, Lighthouse beacon node, and a Lighthouse validator client. Using this setup, I am yielding about 6.5% APY this year on my ETH. As long as your computer is online more than 50% of the time, you will increase your Ethereum while helping participate in the decentralization that secures the network.
<!-- more -->
Unlike Ethereum 1.0 which required very expensive and power hungry graphics cards, Ethereum 2.0 uses relatively little hardware and energy. Even a modest modern computer with a reliable internet connection can do the job. The only significant requirement is a NVME SSD if you plan to run an Ethereum 1.0 client, as the I/O requirements are high. I can run the entire Ethereum 1.0 and 2.0 stack on an incredibly efficient Intel NUC 10 i7 computer which consumes only 50 watts, runs silent, and is incredibly tiny. The total build cost was under $1600 in early 2021. This is higher in cost than normal because of the global microchip shortage. I also use a consumer grade UPS battery backup on my router, modem, and computer to ensure reliable operation. While many choose to run their setups on cloud computing, I prefer to keep my validator at home for the fun of it and lower total cost of ownership. As my home is in mainland China, I did check to see whether running any type of cryptocurrency mining (which validating would still qualify as) at home for personal purposes was still allowed - and they claim there are no laws against it: [http://www.12348.gov.cn/english.html#/english/englishDetail?info\_id=cdc1985e-e0cf-4df7-8602-65161f940532](http://www.12348.gov.cn/english.html#/english/englishDetail?info_id=cdc1985e-e0cf-4df7-8602-65161f940532).

Assembling an Intel NUC takes only perhaps 30 minutes and is about the simplest computer you could build. I opted for an i7 to hopefully future proof some of the processing requirements. I included 32GB of RAM such that I could reliably run both a mainnet and testnet client simultaneously on the same hardware for testing purposes. You will only need less than 1TB of storage for the geth client, but I opted for 2TB to future proof the growth. My second computer is my old mining rig, but using two machines is not strictly required and arguably creates more complexity than benefits since you can only run a single validator.

![Intel NUC 10 i7](http://www.benchodroff.com/wp-content/uploads/2021/08/intelnuc10i7.jpg)

Item

Amount

Price (RMB)

Total (RMB)

Intel BXNUC10i7FNH i7 NUC

1

3899

3899

16GB RAM DDR4 2666

2

599

1198

Intel 480GB SSD S4600

1

579

579

WD 2TB NVME

1

4550

4550

Total (RMB)

10226

Total (USD)

$1598

I run a high availability configuration involving two physical servers for resiliency. As a validator node, you can only be active on a single machine, or your stake will get penalized severely by the network by being "slashed". However, by using multiple servers, I am able to create redundancy on the Ethereum 1.0 geth client as well as the Ethereum 2.0 Lighthouse beacon node to help my single validator stay online. In addition, I have a third resiliency layer using a free account at [https://infura.io](https://infura.io/) which has both Ethereum 1.0 and 2.0 beacon node clients configured as a final fallback option for my validator. Monitoring is performed using [https://beaconcha.in](https://beaconcha.in/) as well as a using Telegraf, Influxdb, and Grafana for alerting. While there is a Prometheus monitoring option, I have not yet configured it or found it necessary.

*   The Ethereum launchpad has all the resources you need to get started: [https://launchpad.ethereum.org/en/overview](https://launchpad.ethereum.org/en/overview)
*   The Lighthouse documentation explains many of the parameters I am using: [https://lighthouse-book.sigmaprime.io/](https://lighthouse-book.sigmaprime.io/)
*   I highly recommend joining the Ethstaker discord for questions: [https://discord.gg/uN2wyUYutM](https://discord.gg/UmeDMD9a)
*   Lighthouse also has their own discord too for technical help: [https://discord.gg/RPS8GZyx3c](https://discord.gg/RPS8GZyx3c )

This is the gist of my setup. Of course, follow the documentation and more steps should be taken to harden your configuration.

```
# Set up UFW firewall to allow traffic between all servers/docker involved
ufw allow from 192.168.50.13
ufw allow from 192.168.50.16
ufw allow from 172.17.0.0/24
ufw allow 22/tcp
ufw enable
```

```
# Install Docker on Ubuntu 20.04
apt update
apt install -y docker-ce

# Pull Lighthouse for Ethereum 2.0
docker pull sigp/lighthouse:latest

# Pull geth for Ethereum 1.0 geth client stable
docker pull ethereum/client-go:stable

# Dockupdater is a great way to automatically update docker 
docker pull dockupdater/dockupdater:latest
```

Below are the docker scripts I use with \[VARIABLES\] redacted to protect my exact configuration. I have set up each docker container to auto restart so that if the computer reboots, it will come back online. Of course, be very careful with your validator. You can only run one validator at a time or you will get slashed. NEVER store your validator keys on more than one host, or it is inevitable you _will_ make a mistake and get slashed. Always keep an encrypted backup of your validator keys offsite. It's a lot of ETH to stake. Make sure you test thoroughly all possible scenarios like computer reboots, power outages, geth or beacon node failures, and harden your Linux security.

My docker containers are running as root as I could not find an easy way with docker-compose to run as an underprivileged user while automatically updating containers when new releases are made available. As a result, I have sacrificed some security for convenience by using dockupdater to automatically update the docker containers each time a new release is made available. Perhaps someone has a simple and better approach - please tell me.

I would love to learn how to run a slasher. Perhaps that will be a future article!

```
# Set up an Ethereum 1.0 Geth client
docker run --restart=always -d --stop-timeout 300 --name ethereum-node-mainnet -h ethereum-node-mainnet -v /opt/ethereum/mainnet/ethereum-mainnet:/root/.ethereum -p 28545:28545 ethereum/client-go:stable --http --http.addr 0.0.0.0 --http.port 28545 --http.api eth,net --http.corsdomain '*' --ipcdisable --gcmode full --syncmode snap --cache 2048 --metrics --metrics.influxdb --metrics.influxdb.endpoint "https://[INFLUXDBHOST]:8443" --metrics.influxdb.username "[USER]" --metrics.influxdb.password "[INFLUX_PASSWORD]" --metrics.influxdb.database geth --metrics.influxdb.tags host="[INFLUX_HOSTNAME]-mainnet" --txpool.accountslots 32 --txpool.globalslots 8192 --txpool.accountqueue 128 --txpool.globalqueue 2048 --txpool.journal ""

# Create an Ethereum 2.0 Lighthouse Beaconnode
docker run -d --restart=always --stop-timeout 300 --name lighthouse-bn-mainnet -h lighthouse-bn-mainnet -p 29000:9000 -p 25052:5052 -v /opt/ethereum/mainnet/lighthouse:/root/.lighthouse sigp/lighthouse lighthouse --network mainnet bn --target-peers 120 --staking --http --http-address 0.0.0.0 --metrics --eth1-endpoints http://[HOSTIP_1]:28545,http://[HOSTIP_2]:28545,https://mainnet.infura.io/v3/[INFURA_AUTH] --monitoring-endpoint https://beaconcha.in/api/v1/client/metrics?apikey=[BEACONCHAIN_KEY]&machine=[HOSTNAME]

# Create an Ethereum 2.0 Validator 
# Be careful to only run this on ONE host or you will get slashed
docker run -d --restart=always --stop-timeout 300 --name lighthouse-vc-mainnet -h lighthouse-vc-mainnet -p 25062:5062 -p 25064:5064 -v /opt/ethereum/mainnet/lighthouse:/root/.lighthouse -v /opt/ethereum/mainnet/validator_keys:/root/validator_keys sigp/lighthouse lighthouse --network mainnet vc --enable-doppelganger-protection --metrics --beacon-nodes http://[HOSTIP_1]:25052,http://[HOSTIP_2]:25052,https://[INFURA_AUTH]@eth2-beacon-mainnet.infura.io --http --monitoring-endpoint https://beaconcha.in/api/v1/client/metrics?apikey=[BEACONCHAIN_KEY]&machine=[HOSTNAME]
```

I welcome any suggestions, questions, or please leave a comment if this helped you.