# Cloudwatch

Cloudwatch has three primary responsibilities:
- Metrics collection on core infrastructure (e.g., CPU utilization, memory usage, I/O,etc.)
- Cloudwatch Logs handles logging for infrastructure (e.g., logs from services)
- Cloudwatch Events handles event generation based on infrastructure triggers

## Concepts

**Namespace**: A container for data from monitoring. Any name can be used except for the namespace reserved for AWS's data (follows `/AWS/service`).

**Metric**: Namespaces contain related metrics. Metrics are a time ordered set of **datapoints**. Metrics do not measure a specific server; they are aggregated across the namespace.

**Dimension**: Dimensions are name-value pairs included in the datapoint payload to help separate **things** or **perspectives** within the metric. Dimensions allow Cloudwatch users to trace datapoints to analyze specific subsets of datapoints.

Example: A server can measure its CPU utilization and report it to Cloudwatch. Each measurement is called a **datapoint**. 
- The datapoint contains a **timestamp** and a **value**. 
- It will also include the `InstanceId` and `InstanceType` dimensions to allow users to identify a specific instance or data across a type of instance.

**Alarm**: Alarms are created as a link to a specific metric. Alarms are either in an (1) Insufficient Data, (2) OK, or (3) Alarm state. The Insufficient Data state symbolizes that enough data is not available yet. The Alarm state can be used as a trigger for actions.
