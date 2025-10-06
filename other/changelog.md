---
description: Track updates, improvements, and fixes across each release of the platform.
icon: arrows-rotate-reverse
---

# Changelog

## Release Notes – October 6, 2025

**✨ New Features**

* Added support for secret type “other”, expanding secret management capabilities.
* Enabled automatic injection of secrets into apps and engines, allowing access through environment variables.



***

## Release Notes – September 19, 2025

**✨ New Features**

* Introduced entity delete APIs for streamlined data management.
* Added Server-Sent Events (SSE) for real-time task status updates in the backend.

**⚡ Improvements**

* Renamed secrets and dataSourceDescriptor references from S3 to AWS for consistency across services.



***

## Release Notes – September 12, 2025

**⚡ Improvements**

* Updated backend API routing from v1/customer/ → v1/computing/, preparing for the deprecation of older endpoints.
* Populated the username field for existing user records to improve identity consistency.

**🐞 Fixes**

* Resolved issues related to template and engine import operations.

**🗑️ Deprecations**

* Marked the /customer/stats endpoint as deprecated in favor of updated computing metrics APIs.



***

## Release Notes – September 3, 2025

**✨ New Features**

Implemented a shared cache directory on GCS for worker VMs, allowing apps to access cached data via predefined environment variables.



***

## Release Notes – August 29, 2025

**✨ New Features**

* Introduced APIs/CLI commands for publishing and unpublishing apps, engines, and templates
* Added public entity import functionality through both API and CLI commands
* Added new API endpoints for browsing and uploading cache
* Added support for username creation

**⚡ Improvements**

* Improved task assignment to ensure correct distribution across CPU and GPU devices

**🐞 Fixes**

* Fixed issues with validating data sources and destinations in templates

**🗑️ Deprecations**

* Removed support for stale tasks
* Deprecated job presets and schemas



***

## Release Notes – August 21, 2025

**✨ New Features**

* Added onboarding prompt displayed on first login to the computing UI platform

**⚡ Improvements**

* Restructured jobs and templates table
* Enabled job creation from either a template or from scratch
