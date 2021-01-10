---
title: Installing the Logz.io Jaeger Collector for Distributed Tracing
logo:
  logofile: jaeger.svg
  orientation: vertical
data-source: Jaeger
description: How to deploy a Logz.io Jaeger Collector 
open-source:
  - title: 
    github-repo: jaeger-logzio
contributors:
  - yyyogev
  - yberlinger
  - doron-bargo
shipping-tags:
  - traces
---
## Overview

Logz.io's trace exporter for Jaeger allows you to ship distributed traces to Logz.io from different APM (Application Performance Management/Monitoring) agents, including Zipkin.

This topic explains how to install the Logz.io Jaeger Collector. <br> For an overview of the process to send traces to Logz.io, see <a href="/user-guide/distributed-tracing/getting-started-tracing" target ="_blank"> Getting started with Logz.io Distributed Tracing</a>. 

We recommend that you use the OpenTelemetry collector to gather trace transaction data from your system. <br> See _<a href ="/shipping/tracing-sources/opentelemetry" target="_blank">Ship traces with OpenTelemetry </a>_ for the procedure to configure and deploy the OpenTelemetry collector. You may consider using the Jaeger Collector as a secondary option, if you experience issues with the OpenTelemetry Collector. 

### Logz.io Jaeger Collector

The Logz.io integration builds on the Jaeger Collector base image and uses the gRPC plugin framework to enable communication between the Collector and Logz.io.

#### Deploy OpenTelemetry Collector with Logz.io Exporter

<div class="tasklist">

##### Deploy Logz.io Jaeger Collector

###### Configure the Logz.io extension
Configure the Logz.io extension with shell variables or environment variables. <br> The required ports are described 
<a href ="https://www.jaegertracing.io/docs/latest/deployment/#collectors)" target="_blank">here <i class="fas fa-external-link-alt"></i>.</a> 
You'll need to change to the version page for your deployment. 

```bash
docker run -e ACCOUNT_TOKEN=<<SHIPPING-TOKEN>>  # see parameter list below\
 --network=net-logzio \
 --name=jaeger-logzio-collector \
 -p 14268:14268 \
 -p 9411:9411 \
 -p 14267:14267 \
 -p 14269:14269 \
 -p 14250:14250 \
logzio/jaeger-logzio-collector:latest
```

The complete list of Collector parameters is presented below. In addition to these parameters, you can also use 
 <a href ="https://www.jaegertracing.io/docs/latest/cli/#jaeger-collector-grpc-plugin" target="_blank">Jaeger's collector parameters <i class="fas fa-external-link-alt"></i> </a> . You'll need to change to the version page for your deployment. 

###### Parameters

 Collector Parameter | Description
 ------------ | -------------
  ACCOUNT_TOKEN (Required) | - The account token is required when you use the collector to send traces to Logz.io <br> -  Replace `<SHIPPING-TOKEN>` with the token of the Distributed Tracing account you want to send data to <br><a href ="/user-guide/accounts/finding-your-tracing-account-token" target="_blank">How do I look up my Distributed Tracing account token?</a>
REGION | -   Two-letter region code that determines the suggested listener URL (where you will be sending trace data to)  <br>-   Find your region code in the Regions and URLs table <br>-   This parameter is left blank for US East (Northern Virginia)<br><a href ="/user-guide/accounts/account-region.html#available-regions " target="_blank">How do I look up the Listener host URL for my region?</a>
GRPC_STORAGE_PLUGIN_LOG_LEVEL| -   The lowest log level to send <br> -   Default: **warn** <br>-   From lowest to highest, log levels are: **trace, debug, info, warn, error** <br>-   Controls logging only for the Jaeger Logz.io Collector  <br>-   Does not affect Jaeger components
COLLECTOR_ZIPKIN_HTTP_PORT | If you’re using a Zipkin implementation to create traces, set this optional environment variable to the HTTP port for the Zipkin collector service