---

copyright:
  years: 2019, 2020
lastupdated: "2020-02-14"

keywords: vpn for vpc, vpn, connection, on premise connection, on-premises connection

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:table: .aria-labeledby="caption"}
{:download: .download}

# Connecting to your on-premises network  
{: #vpn-onprem-example} 

You can use VPN for VPC to securely connect your VPC to an on-premises network through a VPN tunnel. This topic provides guidance about how to configure your VPN for VPN gateway to connect to your on-premises network.
{:shortdesc}

Create a VPN gateway in your VPC and create a VPN connection between the VPC and the peer gateway of the on-premises network by specifying the following information.
* **Connection name**: Enter a name for the connection, such as `onprem-connection`.
* **Peer gateway address**: Specify the IP address of the VPN gateway for the on-premises network.
* **Preshared key**: Specify the authentication key of the VPN gateway for the on-premises network.
* **Local subnets**: Specify one or more subnets in the VPC you want to connect through the VPN tunnel.
* **Peer subnets**: Specify one or more subnets in the on-premises network you want to connect through the VPN tunnel.

For the Internet Key Exchange (IKE) and IPsec security parameters, select **Auto** so the cloud gateway uses auto-negotiation to automatically establish the connection with the on-premises gateway.

The gateway status appears as **Pending** while the VPN gateway is being created, and the status changes to **Available** after the gateway is created.
{: tip}

## Configuring the on-premises VPN gateway
{: #configuring-onprem-gateway}

The next step is configuring your on-premise VPN gateway peer to connect to your IBM VPN for VPC gateway. The configuration depends on the type of VPN gateway. See the following topics for example configuration.

- [Connecting to a Check Point Security Gateway peer](/docs/vpc?topic=vpc-check-point-config)
- [Connecting to a Cisco ASAv peer](/docs/vpc?topic=vpc-cisco-asav-config)
- [Connecting to a FortiGate peer ](/docs/vpc?topic=vpc-fortigate-config)
- [Connecting to a Juniper vSRX peer](/docs/vpc?topic=vpc-juniper-vsrx-config)
- [Connecting to a strongSwan peer](/docs/vpc?topic=vpc-strongswan-config)
- [Connecting to a Vyatta peer](/docs/vpc?topic=vpc-vyatta-config)

## Checking the status of the secure connection
{: #check-connection-status}

You can check the status of your connection in the {{site.data.keyword.cloud_notm}} console. On the VPN for VPC page, select your VPN gateway and click **Connections** from the navigation pane on the left side of the page.

You can also test the connection by doing a ping from a virtual server instance in your VPC to a server in the on-premises network.
