<!-- loio3416f8fae3374cd9a71e5c4ab8ddd399 -->

# Ingest via JSON API Endpoint

You can ship documents to SAP Cloud Logging via `JSON` endpoint.

> ### Note:  
> Alternately, you can also [ingest logs, metrics, and traces in OpenTelemetry format](ingest-via-opentelemetry-api-endpoint-fdc78af.md).

> ### Caution:  
> Ensure that you consider [SAP BTP Security Recommendation BTP-CLS-0003](https://help.sap.com/docs/btp/sap-btp-security-recommendations-c8a9bb59fe624f0981efa0eff2497d7d/sap-btp-security-recommendations?seclist-index=BTP-CLS-0003).

You can use arbitrary log shippers to send to the mTLS ingest JSON API endpoint. For example, a Fluent Bit output configuration for sending logs would be:

```
[OUTPUT]
    Name  http
    Match *
    Host <endpoint>
    Port 443
    tls.crt_file <cert-file>
    tls.key_file <key-file>
    tls true
    Compress gzip
    URI /
    Format json
    Retry_Limit 3

```

With `Host`, `tls.crt_file`, and `tls.key_file` configured according to your service instance.

To find and analyze the data, the payload should contain a `date` field. Use the `msg` field to contain the string information for a document.



<a name="loio3416f8fae3374cd9a71e5c4ab8ddd399__section_jh4_thx_jbc"/>

## Procedure

The Ingest JSON API allows you to add `JSON` documents to your Cloud Logging instance.



### Single document

This operation adds a single document in JSON format:

```
PUT /
{
  "msg": "Single log ingest",
  "date": 1668415176,
  "additional_field":"additional value"
}

```

The following fields are expected:

-   `date`: This field must contain the timestamp, under which the record will be indexed into OpenSearch. There are two formats possible:
    -   `Epoch Unix Timestamp`: This works correctly with all possible precisions, from seconds to nanoseconds. Format it as a number.
    -   `ISO 8601`: This is the standard json timestamp. Format it as a string.
    -   `default`: If the date field cannot be extracted from the payload, the current time while parsing is used instead. This timestamp is relevant to find the document in the OpenSearch Dashboard UI.

-   `msg`: This field must contain the string payload of a log. It is added if not present in a log, and defaults to "-". Format it as a string.
-   The `additional_data` field is a placeholder for any further fields. There are no restrictions on what data you can add, but keep two things in mind:
    -   It supports a maximum of 1000 fields.
    -   Field type collisions \(such as string and object\) result in log rejects, and possibly make indices incoherent when rolling over.




### Multiple documents

In addition to sending one document per HTTP call, you can upload multiple documents in one HTTP call using JSON batching:

```
PUT /
[
  {
    "msg": "Message 1",
    "date": 1668415178
  },
  {
    "msg": "Message 2",
    "date": 1668415185
  }
]

```

Setting the `Content-Encoding` header allows HTTP clients to transfer payload more efficiently by applying compression. Currently, it supports gzip and deflate. You can test gzip using the following command:

```
echo 'json={"msg":"foobar"}' | gzip > json.gz
curl --data-binary @json.gz -H "Content-Encoding: gzip" \
     --cert client.crt --key client.key --cacert server-ca.crt \
       https://<ingest-mtls-endpoint>/

```

This feature is especially useful for users who handle large amounts of data.



### Host and Authentication

You can read the `ingest-mtls-endpoint`, as well as the credentials \(`ingest-mtls-username` and `ingest-mtls-password`\) that have to be added to the request header, from the service key or service binding. Find out how to create service keys or bindings from the following: [Creating Service Keys](https://help.sap.com/docs/btp/sap-business-technology-platform/creating-service-keys), [Create Bindings](https://help.sap.com/docs/btp/sap-business-technology-platform/binding-service-instances-to-applications), [Create bindings with SAP BTP Operator](https://github.com/SAP/sap-btp-service-operator/blob/main/README.md#step-2-create-a-service-binding).

> ### Caution:  
> TLS certificates for client authentication are issued with a validity period of 90 days by default. Don't forget to rotate the service key and update the credentials in all sender configurations. Otherwise, ingestion will stop!

> ### Note:  
> The validity period can be configured via service binding parameter within the range of 1 to 180 days by utilizing the certValidityDays. For example, passing `{"ingest":{"certValidityDays":30}}` as a configuration parameter for binding creation sets the validity to 30 days.

> ### Note:  
> Deleting a binding does not revoke the corresponding certificate. [Rotate the Ingestion Root CA Certificate](rotate-the-ingestion-root-ca-certificate-bbcb3e7.md) if the root CA of your service instance is expiring soon, or the private key of a certificate was leaked.



### Request body

Your request body must contain the information you want to index.

```
{
  "date": 123456.
  "msg": "This is just a sample document",
  ...
}
```



### Response

Cloud Logging adheres to HTTP response code standards. If your request was successful, the response looks like:

```
HTTP/2 200
date: Mon, 17 Oct 2022 08:52:17 GMT
content-type: text/plain
content-length: 0
strict-transport-security: max-age=15724800; includeSubDomains

```

If the payload is not valid JSON, an example response looks like:

```
HTTP/2 400
date: Mon, 17 Oct 2022 08:57:54 GMT
content-type: text/plain
content-length: 51
strict-transport-security: max-age=15724800; includeSubDomains

400 Bad Request
Received event is not JSON: {12121

```

If you cannot find the payload shipped in the respective index, the default dashboards may provide a hint on the root cause.



<a name="loio3416f8fae3374cd9a71e5c4ab8ddd399__section_fvt_qnx_jbc"/>

## Result

You can analyze the ingested data in OpenSearch Dashboards \(see [Access and Analyze Observability Data](access-and-analyze-observability-data-dad5b01.md), based on custom dashboards or the Discover view. Indices match the following pattern: `logs-json-.*` 

> ### Note:  
> There are OpenSearch specifics restricting ingestion, such as mapping conflicts. Mapping conflicts in OpenSearch occur when indices have conflicting field mappings, leading to ambiguity in data interpretation and query failures. Resolving these conflicts involves aligning field types, analyzers, and other properties across indices to ensure consistent data handling and accurate search results. Cloud Logging performs basic sanitation and unification steps to prevent mapping conflicts. However, sending type-aligned data is mandatory to prevent mapping conflicts.

> ### Note:  
> Cloud Logging provides parsing for w3c headers, sap\_passport, and other correlation mechanisms.

