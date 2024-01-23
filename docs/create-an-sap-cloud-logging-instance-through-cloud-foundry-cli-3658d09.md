<!-- loio3658d09227dd43c2bcc7df7d774bb925 -->

# Create an SAP Cloud Logging Instance through Cloud Foundry CLI





<a name="loio3658d09227dd43c2bcc7df7d774bb925__section_w5t_wyy_kzb"/>

## Prerequisites

See [Prerequisites](prerequisites-41d8559.md).



<a name="loio3658d09227dd43c2bcc7df7d774bb925__section_ngq_1zy_kzb"/>

## Create a Service Instance

1.  `cf marketplace` displays the service in the marketplace.
2.  To create a service instance, execute the following command and provide the necessary information:
    -   `cf create-service cloud-logging <service-plan> <instance_name> -c <parameters>`.
    -   See [Service Plans](service-plans-a9d2d1b.md) and [Configuration Parameters](configuration-parameters-1830bca.md) for configuration options.
    -   Here is an example command with additional parameters:

        ```
        cf create-service cloud-logging standard cloud-logging -c '{
           "retention_period":  14,
            "backend": {
               "max_data_nodes":  10,
               "api_enabled": false
            },
            "ingest": {
               "max_instances":  10
            }
         }'
        
        ```


3.  Wait for your dedicated instance to be provisioned.
    -   Use the following command to verify the service provisioning. This checks the `last operation` status:

        ```
        cf services
        ```

    -   Wait until the last operation reads `create succeeded`. The service instance is now available for consumption.


