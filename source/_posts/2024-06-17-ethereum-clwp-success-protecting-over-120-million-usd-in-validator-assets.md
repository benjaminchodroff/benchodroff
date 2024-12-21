---
title: 'Ethereum CLWP Success: Protecting over $120 Million USD in Validator Assets'
tags:
  - clwp
  - cybersecurity
  - ethereum
id: '2105'
categories:
  - - uncategorized
alias: >-
  /2024/06/17/ethereum-clwp-success-protecting-over-120-million-usd-in-validator-assets/
date: 2024-06-17 13:47:58
---

In 2013, I ventured into the world of blockchain, and one phrase has always stuck with me: "Not your keys, not your coins." But what if a mistake happens and your private key is compromised? This situation is more common than many admit. While it often results in the loss of assets, it can be at best an expensive life lesson and teachable moment. However, with a bit of luck and a lot of help, it is still possible to rescue over $120 million USD like I did by during the Ethereum Shapella hardfork on April 12, 2023 by creating EIP-4736 "CLWP" Consensus Layer Withdrawal Protection.
<!-- more -->
## The Ethereum Proof of Stake Shift

Ethereum began to shift to Proof of Stake (PoS) in 2020. Moving away from Proof of Work (PoW) mining allowed Ethereum users to earn yields on their ETH by setting up a small server for staking. However, while staking was possible, withdrawals were initially not possible, with no clear timeline for when this would be supported. Driven by my desire to learn, I decided to set up a validator knowing it would be years before I could withdraw.

Setting up an Ethereum validator is daunting, even for the tech-savvy. It involves open-source code, Linux hardening, generating a cryptographic seed phrase, and sending 32 ETH per validator to a smart contract. The process is now well-documented and understood, but I made several mistakes along the way.

## Mistakes and Compromises

Catastrophes often result from multiple failures, and a lack of appropriate risk management. My first mistake was not informing my wife about my grand staking experiment using a material portion of our Ethereum. My second mistake was copying the newly created Ethereum validator seed phrase into my electronic notes during setup. I normally would never make this mistake with a private key material as I always inscribe my seed phrases on metal and store them in offline secure locations with a hardware wallet, but I am human and this validator setup process was new. These validator setup notes were backed up on a USB key, which were then backed up on a NAS that was compromised. Needless to say, there were many failures in this chain of events that will never happen again.

In December 2021, I received a suspicious email from an unknown contact. As I read their email, my worst fears were confirmed: my validator seed phrase was compromised, and the sender demanded a "reward." I told my wife about the situation.

![](/2024/06/eth2compromise-1024x281.jpg)

Welcome to the race

## Navigating the Reward

Together, we decided to pay the "reward" to the greyhat, hoping they would honor the agreement to delete the validator seed phrase. While many experts advise against paying ransoms, I was grateful for the early notification, as it gave me a chance to fight back. A year later, as predicted, the greyhat returned, demanding even more "reward". This time, I ignored their messages as I already had a plan in mind for my unique situation.

![](/2024/06/eth2compromiseagain-1024x441.jpg)

不回答! Do not answer!

## A Unique Situation

At that time in 2021, Ethereum did not allow withdrawals. This meant that neither I, the greyhat, nor any other potential attackers who had compromised my validator seed phrase could access the funds. This was an unusual situation. Typically, once a seed phrase is compromised, assets are quickly drained from the wallet by the attacker. On the blockchain, ownership is psuedo-anonymous. Despite knowing my seed phrase was compromised, no one could act on it.

I realized I could prove my validator ownership by signing a message with my deposit address, which was still securely inscribed on metal in a safe and certain that my attacker had no access to. However, the Proof of Stake consensus on the blockchain could not consider this evidence. To outpace the attacker, I needed more people to help broadcast my message before an attacker could.

## The Birth of Consensus Layer Withdrawal Protection (CLWP)

