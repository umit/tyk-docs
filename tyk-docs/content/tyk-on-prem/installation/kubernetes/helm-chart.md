---
title: "Tyk Helm Chart "
date: 2021-07-01
tags: [""]
description: ""
menu:
  main:
    parent: "Kubernetes "
weight: 1
url: "/tyk-self-managed/tyk-helm-chart"
---

## Introduction

This is the preferred (and easiest) way to install Tyk Pro on Kubernetes. It will install Tyk in your Kubernetes cluster where you can add and manage APIs via the Tyk Kubernetes Operator or, as with a normal Tyk Pro Installation, via the Tyk Dashboard.
## Tyk Licensing

If you are evaluating Tyk on Kubernetes, [contact us](https://tyk.io/about/contact/) to obtain an temporary licence.

Tyk Pro Licenses allow for different numbers of Gateway nodes to connect to a single Dashboard instance. Ensure that your Gateway pods will not scale beyond your license number by setting the Gateway resource kind to Deployment and setting the replica count to your license node limit. For example, use the following options for a single node license: `--set gateway.kind=Deployment --set gateway.replicaCount=1` in your `values.yaml` file.

{{< note success >}}
**Note**  

There may be intermittent issues on the new pods during the rolling update process, when the total number of online gateway pods is more than the license limit with lower amounts of Licensed nodes.

{{< /note >}}

## Prerequisites

The following are required for a *Tyk Self-managed* demo installation:

 - Redis. Redis is required for all of the Tyk installations and must be installed in the cluster or reachable from inside K8s.
 - MongoDB. MongoDB is only required for a Tyk Pro install (with the Tyk Dashboard) and must be installed in the cluster, or reachable from inside K8s.


### Installing Redis and MongoDB

{{< warning  success >}}
**Warning**

To get started quickly, you can use `redis.yaml` and `mongo.yaml` manifests to install Redis and MongoDB inside your Kubernetes cluster. Please note that these provided manifests must never be used in production and for a quick start evaluation only. Use external DBs or Official Helm charts for MongoDB and Redis in any other case. We provide these manifests so you can quickly have Tyk up and running, however they are not meant for long term storage of data.
{{< /warning >}}

```{copy.Wrapper}
kubectl create namespace tyk
kubectl apply -f deploy/dependencies/mongo.yaml -n tyk
kubectl apply -f deploy/dependencies/redis.yaml -n tyk
```

## Installation

Firstly run the following command.

```{copy.Wrapper}
helm repo add tyk-helm https://helm.tyk.io/public/helm/charts/
helm repo update
```
Then, before you proceed with installation of the chart you need to set some custom values in your values.yaml file. Run:

```{copy.Wrapper}
helm show values tyk-helm/tyk-pro > values.yaml
```

Add your Tyk License to the `dash.license` option


Then run the following command from the root of the repository:


```{copy.Wrapper}
helm install tyk-pro tyk-helm/tyk-pro --version 0.9.0 -f values.yaml -n tyk --wait
```

Please note the `--wait` argument is important to successfully bootstrap your Tyk Dashboard.

Follow the instructions in the Notes that follow the installation to find your Tyk login credentials.

## Tyk Developer Portal
You can disable the bootstrapping of the Developer Portal by setting the `portal.bootstrap` option to `false` in your `values.yaml`.

## Using TLS

 You can turn on the TLS option under the gateway section in your `values.yaml` file which will make a Gateway listen on port 443 and load up a dummy certificate. You can set your own default certificate by replacing the file in the `certs/` folder.
## Sharding APIs

Sharding is the ability for you to decide which of your APIs are loaded on which of your Tyk Gateways. This option is turned off by default, however, you can turn it on by updating the `gateway.sharding.enabled option`. Once you do that you will need to also populate the `gateway.sharding.tags` value with the tags that you want that particular Gateway to load. (ex. tags: "external,ingress".) You can then add those tags to your APIs in the API Designer, under the **Advanced Options** tab, and the Segment Tags (Node Segmentation) section in your Tyk Dashboard. See 

## Other Tyk Components

### Tyk Identity Broker (TIB)

TIB is not necessary to install for this chart as it's functionality is included in the Tyk Dashboard API Manager. However, if you want to run it seperately from the Dashboard you can do so.

The Tyk Identity Broker (TIB) is a micro-service portal that provides a bridge between various Identity Management Systems such as LDAP, Social OAuth (e.g. GPlus, Twitter, GitHub), legacy Basic Authentication providers, to your Tyk installation. See [TIB]({{< ref "/content/tyk-stack/tyk-identity-broker/getting-started.md" >}}) for more details.

Once you have installed the Gateway and Dashboard you can configure TIB by adding its configuration environment variables under the `tib.extraEnvs` section and updating the `profile.json` in your `configs` folder. See our [TIB GitHub repo](https://github.com/TykTechnologies/tyk-identity-broker#how-to-configure-tib). Once you complete your modifications you can run the following command from the root of the repository to update your helm chart.

```{copy.Wrapper}
helm upgrade tyk-pro ./tyk-pro -n tyk
```

This chart implies there's a ConfigMap with a `profiles.json` definition in it. Please use `tib.configMap.profiles` value to set the name of this ConfigMap (tyk-tib-profiles-conf by default).

## Next Steps Tutorials

Follow the Tutorials on the Self Managed tabs for the following:

1. [Add an API](/docs/getting-started/tutorials/create-api/)
2. [Create a Security Policy](/docs/getting-started/tutorials/create-security-policy/)
3. [Create an API Key](/docs/getting-started/tutorials/create-api-key/)