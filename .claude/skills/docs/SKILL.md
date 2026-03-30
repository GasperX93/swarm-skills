---
name: docs
description: Route conceptual and technical questions to the correct authoritative Swarm documentation
user-invocable: true
---

# Swarm Documentation Reference

Use this skill when a developer asks a conceptual or technical question that is not explicitly covered by the steps in another skill. Do not answer from prior training knowledge — identify the most relevant page from the map below and share the direct link.

## What to Do

1. Identify the topic from the user's question.
2. Find the most specific matching link below.
3. Share that link and suggest the user read that page.
4. If multiple topics apply, share all relevant links.
5. If no specific page matches, direct the user to https://docs.ethswarm.org to search there.

Do not attempt to summarize or paraphrase the documentation content. The goal is to get the user to the authoritative source, not to answer from training data.

## Documentation Map

### Node installation
- Full quickstart guide: https://docs.ethswarm.org/docs/bee/installation/quick-start
- System requirements: https://docs.ethswarm.org/docs/bee/installation/quick-start#requirements
- Installing the Bee binary: https://docs.ethswarm.org/docs/bee/installation/quick-start#install-bee
- Installing swarm-cli: https://docs.ethswarm.org/docs/bee/installation/quick-start#install-swarm-cli
- Starting your node for the first time: https://docs.ethswarm.org/docs/bee/installation/quick-start#start-your-bee-node
- Getting your node's wallet address: https://docs.ethswarm.org/docs/bee/installation/quick-start#get-your-nodes-address
- Waiting for sync after startup (~5 min): https://docs.ethswarm.org/docs/bee/installation/quick-start#wait-to-sync-5-minutes
- Installing with Docker: https://docs.ethswarm.org/docs/bee/installation/docker
- Docker node setup process: https://docs.ethswarm.org/docs/bee/installation/docker#node-setup-process
- Running a hive (multiple Docker nodes): https://docs.ethswarm.org/docs/bee/installation/docker#run-a-hive
- Building Bee from source: https://docs.ethswarm.org/docs/bee/installation/build-from-source

### Funding your node
- Funding overview: https://docs.ethswarm.org/docs/bee/installation/fund-your-node
- What xDAI is used for: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#xdai-is-required-for
- What xBZZ is used for: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#xbzz-is-required-for
- How much funding is needed by use case: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#token-amounts-by-use-case
- How to get xDAI: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#how-to-get-xdai
- How to get xBZZ: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#how-to-get-xbzz
- Testnet tokens (Sepolia ETH & sBZZ): https://docs.ethswarm.org/docs/bee/installation/fund-your-node#getting-testnet-tokens-sepolia-eth--sbzz
- Node wallet and chequebook explained: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#node-wallet--chequebook
- How to fund your wallet: https://docs.ethswarm.org/docs/bee/installation/fund-your-node#funding-your-wallet

### Node configuration
- Configuration overview and priority rules: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#configuration-methods-and-priority
- Using command-line arguments: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#command-line-arguments
- Using environment variables: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#environment-variables
- Using a YAML config file: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#yaml-configuration-file
- Generating a YAML config file automatically: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#manually-generating-yaml-config-file-for-bee-start
- Node types explained (full / light / ultra-light): https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#node-types
- How to set node type: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#how-to-set-node-type
- Full node configuration example: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#full-node-configuration
- Light node configuration example: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#light-node-configuration
- Ultra-light node configuration example: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#ultra-light-node-configuration
- Default data and config directory locations: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#default-data-and-config-directories
- Creating a password: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#create-password
- Setting the blockchain RPC endpoint: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#setting-blockchain-rpc-endpoint
- Available RPC providers: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#rpc-providers
- Configuring swap initial deposit: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#configuring-swap-initial-deposit-optional
- NAT address configuration: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#nat-address
- ENS resolution configuration: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#ens-resolution-optional
- Sepolia testnet configuration: https://docs.ethswarm.org/docs/bee/working-with-bee/configuration#sepolia-testnet-configuration

