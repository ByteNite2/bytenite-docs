---
description: >-
  Explore ByteNite Docs to learn how to build, deploy, and run distributed
  apps—from quick scripts to complex pipelines—at cloud scale.
icon: hand-wave
---

# Introduction

{% embed url="https://youtu.be/Jf9UYfrxbAo" %}

ByteNite’s serverless container platform is built for high-performance teams and developers who need fast startup times, flexible compute, and a simpler path to getting things done—without the headaches of traditional cloud infrastructure.

But we don’t just host containers. We’ve designed a distributed execution fabric that **eliminates cold starts**, **streamlines app architecture**, and gives you **full control over your container environments**—without the overhead of managing infrastructure.

With ByteNite, you can focus on building your app and leave the heavy lifting to us:

* Write your core application and fan-out/fan-in logic in the **programming language you know best**.
* Package your dependencies using any public or private **Docker container image**.
* Define environments and hardware specs with a **lightweight manifest file**.
* Submit jobs that are automatically partitioned, scheduled, and executed across pre-warmed cloud runners—powered by our proprietary distributed execution fabric.\


Ready to dive in? This documentation site covers everything you need, from getting started to scaling complex workloads.



## Documentation overview

Our docs are here to guide you—from first steps to advanced workloads. Whether you’re exploring tutorials, learning about core system components, or diving into API references, you’ll find everything you need to build, manage, and scale apps on ByteNite.

{% hint style="info" %}
**Just checking things out?**&#x20;

If you’re unsure whether ByteNite is the right fit for you or your team, start with our [how-it-works.md](getting-started/how-it-works.md "mention") guide or read our tutorials—no account needed!
{% endhint %}

### 1. Understanding the workflow

We’ve broken down distributed computing into a few key components to make building and managing apps simpler. To get started, check out our [how-it-works.md](getting-started/how-it-works.md "mention") guide, where you’ll dive into the typical processing job's lifecycle and get an overview of each stage, learning example use cases across different data types.

Here’s a quick rundown of a typical setup:

* **Partitioner** – Pre-processes and splits your input data into manageable chunks.
* **App** – Runs the core logic and handles the heavy lifting of your workload.
* **Assembler** – Merges or cleans up the output once processing is complete.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Get a step-by-step overview of <strong>ByteNite’s distributed job lifecycle</strong> and see how your workloads run at scale.</td><td><a href=".gitbook/assets/how-it-works-cover.webp">how-it-works-cover.webp</a></td><td><a href="getting-started/how-it-works.md">how-it-works.md</a></td></tr></tbody></table>

### 2. Checking out tutorials

If you’re new to ByteNite, a great place to start is with our tutorials. They walk you through real examples, so you can get hands-on experience and see how everything fits together.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Get started with <strong>code examples</strong>. Read, learn, replicate.</td><td><a href=".gitbook/assets/tutorials-cover.webp">tutorials-cover.webp</a></td><td><a href="examples/tutorials/">tutorials</a></td></tr></tbody></table>

While it’s tempting to jump straight into building your own applications, we recommend browsing the full system guides first. They’ll help you understand the bigger picture and the components that power ByteNite.

### 3. Taking the next steps: build and launch your apps

Once you’re comfortable with how ByteNite works, the next step is diving into the guides that help you **build, launch, and manage your apps** effectively.

You’ll learn how to structure your workloads using ByteNite’s modular components, connect your data sources, and run distributed jobs. After that, you’ll explore the API that ties everything together—allowing you to automate job launches, manage secrets, access logs, and more.

Head over to the guides below to get the full rundown:

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Explore ByteNite's Building Blocks, our modular <strong>software components</strong> designed to run your code on our <strong>distributed infrastructure</strong>.</td><td><a href=".gitbook/assets/building-blocks-cover.webp">building-blocks-cover.webp</a></td><td><a href="create-with-bytenite/building-blocks/">building-blocks</a></td></tr><tr><td>Learn about our supported <strong>storage integrations</strong>, and connect your data source to ByteNite.</td><td><a href=".gitbook/assets/data-sources-cover.webp">data-sources-cover.webp</a></td><td><a href="launch-with-bytenite/data-sources/">data-sources</a></td></tr><tr><td>Link template, data sources, and parameters to a <strong>job API request</strong> and run your app!</td><td><a href=".gitbook/assets/jobs-cover.webp">jobs-cover.webp</a></td><td><a href="launch-with-bytenite/jobs.md">jobs.md</a></td></tr></tbody></table>





***

## Product & services

ByteNite’s serverless computing platform offers APIs and interfaces designed to make app development and workload management smoother at every stage.

As a ByteNite user, you have access to all our services, fully documented right here.

Below, you’ll find our user-facing components, grouped into:

* **UIs** – For interacting with your apps, monitoring jobs, and managing configurations.
* **APIs** – For programmatic access to launch jobs, manage data, and integrate with your workflows.

### UIs

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Learn <strong>commands</strong> that let you <strong>build and submit apps</strong> to our system.</td><td><a href=".gitbook/assets/dev-cli-cover.webp">dev-cli-cover.webp</a></td><td><a href="sdk/bytenite-dev-cli.md">bytenite-dev-cli.md</a></td></tr><tr><td><strong>Launch jobs</strong> and <strong>manage settings</strong>, including billing and usage from a handy <strong>web platform</strong>.</td><td><a href=".gitbook/assets/computing-platform-cover.webp">computing-platform-cover.webp</a></td><td><a href="gui/bytenite-computing-platform.md">bytenite-computing-platform.md</a></td></tr></tbody></table>

### APIs

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Configure and run <strong>jobs</strong>, manage <strong>data sources</strong>, <strong>templates</strong>, and <strong>schemas</strong>.</td><td><a href=".gitbook/assets/jobs-api-cover.webp">jobs-api-cover.webp</a></td><td><a href="api-reference/customer-api/">customer-api</a></td></tr><tr><td>Get <strong>credentials</strong>, store <strong>secrets</strong>, and authenticate your requests.</td><td><a href=".gitbook/assets/auth-api-cover.webp">auth-api-cover.webp</a></td><td><a href="api-reference/authentication-api/">authentication-api</a></td></tr><tr><td>List and manage your <strong>apps</strong> and <strong>data engines</strong>. Powers the Dev CLI.</td><td><a href=".gitbook/assets/dev-api-cover.webp">dev-api-cover.webp</a></td><td><a href="api-reference/developer-api/">developer-api</a></td></tr><tr><td>See your <strong>transaction history</strong>, monitor your <strong>usage</strong> and <strong>current balance</strong>.</td><td><a href=".gitbook/assets/wallet-api-cover.webp">wallet-api-cover.webp</a></td><td><a href="api-reference/wallet-api/">wallet-api</a></td></tr></tbody></table>
