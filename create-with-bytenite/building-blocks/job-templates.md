---
icon: memo-circle-info
---

# Job Templates

A **Job Template** (or simply _Template_) combines Building Blocks into one configuration file for your jobs. It:

* Provides instructions to the services that execute your job.
* Ensures your app and data engines are connected correctly.

By default, ByteNite generates a job template when you start building a new app. This allows you to focus on developing your app without needing to configure the other components upfront.

However, since your app may expect specific data types or formats, you can create and customize a job template to link your app to the appropriate partitioning and assembling engines.

### Job Template file

Example:

{% code title="template.json" %}

```json
{
  "id": "img-gen-diffusers-template",
  "description": "A template for my img-gen-diffusers app",
  "app": "img-gen-diffusers@1",
  "partitioner": "replicate-fanout",
  "assembler": "zipper",
  "dataSource": {
    "dataSourceDescriptor": "bypass"
  },
  "dataDestination": {
    "dataSourceDescriptor": "bucket"
  },
  "params": {
    "partitioner": {
      "num_replicas": 5
    },
    "assembler": {},
    "app": {
      "prompt": "A fantasy landscape, trending on artstation"
    }
  },
  "config": {
    "jobTimeout": 7200,
    "maxTaskRetries": 3,
    "taskTimeout": 7200
  }
}
```

{% endcode %}

#### **How to create a job template**

You can either:

1. Define the fields manually as shown in the example, or
2. Generate a blank template using the ByteNite CLI:

```
bytenite template new [template_name]
```

#### Key notes

* When you create a new app, a [template is generated automatically ](apps.md#app-directory-overview)and is uploaded with the app.
* If you prefer to manage templates separately, you can move your template out of the app folder and use the CLI to manage it independently.

Your template includes a user-defined `id` and an optional `description`. Use the `app`, `partitioner`, and `assembler` fields to specify which components should be used in jobs built from this template:

<details>

<summary><code>app</code>  <em>string</em></summary>

**Description:**

An **app tag** to be used with this template.&#x20;

**Supported Format:**

Please refer to the [#tag](../../other/glossary.md#tag "mention") definition.

**Examples:**

* `"img-gen-diffusers"`
* `"img-gen-diffusers@1"`
* `"img-gen-diffusers@1.2"`

</details>

<details>

<summary><code>partitioner</code>  <em>string</em></summary>

**Description:**

An **engine tag** to be used with this template.&#x20;

**Supported Format:**

Please refer to the [#tag](../../other/glossary.md#tag "mention") definition.

**Examples:**

* `"replicate-fanout"`
* `"replicate-fanout@2"`
* `"replicate-fanout@0.4"`

</details>

<details>

<summary><code>assembler</code>  <em>string</em></summary>

**Description:**

An **engine tag** to be used with this template.&#x20;

**Supported Format:**

Please refer to the [#tag](../../other/glossary.md#tag "mention") definition.

**Examples:**

* `"zipper"`
* `"zipper@3"`
* `"zipper@1.0"`

</details>

<details>

<summary><code>dataSource</code> <em>json</em></summary>

**Description**

A data source option defines where the input data for the job will come from.

**Supported Format:**
Please refer to the [data source descriptor](../../launch-with-bytenite/data-sources/README.md) definition.

**Examples:**

*Example 1: Bypass*

```json
"dataSource": {
  "dataSourceDescriptor": "bypass"
}
```

*Example 2: AWS*

```json
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
```

</details>

<details>

<summary><code>dataDestination</code> <em>json</em></summary>

**Description**

A data destination option defines where the output data for the job will be sent to.

**Supported Format:**
Please refer to the [data source descriptor](../../launch-with-bytenite/data-sources/README.md) definition.

**Examples:**

*Example 1: Bucket*

```json
"dataDestination": {
  "dataSourceDescriptor": "bucket"
}
```

*Example 2: AWS*

```json
"dataDestination": {  
      "dataSourceDescriptor": "aws", 
      "params": {  
          "@type": "type.googleapis.com/bytenite.data_source.S3DataSource",  
          "name": "/outputs/",
          "bucketName": "my-app-output-bucket-12345",
          "cloudRegion": "us-east-2",
          "secret_id": "aws_full_s3_access_key"
      }  
  }
```

</details>

<details>

<summary><code>params</code>  <em>json</em></summary>

**Description:**
  
A set of parameters for the app and engines used by the job.&#x20;  

**Supported Format:**

A JSON object with the following structure:

```json
  "partitioner": { ... },
  "assembler": { ... },
  "app": { ... }
```

**Examples:**

```json
"params": {
    "partitioner": {
      "num_replicas": 5
    },
    "assembler": {},
    "app": {
      "prompt": "A fantasy landscape, trending on artstation"
    }
  }
```

</details>

<details>

<summary><code>config</code>  <em>json</em></summary>

**Description:**

A set of configuration options for jobs created from this template.&#x20;

**Supported Format:**

A JSON object with the following structure:

```json
"config": {
  "key": "value"
}
```

**Examples:**

```json
  "config": {
    "jobTimeout": 7200,
    "maxTaskRetries": 3,
    "taskTimeout": 7200
  }
```

</details>
