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
