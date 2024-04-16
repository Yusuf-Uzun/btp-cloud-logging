<!-- loioa7c0d8d3e6f1438c83cc313cba1f57fc -->

# Stability

The service quality of Cloud Logging instances is a shared responsibility between service provider and service user.



<a name="loioa7c0d8d3e6f1438c83cc313cba1f57fc__section_w2m_tgy_11c"/>

## What does Cloud Logging manage on my behalf?

Cloud Logging manages the work involved in setting up a domain, from provisioning infrastructure capacity to installing the OpenSearch software. Once your instance is running, Cloud Logging automates common administrative tasks, such as auto-scaling, retention, and patching software. Cloud Logging also offers and maintains end-to-end integration into different SAP environments by providing specific ingestion endpoints, parsing, and preconfigured dashboards.



<a name="loioa7c0d8d3e6f1438c83cc313cba1f57fc__section_vtm_vgy_11c"/>

## How can atypical usage compromise service qualities?

Cloud Logging strives to deliver a resilient service.

However, the flexibility in utilization and configuration of OpenSearch features comes at the cost that service quality may be compromised through atypical usage.

Areas of usage that may result in service degradation when deviating from documented paths include:

-   **Overloading the OpenSearch component**
-   -   **Exceeding Ingestion Limits**: going beyond the save ingestion limits specified in [Service Plans](service-plans-a9d2d1b.md) documentation may impact the service quality of the database. Deleting indices that is written to or filling the disk \(without considering auto-scaling\) within one day may disrupt service qualities.
-   **Resource-Intensive Recurring Tasks**: The configuration of recurring tasks in a resource-expensive way may impact the service quality of the database.
-   **Backend API Usage**: OpenSearch API can be enabled which opens a plethora of possibilities to impact the service quality of the database.

-   **Incorrect Identity Provider Configuration**: Incorrectly configured identity provider may result in the inability to login to the dashboards UI.
-   **Neglecting Certificate Rotation**: Lack of certificates rotation leads to interrupted ingestion after certificate expiry.

