<!-- loio1830bca1b060484e9cfabc0e62472e8e -->

# Configuration Parameters

SAP Cloud Logging supports the following parameters for `create service` and `update service` operations.



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_gzh_zcn_lzb"/>

## Configuration Parameters


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

backend

</td>
<td valign="top">

No

</td>
<td valign="top">

[backend](configuration-parameters-1830bca.md#loio1830bca1b060484e9cfabc0e62472e8e__table_xyd_p3x_jzb) 

</td>
<td valign="top">

Configures the OpenSearch backend.

</td>
</tr>
<tr>
<td valign="top">

dashboards

</td>
<td valign="top">

No

</td>
<td valign="top">

[dashboards](configuration-parameters-1830bca.md#loio1830bca1b060484e9cfabc0e62472e8e__table_gqx_w3x_jzb) 

</td>
<td valign="top">

Configures the dashboards UI.

</td>
</tr>
<tr>
<td valign="top">

ingest

</td>
<td valign="top">

No

</td>
<td valign="top">

[ingest](configuration-parameters-1830bca.md#loio1830bca1b060484e9cfabc0e62472e8e__table_brp_bjx_jzb) 

</td>
<td valign="top">

Configures the ingest endpoint.

</td>
</tr>
<tr>
<td valign="top">

ingest\_otlp

</td>
<td valign="top">

No

</td>
<td valign="top">

[ingest\_otlp](configuration-parameters-1830bca.md#loio1830bca1b060484e9cfabc0e62472e8e__table_zcy_jjx_jzb) 

</td>
<td valign="top">

Configures the data ingestion over the ingest-otlp endpoint \(OpenTelemetry Protocol\).Configures the maximum number of OpenSearch data nodes for disk-based auto-scaling. Must be between `2` and `10`. Defaults to `10`.

</td>
</tr>
<tr>
<td valign="top">

retention\_period

</td>
<td valign="top">

No

</td>
<td valign="top">

Integer

</td>
<td valign="top">

The time in days until data \(see [Ingest Observability Data](ingest-observability-data-ba16ff7.md)\) is deleted. Range is between `1` and `90`. Defaults to `7`. That deletion of ingested data can also happen due to size-based curation. Changing this parameter will only affect newly created indices.

</td>
</tr>
<tr>
<td valign="top">

saml

</td>
<td valign="top">

No

</td>
<td valign="top">

[saml](configuration-parameters-1830bca.md#loio1830bca1b060484e9cfabc0e62472e8e__table_nrv_sjx_jzb) 

</td>
<td valign="top">

Configures the SAML Integration to authenticate in dashboards.

</td>
</tr>
<tr>
<td valign="top">

rotate\_root\_ca

</td>
<td valign="top">

No

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

> ### Note:  
> Updating this parameter can invalidate bindings permanently

Controls the rotation of the ingestion root Certificate Authority \(CA\) certificate. Defaults to `false`.

Refer to [Rotate the Ingestion Root CA Certificate](rotate-the-ingestion-root-ca-certificate-bbcb3e7.md) for more details.

</td>
</tr>
</table>



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_fhr_wcn_lzb"/>

## Configuration Parameters for `backend`


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

max\_data\_nodes

</td>
<td valign="top">

No

</td>
<td valign="top">

Integer

</td>
<td valign="top">

Indirectly, this parameter sets the maximum disk size for storing observability data as described in [Service Plans](service-plans-a9d2d1b.md). This parameter has no effect for the `dev` plan. Needs to be between `2` and `10`. Default is `10`.

</td>
</tr>
</table>



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_hnn_zbn_lzb"/>

## Configuration Parameters for `dashboards`


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

custom\_label

</td>
<td valign="top">

No

</td>
<td valign="top">

String

</td>
<td valign="top">

Set a custom label to be displayed in OpenSearch Dashboards in the top bar to identify and distinguish multiple service instances. The label is embedded into a fixed sized element due to technical limitations. It gets cut off if the content is too long. 12 characters is ideal, and the maximum length is 20. Supported characters are `A-Z`, `a-z`, `0-9`, `#`, `+`, `-`, `_`, `/`, `*`, `(`, `)`, and space.

</td>
</tr>
</table>



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_o51_4bn_lzb"/>

## Configuration Parameters for `ingest`


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

max\_instances

</td>
<td valign="top">

No

</td>
<td valign="top">

Integer

</td>
<td valign="top">

Specifies the maximum number of provisionable ingest instances, which are scaled automatically based on their overall CPU utilization. Must be between `2` and `10`. Defaults to `2`. This parameter impacts peak throughput and buffering. Scale-out happens when the overall CPU utilization exceeds 80%. Scale-in happens when the overall CPU utilization or configuration parameter decreases. This parameter has no effect on the `dev` plan, which is limited to a single instance.

</td>
</tr>
</table>



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_vc3_w1n_lzb"/>

## Configuration Parameters for `ingest_otlp`


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

enabled

</td>
<td valign="top">

No

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Enables ingestion over the OpenTelemetry Protocol. Defaults to false. For more information, refer to [Ingest via OpenTelemetry API Endpoint](ingest-via-opentelemetry-api-endpoint-fdc78af.md).

</td>
</tr>
</table>



<a name="loio1830bca1b060484e9cfabc0e62472e8e__section_m4y_p1n_lzb"/>

## Configuration Parameters for `saml`

> ### Caution:  
> Ensure that you consider the [SAP BTP Security Recommendation BTP-CLS-0001](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0001&version=Cloud).

Configuration to integrate the service with a SAML Idenditiy Provider \(IdP\), like SAP Cloud Identity Services - Identity Authentication \(Identity Authentication\). See [Prerequisites](prerequisites-41d8559.md) on how to integrate SAP Cloud Logging with Identity Authentication. This configuration exposes a subset of the SAML parameters of OpenSearch. Learn more about configuration parameters from [OpenSearch](https://opensearch.org/)


<table>
<tr>
<th valign="top">

Name

</th>
<th valign="top">

Required

</th>
<th valign="top">

Type

</th>
<th valign="top">

Description

</th>
</tr>
<tr>
<td valign="top">

enabled

</td>
<td valign="top">

Yes

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Enables SAML authentication. We strongly recommend SAML authentication for production use cases, because of improved security and login flow. Basic authentication is configured if this parameter is set to `false`.

</td>
</tr>
<tr>
<td valign="top">

admin\_group

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

String

</td>
<td valign="top">

The SAML group to grant administrative access and permissions to modify the security module. Required if `enabled` is set to `true`. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

initiated

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

Boolean

</td>
<td valign="top">

Enables IdP-initiated SSO. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

roles\_key

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

String

</td>
<td valign="top">

The list of backend\_roles will be read from this attribute during user login.

This field must be set to the corresponding attribute for IdP groups,usually `groups`. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

idp.metadata\_url

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

URL

</td>
<td valign="top">

The URL to get the SAML IdP metadata from. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

idp.entity\_id

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

String

</td>
<td valign="top">

The Entity ID of the SAML IdP.

Open the metadata URL in your browser and copy the full value of the `entityID` field. It is located in the first line of the response. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

sp.entity\_id

</td>
<td valign="top">

Conditionally

</td>
<td valign="top">

String

</td>
<td valign="top">

The Entity ID of the service provider. Generally, this parameter is set to the name of your application configured in your IdP. Required if `enabled` is set to `true`.

</td>
</tr>
<tr>
<td valign="top">

sp.signature\_private\_key

</td>
<td valign="top">

No

</td>
<td valign="top">

String

</td>
<td valign="top">

The private key is used to sign the requests. This parameter must be valid base64 encoded and PKCS8 format.

</td>
</tr>
<tr>
<td valign="top">

sp.signature\_private\_key\_password

</td>
<td valign="top">

No

</td>
<td valign="top">

String

</td>
<td valign="top">

The private key used to sign the requests. Valid base64 encoded and PKCS8 format of private key.

</td>
</tr>
<tr>
<td valign="top">

exchange\_key

</td>
<td valign="top">

No

</td>
<td valign="top">

String

</td>
<td valign="top">

Key to sign tokens. Provide a `random` key with an `even number (min. length: 32)` of `alphanumeric characters (A-Z, a-z, 0-9)`. A random key is generated if the key isn't provided.

</td>
</tr>
</table>

