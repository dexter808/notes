# Client Side Monitoring

## Components
- Collector service
- Agent

### Collector Service
- A service or a mini application running seperate from the actual application in a seperate **Fault Domain**.
- Keeping it seperate makes sure that even if application is down / unreachable from client the collection of errors happen independently.

### Agent
- Either a service running inside of client application which asynchronusly collects reports and sends it to the Collection service.
- Or integrated with Browsers to support such reporting.
- Data collected should be minimal and should avoid:
    - Tracing of the user
    - Location leak of the user
    - Credential, Passwords and Fingerrpints should be avoided as well.
- Data should be encrypted to prevent ISP and middleboxes to modify the stats, or they can change the stats to save their SLAs.