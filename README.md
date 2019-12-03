# Wk10-11-Honeypot

## Overview
The purpose is to stand up a basic honeypot and demonstrate its effectiveness at detecting and/or collecting data about an attack. Our goal is to demonstate these basic priciples:
- Successful configuration and deployment of a network-accessible honeypot server with two primary features:
  - An attack surface that is vulnerable or exposed in some way to network-based attacks
  - A network security feature such as an IDS configured to detect and log such attacks
- Illustration of at least one attack against the honeypot that can be detected or logged in a way that captures information about the attack or the attacker

## Setup
- [x] Milestone 0: To the cloud 
- Download sdk, https://cloud.google.com/sdk/docs/quickstarts
- Also sign up for this service https://cloud.google.com/free/
- Download and install the GCP SDK on your local machine and initialize it such that you can use gcloud from the command line. Set the time zone as well
- Do these commands to get started with the command line and set up time zone
- Intitialize:
`gcloud init`
- Set the zone and region:
	- `set CLOUDSDK_COMPUTE_REGION=us-central1`
	- `set CLOUDSDK_COMPUTE_ZONE=us-central1-fzone`
- To check your configurations:	
	- `gcloud config list`
- [x] Milestone 1: Create MHN Admin VM 
   go to google cloud platform on the web -> compute engine - > new instance 
	- put inthe region and zone
	- choose the Ubuntu 18.04 Minimal boot disk	
	- Allow HTTP traffic allowed
- Setup the firewall rules
	- `gcloud compute firewall-rules create http --allow tcp:80 --description="Allow HTTP from Anywhere" --direction ingress --target-tags="mhn-admin"`
	- `gcloud compute firewall-rules create honeymap --allow tcp:3000 --description="Allow HoneyMap Feature from Anywhere" --direction ingress --target-tags="mhn-admin"`
	- `gcloud compute firewall-rules create hpfeeds --allow tcp:10000 --description="Allow HPFeeds from Anywhere" --direction ingress --target-tags="mhn-admin"`
- Creating the VM itself, will call using mhn-admin:
	- `gcloud compute instances create "mhn-admin" --machine-type "n1-standard-1" --subnet "default" --maintenance-policy "MIGRATE"  --tags "mhn-admin"  --image "ubuntu-minimal-1804-bionic-v20191024"  --image-project "ubuntu-os-cloud"  --boot-disk-size "10"  --boot-disk-type "pd-standard"  --boot-disk-device-name "mhn-admin"`
- Establish ssh access to the VM:
	- `gcloud compute ssh mhn-admin`

- [x] Milestone 2: Install the MHN Admin Application
	- Run these commands on the new terminal that pops. Download MHN and install it:
		- `cd /opt/`
		- `sudo git clone https://github.com/pwnlandia/mhn.git`
		- `cd mhn/`
		- `sudo ./install.sh`
	- Then when you get any questions during the installations press n. If its a statement press enter.

- [x] Milestone 3: Create a MHN Honeypot VM 
- Open up a new cmd terminal.
- Make new firewall rule to allow incomming TCP and UDP traffic on all ports for Honeypot:
	- `gcloud compute firewall-rules create wideopen --description="Allow TCP and UDP from Anywhere"   --direction ingress  --priority=1000  --network=default  --action=allow  --rules=tcp,udp  --source-ranges=0.0.0.0/0  --target-tags="honeypot"`
- Create a VM for honeypot naming it honeypot-1:
	- `gcloud compute instances create "honeypot-1"  --machine-type "n1-standard-1"  --subnet "default"   --maintenance-policy "MIGRATE"  --tags "honeypot"  --image "ubuntu-minimal-1804-bionic-v20191024" --image-project "ubuntu-os-cloud"  --boot-disk-size "10"   --boot-disk-type "pd-standard"   --boot-disk-device-name "honeypot-1"`
- Establish SSH access to the VM using `gcloud compute ssh honeypot-1`

      
- [x] Milestone 4: Install the Honeypot Application
	- In the new Honeypot VM we need to install the honeypot application into the VM and wire it to connect back to the admin server. This is shown in my gif
- [x] Milestone 5: Attack!
	- In kali linux run `nmap -A -T4 <IP-Address-of-Honeypot>`
	- In the mhn admin browser you should be able to see attacks on the honeypot


## Which Honeypots I depoyed:
- Dionaea (honeypot-1)
- Amun (honeypot-2)
- Snort (honeypot-4)
- Cowrie (honeypot-5)
- Agave (honeypot-3)

## Which Honeypots I depoyed:
One Issue I encountered was that when installing the mhn admin I recieved some errors that I couldnt resolve so I would have to restart the process. This took some time.

## Summary of the data collected
I will summarize this in my gif
### Creating an Honeypot VM instance
![] (CreatingHoneypotVMInstance.gif)
### Connecting HoneyPot with MHN admin server
![] (ConnectingHoneypotwithmhn.gif)
### Attacking the honeypot to reveal hackers information
![] (AttackingHoneypot.gif)









    
    
    
    
    
