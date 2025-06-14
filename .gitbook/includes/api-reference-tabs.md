---
title: API Reference Tabs
---

{% tabs %}
{% tab title="Request URL" %}
<mark style="color:yellow;">`POST`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/configs`

<mark style="color:green;">`GET`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/configs`

<mark style="color:purple;">`PATCH`</mark> `https://api.bytenite.com/v1/customer/jobs/{jobId}/configs`

***

Main ref: [#access\_token](../../api-reference/authentication-api/access-token.md#access_token "mention")
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
| Name          | Type     | Description                                                                  |
| ------------- | -------- | ---------------------------------------------------------------------------- |
| `app`         | _object_ | A JSON object containing parameters as expected by your app.                 |
| `partitioner` | _object_ | A JSON object containing parameters as expected by your partitioning engine. |
| `assembler`   | _object_ | A JSON object containing parameters as expected by your assembling engine.   |
{% endtab %}

{% tab title="Response [200]" %}
| Name        | Type     | Description                      |
| ----------- | -------- | -------------------------------- |
| `token`     | _string_ | Your ByteNite access token       |
| `expiresIn` | _string_ | An expiration timeout in seconds |
{% endtab %}
{% endtabs %}
