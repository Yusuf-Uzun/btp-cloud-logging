<!-- loiobbcb3e7f5a384ce794e8444158055c0e -->

# Rotate the Ingestion Root CA Certificate

Rotating the ingestion root Certification Authority \(CA\) certificate is a delicate process that must be performed with special care. It's a three-step process that provisions a new root CA certificate for your instance, then invalidates the previous root CA certificate.



Root CA rotation is only necessary when ingesting data using mutual TLS authentication \(mTLS\), and

-   The root CA certificate of your CLS instance is expiring soon, or
-   The private key of a client certificate was leaked.

> ### Caution:  
> Not following this process can result in premature invalidation of certificates and, as a result, interruption of log ingestion.



<a name="loiobbcb3e7f5a384ce794e8444158055c0e__section_iq2_1md_dzb"/>

## CA Rotation Procedure

1.  Create a new Root CA certificate.

    Start the root CA certificate rotation by updating the `rotate_root_ca` [Configuration Parameters](configuration-parameters-1830bca.md) from `false` to `true`. This creates a new root CA certificate for your service instance, while retaining the previous root CA certificate. Bindings created before the service instance update continue to work, so ingestion isn't affected. New bindings return certificates issued by the new root CA.

2.  Rebind all Applications.

    With the new root CA certificate in place, create new bindings for each shipping mechanism. Since this process depends on the sending mechanism, refer to [Ingest Observability Data](ingest-observability-data-ba16ff7.md).

3.  Delete the old root CA certificate.

    After recreating all of your ingestion-related bindings, update your service instance configuration `rotate_root_ca` service parameter to `false`. Afterwards, validate that the ingestion still works as expected for all bindings.


