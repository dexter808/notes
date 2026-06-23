# Monitoring
- Early detection of failures of specific components.
- Can prevent cascading failures.
- Manual debugging does not scale at large scal systems where we have millions of servers running
- There are downtime costs involved.
hey 

## Server Side Errors
- Required for monitoring server side errors
- Can include metric alarm, monitoring, alerts etc.

## Client Side Errors:
- Even though the product of the backend / application is up, the client are facing some errors or issues due to some other issue, the engineers cant tell if their dstributed application is facing problems or not.
- Client side distributed monitoring accumulates data, organizes them and finds important patter and alerts if required.

## Pre-req of Monitoring systems

### Metrics and Alerts
- Measurements / Metrics are defined and calculated in real time. These mechanisms are inbuilt inside the code to calculate and report the metric with minimal performance hit.
- Based on these Metrics crossing some thresholds, a alerting system can be configured in place to trigger alerts to notify th participants via email, slack, text message etc..
- This telemetry provides easy debugging and identification of issues pre hand before they cause large scale impact.

#### Common metrics:
- Processor load and CPU statistics (eg, cache hit / misses).
- RAM usage
- Disk space status, read/write latencies, swap space usage.
- Metrics from servers like
    - API throughput
    - Number of requests being handled
- Token usage ..etc

## Data / Metric collection

### Push Model
- The server or a local agent sends the metrics to rhe monitoring system periodically. 
- Server controls the polling and might overwhelm the monitoring system if it has less resource or if a lot of servers are configured then this may overwhelm the monitoring system.
- Jobs can use the push model as they cannot expose endpoints or have live running servers.

### Pull Model
- The monitoring system pulls from an exposed endpoint on the server.
- The server stores in memory and serves the metric when requested.
- The monitoring system controls the polling frequency.

### Hybrid Model
- If there are thounds of servers, then it becomes difficult to querry each of the services.
- We use a hybrid pull and push model in such cases.
- Multi Level Data collection services
- Lowest level still gets the data using the pull model
- Higher level accumulates the data and pushes the data periodically to the TSDBs.

---

## Components
- Service Discovery: This is a task that the platform can help us identify the services we want to monitor. Kubernetes can help here.
- Data Collection service - A collection service which can pull data from several places and push to a TSDB
- TSDB - Prometheus can be used for a TSDB, which acts as a fast read/write storage.
- Blob Storage for persistance.
- Alerting system and other visualization systems can querry the blob storage or the TSDB to show real time monitoring patters like heat map and raise alarm if a specific rules is broken.

## Visualization
- Graffana provides a beautiful interface for visualization and configuration management.
- Prometheus can provide the query system to power the visualizations.
- Heat Maps, Graphs .. etc
- These can show live events and monitor problems vbefore they even occur.
