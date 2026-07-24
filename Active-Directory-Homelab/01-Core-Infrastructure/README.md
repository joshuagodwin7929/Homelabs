# Core Infrastructure — AD DS, DNS & DHCP

Part of the [Active Directory Homelab](../) series.

## Objective
Stand up the foundational services that every other part of this domain 
depends on: like the domain itself, name resolution, and automatic IP 
assignment.

## Active Directory Domain Services
- Installed the AD DS role on [Joshua-Server2022]
- Promoted the server to a domain controller and created the forest/domain: 
  **Joshua.local**
- Verified domain health using `dcdiag` and `repadmin`

- 
- <img width="584" height="416" alt="AD DS Confirmation" src="https://github.com/user-attachments/assets/fe13a76c-b489-4aa6-95c4-d23e28f0d63b" />

- <img width="586" height="416" alt="AD DS Server Role" src="https://github.com/user-attachments/assets/e25857e0-c45f-483d-94f3-ebb7d14ad71c" />

- <img width="587" height="418" alt="AD DS Installation" src="https://github.com/user-attachments/assets/bf0bd402-8c08-4ba3-859f-5d5c22bd4c82" />

- <img width="566" height="415" alt="AD DS Promoting Server" src="https://github.com/user-attachments/assets/18c888e6-8091-43b3-bb9d-277eaf172d10" />

- <img width="568" height="416" alt="AD DS Perequisite check Complete" src="https://github.com/user-attachments/assets/a7c2bdb8-40a2-4703-b495-2e7ec96bf9e3" />





## DNS
- Installed the DNS Server role (co-located on the domain controller)
- Created a forward lookup zone for **Joshua.local**, integrated with AD 
  for automatic replication
- Verified resolution using `nslookup` and `Resolve-DnsName`
- <img width="584" height="416" alt="DNS Server Roles" src="https://github.com/user-attachments/assets/920029d2-47c7-402b-99d7-1a3c51367d1a" />

- <img width="460" height="383" alt="DNS Zones" src="https://github.com/user-attachments/assets/4101258a-3f4e-4b88-a8f0-2798806bbf38" />

- <img width="685" height="428" alt="DNS Forward Zones" src="https://github.com/user-attachments/assets/fec64650-8f17-4cf7-aa68-e8047a907a6c" />

- <img width="536" height="536" alt="Screenshot 2026-07-24 020327" src="https://github.com/user-attachments/assets/c70bf0c7-0a5c-4332-a2ab-e6fe4f4b52e8" />

- <img width="233" height="264" alt="24 DNS" src="https://github.com/user-attachments/assets/216fcabb-1ecc-471b-a43b-53f4e615cd60" />

- <img width="473" height="251" alt="24 nslookup" src="https://github.com/user-attachments/assets/637f24ee-1c3d-4863-b4b1-19c50fc831c3" />



## DHCP
- Installed the DHCP Server role on [Joshua-Server2022]
- Created a scope: [IP range, e.g. 192.168.18.55–192.168.18.155]
- Configured lease duration, default gateway, and DNS server options
- Authorized the DHCP server in Active Directory
- Verified clients received correct IP, gateway, and DNS settings
- 
- <img width="584" height="413" alt="DHCP Installed" src="https://github.com/user-attachments/assets/0aedfe04-1fdc-49fc-b7fa-60a67ff3b76b" />

- <img width="386" height="317" alt="24 DHCP IP Scope" src="https://github.com/user-attachments/assets/7fb2e759-643a-40fc-9d9e-00c2f018ae1a" />

- <img width="383" height="312" alt="24 finished scope" src="https://github.com/user-attachments/assets/4cb23c91-dc6a-427d-8518-2d17684a5d05" />


- <img width="685" height="317" alt="24 IP Range" src="https://github.com/user-attachments/assets/687eda3e-0402-43a8-b5f6-89b55880dfc5" />



## What This Demonstrates
Understanding of the core services an enterprise domain runs on — 
DNS/DHCP misconfigurations are a frequent root cause investigated during 
troubleshooting and log analysis, and knowing how they should look when 
healthy is foundational to spotting when they're not.

## Notes / Lessons Learned

**DNS replication delay:** After creating the forward lookup zone, name 
resolution didn't work immediately from a test client — the record 
hadn't finished replicating yet. Running `repadmin /syncall` forced an 
immediate replication instead of waiting for the default interval, which 
confirmed the issue was replication timing rather than a misconfiguration. 
This was a useful reminder that AD-integrated DNS changes aren't always 
instant, and `repadmin` is a fast way to rule out replication delay 
before troubleshooting further.

**DHCP scope IP conflict:** A statically assigned device fell inside the 
DHCP scope range, causing an IP conflict when the scope tried to lease 
that same address to another client. Resolved it by adding an exclusion 
range in the DHCP scope for any statically assigned addresses. This 
reinforced the importance of planning static IP ranges *before* defining 
a DHCP scope, rather than discovering the overlap after clients start 
picking up addresses.
