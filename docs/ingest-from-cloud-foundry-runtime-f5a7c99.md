<!-- loiof5a7c993743c4ee79722479371b90b37 -->

# Ingest from Cloud Foundry Runtime

Shipping logs from applications deployed on SAP BTP Cloud Foundry can be configured in different ways. Even without any specific application logs, you can analyze your applications based on the automatically issued request logs from the Cloud Foundry router. Further, you can benefit from default contents, such as dashboards, index patterns, and retention settings.



> ### Note:  
> You can also [Ingest via OpenTelemetry API Endpoint](ingest-via-opentelemetry-api-endpoint-fdc78af.md).

> ### Caution:  
> Ensure that you consider [SAP BTP Security Recommendation BTP-CLS-0002](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0002).

All configuration steps can be done either using SAP BTP Cockpit or the Cloud Foundry Command Line Interface.



<a name="loiof5a7c993743c4ee79722479371b90b37__section_fbf_yc4_xyb"/>

## Procedures

Shipping logs from applications deployed on SAP BTP Cloud Foundry can be configured using one of the below options:

-   [Bind the Application to the Service Instance](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__bind_the_application), optionally using [Share Service Instance Across Different Spaces](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__share_service_instance_across_different_spaces) to consolidate observability data across spaces within one organization.
-   [Bind the Application to a User Provided Service](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__bind_the_application_to_user_provided_service).
-   We recommend you use one of the Cloud Foundry open source logging libraries \([Java](https://github.com/SAP/cf-java-logging-support)/[NodeJS](https://github.com/SAP/cf-nodejs-logging-support)\) to configure logging within your application.



### Bind the Application to the Service Instance

**Bind the Application Using the Command Line Interface**

1.  [Log On to the Cloud Foundry Environment Using the Cloud Foundry Command Line Interface](https://help.sap.com/docs/btp/sap-business-technology-platform/log-on-to-cloud-foundry-environment-using-cloud-foundry-command-line-interface).
2.  To bind the application via command line interface, execute the following command:

    ```
    cf bind-service <app-name> <service-instance>
    
    ```

    > ### Note:  
    > Although the command line interface prompts you to restage the app, the binding takes effect without restaging.

3.  Go to the [Result](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__section_gvg_4k4_xyb) section.

**Bind the Application Using the SAP BTP Cockpit**

1.  [Log On to the Cloud Foundry Environment Using the SAP BTP Cockpit](https://help.sap.com/docs/btp/sap-business-technology-platform/cloud-foundry-environment).
2.  Execute [Bind Service Instances to Applications Using the Cockpit](https://help.sap.com/docs/service-manager/sap-service-manager/binding-service-instances-to-cloud-foundry-applications) without binding parameters.
3.  Go to the [Result](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__section_gvg_4k4_xyb) section.



### Share Service Instance Across Different Spaces

You can share a single service instance across multiple spaces. Skip this step if you don't need to share a single instance across multiple spaces of the same org.

1.  [Log on to the Cloud Foundry Environment Using the Cloud Foundry Command Line Interface](https://help.sap.com/docs/btp/sap-business-technology-platform/log-on-to-cloud-foundry-environment-using-cloud-foundry-command-line-interface).
2.  To share the services in an additional space, execute the following command:

    ```
    cf share-service <service-instance> -s <other-space>
    ```

3.  Go to the [Result](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__section_gvg_4k4_xyb) section.

> ### Note:  
> Be careful when deleting service keys. Credentials are invalidated if the service key is deleted, and there is no automated mechanism to track in which user provided services instances the information of a service key is used.



### Bind the Application to a User Provided Service

[Bind the Application to the Service Instance](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__bind_the_application) is the recommended approach, because of simplicity in setup and maintenance. Introducing an indirection via service key and [binding to a user-provided service](https://docs.cloudfoundry.org/devguide/services/user-provided.html) to send logs is only advisable:

-   to reduce the dependency on bind operations in automated procedures.
-   to ingest across Cloud Foundry orgs to a single Cloud Logging instance.

**Bind the Application to User Provided Service Using the Command Line Interface**

1.  [Log on to the Cloud Foundry Environment Using the Cloud Foundry Command Line Interface](https://help.sap.com/docs/btp/sap-business-technology-platform/log-on-to-cloud-foundry-environment-using-cloud-foundry-command-line-interface).
2.  Execute the following command to list the service instances:

    ```
    cf services
    ```

3.  Execute the following command to create a service key without binding to any application:

    ```
    cf create-service-key <service-instance> <service-key>
    ```

4.  Extract `ingest-endpoint`, `ingest-username`, and `ingest-password` from the response of executing:

    ```
    cf service-key <service-instance> <service-key>
    ```

5.  Create a user provided service using the following the template filled with the values of the previous step and a user-provided-service-name of your choice:

    ```
    cf cups <user-provided-service-name> -l https://<ingest-username>:<ingest-password>@<ingest-endpoint>/cfsyslog
    ```

6.  Proceed with [Bind the Application to the Service Instance](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__bind_the_application) and bind to the user provided service.

**Bind the Application to User Provided Service Using SAP BTP Cockpit**

1.  [Log On to the Cloud Foundry Environment Using the SAP BTP Cockpit](https://help.sap.com/docs/btp/sap-business-technology-platform/cloud-foundry-environment).
2.  Create a service key according to [Creating Service Keys in Cloud Foundry](https://help.sap.com/viewer/09cc82baadc542a688176dce601398de/Cloud/en-US/6fcac08409db4b0f9ad55a6acd4d31c5.html).
3.  Create a User-Provided Service following [Creating User-Provided Service Instances in Cloud Foundry Environment](https://help.sap.com/docs/service-manager/sap-service-manager/creating-user-provided-service-instances-in-cloud-foundry-environment) using `Instance Name` of your choice and the information from the the service key to configure `System Logs Drain URL`:

    ```
    https://<ingest-username>:<ingest-password>@<ingest-endpoint>/cfsyslog
    ```

4.  Proceed with [Bind the Application to the Service Instance](ingest-from-cloud-foundry-runtime-f5a7c99.md#loiof5a7c993743c4ee79722479371b90b37__bind_the_application) and bind to the user provided service.



<a name="loiof5a7c993743c4ee79722479371b90b37__section_gvg_4k4_xyb"/>

## Result

The ingested data can be analyzed in OpenSearch Dashboards \(see [Access and Analyze Observability Data](access-and-analyze-observability-data-dad5b01.md)\) based on the `logs-cfsyslog-*` index pattern.

