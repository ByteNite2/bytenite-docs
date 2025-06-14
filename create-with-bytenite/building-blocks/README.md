---
description: Overview of ByteNite's core components and functionalities
icon: block-brick
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Building Blocks

ByteNite's building blocks are designed to help you structure your distributed workloads into modular, reusable components. These components—Partitioners, Apps, and Assemblers—let you focus on your core logic without worrying about the complexities of distributed execution, fault tolerance, retries, or logging. We've got that covered.

At a high level, ByteNite breaks down a distributed job's lifecycle into three phases:

1. **Partitioning Engine**: Handles input downloading, pre-processing, and task creation.
2. **App**: Executes the core logic for each individual task.
3. **Assembling Engine**: Collects and merges the results from individual tasks.

Each component is fully customizable and versionable, enabling you to build flexible pipelines that fit your specific needs.&#x20;

Explore the following guides to learn how to develop your own ByteNite Apps, Partitioning Engines, and Assembling Engines to create distributed workflows tailored to your needs.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Apps</strong> let you submit code to run on ByteNite's infrastructure.</td><td><a href="apps.md">apps.md</a></td><td><a href="../../.gitbook/assets/apps-card-cover.png">apps-card-cover.png</a></td></tr><tr><td>Build a <strong>Data Partitioning Engine</strong> to tell your app how to pre-process input data. </td><td><a href="partitioning-engines.md">partitioning-engines.md</a></td><td><a href="../../.gitbook/assets/partitioning-engines-card-cover.png">partitioning-engines-card-cover.png</a></td></tr><tr><td>Add a <strong>Data Assembling Engine</strong> to define chunk merging logic.</td><td><a href="assembling-engines.md">assembling-engines.md</a></td><td><a href="../../.gitbook/assets/assembling-engines-card-cover.png">assembling-engines-card-cover.png</a></td></tr><tr><td>Use <strong>Job Templates</strong> to harmonize versions across apps and data engines.</td><td><a href="job-templates.md">job-templates.md</a></td><td><a href="../../.gitbook/assets/job-templates-card-cover.png">job-templates-card-cover.png</a></td></tr></tbody></table>

