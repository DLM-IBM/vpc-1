---

copyright:
  years: 2021
lastupdated: "2021-03-30"

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:beta: .beta}


# Resizing a virtual server instance
{: #resizing-an-instance}

Instance resize allows you to vertically scale virtual server instances to any supported profile size in minutes. You can increase or decrease the amount of vCPU and RAM available for greater flexibility in workload management to address resource requirement changes, optimize cost or workload performance.
{:shortdesc}

Virtual servers are configured using profiles, or a combination of instance attributes, such as the number of vCPUs, amount of RAM, network bandwidth, and more that define the size and capabilities of the virtual server instance.
When you upgrade or downgrade an existing server, you choose another profile that has the pre-defined specifications that you need. You cannot customize the configuration of a virtual server. The virtual server profile that you select determines the valid cores, RAM, and disk sizes on the resized instance. For more information on profiles, see [Instance Profiles](/docs/vpc?topic=vpc-profiles).

When you resize an instance:
* you are required to stop, update and start the instance being resized
* data is not deleted from the boot/primary volume or the data volume
* RAM is wiped from the resized instance
* all network configurations are maintained, such as Private IPs, Floating IPs, vNICs, and security groups
* the instance name does not change
* the DC location does not change

Once resizing an instance is complete, you are billed the hourly rate of the new instance profile selected.

You can track the resizing of an instance in Activity Tracker and/or {{site.data.keyword.la_full}} for troubleshooting & audit purposes.

## Resizing dedicated virtual servers
{: #resizing-dedicated-virtual-servers}

Dedicated virtual servers can only be resized to profiles supported by the dedicated host the instance is hosted on. For example, a virtual server provisioned with a profile from the Memory family, can resize to other profiles also belonging to the Memory family.

## Resizing with instance storage
{: #resizing-with-instance-storage}

When you stop a virtual server instance with an instance storage profile, that storage is temporary storage that is available only while your virtual server is running. Data on the drive is unrecovered after stopping the instance.

## Resizing with data volumes
{: #resizing-with-data-volumes}

Any attached data volume remains intact and attached in the resized instance.

## Resizing a virtual server instance using the UI
{: #resizing-a-virtual-server-UI}

Complete the following steps to resize an existing virtual server instance.

1. From the **IBM Cloud Console** menu, select **Virtual server instances**.
2. From the **Virtual server instances for VPC** list, find the virtual server you want to resize and verify that its status is Stopped or Stopping.
3. Select the veritcal ellipsis, and select **Resize**.
4. From the list of available profiles, select the profile you want to use.
    * If you are resizing a dedicated virtual server, you will only see profiles that the dedicated host supports
    * If you are resizing a profile that uses instance storage, you see only profiles that have instance profiles, you can not resize from a virtual server instance that has an instance storage profile to one that does not
5. In the right column, review and check the Terms and Conditions.
6. Select **Resize virutal server instance**.
7. Start the virtual server instance.

## Resizing a virtual server using the CLI
{: #resizing-a-virtual-server-CLI}

Use the `instance-update` command to resize a virtual server.

```ibmcloud is instance-update instance-id --profile profile-id  ```
{:pre}

Where:
* `instance-id` is the ID of the instance you want to resize
* `profile-id` is the ID of the profile you want to use

For example, if you want to resize an instance to the bx2-16x64 profile, the command would look similar to the following sample:

```ibmcloud is instance-update 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --profile bx2-16x64```

## Resizing a virtual server using the API
{: #resizing-a-virtual-server-API}

Use the `instance-update` command to resize a virtual server.



1. Run the following command to find the name of the profile you want to use:
   ```curl  -s -X GET "<api_endpoint>/v1/instance/profiles?generation=2&version=2021-02-01" -H "Authorization: Bearer <IAM token>" ```
2. Select a profile that will work with your instance.
    * For a dedicated virtual server, chose a profile that the dedicated host supports
    * If you use instance storage, choose a profile that has instance profile
    * For data volumes, choose a profile that has data volumes
3. Run the following command:
   ```
   curl -k -sS -X PATCH "<api_endpoint>/v1/instances/<instance id>?generation=2&version=2021-02-01" \
       -H "Authorization: Bearer <IAM token>" \
       -d '
   {
       "profile": {
          "name": "<new profile>"
       }
   } '
   ```
   Where:
      * `instance-id` is the ID of the instance you want to resize
      * `profile-id` is the ID of the profile you want to use
