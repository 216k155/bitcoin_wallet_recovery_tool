# Bitcoin Wallet Brute Force Recovery Tool

###Disclaimer

**I am not responsible for any consequences from the use/misuse of this software by ANY party, and provide it for reference only. Please backup any wallets you plan to attempt a recovery on, and note that this program will store your private keys in a plaintext image on your computer. See the LICENSE for full disclaimer.**

###Introduction

Entropy happens, and bitcoin wallets can get corrupted to the point where they cannot be recovered by the bitcoin-core software or pyWallet. This software checks the entire file for private keys, and makes no assumptions about the presence/correctness of formatting or metadata. _As long as the bitcoin private key(s) within the file is not corrupted or encrypted, this software will find and recover the bitcoin stored within_, and exports it to an easy-to-scan WIF QR code for import into a mobile wallet.  

####Design

This software establishes a 256-bit sliding window at the start of the wallet file. It then moves one byte ahead and exports the next 32 bytes, until it reaches the end of the file. Each set of 32 bytes is a potential EC keypair private key. 

It then checks the balance at each private key using a web API, 150 key candidates at a time. The vast majority of the candidates will just have been generated from metadata and will have a balance of 0 bitcoin. When the program does find a positive balance, it saves the keypair. 

Finally, any private keys with positive bitcoin balances are exported to WIF, and image files with QR codes are generated. Simply scan these with any of the major mobile wallet programs, and the bitcoin will be immediately spend-able. 

###How to use

Run the Main.java file in a maven-compatible Java 8 environment. The software will prompt you to specify the location of the wallet file, or you can send it is as the first arg. 

This wallet recovery software will output the private keys in Wallet Import Format (WIF) QR codes in the same directory as the wallet file being recovered. 

###Limitations

This recovery tool cannot be used on encrypted wallets. This wallet assumes private keys are directly available in the wallet, and cannot reconstruct addresses from master seeds in HD wallets, etc. 

####Reliance on 3rd-party web APIs

This wallet recovery tool leverages the [Blockchain.info API](https://blockchain.info/api) to quicky check tens of thousands of bitcoin balances. The wallet client is rate-limited when it consumes this API; please don't abuse this web resource and ruin it for all. 

###License 

This software is licensed under the GPL license, available in the LICENSE file. 

##Copyright (C) 2017 Alex Meijer