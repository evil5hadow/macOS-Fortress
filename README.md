osxfortress
===========

OS X Fortress: Firewall, Blackhole, and Privatizing Proxy for Trackers, Attackers, Malware, Adware, and Spammers

Kernel-level, OS-level, and client-level security for OS X. Built to
address a steady stream of attacks visible on snort and server logs,
as well as blocks ads, malicious scripts, and conceal information used
to track you around the web. After this package was installed, snort
and other detections have fallen to a fraction with a few simple
blocking actions.  This setup is a lot more capable and effective than
using a simple adblocking browser Add-On. There's a world of
difference between ad-filled web pages with and without a filtering
proxy server. It's also saved me from inadvertantly clicking on
phishing links.

This package uses these features:

	* OS X adaptive firewall
	* Adaptive firewall to brute force attacks
	* IP blocks updated about twice a day from emergingthreats.net
	  (IP blocks, compromised hosts, Malvertisers) and
	  dshield.org’s top-20
	* Host blocks updated about twice a day from hphosts.net
	* Special proxy.pac host blacklisting from hostsfile.org

This install script installs and configures an OS X Firewall and Privatizing
Proxy. It will:

	* Prompt you to install Apple's Xcode Command Line Tools and
	  Macports <https://www.macports.org/> Uses Macports to
	* Download and install several key utilities and applications
	  (wget gnupg p7zip squid privoxy nmap)
	* Configure OS X's PF native firewall (man pfctl, man pf.conf),
	  squid, and privoxy
	* Turn on OS X's native Apache webserver to serve the
	  Automatic proxy configuration http://localhost/proxy.pac
	* Networking on the local computer can be set up to use this
          Automatic Proxy Configuration without breaking App Store or
          other updates (see squid.conf)
	* Uncomment the nat directive in pf.conf if you wish to set up
          an OpenVPN server <https://discussions.apple.com/thread/5538749>
	* Install and launch daemons that download and regularly
	  update open source IP and host blacklists. The sources are
	  emergingthreats.net (net.emergingthreats.blockips.plist),
	  dshield.org (net.dshield.block.plist), hosts-file.net
	  (net.hphosts.hosts.plist), and securemecca.com
	  (net.securemecca.pac.plist)
	* Installs a user launch daemon that deletes flash cookies not
          related to Adobe Flash Player settings every half-hour
          <http://goo.gl/k4BxuH>
	* After installation the connection between clients and the
	  internet looks this this:

	  Application  <--port 3128-->  Squid  <--port 8118--> Privoxy  <----> Internet

Installation:

sudo sh ./readme-and-install.sh

Notes:

	* Configure the squid proxy to accept connections on the LAN IP
	  and set LAN device Automatic Proxy Configurations to
	  http://lan_ip/proxy.pac to protect devices on the LAN.
	* Count the number of attacks since boot with the script
	  pf_attacks. ``Attack'' is defined as the number of blocked IPs
	  in PF's bruteforce table plus the number of denied connections
	  from blacklisted IPs in the tables compromised_ips,
	  dshield_block_ip, and emerging_threats.
	* Both squid and Privoxy are configured to forge the User-Agent.
	  The default is an iPad to allow mobile device access. Change
	  this to your local needs if necessary.
	* Whitelist or blacklist specific domain names with the files
	  /usr/local/etc/whitelist.txt and
	  /usr/local/etc/blacklist.txt. After editing these file, use
	  launchctl to unload and load the plist
	  /Library/LaunchDaemons/net.hphosts.hosts.plist, which
	  recreates the hostfile /etc/hosts-hphost and reconfigures
	  the squid proxy to use the updates.
	* Sometimes pf and privoxy do not launch at boot, in spite of
	  the use of the use of their launch daemons.  Fix this by
	  hand after boot with the scripts osxfortress_boot_check, or
	  individually using pf_restart, privoxy_restart, and
	  squid_restart. And please post a solution if you find one.
	* All open source updates are done using the 'wget -N' option
    to save everyone's bandwidth

Security:

	* These services are intended to be run on a secure LAN behind
	  a router firewall.
	* Even though the default proxy configuration will only accept
	  connections made from the local computer (localhost), do not
	  configure the router to forward ports 3128 or 8118 in case
	  you ever change this or you will be running an open web proxy.