---
title: NFT Stickers using NFC and POAP
tags: []
id: '2061'
categories:
  - - uncategorized
alias: /2022/06/30/nft-stickers-using-nfc-and-poap/
date: 2022-06-30 23:17:35
---

Non-fungible tokens have taken the web3 world by storm with rags to riches stories. However, did you know NFTs don't have to be used for speculative trading of digital artwork? Anything can be tokenized, and [POAP](https://poap.xyz/) "Proof of Attendance Protocol" allows tracking attendees at any event use the power of blockchain to provide an immutable record of who showed up by issuing each a free NFT. POAP is completely free to create events, and free to issue NFTs. I wanted to take the POAP NFT and give it a digital twin in the real world - something physical you could give out as a prize for attending that corresponded exactly with the digital NFT: a NFC sticker.
<!-- more -->
![](https://www.androidauthority.com/wp-content/uploads/2013/09/NFC-tag-induction-circuit.jpg)

NFC ntag213 25mm Sticker

NFC, or Near-Field Communication, tags are low cost, low bandwidth, small range, and are already supported by both iPhone and Android phones. Your phone likely already has this feature, but many are unaware of using it with these tags. The tags themselves come in multiple formats, but the ntag213 is a 25mm circle no thicker than a normal sticker that can store a small amount of data - such as a URL. Programming these tags can be done with any standard iPhone or Android phones using free software. Reading the tag requires no software to be installed on the phone, and the tag requires no battery as it uses the radio signals from your cellphone to power the incredibly tiny chips in the sticker.

![OffChain POAP NFC Stickers](/2022/06/offchainpoapsticker-1-300x225.jpg)

OffChain NFC Sticker POAP NFT

Normally, to claim a POAP NFT, you are given a claim code or QR code to scan or type into the [POAP app](https://poap.xyz/). However, we can take the list of claim URLs and write them to NFC ntag213 stickers to make a cost effective and very unique digital twin experience. Simply tapping the iPhone or Android against a regular looking sticker magically loads the URL to mint the NFT to the user's web3 wallet. 

I ordered 1000 ntag213 25mm stickers from Taobao for only 0.35 RMB each. I then ordered 1000 vinyl 25mm stickers for 108RMB. In total, that is only $0.07 USD per sticker in material costs. Sadly, I have yet to find a vendor that "does it all" to print a vinyl sticker, attach it on the NFC tag, and program it with the unique URL. As such, I had to manually peel 1000 stickers and attach them to the tags. After attaching each sticker, you will then need to write the URL contents. I found a free application "[NXP TagWriter](http://NFC TagWriter by NXP on the App Storehttps://apps.apple.com › app › nfc-tagwriter-by-nxp)" which claims to have CSV support, but in reality it doesn't work on iPhone (but perhaps csv imports work on Android?). Luckily, they did have a dataset export/import feature which can import a json-like file. I made a small python POAP URL to json file converter by using the ndef python library to hex encode and format the tag data contents.

![NXP TagWriter](/2022/06/nxp_tagwriter-139x300.jpg)

NXP TagWriter Write Multiple

Upload the resulting json file to your iPhone or Android, import it with NXP TagWriter, and then multi-write the tags one by one. Now, hand the tags out at your next event. Most people were… amazed. They couldn't believe that a sticker could be scanned by a phone, or that NFTs can be free. I hope this guide helps others recreate the POAP NFT via NFC sticker experience.