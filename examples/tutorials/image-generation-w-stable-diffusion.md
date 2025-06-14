# Image Generation w/ Stable Diffusion

{% embed url="https://www.loom.com/share/9985936764164ed2b6977b7f55060c69?sid=505db4a9-c4c0-4f59-baab-1ec0f5fd650a" %}

This tutorial shows you how to run an **arbitrary number of parallel Stable Diffusion inference tasks on ByteNite**—generating multiple images from the same prompt, all at once.

We’ll use **PyTorch** and **diffusers** ([HuggingFace](https://huggingface.co/docs/diffusers/main/en/index)) to set up a Stable Diffusion pipeline, specifically from `runwayml/stable-diffusion-v1-5` . The jobs will run on 16-core CPUs with 32 GB RAM.

{% hint style="info" %}
⚡ **Note:**\
GPU support is coming soon. For now, you can comfortably achieve similar performance by setting 16 or 24 vCPUs in the `device_requirements` field.
{% endhint %}

We’ll use a simple **replicator Partitioner**, which fans out a number of tasks based on a parameter set during job creation.

| Duration | Difficulty                                | Prerequisites                                                                                                                                                                                                                                                                |
| -------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \~45 min | <mark style="color:yellow;">Medium</mark> | <ul class="contains-task-list"><li><input type="checkbox" checked><a data-mention href="../../getting-started/onboarding.md">onboarding.md</a></li><li><input type="checkbox" checked><a data-mention href="../../sdk/bytenite-dev-cli.md">bytenite-dev-cli.md</a></li></ul> |



<figure><img src="../../.gitbook/assets/img-gen-diffusers Tutorial.webp" alt=""><figcaption></figcaption></figure>

***

## Part 1: App Setup

{% stepper %}
{% step %}
### Initialize sample app

First, create a new app directory locally by running:

```bash
bytenite app new img-gen-diffusers
```

This will generate a folder named img-gen-diffusers.

***
{% endstep %}

{% step %}
### Use a ready-made container for diffusers and torch

We’ve prepared a public container image for this tutorial. It includes all required dependencies—like torch and diffusers—bundled into a single build for **amd64**.

You can check it out here: 🔗 [ByteNite Docker Hub - python-torch-diffusers](https://hub.docker.com/repository/docker/bytenite/python-torch-diffusers)

{% hint style="info" %}
**💡 Note:**\
For this tutorial, you don't need to download the container. Want to personalize it?\
Feel free to create your own container image to include additional libraries or specific environment tweaks!
{% endhint %}

***

Use this container image in your `manifest.json`, setting it under `platform_config` -> `container`.

Since Stable Diffusion is resource-intensive, set your minimum resource requirements:

* `min_cpu`: `16`
* `min_memory`: `32`

{% code title="img-gen-diffusers/manifest.json" %}
```json
{
  "name": "img-gen-diffusers",
  "version": "0.1",
  "platform": "docker",
  "description": "My first Stable Diffusion app on ByteNite",
  "entrypoint": "main.py",
  "platform_config": {
    "container": "bytenite/python-torch-diffusers:latest",
    "private_image": false
  },
  "device_requirements": {
    "min_cpu": 16,
    "min_memory": 32
  }
}
```
{% endcode %}

***
{% endstep %}

{% step %}
### Write a 'generate\_image' function and main script

Go into your `./img-gen-diffusers/app/main.py` file and set up the image generation logic.

We’ll start by importing a few libraries:

* `json`, `os`, `time`, `psutil` — for system utilities.
* `torch`, `diffusers` — for the image generation task.

***

Next, we’ll wrap the core logic inside a function named `generate_image`, which:

* Accepts a **prompt** and an **output path** as inputs.
* Saves the generated image to the output path.

***

In the **entry point**, you will:

* Load parameters from the `APP_PARAMS` environment variable.
* Build the output path using the `TASK_RESULTS_DIR` variable (where ByteNite expects your result to be saved).
* Call the `generate_image` function to process the input and write the output.

***

The `main.py` code, including the `generate_image` function, looks like this:

{% code title="img-gen-diffusers/app/main.py" lineNumbers="true" fullWidth="true" %}
```python
# === BYTENITE APP - MAIN SCRIPT ===

import json
import os
import torch
from diffusers import StableDiffusionPipeline
import time
import psutil 

task_dir = os.getenv('TASK_DIR')
task_results_dir = os.getenv('TASK_RESULTS_DIR')
app_params = json.loads(os.getenv('APP_PARAMS'))

def generate_image(prompt, output_path):
    # Extract the prompt from params
    print(f"Generating image for prompt: {prompt}")

    # Log the number of available CPU cores
    num_threads = os.cpu_count() or 1
    print(f"Available CPU cores: {num_threads}")

    # Log current CPU utilization before inference
    print(f"Initial CPU usage: {psutil.cpu_percent(interval=1)}%")

    # Determine the appropriate data type for CPU execution
    dtype = torch.bfloat16 if torch.has_mps else torch.float32

    # Load Stable Diffusion pipeline with CPU-compatible settings
    pipeline = StableDiffusionPipeline.from_pretrained(
        "runwayml/stable-diffusion-v1-5", 
        torch_dtype=dtype
    )
    pipeline.to("cpu")

    # Enable PyTorch CPU optimizations
    torch.set_float32_matmul_precision("high")

    # Ensure PyTorch uses all available CPU cores
    torch.set_num_threads(num_threads)
    torch.set_num_interop_threads(num_threads)

    # Log the actual number of threads PyTorch is using
    print(f"PyTorch intra-op threads: {torch.get_num_threads()}")
    print(f"PyTorch inter-op threads: {torch.get_num_interop_threads()}")

    # Measure inference time
    start_time = time.time()

    # Run inference in no_grad mode
    with torch.inference_mode():
        print("Inference started")
        image = pipeline(prompt).images[0]

    end_time = time.time()
    print(f"Inference completed in {end_time - start_time:.2f} seconds")

    # Log CPU utilization after inference
    print(f"Post-inference CPU usage: {psutil.cpu_percent(interval=1)}%")

    # Save the output image
    image.save(output_path)
    
    print(f"Image saved to: {output_path}")

if __name__ == '__main__':
    print("Python task started")
    prompt = app_params["prompt"]
    output_path = os.path.join(task_results_dir, "output_image.png")
    try:
        generate_image(prompt, output_path)
    except Exception as e:
        print("Python exception: ", e)
        raise e
```
{% endcode %}

***
{% endstep %}

{% step %}
### Push and activate img-gen-diffusers

First, **remove** the `template.json` file from your folder. We’ll upload a separate template later.

***

Then, upload your app:

```bash
bytenite app push ./img-gen-diffusers
```

***

Activate your app to make it ready for jobs:

```bash
bytenite app activate img-gen-diffusers
```

***

Finally, check your app’s status:

```bash
bytenite app status img-gen-diffusers
```

***
{% endstep %}
{% endstepper %}

## Part 2: Partitioner Setup

{% stepper %}
{% step %}
### Initialize sample partitioner

Create a new engine directory locally named `fanout-replica` by running:

```bash
bytenite engine new fanout-replica
```

When prompted, **select “partitioner”** (not assembler).

***
{% endstep %}

{% step %}
### Set up Partitioner manifest

No major edits are needed here. We’ll use a simple Python image as the base container.

You can update the description as follows:

{% code title="fanout-replica/manifest-json" %}
```json
{
  "name": "fanout-replica",
  "version": "0.1",
  "type": "partitioner",
  "description": "A partitioning engine that copies the data source num_replicas times onto the chunks directory",
  "entrypoint": "main.py",
  "platform": "docker",
  "platform_config": {
    "container": "python:latest"
  }
}
```
{% endcode %}

***
{% endstep %}

{% step %}
### Write fanout-replica Partitioner code

We want to make this partitioner generate **one identical task** for each `num_replicas` parameter imported from the job request.

Since a new task is created for every output file found in the chunks directory, the partitioner needs to create `num_replicas` chunks in the chunks directory.&#x20;

{% hint style="info" %}
**💬 Note:**\
Although this partitioner is generic and copies any input file to the chunks directory, our **img-gen-diffusers app doesn’t use input data**.

However, ByteNite still requires a **one-to-one mapping** between chunks and tasks.\
To handle this, we’ll use a `bypass` data source in the job request, which provides a dummy file. This allows the partitioner to properly fan out the tasks.
{% endhint %}

***

{% code title="fanout-replica/app/main.py" lineNumbers="true" %}
```python
# === BYTENITE PARTITIONER - MAIN SCRIPT ===
try:
    import json
    import os
    import re
except ImportError as e:
    raise ImportError(f"Required library is missing: {e}")

# Path to the folder where your data lives. This folder will be automatically populated with the files imported from your data source.
source_dir = os.getenv("SOURCE_DIR")
if source_dir is None:
    raise ValueError("Environment variable 'SOURCE_DIR' is not set.")
if not os.path.isdir(source_dir):
    raise ValueError(f"Source directory '{source_dir}' does not exist or is not accessible.")

chunks_dir = os.getenv("CHUNKS_DIR")
if not chunks_dir:
    raise ValueError("Environment variable 'CHUNKS_DIR' is not set.")
if not os.path.isdir(chunks_dir):
    raise ValueError(f"Chunks directory '{chunks_dir}' does not exist or is not a directory.")
if not os.access(chunks_dir, os.W_OK):
    raise ValueError(f"Chunks directory '{chunks_dir}' is not writable.")
# Define the naming convention for chunks
chunk_file_naming = "data_{chunk_index}.bin"

# The partitioner parameters as imported from the job body in "params" -> "partitioner".
try:
    partitioner_params = os.getenv("PARTITIONER_PARAMS")
    if not partitioner_params:
        raise ValueError("Environment variable 'PARTITIONER_PARAMS' is not set.")
    params = json.loads(partitioner_params)
except json.JSONDecodeError:
    raise ValueError("Environment variable 'PARTITIONER_PARAMS' contains invalid JSON.")


# === Utility Functions ===

def list_source_files():
    """Lists all files in the source directory."""
    try:
        return [
            f for f in os.listdir(source_dir)
            if os.path.isfile(os.path.join(source_dir, f))
        ]
    except OSError as e:
        raise RuntimeError(f"Error accessing source directory '{source_dir}': {e}")

def write_chunk(data):
    """Writes a chunk of data to the next available file based on the naming convention."""
    # Use a regex pattern derived from the chunk_file_naming variable
    chunk_pattern = re.compile(re.escape(chunk_file_naming).replace(r"\{chunk_index\}", r"(\d+)"))
    
    # Determine the next chunk index
    existing_files = (
        f for f in os.listdir(chunks_dir)
        if os.path.isfile(os.path.join(chunks_dir, f)) and chunk_pattern.match(f)
    )
    chunk_indices = []
    for f in existing_files:
        match = chunk_pattern.match(f)
        if match:
            chunk_indices.append(int(match.group(1)))
    try:
        chunk_indices = sorted(chunk_indices)
        next_chunk_index = chunk_indices[-1] + 1 if chunk_indices else 0
        output_path = os.path.join(chunks_dir, chunk_file_naming.format(chunk_index=next_chunk_index))
        with open(output_path, "wb") as outfile:
            outfile.write(data)
        print(f"Chunk {next_chunk_index} written to {output_path}")
    except (IOError, OSError) as e:
        raise RuntimeError(f"Failed to write chunk {next_chunk_index} to {output_path}: {e}")


# === Main Logic ===

if __name__ == "__main__":
    print("Partitioner task started")

    source_files = list_source_files()
    nfiles = len(source_files)
    num_replicas = params["num_replicas"]

    for file in source_files:
        source_path = os.path.join(source_dir, file)

        # Copy the source file to the output path num_replicas times.
        with open(source_path, "rb") as infile:
            for i in range(num_replicas):
                # Reset the file pointer to the beginning of the file
                infile.seek(0)
                # Write the chunk to the output directory
                write_chunk(infile.read())
```
{% endcode %}

***
{% endstep %}

{% step %}
### Push and activate fanout-replica

First, upload your engine:

```bash
bytenite engine push ./fanout-replica
```

***

Then, activate the engine to make it ready for jobs:

```bash
bytenite engine activate fanout-replica
```

***

Finally, verify your engine’s details and status:

```bash
bytenite engine status fanout-replica
```

***
{% endstep %}
{% endstepper %}



## Part 3: Job Launch

{% stepper %}
{% step %}
### Create a template for your job

Now that both `img-gen-diffusers` and `fanout-replica` are ready, it’s time to connect them with a template.

In this case, we don’t need an assembler, because we’ll store all output images directly into a bucket.

So, we’ll specify `"passthrough"` as the assembler.

{% code title="template.json" %}
```json
{
  "id": "img-gen-diffusers-template",
  "description": "A template for img-gen-diffusers-template",
  "app": "img-gen-diffusers",
  "partitioner": "fanout-replica",
  "assembler": "passthrough"
}
```
{% endcode %}

***

Now, upload your template:

```bash
bytenite template push ./template.json
```

***
{% endstep %}

{% step %}
### Launch a job with img-gen-diffusers-template

Let’s make some inference magic happen! 🚀

You can **set up, launch, and retrieve your distributed image generation job** easily with this ready-made Postman collection:

{% embed url="https://www.postman.com/bytenite-team/workspace/bytenite-api-demos/collection/36285584-ecd38b83-0879-4d97-a9f6-e700db1ef305?action=share&active-environment=36285584-29ed148e-d761-4e24-9d58-0175af333612&creator=36285584" %}

***

This Postman collection automates a job workflow with these steps:

1. **Get Access Token**: Obtain an access token using your API key.
2. **Create Image Generation Job**: Send a **POST** request to the Job [create.md](../../api-reference/customer-api/jobs/create.md "mention") endpoint, including these fields in the request body:

<table><thead><tr><th width="174.77734375">Key</th><th>Value</th></tr></thead><tbody><tr><td><code>templateId</code></td><td><code>img-gen-diffusers-template</code></td></tr><tr><td><code>dataSource</code></td><td><pre class="language-json"><code class="lang-json">{
<strong>    "dataSourceDescriptor": "bypass"
</strong>}
</code></pre></td></tr><tr><td><code>dataDestination</code></td><td><pre class="language-json"><code class="lang-json">{
    "dataSourceDescriptor": "bucket"
}
</code></pre></td></tr><tr><td><code>params</code></td><td><pre class="language-json" data-overflow="wrap"><code class="lang-json">{
    "partitioner": {
        "num_replicas": 5 // change with desired number of replicas
    },
    "app": {
        "prompt": "A peaceful sunset over the ocean, in a photorealistic style, with rich detail and vibrant lighting." // change with your desired prompt
    }
}
</code></pre></td></tr></tbody></table>

Quick notes about the request fields:

*   **Bypass data source**:

    Since the app doesn’t need real input data, we use `bypass` to generate a dummy file for the partitioner.
*   **Customizing parameters**:

    Adjust the `params` to:

    * Set your number of replicas (`num_replicas`).
    * Write your own prompt under `app.prompt` to generate different images.



3. **Launch Job**: Initiate the job.
4. **Check Job Status**: Monitor the job status.
5. **Get Job Results**: Access the job results.
6. **Get Job Logs**: Review the job logs.

***
{% endstep %}
{% endstepper %}
