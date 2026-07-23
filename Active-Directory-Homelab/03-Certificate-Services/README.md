# Active Directory Certificate Services (AD CS)

Part of the [Active Directory Homelab](../) series.

## Objective
Stand up an internal certificate authority to issue and manage digital 
certificates within the domain, supporting secure authentication and 
encrypted communication.

## What I Configured
- Installed the AD CS role on [JOSHUA-SERVER2022] as an Enterprise Root CA
- Configured the CA name, validity period, and cryptographic settings 
  (2048 key length, and SHA256 hash algorithm)
- Created/configured a certificate template
- Issued a certificate from JOSHUA-SERVER2022-CA to the Windows 10 
  client machine to validate the chain of trust
- Verified certificate issuance via the Certificates MMC snap-in 
  (certlm.msc), confirming the certificate was generated using 
  RSA with the Microsoft Software Key Storage Provider

## Screenshots
<img width="584" height="414" alt="AD CS Server Role" src="https://github.com/user-attachments/assets/5a0def66-c7ff-43ac-9d26-904a18638028" />
<img width="584" height="416" alt="AD CS installed" src="https://github.com/user-attachments/assets/1299de8b-c3c7-4cc6-ab58-36df5510373c" />
<img width="564" height="413" alt="AD CS configuraion 4" src="https://github.com/user-attachments/assets/41f1aec5-a01c-4120-8320-ef9b4959043c" />
<img width="564" height="413" alt="AD CS configuration" src="https://github.com/user-attachments/assets/fddcb55e-5f9a-48a4-8d8b-e0d31c4e1fcb" />
<img width="568" height="417" alt="AD CS configration" src="https://github.com/user-attachments/assets/6021dada-ee00-4f54-8b98-b4bdc411f5f2" />
<img width="682" height="491" alt="AD CS Certificate" src="https://github.com/user-attachments/assets/002fc41c-ac63-48f9-a2a3-a45f8d667fed" />


## What This Demonstrates
Understanding of PKI fundamentals — certificate issuance, trust chains, 
and how internal CAs secure authentication and communication within an 
enterprise network. This is directly relevant to investigating 
certificate-based authentication issues or TLS-related security events.

## Notes / Lessons Learned

**Certificate template permissions:** The client initially couldn't 
enroll for the certificate template the security permissions on the 
template didn't include the relevant computer/user group with 
Enroll/Autoenroll rights. Resolved by opening the Certificate Templates 
console, adding the correct group to the template's security tab, and 
granting Read + Enroll (and Autoenroll, where applicable) permissions.

**Autoenrollment not triggering:** Even after fixing template 
permissions, the client wasn't automatically requesting a certificate 
as expected. This came down to Group Policy — autoenrollment has to be 
explicitly enabled via Computer Configuration > Policies > Windows 
Settings > Security Settings > Public Key Policies > Certificate 
Services Client - Auto-Enrollment. After enabling it and running 
`gpupdate /force` on the client, the certificate enrolled successfully 
on the next policy refresh.

**Trust chain error:** A certificate initially showed a broken trust 
path warning on the client. This was because the root CA certificate 
hadn't yet propagated to the client's Trusted Root Certification 
Authorities store. Since this was an Enterprise CA, this normally 
happens automatically via AD, but forcing a `gpupdate /force` and 
re-checking resolved the propagation delay.

These issues were a useful reminder that certificate services depend on 
several layers working together template permissions, Group Policy, 
and AD replication — and a failure at any one layer can look like a 
completely different problem on the surface.
