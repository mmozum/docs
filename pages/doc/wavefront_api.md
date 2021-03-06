---
title: Wavefront API
keywords: getting started
tags: [getting started]
sidebar: doc_sidebar
permalink: wavefront_api.html
summary: Learn how to view API documentation, how the Wavefront API is organized, and how to invoke the API.
---

Wavefront is fully API driven. This means that all interactions between the Wavefront UI and your Wavefront instance occur through the Wavefront API.

**Note:** The Wavefront API is not the same as the `/api` endpoint that you specify for the Wavefront proxy.

The current version of the API is v2 and is accessed at `<wavefront_instance>/api/v2`. The v1 API (`<wavefront_instance>/api/`) was deprecated in 2017 and is no longer supported. 

## API Documentation

The API is publicly documented in the Swagger generated API documentation. To access the API documentation from the Wavefront UI, click the gear icon <i class="fa fa-cog"/> at the top right of the task bar and select **API Documentation**.

## Code Samples

You can find some code samples on the [VMware code website](https://code.vmware.com/samples?categories=Sample&tags=wavefront). We're providing these samples as is - some are from the Wavefront team, others will come from the community.

## API Categories
Wavefront supports the following API categories:

- **Alert** - Allows all users to retrieve all active, snoozed, in-maintenance, and invalid alerts. Users with [Alert Management permission](permissions_overview.html) can create and update alerts.
- **Cloud Integration** - Allows all users to retrieve cloud integration data types such as those available with the [AWS integration](integrations_aws_metrics.html). Users with [Proxy Management permission](permissions_overview.html) can add and remove cloud integration data types.
- **Dashboard** - Allows all users to retrieve data about dashboards, list dashboards, and return version history. Users with [Dashboard Management permission](permissions_overview.html) can save, create, delete, clone, undelete dashboards.
- **Event** - Allows all users to retrieve events and tags associated with a specific event. Users with [Event Management permission](permissions_overview.html) can create, update, and delete events. Deleting events is limited to non-system events. System events include events based on alert firings, error events, and maintenance windows.
- **External Link** - Allows all users to navigate external links. Users with [External Links Management permission](permissions_overview.html) can create, update, and delete external links.
- **Integration** - Allow all users to retrieve integrations. Users with [Integration Management permission](permissions_overview.html) can install and uninstall integration dashboards.
- **Maintenance Window** - Allows all users to retrieve a complete or filtered list of existing maintenance windows. Users with [Alert Management permission](permissions_overview.html) can create, close, update, and delete maintenance windows.
- **Message** - Allows all users to retrieve messages and mark messages read.
- **Metric** - Allows all users to retrieve details on a metric.
- **Proxy** - Allows all users to retrieve information about Wavefront proxies. Users with [Proxy Management permission](permissions_overview.html) can add and remove Wavefront proxies.
- **Query** - Allows all users to perform queries.
- **Saved Search** - Allows all users to retrieve, add, and remove saved searches.
- **Search** - Allows all users to search agents, alerts, integrations, dashboards, external links, maintenance windows, sources, and web hooks.
- **Source** - Allows all users to retrieve sources and tags associated with a source. Users with [Source Tag Management permission](permissions_overview.html) can add and remove source tags and set descriptions.
- **User** - Allows users with [User Management permission](permissions_overview.html) to retrieve a list of all users, create, update, and delete users, and manage permissions.
- **Webhook** - Allows all users to retrieve webhooks. Users with [Alert Management permission](permissions_overview.html) can create, update, and delete webhooks.


## Invoking the API

When invoking the API outside of the Wavefront UI, you must pass a Wavefront API token in the `Authorization: Bearer` header. For example, to return all alerts, invoke the following:

```shell
curl 'https://<wavefront_instance>/api/v2/alert' --header 'Authorization: Bearer <wavefront_api_token>'
```
A Wavefront API token is a string of hexadecimal characters and dashes. For example:

```
a411c16b-3cf7-4f03-bf11-8ca05aab898d
```

### Generating an API Token

To generate an API token:

1. In the Wavefront UI, click the gear icon <i class="fa fa-cog"/>  at the top right of the task bar and select your username.
1. At the bottom of your user profile, locate the section **API Access**.
1. Click **Generate**. You can have up to 2 tokens at any given time. If you want to generate a new token but already have two tokens, then you must revoke one of the existing tokens by clicking the **Revoke** link next to a token. If you create an API script using a token, and that token is revoked, that script will return an authorization error.

## API SDKs

Wavefront provides SDKs in several languages for accessing the Wavefront API and integrations for sending metrics from applications written in Go and Java:

<table style="width: 100%;">
<colgroup>
<col width="10%"/>
<col width="30%"/>
<col width="30%"/>
<col width="30%"/>
</colgroup>
<thead>
<tr><th>Language</th><th>V1 SDK</th><th>V2 SDK</th><th>Metrics</th></tr>
</thead>
<tbody>
<tr>
<td>Go</td>
<td>NA</td>
<td>NA</td>
<td markdown="span">[Go Metrics Integration](go.html)</td>
</tr>
<tr>
<td>Java</td>
<td markdown="span">[Wavefront Java client](https://github.com/wavefrontHQ/java)</td>
<td>NA</td>
<td markdown="span">[DropWizard Java Metrics Integration](dropwizard.html)</td>
</tr>
<tr>
<td>Python</td>
<td markdown="span">[Wavefront Python client](https://github.com/wavefrontHQ/python-client/tree/api-v1)</td>
<td markdown="span">[Wavefront Python client](wavefront_api_python.html)</td>
<td>NA</td>
</tr>
<tr>
<td>Ruby</td>
<td markdown="span">[Wavefront Ruby client](https://github.com/wavefrontHQ/ruby-client)</td>
<td>NA</td>
<td>NA</td>
</tr>
</tbody>
</table>
