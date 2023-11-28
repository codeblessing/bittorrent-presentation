---
favicon: resources/favicon.png
titleTemplate: BitTorrent - %s
author: Jakub Kwiatkowski, Maciej Grześ, Krzysztof Jajeśnica
fonts:
  sans: 'JetBrains Mono'
theme: default
title: Distributed file sharing protocol
background: resources/title-cover.jpg
layout: cover
hideInToc: true
---

# BitTorrent

## DISTRIBUTED FILE SHARING PROTOCOL

---
theme: seriph
title: Agenda
hideInToc: true
---

# Agenda

<Toc maxDepth="1"></Toc>

---
title: Overview
---

# Overview

BitTorrent is the protocol designed to load-balance download servers and make file-sharing more accessible to everyone.

It utilizes file chunking and P2P (peer-to-peer) connections to distribute load between multiple nodes.

<!-- TODO: Insert animation presenting file download with let's say 5 nodes - 1 master, 4 clients. -->

---
title: Tracker
---

# Tracker

Tracker is a server that holds information about torrent 'state'.

In order to work properly BitTorrent have to have information about parts and peers.
Holding these information is work of so called tracker.

Tracker is kind of control server for BitTorrent network.
At first it knows only about _seed_ (origin) server and have list of pieces with appropriate hashes.
When new peer wants to download file it sends `announce` message to tracker.
Tracker gather all `announce` messages and adds peers to it's database.

---
title: Peers
---

# Peers

Peer is any BitTorrent client that takes part in torrent network.

When client wants to download file it has to send `announce` message.
`announce` message contains following data:

```yaml
info_hash: <torrent_hash> 
peer_id: <client_hash>
ip: client's IP
port: port on which client is listening
uploaded: <number of parts uploaded>
downloaded: <number of parts downloaded>
left: <byte size of parts left to download>
event: one of started|completed|stopped|empty
```

Because BitTorrent uses HTTP this request is HTTP `GET` and data are passed as parameters.

```HTTP
GET /announce?info_hash=abcdef1234567890&peer_id=client_peer_id&ip=192.168.1.100&port=6881&uploaded=0&downloaded=0&left=remaining_bytes&event=started HTTP/1.1
Host: tracker.example.com
```

---

In response to `announce` tracker returns list of peers to connect with:
```bencode
d8:intervali3600e5:peersld2:ip13:192.168.1.1007:peer_id20:SirAlexanderHamilton4:porti6881eeee
```

Having this info client can connect directly to other peers to download files.

---
title: Seed
---

# Seed

Seed is origin server which have whole file that is shared.



---
theme: seriph
layout: cover
class: text-center
title: .torrent
---

# .torrent

---
theme: seriph
class: text-center
highlighter: shiki
lineNumbers: false
transition: slide-left
title: .torrent
mdc: true
---

`.torrent` file defines metadata of shared file. It contains information about tracker and file itself.

```yaml
announce: "https://example.com/torrent/announce",
info: 
  name: "srds-exam-answers.pdf",
  piece length: 262144,
  pieces: èvöz*ˆ†èókg&Ã¢—-n"uæ vfVsnÿµR­,
  length: 26214400
```

