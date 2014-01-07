---
title: Making Ghost And WordPress Run Together On The Same Server
author: bepitulaz
layout: post
permalink: /2013/12/making-ghost-and-wordpress-run-together-on-the-same-server/
categories:
  - Blog
  - Programming
tags:
  - Digital Ocean
  - Ghost
  - Wordpress
comments: true
---
My fiancee has an existing WordPress blog. But, she wants to try Ghost, a new blogging platform written in nodejs. She asked me to install Ghost on the same server that runs her WordPress blog. I never have experience with nodejs before it. After read Ghost&#8217;s documentation and googling, I discovered that Ghost has its own web server, it doesn&#8217;t need Apache. But, her existing WordPress blog runs on Apache web server. \*headache\*. More googling and I found some articles that mention how to run nodejs application on Apache.<!--more-->

Here are the steps:

**Install nodejs**

Update the package repository (her server is using Ubuntu 12.04).

<pre>apt-get update
apt-get upgrade
apt-get install build-essential zip</pre>

**Install nodejs from source code**

Go to <a title="Download nodejs" href="http://nodejs.org/download" target="_blank">nodejs.org</a> and download the latest version (currently the latest is v.0.10.22).

<pre>wget http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
tar -xvzf http://nodejs.org/dist/v0.10.22/node-v0.10.22.tar.gz
cd node-v0.10.22/
./configure
make
make install</pre>

**Download and install Ghost**

Her wordpress blog is using pre-config setting from Digital Ocean, therefore her wordpress is in /home/wordpress/public_html.

<pre>mkdir /home/wordpress/public_html/ghost
cd /home/wordpress/public_html/ghost
wget https://ghost.org/zip/ghost-0.3.3.zip
unzip ghost-0.3.3.zip
npm install --production</pre>

**Configure Ghost**

Create and edit config.js.

<pre>cp config.example.js config.js
nano config.js</pre>

In the &#8216;production&#8217; section change with the following

<pre>url: 'http://yourghostdomain.com',
...... // just leave this line until the server section
......
server: {
    // Host to be passed to node's `net.Server#listen()`
    host: '127.0.0.1',
    // Port to be passed to node's `net.Server#listen()`, for iisnode set this to `process.env.PORT`
    port: '8080'
}</pre>

That&#8217;s all for installing and configuring Ghost. Now the main trick to run Ghost on Apache.

**Make Apache as a proxy to nodejs webserver**

Installing mod_proxy

<pre>a2enmod proxy proxy_http
service apache2 restart</pre>

**Create virtual host for your Ghost&#8217;s domain**

<pre>cd /etc/apache2/sites-available
nano whateverthename.conf</pre>

fill that virtual host file with

<pre>&lt;VirtualHost *:80>
    ServerName yourghostdomain.com
    ProxyRequests off

    &lt;proxy *>
       order deny,allow
       allow from all
    &lt;/proxy>
    &lt;location />
       ProxyPass http://127.0.0.1:8080/
       ProxyPassReverse http://127.0.0.1:8080/
    &lt;/location>
&lt;/VirtualHost>
</pre>

enable that configuration using

<pre>a2ensite whateverthename.conf
</pre>

after that restart Apache.

<pre>service apache2 restart</pre>

**Finally, runs Ghost&#8217;s web server**

Install &#8216;forever&#8217; package, so you can run Ghost forever on your server.

<pre>cd /home/wordpress/public_html/ghost
npm install forever -g  
NODE_ENV=production forever start index.js</pre>

That&#8217;s all. Happily ever after, now Ghost can run on the same server with WordPress. You can access it from http://yourghostdomain.com. Happy blogging <img src="http://asep.co/wp-includes/images/smilies/icon_biggrin.gif" alt=":D" class="wp-smiley" />