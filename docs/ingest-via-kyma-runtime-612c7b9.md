<!-- loio612c7b9e0c2644fba43295a7629855bd -->

# Ingest via Kyma Runtime

Kyma's [Telemetry](https://kyma-project.io/docs/kyma/latest/01-overview/telemetry) component supports shipping observability signals to SAP Cloud Logging instances. You can configure different observability signal types independently of each other.

> ### Caution:  
> Ensure that you consider the [SAP BTP Security Recommendation BTP-CLS-0003](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0003&version=Cloud).



<a name="loio612c7b9e0c2644fba43295a7629855bd__section_x2h_41p_xyb"/>

## Prerequisites

-   An [SAP BTP Kyma runtime](https://help.sap.com/docs/btp/sap-business-technology-platform/create-kyma-environment-instance) instance
    -   With `telemetry` [module](https://help.sap.com/docs/btp/sap-business-technology-platform/kyma-modules) [enabled](https://help.sap.com/docs/btp/sap-business-technology-platform/enable-and-disable-kyma-module).
    -   With `btp-operator` [module](https://help.sap.com/docs/btp/sap-business-technology-platform/kyma-modules) [enabled](https://help.sap.com/docs/btp/sap-business-technology-platform/enable-and-disable-kyma-module).

-   Kubernetes CLI \(kubectl\) v1.23 or higher \(see the [kubectl tutorial](https://developers.sap.com/tutorials/cp-kyma-download-cli.html)\).
-   SAP Cloud Logging instance with [Ingest via OpenTelemetry API Endpoint](ingest-via-opentelemetry-api-endpoint-fdc78af.md) enabled to ingest distributed traces. We recommend you create it with the SAP BTP Service Operator \(see [Create an SAP Cloud Logging Instance through SAP BTP Service Operator](create-an-sap-cloud-logging-instance-through-sap-btp-service-operator-f6aa131.md)\), because it takes care of creation and rotation of the required Secret.



<a name="loio612c7b9e0c2644fba43295a7629855bd__section_r2q_hbp_xyb"/>

## Procedure

To integrate Cloud Logging on SAP BTP Kyma Runtime, please follow Kyma documentation on [how to ship from Kyma to SAP Cloud Logging](https://kyma-project.io/#/telemetry-manager/user/integration/sap-cloud-logging/README).



<a name="loio612c7b9e0c2644fba43295a7629855bd__section_fdm_mcm_pzb"/>

## Results

You can analyze the ingested data in OpenSearch Dashboards \(see [Access and Analyze Observability Data](access-and-analyze-observability-data-dad5b01.md)\) based on the following index patterns:

1.  `logs-json-istio-envoy-kyma`\* for istio access logs
2.  `logs-json-kyma*` for application logs
3.  [OpenTelemetry related indices](ingest-via-opentelemetry-api-endpoint-fdc78af.md) 

