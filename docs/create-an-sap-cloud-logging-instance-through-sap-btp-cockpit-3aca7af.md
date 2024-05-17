<!-- loio3aca7afd9b8e4a128cc0217c6903b9fa -->

# Create an SAP Cloud Logging Instance through SAP BTP Cockpit





<a name="loio3aca7afd9b8e4a128cc0217c6903b9fa__section_rh2_myy_kzb"/>

## Prerequisites

See [Prerequisites](prerequisites-41d8559.md).



<a name="loio3aca7afd9b8e4a128cc0217c6903b9fa__section_ikf_ctg_xyb"/>

## Create a Service Instance

To create an SAP Cloud Logging instance using the SAP BTP Cockpit, follow these steps:

1.  Open the SAP BTP Cockpit and navigate to the *Instances and Subscriptions* page of your subaccount.
2.  Click *Create*.
3.  Configure your Instance:
    -   Select *cloud-logging service*.
    -   Select your preferred service plan \(see [Service Plans](service-plans-a9d2d1b.md)\).
    -   Set an *Instance Name*.

4.  Configure *Service Configuration Parameters* \(see [Configuration Parameters](configuration-parameters-1830bca.md)\)
5.  Review and click *Create*. It takes some time until SAP Cloud Logging is up.



<a name="loio3aca7afd9b8e4a128cc0217c6903b9fa__section_nb5_hyy_kzb"/>

## Create a Service Key

Get a service key to access instance credentials.

1.  Select your SAP Cloud Logging instance to open the *Bindings* panel and click *Create*.
2.  Enter a name for the binding and click *Create*.
3.  Click the three dots next to the newly created binding and select *View* to show the credentials of the service instance.