I researched the Ethereum Proof of Stake network and [discovered others who had their validator seed phrases compromised](https://ethresear.ch/t/preparing-withdrawals-for-compromised-validator-or-withdrawal-keys-fao-zilm/10453/1). I collaborated with researchers, like Jim McDonald, who were intrigued by using the deposit address to prove validator ownership. Unfortunately, the Ethereum withdrawal specifications in draft did not accommodate this method.

We found another way: social consensus. By convincing a broader community of the evidence and incentivizing their behavior, we developed the Consensus Layer Withdrawal Protection (CLWP). Documented in [EIP-4736](https://eips.ethereum.org/EIPS/eip-4736), CLWP proposed creating a list of verifiable messages to broadcast via Ethereum consensus nodes at the Shanghai Capella hard fork. The goal was to propagate CLWP messages faster than an attacker, [leveraging a volunteer network "sneakernet" using submissions via a GitHub repository](https://github.com/benjaminchodroff/ConsensusLayerWithdrawalProtection) and low latency bots, with [social arbitration moderation using Kleros curated lists](https://curate.kleros.io/tcr/1/0x479083b5343aB89bb39608e3176D750c8A6957B5). None of this social consensus changed Ethereum on-chain consensus, nor could guarantee success against an attacker. Volunteers were incentivized through altruism or self preservation of their own compromised validator by statistically shifting success in favor of legitimate validator owners.

![](/2024/06/CLWP-Diagram.drawio.png)

Social consensus can win against on-chain consensus

## The Outcome

With the help of many brilliant engineers and volunteers, we prepared for this sprint, documenting the evolving withdrawal specifications and [convincing Ethereum client teams and users to adopt our ideas](https://clwp.xyz/). The result? On 4/12/2023 at the Ethereum Shanghai Capella "Shapella" hardfork, the CLWP project helped 2,133 validators protect more than 68,256 ETH in principal and additional rewards, valued over $120 million USD (and worth much more now). Every single validator, including mine, was successfully protected.

In a twist, my validators were not protected by CLWP. Instead, due to a race condition timing attack bug discovered in the last days by an Ethereum Researcher, my validators were rescued by a secret sniping bot that was designed to outperform the CLWP protocol. Even the Ethereum consensus layer is a [dark forest](https://www.paradigm.xyz/2020/08/ethereum-is-a-dark-forest).

This journey taught me invaluable lessons, allowed me to assist others, and introduced me to some of the most brilliant minds in the cryptocurrency community. I hope my story inspires others to keep their seed phrases secure offline and view mistakes as opportunities for growth and learning.

## Thank you

I cannot thank enough all the people who helped me. Special thanks to Tobes for running [@EthCLWP](https://x.com/EthCLWP) and website, [@jgm](https://x.com/jgm) for his expert knowledge in ethdo and Ethereum withdrawals, [@offchainglobal](https://x.com/offchainglobal) for organizing volunteers, [@superphiz](https://x.com/superphiz) for getting the word out in the community, [@ethStaker](https://x.com/ethStaker), [ETH Magicians](https://ethereum-magicians.org/), [Ethereum Foundation R&D](https://blog.ethereum.org/category/research-and-development) for helping us connect to the best minds in the industry, [@KlerosCurate](https://x.com/KlerosCurate) for assisting us with moderating social consensus, [@Allnodes](https://x.com/Allnodes) and [@LidoFinance](https://x.com/LidoFinance) for broadcasting CLWP, [@poojaranjan19](https://x.com/poojaranjan19) at [Eth Cat Herders](https://www.ethereumcatherders.com/) for helping get the EIP through the process, [Flashbots](https://www.flashbots.net/), all of the Consensus client teams [@sigp\_io](https://x.com/sigp_io) [@lodestar\_eth](https://x.com/lodestar_eth) [@prylabs](https://x.com/prylabs) [@Teku\_ConsenSys](https://x.com/Teku_Consensys) for supporting CLWP, and the [hundreds of people who volunteered to submit their validators withdrawal address and broadcast the messages](https://github.com/benjaminchodroff/ConsensusLayerWithdrawalProtection).

To the greyhat who notified me that I was compromised: thank you. I hope one day to get a drink together. The world is more friendly than it seems when good people work together.

![](/2024/06/web3hostile-1024x276.jpg)

The world _is_ more friendly than it seems

https://www.youtube.com/watch?v=C8rxSljl2PM&t=18s

PeeP an EIP: EIP-4736 Consensus Layer Withdrawal Protection

https://www.youtube.com/watch?v=EWkGyorgpAg

CLWP Presentation