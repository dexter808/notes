# Monitoring
- Early detection of failures of specific components.
- Can prevent cascading failures.
- Manual debugging does not scale at large scal systems where we have millions of servers running
- There are downtime costs involved.


## Server Side Errors
- Required for monitoring server side errors
- Can include metric alarm, monitoring, alerts etc.

## Client Side Errors:
- Even though the product of the backend / application is up, the client are facing some errors or issues due to some other issue, the engineers cant tell if their dstributed application is facing problems or not.
- Client side distributed monitoring accumulates data, organizes them and finds important patter and alerts if required.

## Pre-req of Monitoring systems

### Metrics and Alerts
- Measurements / Metrics are defined and calculated in real time by engineers. These mechanisms are inbuilt inside the code.
- Based on these Metrics crossing some thresholds, a alerting system can be configured in place to trigger alerts to notify th participants via email, slack, text message etc..
- This telemetry provides easy debugging and identification of issues pre hand before they cause large scale impact.

