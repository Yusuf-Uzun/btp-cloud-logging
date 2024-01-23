<!-- loiof5a7c993743c4ee79722479371b90b37 -->

# Ingest via Cloud Foundry Runtime

Following this guide allows you to benefit from default contents, such as dashboards, index patterns, and retention settings.



<a name="loiof5a7c993743c4ee79722479371b90b37__section_qdq_rb4_xyb"/>

## Ship Logs from a Cloud Foundry Application

> ### Note:  
> Even without any specific application logs, you can analyze your applications based on the automatically issued request logs from the Cloud Foundry router.

> ### Note:  
> You can [Ingest via OpenTelemetry API Endpoint](ingest-via-opentelemetry-api-endpoint-fdc78af.md). There are no predefined dashboards yet, but you can use the observability plugin by OpenSearch Dashboards.

Ship logs from applications deployed on SAP BTP Cloud Foundry by binding the application. Bind applications either using the SAP BTP Cockpit or the Cloud Foundry Command Line Interface \(CLI\).



<a name="loiof5a7c993743c4ee79722479371b90b37__section_fbf_yc4_xyb"/>

## Indirection via Service Key and User-Provided Service

> ### Note:  
> If you delete the service key, the certificates and credentials are invalidated.

> ### Note:  
> Skip this step and bind to your application directly if you are sending with certificates \(as it manages certificate rotation for you\).

Bind the application directly to an SAP Cloud Logging instance. However, to be resilient against issues during the binding process \(important for automated builds\), we recommend an indirection via service key and binding to a [user-provided service](https://docs.cloudfoundry.org/devguide/services/user-provided.html). Cloud Foundry operations can lead to an implicit rebind, without the need for a rebind. Using service keys provides control over the credential lifecycle.



### Using the Cloud Foundry Command Line Interface

1.  `cf services` lists the service instance.
2.  To create a service key without binding to any application via `cf cli`, execute the following command:

    ```
    cf create-service-key <service-instance> <service-key>
    ```

    > ### Note:  
    > SAP Cloud Logging needs no configuration parameters during service key creation.

3.  The service key holds all the credentials. To view a service key, execute:

    ```
    cf service-key <service-instance> <service-key>
    
    ```

    and extract ingest-endpoint, ingest-username, and ingest-password.

4.  Create a user-provided service using the following pattern:

    ```
    cf cups <user-provided-service-name> -l https://ingest-username:ingest-password@ingest-endpoint/cfsyslog
    
    ```




### Using the SAP BTP Cockpit

1.  Create a service key according to [Creating Service Keys in Cloud Foundry](https://help.sap.com/viewer/09cc82baadc542a688176dce601398de/Cloud/en-US/6fcac08409db4b0f9ad55a6acd4d31c5.html).
2.  Create a User-Provided Service following [Creating User-Provided Service Instances in Cloud Foundry Environment](https://help.sap.com/docs/service-manager/sap-service-manager/creating-user-provided-service-instances-in-cloud-foundry-environment), using the information from the service key

Instance Name:`<user-provided-service-name>`

System Logs Drain URL: `https://ingest-username:ingest-password@ingest-endpoint/cfsyslog`

> ### Note:  
> SAP Cloud Logging needs no configuration parameters during service key creation.



<a name="loiof5a7c993743c4ee79722479371b90b37__section_ewx_pg4_xyb"/>

## Bind the Application to the Service Instance



### Bind the Application Using the Cloud Foundry Command Line Interface

To bind the application using CF CLI, execute the following command:

```
cf bind-service <app-name> <service-instance>
```

> ### Note:  
> CF CLI asks you to restage, but this isn't mandatory to use SAP Cloud Logging.



### Bind the Application Using the SAP BTP Cockpit

You can bind service instances to applications both at the application view, and at the service-instance view in the cockpit.

1.  [Log On to the Cloud Foundry](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/350356d1dc314d3199dca15bd2ab9b0e.html) environment using the SAP BTP Cockpit.
2.  Navigate to the space in which your application is deployed. For more information, see [Navigate to Global Accounts, Subaccounts, Orgs, and Spaces in the Cockpit](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/5bf87353bf994819b8803e5910d8450f.html).
3.  In the navigation area, choose *Services* \> *Service Marketplace*.
4.  Search for SAP Cloud Logging.
5.  In the navigation area, choose `Instances`.
6.  To create a new instance, choose `New Instance`. In the following steps, you assign an application to this service. This application then writes its logs to the newly created service instance.
    1.  Choose the service plan, then choose `Next`.
    2.  **Optional**. Browse for the .json file of the app for which you want to write logs. Then choose `Next`.
    3.  **Optional**. Choose an application from the dropdown that lists all deployed applications. Then choose `Next`.
    4.  Enter the name of the new instance, then choose `Finish`.

7.  **Optional**. If you haven't bound an application to the service instance in the optional steps above, you can bind it from the application’s dashboard. For more information, see [Bind Service Instances to Applications Using the Cockpit](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/2d2a3e8b2f1348ffbb54eaae10d80b95.html).



<a name="loiof5a7c993743c4ee79722479371b90b37__section_gfw_fk4_xyb"/>

## Share Service Instance Across Different Spaces

You can share a single service instance across multiple spaces. Skip this step if you don't need to share a single instance across multiple spaces of the same org. To share the services in an additional space, execute the following command:

```
cf share-service <service-instance> -s <other-space>
```



<a name="loiof5a7c993743c4ee79722479371b90b37__section_gvg_4k4_xyb"/>

## Include Logging Libraries

> ### Caution:  
> Ensure that you consider the [SAP BTP Security Recommendation BTP-CLS-0002](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0002&version=Cloud).

We recommend using one of the Cloud Foundry open-source logging libraries \([Java](https://github.com/SAP/cf-java-logging-support)/[NodeJS](https://github.com/SAP/cf-nodejs-logging-support)\) within your application.

