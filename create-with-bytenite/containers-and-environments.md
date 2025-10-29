---
icon: box-open
---

# Containers & Environments

Alongside your entry point script, ByteNite injects your container with **environment variables**. These variables provide paths to your app’s input and output folders, cache directories for data sharing and persistence, as well as job parameters and secrets for secure configuration.

Configuring your app’s data flow using these variables ensures your job runs smoothly and interacts properly with ByteNite’s components.

This page explains how ByteNite’s environment works for your containers, focusing on two key areas:

* **Secret management**: How secrets are made available to your app (data source/destination linking, environment variable injection, and API-based access).
* **Cache directories**: How ByteNite provides built-in directories for storing and accessing data at runtime.\


You’ll learn how to structure your `platformConfig` object within your `manifest.json`, what environment variables are available inside your container, and how these features impact your build and deployment process.

***

## 🔑 Secret Management Approaches

ByteNite supports multiple ways to provide secrets to your app, depending on your workflow:

#### 🔗 Job Data Source/Destination Linking

**How it works:**

* You link secrets to data sources/destinations (e.g., S3, GCS) during job submission (via UI or [API](../launch-with-bytenite/jobs.md#submit-a-data-source-and-destination)).
* The `platformConfig`'s `secrets` property is typically **not required** for this approach.
* The secret is referenced in the job’s [data source/destination config](../launch-with-bytenite/data-sources/#setting-up-data-sources), not in the platform config.

**Example (`platformConfig`):**

```json
{
  "container": "python:3.8-alpine",
  "private_image": false
}
```

> _No need to reference secrets in_ `platformConfig` _for data movement._

#### **Build/Deployment:**

* Build your container as usual.
* ByteNite manages authentication and data transfer using the linked secrets, **so your code never handles credentials**.

#### **When to use:**

* When your job only needs to read or write data (like to S3 or GCS) and does not need to make custom API calls.
* This method is simple and secure, as **your code never sees the credentials** and ByteNite manages all authentication and data transfer automatically.



#### ✍️ Environment Variable Injection

**How it works:**

* Use this when your app code needs to interact with external services (APIs, SDKs, databases) that require credentials at runtime.
* ByteNite injects your secrets as environment variables so your code can securely access them.

**Typical use cases:**

* Authenticating to external APIs (e.g., AWS, GCP, OpenRouter, Twilio, Hugging Face, etc.)
* Connecting to managed databases or message queues
* Using SDKs or CLI tools that expect credentials in environment variables

**Example (`platformConfig`):**

```json
{
  "container": "python:3.8-alpine",
  "private_image": false,
  "secrets": ["MY_SECRET_ID"]
}
```

Once your `manifest.json` is configured, you can access the injected secrets in your code like shown below:

**Example (Python):**

```python
# Retrieve the access & secret key for your secret
access_key = os.environ.get('MY_SECRET_ID_ACCESS_KEY')
secret_key = os.environ.get('MY_SECRET_ID_SECRET_KEY')
```

**Build/Deployment:**

* Build your container as usual.
* Ensure your code reads secrets from environment variables.
* Ensure the correct secret IDs are referenced in `platformConfig`.

#### **When to use:**

* When your code needs to make authenticated requests to external services, use cloud SDKs, or perform any operation that requires direct access to credentials at runtime.

***

## 📂 ByteNite Cache Directory Environment Variables

In addition to secrets, ByteNite automatically injects cache directory environment variables (`USER_CACHE_DIR` and `SHARED_CACHE_DIR`) into your container at runtime. These directories are essential for storing user-specific data and accessing shared resources within your app environment.

For a full explanation of how these variables work, including usage examples and best practices, see the [Environment Variables](building-blocks/apps.md#default-environment-variables) section.

***

#### **Summary:**

* **Secrets**: Use data source/destination linking for simple data movement, or environment variable injection for direct credential access in your code.
* **Cache Directories**: Use `USER_CACHE_DIR` for user-specific, persistent storage; use `SHARED_CACHE_DIR` for global, read-only resources.
* All these environment variables are automatically injected by ByteNite and available inside your container at runtime.

By understanding these environment variables and secret management approaches, you can configure your containers securely and efficiently, and make the most of the ByteNite platform.
