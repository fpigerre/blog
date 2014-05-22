---
author: psgs
layout: post
excerpt: >
  Using block-chains to transfer data securely and anonymously is quite an interesting idea.
  Data could easily be traded, given, bought and sold anonymously, without others gaining access to the data.
  A data block-chain system could even hook into the BitCoin protocol and execute
  BitCoin transactions directly when transacting data for currency or vice versa.
categories:
  - thoughts
tags:
  - bitcoin
  - block-chain
  - data
  - transaction
  - gitchain
  - anonymity
  - distributed
reading-time: 5 minutes
title: Using Block-Chains to Transfer Data
---

Recently, I read quite an interesting post by Ilya Grigorik that explained the concepts and technologies behind a minimum viable block-chain [^1].
To summarize briefly, Ilya Grigorik  explained that traditionally, a block-chain is made up of 'blocks', each containing a number of valid, authenticated transactions.
Blocks are created by users who are using their CPU power to validate a number of transactions, usually for financial gain.
In terms of BitCoin[^2], the users validating transactions are dubbed 'miners', as they are 'mining' BitCoin using their CPU power by authenticating transactions and receiving BitCoin as a reward.

To understand the actions involved in processing block-chain transactions, a picture is often worth a lot more than a few words...

![BlockChain-Flowchart]({{ site.url }}/public/images/blockchain-flowchart.png)

Grigorik briefly mentioned at the start of the article he wrote, that it is possible that block-chains could be used for anonymous data 'transactions'.
This is reasonably plausible, since transactions across a block-chain can be relatively anonymous, due to the fact that, in order to carry out transactions, users simply need a cryptographic key.

> The block chain is agnostic to any "currency". In fact, it can (and will) be adapted to power many other use cases. As a result, it pays to understand the how and the why behind the "minimum viable block chain".
>
> *Ilyia Grigorik 05/05/2014 [^1]*

Using block-chains to transfer data securely and anonymously is quite an interesting idea. Data could easily be traded, given, bought and sold anonymously, without others gaining access to the data.
A data block-chain system could even hook into the BitCoin protocol and execute BitCoin[^2] transactions directly when transacting data for currency or vice versa.
Some worrying issues with the traditional block-chain model would need to be addressed, however. Would it be possible for transactions to be 'reversed', as can happen with block-chain forks, while a receiving user still keeps the data that was sent?
Would data still be able to be decrypted from the block-chain after the data has been sent, if an encryption key was to be cracked? Would adding data to a traditional 'transaction' object mean that a transaction's nonce[^4] properties would need to be changed, or would it slow down the P2P network considerably?

A solution to data being available for decryption after the data was sent via a transaction, could be to implement a 'seeding' process, similar to that which is defined in the BitTorrent[^5] protocol. A meta-data file or packet[^6] could be constructed using an encryption hash of the data to be sent.
The meta-data file or packet would then be encrypted with the transaction receiver's public encryption key. The meta-data packet could then be embedded in the block-chain transaction[^7] and safely propagated throughout the P2P network.
Once the destination node receives the block that contains the data transaction, the node would decrypt the meta-data file or packet and store it so that every host on the network could see that it possesses a valid meta-data file for a certain piece of data.
The sending node could then 'seed' the data that corresponds to the meta-data file's data hash to the host that displays the valid meta-data file. This way, even if the destination node's encryption keys were cracked after the data transaction took place, the data would remain secure.

![BlockChain-Meta]({{ site.url }}/public/images/blockchain-meta.png)

After the meta-data is embedded, block-chain transactions and blocks for a network making data transactions could look similar to the following representation:

![BlockChain-Meta]({{ site.url }}/public/images/blockchain-transactions.png)

I believe that, with the help of a number of dedicated volunteers, a data transfer protocol that uses block-chain technology could successfully solve many data transfer issues that involve anonymity and security.
Already, projects such as GitChain[^8], an open-source solution for distributed git repositories, have been levering block-chain technology in order to provide secure, reliable data.

*I hope you enjoyed these thoughts. If you feel that some details are inaccurate, or would like to have a chat, feel free to send me a [tweet](http://twitter.com/psgs00) or an [email](http://github.com/psgs).*

Further Reading:

* [TahoeLAFS](http://www.laser.dist.unige.it/Repository/IPI-1011/FileSystems/TahoeDFS.pdf), a 'least authority' file system designed for secure, distributed storage.
* [BitCoin Whitepaper](https://bitcoin.org/bitcoin.pdf), a white-paper detailing the original block-chain technology that was implemented in the early versions of BitCoin.

[^1]: [Minimum Viable Block Chain - igvita.com](http://www.igvita.com/2014/05/05/minimum-viable-block-chain/)
[^2]: [BitCoin - WikiPedia](http://en.wikipedia.org/wiki/Bitcoin)
[^3]: [BlockChain Fork Explained - Reddit/r/BitCoin](http://www.reddit.com/r/Bitcoin/comments/1a51xx/now_that_its_over_the_blockchain_fork_explained/)
[^4]: [Nonce - BitCoin Wiki](https://en.bitcoin.it/wiki/Nonce)
[^5]: [BitTorrent Protocol Specification - bittorrent.org](http://www.bittorrent.org/beps/bep_0003.html)
[^6]: [Torrent File - WikiPedia](http://en.wikipedia.org/wiki/Torrent_file)
[^7]: [Transaction - BitCoin Wiki](https://en.bitcoin.it/wiki/Transaction)
[^8]: [GitChain.org](http://gitchain.org)