# aMule - broadband branch
The initial purpose of this project was to provide an aMule repository (including dependencies) that is ready to build and update the dependent libraries when possible. This branch specifically focuses on providing a build that is better suited to address nowadays file sizes and broadband availability. Default hard-coded parameters of aMule were better suited for small-files/slow-connections, leading to very low per-client transfer rates by nowadays standards.  
The focus here is to maximise throughput for broadband users, to optimize seeding.  
The focus is as well to introduce the least amount of changes to preserve the original quality and stability of the client.  

## Dockerhub
### mercu/builder-amule:arm64

tags: 
- arm64
- amd64

https://hub.docker.com/r/mercu/builder-amule

## Building
Docker for build:
	https://github.com/mercu01/docker-amule-broadband
	
## Issue:
The upload slot algorithm is broken. Amule generates infinite upload slots, each slot of a few kb/s.

Amule does not get credits, the download is penalized.

## Summary of changes
### 2.3.3_broadband_RC4
 - Feature: Alternative Rate Limits, [more info](#feature-alternative-rate-limits)
### 2.3.3_broadband_RC3
 - Feature: Advanced Upload Manager, [more info](#feature-advanced-upload-manager)
### 2.3.3_broadband_RC2
 - Fix Default upload rate = 0 for broadband branch, 1 slot = 1 mb/s upload rate
### 2.3.3_broadband_RC1
 - Fix Slot Allocation = upload slots (min 5 slots, recomended: 10/20)
 - The client upload has 200 shipments with warning (< 50kb/s), before removing it from the queue, 200 shipments = 1 minute
 - Infinite POWERSHARE Clients
 - Zip compression disabled (Reduce CPU usage)
 - Change size upload package: from 10 kilobytes to 100 kilobytes (Reduce number loops to upload)



## Example
### I have 15mb/s upload connection:
#### Option A:
 - Configure 20 slots allocation, (1 slot = 500 Kb's, 20 slots = 10 Mb's approximate)
 - Max upload 10000
 - Max download 0
#### Option B:
 - Configure 20 slots allocation, (1 slot = 500 Kb's, 20 slots = 10 Mb's approximate)
 - Max upload 0
 - Max download 0
 
![config](https://user-images.githubusercontent.com/9451876/187890550-f733421e-495b-458f-8221-f54bd9cb6167.jpg)

### Results, only uploading:

![uploading only](https://user-images.githubusercontent.com/9451876/187890695-4c40d231-ea29-4d19-bea5-0aeacd60b44d.jpg)

### Statistics, only uploading:

![image](https://user-images.githubusercontent.com/9451876/187071859-3afc0544-f550-4fd5-9de2-b1b88fccc1be.png)

## Feature: Advanced Upload Manager
![image](https://user-images.githubusercontent.com/9451876/195408957-a91841a9-986c-4e8d-abe4-3145d524564d.png)

The system will handle the slots, you'll get a steady load. If your load queue is big, the system will kick slow client. When everyone is fast, it will be deactivated
### More technical information:
Basically, we will try to find the maximum upload depending on the amount of clients that are waiting.
#### ENABLED when:  
- SOFT KICK 
Example: 10 upload slots and >10 waiting in queue: kick slow clients, but slowly. (1 kick every 1 min)
*(I don't have many customers waiting, I can run out of customers in the queue)

![image](https://user-images.githubusercontent.com/9451876/195412442-0b54d30e-c877-4421-b983-8d14eecb70c0.png)

- HARD KICK
Example: 10 upload slots and >30 waiting in queue: kick slow clients, but more aggressive. (1 kick every 30s)
*(I can afford to look for quality clients)

![image](https://user-images.githubusercontent.com/9451876/195413533-d2b9fecc-a637-4f8b-ac52-696109d09613.png)

#### DISABLED when: 
- Very few customers in the queue, Example: 10 upload slots and <10 waiting in queue: none, no kick clients *(this state occurs at night)
- The total speed is over than 85%.
- The slowest client is not very slow, at least 75% compared to the fastest client.
- New clients, they can't be kicked. (It is considered a new client, when it takes less than 1 minute).
- New clients is over 25% of the slots 

## Feature: Alternative Rate Limits
Schedule the use of alternative rate limits

![image](https://user-images.githubusercontent.com/9451876/233133631-6d51708d-8000-4f5c-90c0-d35937fce93d.png)

## Inspired by
- [itlezy/eMule](https://github.com/itlezy/eMule )

---

# aMule

![aMule](https://raw.githubusercontent.com/amule-org/amule/master/org.amule.aMule.png)

aMule is an eMule-like client for the eDonkey and Kademlia networks.

[Forum] | [Wiki] | [FAQ]

[Forum]: https://github.com/amule-org/amule/discussions "aMule Forum"
[Wiki]:  https://github.com/amule-org/amule/wiki "aMule Wiki"
[FAQ]:   https://github.com/amule-org/amule/wiki/FAQ-aMule "FAQ on aMule"

## Overview

aMule is a multi-platform client for the eD2k / Kad file-sharing network,
originally a fork of the Windows client eMule (via xMule and lMule).
aMule started in August 2003.

Supported platforms today: Linux, FreeBSD, OpenBSD, macOS, and Windows
(MSYS2 / mingw-w64), on both x86_64 and ARM64.

aMule aims to stay close to eMule in look-and-feel so users moving between
the two have minimal friction. New eMule protocol-level features are
generally adopted into aMule shortly after.

---

| Distributions |
| --- |
| [![Arch Linux](https://repology.org/badge/version-for-repo/arch/amule.svg)](https://archlinux.org/packages/extra/x86_64/amule/) |
| [![AUR](https://repology.org/badge/version-for-repo/aur/amule.svg)](https://aur.archlinux.org/packages/amule) |
| [![Debian stable](https://badges.debian.net/badges/debian/stable/amule/version.svg)](https://buildd.debian.org/amule) |
| [![Debian testing](https://badges.debian.net/badges/debian/testing/amule/version.svg)](https://buildd.debian.org/amule) |
| [![FreeBSD](https://repology.org/badge/version-for-repo/freebsd/amule.svg)](https://www.freshports.org/net-p2p/amule/) |
| [![Gentoo](https://repology.org/badge/version-for-repo/gentoo/amule.svg)](https://packages.gentoo.org/packages/net-p2p/amule) |
| [![Kali Linux](https://repology.org/badge/version-for-repo/kali_rolling/amule.svg)](https://pkg.kali.org/pkg/amule) |
| [![Manjaro](https://repology.org/badge/version-for-repo/manjaro_stable/amule.svg)](https://repology.org/project/amule/versions) |
| [![NixOS 25.05](https://repology.org/badge/version-for-repo/nix_stable_25_05/amule.svg)](https://search.nixos.org/packages?channel=25.05&query=amule) |
| [![OpenBSD](https://repology.org/badge/version-for-repo/openbsd/amule.svg)](https://openports.pl/path/net/amule) |
| [![openSUSE Tumbleweed (Packman)](https://repology.org/badge/version-for-repo/packman_opensuse_tumbleweed/amule.svg)](http://packman.links2linux.org/package/aMule) |
| [![RPMFusion Fedora 42](https://repology.org/badge/version-for-repo/rpmfusion_fedora_42/amule.svg)](https://repology.org/project/amule/versions) |
| [![Slackware](https://repology.org/badge/version-for-repo/slackbuilds/amule.svg)](https://slackbuilds.org/result/?search=amule) |
| [![Solus](https://repology.org/badge/version-for-repo/solus/amule.svg)](https://repology.org/project/amule/versions) |
| [![Ubuntu 24.04 LTS](https://repology.org/badge/version-for-repo/ubuntu_24_04/amule.svg)](https://packages.ubuntu.com/noble/amule) |
| [![Ubuntu 25.04](https://repology.org/badge/version-for-repo/ubuntu_25_04/amule.svg)](https://packages.ubuntu.com/plucky/amule) |

---

Development Statistics:

| [![Open Issues](https://img.shields.io/github/issues/amule-project/amule)](https://github.com/amule-project/amule/issues) | [![Open Pull Requests](https://img.shields.io/github/issues-pr/amule-project/amule)](https://github.com/amule-project/amule/pulls) |
| --- | --- |
| [![Bug](https://img.shields.io/github/issues/amule-project/amule/bug)](https://github.com/amule-project/amule/issues?q=is%3Aopen+is%3Aissue+label%3Abug) | |
| [![Bug - Delayed Fix](https://img.shields.io/github/issues/amule-project/amule/bug%20-%20delayed%20fix)](https://github.com/amule-project/amule/issues?labels=bug%20-%20delayed+fix) | |
| [![Feature Request](https://img.shields.io/github/issues/amule-project/amule/feature%20request)](https://github.com/amule-project/amule/issues?labels=feature+request) | |
| [![Enhancement](https://img.shields.io/github/issues/amule-project/amule/enhancement)](https://github.com/amule-project/amule/issues?labels=enhancement) | |

## Features

* `amule` — all-in-one GUI client.
* `amuled` — headless daemon, no GUI.
* `amulegui` — remote GUI; connects to a local or remote `amuled` over the
  EC (External Connection) protocol.
* `amuleweb` — HTTP interface to a running `amuled`.
* `amulecmd` — interactive CLI for a running `amuled`.

## Compiling

aMule uses CMake. Quick start:

```sh
cmake -B build -DBUILD_MONOLITHIC=YES -DBUILD_REMOTEGUI=YES
cmake --build build -j"$(nproc)"
sudo cmake --install build
```

See [docs/INSTALL.md](docs/INSTALL.md) for the full list of dependencies,
build options (`BUILD_DAEMON`, `BUILD_AMULECMD`, `ENABLE_NLS`, `ENABLE_UPNP`,
`ENABLE_IP2COUNTRY`, etc.), and platform-specific notes. The CI workflow
[`.github/workflows/ccpp.yml`](.github/workflows/ccpp.yml) is the
authoritative reference for the exact deps and flags used to build aMule
on Linux, macOS, and Windows.

## Setting Up

aMule comes with reasonable default settings and should be usable as-is.
However, to receive a [HighID] you need to open aMule's ports on your
firewall and/or forward them on your router. The wiki has articles on
[getting a HighID][2] and [setting up firewall rules][3].

[HighID]: https://github.com/amule-org/amule/wiki/FAQ_eD2k‐Kademlia#what-is-lowid-and-highid "What is LowID and HighID?"
[2]: https://github.com/amule-org/amule/wiki/Get-HighID "How to get HighID"
[3]: https://github.com/amule-org/amule/wiki/Firewall "How to set up firewall rules for aMule"

## Reporting Bugs

If you find a bug or miss a feature, please open an issue on
[GitHub][5] (preferred) or report it on the [forum]. A good bug report
includes the exact aMule version (`amuled --version`), the platform you're
on, and steps to reproduce.

[5]: https://github.com/amule-org/amule/issues "aMule Issues"

## Contributing

*Contributions are always welcome!*

You can contribute to aMule in several ways:

* **Code** — fix a bug, implement a feature, improve performance. The preferred
  path is a [pull request][6] on GitHub; patches on the [forum] also work.
* **Translation** — [translate aMule][7], [translate the wiki][8], or
  [translate aMule's documentation][9] into your language.
* **Wiki** — aMule's wiki contains historical content that no longer matches
  current behavior. Updating outdated pages is genuinely helpful.

[6]: https://github.com/amule-org/amule/pulls "aMule Pull Requests"
[7]: https://github.com/amule-org/amule/wiki/Translations "Translating aMule"
[8]: https://github.com/amule-org/amule/wiki/Translating-Wiki "Translating the wiki"
[9]: https://github.com/amule-org/amule/wiki/Translating-Docs "Translating the documentation"
