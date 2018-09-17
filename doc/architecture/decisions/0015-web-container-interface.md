# 15. Web <> Container interface

Date: 2018-09-14

## Status

Proposed

## Context
* We have an implementation of a Holochain container that can load QML based UIs: https://github.com/holochain/holosqape
* We also want to support HTML/JS/CSS based UIs
* We need to enable pure Web clients to connect to a HoloPort for the Holo use-case

    

## Requirements
It seems we need the advantages of both worlds:
* having a rich context in which apps run so we can do bridging and other sophisticated
  app composability features (the need for a container/composer)
* we need clients that are running any kind of UI (or connecting with IoT devices)
  to be able to connect with Holochain apps easily
### Mandatory
- Any full-node Holochain instance must be running *persistenly* (i.e. not tied to the browser process)
  to be available to the DHT, which makes it unattractive to run the whole instance in a web browser.


### Desirable
- Bi-directional communication (pub/sub)
    From the UI client side. It should be possible to subscribe to certain events such as when a node receives new data, receives a message etc.
- Exposed through an object abstraction in the browser runtime (like web3)
- Can also be used to connect to a remote node for compatability with Holo

## Considered Solutions

### 1. Daemon container that accepts function calls via HTTP requests
This is the solution implemented in proto. 
- A single daemon container exposes end-points for calling functions namespaced by app and by zome
- This will need to expose new functions such as getInstalledApps
- Maybe use JSON-RPC spec this time?

### 2. Daemon container that exposes web sockets for bi-directional communication
Extension of above to also allow back->front end calls via websockets. 
- Will need a way for the front-end to register for certain messages
    - New data in DHT shard, direct message received, validationCallback, brigeGenesis etc.

### 2. Daemon container communicating with browser plug-in via IPC
This is similar to metamask for ethereum communicating with a local node via IPC. 
- Plug-in exposes holochain calls to the javascript running on the page via an injected object
- Plug-in communicates with daemon via an ipc file or similar
- Not sure yet what substantial advantage this holds over browser without plug-in

### 3. Holochain container runs a custom browser that exposes calls directly
This is comparable to the Mist browser for Ethereum and currently implemented for QML
front-ends with HoloSqape. 
This could be re-done with web technology such as [Electron](https://electronjs.org/) and [Holochain-nodejs](https://github.com/holochain/holochain-nodejs).  
- Similar to above but removes the need for IPC
- Would need to continue running in the background when no windows are open


### Other Discussions

There is also the possibility to implement multiple of the above (as Ethereum has). This might be more practical where there are many more people working on core. The real problem is which to do first

## Decision

Decision here...

## Consequences

Consequences here...

