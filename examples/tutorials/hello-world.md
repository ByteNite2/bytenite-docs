# 👋 Hello, World!

The goal of this tutorial is to build a simple ByteNite app that outputs the string "Hello, World!" to a file. Use this tutorial if you're new to ByteNite and want to have a quick sense of the workflow.

| Duration | Difficulty                                  | Prerequisites                                                                                                                                                                                                                                                                |
| -------- | ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \~15 min | <mark style="color:green;">Very Easy</mark> | <ul class="contains-task-list"><li><input type="checkbox" checked><a data-mention href="../../getting-started/onboarding.md">onboarding.md</a></li><li><input type="checkbox" checked><a data-mention href="../../sdk/bytenite-dev-cli.md">bytenite-dev-cli.md</a></li></ul> |



***

{% stepper %}
{% step %}
### Download sample app

Run the `app new` command in your terminal to create an app directory locally named "hello-world":

```bash
bytenite app new hello-world
```

Check that a new directory named "hello-world" was indeed created at your base path:

```
ls
```
{% endstep %}

{% step %}
### Write the main script

Locate your pre-generated Python entry point  (`./hello-world/app/main.py)` and add the code to the file.

We'll make the app perform a few simple steps:

* Read a string from the input text file (chunk).
* Convert the string to a case matching the input parameter 'case'.
* Write the result to a text file in the task results directory.

{% code title="main.py" fullWidth="true" %}
```python
if __name__ == '__main__':
    print("Python task started")
    result_path = os.path.join(task_results_dir, 'processed_chunk.txt')
    try:
         # 1. Reading Inputs
    
        # Expect a text file to be passed by the partitioner
        with open(chunk_path, 'r', encoding="utf-8") as infile:
            my_string = infile.read()

        # 2. Handling Parameters
        
        # Expect the job parameters to have a key named 'case', and the options to be "upper", "lower", and "title"
        case = app_params['case']

        # 3. Developing the Core Functionality
        
        # Process the string based on the case parameter
        if case == "upper":
            my_string = my_string.upper()
        elif case == "lower":
            my_string = my_string.lower()
        elif case == "title":
            my_string = my_string.title()
        else:
            my_string = my_string
        
        # 4. Saving Outputs

        # Save the string directly into a text file to the default task results directory
        with open(os.path.join(task_results_dir, "hello_world_processed.txt"), 'w', encoding="utf-8") as outfile:
            outfile.write(my_string)

    except Exception as e:
        print("Python exception: ", e)
        raise e

```
{% endcode %}
{% endstep %}

{% step %}
### Check your manifest and template files

The manifest and template files are automatically generated and located in your `hello-world` directory. Ensure they contain the correct configurations as shown below.&#x20;

Note: We're using passthrough partitioning and assembling engines in this template, so there's no need to configure these components.

{% code title="manifest.json" %}
```json
{
  "name": "hello-world",
  "version": "0.1",
  "platform": "docker",
  "description": "An app named hello-world",
  "entrypoint": "main.py",
  "platform_config": {
    "container": "python:latest"
  },
  "device_requirements": {
    "min_cpu": 2,
    "min_memory": 2
  }
}
```
{% endcode %}

{% code title="template.json" %}
```json
{
  "id": "hello-world-template",
  "description": "A template for hello-world",
  "app": "hello-world",
  "partitioner": "passthrough",
  "assembler": "passthrough"
}
```
{% endcode %}
{% endstep %}

{% step %}
### Submit and activate your app

Upload the content of your app:

```bash
bytenite app push hello-world
```

Activate your app to make it accept jobs:

```
bytenite app activate hello-world
```

Now, check your app's details and status by running the command:

```
bytenite app get hello-world
```

Finally, ensure that your template was correctly uploaded—you will need that for running jobs.&#x20;

```
bytenite template get hello-world-template
```


{% endstep %}

{% step %}
### Launch a job with 'hello-world-template'

Let's launch a job with your new `hello-world-template`  and test your app.

Send a POST request to the Job [create.md](../../api-reference/customer-api/jobs/create.md "mention") endpoint, including the following fields in the request body:

<table><thead><tr><th width="174.77734375">Key</th><th>Value</th></tr></thead><tbody><tr><td><code>templateId</code></td><td><code>hello-world-template</code></td></tr><tr><td><code>dataSource</code></td><td><pre class="language-json"><code class="lang-json">{
<strong>    "dataSourceDescriptor": "url",
</strong>    "params": {
        "@type": "type.googleapis.com/bytenite.data_source.HttpDataSource",
        "url": "https://storage.googleapis.com/video-test-public/hello-world-I.txt"
    }
}
</code></pre></td></tr><tr><td><code>dataDestination</code></td><td><pre class="language-json"><code class="lang-json">{
    "dataSourceDescriptor": "bucket"
}
</code></pre></td></tr><tr><td><code>params</code></td><td><pre class="language-json"><code class="lang-json">{
    "app": {
        "case": "upper"
    }
}
</code></pre></td></tr></tbody></table>

Then, launch your job and check the results.

Here's a Postman collection that you can use to run your Hello World job:

{% embed url="https://www.postman.com/bytenite-team/workspace/bytenite-api-demos/collection/36285584-17ffcb17-3596-4425-89a1-21b85b238a1e?action=share&active-environment=36285584-29ed148e-d761-4e24-9d58-0175af333612&creator=36285584" %}
{% endstep %}
{% endstepper %}
