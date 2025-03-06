# Skyhole-VPN-AWS - Will hopefully complete this after graduate school!
Create a Skyhole (PiHole + Cloud + Unbound) VPN (Wireguard/OpenVPN) in the AWS Cloud using Lightsail.

## Skyhole VPN in the AWS Cloud &#11123;

To begin, we will need a couple of things

1. An AWS Account
2. Time
3. Money
4. 5th Grade reading comprenhension level
5. More time to troubleshoot.

Once you have created or logged into your AWS account.

Using the search bar, look up Lightsail, and click on it. (Should open a new tab in your browser)

Context: Amazon Lightsail is a virtual private server (VPS) provider and is the easiest way to get started with AWS for developers, small businesses, students, and other users who need a solution to build and host their applications on the cloud. Lightsail provides developers compute, storage, and networking capacity and capabilities to deploy and manage websites and web applications in the cloud. Lightsail includes everything you need to launch your project quickly – virtual machines, containers, databases, CDN, load balancers, DNS management, etc. – for a low, predictable monthly price. <br>

## Lightsail - Start a new Debian-based EC2 instance

Click on the 'Create Instance' button

From there, you will click the respectable Region and AZ (Availability Zone) you want to use. For this example, we will use US-East-1/Zone A but feel free to pick whichever you want.

As for the Pick your instance image:
Under Linux/Unix, click 'OS Only' and select the latest Debian or Ubuntu-based image.

Under SSH key, use a new key or existing key you may already have. Be sure to keep this is a safe place, such as a secure and private S3 bucket, or under your mom's bed through an USB.

If you'd like to have automatic snapshots, you can but I recommend only doing automatic snapshots ONCE everything is running perfectly and you're only coming into the VM for updates, maintenance, etc.

Choose your instance plan:
Personally, if it's just you, for 3-4 devices, you can and should be fine with doing the $5 a month instance. If you want some headroom, the $7 a month is good for more RAM, storage, and transfer.

If you're going to use this for multiple people/devices exceeding 20-40. Just think if it's going to be concurrent or not and go based on CPU/Memory needs for that needed capacity.

Identify your instance:
Pick a name, create tags if you want, and create the instance.

## Networking Detour
Go into the Networking tab within AWS Lightsail. (4th tab from left to right)

Sooo the reason we are going to the network tab is... to get a static IP from AWS. This will help to not only ensure our VPN and instance is always accessible from the same external IP, but it will also help avoid headaches.

