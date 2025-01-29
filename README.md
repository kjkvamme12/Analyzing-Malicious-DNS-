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

1. We want to pick out normal DNS usage resolving poor reputation domains. One way to do this is looking for anomalous TLDs.

2. Look through a SIEM to find queries to supscious domains, to make it easier we will use data enrichment tactics.
   - Set up the SIEM so that it is easy to separately search on the TLD portion of a request (.com, .gov, .edu, etc) 
