# Storj

## Introduction

Storj is a decentralized cloud storage platform that offers secure, private, and cost-effective data storage. By distributing data across a global network of nodes, Storj ensures highly-available and resilient data access, while securing cost savings to up to 90% than traditional cloud providers.

Follow the guide below to set up an account and get S3-compatible access keys on Storj:

<details>

<summary>Getting Started with  <img src="../../.gitbook/assets/storj-logo-full-color.png" alt="Storj" data-size="line"></summary>

## Getting Started with Storj

**1. Create a Storj Account Using ByteNite's Referral Link:**

Visit the link below to create an account on Storj&#x20;

🔗 [<mark style="color:purple;">**Unlock 25 GB Free Storage on Storj - ByteNite's Referral Link**</mark>](https://us1.storj.io/signup?partner=bytenite)

Complete the registration process by verifying your email and setting up your password. You can start with a free trial or select a paid plan based on your needs.

**2. Log in to the Storj Console:**

Access your [Storj console](https://www.storj.io/login) using your account credentials.

**3. Create a Project:**

* Once logged in, click “**New Project**.”
* Provide a project name (e.g., “My S3 Project”).
* Click “**Create Project**” to finalize.

**4. Create a Bucket:**

* Inside your project dashboard, navigate to the “**Buckets**” tab.
* Click “**New Bucket**.”
* Provide a bucket name (e.g., my-s3-bucket).
* Optionally, configure additional settings such as default encryption or access permissions.
* Click “**Create Bucket**” to complete the setup.

***

## Setting Up S3-Compatible Access Keys for Storj

**1. Navigate to API Keys:**

* Inside your project dashboard, click on the “**Access Keys**” tab in the navigation bar.

**2. Generate an Access Grant:**

* Click “**New Access Key**.”
* Select the "**S3 Credentials"** configuration checkbox.
* Choose the scope of the access (e.g., full project access or limited to specific buckets).

**3. Retrieve Your S3-Compatible Credentials:**

* Once the access grant is generated, the console will display the **Access Key** and **Secret Key** for S3 compatibility.
* Save these credentials securely, as the Secret Key will not be shown again.

</details>



***

## Storj Secret&#x20;

{% hint style="info" %}
`secretType`  : **`storj`**
{% endhint %}

If your Storj bucket requires authentication for read or write access, set up a secret to store your S3-compatible credentials securely with ByteNite (see [#setting-up-secrets](./#setting-up-secrets "mention"))

Here's an example of a request body of the [secrets.md](../../api-reference/authentication-api/secrets.md "mention") endpoint for saving Storj keys:

{% code title="POST /auth/secrets" %}
```json
{
    "secret": {
        "id": "my_storj_secret",
        "secretType": "storj",
        "expiresAt": "2025-12-29T18:02:27.140Z", 
        "accessKey": "jwcxl2mccgasmhs1dcir5ex4mple",
        "name": "Storj Full Access S3 Keys - Project 'My App'"
    },
    "secretKey": "jzwi4pcamnhyjpldu4my2cmfscmg55slex4mpleex4mpleex4mple"
}
```
{% endcode %}



***

## Storj Data Source Object

{% hint style="info" %}
`dataSourceDescriptor`  : **`storj`**

`@type`  : [**`type.googleapis.com/bytenite.data_source.S3DataSource`**](#user-content-fn-1)[^1]&#x20;
{% endhint %}

Set up your data source with Storj using the your previously configured storj secret and the following `params` :

<details>

<summary><code>@type</code>  <em><strong>string</strong></em></summary>

**Description:**

Use the `type.googleapis.com/bytenite.data_source.S3DataSource` params type.

</details>

<details>

<summary><code>bucketName</code>  <em><strong>string</strong></em></summary>

**Description:**

The name of your Storj bucket.

**Example:**

"my-app-data-bucket-12345"

</details>

<details>

<summary><code>cloudRegion</code>  <em><strong>string</strong></em></summary>

**Description:**

Use `global`  as the Storj bucket region.

</details>

<details>

<summary><code>name</code>  <em><strong>string</strong></em></summary>

**Description:**

* _Usage for **Data Sources**:_\
  The **path** to your input **file** following the bucket name.
* _Usage for **Data Destinations**:_\
  The **path** to the output **folder** following the bucket name. Note: a path will be created if it doesn't exist.

**Example:**

* _Data Source:_\
  "/vids/big\_buck\_bunny.mp4"
* _Data Destination:_\
  "/vids/encoded/"

</details>

<details>

<summary><code>secret_id</code>  <em><strong>string</strong></em></summary>

**Description:**

The ID of an existing `storj` secret.

**Example:**

"my\_storj\_secret"

</details>



Here is an example Storj data source and destination request body:

{% code title="POST /customer/jobs/{jobId}/datasource" %}
```json
{
    "dataSource": {  
        "dataSourceDescriptor": "storj", 
        "params": {  
            "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
            "name": "/vids/big_buck_bunny.mp4",
            "bucketName": "my-app-data-bucket-12345",
            "cloudRegion": "global",
            "secret_id": "my_storj_secret"
        }  
    },
    
    "dataDestination": {  
        "dataSourceDescriptor": "storj", 
        "params": {  
            "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
            "name": "/vids/encoded/",
            "bucketName": "my-app-data-bucket-12345",
            "cloudRegion": "global",
            "secret_id": "my_storj_secret"
        }  
    }
}
```
{% endcode %}

[^1]: This data source belongs to the S3-compatible object storage category. We use the same API request structure for all data sources having this @type.
