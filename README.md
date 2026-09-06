# Rogue Planet Theme Park SOC

Rogue Planet Theme Park SOC is a hands-on cybersecurity portfolio project that simulates a Security Operations Center for a fictional entertainment and theme park organization.

The project was designed to demonstrate network security monitoring, intrusion detection, event ingestion, SOC investigation, custom detection engineering, troubleshooting, and security visualization using an isolated virtual lab environment.

## Project Highlights

- Built an end-to-end network security monitoring pipeline
- Configured Suricata IDS on an Ubuntu SOC server
- Forwarded Suricata EVE telemetry with Filebeat
- Indexed and searched security events with Elasticsearch
- Investigated live network activity using Kibana Discover
- Created and validated a custom Suricata detection rule
- Generated controlled reconnaissance traffic from Kali Linux
- Confirmed custom IDS alerts in Kibana
- Built a Kibana SOC dashboard for security monitoring
- Troubleshot authentication, ingestion, networking, and virtual-machine performance issues

## Lab Architecture

```text
                    Rogue Planet Theme Park SOC

    ┌──────────────────────────────┐
    │          Kali Linux          │
    │        192.168.56.20         │
    │                              │
    │   Controlled Test Traffic    │
    └──────────────┬───────────────┘
                   │
                   │ Network Traffic
                   ▼
    ┌──────────────────────────────┐
    │       Ubuntu SOC Server      │
    │        192.168.56.10         │
    │                              │
    │          Suricata IDS        │
    │               │              │
    │               ▼              │
    │           Filebeat           │
    │               │              │
    │               ▼              │
    │        Elasticsearch         │
    │               │              │
    │               ▼              │
    │             Kibana           │
    └──────────────┬───────────────┘
                   │
                   ▼
          SOC Investigation
          and Visualization

## Technology Stack

| Technology | Purpose |
|---|---|
| Ubuntu Linux | Operating system for the SOC server |
| Kali Linux | Controlled network traffic generation and security testing |
| Suricata | Network intrusion detection and traffic analysis |
| Filebeat | Forwarding Suricata EVE events to Elasticsearch |
| Elasticsearch | Security event indexing, storage, and search |
| Kibana | Investigation, filtering, and security visualization |
| VirtualBox | Isolated virtual lab environment |
| Nmap | Controlled network reconnaissance testing |

## Detection Pipeline

Rogue Planet uses the following monitoring pipeline:

```text
Network Traffic
      ↓
Suricata IDS
      ↓
EVE JSON Events
      ↓
Filebeat
      ↓
Elasticsearch
      ↓
Kibana
      ↓
SOC Investigation and Visualization
```

Suricata monitors network traffic inside the virtual environment and generates network telemetry and IDS events. Filebeat forwards the events to Elasticsearch, where they can be searched, investigated, and visualized through Kibana.

## Custom Detection Engineering

A custom Suricata rule was created to detect ICMP reconnaissance traffic originating from the Kali Linux testing system and targeting the Rogue Planet SOC server.

```text
alert icmp 192.168.56.20 any -> 192.168.56.10 any (msg:"ROGUE PLANET LAB - Kali ICMP Test"; itype:8; sid:1000001; rev:1;)
```

The Suricata configuration and custom rule were validated before testing.

Controlled ICMP traffic was then generated from Kali Linux at `192.168.56.20` and directed toward the SOC server at `192.168.56.10`.

The test produced four confirmed Suricata alerts in Kibana, demonstrating that the complete detection pipeline was functioning successfully.

A second TCP reconnaissance rule was also created and successfully validated for future testing.

## SOC Investigation

Kibana Discover was used to investigate network activity and verify custom detections.

Fields reviewed during analysis included:

- Source IP address
- Destination IP address
- Destination port
- Network protocol
- Event category
- Suricata alert signature
- Event timestamp
- Host information

Traffic generated from the Kali Linux host was successfully identified and filtered within Kibana.

## Rogue Planet SOC Dashboard

A custom Kibana dashboard was created to provide a high-level view of security activity within the Rogue Planet environment.

The dashboard includes:

- Total Security Alerts
- Alerts Over Time
- Top Source IPs
- Top Destination IPs
- Alert Signatures

The dashboard allows an analyst to quickly identify where alerts originate, which systems are being targeted, and which detection signatures are being triggered.

## Troubleshooting and Validation

Building Rogue Planet required troubleshooting multiple components across the monitoring pipeline.

### Filebeat Authentication

During testing, Suricata continued generating events but new data stopped appearing in Kibana.

Filebeat logs revealed an Elasticsearch authentication error:

```text
401 Unauthorized
unable to authenticate user [elastic]
```

The Elasticsearch credentials in the Filebeat configuration were corrected and connectivity was tested with:

```bash
sudo filebeat test output
```

Successful communication was confirmed with:

```text
talk to server... OK
```

After Filebeat was restarted, live Suricata events resumed appearing in Kibana.

### Suricata Rule Validation

Suricata configuration and custom detection rules were tested with:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

Successful validation returned:

```text
Configuration provided was successfully loaded
```

### Virtual Lab Performance

Running Elasticsearch, Kibana, Filebeat, Suricata, Ubuntu, and Kali Linux simultaneously required monitoring system resources and virtual-machine performance.

This provided additional hands-on experience troubleshooting CPU, memory, service, and virtualization issues within a multi-system security lab.

## Skills Demonstrated

This project demonstrates hands-on experience with:

- Security Operations Center workflows
- Network security monitoring
- Intrusion detection systems
- SIEM and security event analysis
- Suricata rule creation
- Detection engineering fundamentals
- Elasticsearch
- Kibana
- Filebeat
- Linux administration
- Network traffic analysis
- Nmap
- Virtual networking
- Security dashboard development
- Log pipeline troubleshooting
- Authentication troubleshooting

## Project Status

The core Rogue Planet SOC environment is operational.

Completed milestones include:

- Ubuntu SOC server deployment
- Suricata IDS configuration
- Filebeat event forwarding
- Elasticsearch event ingestion
- Kibana investigation
- Kali Linux traffic generation
- Custom Suricata detection rule
- Detection validation
- SOC dashboard creation
- End-to-end monitoring pipeline verification

## Future Improvements

Future improvements may include:

- Additional Suricata detection rules
- Expanded reconnaissance and attack simulations
- MITRE ATT&CK mapping
- Alert severity categorization
- Additional monitored hosts
- More advanced Kibana visualizations
- Automated incident reporting
- Additional detection and response scenarios

## Disclaimer

Rogue Planet Entertainment Resorts is a fictional organization created for this cybersecurity portfolio project.

All security testing was performed inside an isolated virtual lab environment using systems owned and controlled by the project author.
