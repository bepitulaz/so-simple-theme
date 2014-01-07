---
title: Deploying Riak CS Cluster Using Docker On Ubuntu
author: bepitulaz
layout: post
permalink: /2014/07/deploying-riak-cs-cluster-using-docker-on-ubuntu/
categories:
  - Blog
  - Programming
tags:
  - programming
  - nosql
  - riak
comments: true
---
![image](https://www.docker.io/static/img/homepage-docker-logo.png)

I said in [this post](http://asep.co/2014/01/what-tech-related-that-i-want-to-do-in-2014/) that I will learn Riak CS this year. 2 days ago I started to read [A Little Riak Book](http://littleriakbook.com) as my reference book.

> Riak is an open-source, distributed key/value database for high availability, fault-tolerance, and near-linear scalability. In short, Riak has remarkably high uptime and grows with you.

The explanation above is quoted from that book. It means Riak need to be operated in a cluster to do data replication and partition, and the best minimal node to be set up in one ring (ring is a term for cluster in Riak world) are 5 nodes. 3 nodes for partition and 2 nodes for replication.

It's too expensive if I set up 1 node for 1 server just for learning purpose. Therefore I find a way how to create cluster with 5 nodes only for 1 server (local cluster). Luckily I found [Docker](http://docker.io) a Linux container (Thanks [@Gozali](http://twitter.com/gozali)).

I'm using Ubuntu 12.04 LTS on my vps. Here are the steps.
###1. Install Linux 3.8 kernel
Docker works best on the 3.8 kernel. Ubuntu 12.04 comes with a 3.2 kernel. So, we need to upgrade it.
<pre>
install the backported kernel
$ sudo apt-get update
$ sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring

reboot
$ sudo reboot
</pre>

###2. Install Docker
<pre>
$ sudo sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"

$ sudo sh -c "echo deb http://get.docker.io/ubuntu docker main /etc/apt/sources.list.d/docker.list"
$ sudo apt-get update
$ sudo apt-get install lxc-docker
</pre>

###3. Setup local Riak cluster
Thank God, someone had created a ready-to-use Docker container for creating local Riak cluster. That is [docker-riak](https://github.com/hectcastro/docker-riak) project. Here are the steps, taken from its Github page.

Install dependencies
<pre>
$ sudo apt-get install -y git curl make sshpass
</pre>

Clone repository
<pre>
$ git clone https://github.com/hectcastro/docker-riak.git
$ cd docker-riak
$ make
$ make riak-container
</pre>

Launch cluster
<pre>
$ make start-cluster
</pre>

It will takes a few times for waiting the process. Here is the response in my terminal when it has finished the installation.
<pre>
Started [riak1] and assigned it the IP [33.33.33.10]
Started [riak2] and assigned it the IP [33.33.33.20]
Requesting that [riak2] join the cluster..
Success: staged join request for 'riak@33.33.33.20' to 'riak@33.33.33.10'
Started [riak3] and assigned it the IP [33.33.33.30]
Requesting that [riak3] join the cluster..
Success: staged join request for 'riak@33.33.33.30' to 'riak@33.33.33.10'
Started [riak4] and assigned it the IP [33.33.33.40]
Requesting that [riak4] join the cluster..
Success: staged join request for 'riak@33.33.33.40' to 'riak@33.33.33.10'
Started [riak5] and assigned it the IP [33.33.33.50]
Requesting that [riak5] join the cluster..
Success: staged join request for 'riak@33.33.33.50' to 'riak@33.33.33.10'
=============================== Staged Changes ================================
Action         Details(s)
-------------------------------------------------------------------------------
join           'riak@33.33.33.20'
join           'riak@33.33.33.30'
join           'riak@33.33.33.40'
join           'riak@33.33.33.50'
-------------------------------------------------------------------------------


NOTE: Applying these changes will result in 1 cluster transition

###############################################################################
                         After cluster transition 1/1
###############################################################################

================================= Membership ==================================
Status     Ring    Pending    Node
-------------------------------------------------------------------------------
valid     100.0%     20.3%    'riak@33.33.33.10'
valid       0.0%     20.3%    'riak@33.33.33.20'
valid       0.0%     20.3%    'riak@33.33.33.30'
valid       0.0%     20.3%    'riak@33.33.33.40'
valid       0.0%     18.8%    'riak@33.33.33.50'
-------------------------------------------------------------------------------
Valid:5 / Leaving:0 / Exiting:0 / Joining:0 / Down:0

Transfers resulting from cluster changes: 51
  12 transfers from 'riak@33.33.33.10' to 'riak@33.33.33.50'
  13 transfers from 'riak@33.33.33.10' to 'riak@33.33.33.40'
  13 transfers from 'riak@33.33.33.10' to 'riak@33.33.33.30'
  13 transfers from 'riak@33.33.33.10' to 'riak@33.33.33.20'

Commit these cluster changes? (y/n): y
Cluster changes committed
</pre>

###4. Check the cluster
Then I check my local Riak cluster whether it runs or not using.
<pre>
$ sudo docker ps
</pre>
My terminal shows that 5 Riak nodes is running.
<pre>
CONTAINER ID        IMAGE                    COMMAND                CREATED             STATUS              PORTS                        NAMES
4a3988be78b6        hectcastro/riak:latest   /usr/bin/supervisord   2 minutes ago       Up 2 minutes        22/tcp, 8087/tcp, 8098/tcp   pensive_darwin       
0c5b9c792a02        hectcastro/riak:latest   /usr/bin/supervisord   3 minutes ago       Up 3 minutes        22/tcp, 8087/tcp, 8098/tcp   agitated_mccarthy    
bf777b04b633        hectcastro/riak:latest   /usr/bin/supervisord   3 minutes ago       Up 3 minutes        22/tcp, 8087/tcp, 8098/tcp   stupefied_torvalds   
f3bed4b633de        hectcastro/riak:latest   /usr/bin/supervisord   4 minutes ago       Up 4 minutes        22/tcp, 8087/tcp, 8098/tcp   sleepy_turing        
381c90578f14        hectcastro/riak:latest   /usr/bin/supervisord   4 minutes ago       Up 4 minutes        22/tcp, 8087/tcp, 8098/tcp   ecstatic_mclean  
</pre>

Now I can start learn to develop something with this Riak cluster.  