### Connectivity and networking
- Connectivity overview: https://docs.ethswarm.org/docs/bee/installation/connectivity
- Networking basics and IP addresses: https://docs.ethswarm.org/docs/bee/installation/connectivity#networking-basics
- Home and business networks behind NAT: https://docs.ethswarm.org/docs/bee/installation/connectivity#home-commercial-and-business-networks-and-other-networks-behind-nat
- UPnP automatic NAT traversal: https://docs.ethswarm.org/docs/bee/installation/connectivity#automatic-universal-plug-and-play-upnp
- Manual port forwarding setup: https://docs.ethswarm.org/docs/bee/installation/connectivity#manual-configure-your-router-and-bee
- Using multiple P2P transports (TCP, WS, WSS): https://docs.ethswarm.org/docs/bee/installation/connectivity#using-multiple-p2p-transports-tcp-ws-wss
- Troubleshooting connectivity: https://docs.ethswarm.org/docs/bee/installation/connectivity#troubleshooting-connectivity

### Logging
- Logging overview: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files
- Log file locations by OS: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#log-locations
- Log levels and what they mean: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#logging-levels
- Setting log verbosity: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#setting-verbosity
- Fine-grained per-logger control: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#fine-grained-logging-control
- Retrieving the loggers list: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#1-retrieving-loggers-list
- Adjusting logger verbosity at runtime: https://docs.ethswarm.org/docs/bee/working-with-bee/logs-and-files#2-adjusting-logger-verbosity

### Backups
- Backup overview: https://docs.ethswarm.org/docs/bee/working-with-bee/backups
- What files to back up: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#bee-files
- Data directory locations by OS: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#data-directory-locations
- Backing up node data: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#back-up-your-node-data
- Backing up your password: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#back-up-your-password
- Backing up blockchain keys only: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#back-up-blockchain-keys-only
- Importing keys into MetaMask: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#metamask-import
- Getting your private key from keystore: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#get-private-key-from-keystore-and-password
- Restoring from backup: https://docs.ethswarm.org/docs/bee/working-with-bee/backups#restore-from-backup

### Cashing out (full nodes only)
- Cashing out overview: https://docs.ethswarm.org/docs/bee/working-with-bee/cashing-out
- Withdrawing xBZZ rewards and native xDAI: https://docs.ethswarm.org/docs/bee/working-with-bee/cashing-out#withdrawing-xbzz-rewards-and-native-xdai
- Cashing out SWAP cheques: https://docs.ethswarm.org/docs/bee/working-with-bee/cashing-out#cashing-out-cheques-swap
- Managing uncashed cheques: https://docs.ethswarm.org/docs/bee/working-with-bee/cashing-out#managing-uncashed-cheques

### Staking (full nodes only)
- Staking overview: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#staking-overview
- Staking quickstart guide: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#quickstart-guide
- How to stake xBZZ: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#step-2-stake-xbzz
- Checking stake status: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#check-status
- Partial stake withdrawals: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#partial-stake-withdrawals
- Reserve doubling (increasing storage): https://docs.ethswarm.org/docs/bee/working-with-bee/staking#reserve-doubling
- Maximizing staking rewards: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#maximize-rewards
- Neighborhood selection and stake density: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#neighborhood-selection
- Neighborhood hopping: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#neighborhood-hopping
- Migrating stake to a new node: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#stake-migration
- Troubleshooting frozen node: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#frozen-node
- Repairing corrupt reserve: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#repairing-corrupt-reserve
- Node not participating in redistribution: https://docs.ethswarm.org/docs/bee/working-with-bee/staking#node-not-participating-in-redistribution

### Monitoring your node
- Monitoring overview: https://docs.ethswarm.org/docs/bee/working-with-bee/monitoring

### Postage stamps
- How stamps work: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch
- Funding your wallet before buying: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#fund-your-nodes-wallet
- Buying a stamp batch: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#buying-a-stamp-batch
- Choosing depth (capacity): https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#choosing-depth
- Choosing amount (duration): https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#choosing-amount
- Mutable vs immutable stamps: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#mutable-or-immutable
- Depth and amount calculators: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#calculators
- Listing your stamps: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#viewing-stamps
- Checking stamp TTL (time to live): https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#checking-the-remaining-ttl-time-to-live-of-your-batch
- Topping up a stamp (extending duration): https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#top-up-your-batch
- Diluting a stamp (increasing capacity): https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#dilute-your-batch
- Stewardship and content retrievability: https://docs.ethswarm.org/docs/develop/tools-and-features/buy-a-stamp-batch#stewardship

