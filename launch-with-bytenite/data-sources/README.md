---
description: Data storage methods supported by ByteNite
icon: link
---

# Data Sources

If your app needs data to function (which most do), it’s crucial to **configure your data connections** properly. This guide walks you through securely storing credentials, testing connections, and setting up your data sources.

ByteNite jobs can **read and write from popular storage services**, including Amazon S3 and Google Cloud Storage buckets.

#### How it works

* **Secrets** handle external credentials securely.
* **Data Sources** connect your storage options to your ByteNite jobs.

Once configured, secrets and data sources work seamlessly across any job.



***

## 🔐 Setting up secrets

If you’re using an authenticated service like S3, you’ll store your credentials securely in your ByteNite account—**keeping them out of your code**.

#### Steps:

1.  **Use the `secretType` field** to select your secret provider (e.g., AWS, Google Cloud Storage).

    ByteNite supports multiple providers. See: [#supported-data-source-options](./#supported-data-source-options "mention").
2.  **Send a request to the** [secrets.md](../../api-reference/authentication-api/secrets.md "mention") **endpoint** to securely create and store your secret in your account.

    This unlocks access to your data sources without hardcoding credentials.

Configure your secret request with the following parameters:

<details>

<summary><code>id</code>  <em><strong>string</strong></em></summary>

**Description:**

A given ID for your secret. \
&#xNAN;_**Hint:**_ Use a string that can easily help remember the credential's scope, permissions, and provider.

**Example:**

"aws\_full\_s3\_access\_key"

</details>

<details>

<summary><code>name</code>  <em><strong>string</strong></em></summary>

**Description:**

A descriptive name for your secret.

**Example:**

"John's AWS S3 Full Access Key"

</details>

<details>

<summary><code>secretType</code>  <em><strong>string</strong></em></summary>

**Description:**

The data source provider of this secret.

**Supported Values:**

Please see [#supported-data-source-options](./#supported-data-source-options "mention")

**Example:**

"aws"

</details>

<details>

<summary><code>accessKey</code>  <em><strong>string</strong></em></summary>

**Description:**

Your data source's access key.

**Example:**

"AKIAIOSFODNN7EXAMPLE"

</details>

<details>

<summary><code>secretKey</code>  <em><strong>string</strong></em></summary>

**Description:**

Your data source's secret key.

**Example:**

"wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

</details>

<details>

<summary><code>expiresAt</code>  <em><strong>string</strong></em></summary>

**Description:**

A given expiry date for your secret, in ISO 8601 format.

**Example:**

"2025-12-29T18:02:27.140Z"

</details>

Here's an example of a secret request body:

{% code title="POST /auth/secrets" %}
```json
{
    "secret": {
        "id": "aws_full_s3_access_key",
        "name": "John's AWS S3 Full Bucket Access",
        "secretType": "aws",
        "accessKey": "AKIAXXEXAMPLEEXAMPLEX",
        "expiresAt": "2025-12-29T18:02:27.140Z"    
    },
    "secretKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
}
```
{% endcode %}



***

## 🔗 Setting up data sources

To enable **input and output** with your current storage provider, configure your data sources. You’ll provide a **`dataSource` object** for both inputs (called Data Source) and outputs (called Data Destination). Once both are set, you can:

* **Test your `dataSource` object** by sending a request to the /datasource/test endpoint.
* **Attach a Data Source** to a job via the `dataSource` field in the /jobs endpoint.
* **Attach a Data Destination** to a job via the `dataDestination` field in the /jobs endpoint.

**About the dataSource Object**

The fields below define the dataSource object. Please note:

* The `params` object varies depending on the data source type.
* For data sources requiring authentication (like S3), provide the secret ID for the required credentials in the params body.
* Use the `bypass` data source descriptor if your app doesn’t require input or output (e.g., no input data or output files).

<details>

<summary><code>dataSourceDescriptor</code>  <em><strong>string</strong></em></summary>

**Description:**

The data source type selector.

**Supported Values:**

Please see [#supported-data-source-options](./#supported-data-source-options "mention")for all data source configuration parameters.&#x20;

Use `bypass` to skip a data source.

**Example:**

"aws"

</details>

<details>

<summary><code>params</code>  <em><strong>object</strong></em></summary>

**Description:**

The data source parameters.

**Supported Properties:**

* `@type` \
  A params object type. Please see [#supported-data-source-options](./#supported-data-source-options "mention").
* Other parameters. Please refer to the specific guides under [#supported-data-source-options](./#supported-data-source-options "mention").

**Example:**

```json
{  
    "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
    "name": "/vids/big_buck_bunny.mp4",
    "bucketName": "my-app-data-bucket-12345",
    "cloudRegion": "us-east-2",
    "secret_id": "aws_full_s3_access_key"
}
```

</details>

Here is a full `dataSource`  object example:

{% code title="POST /customer/jobs/datasource/info" %}
```json
{
    "dataSource": {  
        "dataSourceDescriptor": "aws", 
        "params": {  
            "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
            "name": "/vids/big_buck_bunny.mp4",
            "bucketName": "my-app-data-bucket-12345",
            "cloudRegion": "us-east-2",
            "secret_id": "aws_full_s3_access_key"
        }  
    }
}
```
{% endcode %}



***

## ☁️ Supported data source options

Below is a list of currently supported data source connections and parameters for both inputs and outputs.

### I/O data sources

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>AWS S3</h3></td><td>secretType : <code>aws</code></td><td>dataSourceDescriptor : <code>aws</code></td><td>@type : <code>type.googleapis.com/bytenite.data_source.S3DataSource</code></td><td><a href="../../.gitbook/assets/aws-data-source-cover.webp">aws-data-source-cover.webp</a></td><td><a href="aws-s3.md">aws-s3.md</a></td></tr><tr><td><h3>Google Cloud Storage</h3></td><td>secretType : <code>gcp</code></td><td>dataSourceDescriptor : <code>gcp</code></td><td>@type : <code>type.googleapis.com/bytenite.data_source.S3DataSource</code></td><td><a href="../../.gitbook/assets/gcp-data-source-cover.webp">gcp-data-source-cover.webp</a></td><td><a href="google-cloud-storage.md">google-cloud-storage.md</a></td></tr><tr><td><h3>Storj</h3></td><td>secretType : <code>storj</code></td><td>dataSourceDescriptor : <code>storj</code></td><td>@type : <code>type.googleapis.com/bytenite.data_source.S3DataSource</code></td><td><a href="../../.gitbook/assets/storj-data-sources-cover.webp">storj-data-sources-cover.webp</a></td><td><a href="storj.md">storj.md</a></td></tr></tbody></table>

### Input-only data sources

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>Generic HTTP</h3></td><td></td><td>dataSourceDescriptor : <code>url</code></td><td>@type: <code>type.googleapis.com/bytenite.data_source.HttpDataSource</code></td><td><a href="../../.gitbook/assets/http-data-source-cover.webp">http-data-source-cover.webp</a></td><td><a href="http.md">http.md</a></td></tr><tr><td><h3>File Upload</h3></td><td></td><td>dataSourceDescriptor : <code>file</code></td><td>@type: <code>type.googleapis.com/bytenite.data_source.LocalFileDataSource</code></td><td><a href="../../.gitbook/assets/file-upload-data-source-cover.webp">file-upload-data-source-cover.webp</a></td><td><a href="file-upload.md">file-upload.md</a></td></tr></tbody></table>

### Output-only data sources

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>Temporary Bucket</h3></td><td></td><td>dataSourceDescriptor : <code>bucket</code></td><td></td><td><a href="../../.gitbook/assets/temporary-bucket-data-source-cover.webp">temporary-bucket-data-source-cover.webp</a></td><td><a href="temporary-bucket.md">temporary-bucket.md</a></td></tr></tbody></table>

