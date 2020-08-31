---

copyright:
  years: 2020
lastupdated: "2020-07-10"

keywords: network load balancer, public, listener, back-end, front-end, pool, round-robin, weighted, connections, methods, policies, APIs, access, ports, vpc, vpc network, create

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Creating a network load balancer
{: #nlb-ui-creating-network-load-balancer}

You can create a {{site.data.keyword.cloud}} Network Load Balancer (NLB) for VPC using the UI, CLI or API. To order and start using the Network Load Balancer for VPC, you need two main items:

   * An [IBMid](https://www.ibm.com/account/us-en/signup/register.html){:external} account.
   * A VPC in which to deploy the network load balancer.


## Using the UI
{: #nlb-ui}

To create and configure a network load balancer using the UI, follow these steps:

1. From your browser, open the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com){:external} and log in to your account.
1. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **VPC Infrastructure > Load balancers**
1. Click **New load balancer +** in the upper right of the page.
1. In the order form, complete the following information:
   * Type a unique name for your load balancer and select the VPC instance you created.
   * Select a resource group. Use the default group, or select from the list (if defined for your account). Remember that you cannot change the resource group after the load balancer is created.
   * Select the region, load balancer Network tile, and subnet where you want to deploy the load balancer.
   * Optionally, add tags.

1. Click on **New Pool** and specify the following information to create back-end
pool. You can create one or more pools.
   * Type a name for the pool, such as my-pool.
   * Enter a protocol for your instances in this pool. The protocol of the pool
   must match the protocol of its associated listener. For example, if the
   listener is TCP, the protocol of the pool must be TCP.
   * Select the method: the load balancing algorithm. Options are round robin, weighted round
   robin, and least connections.
     * **Round robin**: Forward requests to each instance in turn. All instances receive approximately an equal number of client connections.
     * **Weighted round robin**: Forward requests to each instance in proportion to its assigned weight. For example, you have instances A, B, and C, and their weights are set to 60, 60 and 30. Instances A and B receive an equal number of connections, and instance C receives half as many connections.
     * **Least connections**: Forward requests to the instance with the least number of connections at the current time.
   * Choose session stickiness and select None or Source IP.
   * Under the health checks, the following options are shown.
     * **Health check path**: Health path is applicable only if HTTP is selected as the health check protocol. The health path specifies the URL used by the load balancer to send the HTTP health check requests to the instances in the pool. By default, health checks are sent to the root path (/).
     * **Health protocol**: The protocol used by the load balancer to send health check messages to the instances in the pool.
     * **Health port**: The port on which to send health check requests. By default, health checks are sent on the same port on which traffic is sent to the instance.
     * **Interval**: Interval in seconds between two consecutive health check attempts. By default, health checks are sent every 5 seconds.
     * **Timeout**: Maximum amount of time the system waits for a response from a health check request. By default, the load balancer waits 2 seconds for a response.
     * **Max retries**: Maximum number of health check attempts that the load balancer makes before an instance is declared unhealthy. By default, an instance is no longer considered healthy after two failed health checks.

       Although the load balancer stops sending connections to unhealthy instances, the load balancer continues monitoring the health of these instances and resumes their use if they're found healthy again (that is, if they successfully pass two consecutive health check attempts).

     If instances in the pool are unhealthy and you believe that your application is running fine, double check the health protocol and health path values. Also, check any security groups that are attached to the instances to ensure that the rules allow traffic between the load balancer and the instances.
     {: tip}

1. Click **New listener** and specify the following information to create a listener. You can create one or more listeners.
   * **Protocol**: The protocol to use for receiving incoming requests.
   * **Port**: The listening port on which requests are received. The port range of 56500 to 56520 is reserved for management purposes and can't be used.
   * **Back-end pool**: The default back-end pool to which this listener forwards traffic.
   * **Max connections** (optional): Maximum number of concurrent connections the listener allows.

1. An order summary shows pricing estimates. Review the Cloud Services terms.
Then, click **Create** to complete your order.

## Using the CLI
{: #nlb-cli-creating-network-load-balancer}

The following example illustrates using the CLI to create a network load balancer. In this example, it is in front of one VPC virtual server instance (id `0716_6acdd058-4607-4463-af08-d4999d983945`) running a TCP server that listens on port 9090. The load balancer has a front-end listener, which allows secure access to the TCP server. 

To create a network load balancer using the CLI, perform the following procedure:

1. Set up your [CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

2. Use the terminal to log in to your account using the CLI. After you enter the password, the system asks which account and region you want to use:

    ```
    ibmcloud login --sso
    ```

3. Create a load balancer:

  ```
  bash
  ibmcloud is load-balancer-create nlb-test public --subnet 0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c --family network 
  ```
  {: codeblock}
  
  Sample output:

  ```
  Creating load balancer nlb-test in resource group  under account IBM Cloud Network Services as user test@ibm.com...
                            
    ID                 r134-99b5ab45-6357-42db-8b32-5d2c8aa62776    
    Name               nlb-test   
    CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r134-99b5ab45-6357-42db-8b32-5d2c8aa62776  
    Family             Network   
    Host name          99b5ab45-us-south.lb.test.appdomain.cloud   
    Subnets            ID                                          Name      
                       0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb      
                          
    Public IPs            
    Private IPs           
    Provision status   create_pending   
    Operating status   offline   
    Is public          true   
    Listeners             
    Pools              ID   Name      
                          
    Resource group     ID                                 Name      
                       3021f90279574ce287dd5fba82c08899   Default      
                          
    Created            2020-08-27T14:34:34.732-05:00 
  ```
  {: screen}

4. Create a pool:

  ```
  bash
  ibmcloud is load-balancer-pool-create nlb-pool r134-99b5ab45-6357-42db-8b32-5d2c8aa62776  weighted_round_robin tcp 10 
  ```
  {: codeblock}
  
  Sample output:

  ```
  Creating pool nlb-pool of load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776  under account IBM Cloud Network Services as user test@ibm.com...
                              
  ID                         r134-3b66d605-6aa5-4166-9f66-b16054da3cb0   
  Name                       nlb-pool   
  Protocol                   tcp   
  Algorithm                  weighted_round_robin   
  Instance group             ID   Name      
                             -    -      
                                 
  Health monitor             Type   Port   Health monitor URL   Delay   Retries   Timeout      
                             http   8080   /                    10      2         5      
                                
  Session persistence type   source_ip   
  Members                       
  Provision status           active   
  Created                    2020-08-27T14:45:42.038-05:00 
  ```
{: screen}
  
5. Create a member:
  
  ```
  bash
  ibmcloud is load-balancer-pool-member-create r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 r134-3b66d605-6aa5-4166-9f66-b16054da3cb0 9090 0716_6acdd058-4607-4463-af08-d4999d983945 --weight 70
  ```
  {: codeblock}
  
  Sample output:
  ```
  Creating member of pool r134-3b66d605-6aa5-4166-9f66-b16054da3cb0 under account IBM Cloud Network Services as user test@ibm.com...
                        
  ID                 r134-61f8b000-a90d-4abe-909e-c507dffec565   
  Port               9090   
  Target             0716_6acdd058-4607-4463-af08-d4999d983945   
  Weight             70   
  Health             unknown   
  Created            2020-08-27T14:59:55.446-05:00   
  Provision status   create_pending 
  ```
  {: screen}
  
6. Create a listener:
  
  ```
  bash
  ibmcloud is load-balancer-listener-create r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 7070 tcp --default-pool r134-3b66d605-6aa5-4166-9f66-b16054da3cb0 
  ```
  {: codeblock} 
  
  Sample output:
  ```
  Creating listener of load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...
                          
  ID                     r134-2847a948-f9b6-4fc1-91c6-f1c49dac3eba   
  Certificate instance   -   
  Connection limit       -   
  Port                   7070   
  Protocol               tcp   
  Default pool           r134-3b66d605-6aa5-4166-9f66-b16054da3cb0   
  Provision status       create_pending   
  Created                2020-08-27T15:16:08.643-05:00  
  ```
  {: screen}

7. Get details about your load balancer:

  ```
  ibmcloud is load-balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776
  ```

  Sample output:

  ```
  Getting load balancer r134-99b5ab45-6357-42db-8b32-5d2c8aa62776 under account IBM Cloud Network Services as user test@ibm.com...
                         
  ID                 r134-99b5ab45-6357-42db-8b32-5d2c8aa62776   
  Name               nlb-test   
  CRN                crn:v1:public:is:us-south-1:a/6266f0fbb7df487d8438b9b31d24cd96::load-balancer:r134-99b5ab45-6357-42db-8b32-5d2c8aa62776   
  Family             Network   
  Host name          99b5ab45-us-south.lb.test.appdomain.cloud   
  Subnets            ID                                          Name      
                     0896-b1f24514-89dc-4afd-b0e2-5489a43cf45c   nlb      
                      
  Public IPs         150.238.50.78, 150.238.54.95   
  Private IPs        10.240.0.58, 10.240.0.59   
  Provision status   active   
  Operating status   online   
  Is public          true   
  Listeners          r134-2847a948-f9b6-4fc1-91c6-f1c49dac3eba   
  Pools              ID                                          Name      
                     r134-3b66d605-6aa5-4166-9f66-b16054da3cb0   nlb-pool      
                        
  Resource group     ID                                 Name      
                     3021f90279574ce287dd5fba82c08899   Default      
                        
  Created            2020-08-27T14:34:34.732-05:00 
  ```
  {: screen}

## Using the API
{: #nlb-api-creating-network-load-balancer}

The following example illustrates using the API to create a network load balancer in front of two VPC virtual server instances (`192.168.100.5` and `192.168.100.6`) running a web application that listens on port 80. The load balancer has a front-end listener, which allows secure access to the web application by using HTTPS. 

The example skips the [prerequisite steps](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis) for using the API to provision a VPC, subnets, and instances.
{: note}

To create a network load balancer by using the API, perform the following procedure:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with the right variables.

2. Store the following variables to be used in the API commands:

   * `ResourceGroupId` - First, get your resource group and then populate the variable:

    ```sh
    export ResourceGroupId=<your_resourcegroup_id>
    ```
    {: pre}

3. Create a load balancer with a listener, pool, and attached server instances (pool members)

  ```
  bash
  curl -H "Authorization: $iam_token" -X POST
  "$api_endpoint/v1/load_balancers?version=$api_version&generation=2" \
      -d '{
          "name": "example-balancer",
          "is_public": true,
          "profile": {
              "name": "network-fixed"
          },
          "listeners": [
              {
                  "certificate_instance": {
                      "crn": "crn:v1:staging:public:cloudcerts:us-south:a/123456:b8877ea4-b8eg-467e-912a-da1eb7f031cg:certificate:43219c4c97d013fb2a95b21dddde1234"
                  },
                  "port": 443,
                  "protocol": "tcp",
                  "default_pool": {
                      "name": "example-pool"
                  }
              }
          ],
          "pools": [
              {
                  "algorithm": "round_robin",
                  "health_monitor": {
                      "delay": 5,
                      "max_retries": 2,
                      "timeout": 2,
                      "type": "tcp",
                      "url_path": "/"
                  },
                  "name": "example-pool",
                  "protocol": "tcp",
                  "session_persistence": {
                      "cookie_name": "string",
                      "type": "source_ip"
                  },
                  "members": [
                      {
                          "port": 80,
                          "target": {
                              "address": "192.168.100.5"
                          },
                          "weight": 50
                      },
                      {
                          "port": 80,
                          "target": {
                              "address": "192.168.100.6"
                          },
                          "weight": 50
                      }
                  ]
              }
          ],
          "subnets": [
              {
                  "id": "7ec87131-1c7e-4990-b4f0-a26f2e61f98e"
              }
          ]
          }'
  ```
  {: codeblock}

  Sample output:

  ```
  {
      "created_at": "2018-07-12T23:17:07.5985381Z",
      "crn": "crn:v1:staging:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
      "hostname": "ac34687d.lb.appdomain.cloud",
      "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
      "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
      "is_public": true,
      "profile": {
          "name": "network-fixed",
          "family": "network"
      },
      "listeners": [
          {
              "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
              "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
          }
      ],
      "name": "example-balancer",
      "operating_status": "offline",
      "pools": [
          {
              "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
              "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
              "name": "example-pool"
          }
      ],
      "provisioning_status": "create_pending",
      "resource_group": {
          "id": "56969d60-43e9-465c-883c-b9f7363e78e8"
      },
      "subnets": [
          {
              "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
              "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
              "name": "example-subnet"
          }
      ]
  }
  ```
  {: screen}

  Save the ID of the load balancer to use in the next steps. For example, save it in the variable `lbid`.

  ```
  lbid=0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727
  ```

4. Get details about the load balancer

  ```
  curl -H "Authorization: $iam_token" -X GET "$api_endpoint/v1/load_balancers/$lbid?version=$api_version&generation=2"
  ```

  Allow some time for provisioning. When the load balancer is ready, it is set to `online` and `active` status, as shown in the following sample output:

  ```
  { 
    "id": "0738-dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "crn": "crn:v1:staging:public:is:us-south:a/123456::load-balancer:dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727",
    "name": "example-balancer",
    "created_at": "2018-07-13T22:22:24.489Z",
    "hostname": "dd754295-e9e0-4c9d-bf6c-58fbc59e5727.lb.appdomain.cloud",
    "is_public": true,
    "profile": {
          "name": "network-fixed",
          "family": "network"
    },
    "listeners": [
      {
        "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
         "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/listeners/70294e14-4e61-11e8-bcf4-0242ac110004"
      }
    ],
    "operating_status": "online",
    "pools": [
      {
        "id": "0738-70294e14-4e61-11e8-bcf4-0242ac110004",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/load_balancers/dd754295-e9e0-4c9d-bf6c-58fbc59e5727/pools/70294e14-4e61-11e8-bcf4-0242ac110004",
        "name": "example-pool"
      }
    ],
    "private_ips": [
      {
        "address": "192.168.10.5"
      },
      {
        "address": "192.168.10.6"
      }
    ],
    "provisioning_status": "active",
    "public_ips": [
      {
          "address": "169.11.111.115"
      },
      {
          "address": "169.11.111.116"
      }
    ],
    "resource_group": {
      "id": "0738-56969d60-43e9-465c-883c-b9f7363e78e8"
    },
    "subnets": [
      {
        "id": "0738-7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/subnets/7ec86020-1c6e-4889-b3f0-a15f2e50f87e",
        "name": "example-subnet"
      }
    ]
  }
  ```
  {: screen}