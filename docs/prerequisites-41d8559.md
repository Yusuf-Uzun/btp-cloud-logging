<!-- loio41d8559375b84adda2596d943404d93a -->

# Prerequisites

To create instances of SAP Cloud Logging, you must configure entitlements for SAP Cloud Logging, and integrate SAP Cloud Identity Services - Identity Authentication SAML 2.0 with SAP Cloud Logging.



<a name="loio41d8559375b84adda2596d943404d93a__section_u5p_fjy_kzb"/>

## Configure Entitlements for SAP Cloud Logging

To create a service instance of SAP Cloud Logging, you need:

-   A Global Account \(see [Getting a Global Account](https://help.sap.com/docs/btp/sap-business-technology-platform/getting-global-account?version=Cloud)\).
-   A Subaccount \(see [Getting a Subaccount](https://help.sap.com/docs/btp/sap-business-technology-platform/create-subaccount?version=Cloud)\).
-   A service Entitlement for SAP Cloud Logging \(see [Configure Entitlements and Quotas for Subaccounts](https://help.sap.com/docs/btp/sap-business-technology-platform/configure-entitlements-and-quotas-for-subaccounts?version=Cloud)\).

Once you have these three prerequisites, the service is available in the Service Marketplace.



<a name="loio41d8559375b84adda2596d943404d93a__section_klg_hjy_kzb"/>

## Integrate SAP Cloud Identity Services - Identity Authentication SAML 2.0 with SAP Cloud Logging

> ### Caution:  
> Ensure that you consider the [SAP BTP Security Recommendation BTP-CLS-0001](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0001&version=Cloud).

This explains how to integrate with SAP Cloud Identity Services - Identity Authentication SAML 2.0. It results in changes in the Identity Authentication tenant and a corresponding SAML configuration to be used for creating or updating SAP Cloud Logging instances. Access to the Identity Authentication administration console as an administrator is a prerequisite.

> ### Note:  
> We recommend you integrate with Identity Authentication. You can also integrate with other SAML providers, but there will be no support or documentation.

> ### Note:  
> You can reuse the resulting SAML configuration for multiple instances of SAP Cloud Logging.



### Obtain SAML 2.0 IdP Information

Obtain SAML 2.0 Identity Provider \(IdP\) Information based on the [Identity Authorization guide](https://help.sap.com/docs/identity-authentication/identity-authentication/tenant-saml-2-0-configuration). Use the console URL to access the tenant’s administration console for the Identity Authentication service. The URL has a `https://<tenantID>.accounts.ondemand.com/admin` pattern.

-   Note down the `idp.metadata_url` information as `https://<tenant ID>.accounts.ondemand.com/saml2/metadata` 
-   Note down the `idp.entity_id`. Open the metadata URL in your browser and copy the full value of the entityID field, which is located in the first line of the response.



### Create a SAML 2.0 application

Create a SAML 2.0 application in your Identity Authentication account based on the [Identity Authorization guide](https://help.sap.com/docs/identity-authentication/identity-authentication/create-saml-2-0-application) and note down the `sp.entity_id` value as name of the SAML 2.0 application.



### Configure the SAML 2.0 application

Go to *Applications & Resources*, choose *Applications*, and select your application from the list. Then perform the following steps to configure the SAML 2.0 application within Identity Authentication:

1.  [Configure a Self-Defined Attribute](https://help.sap.com/docs/identity-authentication/identity-authentication/user-attributes?version=Cloud) with *Name* "groups," *Source* "Identity Directory," and *Value* "Groups."
2.  [Configure Default Name ID Format](https://help.sap.com/docs/identity-authentication/identity-authentication/configure-subject-name-identifier-sent-to-application?version=Cloud) to *E-mail*.
3.  Select *SAML 2.0 Configuration* and *Configure Manually*.
    -   Set the name with value of the `sp.entity_id` from the Create a SAML 2.0 application step.
    -   Continue with one of the following options. **OPTION 1** is recommended, as it removes the need to specify the IdP SAML application's assertion/logout URL.
    -   **OPTION 1:** Enable request signing.
        -   Create a new signing certificate and private key in PKCS8 format.

            ```
            # generate a certificate and a private key in PKCS8 format with a reasonable validity
            openssl req -x509 -newkey rsa:2048 -keyout private.key -out cert.pem -nodes -days <validity>
            # add a password (encrypted)
            openssl pkcs8 -topk8 -v1 PBE-SHA1-3DES -in private.key -out private_pkcs8.key
            # encode key to base64 format
            printf "%s" "$(< private_pkcs8.key)" | base64
            
            ```

        -   Enable request signing in Identity Authentication by setting *Require signed authentication requests* to *ON*, going to the *Signing Certificate* section, clicking *Add*, and uploading the certificate.
        -   Make sure to provide a signing key to the `sp.signature_private_key` field and set the sp.signature\_private\_key\_password field if the signing key is encrypted. The signing certificate in your Identity Authentication SAML 2.0 application can expire, and Identity Authentication rejects login attempts with the error message, "The digital signature of the received SAML2 message is invalid."

    -   **OPTION 2:** ⚠️ This step can only be done after an SAP Cloud Logging instance has been created and has to be repeated for each new service instance.
        -   Set `Assertion Consumer Service Endpoint` to the OpenSearch Dashboards URL plus`/_opendistro/_security/saml/acs`.
        -   Set `Single Logout Endpoint`: Set binding to HTTP\_REDIRECT and the URL must be the OpenSearch Dashboards URL without any path.
        -   To store the configuration, click *Save* .





### Create a Group and Assign Users

-   [Create a group](https://help.sap.com/docs/identity-authentication/identity-authentication/create-new-user-group) and named `admin_group` for the SAML configuration. This group gets administrative access in OpenSearch. It has permission to modify the security module.

    > ### Note:  
    > The login procedure forwards Identity Authentication group names to OpenSearch as backend roles. Backend roles can map to OpenSearch roles that grant permissions to the users assigned to the respective Identity Authentication groups. The configuration parameter `admin_group` is mapped automatically to the "all\_access" role

-   [Add users to the group](https://help.sap.com/docs/identity-authentication/identity-authentication/add-users-to-group) who should have admin access. Users can be added or removed at any time.



### Compose SAML Configuration Parameters

Compose SAML configuration parameters to be used for service instance creation or updates:


<table>
<tr>
<th valign="top">

SAML Configuration Template

</th>
<th valign="top">

Parameterization

</th>
</tr>
<tr>
<td valign="top" rowspan="5">

```
"saml": {
     "enabled": true,
     "initiated": true,
     "idp": {
        "metadata_url": "",
        "entity_id": ""
      },
      "admin_group": "",
     "roles_key": "groups",
     "sp": {
        "entity_id": "",
        "signature_private_key": "",
        "signature_private_key_password": ""
     },
     "exchange_key": ""
  }

```



</td>
<td valign="top">

Set IdP information `idp.metadata_url` and `idp.entity_id` from Obtain SAML 2.0 IdP Information step.

</td>
</tr>
<tr>
<td valign="top">

Set `sp.entity_id` from Create a SAML 2.0 application step.

</td>
</tr>
<tr>
<td valign="top">

Set `admin_group` from Configure a SAML 2.0 application step.

</td>
</tr>
<tr>
<td valign="top">

Set `sp.signature_private_key` and `sp.signature_private_key_password` if you selected OPTION 1 in the Configure SAML 2.0 application step.

</td>
</tr>
<tr>
<td valign="top">

Add `exchange_key` to sign tokens, or remove line. Provide a random key with an even character length \(minimum length: 32\) of alphanumeric characters \(A-Z, a-z, 0-9\). The system generates a random key if the key is not specified.

</td>
</tr>
</table>

See [Configuring Applications](https://help.sap.com/docs/identity-authentication/identity-authentication/configuring-applications) in Identity Authentication Service.

