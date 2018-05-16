# Virtual Interfaces<a name="WorkingWithVirtualInterfaces"></a>

You must create a virtual interface to begin using your AWS Direct Connect connection\. You can create a private virtual interface to connect to your VPC, or you can create a public virtual interface to connect to AWS services that aren't in a VPC, such as Amazon S3 and Amazon Glacier\. You can configure multiple virtual interfaces on a single AWS Direct Connect connection\. For private virtual interfaces, you need one private virtual interface for each VPC to connect to from the AWS Direct Connect connection, or you need to use a Direct Connect gateway\. For more information, see [Direct Connect Gateways](https://docs.aws.amazon.com/directconnect/latest/UserGuide/direct-connect-gateways.html)\.

To connect to other AWS services using IPv6 addresses, check the service documentation to verify that IPv6 addressing is supported\.

We advertise appropriate Amazon prefixes to you so you can reach either your VPCs or other AWS services\. You can access all AWS prefixes through this connection; for example, Amazon EC2, Amazon S3, and Amazon\.com\. You do not have access to non\-Amazon prefixes\. For a current list of prefixes advertised by AWS, see [AWS IP Address Ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *Amazon Web Services General Reference*\.

**Note**  
We recommend that you use a firewall filter \(based on the source/destination address of packets\) to control traffic to and from some prefixes\. If you're using a prefix filter \(route map\), ensure that it accepts prefixes with an exact match or longer\. Prefixes advertised from AWS Direct Connect may be aggregated and may differ from the prefixes defined in your prefix filter\.

To use your AWS Direct Connect connection with another AWS account, you can create a hosted virtual interface for that account\. The owner of the other account must accept the hosted virtual interface to begin using it\. A hosted virtual interface works the same as a standard virtual interface and can connect to public resources or a VPC\.

A sub\-1G connection only supports one virtual interface\.


+ [Prerequisites for Virtual Interfaces](#vif-prerequisites)
+ [Creating a Virtual Interface](create-vif.md)
+ [Viewing Virtual Interface Details](viewvifdetails.md)
+ [Deleting a Virtual Interface](deletevif.md)
+ [Creating a Hosted Virtual Interface](createhostedvirtualinterface.md)
+ [Accepting a Hosted Virtual Interface](accepthostedvirtualinterface.md)
+ [Adding or Removing a BGP Peer](add-peer-to-vif.md)
+ [Associating a Virtual Interface with a Connection or LAG](associate-vif.md)

## Prerequisites for Virtual Interfaces<a name="vif-prerequisites"></a>

To create a virtual interface, you need the following information:

+ **Connection**: The AWS Direct Connect connection or link aggregation group \(LAG\) for which you are creating the virtual interface\.

+ **Virtual interface name**: A name for the virtual interface\.

+ **Virtual interface owner**: If you're creating the virtual interface for another account, you need the AWS account ID of the other account\.

+ \(Private virtual interface only\) **Connection to**: For connecting to a VPC in the same region, you need the virtual private gateway for your VPC\. The ASN for the Amazon side of the BGP session is inherited from the virtual private gateway\. When you create a virtual private gateway, you can specify your own private ASN\. Otherwise, Amazon provides a default ASN\. For more information, see [Create a Virtual Private Gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/SetUpVPNConnections.html#vpn-create-vpg ) in the *Amazon VPC User Guide*\. For connecting to a VPC through a Direct Connect gateway, you need the Direct Connect gateway\. For more information, see [Direct Connect Gateways](http://docs.aws.amazon.com/directconnect/latest/UserGuide/direct-connect-gateways.html)\.

+ **VLAN**: A unique virtual local area network \(VLAN\) tag that's not already in use on your connection\. The value must be between 1 and 4094 and must comply with the Ethernet 802\.1Q standard\. This tag is required for any traffic traversing the AWS Direct Connect connection\.

+ **Address family**: Whether the BGP peering session will be over IPv4 or IPv6\.

+ **Peer IP addresses**: A virtual interface can support a BGP peering session for IPv4, IPv6, or one of each \(dual\-stack\)\. You cannot create multiple BGP sessions for the same IP addressing family on the same virtual interface\. The IP address ranges are assigned to each end of the virtual interface for the BGP peering session\. 

  + IPv4: 

    + \(Public virtual interface only\) You must specify unique public IPv4 addresses \(/30\) that you own\.

    + \(Private virtual interface only\) Amazon can generate private IPv4 addresses for you\. If you specify your own, ensure that you specify private CIDRs for your router interface and the AWS Direct Connect interface only \(for example, do not specify other IP addresses from your local network\)\.

  + IPv6: Amazon automatically allocates you a /125 IPv6 CIDR\. You cannot specify your own peer IPv6 addresses\.

+ **BGP information**:

  + A public or private Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) for your side of the BGP session\. If you are using a public ASN, you must own it\. If you are using a private ASN, it must be in the 64512 to 65535 range\. Autonomous System \(AS\) prepending does not work if you use a private ASN for a public virtual interface\.

  + An MD5 BGP authentication key\. You can provide your own, or you can let Amazon generate one for you\.

+ \(Public virtual interface only\) **Prefixes you want to advertise**: Public IPv4 routes or IPv6 routes to advertise over BGP\. You must advertise at least one prefix using BGP, up to a maximum of 1,000 prefixes\.

  + IPv4: The IPv4 CIDR must not overlap with another public IPv4 CIDR announced via AWS Direct Connect\. If you do not own public IPv4 addresses, your network provider might be able to provide you with a public IPv4 CIDR\. If not, [contact AWS Support](https://aws.amazon.com/support/createCase) to request a /31 public IPv4 CIDR \(and provide a use case in your request\)\.

  + IPv6: Specify a prefix length of /64 or shorter\.
