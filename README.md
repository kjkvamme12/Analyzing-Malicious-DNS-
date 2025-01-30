# Analyzing Malicious DNS

## Objective

- Find DNS requests for malicious websites among normal DNS, using a SIEM
- Further explore DNS traffic in Wireshark
- Find an attack that involves an unexpected use of the DNS protocol
- Use a SIem to find an attack utilizing hte DNS protocol.

### Skills Learned


### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Finding anomalous DNS requests in a SIEM

#### - Find a way to point out a phishing domain

1. We want to pick out normal DNS usage resolving poor reputation domains. One way to do this is looking for anomalous TLDs.

2. Look through a SIEM to find queries to supscious domains, to make it easier we will use data enrichment tactics.
   - Set up the SIEM so that it is easy to separately search on the TLD portion of a request (.com, .gov, .edu, etc) 
3. Group based on the tld.tld.keyword, to see which TLDs are present in the data.
   - Group with a plit rows bucket, aggregating by Terms using tld.tld.keyword field... increase size parameter
     ![Screenshot 2025-01-29 162234](https://github.com/user-attachments/assets/9a83d785-3f8f-4324-9b61-d3d79a8e2536)

4. Ensure full domain is shown for each TLD. Add a second layer of split rows bucketing based on Terms aggregation on the field query.keyword. This will show top few hits for each TLD by default, can be adjusted using size parameter.


![Screenshot 2025-01-29 162753](https://github.com/user-attachments/assets/3b861fc9-3ac2-4864-b324-a8d3a122c7be)

Now look at the least common entries to view suspcious TLD


![Screenshot 2025-01-29 162913](https://github.com/user-attachments/assets/e803bf11-71f6-4538-80b1-9d24498d4a2c)

There it is: micr0soft-365.xyz

Open the packet capture in Wireshark and see if we can find queries relating to the suspicious domain. 


![Screenshot 2025-01-29 163344](https://github.com/user-attachments/assets/2b1a0564-65f3-4860-bbd0-860cce337aff)

Using the filter "frame contains micr0" allowed us to view DNS queries relating to the suspicious TLD

### DNS protocol used against us

Open up a pcap in wireshark, and if there is anything suspicious find out the domain name involved
1. Filter to only DNS traffic, then sort the names and expand


![Screenshot 2025-01-29 164226](https://github.com/user-attachments/assets/f19a7689-38a7-476e-bcf1-1f66babb4d38)

This appear to be DNS tunneling. A lot of this traffic has long random subdomains. If we expand the Name column and see the parenthostname "contentdelivery.info" .... this domain was used for DNS tunnelling traffic using CNAME, MX and TXT record requests. 

### Use a SIEM to find dns tunneling
Create a new visualization that would highlight DNS tunneling. 

