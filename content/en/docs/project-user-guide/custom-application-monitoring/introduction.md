---
title: "Introduction"
keywords: 'monitoring, prometheus, prometheus operator'
description: 'Introduction'

linkTitle: "Introduction"
weight: 2100
---

Custom monitoring allows you to monitor and visualize custom application metrics on KubeSphere. The application can be either a third-party application, such as MySQL, Redis, and Elasticsearch, or your web application. This section introduces the workflow of this feature.

## Overview

The KubeSphere monitoring engine is powered by Prometheus and Prometheus Operator. To integrate custom application metrics into KubeSphere, you need to go through the following steps in general.

### Expose Prometheus-Formatted Metrics

First of all, your application must expose Prometheus-formatted metrics. Prometheus exposition format is the de-facto format in the realm of cloud-native monitoring. Prometheus uses a [text-based exposition format](https://prometheus.io/docs/instrumenting/exposition_formats/). Depending on your application and use case, there are two ways to expose metrics:

#### Direct exposing

Directly exposing Prometheus metrics from applications is a common way among cloud-native applications. It requires developers to import Prometheus client libraries in their codes and expose metrics at a specific endpoint. Many applications, such as ETCD, CoreDNS, and Istio, adopt this method.

Prometheus community offers client libraries for most programming languages. Please find your language on [Prometheus Client Libraries](https://prometheus.io/docs/instrumenting/clientlibs/) page. For Golang developers, read [Instrumenting a Go application](https://prometheus.io/docs/guides/go-application/) to learn how to write a Prometheus-compliant application.

The [sample web application](../get-started/monitor-sample-web) is an example demonstrating how an application exposes Prometheus-formatted metrics directly. 

#### Indirect exposing
  
If you don’t want to modify your code or you cannot do so because the application is provided by a third party, you can deploy an exporter which serves as an agent to scrape metric data and translate them into Prometheus format.

For most third-party applications, such as MySQL, the Prometheus community provides production-ready exporters. Please refer to [Exporters and Integrations](https://prometheus.io/docs/instrumenting/exporters/) for available exporters. In KubeSphere, it is recommended to enable Open Pitrix and deploy exporters from the App Store. Exporter for MySQL, Elasticsearch, and Redis are builtin items on App Store. 

Please read [Monitor MySQL](../get-started/monitor-mysql) to learn how to deploy a MySQL exporter and monitor MySQL metrics.

Writing an exporter is nothing short of instrumenting an application with Prometheus client libraries. The only difference is that exporters need to connect to applications and translate application metrics into Prometheus format.

### Apply ServiceMonitor CRD 

In the previous step, you expose metric endpoints in a Kubernetes Service object. Next, you need to inform the KubeSphere monitoring engine of your new changes.

The ServiceMonitor CRD is defined by [Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator). ServiceMonitor contains information about the metrics endpoints. With ServiceMonitor objects, the KubeSphere monitoring engine knows where and how to scape metrics. For each monitoring target, you apply a ServiceMonitor object to hook your application (or exporters) up to KubeSphere.

In KubeSphere v3.0.0, you should pack ServiceMonitor with your applications (or exporters) into a helm chart for reuse. In the future release, KubeSphere will provide graphical interfaces for easy operation.

Please read [Monitor Sample Web Application](../get-started/monitor-sample-web) to learn how to pack ServiceMonitor with your application.

### Visualize Metrics

Around two minutes, the KubeSphere monitoring engine starts to scape and store metrics. Then you can use PromQL to query metrics and draw panels and dashboards. 

Please read [Querying](../visualization/querying) to learn how to write a PromQL expression. For dashboard features, please read [Visualization](../visualization/overview).