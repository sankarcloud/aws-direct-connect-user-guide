# Routing Policies and BGP Communities<a name="routing-and-bgp"></a>

AWS Direct Connect applies inbound and outbound routing policies for a public AWS Direct Connect connection\. You can also make use of Border Gateway Protocol \(BGP\) community tags on advertised Amazon routes and apply BGP community tags on the routes you advertise to Amazon\.

## Routing Policies<a name="routing-policies"></a>

If you're using AWS Direct Connect to access public AWS services, you must specify the public IPv4 prefixes or IPv6 prefixes to advertise over BGP\. 

The following inbound routing policies apply:

+ You must own the public prefixes and they must be registered as such in the appropriate regional internet registry\.

+ Traffic must be destined to Amazon public prefixes\. Transitive routing between connections is not supported\.

+ AWS Direct Connect performs inbound packet filtering to validate that the source of the traffic originated from your advertised prefix\. 

The following outbound routing policies apply:

+ AS\_PATH is used to determine the routing path, and AWS Direct Connect is the preferred path for traffic sourced from Amazon\. Only public ASNs are used internally for route selection\.

+ AWS Direct Connect advertises all local and remote AWS Region prefixes where available and includes on\-net prefixes from other AWS non\-region points of presence \(PoP\) where available; for example, CloudFront and Route 53\.

+ AWS Direct Connect advertises prefixes with a minimum path length of 3\.

+ AWS Direct Connect advertises all public prefixes with the well\-known `NO_EXPORT` BGP community\.

+ If you have multiple AWS Direct Connect connections, you can adjust the load\-sharing of inbound traffic by advertising prefixes with similar path attributes\.

+ The prefixes advertised by AWS Direct Connect must not be advertised beyond the network boundaries of your connection; for example, these prefixes must not be included in any public internet routing table\.

## BGP Communities<a name="bgp-communities"></a>

AWS Direct Connect supports a range of BGP community tags to help control the scope \(regional or global\) of traffic\.

### Scope BGP Communities<a name="scope-bgp-communities"></a>

You can apply BGP community tags on the public prefixes you advertise to Amazon to indicate how far to propagate your prefixes in the Amazon network—for the local AWS Region only, all regions within a continent, or all public regions\.

You can use the following BGP communities for your prefixes:

+ `7224:9100`—Local AWS Region

+ `7224:9200`—All AWS regions for a continent \(for example, North America–wide\)

+ `7224:9300`—Global \(all public AWS Regions\)

The communities `7224:1` – `7224:65535` are reserved by AWS Direct Connect\.

In addition, the well\-known `NO_EXPORT` BGP community is supported for both public and private virtual interfaces\.

AWS Direct Connect also provides BGP community tags on advertised Amazon routes\. If you're using AWS Direct Connect to access public AWS services, this enables you to create filters based on these community tags\. 

AWS Direct Connect applies the following BGP communities to its advertised routes:

+ `7224:8100`—Routes that originate from the same AWS Region in which the AWS Direct Connect point of presence is associated\.

+ `7224:8200`—Routes that originate from the same continent with which the AWS Direct Connect point of presence is associated\.

+ No tag—Global \(all public AWS Regions\)\.

Communities that are not supported for an AWS Direct Connect public connection are removed\.