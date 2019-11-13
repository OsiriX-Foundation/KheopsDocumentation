---
layout: default
title: Report Providers
nav_order: 6
permalink: /docs/report_providers
---

# Report Providers
{: .no_toc }

Report Providers are third-parties that provide services for viualisation and analysis of DICOM data. Report Providers are also able to upload any resulting data back to Kheops.

The [Report Provider API](https://github.com/OsiriX-Foundation/KheopsAuthorization/wiki/Report-Providers-API) provides an OAuth 2.0 inspired mechanism to securely transmit the DICOM data a user has selected to a Report Provider. The Report provider will only have access to the images the user has specifically selected and will then be able to upload new series to the selected study.

A sample Report Provider is available at [https://reportprovider.kheops.online](https://reportprovider.kheops.online).

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Installing a new report provider

Click on **+New report provider** to install a new Report Provider.

![Click New Report Provider](/img/click_new_report_provider.png)

In the New Report Porvider panel requests a name that will be used in the Kheops user interface, and the Report Provider configuration URL that will have been communicated by the third-party that created the report provider.

Here we can use the demonstration report providers' URL:

```URL
https://reportprovider.kheops.online/.well-known/kheops-report-configuration
```

![New Report Provier](/img/new_report_provider.png)

Once the Report Provider has been properly configured, the report provider will display a small green checkmark to confirm that the Kheops server was able to succefully retreive the report provider's configuration.

![Configure Report Provider](/img/configured_report_provider.png)

## Using a Report Provider

Once a report provider is configured the report provider action ![Report Provider Action](/img/report_provider_action.png) becomes available. Click on the report provider action to display a pop-up of the available report provider. Click on the report provider you want to activate. You will then be redirected to the report provider's web site.

![](/img/click_report_provider_action.png)
