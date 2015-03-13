**Contents**


# Introduction #

[Configuring](Configuring.md)


# Deploy Web Server #
## Installing Apache Webserver ##

  1. Download & Install Apache Webserver (also called Apache http server
    * http://httpd.apache.org/download.cgi
    * [How to Install Apache Web Server on Windows](http://www.sitepoint.com/blogs/2009/04/07/how-to-install-apache-on-windows/)
  1. Install PHP and make sure it is applied to the Webserver
    * http://www.php.net/downloads.php
    * Note: If using Windows, be sure to download the VC6 thread safe version. [How to Install PHP 5.3 on Windows](http://www.sitepoint.com/blogs/2009/07/07/install-php53-windows/#)

## Installing PHP ##

Required modules:
  * OpenSSL
  * PDO
  * SQLite 3

**Note: This no longer seems to be the case.** For windows PHP installer, SQLite 2 is the default. To install SQLite 3, You may need to do following:
  1. download php\_pdo\_sqlite.dll to PHP/ext/
  1. edit php.ini. change "extension=php\_sqlite.dll" to "extension=php\_pdo\_sqlite.dll"
  1. restart Apache

20100120--When installing on a Windows 7 machine Apache complained about not finding ntwdblib.dll which is needed by MySQL so I uncommented MySQL and MSQL from the PHP extensions set up.

## Configure Apache for Perl ##
**This is only needed if you want to deploy Festival TTS server on Linux**

Install following modules:
  * libapache2-mod-perl2
  * libapache2-mod-musicindex

Add following code to apache.conf
```
Alias /perl/ /path/to/perl_scripts/
PerlModule ModPerl::Registry

<Location "/perl/">
SetHandler perl-script
PerlHandler ModPerl::Registry
Options +ExecCGI
PerlOptions +ParseHeaders
</Location> 
```

# Deploy Festival TTS Server (optional) #
**It is not a must step** to setup WebAnywhere because we could use following TTS server
http://webinsight.cs.washington.edu/cgi-bin/getsound.pl?text=hello
And this is only applicable for **Linux** now.

## Install Festival ##
Use following command if you are using Debian based Linux:
```
sudo apt-get install festival
```

## Launch Festival Server ##
```
festival --server
```

You can also setup festival to run in the background with a command like this:
```
nohup festival --server > /dev/null 2> /dev/null < /dev/null &
```

## Install Perl module Speech::Festival ##
```
cpan -i Speech::Festival
```
The module is install in xxx/Speech/Speech/Festival.pm. It's a bug. We need to move folders tree as xxx/Speech/Festival.pm. Also, we need to comment out one print statement.

```
sub DESTROY
{
    my ($self) = @_;

#    print "disconnect\n";
    disconnect $self;
}
```

## Configure getsound.pl ##
The original version of getsound.pl locates at tts/festival/. We should copy it to the right path, /var/www/perl/ for example.

getsound.pl need lame and sox application. Make a symbol link to them under the path of getsound.pl.

Change following line to add our own TTS server:
```
my @servers = ('emil.cs.washington.edu', 'brangane.cs.washington.edu');
```

For example, if you have festival running on the same machine as the web server, then the following would be correct:

```
my @servers = ('localhost');
```

The festival server will by default accept connections to localhost.  If you want to add an additional TTS server, you'll need to edit the festival.scm file to allow connections from other computers in your domain.  For instance, at the University of Washington, I have the following, which enables connections from either localhost or computers whose hostname ends in cs.washington.edu:

```
(defvar server_access_list '(localhost "[^\\.\\\\]+\\.cs\\.washington\\.edu")
```

## Configure config.php ##
We may need to change following lines. Please make sure web server has writing permission on the directory of webanywhere-accesses.sdb.
```
$sound_url_base = 'http://localhost/perl/getsound.pl?text=$text$&cache=1&mtts=1';
...
$sql_lite_filename = "C:/webanywhere-accesses.sdb";
```

# Deploy eSpeak TTS server (optional) #
eSpeak can speak many languages. And it's easy to deploy.

## Install eSpeak ##
Please refer to http://espeak.sf.net
We don't need to launch a eSpeak server like Festival. We just need to make sure command `espeak` can be run.

## Configure getsound.pl ##
We need to copy tts/espeak/getsound.pl to some path. This path is hard code in the script. So we need to change following line:
```
my $base_dir = '/home/hgneng/www-perl';
```

In getsound.pl of espeak, we add a "lang" variable to specify voice in espeak.

## Configure config.php ##
Like Festival, we need to set following line: (espeak.pl is a copy of getsound.pl)
```

$sound_url_base = 'http://localhost/perl/espeak.pl?text=$text$&lang=cantonese';

```

# Deploy Ekho TTS server (optional) #
Ekho is a Chinese(Cantonese/Mandarin) TTS engine. It can also speak English through Festival.

## Install Ekho ##
Please refer to http://www.eguidedog.net/ekho.php
The TTS server function works since verion 2.1 for Linux.

## Launch Ekho TTS server ##
```
ekho --server -v Mandarin
```

To test whether Ekho server works:
```
ekho --request "123" -o output.mp3
```

A file output.mp3 should be generated.

## Configure ekho\_agent.pl ##
We need to copy tts/ekho/ekho\_agent.pl to some path. This path is hard code in the script. So we need to change following line:
```
my $base_dir = '/var/www/cache/';
```

## Configure config.php ##
Like Festival, we need to set following line:
```
$sound_url_base = 'http://localhost/perl/ekho_agent.pl?text=$text$&cache=1&mtts=1';
```

# Deploy Webanywhere #

## Downloading the Code ##
  1. Install svn on the computer and [checkout the code from this site](http://code.google.com/p/webanywhere/source/checkout).
    * [TortoiseSVN](http://tortoisesvn.net/downloads) is recommended for windows machines

## Setting Up Webanywhere in Apache ##

  1. Move the downloaded Webanywhere code into the htdocs folder within Apache.
  1. Change the name of the Webanywhere folder from "webanywhere" to "wa"
  1. Adjust the config.php file with the following adjustments: ...
    * Change path of accesses database file: $sql\_lite\_filename = "C:/webanywhere-accesses.sdb";
  1. Directing your internet browser to http://localhost/wa/ should open the Webanywhere hosted on your computer.

# Deploy Guide for Ubuntu Linux #

This guide is based on Ubuntu 12.04 (64bit) and latest code in SVN (2012-09-20).

## 1. Install Web server ##
```
$ sudo apt-get install apache2
$ sudo apt-get install php5
$ sudo apt-get install php5-sqlite
```

## 2. Copy WebAnywhere code to web server ##

We can get latest code with following command:
```
svn checkout http://webanywhere.googlecode.com/svn/trunk/ webanywhere-read-only
```

Or download from a stable release.

In my case, I extract webanywhere in /var/www/wa

I also add full write permission in my dev machine in case of any permission issue. (don't do it on production)
```
$ sudo chmod -R a+w ~/webanywhere
```

## 3. Config WebAnywhere ##
```
$ vi config.php
comment the config for windows path and uncomment the linux path.
$ sudo mkdir /wa
$ sudo touch /wa/webanywhere-accesses.sdb
$ sudo chmod -R a+w /wa
```

## 4. Install Flash plugin to Firefox ##

## 5. Open http://localhost/wa/ ##
(change the URL on your own case)
You should see home page your WebAnywhere server and hear sound.

## 6. Prepare to debug (for developer) ##
Install Firebug.
Install PHP Xdebug extension
```
$ sudo apt-get install php5-xdebug
```
Install easy xdebug extension for Firefox
Install Netbeans add some config to PHP to enable xdebug with Netbeans.(optional, but I use xdebug with Netbeans)

# Deploy Guide for Windows XP #
## 1. Install Web Server ##
Although MySQL is not needed for WebAnywhere, we just install the whole WAMP for simplicity:
http://www.wampserver.com/en/

## 2. Copy WebAnywhere code to web server ##
Download latest code (http://webanywhere.googlecode.com/files/wa-20120920.tar.bz2)

Put it under /wa of web server root (c:/wamp/www)

## 3. Config WebAnywhere ##
edit config.php

Uncomment following line (and comment the next line for Linux path)
```
$sqlite_filename = "C:/webanywhere-accesses.sdb";
```

Replace the TTS server to another server in China (I don't know why the default TTS server doesn't work. I remember it worked when I wrote the deploy guide for Ubuntu last month)
```
  $voices["en"] = 'http://wa.eguidedog.net/cgi-bin/ekho.pl?text=$text$'; // English
```

## 4. Open http://locahost/wa/ in Chrome Browser ##
You should see home page your WebAnywhere? server and hear sound.

If you use other web browser, please make sure Flash is installed.

# Another Guide #
This is another home of WebAnywhere. It's worth taking a look at it:
http://hci.cs.rochester.edu/wa/getting_started.html