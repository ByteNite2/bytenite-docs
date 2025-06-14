# AWS S3

## Introduction

Amazon S3 (Simple Storage Service) is a scalable, high-speed, web-based cloud storage service designed for online backup and archiving of data and applications. It offers secure, durable, and highly-scalable object storage, making it ideal for a wide range of use cases, from data lakes and mobile applications to backup and restore operations.

Follow the guide below to set up an account and get IAM credentials on AWS S3:

<details>

<summary>Getting Started with  <img src="../../.gitbook/assets/aws-logo.png" alt="AWS" data-size="line">  S3</summary>

## Getting Started with AWS S3&#x20;

#### 1. **Create an AWS Account**:&#x20;

Visit [aws.amazon.com](https://aws.amazon.com/), click “**Create an AWS Account**”, and follow the steps to register. Provide payment information and verify your identity to activate the account.&#x20;

#### 2. **Access S3**:&#x20;

Log in to the AWS Console, search for **S3**, and navigate to the S3 dashboard.&#x20;

#### 3. **Create a Bucket**:&#x20;

* Click “**Create Bucket**” in the S3 dashboard.&#x20;
* Provide a unique bucket name and choose a region (e.g., `us-east-1`).&#x20;
* Keep public access blocked unless specific use cases require otherwise.
* Finalize the setup by clicking “**Create Bucket**”.

***

## Setting Up IAM User for S3 Access&#x20;

#### 1. **Open IAM Service**:&#x20;

In the AWS Console, search for **IAM** and go to **Users**.&#x20;

#### 2. Create a User:&#x20;

* Click “**Add Users**”, provide a username (e.g., `s3-bytenite-user`), and enable **Programmatic Access**.
* Attach the `AmazonS3FullAccess` policy or create a custom policy for specific bucket access.

#### &#x20;3. Generate Credentials:&#x20;

* Complete the user creation process and download the **Access Key ID** and **Secret Access Key**.&#x20;
* Save these credentials securely; they will not be displayed again.

</details>



***

## S3 Secret

{% hint style="info" %}
`secretType`  : **`s3`**
{% endhint %}

If your S3 bucket requires authentication for read or write access, set up a secret to store your S3 credentials securely with ByteNite (see [#setting-up-secrets](./#setting-up-secrets "mention"))

Here's an example of a request body of the [secrets.md](../../api-reference/authentication-api/secrets.md "mention") endpoint for saving S3 keys:

{% code title="POST /auth/secrets" %}
```json
{
    "secret": {
        "id": "my_aws_secret",
        "secretType": "s3",
        "expiresAt": "2025-12-29T18:02:27.140Z", 
        "accessKey": "AKIAXXEXAMPLEEXAMPLEX",
        "name": "My AWS Full Bucket Access"
    },
    "secretKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
}
```
{% endcode %}



***

## S3 Data Source Object

{% hint style="info" %}
`dataSourceDescriptor`  : **`s3`**

`@type`  : [**`type.googleapis.com/bytenite.data_source.S3DataSource`**](#user-content-fn-1)[^1]&#x20;
{% endhint %}

Set up your data source with S3 using the your previously configured S3 secret and the following `params` :

<details>

<summary><code>@type</code>  <em><strong>string</strong></em></summary>

**Description:**

Use the `type.googleapis.com/bytenite.data_source.S3DataSource` params type.

</details>

<details>

<summary><code>bucketName</code>  <em><strong>string</strong></em></summary>

**Description:**

The name of your S3 bucket.

**Example:**

"my-app-data-bucket-12345"

</details>

<details>

<summary><code>cloudRegion</code>  <em><strong>string</strong></em></summary>

**Description:**

The S3 bucket's region name.

**Example:**

"us-east-2"

</details>

<details>

<summary><code>name</code>  <em><strong>string</strong></em></summary>

**Description:**

* _Usage for **Data Sources:**_\
  The **path** to your input **file** following the bucket name.
* _Usage for **Data Destinations:**_\
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

The ID of an existing `s3` secret.

**Example:**

"my\_aws\_secret"

</details>



Here is an example S3 data source and destination request body:

{% code title="POST /customer/jobs/{jobId}/datasource" %}
```json
{
    "dataSource": {  
        "dataSourceDescriptor": "s3", 
        "params": {  
            "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
            "name": "/vids/big_buck_bunny.mp4",
            "bucketName": "my-app-data-bucket-12345",
            "cloudRegion": "us-east-2",
            "secret_id": "my_aws_secret"
        }  
    },
    
    "dataDestination": {  
        "dataSourceDescriptor": "s3", 
        "params": {  
            "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
            "name": "/vids/encoded/",
            "bucketName": "my-app-data-bucket-12345",
            "cloudRegion": "us-east-2",
            "secret_id": "my_aws_secret"
        }  
    }
}
```
{% endcode %}

[^1]: This data source belongs to the S3-compatible object storage category. We use the same API request structure for all data sources having this @type.
