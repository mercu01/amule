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

aMule
=====

![aMule](https://raw.githubusercontent.com/amule-project/amule/master/amule.png)

aMule is an eMule-like client for the eDonkey and Kademlia networks.

[Forum] | [Wiki] | [FAQ]

[![Debian CI](https://badges.debian.net/badges/debian/stable/amule/version.svg)](https://buildd.debian.org/amule)
[![Debian CI](https://badges.debian.net/badges/debian/testing/amule/version.svg)](https://buildd.debian.org/amule)

[Forum]: http://forum.amule.org/		"aMule Forum"
[Wiki]:  http://wiki.amule.org/			"aMule Wiki"
[FAQ]:   http://wiki.amule.org/wiki/FAQ_aMule	"FAQ on aMule"


Overview
--------

aMule is a multi-platform client for the ED2K file sharing network and based on
the windows client eMule. aMule started in August 2003, as a fork of xMule,
which is a fork of lMule.

aMule currently supports Linux, FreeBSD, OpenBSD, Windows, MacOS X and X-Box on
both 32 and 64 bit computers.

aMule is intended to be as user friendly and feature rich as eMule and to
remain faithful to the look and feel of eMule so users familiar with either
aMule or eMule will be able switch between the two easily.

Since aMule is based upon the eMule codebase, new features in eMule tend to
find their way into aMule soon after their inclusion into eMule so users of
aMule can expect to ride the cutting-edge of ED2k clients.


Features
--------

* an all-in-one app called `amule`.
* a daemon app called `amuled`. It's amule but with no interface.
* a client for the server called `amulegui` to connect to a local or distant
  amuled.
* `amuleweb` to access amule from a web browser.
* `amulecmd` to access amule from the command line.


Compiling
---------

In general, compiling aMule should be as easy as running `configure` and `make`.
There are [detailed instructions][1] on the wiki for compiling on a number of
different platforms, though they may be outdated a bit... (updates are welcome).

[1]: http://wiki.amule.org/wiki/Compile		"How to compile and install aMule"


Setting Up
----------

aMule comes with reasonable default settings and should be usable as-is.
However, to receive a [HighID] you need to open aMule's ports on your
firewall and/or forward them on your router. Again, you'll find detailed
articles on the wiki helping you [get HighID][2] and [setting up firewall
rules][3] for aMule.

[HighID]: http://wiki.amule.org/wiki/FAQ_eD2k-Kademlia#What_is_LowID_and_HighID.3F
	  "What is LowID and HighID?"

[2]: http://wiki.amule.org/wiki/Get_HighID	"How to get HighID"
[3]: http://wiki.amule.org/wiki/Firewall	"How to set up firewall rules for aMule"


Reporting Bugs
--------------

We aren't perfect and so aMule isn't perfect, too. We know that. If you find a
bug or miss a feature you can report/request it either on the [forum], the
[bug tracker][4] or on [GitHub][5]. 

[4]: http://bugs.amule.org/				"aMule Bug Tracker"
[5]: https://github.com/amule-project/amule/issues	"aMule Issues"


Contributing
------------

*Contributions are always welcome!*

You can contribute to aMule several ways:

* Contributing code. You can fix some bugs, implement new features, or
  whatever you want. The preferred way is opening a [pull request][6] on
  GitHub, but you can also post your patch on the [forum].
* Translating. You can [translate aMule][7], [translate the wiki][8] or
  [translate aMule's documentation][9] to your language.
* Fixing the wiki. aMule's wiki contains a lot of old, outdated information,
  that is simply not true anymore. One should read through the pages, update
  manuals and references and remove obsolate information.

[6]: https://github.com/amule-project/amule/pulls  "aMule Pull Requests"
[7]: http://wiki.amule.org/wiki/Translations	   "Translating aMule"
[8]: http://wiki.amule.org/wiki/Translating_Wiki   "Translating the wiki"
[9]: http://wiki.amule.org/wiki/Translating_Docs   "Translating the documentation"
