<!-- loio21eb1bd9abfa4f66ac564d97a2b58952 -->

# Create an SAP Cloud Logging Instance through SAP BTP CLI





<a name="loio21eb1bd9abfa4f66ac564d97a2b58952__section_qyd_j1z_kzb"/>

## Prerequisites

-   See [Prerequisites](prerequisites-41d8559.md).
-   Install BTP CLI as described in [Download and Start Using the BTP CLI Client](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/8a8f17f5fd334fb583438edbd831d506.html).
-   Ensure you are [logged in](https://help.sap.com/docs/btp/sap-business-technology-platform/log-in?version=Cloud).



<a name="loio21eb1bd9abfa4f66ac564d97a2b58952__section_czn_41z_kzb"/>

## Create a Service Instance

1.  Create a SAP Cloud Logging instance. See [Service Plans](service-plans-a9d2d1b.md) and [Configuration Parameters](configuration-parameters-1830bca.md) for configuration options:

    ```
    btp create services/instance --subaccount <SUBACCOUNT_ID> --service <CLS_INSTANCE_NAME> --offering-name "cloud-logging" --plan-name <CLS-SERVCIE-PLAN> --parameters <CLS-SERVICE-PARAMETERS>
    ```

2.  Wait for your dedicated instance to be provisioned.



<a name="loio21eb1bd9abfa4f66ac564d97a2b58952__section_kph_jbz_kzb"/>

## Create a Service Binding

Get a service key to access instance credentials.

1.  Create a service binding.

    ```
    btp create services/binding --subaccount <SUBACCOUNT_ID> --binding <CLS_BINDING_NAME> --instance-name <CLS_INSTANCE_NAME>
    ```

2.  Get a binding to access instance credentials.

    ```
    btp get services/binding --name <CLS_BINDING_NAME> --subaccount <SUBACCOUNT_ID>
    ```


