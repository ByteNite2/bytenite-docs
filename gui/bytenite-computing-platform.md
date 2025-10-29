---
icon: sidebar
---

# ByteNite Computing Platform

## Introduction

The ByteNite Computing Platform GUI is a web interface that enables users to manage and monitor distributed computing jobs on ByteNite, allowing users to launch, track, and control their workloads.

***

## Job Management

### **Create New Jobs**

In order to create a new job, from the **Jobs** page, click the "New Job" button at the top. Here, you'll have two options:



* **Start from Scratch** - you'll manually configure all parameters, including:
  * **Job Details** - job name and description
  * **App Details** - deployed app references for your partitioner, app, and assembler (you may specify versioning to test specific versions of your app, partitioner and assembler, by default the most recent version of each will be used)
  * **Setup** - data source and destination types, parameter configs for your app references, and timeout and retry configs.

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FC4oKwqyUo55Lw35MjazY%2Fuploads%2F1IaDEXu75FPMHEGCjcRM%2FMOV%20to%20MP4%20test.mp4?alt=media&token=7183182b-d516-470e-9c7e-ac83b0915a8b" %}



* **Use a Template -** Alternatively, you can pick an existing template from your **Templates** table and click on it to create a new job. This will give you the option to create a job to either run later or immediately, using the preconfigured data source and destination, parameters, and configs you defined in your selected template.

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 5.09.06 PM.png" alt=""><figcaption></figcaption></figure>



### **Launch Jobs**

You can launch a new job immediately after configuration in **Start from Scratch** by clicking **Create & Run Job**, as shown in the demo video in the section above. Alternatively, clicking **Create Job** allows you to save the job as a draft. You can later execute it using job actions from the Jobs page or the job overview by selecting **Run Job**.

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 6.49.42 PM.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 6.50.00 PM.png" alt=""><figcaption></figcaption></figure>



You can additionally launch a job from a pre-saved template using **Use a Template**. Simply select the row of which template you would like to use and confirm that it is configured correctly. For more information on how to properly define your jobs, see the parameters in the **Start from Scratch** section above.&#x20;



### **Delete Jobs**

Remove jobs that are no longer needed, with confirmation prompts to prevent accidental deletion. This can be conducted through the Jobs page or through the red button on the top right of the specific job's Overview page.

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 7.43.57 PM.png" alt=""><figcaption></figcaption></figure>



### **Job Logging & Monitoring**

View real-time statuses such as _Running, Complete, and Failed_, along with progress bars for each job on the Jobs page. More detailed, time-stamped task status tracking is available on the Job's Overview page.&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Additionally, the Job Overview page includes a Logs section, which contains logs from the partitioner, app, and assembler processes. While the logs are presented comprehensively, they can be filtered for troubleshooting by categories: error, warning, info, debug, and unknown. These logs are also queryable and downloadable.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

***

## Task Management

Tasks are the individual units of work that make up a job. Each job may consist of multiple tasks, which can be executed in parallel or sequence, depending on the workload.

#### **View Task Breakdown & Progress**

Inspect the list of tasks associated with a job, breaking down their time-stamped state (_Scheduled, Assigned, Received, Running, Completed_) and price. Deeper insights are available at the individual task level, including the container stats, network stats, and device info.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

#### Task Logs

Access detailed logs for each task by clicking into that specific **Task ID** to diagnose issues at a granular level.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

***

## Templates Management

The **Templates** page allows users to observe and reuse submitted job templates for common or recurring workloads. Templates help ensure consistency, reduce setup time, and minimize errors when launching similar jobs. For info on using templates for job setup, view **Use a Template** in the [Create New Job](bytenite-computing-platform.md#create-new-jobs) section above.

If you want to delete a template, simply select the ellipses at the end of the row of the desired template and click **Delete**.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 8.03.42 PM.png" alt=""><figcaption></figcaption></figure>

***

## Security

The Security section houses both the pages for API Key and Secret Management:

### API Keys

The API Keys page allows users to securely generate and manage their personal API keys. These keys are essential credentials required to access the platform’s APIs, enabling programmatic interaction with ByteNite’s distributed computing services.

On this page, users can:

* **Create New API Keys:** Generate unique API keys for use in scripts, applications, or integrations that need to interact with the ByteNite platform outside of the web interface.
* **View and Copy Existing Keys:** Easily view your active API keys and copy them for use in your development environment or automation tools.
* **Revoke Keys:** Revoke keys that are no longer needed.

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 8.46.03 PM.png" alt=""><figcaption></figcaption></figure>

### Secrets

The Secrets page lets you manage credentials used by your apps and engines across your jobs. ByteNite currently supports secret types from AWS, GCP, and Storj. All that's needed from you prior to setting up your secret is your Access Key and your Secret Key from that respective platform.

Each secret is assigned a unique **Secret ID**, defined by you, which you can reference in your environment variable configuration to inject credentials at runtime—eliminating the need to hardcode sensitive values.

For step-by-step instructions on using Secret IDs in environment variables, see: [Environment Variable Injection](https://app.gitbook.com/o/hWTaF6n2ZDainV7RJ5Q9/s/C4oKwqyUo55Lw35MjazY/~/changes/47/create-with-bytenite/containers-and-environments#environment-variable-injection).

<figure><img src="../.gitbook/assets/Screenshot 2025-10-22 at 8.56.05 PM.png" alt=""><figcaption></figcaption></figure>

***

## Billing

The Billing page is where users manage their transactions. It includes several key sections:

#### **Usage Overview**&#x20;

This section displays ByteChip usage over a selected month. Users can adjust the view to focus on a specific month or template.

#### **Wallet Balance**&#x20;

Here, users can add ByteChips to their account via Top Up or Coupon Redemption. This section also allows users to view and copy their account ID, check both total and available balances, and see the amount due. Additionally, it includes a Customer Portal button for accessing the ByteNite Stripe integration.

#### **Wallet Transactions**&#x20;

This section lists all your transactions. It provides detailed information such as transaction type (Top Up, Coupon, Payment), description, costs in both ByteChips and USD, and the remaining balance.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

***

## Contact Us

The Contact Us page provides a simple way for users to reach the ByteNite team with questions, issues, or feedback. To submit a query, just fill out the following fields:&#x20;

* **Email:** Enter your email address so we can get back to you.
* **Summary:** Provide a brief summary of your request or concern.
* **Description:** Use this field to describe your issue or feedback in detail.



Once submitted, our team will review your message and respond as soon as possible.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

***

## Documentation

The Documentation page will redirect you to [Introduction](../) page, where you can find detailed guides, feature overviews, and best practices for using the ByteNite Computing Platform.

***

## Developer CLI&#x20;

The Developer CLI page will take you directly to the [Developer CLI](../create-with-bytenite/bytenite-dev-cli.md) page, explaining setup instructions, usage examples, and advanced command references.

{% include "../.gitbook/includes/page-under-construction.md" %}
