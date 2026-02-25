{{- generatedHeader }}
{{/*
This template can be used as a starting point for writing documentation for your new integration. For each section, fill in the details
described in the comments.

Find more detailed documentation guidelines in https://www.elastic.co/docs/extend/integrations/documentation-guidelines
*/}}
# UniFi Integration for Elastic

## Overview
{{/* Complete this section with a short summary of what data this integration collects and what use cases it enables */}}
The UniFi integration for Elastic enables collection of ...
This integration facilitates ...

### Compatibility
{{/* Complete this section with information on what 3rd party software or hardware versions this integration is compatible with */}}
This integration is compatible with ...

### How it works
{{/* Add a high level overview on how this integration works. For example, does it collect data from API calls or recieving data from a network or file.*/}}

## What data does this integration collect?
{{/* Complete this section with information on what types of data the integration collects, and link to reference documentation if available */}}
The UniFi integration collects log messages of the following types:
* ...

### Supported use cases
{{/* Add details on the use cases that can be enabled by using this integration. Explain why a user would want to install and use this integration. */}}

## What do I need to use this integration?
{{/* List any vendor-specific prerequisites needed before starting to install the integration. */}}

## How do I deploy this integration?

### Agent-based deployment

Elastic Agent must be installed. For more details, check the Elastic Agent [installation instructions](docs-content://reference/fleet/install-elastic-agents.md). You can install only one Elastic Agent per host.

Elastic Agent is required to stream data from the syslog or log file receiver and ship the data to Elastic, where the events will then be processed via the integration's ingest pipelines.

{{/* If agentless is available for this integration, we'll want to include that here as well.
### Agentless deployment

Agentless deployments are only supported in Elastic Serverless and Elastic Cloud environments. Agentless deployments provide a means to ingest data while avoiding the orchestration, management, and maintenance needs associated with standard ingest infrastructure. Using an agentless deployment makes manual agent deployment unnecessary, allowing you to focus on your data instead of the agent that collects it.

For more information, refer to [Agentless integrations](https://www.elastic.co/guide/en/serverless/current/security-agentless-integrations.html) and [Agentless integrations FAQ](https://www.elastic.co/guide/en/serverless/current/agentless-integration-troubleshooting.html) 
*/}}

### Onboard / configure
{{/* List the steps that will need to be followed in order to completely set up a working integration.
For integrations that support multiple input types, be sure to add steps for all inputs.
*/}}

### Validation
{{/* How can the user test whether the integration is working? Including example commands or test files if applicable */}}

## Troubleshooting

For help with Elastic ingest tools, check [Common problems](https://www.elastic.co/docs/troubleshoot/ingest/fleet/common-problems).
{{/*
Add any vendor specific troubleshooting here.

Are there common issues or “gotchas” for deploying this integration? If so, how can they be resolved?
If applicable, links to the third-party software’s troubleshooting documentation.
*/}}

## Scaling

For more information on architectures that can be used for scaling this integration, check the [Ingest Architectures](https://www.elastic.co/docs/manage-data/ingest/ingest-reference-architectures) documentation.
{{/* Add any vendor specific scaling information here */}}

## Reference
{{/* Repeat for each data stream of the current type
### {Data stream name}

The `{data stream name}` data stream provides events from {source} of the following types: {list types}.

For each data_stream_name, include an optional summary of the datastream, the exported fields reference table and the sample event.

The fields template function will be replaced by a generated list of all fields from the `fields/` directory of the data stream when building the integration.

#### {data stream name} fields

To include a generated list of fields from the `fields/` directory, uncomment and use:
{{ fields "data_stream_name" }}

The event template function will be replace by a sample event, taken from `sample_event.json`, when building this integration.

To include a sample event from `sample_event.json`, uncomment and use:
{{ event "data_stream_name" }}

*/}}

### Inputs used
{{/* All inputs used by this package will be automatically listed here. */}}
{{ inputDocs }}

### API usage
{{/* For integrations that use APIs to collect data, document all the APIs that are used, and link to relevent information */}}
These APIs are used with this integration:
* ...


# Common Event Format (CEF) Integration

This is an integration for parsing Common Event Format (CEF) data. It can accept
data over syslog or read it from a file.

CEF data is a format like

`CEF:0|Elastic|Vaporware|1.0.0-alpha|18|Web request|low|eventId=3457 msg=hello`

When syslog is used as the transport the CEF data becomes the message that is
contained in the syslog envelope. This integration will parse the syslog
timestamp if it is present. Depending on the syslog RFC used the message will
have a format like one of these:

`<189> Jun 18 10:55:50 host CEF:0|Elastic|Vaporware|1.0.0-alpha|18|Web request|low|eventId=3457 msg=hello`

`<189>1 2021-06-18T10:55:50.000003Z host app - - - CEF:0|Elastic|Vaporware|1.0.0-alpha|18|Web request|low|eventId=3457 msg=hello`

In both cases the integration will use the syslog timestamp as the `@timestamp`
unless the CEF data contains a device receipt timestamp.

The Elastic Agent's `decode_cef` processor is applied to parse the CEF encoded
data. The decoded data is written into a `cef` object field. Lastly any Elastic
Common Schema (ECS) fields that can be populated with the CEF data are
populated.

## Pre-Processors

There are cases where incoming CEF messages do not follow the CEF specification exactly, and this can cause errors with
message decoding. To work around this, there is an option for pre-processors, which are run before the CEF message is decoded.
These can be used modify the message to follow the CEF specification correctly, which will allow proper decoding.

The pre-processors will modify the `message` field before CEF decoding is done on the agent, but the original, non-preprocessed,
message will still be preserved in the `event.original` field when the agent sends the event.

## Logs


**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cef.device.event_class_id | Unique identifier of the event type. | keyword |
| cef.device.product | Product of the device that produced the message. | keyword |
| cef.device.vendor | Vendor of the device that produced the message. | keyword |
| cef.device.version | Version of the product that produced the message. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| destination.service.name |  | keyword |
| event.dataset | Event dataset | constant_keyword |
| event.module | Event module | constant_keyword |
| input.type | Type of Filebeat input. | keyword |
| log.source.address | Source address from which the log event was read / sent from. | keyword |
| source.service.name |  | keyword |
