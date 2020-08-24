---
title: "Managing Control Planes"
date: 2020-08-21T11:20:20+01:00
menu:
  main:
    parent: "Environments & Deployments"
weight: 3
url: "/tyk-cloud/environments-&-deployments/managing-control-planes"
---

## Introduction

Control Planes are situated in your Organisation's home region and provide links to an instance of the [Tyk Dashboard](/docs/getting-started/tyk-components/dashboard/) and the [Developer Portal](/docs/getting-started/tyk-components/developer-portal/). The Dashboard is where you perform all your API tasks. The devloper portal allows your 3rd party developers access to your APIs. Edge Gateways are then connected to your Control Planes.


## Prerequisites

The following [user roles](/docs/tyk-cloud/reference-docs/user-roles/) can perform Control Plane Admin tasks:

* Org Admin
* Team Admin

## Adding a new Control Plane

{{< note success >}}
**Note**
  
The number of Control Planes you can add is dependent on your [plan](/docs/tyk-cloud/account-billing/plans/)
{{< /note >}}

1. From the Environments screen click **Add Deployment**
2. Enter a name for the new Control Plane
3. Select Control Plane from the Type drop-down list
4. Select the Bundle Channel and Version
5. (Optional) Enter a [custom domain](/docs/tyk-cloud/using-custom-domains/) if required
6. (Optional) Enable [plugins](/docs/tyk-cloud/using-plugins/) if required

## Edit Control Planes

You can edit the following Control Plane settings:
* Change the Control Plane name
* Add a [custom domain](/docs/tyk-cloud/using-custom-domains/)
* Change the Bundle Channel and Bundle Version
* Enable [plugins](/docs/tyk-cloud/using-plugins/)

{{< note success >}}
**Note**
  
The use of custom domains is dependent on your [plan](/docs/tyk-cloud/account-billing/plans/)
{{< /note >}}

To edit an existing Control Plane:

1. From the Overview screen, click the **Control Plane Name** from the list
2. Select **Edit** from the Deployed drop-down list

![Edge drop-down](/docs/img/admin/cp-edit.png)