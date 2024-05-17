<!-- loiof6aa131faee64f78b9cbba6a5b579b8f -->

# Create an SAP Cloud Logging Instance through SAP BTP Service Operator

> ### Note:  
> Instances created with SAP BTP Operator can only be managed from SAP BTP Operator. Management operations that require SAP BTP Operator include service update and deletion, as well as binding creation and access.



<a name="loiof6aa131faee64f78b9cbba6a5b579b8f__section_ndz_bcz_kzb"/>

## Prerequisites

-   See [Prerequisites](prerequisites-41d8559.md).
-   You need [sap-btp-service-operator installed in Kubernetes cluster](https://github.com/SAP/sap-btp-service-operator/blob/main/README.md#setup). In [SAP BTP, Kyma runtime](https://help.sap.com/docs/btp/sap-business-technology-platform/create-kyma-environment-instance), SAP BTP Service Operator is available if the `btp-operator` [module](https://help.sap.com/docs/btp/sap-business-technology-platform/kyma-modules) is [enabled](https://help.sap.com/docs/btp/sap-business-technology-platform/enable-and-disable-kyma-module).



<a name="loiof6aa131faee64f78b9cbba6a5b579b8f__section_kf5_12z_kzb"/>

## Create a Service Instance

1.  To create the namespace `sap-cloud-logging-integration`, execute the following command:

    ```
    kubectl create namespace sap-cloud-logging-integration
    ```

2.  To create a service instance of SAP Cloud Logging, first create a `ServiceInstance` custom-resource yaml file. See [Service Plans](service-plans-a9d2d1b.md) and [Configuration Parameters](configuration-parameters-1830bca.md) for configuration options.

    ```
       apiVersion: services.cloud.sap.com/v1alpha1
       kind: ServiceInstance
       metadata:
           name: < name >
       spec:
           serviceOfferingName: cloud-logging
           servicePlanName: < service plan >
           externalName: < externalName >
           parameters:
             < parameterName1 >: < parameterValue1 >
             < parameterName2 >: < parameterValue2 >
    
    ```

    For example:

    ```
       apiVersion: services.cloud.sap.com/v1alpha1
       kind: ServiceInstance
       metadata:
           name: created-with-sap-btp-service-operators
       spec:
           serviceOfferingName: cloud-logging
           servicePlanName: standard
           externalName: cloud-logging-created-with-sap-btp-service-operators
           parameters:
             retentionPeriod: 14
             ingest:
                max_instances: 10
    
    ```

3.  Apply the custom-resource file in your cluster to create the instance in the sap-cloud-logging-integration namespace. Deploy the configuration with:

    ```
    kubectl apply -n sap-cloud-logging-integration -f path/to/my-service-instance.yaml
    ```

4.  Wait for your dedicated instance to be provisioned. Check the status by executing:

    ```
    kubectl get serviceinstances.services.cloud.sap.com -o yaml
    ```


> ### Note:  
> To update service parameters, change the values in your yaml file and deploy the changes with `kubectl apply`.

> ### Note:  
> If you have questions about these steps, see [SAP BTP Service operator service instance creation documentation](https://github.com/SAP/sap-btp-service-operator/blob/main/README.md#step-1-create-a-service-instance).



<a name="loiof6aa131faee64f78b9cbba6a5b579b8f__section_ubq_kfz_kzb"/>

## Create a Service Binding

This step results in a `secret` with the name `cls``sap-cloud-logging-integration` namespace of the Kyma cluster, which provides credentials to see and ingest data.

1.  Create a `ServiceBinding` and `secret` with the BTP Operator in the `sap-cloud-logging-integration` namespace by executing the following command: in the

    ```
    cat <<EOF | kubectl apply -n sap-cloud-logging-integration -f -
    apiVersion: services.cloud.sap.com/v1
    kind: ServiceBinding
    metadata:
      name: cls-binding
    spec:
      serviceInstanceName: cls-service-instance
      externalName: cls-binding-external
      secretName: sap-cloud-logging
      credentialsRotationPolicy:
        enabled: true
        rotationFrequency: 168h
    
    ```

    We recommend you enable [credentials rotation](https://github.com/SAP/sap-btp-service-operator/blob/main/README.md#credentials-rotation) for the ServiceBinding, so that the configuration is updated automatically. Assure that the *rotationFrequency* binding parameter reflects a period of more than a day to avoid frequent restarts.

    The binding creation automatically triggers the creation of a `secret` with the name `sap-cloud-logging` in in the same namespace.




### How to Share Credentials to Ship Observability Data from Other Clusters

Insert the endpoints and credentials from an SAP Cloud Logging key in the following snippet, and run the command to create a Kubernetes secret.

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
 name: https://help.sap.com/docs/btp/sap-business-technology-platform/enable-and-disable-kyma-module
stringData:
 # To ingest logs, skip if you want to configure tracing only
 # certs/keys should be pasted as is, keeping \n characters
  ingest-mtls-endpoint: "<ingest endpoint from service key json>"
  ingest-mtls-key: "<ingest-mtls-key from service key json>"
  ingest-mtls-cert: "<ingest-mtls-cert from service key json>"
  # To ingest distributed traces, skip if you want to configure logging only
  # certs/keys should be pasted as is, keeping \n  characters
  ingest-otlp-endpoint: "<ingest-otlp-endpoint from  service key json>"
  ingest-otlp-key: "<ingest-otlp-key from service key json>"
  ingest-otlp-cert: "<ingest-otlp-cert from service key json

```

> ### Note:  
> The responsibility to rotate credentials remains with the user when applying this approach.

