---
layout: post
title: ubuntu 16.04 utorrent install
date:	2017-04-02 20:19:00 +0800
categories: document
tag:	Ubuntu
---


download the utorrent file for linux

>	wget http://download-new.utorrent.com/endpoint/utserver/os/linux-x64-ubuntu-13-04/track/beta/ -O utserver.tar.gz

tar the file in the folder of /opt/

>	sudo tar -zxvf utserver.tar.gz -C /opt/

change the permission on uTorrent-server folder

>	sudo chmod 777 /opt/utorrent-server-alpha-v3_3/

setup the utorrentStart.sh file for starting the utorrent

>	#!bin/bash
>	/opt/utorrent-server-alpha-v3_3/utserver -settingspath /opt/utorrent-server-alpha-v3_3/ &
>	echo "utorrent is running"

start the utorrent client

>	sh utorrentStart.sh