### Uploading and downloading
- Upload and download overview: https://docs.ethswarm.org/docs/develop/upload-and-download
- Single file upload with bee-js (Node.js): https://docs.ethswarm.org/docs/develop/upload-and-download#single-file--nodejs
- Single file upload with bee-js (browser): https://docs.ethswarm.org/docs/develop/upload-and-download#single-file--browser
- Multiple files with bee-js (browser): https://docs.ethswarm.org/docs/develop/upload-and-download#multiple-files--browser
- Multiple files with bee-js (Node.js): https://docs.ethswarm.org/docs/develop/upload-and-download#multiple-files--nodejs
- Upload and download with swarm-cli: https://docs.ethswarm.org/docs/develop/upload-and-download#upload--download-with-swarm-cli
- Upload and download with the Bee API: https://docs.ethswarm.org/docs/develop/upload-and-download#upload--download-with-the-bee-api-advanced
- Using the /bzz endpoint: https://docs.ethswarm.org/docs/develop/upload-and-download#upload-with-bzz

### Pinning content locally
- Pinning overview: https://docs.ethswarm.org/docs/develop/tools-and-features/pinning
- Pinning content during upload: https://docs.ethswarm.org/docs/develop/tools-and-features/pinning#pin-during-upload
- Managing pinned content: https://docs.ethswarm.org/docs/develop/tools-and-features/pinning#administer-pinned-content
- Unpinning content: https://docs.ethswarm.org/docs/develop/tools-and-features/pinning#unpinning-content
- Pinning already-uploaded content: https://docs.ethswarm.org/docs/develop/tools-and-features/pinning#pinning-already-uploaded-content

### Chunk types (Swarm internals)
- Chunk types overview: https://docs.ethswarm.org/docs/develop/tools-and-features/chunk-types
- Content addressed chunks (CAC): https://docs.ethswarm.org/docs/develop/tools-and-features/chunk-types#content-addressed-chunks
- Trojan chunks (PSS): https://docs.ethswarm.org/docs/develop/tools-and-features/chunk-types#trojan-chunks
- Single owner chunks (SOC — used by feeds and GSOC): https://docs.ethswarm.org/docs/develop/tools-and-features/chunk-types#single-owner-chunks
- Custom chunk types: https://docs.ethswarm.org/docs/develop/tools-and-features/chunk-types#custom-chunk-types

### Feeds (dynamic content)
- What feeds are and how they work: https://docs.ethswarm.org/docs/develop/tools-and-features/feeds#what-are-feeds
- Creating and updating a feed: https://docs.ethswarm.org/docs/develop/tools-and-features/feeds#creating-and-updating-a-feed
- Feeds and ENS (no transaction cost on update): https://docs.ethswarm.org/docs/develop/tools-and-features/feeds#no-more-ens-transaction-charges
- Feed use cases: https://docs.ethswarm.org/docs/develop/tools-and-features/feeds#use-cases-for-feeds
- Why mutable content needs feeds (the immutability problem): https://docs.ethswarm.org/docs/develop/dynamic-content#the-immutability-problem
- Feed manifests and stable URLs: https://docs.ethswarm.org/docs/develop/dynamic-content#feed-manifests--stable-urls
- How feed resolution works end to end: https://docs.ethswarm.org/docs/develop/dynamic-content#how-it-all-fits-together
- Generating a publisher key: https://docs.ethswarm.org/docs/develop/dynamic-content#create-a-publisher-key
- Writing and reading a feed: https://docs.ethswarm.org/docs/develop/dynamic-content#write-and-read-a-feed
- Updating a feed: https://docs.ethswarm.org/docs/develop/dynamic-content#update-the-feed
- Full blog example project with bee-js: https://docs.ethswarm.org/docs/develop/dynamic-content#example-project--simple-blog

