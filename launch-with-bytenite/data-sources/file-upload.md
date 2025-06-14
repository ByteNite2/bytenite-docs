# File Upload

Uploading input files directly from your computer makes it convenient to process local content for tests.

We require a 4-step process for uploading local files, which involves configuring a data source, retrieving a temporary upload URL, uploading the file, and notifying the server upon completion.

{% stepper %}
{% step %}
### Set Local File Data Source Object

{% hint style="info" %}
`dataSourceDescriptor`: **`file`**

`@type`:**`type.googleapis.com/bytenite.data_source.LocalFileDataSource`**
{% endhint %}

When setting up a data source, use the following `dataSource` object to begin the local upload workflow:

{% code title="POST /customer/jobs/{jobId}/datasource" %}
```json
{
  "dataSource": {
    "dataSourceDescriptor": "file",
    "params": {
      "@type": "type.googleapis.com/bytenite.data_source.LocalFileDataSource"
    }
  }
}
```
{% endcode %}
{% endstep %}

{% step %}
### Retrieve a Temporary URL

After submitting the data source, fetch the generated temporary URL from the job's API response:

```python
temp_url = job_response.json()['job']['dataSource']['params']['tempUrl']
```
{% endstep %}

{% step %}
### Upload a File&#x20;

Upload your local file to the temp URL previously fetched using a PUT request:

```python
my_file = '/local/path/to/my/file.obj'

with open(my_file, mode='rb') as f:
    requests.put(temp_url, data=f, headers={'Content-Disposition': 'attachment'})
```
{% endstep %}

{% step %}
### Notify Server of Successful Upload

Let the server know that your upload is ready via the "Upload Completed" endpoint.

```python
response = requests.post(f'http://api.bytenite.com/v1/customer/jobs/uploadcompleted/{job_id}',
                                  json={}, headers={'Authorization': access_token})
```

This notification ends the workflow, linking the uploaded file to your job.
{% endstep %}
{% endstepper %}
