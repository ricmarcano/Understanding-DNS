<p align="center">

![Screenshot 2023-09-05 at 10 25 58 AM](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/a062343c-43d0-4e6e-91eb-6c570d74c84c)

<h1>Understanding DNS</h1>

This lab centers on exploring DNS (Domain Name System) and its practical application. DNS is a crucial IT concept that converts computer names such as "google.com" to IP addresses which can be used by the computer to locate resources. In this lab, we'll set up DNS records to observe how it operates. This builds upon a prior lab where I had a client connected to my domain, testdomain.com. I'm logged in as Rick James, an admin, on both the client and domain controller VMs. To ensure the lab functions correctly, it's essential to be logged in as an administrator.

<h2>Technologies and Enviroments Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>

While logged in the client as an admin, open the command prompt. Attempt to ping "mainframe," it will not work. Similarly, if you use the "nslookup" command for "mainframe," you'll see there's no DNS record for it. To resolve this, create a DNS A-record for "mainframe" on the domain controller. On the domain controller, open DNS Manager > navigate to your domain under the Forward Lookup Zones tab. In my case, it's testdomain.com. Right-click on the page and select "New Host." Enter "mainframe" for the name, and use the same IP address as the domain controller. This allows "ping" to resolve correctly. Refresh the DNS server so the new record is updated. Now, when you return to the client, try pinging "mainframe" again, and it should work.

![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/957c0f02-3025-494d-abed-f53c5625ae00)
![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/3b565df7-af5f-451b-a702-99eb7ca51d6b)
![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/871f7eb2-5414-4818-b809-5df45f9eb6d9)

Next we'll explore the DNS cache. I made a change on the domain controller by updating the address for "mainframe" to 8.8.8.8 (Google) and refreshed the DNS server. However, when you ping "mainframe" from the client, it still uses the old IP address. Running the command "ipconfig /displaydns" will reveal that the DNS cache retains the old IP. To update the cache, we need to clear it using the command "ipconfig /flushdns." This action empties the cache, so when you ping "mainframe" again, the client will now use the new IP address. Consequently, pinging "mainframe" will display the updated IP address associated with the record.

![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/b24fd164-2786-41f6-88af-848191255323)
![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/5c4330ab-f2eb-416b-ae1f-f78519aaae57)
![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/2548b2f7-79bf-4a2d-ae52-8304f4b40ccc)

Now, we'll create a CNAME (Canonical Name) record on the DNS server to link "search" to Google. CNAME is a type of DNS record that maps an alias name to a true or canonical domain name. CNAME records are typically used to map a subdomain such as "www" or "mail" to the domain hosting that subdomain's content. On the Forward Lookup Zones tab, find your domain and create a new CNAME record named "search" that points to Google. Refresh the server to save these changes. On the client, when you ping "search" or use nslookup, you'll get the results from the CNAME record.

![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/609dd5f7-8a52-4f9d-b135-1bbfd78ddf00)
![image](https://github.com/ricmarcano/Understanding-DNS/assets/141169092/d4a88822-1e93-4fdc-8e2f-691c333ea314)

This lab showcased how DNS works on computers and networks. It emphasized the importance of keeping DNS records updated to avoid connection problems. The "ipconfig /flushdns" command, a common IT troubleshooting tool, became clearer after doing this lab. We started by pinging "mainframe," the computer first checked its DNS cache. If no answer was found there, it looked in the host file. Only if both of these didn't provide an answer did it turn to the DNS server. By setting up a record on the DNS server, we ensured that pinging "mainframe" worked correctly. We also learned about CNAME records, which link one name to another. In our case, "search" was linked to Google.