### Website hosting
- Host a site with swarm-cli: https://docs.ethswarm.org/docs/develop/host-your-website/#host-a-site-with-swarm-cli
- Upload and access by hash (one-time): https://docs.ethswarm.org/docs/develop/host-your-website/#upload--access-by-hash
- Feed-based hosting for seamless updates (swarm-cli): https://docs.ethswarm.org/docs/develop/host-your-website/#use-feeds-for-seamless-updates---swarm-cli
- Host a site with bee-js: https://docs.ethswarm.org/docs/develop/host-your-website/#host-a-website-with-bee-js
- Connect site to ENS domain: https://docs.ethswarm.org/docs/develop/host-your-website/#connect-site-to-ens-domain

### Access control (ACT)
- ACT upload, download, and grantee management (swarm-cli): https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control
- Uploading with ACT encryption: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#upload
- Downloading ACT-encrypted content: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#download
- Grantee management overview: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#grantee-management
- Creating a grantee list: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#create
- Adding or revoking grantees: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#patch
- Viewing the grantee list: https://docs.ethswarm.org/docs/develop/access-the-swarm/access-control#get
- ACT with bee-js (full guide): https://bee-js.ethswarm.org/docs/act
- Creating a grantee list with bee-js: https://bee-js.ethswarm.org/docs/act#create-grantees-list
- Updating grantees with bee-js: https://bee-js.ethswarm.org/docs/act#update-grantees-list
- Getting grantees with bee-js: https://bee-js.ethswarm.org/docs/act#get-grantees-list
- Uploading with ACT using bee-js: https://bee-js.ethswarm.org/docs/act#upload-with-act
- Downloading with ACT using bee-js: https://bee-js.ethswarm.org/docs/act#download-with-act

### Messaging — GSOC
- GSOC overview (official docs): https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc
- bee-js GSOC methods reference: https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#bee-js-gsoc-methods
- gsocMine (key derivation for a target overlay): https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#beegsocmine
- gsocSend (sending a message): https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#beegsocsend
- gsocSubscribe (listening for messages): https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#beegsocsubscribe
- Service node script example: https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#service-node-script
- Writer node script example: https://docs.ethswarm.org/docs/develop/tools-and-features/gsoc#writer-node-script
- GSOC guide with bee-js (including GSOC vs PSS comparison): https://bee-js.ethswarm.org/docs/gsoc
- GSOC vs PSS comparison: https://bee-js.ethswarm.org/docs/gsoc#gsoc-vs-pss
- Setting up a GSOC listener (receiver): https://bee-js.ethswarm.org/docs/gsoc#set-up-a-listener-receiver-node
- Sending a GSOC message: https://bee-js.ethswarm.org/docs/gsoc#send-a-message-sender-node

### Messaging — PSS
- PSS overview (official docs): https://docs.ethswarm.org/docs/develop/tools-and-features/pss
- Subscribing and receiving PSS messages: https://docs.ethswarm.org/docs/develop/tools-and-features/pss#subscribe-and-receive-messages
- Sending PSS messages: https://docs.ethswarm.org/docs/develop/tools-and-features/pss#send-messages
- PSS two-node test network setup: https://docs.ethswarm.org/docs/develop/tools-and-features/pss#send-messages-in-a-test-network
- PSS guide with bee-js: https://bee-js.ethswarm.org/docs/pss
- What PSS is and how it works: https://bee-js.ethswarm.org/docs/pss#what-is-pss
- Getting recipient info (overlay + PSS public key): https://bee-js.ethswarm.org/docs/pss#get-recipient-info-full-node-only
- Listening for PSS messages (full node): https://bee-js.ethswarm.org/docs/pss#listen-for-messages-full-node
- Sending a PSS message (light or full node): https://bee-js.ethswarm.org/docs/pss#send-message-light-or-full-node
- Encrypting PSS messages with public key: https://bee-js.ethswarm.org/docs/pss#encrypt-with-pss-public-key

