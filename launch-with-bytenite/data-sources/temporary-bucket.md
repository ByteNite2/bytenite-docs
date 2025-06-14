# Temporary Bucket

With ByteNite, you can save your output files to a temporary storage location on Google Cloud at no additional cost.&#x20;

This temporary bucket is intended for short-term use and is automatically managed to ensure file security and accessibility. ByteNite generates a temporary authenticated direct access URL for each output file or folder, simplifying access to your results.&#x20;

Files remain available for **48 hours** after being saved in the temporary storage, after which they are permanently deleted. This setup is perfect for workflows requiring short-term file storage without ongoing management.

The temporary storage bucket is exclusively a **data destination**, designed for saving output files from your jobs. It cannot be used as a data origin for sourcing input files.

## Temporary Bucket Data Source Object

{% hint style="info" %}
`dataSourceDescriptor`  : **`bucket`**
{% endhint %}

To set up a temporary bucket data destination, provide the following simple dataSource object in your API request (no `params` expected):

{% code title="POST /customer/jobs/{jobId}/datasource" %}
```json
{
  "dataSource": {
    "dataSourceDescriptor": "bucket",
  }
}
```
{% endcode %}
