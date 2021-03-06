---
title: Zipkin
description: Learn how to configure the proxies to send tracing requests to Zipkin.
weight: 10
keywords: [telemetry,tracing,zipkin,span,port forwarding]
aliases:
    - /docs/tasks/zipkin-tracing.html
---

To learn how Istio handles tracing, visit this task's [overview](../overview/).

## Before you begin

1.  To set up Istio, follow the instructions in the [Installation guide](/docs/setup/).

    Use the Helm chart with tracing enabled to set the `--set tracing.enabled=true` option and
    select the `zipkin` tracing provider with `--set tracing.provider=zipkin`.

1.  Deploy the [Bookinfo](/docs/examples/bookinfo/#deploying-the-application) sample application.

## Accessing the dashboard

1.  To setup access to the tracing dashboard, use port forwarding:

    {{< text bash >}}
    $ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=istio-ingressgateway -o jsonpath='{.items[0].metadata.name}') 15032:15032 &
    {{< /text >}}

    Open your browser to [http://localhost:15032](http://localhost:15032).

1.  To use a Kubernetes ingress, specify the Helm chart option `--set tracing.ingress.enabled=true`.

## Generating traces using the Bookinfo sample

1.  When the Bookinfo application is up and running, access `http://$GATEWAY_URL/productpage` one or more times
    to generate trace information.

1.  From the top panel, select a service of interest (or 'all') from the **Service Name** drop-down list and click
    **Find Traces**:

    {{< image width="100%" ratio="52.68%"
    link="./istio-tracing-list-zipkin.png"
    caption="Tracing Dashboard"
    >}}

1.  Click on the most recent trace at the top to see the details corresponding to the
    latest request to the `/productpage`:

    {{< image width="100%" ratio="36.32%"
    link="./istio-tracing-details-zipkin.png"
    caption="Detailed Trace View"
    >}}

1.  The trace is comprised of a set of spans,
    where each span corresponds to a Bookinfo service, invoked during the execution of a `/productpage` request, or
    internal Istio component, for example: `istio-ingressgateway`, `istio-mixer`, `istio-policy`.

## Cleanup

1.  Remove any `kubectl port-forward` processes that may still be running:

    {{< text bash >}}
    $ killall kubectl
    {{< /text >}}

1.  If you are not planning to explore any follow-on tasks, refer to the
    [Bookinfo cleanup](/docs/examples/bookinfo/#cleanup) instructions
    to shutdown the application.

