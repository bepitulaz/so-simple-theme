---
title: How To Install Ruby 2.1 Using Macport and Rbenv
author: bepitulaz
layout: post
permalink: /2014/04/how-to-install-ruby-2.1-using-macport-and-rbenv/
categories:
  - Blog
  - Programming
tags:
  - programming
  - ruby
comments: true
---
Today is Saturday night, and I decide to replace my Ruby environment from rvm to rbenv on my OS X 10.9. Many tutorials out there are using Brew, but I'm a Macport guy. Here are my steps.

###0. Removing RVM
Remove the RVM using this command.
<pre>
rvm implode
</pre>
Don't forget to remove the path from your ~/.bashrc or ~/.bash_profile after remove RVM successfully.

###1. Installing or Upgrading OpenSSL
If you have not install it yet, you can do with this command below. It will give you the latest version of OpenSSL. 
<pre>
port self-update
port install openssl
</pre>
Or if you have installed it, but not sure you are using the latest version, upgrade it.
<pre>
port upgrade openssl
</pre>

###2. Installing Rbenv and ruby-build
Now installing the Rbenv using rbenv-installer from the terminal.
<pre>
curl https://raw.github.com/fesplugas/rbenv-installer/master/bin/rbenv-installer | bash
</pre>
After that, add these lines into your ~/.bash-profile to complete the installation.
<pre>
export RBENV_ROOT="$HOME/.rbenv"

if [ -d $RBENV_ROOT ]; then
  export PATH="$RBENV_ROOT/bin:$PATH"
  eval "$(rbenv init -)"
fi
</pre>
Next, use these commands from the terminal to setup bundler and rubygems-bundler to manage gem dependencies for Rails application.
<pre>
echo bundler >> ~/.rbenv/default-gems
echo rubygems-bundler >> ~/.rbenv/default-gems
</pre>
Finally, restart your terminal by opening new tab or quit and re-open your terminal. This will ensure that rbenv has loaded.

###3. Compile Ruby 2.1
Install it using this command below.
<pre>
rbenv install 2.1.0
</pre>
progress...progress...and ERROR! Yes, I found an error like this.
<pre>
Downloading ruby-2.1.0.tar.gz...
-> http://dqw8nmjcqpjn7.cloudfront.net/9e6386d53f5200a3e7069107405b93f7
Installing ruby-2.1.0...

BUILD FAILED

Inspect or clean up the working tree at /tmp/ruby-build.20140104201328.63349
Results logged to /tmp/ruby-build.20140104201328.63349.log

Last 10 log lines:
                              io-console 0.4.2
                              json 1.8.1
                              minitest 4.7.5
                              psych 2.0.2
                              rake 10.1.0
                              rdoc 4.1.0
                              test-unit 2.1.0.0
installing rdoc:              /Users/bepitulaz/.rbenv/versions/2.1.0/share/ri/2.1.0/system
installing capi-docs:         /Users/bepitulaz/.rbenv/versions/2.1.0/share/doc/ruby
The Ruby openssl extension was not compiled. Missing the OpenSSL lib?
</pre>
This is how to solve this problem. This command just for Macport users.
<pre>
CONFIGURE_OPTS="--with-openssl-dir=/opt/local --with-readline-dir=/opt/local" rbenv install 2.1.0
</pre>
That prefix tell rbenv where the openssl directory installed by Macport is.

After Ruby is installed successfully, use these commands below.
<pre>
rbenv global 2.1.0
rbenv rehash
</pre>
Now, check the ruby version is correct.
<pre>
ruby -v
</pre>
If you see something like this
<pre>
ruby 2.1.0p0 (2013-12-25 revision 44422) [x86_64-darwin13.0]
</pre>
that means you have finished successfully. Congratulation.