---
description: A general guide for submitting jobs via API
noIndex: true
icon: circle-play
---

# Jobs

Once your [apps.md](../create-with-bytenite/building-blocks/apps.md "mention"), [partitioning-engines.md](../create-with-bytenite/building-blocks/partitioning-engines.md "mention"), and [assembling-engines.md](../create-with-bytenite/building-blocks/assembling-engines.md "mention") are ready, and you've linked them together using [job-templates.md](../create-with-bytenite/building-blocks/job-templates.md "mention"), you're prepared to launch test and production jobs.

ByteNite utilizes an API to handle job requests — the [customer-api](../api-reference/customer-api/ "mention")— simplifying and standardizing the interaction with our server.

To begin using the API and familiarizing yourself with its endpoint, we recommend setting up a Postman collection. You can find more details at [#create-a-postman-collection-from-bytenites-oas](../getting-started/onboarding.md#create-a-postman-collection-from-bytenites-oas "mention").

Below are the steps to configure and send a new job request, broken down and explained. You may refer to the official [jobs](../api-reference/customer-api/jobs/ "mention") reference for a complete parameter list, response codes, and default code examples.



{% stepper %}
{% step %}
### Get an Access Token

Begin by obtaining a temporary access token to authenticate your requests.

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:yellow;">`POST`</mark> `https://api.bytenite.com/v1/auth/access_token`

***

_Main ref:_ [#access\_token](../api-reference/authentication-api/access-token.md#access_token "mention")
{% endtab %}

{% tab title="Body" %}
| Name     | Type     | Description           |
| -------- | -------- | --------------------- |
| `apiKey` | _string_ | Your ByteNite API Key |
{% endtab %}

{% tab title="Response [200]" %}
| Name        | Type     | Description                      |
| ----------- | -------- | -------------------------------- |
| `token`     | _string_ | Your ByteNite access token       |
| `expiresIn` | _string_ | An expiration timeout in seconds |
{% endtab %}
{% endtabs %}

Example:

{% code title="POST /auth/access_token" %}
```python
import requests

response = requests.post(
    "https://api.bytenite.com/v1/auth/access_token",
    json = {
        "apiKey": "ey2WmEsSMK7wdxpK5MaEHXeWCD5KEJZ79Koe68yrHL4kdnnTXT01hu2iss43BdaCCMgJ3dBh2IOVCycTt1mwkT3QR1dLxRFpK7TW7ExvcuCXio6nKsGjk9dYY8nbsFffrUVvYSQYsuQoF3NIb8sS4MDyZfOgGKZL9z8x22cwrwEck7vIokVhQ9fyWRVU2vwRiX3X4bQFuqTkkWCi5Vfy8IkGkga7ZPMPb21FxqK6cHRJ3zmI1JZZoZZxERnQcWJTpZRyCP4SNTuRm3ueVDNntFqYWYYrseNLcCIS42MpR00Z9rI9I5xxuQD6VQvHVrpOaPucg1E4Vw54xXr2LKEy9uHcM5WUQHkdfhiXo6zyVbZMrbjLpepgeS4nEja="
    }
)

token = response.json()["token"]
```
{% endcode %}


{% endstep %}

{% step %}
### Create a New Job

Submit a new job request using an existing job template, and give it a name.

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:yellow;">`POST`</mark> `https://api.bytenite.com/v1/customer/jobs`

***

_Main ref:_ [#jobs](../api-reference/customer-api/jobs/create.md#jobs "mention")
{% endtab %}

{% tab title="Headers" %}
| Name            | Type     | Description                        |
| --------------- | -------- | ---------------------------------- |
| `Authorization` | _string_ | An active ByteNite access `token`  |
{% endtab %}

{% tab title="Body" %}
| Name          | Type     | Description                                   |
| ------------- | -------- | --------------------------------------------- |
| `templateId`  | _string_ | ID of the job template used for this job      |
| `name`        | _string_ | A descriptive name for your job               |
| `description` | _string_ | An optional description with more information |
{% endtab %}

{% tab title="Response [200]" %}
| Name   | Type     | Description                                  |
| ------ | -------- | -------------------------------------------- |
| `job`  | _object_ | A job object, containing job metadata        |
| ↳ `id` | _string_ | The job identifier (automatically generated) |
{% endtab %}
{% endtabs %}

Example:

{% code title="POST /customer/jobs" %}
```python
response = requests.post(
    "https://api.bytenite.com/v1/customer/jobs",
    headers = {
        "Authorization": token
    },
    json = {
        "name": "My job with img-gen-diffusers template",
        "templateId": "img-gen-diffusers"
    }
)

jobId = response.json()["job"]["id"]
```
{% endcode %}


{% endstep %}

{% step %}
### Submit a Data Source and Destination

Link a data source and destination to your job, specifying input and output options as documented in the [data-sources](data-sources/ "mention") guide.

Connecting data sources is optional: if your app doesn't require any input data to work, or doesn't output data, you can specify a `bypass` data source descriptor.

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:purple;">`PATCH`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/datasource`

***

_Main ref:_ [#jobs-jobid-datasource](../api-reference/customer-api/jobs/update.md#jobs-jobid-datasource "mention")
{% endtab %}

{% tab title="Path Params" %}
| Name    | Type     | Description                |
| ------- | -------- | -------------------------- |
| `jobId` | _string_ | The `jobId`of your new job |
{% endtab %}

{% tab title="Headers" %}
| Name            | Type     | Description                        |
| --------------- | -------- | ---------------------------------- |
| `Authorization` | _string_ | An active ByteNite access `token`  |
{% endtab %}

{% tab title="Body" %}
| Name              | Type     | Description                                                                     |
| ----------------- | -------- | ------------------------------------------------------------------------------- |
| `dataSource`      | _object_ | A data source object, containing a `dataSourceDescriptor` and optional `params` |
| `dataDestination` | _object_ | A data source object, containing a `dataSourceDescriptor` and optional `params` |
{% endtab %}
{% endtabs %}

Example:

{% code title="PATCH /customer/jobs/{jobId}/datasource" %}
```python
response = requests.patch(
    f"https://api.bytenite.com/v1/customer/jobs/{jobId}/datasource",
    headers = {
        "Authorization": token
    },
    json = {  
        "dataSource": {
            "dataSourceDescriptor": "url",
            "params": {
                "@type": "type.googleapis.com/bytenite.data_source.HttpDataSource",
                "url": "https://storage.googleapis.com/my-public-bucket/my-input-file.txt"
            }
        },
        "dataDestination": {
            "dataSourceDescriptor": "bucket"
        }
    }
)
```
{% endcode %}


{% endstep %}

{% step %}
### Submit Job Parameters

If your app, partitioner, or assembler expect parameters, provide them at this step. Parameters are organized under three keys: `app`, `partitioner`, and `assembler` for clarity.

If your template includes parameter schemas, the parameters you submit here will be validated immediately, and any errors will be returned.

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:purple;">`PATCH`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/params`

***

_Main ref:_ [#jobs-jobid-params](../api-reference/customer-api/jobs/update.md#jobs-jobid-params "mention")
{% endtab %}

{% tab title="Path Params" %}
| Name    | Type     | Description                |
| ------- | -------- | -------------------------- |
| `jobId` | _string_ | The `jobId`of your new job |
{% endtab %}

{% tab title="Headers" %}
| Name            | Type     | Description                       |
| --------------- | -------- | --------------------------------- |
| `Authorization` | _string_ | An active ByteNite access `token` |
{% endtab %}

{% tab title="Body" %}
| Name          | Type     | Description                                                                 |
| ------------- | -------- | --------------------------------------------------------------------------- |
| `app`         | _object_ | A JSON object containing parameters as expected by your app                 |
| `partitioner` | _object_ | A JSON object containing parameters as expected by your partitioning engine |
| `assembler`   | _object_ | A JSON object containing parameters as expected by your assembling engine   |
{% endtab %}
{% endtabs %}

Example:

{% code title="PATCH /customer/jobs/{jobId}/params" %}
```python
response = requests.patch(
    f"https://api.bytenite.com/v1/customer/jobs/{jobId}/params",
    headers = {
        "Authorization": token
    },
    json = {
        "partitioner": {
            "numImages": 20
        },
        "app": {
            "prompt": "A beautiful sunset over the jungle"
        },
        "assembler": {
            "outExtension": "jpeg"
        }
    }
)
```
{% endcode %}


{% endstep %}

{% step %}
### Launch the Job

Run the job, including execution configurations if needed.&#x20;

Please note that you need this call to initiate the processing of your job. Without this step, your job will remain in a draft state.

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:yellow;">`POST`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/run`

***

_Main ref:_ [#jobs-jobid-run](../api-reference/customer-api/jobs/manage.md#jobs-jobid-run "mention")
{% endtab %}

{% tab title="Path Params" %}
| Name    | Type     | Description                |
| ------- | -------- | -------------------------- |
| `jobId` | _string_ | The `jobId`of your new job |
{% endtab %}

{% tab title="Headers" %}
| Name            | Type     | Description                       |
| --------------- | -------- | --------------------------------- |
| `Authorization` | _string_ | An active ByteNite access `token` |
{% endtab %}

{% tab title="Body" %}
| Name          | Type      | Description                                                                                               |
| ------------- | --------- | --------------------------------------------------------------------------------------------------------- |
| `taskTimeout` | _integer_ | An optional timeout for tasks, in seconds. After this time, a task will be stopped.                       |
| `jobTimeout`  | _integer_ | An optional timeout for jobs, in seconds. After this time, the job and any running tasks will be stopped. |
| `isTestJob`   | _boolean_ | A flag for test jobs.                                                                                     |
{% endtab %}
{% endtabs %}

Example:

{% code title="PATCH /customer/jobs/{jobId}/params" %}
```python
response = requests.post(
    f"https://api.bytenite.com/v1/customer/jobs/{jobId}/run",
    headers = {
        "Authorization": token
    },
    json = {
        "taskTimeout": 3600,
        "jobTimeout": 86400,
        "isTestJob": True
    }
)
```
{% endcode %}


{% endstep %}
{% endstepper %}
