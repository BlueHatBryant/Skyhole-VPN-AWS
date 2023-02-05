# Skyhole-VPN-AWS
Create a Skyhole (PiHole + Cloud + Unbound) VPN (Wireguard/OpenVPN) in the AWS Cloud using Lightsail.=

## Skyhole VPN in the AWS Cloud &#11123;

To begin, we will need a couple of things

1. An AWS Account
2. Time
3. Money
4. 5th Grade reading comprenhension level
5. More time to troubleshoot.

Once you have created or logined into your aws account.

Using the search bar, look up Lightsail, and click on it.

Context: Amazon Lightsail is a virtual private server (VPS) provider and is the easiest way to get started with AWS for developers, small businesses, students, and other users who need a solution to build and host their applications on cloud. Lightsail provides developers compute, storage, and networking capacity and capabilities to deploy and manage websites and web applications in the cloud. Lightsail includes everything you need to launch your project quickly – virtual machines, containers, databases, CDN, load balancers, DNS management etc. – for a low, predictable monthly price. <br>

## Lightsail - Start a new Debian EC2 instance

Click on the 'Create Instance' button

From there, you will click the respectable Region and AZ (Availability Zone) you want to use. For this example, we will use US-East-1/Zone A but feel free to pick whichever you want.

As for the Pick your instance image:
Under Linux/Unix, click 'OS Only' and Debain 11.4 or 10.8.\

Under SSH key, use a new key or existing key you may already have. Be sure to keep this is a safe place, such as a secure and private S3 bucket, or under your mom's bed through an usb. 

If you'd like to have automatic snapshots, you can but I recommend only doing automatic snapshots ONCE everything is running perfectly and you're only coming into the VM for updates, maintence, etc.

Choose your instance plan:
Personally if it's just you, for 3-4 devices, you can and should be fine with doing the $3.5 a month instance. If you want some headroom, the $5 a month is good for more RAM, storage and transfer. 

If you're going to use this for multiple people/devices exceeding 20-40. Just think if it's going to be concurrent or not and go based on CPU/Memory needs for that needed capacity.

Identify your instance:
Pick a name, create tags if you want, and create instance.

## Networking Detour
Go to the Networking tab within AWS Lightsail. (4th tab from left to right)

Sooo the reason we are going to the network tab is... to get a static IP from AWS. This will help to not only ensure our VPN and instance is always assessable from the same external IP, but it just helps avoid headaches.

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

Under IPv6 Netowrking, we will be disabling it so click the switch to ensure that IPv6 networking is diabled. This means, that this resource will only communicate using the IPv4 Protocol. Important to take note, but IPv4 > IPv6. \

Under IPv4 Firewall, we can remove port 80 and 443. We can keep Port 22 to SSH into the instance in a bit, and will later add another port for our VPN client to use. \

Click the Connect tab and Connect using SSH to the instance.

## SSH to Instance and Basic Setup

Once you connect to the instance, we need to update the instance for the newest packages and security updates. 

<code>sudo apt-get update && sudo apt-get upgrade</code> \
Once that finishes, rejoice as you've completed the easiest part of this tutorial.



