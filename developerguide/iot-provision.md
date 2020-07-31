# Device provisioning<a name="iot-provision"></a>

When you provision a device with AWS IoT, you must create resources so your devices and AWS IoT can communicate securely\. Other resources can be created to help you manage your device fleet\. The following resources can be created during the provisioning process: 
+ An IoT thing\.

  IoT things are entries in the AWS IoT device registry\. Each thing has a unique name and set of attributes, and is associated with a physical device\. Things can be defined using a thing type or grouped into thing groups\. For more information, see [Managing devices with AWS IoT](iot-thing-management.md)\.

   Although not required, creating a thing makes it possible to manage your device fleet more effectively by searching for devices by thing type, thing group, and thing attributes\. For more information, see [Fleet indexing service](iot-indexing.md)\.
+ An X\.509 certificate\.

  Devices use X\.509 certificates to perform mutual authentication with AWS IoT\. You can register an existing certificate or have AWS IoT generate and register a new certificate for you\. You associate a certificate with a device by attaching it to the thing that represents the device\. You must also copy the certificate and associated private key onto the device\. Devices present the certificate when connecting to AWS IoT\. For more information, see [Authentication](authentication.md)\.
+ An IoT policy\.

  IoT policies define the operations a device can perform in AWS IoT\. IoT policies are attached to device certificates\. When a device presents the certificate to AWS IoT, it is granted the permissions specified in the policy\. For more information, see [Authorization](iot-authorization.md)\. Each device needs a certificate to communicate with AWS IoT\.

AWS IoT supports automated fleet provisioning using provisioning templates\. Provisioning templates describe the resources AWS IoT requires to provision your device\. Templates contain variables that enable you to use one template to provision multiple devices\. When you provision a device, you specify values for the variables specific to the device using a dictionary or *map*\. To provision another device, specify new values in the dictionary\.

You can use automated provisioning whether or not your devices have unique certificates \(and their associated private key\)\.

## Fleet provisioning APIs<a name="provisioning-apis"></a>

There are several categories of APIs used in fleet provisioning:
+ These control plane functions create and manage the fleet provisioning templates and configure trusted user policies\.
  + [CreateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplate.html)
  + [ CreateProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningTemplateVersion.html)
  + [DeleteProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplate.html)
  + [DeleteProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DeleteProvisioningTemplateVersion.html)
  + [DescribeProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplate.html)
  + [DescribeProvisioningTemplateVersion](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeProvisioningTemplateVersion.html)
  + [ListProvisioningTemplates](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplates.html)
  + [ListProvisioningTemplateVersions](https://docs.aws.amazon.com/iot/latest/apireference/API_ListProvisioningTemplateVersions.html)
  + [UpdateProvisioningTemplate](https://docs.aws.amazon.com/iot/latest/apireference/API_UpdateProvisioningTemplate.html)
+ Trusted users can use this control plane function to generate a temporary onboarding claim\. This temporary claim is passed to the device during Wi\-Fi config or similar method\.
  + [CreateProvisioningClaim](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateProvisioningClaim.html)\.
+ The MQTT API used during the provisioning process by devices with a provisioning claim certificate embedded in a device, or passed to it by a trusted user\.
  + [CreateCertificateFromCsr](fleet-provision-api.md#create-cert-csr)
  + [CreateKeysAndCertificate](fleet-provision-api.md#create-keys-cert)
  + [RegisterThing](fleet-provision-api.md#register-thing)