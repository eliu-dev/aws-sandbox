# Cloudwatch


Cloudwatch is a public AWS service usable from AWS or an on-premises setup. Cloudwatch has three primary responsibilities:
- Metrics collection on core infrastructure (e.g., CPU utilization, memory usage, I/O,etc.)
- Cloudwatch Logs handles logging for infrastructure (e.g., logs from services)
- Cloudwatch Events handles event generation based on infrastructure triggers

Cloudwatch is used for storing, accessing, and monitoring log data. It integrates with various AWS services, such as EC2, VPC flow logs, Lambda, Cloudtrail, Route53, etc. and centralizes logs from their services.


## Concepts

**Namespace**: A container for data from monitoring. Any name can be used except for the namespace reserved for AWS's data (follows `/AWS/service`).

**Metric**: Namespaces contain related metrics. Metrics are a time ordered set of **datapoints**. Metrics do not measure a specific server; they are aggregated across the namespace.

**Dimension**: Dimensions are name-value pairs included in the datapoint payload to help separate **things** or **perspectives** within the metric. Dimensions allow Cloudwatch users to trace datapoints to analyze specific subsets of datapoints.

Example: A server can measure its CPU utilization and report it to Cloudwatch. Each measurement is called a **datapoint**. 
- The datapoint contains a **timestamp** and a **value**. 
- It will also include the `InstanceId` and `InstanceType` dimensions to allow users to identify a specific instance or data across a type of instance.

**Alarm**: Alarms are created as a link to a specific metric. Alarms are either in an (1) Insufficient Data, (2) OK, or (3) Alarm state. The Insufficient Data state symbolizes that enough data is not available yet. The Alarm state can be used as a trigger for actions.

Cloudwatch can generate metrics baesd on logs known as a **metrics filter**. Metric filters incremenent a metric. Alarms can be created based on metrics .

## Architecture
Cloudwatch is a regional service. AWS services and external sources can inject log event data into Cloudwatch. **Log events** are a timestamp and message from a single source.

Log events are stored in **log streams**. Log streams are a sequence of log events from the **same source**. For example, one EC2 instance will have one log stream while a second EC2 instance will have its own log stream. 

Log streams are grouped into **log groups**. A log group is a container for log streams from the same **type** of source. For example, a log group may contain the log streams for a group of EC2 instances.

**Metric filters** are defined on log groups. Metric filters increment a **metric** which can have alarms attached to them.

# Cloudtrail
Cloudltrail logs almost any event that can occur in an AWS account.

## Scope
- Logs API calls/activities as a **Cloudtrail Event**. A Cloudtrail Event can be a user event or service event.
- 90 days of history by default are stored in **Event History**.
- Customizing Cloudtrail, such as modifying the history length, requires creating a Cloudtrail.
- Cloudtrail is **NOT** real-time. The latency can be on the order of minutes.

### Event Types
Cloudtrail covers three types of events:

1. **Management Events** (aka Control Plane operations): Events such as creating and terminating EC2 instances.  

2. **Data Events**: Operations performed on or in a resource, such as accessing or uploading S3 objects.

3. **Insight Events**

### Cloudtrail Trail
A trail logs an event for the **region** it is configured in. However, a trail created by a user can be configured as a single-region trail or an all-region trail. 

- Regional serviecs log their events in the region they are generated in.
- Global services, such as IAM, STS, and Cloudfront generate **global service events**. Global service events log their events to one region (US-East-1).

Once a trail is created, all management events are logged by default. Data Events can be enabled.

Organization trails can store all trails in the management account.


### Storage Options

1. **S3 bucket**: Trails can be stored in a S3 bucket which has an indefinite storage period.
2. **Cloudwatch Logs**: Trails can be stored in Cloudwatch Logs to make it easier to search and filter using metrics filters.


### Pricing
- 90 days of built-in history is free.
- Delivering one copy of management events to a S3 bucket is free.
- $2/100k events charge for management events delivered to S3 associated with any custom trails.
- $0.10/100k events charge for data events delivered to S3 associated with any custom trails.