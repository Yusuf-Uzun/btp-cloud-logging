<!-- loiofdc78af7c69246bc87315d90a061b321 -->

# Ingest via OpenTelemetry API Endpoint

You can ship OpenTelemetry data via OpenTelemetry protocol \(OTLP\) using a combined endpoint for logs, metrics, and traces. This endpoint supports the gRPC protocol, but no other protocol formats of the OTLP specification.

> ### Note:  
> Protocols like http/protobuf or http/json can be converted to gRPC using the [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/).



<a name="loiofdc78af7c69246bc87315d90a061b321__section_t1n_q1d_dzb"/>

## Procedure

OpenTelemetry support in SAP Cloud Logging needs to be enabled with a service instance configuration parameter that can be set with service instance creation or update. Once enabled, all new service bindings and service keys contain the OTLP endpoint and credentials.

1.  Enable Endpoint via Configuration Parameter.

    OpenTelemetry ingestion must be enabled explicitly via the following in the service instance configuration:

    ```
    {
        "ingest_otlp": {
            "enabled": "true"
        }
    }
    ```

    After OpenTelemetry has been enabled, the endpoint is added to the service instance.

2.  Retrieve Endpoint and Certificates.

    Once OpenTelemetry ingestion is enabled, all new service bindings and service keys contain the required endpoint and credentials for mutual TLS. SAP Cloud Logging only supports mTLS for the OTLP endpoint. The service key contains the following OTLP-related properties:

    ```
    {
      "credentials": {
        "ingest-otlp-endpoint":
            "ingest-otlp-sf-<your-service-id>.<your-cluster-url>:443",
        "ingest-otlp-cert":
            "-----BEGIN CERTIFICATE-----\n
             Your client certificate in PEM format\n
             -----END CERTIFICATE-----\n",
        "ingest-otlp-key":
            "-----BEGIN PRIVATE KEY-----\n
             Your client key in PCKS #8 format\n
            -----END PRIVATE KEY-----\n",
        "server-ca":
          "-----BEGIN CERTIFICATE-----\n
           Your instance server certificate in PEM format\n
           -----END CERTIFICATE-----\n",
      }
    }
    ```

    > ### Note:  
    > TLS certificates for client authentication are issued with a validity period of 90 days by default. Rotate the service key and update the credentials in all sender configurations, otherwise, ingestion will stop.

    > ### Caution:  
    > Ensure that you consider the [SAP BTP Security Recommendation BTP-CLS-0003](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0003&version=Cloud).

    > ### Note:  
    > Use the `certValidityDays` to configure the validity period via a service binding parameter within the range of 1 to 180 days. For example, passing `'{"ingest":{"certValidityDays":30}}` as the configuration parameter for binding creation sets the validity to 30 days.

    > ### Note:  
    > Deleting a binding doesn't revoke the corresponding certificate. [Rotate the Ingestion Root CA Certificate](rotate-the-ingestion-root-ca-certificate-bbcb3e7.md) if the root Certification Authority \(Certification Authority\) of your service instance expires soon, or the private key of a certificate was leaked.

3.  Ship OpenTelemetry data with method of choice.

    You can use the endpoint and credentials to ship OpenTelemetry signals retrieved by one of the following instrumentation options: OpenTelemetry SDK, OpenTelemetry Agent, or OpenTelemetry Collector. See [OpenTelemetry documentation on instrumentation](https://opentelemetry.io/docs/concepts/instrumentation/) for more details.




<a name="loiofdc78af7c69246bc87315d90a061b321__section_kbr_nbd_dzb"/>

## Result

You can analyze the ingested OpenTelemetry data in OpenSearch Dashboards \(see [Access and Analyze Observability Data](access-and-analyze-observability-data-dad5b01.md)\). The index names match the requirements for the default analysis features of OpenSearch Dashboards. Indices match the following patterns:

-   `logs-otel-v1-*` for logs
-   `metrics-otel-v1-*` for metrics
-   `otel-v1-apm-span-*` for traces/spans
-   `otel-v1-apm-service-map` for the service map

> ### Note:  
> Due to limitations of OpenSearch/Lucene, signal and resource attribute names have dots replaced with "@," and contain a prefix specifying the attribute type. For example, the resource attribute `service.name` is mapped to `resource.attributes.service@name`. Because of its prominent role, the service name is also available as a field `serviceName`, which reflects the open-source defaults.

