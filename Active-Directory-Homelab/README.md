# Active Directory Homelab

I designed a self-built enterprise-style Active Directory environment simulating the 
core identity, network, and security infrastructure a real organization 
relies on. This homelab was built to strengthen my hands-on SOC and IT administration skills.

## Objective
My main objective was to design and deploy a small multi-region AD domain covering identity 
management, network services, PKI, group policy enforcement, and 
file/data security, the same building blocks a SOC analyst needs to 
understand when investigating incidents or reviewing access controls.

## Environment
- **OS:** Windows Server, Windows 10 client
- **Virtualization:** VMware
- **Domain:** joshua.local
- **Scale:** 60 users across 3 regions and 5 departments

## What's in this project

| Section | Covers |
|---|---|
| [01 – Core Infrastructure](./01-Core-Infrastructure) | AD DS setup, DNS, DHCP |
| [02 – Users & OUs](./02 - Users-And-OUs) | OU design, 60 users across 3 regions |
| [03 – Certificate Services](./03-Certificate-Services) | AD CS setup and use case |
| [04 – Client Domain Join](./04-Client-Domain-Join) | Windows 10 joined to domain |
| [05 – Group Policies](./05-Group-Policies) | Password policy, account lockout, wallpaper, USB block, mapped drives |
| [06 – File Server & Shares](./06-File-Server-Shares) | File server with 5 department shares |

## Why this project
Most of my early cybersecurity learning happened one post at a time on 
LinkedIn but this repo brings all of it together as a single, structured 
project so it's easier to see the full scope of what was built and why 
each piece matters for identity, access, and endpoint security.

## Notes / Lessons Learned

Building this lab gave me practical experience with Active Directory administration, DNS, DHCP, Group Policy, user management, and file services. 
It also reinforced the importance of proper planning, centralized administration, and layered security in enterprise Windows environments.
