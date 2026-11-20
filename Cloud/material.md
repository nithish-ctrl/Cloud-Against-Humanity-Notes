# What is the Cloud 

Cloud is not just someone's else computer. 
The part that matters is how you get to use them: by the hour, through an API, with a public internet address attached, and with a bill that follows whatever you leave running.

## What a clour provider sells : 
1. Virutal Machine
 A VM is a slice of a server that behaves like a whole computer, It has its own CPU share, memoery, disk, OS and network address. You can log into it and run whatever you want. 
 Ex : DigitalOcean : Droplet , AWS: EC2 Instance

 One can use a raspberry Pi or a Proxmox VM as an alternative to the same, this is the homelab equivalent of this. 

2. Object Storage 
 A place to drop your files, you will reach it over https with an access key. No need of OS, no disk to manage. 
 Ex : AWS S3, DigitalOcean Spaces

 A NAS (Network attached Storage) is the homelab equivalent of this. 

3. Serverless Functions
 You provide a program or schedule or web address for the provider to run it when triggered and bills the second it runs. No machine to maintain at all.
 Ex : DigitalOcean : Functions, AWS : Lambda

 A cron job with no computer. 

## Virtual Machine
Physical server runs a program called *hypervisor*. It carves the hardware into slices and runs a separate operating system in each and each od this slice is a VM. 
That is why the cheapest droplet here is called s-1vcpu-1gb: one virtual CPU, one gigabyte of RAM.

### Differences from a Pi at home
Pi at home has an address like 192.168.1.20 that only works inside the house. The router between us ans the internet does not forward anything unless told so. Protected by default. 

A droplet from DigitalOcean has a public IP address that anone in the world can send packets to, and we can turn that off. Once a new machine comes online, automated scanners from all over the interent will have probed it, because they probe every address all the time. If SSH is listening with a password, the password guessing starts with an hour. 

The first job is to close things down, we do it before the machine exists, not after. The answer given in the repo is a firewall with zero inbound rules attached to every droplet at the moment it is created. 

So basically no password exists for the machine to use and its stateless too and nothing on it is irreplaceable. 

A Pi costs electricity and a droplet costs money every hour it exists, whether or not it does anything, and do does every disk, snapshot and backup attached to it. Forgetting to delete something is the most common way to get a surpise bill. 

A provider will have building in several cities and each region is represented is a short code like nyc3 for New York, tor1 for Toronto and am3 for Amsterdam datacentre. We pick a region for the latency, prize and which sizes are offered there. 

How in home, we configure things by logging in and typing while in cloud, every action (create a machine, attach a firewall, add a rule, delete a disk) is an API call which means a request to the provider's web service, authenticated with a token. The web console we click is just a front end for that. 

Since, everthing is an API call, everything can be written down as code and run by a programm. This is basically infrastruture as code, and it how this whole env is built. 

Because a token can do anything the account can do, the token is the crown jewel. Whoever holds a write-capabale DigitalOcean token can delete every machine or create a hundred expensive ones and send you a bill. 

### Responsibility
The provider is responsible for 
1. The building, power, cooling and the physical security
2. Physical servers and the hypervisor
3. Keeping tenants separated
4. Network between datacentres
5. Thier own API and console being safe. 

You are responsible for : 
1. Which ports your firewall opens
2. The operating system updates on your VM
3. Who can log in and how
4. Every secret you create and where you put it. 
5. Your account's password and MFA, and your tokens. 

## Why DigitalOcean and not AWS


