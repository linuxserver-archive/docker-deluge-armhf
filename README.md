[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: http://deluge-torrent.org/
[hub]: https://hub.docker.com/r/lsioarmhf/deluge/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/deluge
[![](https://images.microbadger.com/badges/version/lsioarmhf/deluge.svg)](https://microbadger.com/images/lsioarmhf/deluge "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/deluge.svg)](https://microbadger.com/images/lsioarmhf/deluge "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/deluge.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/deluge.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/armhf/armhf-deluge)](https://ci.linuxserver.io/job/Docker-Builders/job/armhf/job/armhf-deluge/)

[deluge](http://deluge-torrent.org/) Deluge is a lightweight, Free Software, cross-platform BitTorrent client.

* Full Encryption
* WebUI
* Plugin System
* Much more...

[![deluge](https://avatars2.githubusercontent.com/u/6733935?v=3&s=200)][appurl]

## Usage

```
docker create \
  --name deluge \
  --net=host \
  -e PUID=<UID> -e PGID=<GID> \
  -e TZ=<timezone> \
  -e UMASK_SET=<022> \
  -v </path/to/your/downloads>:/downloads \
  -v </path/to/deluge/config>:/config \
  lsioarmhf/deluge
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `--net=host` - Shares host networking with container, **required**.
* `-v /config` - deluge configs
* `-v /downloads` - torrent download directory
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e UMASK_SET` for umask setting of deluge, *optional* , default if left unset is 022.
* `-e TZ` for timezone information, eg Europe/London

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it deluge /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 
`IMPORTANT... THIS IS THE ARMHF VERSION`

The admin interface is available at http://<ip>:8112 with a default user/password of admin/deluge.
To change the password (recommended) log in to the web interface and go to Preferences->Interface->Password.

## Info

* Monitor the logs of the container in realtime `docker logs -f deluge`.

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' deluge`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/deluge`

## Versions

+ **11.10.18:** Rebase to ubuntu bionic.
+ **09.04.18:** update to libressl2.7-libssl.
+ **31.03.18:** Rebase to alpine edge.
+ **19.12.17:** Rebase to alpine 3.7.
+ **20.11.17:** Change libressl2.6-libssl repo.
+ **01.07.17:** Add curl package.
+ **27.05.17:** Rebase to alpine 3.6.
+ **29.04.17:** Add variable for user defined umask.
+ **01.02.17:** Rebase to alpine 3.5.
+ **17.11.16:** Rebase to edge baseimage.
+ **13.10.16:** Switch to libressl as openssl deprecated from alpine linux and deluge dependency
no longer installs.
+ **30.09.16:** Fix umask.
+ **11.09.16:** Add layer badges to README.
+ **06.09.16:** Add badges to README.
+ **15.08.16:** Initial Release.
