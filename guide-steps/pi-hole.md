# Title #
Setup and run Pi-Hole on a Raspberry Pi

# Summary #
Blocking ads and trackers on your devices typically requires manual labour on
each individual device (e.g. installing an ad-blocker on your browser, another
in your phone, and another in your tablet). In some cases, for instance smart
TVs, it's not even possible to install an ad-blocker. Pi-Hole is a general
purpose network-wide ad-blocker, that when installed will block ads on **any**
device connected to your home network, which means you manage your ad-blocking
on one place and have these changes automatically pushed to your connected
devices.

In this guide you'll learn how to install and setup Pi-Hole on a Raspberry Pi.

# Body #

## Overview ##

Pi-hole is a DNS ad-blocker that protects your devices from ads and trackers
without requiring any setup on said devices. As such, it is able to block ads on
any network device (e.g. smart appliances), and, unlike browser add-ons, Pi-hole
will prevent ads from being displayed on any type of software, not only web
browsers.

The general setup works as follows (Fig. 1). You install Pi-hole on your server
(in this case, we're using a Raspberry Pi) and assign it a static IP address. On
your router, you set the DNS primary server to the Pi-hole IP address. When a
device connects to your home network, it gets the Pi-hole IP address as its main
DNS server. When your device looks up the address for a hostname, it contacts
the Pi-hole. If the host is an ad or tracker, the request is instantly blocked.
Otherwise, the lookup is performed on some upstream server of your choice (e.g.
OpenDNS, Cloudfare, GoogleDNS, your ISP).

![Fig. 1: Pi-hole setup overview](../images/Pihole/overview.png)

## Installation ##
Make sure you have atleast 2GB of storage space available, and 512MB of RAM
before proceeding with the installation. Start by installing Raspbian according
to the [Raspberry Pi documentation](https://www.raspberrypi.org/software/).
Then, be sure to install `git` with the following command:

```bash
sudo apt install git
```

To install Pi-hole, clone its git repository and run the install script.

```bash
git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
cd "Pi-hole/automated install/"
sudo bash basic-install.sh
```

The script will guide you through the installation steps and ask for your input
to configure basic settings. At some point, it asks you to select an upstream
DNS provider (Fig. 1). This is the server on which lookups of non-blocked
hostnames will be performed.

![Fig. 1: Select upstream DNS](../images/Pihole/dns.png)

Then, it will ask you to select a couple of adlists. We suggest you leave both
on, which is the default (Fig. 2). Later, you'll be able to add custom lists if
you wish to.

![Fig. 2: Pi-hole adlist selection](../images/Pihole/adlists.png)

Pi-hole is able to block ads on IPv4 and IPv6. Unless you have a specific reason
to disable any of those protocols, you can leave both on (Fig. 3).

![Fig. 3: Pi-hole protocol selection](../images/Pihole/protocols.png)

It also includes a web interface which you can access to manage your Pi-hole
instance. If you're comfortable with command line usage, you can skip the web
interface installation. Otherwise, we suggest you install it (Fig. 4), as well
as the corresponding web server (Fig. 5).

![Fig. 4: Install web interface](../images/Pihole/webinterface.png)

![Fig. 5: Install web server](../images/Pihole/webserver.png)

You can choose to log the queries performed on your Pi-hole (Fig. 6), and set a
privacy level dictating which kind of logs are stored (Fig. 7). If you're
sharing your Pi-hole instance with other people, beware that logs may leak
private information (that will be visible to you), so choose your privacy levels
accordingly.

![Fig. 6: Set query logs](../images/Pihole/logs.png)

![Fig. 7: Set log privacy level](../images/Pihole/privacy.png)


When the installation is finished, you'll get a summary message that includes
the IP addresses of your Pi-hole and the randomly generated admin password (Fig.
8). Be sure to save this somewhere (either screenshot or pen & paper) as you'll
need it later.

![Fig. 8: Pi-hole installation summary](../images/Pihole/summary.png)

Click OK and Pi-hole is now installed on your device. To check the installation
succeeded, open a web browser and go to <http://IP_ADDRESS/admin>, where
`IP_ADDRESS` is the IPv4 address of your Pi-hole device (Fig. 8). Note that the
<http://pihole/admin> only works **after** you setup your device to use the
Pi-hole DNS server. Click on log-in and enter your password. You should now be
in the Pi-hole admin panel (Fig. 9).

![Fig. 9: Pi-hole admin panel](../images/Pihole/admin.png)

## Setup ##
Now that you have Pi-hole installed, the last step is configuring your clients
to use it as its DNS server. You can do so in the following ways:

1. Change your router's DNS server and point it to the Pi-hole IP address,
   ensuring any client that connects to your network receives the Pi-hole as its
   DNS server.
2. Manually setup each client, changing its DNS primary server to match the
   Pi-hole IP address.

The first method is preferred as you only have to do it once. However, it's not
always possible as some routers do not allow you to change the DNS settings.
Check your router's documentation to learn how to change its DNS servers.
Typically this requires you to access the router's administration panel. There,
you should have a field to set the primary and secondary DNS servers. Set the
primary address to the Pi-hole's IP address, and reset any open network
connection you may have on your devices. Now, when you connect to your home
network through a device, you should get the Pi-hole as the DNS server.

If you're unable to change your router's DNS settings, you'll have to manually
configure each device to use the Pi-hole address as DNS server. This should be
relatively straightforward, and you can find many guides online on how to change
DNS settings on a particular device/operating system. Note that there is also a
more advanced alternative which involves setting the Pi-hole as the DHCP server,
instead of your router. Refer to the [official
documentation](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026)
to learn how to do so.

<!-- ## Further configuration ## -->
<!-- After following this guide you should have a working installation of Pi-hole -->
<!-- which is assigned as the DNS servers of devices inside your home network. If you -->
<!-- wish to fine-tune your installation, consult the Pi-hole documentation. -->

<!-- If you wish to be able to use your Pi-hole instance *outside* of your home -->
<!-- network, you'll need to either setup a VPN or expose port 53 in your router to -->
<!-- the world. -->