# RTCTunnel

RTCTunnel builds network tunnels over WebRTC.

_WARNING: this is a proof of concept and should not be used in production. Things like disconnects and invalid data are not handled gracefully and will result in the program crashing._

An overview of the application and how and why it was built is available here: [RTCTunnel: Building a WebRTC Proxy with Go](http://www.doxsey.net/blog/rtctunnel--building-a-webrtc-proxy-with-go).

## Installation

RTCTunnel can be installed via `go get`:

```bash
go get github.com/rtctunnel/rtctunnel/cmd/rtctunnel
```

Or downloaded from the releases page (for linux). Currently the [pions/webrtc](https://github.com/pions/webrtc) library requires libopenssl to compile.

## Usage

RTCTunnel creates a network tunnel over WebRTC between two peers. Those peers are identified by a public key. 

To use RTCTunnel first create a config with:

```bash
rtctunnel init
```

You can see info with:

```bash
rtctunnel info
```

Once you've done that on both peers, copy the two public keys and add a route. A route has four components: a local peer, a local port, a remote peer and a remote port. All network connections and data sent to the local port will be forwarded to the remote peer. For example:

```bash
export CLIENT_KEY=
export SERVER_KEY=
rtctunnel add-route \
    --local-peer=$CLIENT_KEY \
    --local-port=6379 \
    --remote-peer=$SERVER_KEY \
    --remote-port=6379
```

The route must be added on both peers and the `CLIENT_KEY` and `SERVER_KEY` should be set to the peer keys.

Once the routes are added you can run rtctunnel with:

```bash
rtctunnel run
```

Typically it would be run in the background.

A docker-compose example is available in [examples/redis](https://github.com/rtctunnel/rtctunnel/tree/master/examples/redis).
