---
title: Building a six GPU Ethereum Mining Rig
tags:
  - blockchain
  - cryptocurrency
  - ethereum
  - mining
  - proof of work
id: '315'
categories:
  - - uncategorized
alias: /2018/01/31/building-six-gpu-ethereum-mining-rig/
date: 2018-01-31 18:05:39
---

I have been working with blockchains since 2013 but always wanted to know more about how the mining process worked. Last year, I fell in love with Ethereum and was excited to learn the project uses a GPU friendly algorithm. I convinced my wife to allow me to spend some money to build a GPU mining rig. At the time I built the rig, I calculated it would take over a year to break even in mining Ethereum. Luckily, the Ethereum project has taken off in a big way. The rig paid for itself in a few months, and continues to deliver. I had a lot of fun building the rig and learned a lot about how Ethereum works in the process. My rig does about 160 million hashes a second and dual mines Ethereum and Decred. Here are some details on the setup.
<!-- more -->
\[caption id="attachment\_412" align="aligncenter" width="640"\]![Mining Rig](https://benchodroff.com/wp-content/uploads/2018/01/miningrig-1024x768.jpg) 6 GPUs ~160MH/s\[/caption\]

My build was determined based on the lack of availability of GPU's. I did not want to mix and match AMD and NVIDIA cards, but sadly this was the only available option. It would be much easier if I had six of the same GPU's. If you are struggling to find a GPU I would recommend using the website [nowinstock.com](http://nowinstock.com) and sign up to their google group.

I used Ubuntu 16 LTS as the operating system. I started by using Etcher on my Mac to load the Ubuntu installation media on a USB stick. Once booted up, I installed Ubuntu on the SSD hard drive with no GPU's running. While I could have used a USB stick as the main storage, I planned on needing the extra hard drive space for some other blockchain projects.

The biggest challenge was the motherboard. The MSI Z170A is notoriously a pain to work with and mine was no exception. Unfortunately, it was the only motherboard in stock that could support six GPUs. I updated the firmware on the motherboard bios because the included bios would not support six GPUs. I would strongly recommend using the integrated graphics and avoid connecting any GPUs or PCIe risers until your operating system is fully installed and ready to go. After the OS was loaded, I began the process of setting up the Ethereum mining software with a single GPU connected directly in the primary PCIe slot with no riser. I tried many versions of ethereum for mining but ultimately have settled on using Genfoil's dual ethereum miner. You can find the download link and more discussion than you can ever read over here: [https://bitcointalk.org/index.php?topic=1433925.0](https://bitcointalk.org/index.php?topic=1433925.0)

Once you have a basic single GPU mining rig working, keep adding one card and riser at a time. It is important to monitor the performance of the mining speeds each time you add a GPU to ensure it is running correctly. After four GPUs, I had to adjust the settings of my motherboard to set an advanced settings "Above 4G Decoding - Enabled". I did not end up using Gen1 for PEG0 or PEG1 - I left these set at auto. Setting it to Gen1, as other guides recommended worked, but would result in a ~50% loss in mining performance. Here is an example of the other guide: [https://forum.z.cash/t/msi-z170-7-gpu-bios-setting-step-by-step/14473](https://forum.z.cash/t/msi-z170-7-gpu-bios-setting-step-by-step/14473)

You will want to join a mining pool because ethereum is not profitable to solo mine. My personal favorite has been using [nanopool.com](http://nanopool.com), although I tried many others too. You will need to create an ethereum wallet, save the private key in a safe place, and use the public key as your wallet address. I highly recommend using [myetherwallet.com](http://myetherwallet.com) for storing your ETH safely.

\[caption id="attachment\_401" align="aligncenter" width="640"\]![Nanopool](https://benchodroff.com/wp-content/uploads/2018/01/nanopool-1024x728.png) Nanopool\[/caption\]

I have dual mined SIACoin in the past but currently am dual mining Decred. You may want to check out the website [whattomine.com](http://whattomine.com) to decide what coins you wish to mine based on the expected profitability.

It is very easy to spend a lot of time tuning and it is far too time consuming to explain all the options available, but I am happy to share my current settings:

My claymore startup script looks like this: `export GPU_MAX_HEAP_SIZE=100 export GPU_USE_SYNC_OBJECTS=1 export GPU_MAX_ALLOC_PERCENT=100 export GPU_SINGLE_ALLOC_PERCENT=100 /opt/claymore10.6/ethdcrminer64 -tstop 90 -allpools 1 -mode 0 -r 1 -mpsw dummydummy -epool eth-us-east1.nanopool.org:9999 -ewal $ETH/$NAME/$EMAIL -epsw x -dpool dcr.suprnova.cc:3252 -dwal $NAME -dpsw $PASSWORD -ftime 10 > /dev/null 2>&1 &`

I use the following script to tune my AMD and NVIDIA cards prior to launching: ``#!/bin/bash sudo su -c 'nvidia-smi -pm 1' # Set power limit to 200 watts per card sudo su -c 'nvidia-smi -pl 200' # Unrestrict all GPUs sudo su -c 'nvidia-smi -acp UNRESTRICTED' timeout 10 sudo su -c '/usr/sbin/gpucontrol 100 0 1210 0 >> /var/log/gpucontrol.log 2>&1' timeout 10 sudo su -c '/usr/sbin/gpucontrol 100 0 100 1 >> /var/log/gpucontrol.log 2>&1' timeout 10 sudo su -c '/usr/sbin/gpucontrol 100 0 100 2 >> /var/log/gpucontrol.log 2>&1' timeout 10 sudo su -c '/usr/sbin/gpucontrol 100 0 1210 3 >> /var/log/gpucontrol.log 2>&1' for i in `ls /sys/class/drm/card*/device/hwmon/hwmon*/pwm1`;do timeout 10 sudo su -c "echo 255 > $i"; done for i in `ls /sys/class/drm/card*/device/power_dpm_force_performance_level`; do timeout 10 sudo su -c "echo high > $i"; done``

As you can see, this script is calling a "gpucontrol script": `#!/bin/bash PERCENT=$1 GRAPHICS=$2 MEMORY=$3 CARD=$4 cp -f /home/benjaminchodroff/.Xauthority /root/ if [ -z "$CARD" ]; then NUMGPU="$(nvidia-smi -L wc -l)"; echo "Setting up ${NUMGPU} GPU(s)"; n=0; while [ $n -lt $NUMGPU ]; do DISPLAY=:0 nvidia-settings -a [gpu:${n}]/GPUFanControlState=1 -a [fan:${n}]/GPUTargetFanSpeed=$1 -a [gpu:${n}]/GPUPowerMizerMode=1 -a [gpu:${n}]/GPUMemoryTransferRateOffset[3]=$3 -a [gpu:${n}]/GPUGraphicsClockOffset[3]=$2; let n=n+1; done else n=$CARD; DISPLAY=:0 nvidia-settings -a [gpu:${n}]/GPUFanControlState=1 -a [fan:${n}]/GPUTargetFanSpeed=$1 -a [gpu:${n}]/GPUPowerMizerMode=1 -a [gpu:${n}]/GPUMemoryTransferRateOffset[3]=$3 -a [gpu:${n}]/GPUGraphicsClockOffset[3]=$2 fi` There are so many helpful guides that I read - more than I can ever remember. The following links may help so I'm providing them here:

[https://gist.github.com/wangruohui/df039f0dc434d6486f5d4d098aa52d07](https://gist.github.com/wangruohui/df039f0dc434d6486f5d4d098aa52d07) [https://www.meebey.net/posts/ethereum\_gpu\_mining\_on\_linux\_howto/](https://www.meebey.net/posts/ethereum_gpu_mining_on_linux_howto/) [https://askubuntu.com/questions/902636/nvidia-smi-command-not-found-ubuntu-16-04](https://askubuntu.com/questions/902636/nvidia-smi-command-not-found-ubuntu-16-04) [https://askubuntu.com/questions/761136/ubuntu-16-04-nvidia-drivers-dont-work](https://askubuntu.com/questions/761136/ubuntu-16-04-nvidia-drivers-dont-work) [http://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Install.aspx](http://support.amd.com/en-us/kb-articles/Pages/AMDGPU-PRO-Install.aspx)

My rig is nothing fancy and certainly not bulletproof, but it allowed me to better understand blockchain mining for proof of work while making a few extra dollars.