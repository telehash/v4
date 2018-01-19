# Telehash protocol version 4 workspace

> Any work in here is considered a work-in-progress, do not consider anything here to be final.

## Overview

Telehash began almost 10 years ago and has had two major evolutions in that time, the earlier ones being more focused on a distributed hash table and more recently focusing on the security and end-to-end privacy.

This next evolution continues that tradition by moving the protocol's self-defined primary data structures all to use newer standards instead, [_JavaScript Object Signing and Encryption_ (JOSE)](https://datatracker.ietf.org/wg/jose/documents/) and [_Constrained Binary Object Representation_ (CBOR)](https://datatracker.ietf.org/wg/cbor/documents/).  The lightweight nature of Telehash version 3 (v3) has led to adoption in IoT use cases and version 4 (v4) is embracing constrained environments as a principal architecture.

## Scratch / Notes

* All messages will become [_JSON Web Encryption_ (JWE)](https://tools.ietf.org/html/rfc7516) objects.
* Handshakes will contain a [_JSON Web Signature_ (JWS)](https://tools.ietf.org/html/rfc7515) + ephemeral [_JSON Web Key_ (JWK)](https://tools.ietf.org/html/rfc7517), and will establish a single channel (instead of a v3 'link'). Multiple channels can exist simultaneously between peers
* All JWE/JWS bodys will be re-encoded as minimized CBOR using [_JSON Constrained Representation_ (JCOR)](https://github.com/quartzjer/JCOR) (CBOR-based JSON)
* Primary/Required [_JSON Web Algorithm_ (JWA)](https://tools.ietf.org/html/rfc7518) is ECC P-256, implementations should support others
* Bindings will be defined for common transports (HTTPS, CoAP, MQTT, XMPP, USB/CDC, UART, etc)
* Reliable channels will not be in v4, messages must be stand-alone or a reliable transport must be used instead
* Communities are being introduced to increase metadata privacy, every endpoint must be part of one or more communities
* Hashnames are unique to each community
* Bindings to [OpenID Connect](https://en.wikipedia.org/wiki/OpenID_Connect) for seamless bootstrap from external identity systems

### Communities

* a community is defined by a shared JWK
* they are either public or private
* private must be known before joining the community
* public maybe open or password protected
* public has a UTF-8 string name, KDF used to derive the JWK, when password protected it is an additional KDF
* JWE encrypted by the community JWK is used for discovery, can be broadcast on local networks
* JWE contains a JWS+JWK of the joining/announcing peer, JWK is optional on private networks
* when public the community name is available in the JWE headers
* every peer in a community has a unique hashname based on the hash of the thumbprints of the community+peer's JWKs
* advisable to join public communities with a new peer JWK each time so the hashname will change, long-lived identity established at app/higher layer (via OIDC)
* once a peer's joining JWE is validated and associated with a local transport path, E2E channels can be established









