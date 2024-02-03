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

## Lightsail - Start a new Debian EC2 instance

Click on the 'Create Instance' button

From there, you will click the respectable Region and AZ (Availability Zone) you want to use. For this example, we will use US-East-1/Zone A but feel free to pick whichever you want.

As for the Pick your instance image:
Under Linux/Unix, click 'OS Only' and Debian 11.4 or 10.8.

Under SSH key, use a new key or existing key you may already have. Be sure to keep this is a safe place, such as a secure and private S3 bucket, or under your mom's bed through an USB.

If you'd like to have automatic snapshots, you can but I recommend only doing automatic snapshots ONCE everything is running perfectly and you're only coming into the VM for updates, maintenance, etc.

Choose your instance plan:
Personally, if it's just you, for 3-4 devices, you can and should be fine with doing the $3.5 a month instance. If you want some headroom, the $5 a month is good for more RAM, storage, and transfer.

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
<code>sudo curl -sSL https://install.pi-hole.net | bash </code>

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
