# UniFi Integration for Elastic

NOTE: This integration is in the early alpha phase.

This integration for Elastic Agents facilitates receiving log data from Unifi devices.

Changelogs will not be maintained until this integration reaches version 0.1.0.

## Installing

To install this for testing purposes, take the built ZIP file and go to the Integrations page in Kibana.
In the top right corner, there's an option to "Create new integration".
This page will have an option to upload a ZIP file.
Select the ZIP file and click "Add to Elastic" at the bottom.
Once the integration has been uploaded, you can add it to Agent policies.

### Upgrading

To upgrade a newer version of the integration, repeat the above steps to upload the ZIP as a new integration.
Once done, you can go to the agent policy and select "upgrade" to apply the new version to your policy.
As pipelines and mapping are likely to change between versions, after installing upgrades and applying to the agent policy the data streams should manually be rolled over.
