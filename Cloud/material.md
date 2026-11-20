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

