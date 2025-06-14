---
title: Download and Set Up ByteNit...
---

{% stepper %}
{% step %}
## Download and Set Up ByteNite Dev CLI

Follow the [bytenite-dev-cli.md](../../sdk/bytenite-dev-cli.md "mention") guide to get started with ByteNite's development tools.
{% endstep %}

{% step %}
## Initialize Sample App Directory

The Dev CLI includes a command to quickly set up your app directory using a sample, allowing you to start coding and get your app ready faster.

```bash
bytenite app new [app_name]
```

Using this command creates a new app directory at your current local path. Note that this command does not register your app in our system; it only generates a sample folder and files on your local computer.
{% endstep %}

{% step %}
## Write Your Code and Configuration Files

For this step, please refer to our dedicated guide [apps.md](../../create-with-bytenite/building-blocks/apps.md "mention"). This guide offers comprehensive documentation of the functions and processes for building an app on ByteNite.&#x20;
{% endstep %}

{% step %}
## Upload App to ByteNite

When you're done coding, submit your app to our system using the following command:

```bash
bytenite app push [app_folder]
```

Checks are performed to confirm that all the information has been provided in the correct format, and an error message is displayed otherwise.

Ensure you're controlling the version of the app which is being uploaded through the manifest.
{% endstep %}

{% step %}
## Activate Your App

Run the following command to activate an app:

```bash
bytenite app activate [app_tag]
```

To query your app status, run this command:

```bash
bytenite app status [app_tag]
```


{% endstep %}
{% endstepper %}