Click 'create static IP'\
[If you picked a Region that wasn't us-east-1, for the love of god, change the region to be the same as the static ip]

Give it a name, and create! \
Important note to read and understand: \
Static IP addresses are free only while attached to an instance. You can manage five at no additional cost.

So if you are going to make more than one Skyholes in different regions around the world... take note, you only get 5 at no cost.

Click Create at the bottom, and now look at the beautiful Public static IP that is now yours.

## Instance Settings

Click the instance once it is finished initiating. \
There are 6 tabs to take notice: \
Connect, Metrics, Snapshots, Storage, Networking, and Domains.

Click on Networking.

Attach the Public Static IP to the instance.

Under IPv6 Networking, we will be disabling it so click the switch to ensure that IPv6 networking is disabled. This means, that this resource will only communicate using the IPv4 Protocol. Important to take note, but IPv4 > IPv6. 

Under IPv4 Firewall, we can remove port 80 and 443. We can keep Port 22 to SSH into the instance in a bit, and will later add another port for our VPN client to use.

Click the Connect tab and Connect using SSH to the instance.

## SSH to Instance and Basic Setup

Once you connect to the instance, we need to update the instance for the newest packages and security updates. 

<code>sudo apt-get update</code><br>
<code>sudo apt-get upgrade -y</code><br>
<code>sudo reboot</code><br>

After it finishes rebooting, reconnect to the instance.

Once you reconnect, rejoice as you've completed the easiest part of this tutorial.

## Install PiHole

To install Pihole, we will use this one-step automated install \
```
sudo curl -sSL https://install.pi-hole.net | bash
```

We will press enter to "Ok" through a lot of these messages and prompts such as:
"This installer will transform your device into a network-wide ad blocker!"
"The Pi-hole is a SERVER so it needs a static IP address to function properly."

It may provide the static IP address we already got and provided the instance, so use that and click enter.

"Select Upstream DNS Provider. To use your own, select Custom"
As much as we'd like to use Custom, for NOW, we will select whichever, as we will change it later on. I personally use Cloudflare as I trust them FARRR more than Google.

"Pi-hole relies on third party lists in order to block ads." <br>
[*] Steven Blacok Unfied Host List \
Just ensure it is marked with the * click "Ok" and move on.

"Select Protocols" Options are IPv4, and IPv6. \
Ensure that only IPv4 is marked, press Ok, and move on.

Now you should at this point be displayed with your systems default IP, and Gateway. Confirm this, write it down even, and press Yes.

"Installation Complete!" \
You will be provided your IPv4, the web interface for doing admin tasks such as whitelisting, blacklisting, and updating the pihole to your personal specifications. For the love of all that is good, write down or copy on a notepad file the: <br>
1. Web interface URL (should be http://pi.hole/admin or http://172.x.x.x/admin) with x being some unique number. \ 
2. The Admin Webpage login password which will be a random passoword. This will be the password you will use to get into your admin panel later on in this tutorial. You can change it once your inside the admin webpage, but for now as Spongebob said; "Write that down, write that down!"

Once you press enter, the pihole will restart and now you have completed the Pi-hole step of this tutorial!

## PiVPN - OpenVPN/WireGuard

Now that Pi-hole is up and running, let’s get your VPN set up with PiVPN. You can choose between OpenVPN and WireGuard (both are solid choices, but WireGuard is like the cool kid on the block).

To install PiVPN, run the following command: 

```
curl -L https://install.pivpn.io | bash
```
Follow the prompts that appear. The installer will guide you through the setup process and will automatically detect that you’re using Pi-hole. This means it can help you configure your VPN settings to work seamlessly with your ad blocker. 

You’ll choose the protocol (OpenVPN or WireGuard), set up your user credentials, and pick your desired VPN settings. 

When prompted for the local user account, select the one you’re currently using. You’ll also be asked for a static IP or hostname for your VPN – use the static IP you set up earlier. 

Once everything is configured, the installer will generate the necessary configuration files. Make sure to save these somewhere safe (like your USB under your mom’s bed).

Add client 
qr code

Use WinSCP with the pem file you obtained when you launched the instance to connect to the instance using SFTP.
For MacOS, use whatever works for that.

## Setting up Pi-hole as a recursive DNS server solution
We will use unbound, a secure open-source recursive DNS server. The first thing you need to do is to install the recursive DNS resolver:
```
sudo apt install unbound
```

If you are installing unbound from a package manager, it should install the root.hints file automatically with the dependency dns-root-data. The root hints will then be automatically updated by your package manager.

Listen only for queries from the local Pi-hole installation (on port 5335)
Listen for both UDP and TCP requests
Verify DNSSEC signatures, discarding BOGUS domains
Apply a few security and privacy tricks

/etc/unbound/unbound.conf.d/pi-hole.conf:

```
server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0

    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # May be set to no if you don't have IPv6 connectivity
    do-ip6: yes

    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no

    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"

    # Trust glue only if it is within the server's authority
    harden-glue: yes

    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes

    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no

    # Reduce EDNS reassembly buffer size.
    # IP fragmentation is unreliable on the Internet today, and can cause
    # transmission failures when large DNS messages are sent via UDP. Even
    # when fragmentation does work, it may not be secure; it is theoretically
    # possible to spoof parts of a fragmented DNS message, without easy
    # detection at the receiving end. Recently, there was an excellent study
    # >>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<
    # by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)
    # in collaboration with NLnet Labs explored DNS using real world data from the
    # the RIPE Atlas probes and the researchers suggested different values for
    # IPv4 and IPv6 and in different scenarios. They advise that servers should
    # be configured to limit DNS messages sent over UDP to a size that will not
    # trigger fragmentation on typical network links. DNS servers can switch
    # from UDP to TCP when a DNS response is too big to fit in this limited
    # buffer size. This value has also been suggested in DNS Flag Day 2020.
    edns-buffer-size: 1232

    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes

    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1

    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m

    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
Start your local recursive server and test that it's operational:
```
```
sudo service unbound restart
dig pi-hole.net @127.0.0.1 -p 5335
```
The first query may be quite slow, but subsequent queries, also to other domains under the same TLD, should be fairly quick.

### Test validation
You can test DNSSEC validation using
```
dig fail01.dnssec.works @127.0.0.1 -p 5335
dig dnssec.works @127.0.0.1 -p 5335
```
The first command should give a status report of SERVFAIL and no IP address. The second should give NOERROR plus an IP address.

Configure Pi-hole
Finally, configure Pi-hole to use your recursive DNS server by specifying 127.0.0.1#5335 as the Custom DNS (IPv4):

Upstream DNS Servers Configuration

(don't forget to hit Return or click on Save)

Disable resolvconf.conf entry for unbound (Required for Debian Bullseye+ releases)
Debian Bullseye+ releases auto-install a package called openresolv with a certain configuration that will cause unexpected behaviour for pihole and unbound. The effect is that the unbound-resolvconf.service instructs resolvconf to write unbound's own DNS service at nameserver 127.0.0.1 , but without the 5335 port, into the file /etc/resolv.conf. That /etc/resolv.conf file is used by local services/processes to determine DNS servers configured. You need to edit the configuration file and disable the service to work-around the misconfiguration.

Step 1 - Disable the Service
To check if this service is enabled for your distribution, run below one. It will show either active or inactive or it might not even be installed resulting in a could not be found message:

```
systemctl is-active unbound-resolvconf.service
```

To disable the service, run the statement below:
```
sudo systemctl disable --now unbound-resolvconf.service
```

Step 2 - Disable the file resolvconf_resolvers.conf
Disable the file resolvconf_resolvers.conf from being generated when resolvconf is invoked elsewhere.
```
sudo sed -Ei 's/^unbound_conf=/#unbound_conf=/' /etc/resolvconf.conf
sudo rm /etc/unbound/unbound.conf.d/resolvconf_resolvers.conf
```
Restart unbound.
```
sudo service unbound restart
```

## Clean up
