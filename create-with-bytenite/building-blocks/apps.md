---
icon: compass-drafting
---

# Apps

An **App** is a versioned program that lets you run code on ByteNite.

Apps can be simple—just a single function—or complex systems with multiple libraries and functions.

This guide walks through the structure of an app’s directory and explains where (and how) to write your code so it integrates smoothly into a ByteNite app.

## 📁 App directory overview

To create a new `app` directory in your local environment, execute the following [bytenite-dev-cli.md](../bytenite-dev-cli.md "mention") command:

```bash
bytenite app new [app_name]
```

A sample folder with pre-populated files and fields will be generated at your current path:

```
/[app_name]
├── app
│   ├── main.*
│   └── [scripts/libraries]
├── manifest.json
├── template.json
└── schema.json
```

**Directory Structure**:

* `app/`\
  Contains your application’s entry point (`main.*`), plus any additional scripts or libraries.\
  See: [#develop-the-main-script](apps.md#develop-the-main-script "mention").
* `manifest.json`\
  Holds configurations, details, and app requirements.\
  See: [#configure-app-settings-manifest.json](apps.md#configure-app-settings-manifest.json "mention").
* `template.json`\
  Defines your job template, which ties together your building blocks into a single configuration. This template can be optionally submitted with your app.\
  Learn more: #[job-templates.md](job-templates.md "mention").&#x20;
* `schema.json`\
  Provides input parameter validation for your app using a JSON schema definition.\
  See:[#optional-require-parameter-validation-schema.json](apps.md#optional-require-parameter-validation-schema.json "mention").



***

## 🔧 Configure app settings: `manifest.json`

The **manifest.json** file holds the core details of your app—things like platform configuration, hardware requirements, the app’s name, and version.

Here’s a sample manifest:

{% code title="manifest.json - full example" %}
```json
{
  "name": "my-first-stable-diffusion-app",
  "version": "0.4",
  "description": "A stable diffusion app using HuggingFace's diffusers",
  "platform": "docker",
  "entrypoint": "main.py",
  "platform_config": {
    "container": "huggingface/diffusers-pytorch-cuda:latest",
    "private_image": true,
    "username": "alex_rivers6241",
    "token":"dckr_pat_HgNOmERVLDm1YBSvAJELJeGOOAM"
  },
  "device_requirements": {
    "min_cpu": 2,
    "min_memory": 2,
    "gpu": ["NVIDIA A100-SXM4-40GB", "NVIDIA GeForce RTX 4090"] 
  }
}
```
{% endcode %}

**How the manifest works**

The manifest defines key settings your app depends on, including:

* Platform configuration (e.g., Docker container details)
* Hardware requirements (e.g., minimum CPU and memory)
* App metadata (name, version, description)

Since many apps rely on specific hardware or container setups, tweaking this file to align with your app’s needs is essential for reliable performance.

The following sections dive deeper into app versioning, platform settings, and hardware requirements—all centered around how to define them in your manifest.json.

***

### App versioning

Each app is identified by a `name` and a `version`, following the semantic versioning format "major.minor".&#x20;

* **Unlimited versions**: You can store and activate unlimited versions of your app, provided that each one has a unique `(name, version)` pair.
* **Uploading a new version**: Uploading an app with a new `(name, version)` combination will create a new, independent record. This is helpful if you need to maintain separate versions for testing, staging, or production.
* **Updating versions**: Uploading an app with an existing `(name, version)` pairwill overwrite the current version with your updated code and configurations.

Use the `name` and `version` fields in the manifest to manage app uploads and ensure consistency across your deployments:

<details>

<summary><code>name</code>  <em>string</em></summary>

**Description:**

The human-readable name of your app. This acts as the app’s identifier.

**Supported Format:**

* Alphanumeric characters, dashes (-), and underscores (\_)
* Max length: 64 characters

**Example:**

`"my-first-stable-diffusion-app"`

</details>

<details>

<summary><code>version</code>  <em>string</em></summary>

**Description:**

A semantic version for managing updates.

**Supported Format:**

`"[major].[minor]"`&#x20;

* `major`  _int_
* `minor`  _int_

**Example:**

`"0.4"`

</details>

***

### Platform & hardware requirements

ByteNite supports containerized workloads by integrating with Docker images from [Docker Hub](https://hub.docker.com). Docker lets you package your app’s dependencies, libraries, and runtime into a single, portable container—so your app runs consistently across environments.

To configure your app’s environment:

* Define the **platform** type and the container image using the `platform` and `platformConfig` fields in your manifest.
* Set your app’s minimum **hardware requirements** (like CPU and memory) using the `deviceRequirements` field.

These settings ensure your app runs in the right environment with the right resources. Learn more below.

#### Platform Configs

To run your app on ByteNite, use the `platform` and `platformConfig` fields in the manifest to define the Docker container image your app should use.

The container image must be either publicly accessible or include the proper credentials if it’s private. It can be hosted on Docker Hub or any compatible container registry.

While there are many ready-to-use images, most apps require custom images tailored to specific dependencies, environment variables, or system configurations. Defining this ensures your app runs exactly how you need it.

<details>

<summary><code>platform</code>  <em>string</em></summary>

**Description:**

Specifies the platform your app will run on.

**Supported Values:**

* `["docker"]`

</details>

<details>

<summary><code>platformConfig</code>  <em>object</em></summary>

**Description:**

Contains platform-specific configurations.

**Supported Properties:**

* `container`  _string_\
  A **Docker container image reference**. Refer to the official [Docker documentation](https://docs.docker.com/reference/cli/docker/image/ls/) to choose the right base image for your app.\
  Examples:
  * "python:3.8-alpine"
  * "tensorflow/tensorflow:latest-gpu"
  * "blender/blender:latest"\

* `private_image`  _boolean_\
  Set this to true if your **image repository is private**.\

* `username` _string_\
  Your **Docker Hub username** (only needed if `private_image` is true).\

* `token` _string_\
  Your [**Docker Hub Personal Access Token (PAT)**](https://www.docker.com/blog/docker-hub-new-personal-access-tokens/) for authenticating image pull requests (only required if `private_image` is true).

**Example:**

```json
{
    "container": "huggingface/transformers-pytorch-cpu:latest",
    "private_image": true,
    "username": "alex_rivers6241",
    "token":"dckr_pat_HgNOmERVLDm1YBSvAJELJeGOOAM"
}
```

</details>

In summary, these are the suggested steps to pair your ByteNite app with a Docker container image:

1\. **Build or choose** a Docker image with all the necessary libraries and dependencies.

2\. **Develop your app**’s logic _outside_ the image—place your code in the app/ directory.

3\. **Upload your app**  using the ByteNite CLI, referencing your Docker image in the manifest file.

#### Hardware Requirements

At ByteNite, we handle the infrastructure so you can focus on building your apps—not worrying about hardware.

To ensure smooth performance, you’ll define your app’s **minimum hardware requirements**, like the number of CPU cores and memory size. This helps ByteNite allocate machines that meet (or exceed) these requirements, making sure your app runs reliably.

You set these requirements in the `deviceRequirements` field of your manifest.json.

{% openapi-schemas spec="dev-api" schemas="commonDeviceRequirements" grouped="true" %}
[OpenAPI dev-api](https://api.bytenite.com/v1/dev/docs/swagger.json)
{% endopenapi-schemas %}

***

## 👨🏽‍💻 Develop the main script

The entry point script powers your app’s core functionality—it reads input chunks, processes data, and returns results.

What your script can do is nearly limitless. You can run model inferences, process large datasets, render video frames, generate PDFs in bulk, scrape the web. If it can run inside a container, it can run on ByteNite.

See: [#examples-of-core-processing-use-cases](../../getting-started/how-it-works.md#examples-of-core-processing-use-cases "mention") for common serverless job examples.

If needed, extend your entry point script by adding custom scripts or libraries to the app/ folder. These can be imported into your main script to support your logic.

The key requirement is **how you handle inputs, parameters, and outputs**. These must follow ByteNite’s conventions to ensure your workflow runs smoothly across distributed infrastructure.

**How ByteNite Executes Your Script**

* ByteNite **pulls the container image** specified in your manifest.
* It **overrides the container’s original entry point** and runs the **ByteNite entry point script** instead—your script inside the app/ folder.

This script must align with the programming language and libraries supported by your container image.

{% hint style="info" %}
Make sure any external dependencies your code needs are installed in the container image. This ensures they’re available at runtime.
{% endhint %}

{% hint style="warning" %}
Important: Container Entrypoint ≠ ByteNite Entrypoint

When your app runs:

* ByteNite launches the container based on your specified image.
* Your container’s original entrypoint is overridden.
* ByteNite runs its own entrypoint to execute the script inside the app/ folder.

This setup ensures your runtime environment stays flexible and modular—making it easier to iterate on your app’s logic without changing the container itself.
{% endhint %}

### Default environment variables

Alongside your entry point script, ByteNite injects your container with **environment variables**. These variables provide paths to your app’s input and output folders, plus any job parameters.

Configuring your app’s data flow using these variables ensures your job runs smoothly and interacts properly with ByteNite’s components.

Here are some predefined environment variables available in every ByteNite App:

<details>

<summary><code>TASK_DIR</code>  <em>environment variable</em></summary>

**Description:**

Contains the path to the directory holding a single input data chunk. The chunk is stored in a file named `data.bin`.

* This file holds the raw binary input data for your task, passed by the Partitioning Engine.
* If you use a passthrough partitioner, data.bin is the original data source.

The directory is **automatically created** and contains only this one chunk—you don’t need to manage it manually. Just use this variable to access the input file.

Be sure to **load and decode `data.bin`** based on your app’s expected data type.

If your app doesn’t process input files, you can skip this step.

**Usage (Python):**

```python
task_dir = os.getenv('TASK_DIR')
chunk_path = os.path.join(task_dir, 'data.bin')
```

</details>

<details>

<summary><code>TASK_RESULTS_DIR</code>  <em>environment variable</em></summary>

**Description:**

This is the directory where your app should **store processed results**. Files saved here must be readable by the **Assembling Engine**.

* The assembler collects these files across all runs of your app.
* For passthrough assemblers, any file saved here is directly uploaded to the job’s data destination.

The folder is **automatically created** inside your app's environment. There’s no required output format—just make sure the assembler can access the files.

**Usage (Python):**

```python
task_results_dir = os.getenv('TASK_RESULTS_DIR')
```

</details>

<details>

<summary><code>APP_PARAMS</code>  <em>environment variable</em></summary>

**Description:**

This variable provides your app with **parameters passed through a job**. It contains a dictionary based on the data provided in the **Create Job** request (under `params` -> `app`).

These parameters let you adjust your app’s behavior per job.

For example, a parameter like "case" could control output string formatting, or a parameter "prompt" might contain a text prompt for a stable diffusion pipeline.

**Usage (Python):**

```python
app_params = json.loads(os.getenv('APP_PARAMS'))
```

</details>

<details>

<summary><code>CHUNK_NUMBER</code> <em>environment variable</em></summary>

**Description**

This variable represents the current chunk number of the task being executed.
It starts at 0 and goes up to the total number of chunks in that particular job - 1.

You can use this variable to implement logic specific to each chunk. For example, saving output files with unique names based on the chunk index (result_0.txt, result_1.txt, etc.) or applying different processing rules depending on the chunk number.

**Usage (Python):**

```python
chunk_number = os.getenv('CHUNK_NUMBER')
```

</details>

<details>

<summary><code>SHARED_CACHE_DIR</code> <em>environment variable</em></summary>

**Description**

This variable provides the path to the shared cache directory available within your application environment.
It allows you to read large pre-stored files such as machine learning models, without needing to download or bundle them with your app.

For example, large LLM model files (in the order of gigabytes), like:

* `Llama-4-Scout-Q4_K_M-00001-of-00002.gguf`
* `Llama-4-Scout-Q4_K_M-00002-of-00002.gguf`

are already available in this shared cache directory. You can directly reference and read these files through this variable.

> **Note:** This directory is **read-only**; you cannot modify or write files to it.

**Usage (Python):**

```python
  shared_cache_dir = os.getenv('SHARED_CACHE_DIR')
  model_path = os.path.join(shared_cache_dir, 'Llama-4-Scout-Q4_K_M-00001-of-00002.gguf')
```

</details>

<details>

<summary><code>USER_CACHE_DIR</code> <em>environment variable</em></summary>

**Description**

Similar to `SHARED_CACHE_DIR`, the `USER_CACHE_DIR` environment variable provides the path to a **user-specific cache directory**.
You can **read and write** files in this directory, and any files stored here are **persisted across subsequent jobs**.

Additionally, a dedicated API: **GET** `/v1/dev/cache/upload-url` is available to **pre-store files** in this directory **before executing a job**, enabling **faster access on the first run**.

While its functionality is similar to the shared cache, the key difference is:

* `SHARED_CACHE_DIR` → shared among multiple users (read-only)
* `USER_CACHE_DIR` → private to your user (read/write access)

**Usage (Python):**

```python
user_cache_dir = os.getenv('USER_CACHE_DIR')
```

</details>

<details>

<summary><code>SECRETS</code> <em>environment variable </em></summary>

**Description**

This environment variable provides access to the **secrets** that have been securely stored in your ByteNite account.
You can use it to read credentials or configuration data (e.g., API keys, cloud access tokens) required by your application during execution.

For a complete overview of how to create and manage secrets, refer to the [Setting up secrets](../../launch-with-bytenite/data-sources/README.md#setting-up-secrets "mention") guide.

The value returned by this variable is a JSON-formatted string containing a list of secret objects.

**Usage (Python):**

```python
secrets = os.getenv('SECRETS')
```

**Example structure:**

```json
[
    {
        "id": "TEST_SECRET",
        "secretType": "other",
        "expiresAt": "2026-07-08T00:00:00Z",
        "accessKey": "AKIAXXEXAMPLEEXAMPLEX",
        "params": {},
        "name": "test_secret",
        "secretKey": "secretKey"
    }
]
```

You can either **parse and filter** the required secret from this list,
**or** directly access the `secretKey` through the environment variable corresponding to its `id`.

ByteNite automatically injects each secret into the container as an environment variable,
so you can retrieve it directly, just make sure to use the **exact secret ID**, as names are **case-sensitive**.

**Usage (Python):**

```python
secret_key = os.getenv('TEST_SECRET')
```

</details>

### Example: Python entry point script

Here’s an example of a Python-based entry point script (`main.py`) that uses the environment variables we just covered.

```python
# === BYTENITE APP - MAIN SCRIPT ===

# Documentation: https://docs.bytenite.com/create-with-bytenite/building-blocks/apps

# == Imports and Environment Variables ==

try:
    import json
    import os
except ImportError as e:
    raise ImportError(f"Required library is missing: {e}")

# Path to the directory containing a single data chunk, passed by your partitioner.
# Note: This folder is automatically created and contains only one chunk. You don't need to create or manage it.
task_dir = os.getenv('TASK_DIR')
if not task_dir:
    raise ValueError("TASK_DIR environment variable is not set or is invalid.")
chunk_path = os.path.join(task_dir, 'data.bin')

# Path to the folder where your app's task results must be saved. The assembler will access these files across all runs of your app.
# Note: The folder is automatically created and passed to your app. There is no required output format—just ensure your assembler can read the files.
task_results_dir = os.getenv('TASK_RESULTS_DIR')
if not task_results_dir:
    raise ValueError("TASK_RESULTS_DIR environment variable is not set or is invalid.")

# App parameters imported from the job request (located under "params" -> "app").
app_params_raw = os.getenv('APP_PARAMS')
if not app_params_raw:
    raise ValueError("APP_PARAMS environment variable is not set or is empty.")
try:
    app_params = json.loads(app_params_raw)
except json.JSONDecodeError as e:
    raise ValueError(f"APP_PARAMS environment variable contains invalid JSON: {e}")


if __name__ == '__main__':
    print("Python task started")
    result_path = os.path.join(task_results_dir, 'processed_chunk.txt')
    try:
        # --------------
        # 1. Reading Inputs
    
        # Example: Expect a text file to be passed by the partitioner
        with open(chunk_path, 'r', encoding="utf-8") as infile:
            my_string = infile.read()

        # --------------
        # 2. Handling Parameters
        
        # Example: Expect the job parameters to have a key named 'case', and the options to be "upper", "lower", and "title"
        case = app_params['case']

        # --------------
        # 3. Developing the Core Functionality
        
        # Example: Process the string based on the case parameter
        if case == "upper":
            my_string = my_string.upper()
        elif case == "lower":
            my_string = my_string.lower()
        elif case == "title":
            my_string = my_string.title()
        else:
            my_string = my_string
        
        # --------------
        # 4. Saving Outputs

        # Example: Save the string directly into a text file to the default task results directory
        with open(os.path.join(task_results_dir, "hello_world_processed.txt"), 'w', encoding="utf-8") as outfile:
            outfile.write(my_string)

    except Exception as e:
        print("Python exception: ", e)
        raise e

```



***

## ☑️ (Optional) Require Parameter Validation: `schema.json`

If your app receives parameters, validating them ahead of time ensures they follow a defined structure. This helps prevent issues like missing keys or malformed data.

You can define a JSON Schema in the `schema.json` file inside your app directory.

Any input parameters submitted to the Jobs API will be checked against this schema before the job starts.

**Example Schema**

Here’s an example of a schema that requires a single input string (prompt) with a maximum length of 50 characters:

{% code title="schema.json" %}
```json
{
  "$id": "db://image-generation-simple-inputs",
  "definitions": {
    "input": {
      "type": "string",
      "description": "A prompt for image generation",
      "maxLength": 50
    }
  },
  "properties": {
    "prompt": {
      "$ref": "#/definitions/input"
    }
  },
  "required": ["prompt"],
  "title": "Stable Diffusion App Schema"
}
```
{% endcode %}

**Why Use a Schema?**

*   **Prevents errors**:

    Ensures parameters meet your expected format before the job runs.
*   **Generates UI**:

    The schema automatically creates a graphical interface in your Job Launch console, making it easy for you (or your users) to configure and launch jobs from the UI.



***

## 💡 App Development Tips

To get the most out of your ByteNite apps, keep the following principles in mind:

* Your app code is executed by **distributed task runners**. Each task runs independently on a worker machine, based on how your data is partitioned. \
  → **Only include the logic that should run on a single worker**—ByteNite handles all task distribution and resource orchestration for you.
* Within each worker, your app can take advantage of **multi-core architectures**. You’re free to parallelize work across available CPU cores using threads or multiprocessing.
* Avoid implementing custom distributed systems logic. ByteNite’s platform is designed to manage scaling, scheduling, and fault tolerance for you.
* Ensure your **data sources are properly configured** via the Customer API (see [data-sources](../../launch-with-bytenite/data-sources/ "mention")). Custom data ingestion or export logic within your app can lead to errors, inefficient performance, or increased container runtime—and ultimately, higher costs.
* Incorporate **robust error handling and logging**. This is essential for monitoring, debugging, and improving the reliability of your app in production.

**In summary:**

✅ **Do’s**

* Define the core functionality of your app.
* Use multiple CPU cores to process subtasks in parallel within a single worker.

❌ **Don’ts**

* Distribute tasks to other workers or machines.\
  → Your app already runs within ByteNite’s distributed environment.
* Read from or write to additional data sources directly.\
  → Input/output is managed by ByteNite’s Partitioning and Assembling Engines.

**⚠️ Use with Caution**

* Interact with external services or APIs only when necessary, and only if the required functionality cannot be handled by ByteNite’s Data Engines or built-in integrations.