### bee-js SDK — key methods
- Full bee-js API reference (Bee class): https://bee-js.ethswarm.org/docs/api/classes/Bee
- uploadData / uploadFile / uploadFiles: https://bee-js.ethswarm.org/docs/api/classes/Bee
- downloadData / downloadFile: https://bee-js.ethswarm.org/docs/api/classes/Bee#downloaddata
- buyStorage (high-level stamp purchase): https://bee-js.ethswarm.org/docs/api/classes/Bee#buystorage
- createPostageBatch (low-level stamp purchase): https://bee-js.ethswarm.org/docs/api/classes/Bee#createpostagebatch
- topUpBatch (extend stamp duration): https://bee-js.ethswarm.org/docs/api/classes/Bee#topupbatch
- diluteBatch (increase stamp capacity): https://bee-js.ethswarm.org/docs/api/classes/Bee#dilutebatch
- getPostageBatches (list all stamps): https://bee-js.ethswarm.org/docs/api/classes/Bee#getpostagebatches
- createFeedManifest: https://bee-js.ethswarm.org/docs/api/classes/Bee#createfeedmanifest
- makeFeedReader: https://bee-js.ethswarm.org/docs/api/classes/Bee#makefeedreader
- makeFeedWriter: https://bee-js.ethswarm.org/docs/api/classes/Bee#makefeedwriter
- createGrantees: https://bee-js.ethswarm.org/docs/api/classes/Bee#creategrantees
- getGrantees: https://bee-js.ethswarm.org/docs/api/classes/Bee#getgrantees
- patchGrantees: https://bee-js.ethswarm.org/docs/api/classes/Bee#patchgrantees
- gsocMine / gsocSend / gsocSubscribe: https://bee-js.ethswarm.org/docs/api/classes/Bee#gsocmine
- pssSend / pssSubscribe / pssReceive: https://bee-js.ethswarm.org/docs/api/classes/Bee#psssend
- isConnected / checkConnection: https://bee-js.ethswarm.org/docs/api/classes/Bee#isconnected
- getNodeAddresses: https://bee-js.ethswarm.org/docs/api/classes/Bee#getnodeaddresses
- getWalletBalance: https://bee-js.ethswarm.org/docs/api/classes/Bee#getwalletbalance
- pin / getAllPins / reuploadPinnedData: https://bee-js.ethswarm.org/docs/api/classes/Bee#pin
- isReferenceRetrievable: https://bee-js.ethswarm.org/docs/api/classes/Bee#isreferenceretrievable
- getStake / depositStake: https://bee-js.ethswarm.org/docs/api/classes/Bee#getstake
- getRedistributionState: https://bee-js.ethswarm.org/docs/api/classes/Bee#getredistributionstate

### Swarm Desktop app
- Desktop app introduction: https://docs.ethswarm.org/docs/desktop/introduction
- Installing Swarm Desktop: https://docs.ethswarm.org/docs/desktop/install#download-and-install-swarm-desktop
- Tour of the Desktop UI: https://docs.ethswarm.org/docs/desktop/install#tour-of-swarm-desktop
- Ultra-light vs light node in Desktop: https://docs.ethswarm.org/docs/desktop/install#ultra-light-and-light-nodes
- Files tab (upload/download via UI): https://docs.ethswarm.org/docs/desktop/install#files-tab
- Account tab (wallet and stamps): https://docs.ethswarm.org/docs/desktop/install#account-tab
- Settings tab: https://docs.ethswarm.org/docs/desktop/install#settings-tab
- Status tab (node health): https://docs.ethswarm.org/docs/desktop/install#status-tab

### bee-js SDK — SOC (Single Owner Chunks)
- makeSOCReader (read raw SOC): https://bee-js.ethswarm.org/docs/api/classes/Bee#makesocreader
- makeSOCWriter (write raw SOC): https://bee-js.ethswarm.org/docs/api/classes/Bee#makesocwriter

### Swarm fundamentals
- Main documentation hub: https://docs.ethswarm.org

### Bee HTTP API reference
- Full API reference: https://docs.ethswarm.org/api/

### Tools
- swarm-cli: https://github.com/ethersphere/swarm-cli
- create-swarm-app: https://github.com/ethersphere/create-swarm-app
- swarm-mcp (AI agent integration): https://github.com/ethersphere/swarm-mcp
- Beeport (no-node website deploy): https://beeport.ethswarm.org

### Support
- Discord: https://discord.gg/hyCr9BMX9U
- Bee GitHub issues: https://github.com/ethersphere/bee/issues
- bee-js GitHub issues: https://github.com/ethersphere/bee-js/issues
