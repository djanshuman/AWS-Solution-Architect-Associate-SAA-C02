# AWS-Solution-Architect-Associate-SAA-C02
Learning Notes on AWS

1. S3 and IAM are GLobal and not region scoped.

2. AMI are built for specific region. They are locked for your account/region.
AMI takes space and live in S3
AMI created from snapshot

Difference between COPY AMI and launching instance from AMI
Only after provision to EBS/snapshot a shared account can copy AMI to different or same region. Else they need to launch
an EC2 Instance from AMI then create their own AMI

3. Placement group: Cluster (low latency hardware setup, single AZ,same rack), spread( 7 instance per AZ), partition (instance spread
among different groups/racks with an AZ,can launch 100 EC2 per group)

Cluster: great network 10gbps , High bandwith, high througput , low latency . Rack fails all fails. risk

Spread: span across multiple AZ's, ec2 instances are on different physical hardware, 7 instance per az.
reduced risk of failure. if one instance fails with an AZ other works. ( MULTIPLE AZ , each AZ contains max 7)

Partition: Each AZ is divided into many partition/racks group. Each partition group has EC2 instances upto 100. 
There can be max 7 partition group per AZ. Failure is not shared across group. data should be properly
distributed across many groups to be highly available. (ONE AZ, EACH AZ can have 7 partition grouP and each group max 100ec2's)
EC2 instances can get partition info as metadata. 



Horizontal Scaling: Load Balancer and Auto scaling
High availability: Load Balancer with multi AZ, Auto scaling with multi AZ


ELB: --> redirect traffic to multiple ec2 instance or many target groups containing multiple ec2's associated with a health check.
Enable stickiness
SSL termination  for websites-- > when a client talks to elb , elb creates a new connection with ec2 this is called
connection termination. So ec2 sees private ip of elb and not client.
Client ip is present in header(x-forwarded-for), port in (x-forwarded-port), protocol in (x-forwarded-proto)


separate public from private traffic
expose single point of DNS to application

3 types of LB:
1. Classic LB 2. Application LB 3. Network LB
health Check : Based on health check it determines whether instance is healthy or not. Status 200 is ok.
check is done on port and route(/health)

LB is used for below balancing:

1. balancing based on route in URL
2. Balancing based on hostname in URL
3. Multiple applications running on same machine
4. Multiple hTTP applications running across machine (target groups)

has port mapping feature--> any dynamic port to redirect same  instance to same machine
 
ALB(layer 7) --> advanced and new support http/https / websocket
SSL termination  for websites-- > when a client talks to elb , elb creates a new connection with ec2 this is called
connection termination. So ec2 sees private ip of elb and not client.

Network ELB(layer 4):--> old support tcp+rules Only
high performance then ALB
can see client IP no x-forwarded-for

4** errors are client side error and 5** errors are application induced error.

Difference betwen ALB and NLB -> ALB is called and resolved by DNS but NLP is static IP or elastic ip per AZ


AutoScaling
==========

ASG default termination policy : 

1.find az having least number of instance
2.Then to choose from instance having multiple launch config choose oldest one.

EBS
1. Locked to az
2. Network drive and can be plugged in to any instance to have persist data.
3. Provisioned capacity in gbs and IOPS.

4 types ebs volumes:
1. GP2 (SSD)-> general purpose SSD volume balances price and performance.
2. IOI (SSD)-> High in performance for critical application having low latency or high throughput.
3. STI (HDD)-> low cost HDD for frequently accessed throughput intensive workloads
4. SCI (HDD)-> low cost HDD for less frequently accessed.

GP2 and IOI are boot volumes
we can mount and unmount drives on ec2 instance.
/etc/fstab for mounting whenever we launch an ec2.

IOI -> ratio of IOPS to volume size is 50:1 
GP2 having 16000 IOPS beyond that till 64000 IOPS we can use IOI.

Snapshot lifecycle manager-> automate backups for creating snapshots from EBS volume
Snapshots are incremental backups for changed volume
can launch new volumes, create image , copy snapshot to other region

EBS  migration-> create snapshots from ebs volume.
Create volume from snapshots in different region.

EBS encryption-> 
1.create a snapshot from uncrypted one.
2. Create a volume from new snapshot and choose encrypt volumne
3. Now the volume is created with encryption,


Instance store are ephemeral store. 

Cloud characteristic:

One demand Self-Service
broad network access
Resource pooling
Rapid	 elasticity
Measure usage


Infrastructure Stack			
application							
data 
Runtime
container
OS
Virtualization
servers
Infrastructure
Facilities

IAAS
application
data 
Runtime
container
OS --------------> Your unit of consumption
Virtualization
servers
Infrastructure
Facilities

PAAS
application
data 
Runtime	 --------------> Your unit of consumption
container
OS
Virtualization
servers
Infrastructure
Facilities


SAAS
application --------------> Your unit of consumption
data 
Runtime	 
container
OS
Virtualization
servers
Infrastructure
Facilities

There are FAAS, DAAS etc.



172.31.0.0/16  --> Default VPC CIDR range

First 4 and last 1 IP are reserved by AWS. So no of IP is always 5 less.

/20-> 4091 IP
/16- > 65531 IP
/17 -> 32763 IP

2**N => 32-16=16 -> 2^16= 65536 IP
2**N => 32-17=15 -> 2^15= 32768 IP
Which means two /17 CIDR can fit into one /16 CIDR range


In default VPC No of subnet=No of AZ in a region
Default VPC are configured to have public IP version 4 address (IPV4) public address to instances. 
IGW, 
SG, NACL , Route table comes configured.

EC2 instance composed of CPU, MEMORY , DISK and networking.

connecting ec2
===============

AMI - Type RDP TCP protocol port 3389 
Linux AMI type SSH TCP protocol port 22
Remote desktop Conn --3389 windows
SSH --22 Linux

IAM Access key: Each access key has two parts : Access_key_id + Secret_access_key
Access_key_id is public and aws has access to it whereas secret_access_key is private part.


Keypair: Every keypair has two parts public key & private key. private key is what we download
and use it while connecting. Public key is accessible by AWS and it places in EC2 instances.
When we ssh it authenticate our private key with public key and connection is established.

.ppm -> linux or any macos
.ppk -> only for putty

EC2 is Iaas cloud model.
Instance status check and system status check both should pass for ec2.
2/2 is healthy
Instance status check - OS is healthy and can accept traffic
System status check - HOST, hardware is reachable and packets are delivered so traffic can be
reachable.

S3: Infinitely scalable storage system.
Object has key and value. Key is name of object and value =content being stored.
The size of value can be 0-5TB. 
every object has metadata,version ID, access control , subresources.




DNS
===

DNS (domain naming system) is a directory.

1.Client sends a request for a domain name (www.amazon.com)
2.The request goes to the DNS resolver which is sitting in your router or Internet service provider.
3.The resolver queries on your behalf to the FIND THE NAMESERVER(DNS NAME SERVER) WHICH CONTAINS THE
ZONE FILE IN THE ZONE. DNS IS distributed global resilent database (DNS) to search for the
zone file IN THE ZONE which contains the DNS record.
4. The zone FILE has the DNS record which is hosted by Name space server which we call as NS lookup.
5. Once the resolver finds the zone file it sends the IP address mapped to the domain name.
6. DNS CLient receives the ack'd request with IP address to query to and finally redirected to the
domain name server www.amazon.com

DNS CLIENT -> DNS RESOLVER -> DNS (DB) -> ZONE (REGION OR PART LIVE INSIDE DNS) -> ZONEFILE(PHYSICAL DB OF THE ZONE)-> NAMESERVER(WHERE ZONEFILES ARE HOSTED)


CLIENT
	\
DNS RESOLVER \
			 \
			www-> 192.19.89.12    | -> NAMESPACE (NS)

			ZONE FILE

DNS Root is the starting point of any website IP search
www.amazon.com.

1. DNS CLIENT asks for the IP add of the domain name to dns resolver which is hosted in your router
or ISP .
THE ISP/router gives the root hints file which contains the DNS ROOT servers(13) address to search or lookup
for the domain name.
3. The DNS ROOT is a database hosted by DNS ROOT Servers(13) maintained by 12 global companies.
4. The ROOT ZONE is managed by IANA.
5. The root servers gives the dns record which is mapped to the domain name in dns root.
6. DNS client gets back the address and proceeds to the URL.

DNS is a system of trusts so at this point we only trust dns root zone.
DNS CLIENT LOOK UP->ROOT ZONE IN A ROOT IANA

The root zone and root are the database of TLD (Top level domains) of the route in dns name.
.com. ( left of .)
.com and .org are genetic TLD's as they are mapped to specific country.

Process:

The User types www.amazon.com. in the device and that request go to the
DNS resolver server present in the router or the ISP site location.
Now the resolver has the root hints file which has all the details of root zone and DNS root servers(13).
The request is deligated to root zone now.
The root zone is a Database which contains list of gTLD's and ccTLD's and they are designated to a vendor or service.
For example .com-> verizon.
The root zone doesnt not contain the dns record for www.amazon.com but it has list of NS authorative for .com domain.

Now .root zone contains set of NAME Servers(NS) for .. IP and server details for all .com zone.
The request can be deligated to any of the NS and next proceed.
As the request reached the root zone, it will check for .com in root zone and give any of the name server for .com.

Next the request is routed to .com ZONE which is trusted by root zone.
.COM zone has many domain names while registration happens in internet. amazon.com, netflix.com etc.. So .com zone
searches its directory and tells I dont have exact IP domain mapping but have entries for
NS which are authorative for www.amazon.com . It returns those servers.

The resolver communicates with one of the servers authoritative for amazon.com
So the request is now routed to amazon.com zone trusted and deli-gated by .com zone.

Now amazon.com zone gets the request and searches its directory. If the matches happens
for a DNS record mapping then the corresponding IP is returned the DNS resolver and back to DNS client.


Route 53:
=========

Route 53 is an AWS managed DNS product.

1. Domain registration
2. Hosted zones with managed name servers.
3. Global service , single database.
4. It is globally resilience. Region resilience. The DNS records/data it stores is globally available
so can tolerate failure of multiple zones.

1. Domain registration:

Route 53 has relationship with domain registries. Each of the registries manage one zone . For example .org, .com, .io.
They are deli-gated by IANA to do so.

Now Route 53 checks with the PIR which manages .org domain to check whether domain is available or not under .org zone.
If the domain is available then route53 creates the zone file for this domain . The zone file is nothing but database containing DNS information.
Apart from creating zone file it also chooses few managed name servers usually 4 which are global to host this zone file.
Now the zone file is hosted in 4 managed server. 
Then route 53 makes an entry (in .org TLD in .org zone file in .org registry managed by PIR) and maps the domain name in .org zone file to authorative our name-servers for the domain name.
It means it add the nameservers under .org zone to add our managed servers list to the domain name registered.

Simple workflow
===============

a. Creating a zone file.
b. Creating a number of managed name servers called hosted zone in AWS.
c. Putting the zone file in these name servers.
d. leasing with the registries and TLD to add the nameserver registries and entries to point back to our managed servers for choosen domain.

DNS IS JUST A SYSTEM OF DELIGATION. IT JUST DELIGATES TO THE CHAIN OR TREE OF NEXT LEVEL ZONES AND AUTHORIZES THEM.
OFTEN KNOWN AS WALKING THE TREE FROM ROOT ZONE.

1. route 53 creates ZONE files.
2. It manages four or more managed nameservers called Hosted zones.
3. Copy the zone files(database) inside this NS.
4. the hosted zones can be public if it present in AWS public zone. Can be private if it is present in aws private zone
and connected to a VPC(s).
5. The records in hosted zones are usually referred as record set in DNS.

In .org registry in .org zone file it adds the Name servers for our new domain. These Name servers are aws managed hosted zones.

Aws also have created .org zone file and copied the zone file to the managed name servers. Now those NS list are added in TLd'S .org zone file to point AWS
managed NS for resolving domain lookup.

You can also Create a domain outside AWS then create separate hosted zone in AWS and edit it to point the NS servers given by aws
to the outside domain name.

DNS record types:

Type of record set:

1.Nameserver (NS): DNS Record type is how the root zone delegates control of .org to the .org registry

Basically everything in DNS working through deligation of NS.
Root zone contains NS records which deligates to Nameserver containing .com zone. --> .com NS example.dns.net
Then .com zone contain NS record set which deligate to amazon.com nameserver. --> .amazon.com NS example.dns.co.uk
Records in amazon.com zone  which points to domain_name -> NS in AWS  ( www.amazon.com -> 198.234.23.10)

2. A and AAAA records: Host to IP mapping.
Here A record set is for IPV4 mapping and AAAA is for IPV6 mapping.
so we have to make two entry so that when DNS client by user queries it can get the required matched value.

A www -> 172.161.89.32 (version 4 ,v4)
AAAA www -> 2309:3433:6803:2534::2004 ( version 6,v6)

3. CNAME records (canonical name) : Host to host . CNAME just points to another name . The another name then points to some IP. 

					A server -> 172.217.25.36 (v4)   -> if we change IP then it will change for all 3 CNAME once. So less overhead.

CNAME www ----------/
CNAME ftp----------/
CNAME mail --------/

4. MX Records: It is used to find a mail server for a domain to connect using SMTP protocol.
it goes to domain zone and finds the entries 
MX 10 mail
MX 20 mail.other.domain.com

A mail ->192.34.19.20

Lower value for priority and only mail and no . on right means same zone.
. to the right means can be of same zone as fully qualified name or different service.
Whichever matches now connect to the mail server using smtp protocol. Client does MX query and get the IP/server address to connect and send mail.

5. TXT records : basically it adds a text to the domain name in the zone. It is used to prove domain ownership or ensure trusted by matching text we added
with domain names and querying server 

So that if we want to add it to a mail hosting zone.
It will query the text for the domain name from our hosted zone and if text matches it will prove our domain ownership.
 
TTL :
===
DNS TTL is value can be set in DNS records.
Walking the tree to get an authorative answer from authorative trusted zone is time taking.
The authorative answer is source of truth.
Often the authorative zone adds a ttl value to the domain name and it gets cached in the ISP dns resolver.
So that when more clients query looking for same domain names it gives the same answer until ceases to exist.
Now this is an non-authorative answer .
TTL must be balanced and set up to ensure not much delay in service /delivery in case dns changes.
So ensure we have lower TTL values. 

organisation maintains the zones for a TLD (e.g .ORG) - REGISTRY
type of organisation has relationships with the .org TLD zone manager allowing domain registration -> REGISTRAR 

IAM :
====
IAM are set of policies or rules which allow/deny any identities for connecting or using any AWS resource/products.
It has identity policies in form of JSON documents.
The identities needs to prove who they are using process of authentication. After the authentication the identity is known as authenticated user.
Now the policy has a set of statements which tells what action an identity can perform on which resources. It can have multiple resources and blocks.

Flow of access pattern:
If a policy has explicit deny on a resource and then has an explicit  allow on those set of same resources or super-set.
The deny always wins.

If no deny and no allow is there explicitly then it is implicit deny always except root AWS account.
New account has no access and all implicit deny.

So if there is an explicit allow it comes to effect only iff there is no explicit deny for it.
Be it users policy, group policy and resource policy all are collected and evaluated together. If there is any explicit deny for 
a resource then its final then explicit deny overtakes all 

Explicit DENY -> Explicit Allow -> Implicit Deny

Types of policy:
================
1. Inline policy : These policies are unique for each identity. so for 100 users we have 100 Inline identies.
2. Managed policy: They are created as their own object. They are common and re-usable. We create managed policy ones
and attach it to n number of users.
benefit of managed policy :
a. reusable.
b. low management overhead.

Usage:
Inline policy are for special permission or exception,restriction for any resources in AWS given to particular user on top of managed policy or separate.
Managed policy are common set of rules(normal operational business rights) ,common access rights or working, access for AWS resources that you want to attach to set of users or group.

Managed policy two types : AWS managed policy and customer managed policy.
We created customer managed policy if AWS managed policy doesn't satisfy our needs and requirement.

IAM users:
IAM Users are identities that are issued to human, application and service accounts which require long term aws access.

PRINCIPAL : Principal can be an user , service or application that tries to prove its identify using authentication in order to connect to aws resource.
As part of authentication, IAM checks whether the user is actual correct user or not what he claims to be using two process Username /password or secret access keys.
During the process of authorization if the user is logged in then IAM matches the set of policy and deny/allow rules that are applied on the user.
based on that it can start interacting with AWS.

ARN (Amazon Resource Name): Uniquely identify resources within any AWS account.

Format:
arn:partition:service:region:account-id:resource-id
arn:partition:service:region:account-id:resource-type/resource-id
arn:partition:service:region:account-id:resource-type:resource-id

arn:aws:s3:::catgifs  --> Bucket level
arn:aws:s3:::catgifs/* --> Wildcard char to specify collection of keys inside a bucket

arn:aws:cloudformation:us-east-1:395080422158:stack/IAMUserDemo/495becc0-ed4c-11ea-b8da-12a417a66525


RULES:
1. 5000 IAM Users per account and each IAM user can be added to max 10 groups.
2. Internet scalable application with millions of user base can't have 5k IAM users but the solution is to have Federated Identiy /IAM roles as fix
3. 300 IAM GROUPS PER ACCOUNT. THIS CAN BE INCREASED WITH SUPPORT TICKET.

IAM ROLES
=========

IAM roles are basically given to multiple principals. They can be IAM users in same account/other accounts, EC2 machines, application etc.
Usually IAM roles are given when short-term credentials are required unlike IAM user where long term AWS access was required.

IAM roles have two part 
1. Trust Policy
2. Permissions Policy

Trust policy define the Principals: users, machines, application who can assume a role to use AWS resources.
If authenticated then they will assume a role to use AWS resources.
Permission Policy define the AWS resources which can be used by identities mentioned in Trusted policy only.

Now when user/EC2 /application assume a role they are given a Temporary security credential by an AWS service called STS (secure token service) ie. sts:AssumeRole
Credentials for a short term to use the AWS services as if they are real AWS user identities.
Its kind of borrowing the role for a short time. If permission policy is modified then STS permission is also changed.

When to use IAM roles:

NOTE: If you are unsure of the number of principal/users/multiple users to be used for a service then ideal scenario of assuming a role.
Anything that needs to execute on your behalf requires sts:AssumeRole.


LAMBDA example:

Applications / services/product running in AWS when need to execute certain operations on our behalf or IAM user behalf they assume a role.
So in Lambda function we create a LambdaExecutionRule which has a trust policy that trust AWS lambda to assume the role for function invocation.
It also has the permission policy which list the resources that the aws lambda can interact using the assume role. 

Now the aws lambda assume the sts:AssumeRole and get temporary credentials from STS and invokes function in its run time environment.
The runtime environment basically assumes the role and invoke function as a service to interact with cloudwatch/s3/sqs etc aws product and services.
Also the resource policy must allow aws lambda usage.

Need of using role is for accessing the service. If we dont assume role then we have to hardcode our secret access keys in code to allow them getting authenticated
as behave as an identified entity in AWS. Hardcoding is never an option so we assume role which is for short time. The role are issues temporary creds by sts
and post completion of task discarded.

a. Security risk
b. Issues when we change/rotate our access keys

When EXTERNAL accounts or identities want to use AWS resouces they have to use a AWS role to interact and use AWS products and services.
As they are external they can't directly login  to AWS to use AWS products. The external identities can login using federated identifies
and each organisation can have 5000+ employees but our hard limit of iam account is 5000 in that case we cant have each IAM user. So
we can reuse their org identities or federated login to authenticate and generate STS token to login in AWS and use service assuming temporary roles.

Other scenario is web identify federation. Suppose a riding sharing app wants to connect to store retrieve data from amazon DynamoDB.
Here we have scaled to millions of users and cant have each IAM user account. We can use web federated identities to login in AWS.
We trust this federated identities and allow them in role trust policy and assign temporary token from STS to use aws.
Benefit:
1. Reusing existing accounts.
2. Can scale to millions of customers.
3. No AWS credentials in App

AWS Organisation
================

AWS org starts with a master account who creates an organisation. It is the master account.
We send invites to other AWS account to be part of our created organisation. Once they accept the invite they gets associated with our org accounts
and become member accounts. With Organization roots it represented as inverted tree.


ORGANIZATION ROOT ( Root can be AWS master account,member account of OU. Simply they are container to start with.)
|
|
--------------ORGANIZATION UNITS ( can have many OU's based on org hierarchy)
					|		|
				Member account	Member account / Master account

Enable Consolidated billing of all member accounts in master account. Now master account only pays for entire org as whole which removes
financial overhead of maintaining each AWS member billing accounts.
Also benefits in consolidation of reservations and volume discounts.

Service Control Policies:
SCP are jSON based policy documents which can be applied to organisation root, organisation UNIT(OU) or independent AWS Org member accounts.
The master account is special and unaffected by any SCP's.

1. SCPs are account permission boundaries.
2. They limit what the account(including root) can do.
3. They don't grant any permission . They just limit the account wide usage and control what is allowed and what not. May be restricting usage to a region only
or particular EC2 instance only etc.

SCP by themselves dont grant permissions.

By default when SCP is enabled in the Root , it attaches a FullAWSAccess policy which means all actions are allowed on all AWS resource.
But if we detach or dont have the FullAWSAccess policy then SCP has implicit deny on everything.
So if you dont have FullAWSAccess policy then implicit deny will apply.

This gives us the Deny list architecture. That means we have FullAWSAccess policy applied to our org and only thing we can do is to create separate denypolicy
to restrict something.
In order to have the Allow list architecture we have to remove the FullAWSAccess policy which lead to implicit deny by default by SCPs.
Now we can have explicit AllowList to allow only required services and actions on them.

Only those policy which are defined to a account in identity policies in accounts 
and same policy can be allowed by SCP's and it is common area.
However if a policy to use certain resources is not attached to a Identity policy document but
SCP allow the usage then also it cant access as it is not allowed in Identity policy which means account by default doesn't own rights 
to hold that usage.

CloudWatch logs:

1. CloudWatch logs are public service which are usable from AWS or on-premises.
2. The end points to which application connect to is hosted at AWS public zone.
3. We can use the CloudWatch within VPC or from on-premises env or other cloud vendors if AWS permissions and network connectivity exist.
4. Store , monitor and access logging data.
5. AWS Intergration  : Ec2, vpc flow logs, Cloudtrail , route53,lambda. All the integration services can store logs inside CloudWatch provided
necessary IAM roles exist.
6. OutSide AWS applications or product (out of AWS ) for loggin in metrics or data require unified CloudWatch agent to be installed.

Two ways : Either the resource or aws product can directly connect to CloudWatch or unified CloudWatch agent.
third way is using SDK .

Flow:
Logging sources -> Log events -> Log streams -> Log group(container for streams)
1. Configuration are set up in group level for example retention of logs etc.
2. Metric filter are setup on log streams to filter for any particular events on the streams of data which increments metric count and also
can raise a alarm.

Cloudtrail:
1. Cloudtrail is used to track API related calls and account related events for traceability.
2. It can be used for security diagnostics at account level.
3. It is enabled by default and can store 90 days of free event history.

Events can be :
1. Management events - Management plane events like interacting with AWS products and service only. ec2 instance start/stop, s3 bucket creation/deletion etc.
2. Data events - Are action on the resources of AWS products. They are heavy and comes with extra cost.

Cloud trails can be setup to fetch trail data from single region or all regions.
In case of all regions, the resources which are region based only log the events in which they are operating. Whereas global region services
like sts,IAM, cloudfront by default logs to us-east-1 . So trails must be configured to capture events from them.
we can store these events history in cloudwatch logs and s3 bucket.
By default only management logs are enabled.
Not realtime logging. Some delay and lagging. It logs like twice in an hour or 15mins gaps.
Recent introductions are AWS organisation trails which can logs multi-account data in an organisation to a trail .
it eases a with a single point to look after for a master account to trace activities.


S3 Resource policy and ACL's:

1. S3 is private by default and only root user has got access.
2. Resource policy are attached to bucket.
3. Identity policy are attached to individual identities/account in aws. So for separate account
you have to attach new identity policy. Cant control all account access together.
Whereas in resource policy we directly attach policy to bucket and it is applicable to all account/specific account
whoever is trying to access the bucket.
4. Allow/deny for same or different account.
5. Allow/deny for anonymous principals.
6. Difference of Resource policy is present of Principal in the policy.
The principal in the resource policy defines the accounts/identities who are affected by the resource policy or for whom this is defined.
Whereas identity policy didn't have principal because logically principal is the identity alone to which identity is applied.
By definition Identity policy is applied to you so you are the principal.

Note : If the policy has a "principal" mentioned then that is mostly a resource policy.
"principal" : * -> It applied to all accounts or principals trying to access the bucket.

3 types of identities mostly we can define for resource bucket policy
1. Same identity who owns the bucket.
2. Partner account/other account
3. anonymous account who are not authenticated by AWS.

For an AWS identity it is a combination of Identity policy + bucket policy for the effect to take place
Whereas for an anonymous principal it is only the resource policy which is applied as it dont have identity policy 

For Cross account access:
1. The cross account identity policy must allow s3 access.
2. Your bucket policy must allow access for external account access.
3. Combination of Their Identity policy + your bucket policy

S3 ACL's: They are legacy and not recommended or used anymore. They are less flexible and only gives certain operations
to be used.
1. READ
2. WRITE
3. READ_ACP
4. WRITE_ACP
5. FULL_CONTROL

These permissions have to be applied on Bucket and object separately. So less flexible and not used .
Bucket policy has replaced them.

S3 Static website hosting:
1. AWS CLI and UI use AWS API's to make list/get object call assuming we are authenticated or authorised to do so.
2. Normal Access is via AWS API's.
3. This feature allow access via a standard HTTP protocol.
4. we have to provide index and error documents. we have to set it.
5. Website endpoint is created by AWS which makes it available for HTML and http access.
6. We dont get to choose the endpoint name . It is generally based on bucket name and region based.
7. Bucket name matters for endpoint. so have the bucket name same as domain name.

benefits:
a. Offloading : If we are hosting a website using compute service suppose ec2. Then the static images can be stored in s3 bucket. The html pages
when gets loaded in customer website can point to images in s3. So cheaper and s3 is designed for scalable storage.
b. Out-of-band pages : If the server or ec2 itself is out of order for maintenance the error page hosted on server also
gone . customer wont be re-directed to a page which shows downtime. In that case we can host such pages in s3 and route it through route 53 
so point DNS to s3 website showing downtime page.

Object versioning:
1.New bucket starts with versioning disabled.
2. Bucket can be moved from enabled to disabled but can't move back to disabled.
3. Can be moved to suspending from enabled.

disable -> enable-> suspended

When versioning is disabled the ID is null for a object key. 
On enabling versioning and uploading new copies id is allocated. On deleting a object the delete marker is associated with that latest id and it is hidden
We can delete a delete marker to show up the object.

MFA delete: If MFA delete is enabled then for changing versioning state of deleting a versioned object we require MFA token.
MFA delete is enabled in versioning configuration.
Serial number MFA + code passed with API CALLS

Note : when we delete the latest version it adds a delete marker. But if we delete the delete marker version then it will enable the
version again.
But if we delete the versioning objects then it will permanently delete the object we cant undelete that.

S3 performance optimization:

Single PUT upload:
a. It uses a single data stream to transfer to bucket.
b. Due to network speed and bandwidth if the upload fails in midway the entire data is wasted.
c. Stream fails , upload fails and require full restart.

We can leverage s3 multipart upload where the data is broken dard to parallel streams.
The data can be broken down to max 10000 parts & each part can be from 5mb-5gb
total network transfer speed is the sum of all transfers.
The lowest threshold of data size for enabling multipart is 100mb.

s3 transfer acceleration: Using s3 transfer acceleration we can transfer data using public network to nearest aws endpoints.
From endpoints data is transferring to direct s3 bucket using aws global network or backbone. 
The network is aws private and superfast not affected by public network latency.

Encryption:
1. Symmetric key: Same key is used for encryption and decryption of data. Suitable for single party of storage and no transit.
2. asymmetric key: Both public and private keys are generated. Public key is used to encrypt plain-text to ciphertext.
Private key is used to decrypt the cipher text to plaintext by the receiver having private key.

Plain-text + key +Algorithm = Cipher-text.
Cipher-text+key+ sameAlgorithm= Plaintext

Plain-text+public-key+Algorithm=cipher-text
Cipher-text+private-key+ SameAlgorithm=plain-text.
Here as the public key is available anyone can encrypt any plain-text and send to receiver.
The receiver will use his private key to decrypt.

Signing: The issue with above method was the ackdment. How will the sender understand that the receiver had received the encrypted text.
Anyone can reply with with encrypting any data.
So in signing the receiver sign the text using private key and send back to sender.the sender uses the receiver public key to match
and confirm that it is send by the receiver.

KMS:
1. Regional and public-service.
2. Used to create, store and manage keys.
3. Supports both symmetric and asymmetric key.
4. Keys never leave KMS and they are isolated to a region. It uses FIPS 140-2 validated hardware security modules to isolate and protect your keys.

KMS is used to create CMK( Customer master key)
1. CMK is logical . It has ID , date , desc, policy & state.
2. It is backed by a physical key.
3. CMK is generated and can also be imported from your own key management infra.
4. It can be used to encrypt only 4KB of data.
5. CMK never leave KMS and they are isolated to a region.

Data Encryption Key ( DEKs)

1. Data encryption keys are generated using CMK.
2. CMK generates data encryption key using generatedataencryption key operation.
3. It can be used to encrypt data > 4KB.
4. As CMK generate DEK's so each DEK is associated/linked with CMK and CMK can tell which DEK it has generated.
5. But CMK discards DEK once generated. It does not store them and perform encryption. its only task is to generate them.
6. User can perform encryption on their side or services using kms to generate dek are responsible for encryption work.

The DEK gives you two versions:
1. Plain-text version.
2. Cipher-text version

You use the plain-text version of key + plain-text to be encrypted to generated cipher-text data + cipher-text key ie. both encrypted data+ encrypted key.
Then you discards the plain-text version of key. 
The encrypted key + data is stored side by side(Stored encrypted key with data) which makes admin easier.So you always have right key to use.
KMS doesn't track the usage of the DEK's. 


In future when you want to decrypt the data you have to pass the encrypted DEK to the KMS. KMS finds the CMK that was used to
perform encryption and then decrypts it and send back the decrypted key to you. Now you can use the decrypted key to decrypt your data.
Then discard the decrypted data encryption key. 
S3 uses kms to generate DEK for every object then discards the plain-text version.

Points:
1. CMK are isolated to region and never leave.
2. CMK are two types : Customer Managed CMK and AWS managed CMK.
3. Customer managed CMK are more flexible and configurable.
3. CMK's support key rotation. So the physical backing key material used for cryptographic encryption is changed.
In AWS managed they are rotated every 3 years and can't be disabled. Whereas in customer managed key it is optional and if enabled done once in a year.
They also store backing key + all previous key.
4. They support alias for a key for a region. Suppose an alias used myapp for an application will point to a CMK for that region.
On rotation we just changed the backend key bt alias remain unchanged for the application. Again all are region scoped.
For every region you have to change and manage that. Neither of them are global.

Key policy:

Just like other resource policy in AWS , we have got Key policy for every CMK.
Every CMK has a policy and in case of customer managed CMK we have to manage it.
Basically we have to tell KMS that the "key trust the account they are in" so they can manage it.
Key trust the account , IAM trust the account then key trust the account. It is all chain.
So if an account is having suitable IAM policy and IAM user and it has CMK policy attached to it then it can perform operations.
We have role separation where one account is responsible for a certain operations and not all.
One can create keys but not encrypt or decrypt key. It is done by someone else. 
benefit of KMS is to separate role.

DEMO for CMK:

# Linux/macOS commands

aws kms encrypt \
    --key-id alias/catrobot \
    --plaintext fileb://battleplans.txt \
    --output text \
    --query CiphertextBlob \
    --profile iamadmin-general | base64 \
    --decode > not_battleplans.enc 
    
aws kms decrypt \
    --ciphertext-blob fileb://not_battleplans.enc \
    --output text \
    --profile iamadmin-general \
    --query Plaintext | base64 --decode > decryptedplans.txt
    
S3 Encryption:
Encryption are defined at object level and not bucket level.

Two main methods of encryption supported by S3:
1. Client side encryption 
2. Server side encryption.
Both are encryption at rest. This is the method of determining how objects are stored persistently on disk.


Client-Side Encryption
SSE-C
SSE-S3
SSE-KMS

Client Side encryption:

	   In-Transit(HTTPS)			    Data at Rest
User ===========================S3 =====================Storage
	 Scrambled encrypted data       Scrambled encrypted data 
	 
In client side encryption the data is encrypted before leaving the user side to tunnel for s3 storage.
The entire data is in scrambled cipher text form through out the tunnel. On reaching s3 it is stored in rest in storage. Here s3 never get to see the plaintext 
format. S3 is only responsible for storage and nothing else.
Client manages:
1. Keys and Encryption operations
2. Process
3. Tooling

Server side encryption:

	   In-Transit(HTTPS)			    			Data at Rest
User ========================================S3===========================Storage
	    Plain-text form of data       			Scrambled encrypted data 
from outside the tunnel is encrypted as 
		HTTPS protocol is used..

Here data in transit is in real plain-text format. On reaching s3 it is encrypted with keys and put in storage.

Different types of Server side encryption are :
1. SSE-C - Server side encryption with customer provided keys.
2. SSE-S3 - Server side encryption with Amazon S3-Managed keys.
3. SSE-KMS - Server side encryption with Customer Master key (CMK) stored in AWS Key management service (KMS).


SSE-C [Server side encryption with customer provided keys]
=====
1. Client manages Keys.
2. S3 Manages the cryptographic operations of encryption and decryption.
3. Client sends the Plaintext(DATA) + KEY to S3 in HTTPS protocol (IN-transit) to S3 Endpoint. The data reach the s3 endpoint and then hash of key is taken.
The original key is discarded. The encryption operations take place using algo then Encrypted data(cipher-text form) + hash(#) of key
is stored persistently on disk.
4. For retrieving the data the client sends the request along with key. The hash of key is taken and discarded. Then it compares the hash along with the stored hash
. If matches it sends back the data in plain-text format.

The process is benefit for client in terms of overhead of CPU processing Unit requirements as the S3 manages the cryptographic operations. Imagine
millions of records and performing encryptions in user side is costly and not feasible.
Cost is on client side as it has to manage the key. But you have control in your side.
So depends on scenario or use case of operations.

	   In-Transit(HTTPS)			    			Data at Rest
User ========================================S3===========================Storage
	    Plain-text form of data + KEY     			#Key + Scrambled encrypted data 
from outside the tunnel is encrypted as 
		HTTPS protocol is used..



SSE-S3 [Server side encryption with Amazon S3 Managed Keys]

In this process the client put the data in s3. s3 manages the key management and cryptographic operations of encryption and decryption.
S3 has a master key for all objects uploads.
It generates key for each object and uses the key to encrypt the data. Then the master key encrypt the generated key.
The encrypted key + encrypted data is stored at rest in storage.
No control or little control of keys by client. No rotation of keys are managed by us so not better for role separation scenario.
Entire overhead is managed by s3.
Note: It uses best Algorithm i.e AES-256

Not suitable for below requirement scenario:
1. Control keys + key material access.
2. Role separation.
3. Key rotation

SSE-KMS [Server side encryption with Customer Master key (CMK) stored in AWS Key management service (KMS)]
1. The S3-master key in SSE-S3 is replaced by Customer Master Key (CMK) which is AWS managed CMK or Customer managed CMK.
2. The CMK is generated using AWS KMS service.
3. The CMK is then used by KMS to generated a Data Encryption Key (DEK) for each object being uploaded in s3.
So the plaintext is uploaded by client. CMK generates a DEK . DEK encrypts the plaintext to cipher text. CMK encrypts DEK.
Now both encrypted DEK  + cipher-text data is stored in s3 storage. plain-text DEK is discarded.
On decryption request CMK decrypts the DEK . DEK then decrypts the data. The plain-text key is discarded.
Data sent to client.

Benefit: You have fine grained control ok keys, key rotation and role-separation.

Bucket default encryption: Uses the default encryption by passing header : x-amz-server-side-encryption 
the value if passes aes:256 means ss3-s3 , if passed aws:kms then SSE-KMS
if the object level encryption is not provided the default bucket level is taken as priority.
We can also specify in bucket policy to restrict upload if header is not passed for a particular type of encryption.

S3 Storage class:
1. S3 Standard: 
Default storage 
11'9 durability and 4 9's availability.
When object is uploaded it is stored atleast 3 AZ.
It is region resilience so can tolerate failure of a AZ. Data is safe across other AZ.
Low latency , high throughput.

2. S3 Standard-IA
a. Less frequent but rapid access.
b. Suitable for archival or disaster copy kind of data when you need access the data often.
When you access it should be available and rapid. It is 54% cheaper then s3:standard.
c. Charged for minimum 128KB.
d. 30 days charged even if 1 day storage.
e. per gb retrieval fee
f. 99.9% availability.

note: 54%-30 days-128kb-99.9-perGB retrieval fee
Designed for immediate access for less frequently accessed data.

3. S3 One ZONE-IA
a. Same tradeoff as IA
b. 80% base cost of standard.
c. One zone storage , No 3+ zone multi az.
d. 99.5 % availability.
e. Can't withstand AZ failure.

Note: Suitable for only data which is re-creatable. Secondary copy of primary data, secondary copy of backup's.

All three are milli-second byte latency means their first byte latency is measured in milli-second. first byte is available in milli-second

4. Amazon S3 Glacier

a. Best for storage of archival data.
b. cost is 17% of base cost of standard.
c. 11 9's durability and 4 9's avail.
d. 40KB minimum and 90 days storage.
e. Retrieval in minutes or hours.

Types of retrieval: 
1. Expedite : 1 to 5 minutes ( <250mb)
2. Standard : 3 to 5 hrs
3. Bulk : 5 to 12 hrs
1 min -> 12 hrs

17%->40kb-90days-same as Standard for AD-retrieval options

5. Amazon S3 Glacier deep Archive
a. replacement to tape storage and designed for backups.
b. cost is 4.3 % of s3-standard
c. 40KB and 180 days
d. much slower retrieval minimum 12 hrs. Bulk retrieval 48 hrs
e. can't make data public or manual download

Both glaciers can't make storage public.Cannot be downloaded.You cannot edit permission or property.
Data has to be restored few hours in advance before making in available in realtime.
no realtime retrieval.

Note:
Immediate access scenario : No glaciers.
Within few minutes  : glacier 
long term storage and hours of retrieval, cheap storage: glacier archive.


S3 Intelligent tiering: 
It automates moving of objects between s3 standard or s3-IA based on their access patterns.
When frequency or use is uncertain then it is the best choice.


S3 Lifecycle policy:
These are set of rules containing set of actions applied on objects or bucket to set transition or expiration Configuration
to move between tiers.
Expiration and transition actions can be applied to both current and previous versions.
with expiration all the older versions can be purged after a specific days.

Transition actions move objects between storage tiers based on rules of days. They are very useful for managing cost by automating the
management.

S3 Replication:
They are of two types:
1. CRR : cross region Replication. Source and destination in different region.
2. SRR : Same region replication. Source and destination in same region.

S3 replication configuration is applied to source bucket.
Role is created which trust the IAM for replication.

If cross across along with IAM ROLE + Configuration, bucket policy is also required in destination bucket
to allow the role to write it to destination bucket.
All the replication is SSL encrypted. Secure.

Replication options:
1. We can choose what to replicate - Particular prefix, object , bucket or filter prefix, tags.
2. By default during replication the Storage class remains same as source.
3. By default the replicated objects owner is the source account. But we can change that for destination account to read keys.
4. Replication time control(RTC): 15 days SLA for data to be in sync across both source and destination system.

Important:
1. Versioning has to be enabled for both source and destination account.
2. One way replication.
3. No deletes, system events, glacier and glacier deep archive are replicated.
4. Can replicate unencrypted, SS3-S3, SSE-kMS(extra config) objects.
5. Source bucket owner should have permission to objects.

SRR benefits:
1. Log aggregation.
2. Prod and test syncup
3. Resilience with strict sovereignty

CRR:
1. global resilience.
2. latency reduction for remote workers.

Pre-signed URL : it is used to generate for a URL for an object stored inside a private bucket.
The URL is passed on to the requestor and it expires after a certain time. The user access the object using URL
and it behaves as if he is the iam user. through the uRL the iam user gives his identity to the requestor for viewing the object.
Can be used for put and get operations.

Pre-signed url when invoked are matched against the current permission of the identity who generated it.
If the permission of the IAM user is changed and then URL is also affected and no access might come.
IAM Role should never be used for generation as IAM role is short-lived and expire sooner than the URL .

IAM user should be specifically created for the application using signed URL and given to users.

S3 Select and Glacier select:
Improves performance. Allow to filter or select required objects in the S3 itself before dats is retrieved by the application.
400% faster and 80% cheaper. Only required data is retrieved as filter point is on s3 whereas in normal application without s3 Select
data is retrieved and selected from source and on app-server side filtering is done so unwanted data is discarded but s3 charges completely.
So we are billed for full transit cost and per gb retrieval.

The data received is pre-filtered and sql like expression is used.


Networking / VPC
================

Private IP's:
10.0.0.0 - 10.255.255.255 (class A)
172.16.0.0 - 172.31.255.255 (16 class B)
192.168.0.0 - 192.168.255.255 (256 class c)

CIDR:
10.0.0.0/16 -> /16 is the size of network

10.0.0.0 - 10.0.255.255 ->/16
No of address : 32-16= 16 => 2**16= 65536 IP address
/17= 32-17= 32768 IP which is half of /16
so two /17 network makes one /16 network.

10.0 -> network part
0.0 -> 255.255 -> host part.

10.0.0.0 -> 10.0.255.255 ---/16 network so post split into two network:
10.0.0.0 -> 10.0.128.0
10.0.127.255 -> 10.0.255.255

Note: Bigger prefix smaller network. Smaller prefix bigger network.

0.0.0.0/0 means all IP address

10.0.0.0/8 means last 3 bit can change   16 million IP
10.0.0.0/16 means last 2 bit can change  65536 IP
10.0.0.0/24 means last 1 bit can change	 256 IP
10.0.0.0/32 means only 1 IP    

IPV4 32 bit 
IPV6 128 bit
::/0 All ipv6

VPC considerations:
1. what size should vpc be ?
2. There should be no overlapping of CIDR for cloud, on-premises, partners and vendors network.
3. VPC structure : Tiers and resiliency zones.

VPC minimum range /28 (16 IP) and maximum range /16 (65536 IP)
1. No of AZ= no of subnet
if we take the maximum allowed IP /16 means 65536 IP
so for 4 subnet we require= 65536/4= 16384 per subnet IP range
So  by calculating to achieve 16384 IP we can get using 2**14 = 32-14= /18
So 4 /18 IP subnets


Custom VPC:
1. Regional service - All AZ's in the region.
2. It is used to create isolated network with no permission for IN and OUT of network.
We have to explicitly configure it.
3. We can have hybrid networking VPC i.e part of network on-primises + cloud.
4. We can choose Default or dedicated tenancy.
5. VPC gets IPV4 CIDR Blocks and public IPs.
6. 1 primary private IPv4 CIDR block and optional secondary ipv4 blocks.
7. optional single assigned IPV6/56 CIDR block.

DNS in VPC:
1. Provided by route53.
2. provided by reserved private IP ( base_ip + 2) IP address.
3. enableDnsHostnames  -> gives instance DNS names ( enable instances with public IP address in VPC gives public DNS hostnames)
4. enableDnsSupport -> Enable DNS resolution in VPC (enable dns in vpc or not)
5. Both must be set to true to enable dns fix and setup 

Subnets:
1. Subnets are AZ resilient. 
2. If AZ fails subnet including all other components fails. 
3. 1 Subnet=1 AZ. 1 AZ can be 0+ more subnets.
4. Each subnet is assigned a IPV4 cidr block which is a subset of VPC cidr.
5. The CIDR across all subnets must not overlap and subnet can communicate with each other in a network.
6. IPV6 Cidr with /64 or /56 can be allocated to a subnet 

5 Reserved IP within each subnet:

Every VPC has 5 reserved IP address which is not allowed for custom use.
example : 10.16.16.0/20

1. 1st IP is blocked for network address - 10.16.16.0 
2. 2nd IP is blocked for network router address - 10.16.16.1 
3. 3rd IP is blocked for DNS resolution - 10.16.16.2
4. 4th IP is reserved for future use - 10.16.16.3
5. last IP is reserved for broadcast address 10.16.31.255 (last IP). Broadcast is not supported by VPC so IP is blocked.

Minus 5 IP usable.

DHCP options set: They are define for a vpc and this is how computing device receives IP address automatically.
1 DHCP options set applied to one VPC. we can create new DHCP options set but can't edit them.
Create new VPC and assign the allocation to this DHCP options set.

Also few other setup provided are
1. Auto-assign IPV4 address: automatically allocates a ipv4 public address to a subnet.
2. Auto-assign IPV6 address: automatically allocates a ipv6 public address to a subnet.
Both are subnet level and flow to resources defined within a subnet.


VPC Router:
Every VPC has a router. It moves traffic from somewhere to somewhere else.
Every subnet can have 1 route table but 1 route table can be associated with many subnets.

The route table destination can be within the VPC default ipv4 cidr or ipv6 cidr or both if two address are enabled.
It controls the routing of traffic of data where it goes when it leaves the subnet.
The target can be local or igw or other specific ip address.
It matches the destination IP and forwards to target if matches.
higher prefix more priority.

Internet gateway (IGW)
1. It is region resilient gateway attached to a VPC.
2. 1 VPC = 0 or 1 IGW , 1 IGW= 0, 1 VPC
3. 	IGW is hosted or runs from AWS public zone. It acts a gateway between aws private zone 
to connect to public internet or aws public zone service ( sqs,sns, s3).
4. It is managed aws service so handles performance.


IMPORTANT: IPV4 is not configured with public ip address inside OS. It is just a mapping in IGW for private IP to public IP.
Inside OS it can only sees private ip.
IGW maintains a record of mapping of ec2 instance private ip to the public IP.

Working of IPV4 address with IGw.
1. Suppose a ec2 instance wants to update the http server and need to send a packet to www.
2. It will create a internet packet: Packet consist of source IP (private IP address), destination Ip(apache server) 
3. Now it will be default route to IGW. Here IGW wont expose the private IP to outside world instead it will refer to the
record mapping and translates/modify source IP to public ipv4 address and sends to server.
4. The apache server when sends the request back it has the packet destination as public ipv4 address of igw.
5. IGW modifies it to private ip of ec2 instance and route it.

No where OS sees the public ipv4 address it is not configured anywhere it only sees private ipv4 address.
In case of ipv6 address they are already publicly routable so no translation happens via igw.

Bastion host/jump boxes:
1. They are instance running in public subnet which allows entry point to private secured VPC.
2. They are useful in incoming management connections and can be used to ssh to private instance.
3. private instance can receive incoming ssh from this bastion hosts.


NACL:

NACL are subnet level firewall. They are stateless. They sees request and initiation as different.
Only impacts when data crossing subnet border.
NACL can be used to explicitly allow and deny whereas security had no allow and deny.
NACL is never attached to any aws resources it is always attached the subnet.
NACL has no logical resource . IP,network,port,protocol.
one subnet=1 nacl at a time

Security group:
They are stateful so sees the initiation and request as one request.
if traffic is allowed then response is allowed .
SG cannot have explicit deny and they can refer aws logical resource and refer other security groups.
Sg is used as default where subnet is not involved. SG is used in conjuction with NACL for restricting specific traffic or deny.

NACL is used only for explicit deny cases 
NACL is used when subnet is associated and sg dont work with the products.


NAT gateway:
1. NAT stands for Network address translation.
2. It performs a set of processes ie. mapping private ips to public ips ( remapping SRC to DST IP)
3. It gives private resources or machines hosted in private zone of aws outgoing public internet.
4. It also performs IP masquerading : Taking a bunch of private ips and mapping them to a single ipv4 public ip.
5. NAT is associated with an elastic IPs (Static IPv4 public)
6. AZ resilient service i.e if AZ fails then NAT also affects can recover later but if az lost then nat lost.
it is good practice to have a separate route table + NAT for each AZ for disaster. Route table in each AZ with NAT as target.
7. it is aws managed and can scales 45 GBPS. Charged for both processing data volumne + duration.


NAT Architecture:
1. When a set of EC2 machines or instance which are hosted in aws private zone /tiers requires to connect to public www or
aws public zone it can be achieved through NAT gateway. 
2. NAT gateway is hosted in public zone and has got a source IP. It has an external facing ipv4 public address.
3. The route table of private zone instance now routes traffic to nat gateway for public outgoing request.
4. NAT gateway receives the packet having the source IP : private IP of machine , dest_IP : of Public www. 
It maintains a translation table where it keeps the record of IP,port,request all details then modified the packet to update
source IP from private IP to its own source IP.
5. The nat gateway then forwards the packet to internet-gateway which is routed by VPC route table.
The igw updates the received packet source address to nat-gateway public ipv4 address and sends to www.
On receiving it updates the destination address to nat-gateway address and nat-gatway updates to private IPS.

Basically the source IP of NAT is public ipv4 but more specific to aws inside zone. So what igw does it translates the nat ipv4 public
ip to a real public IP version 4 address. NAT does the translation of taking a bunch of private ips and mapping to a single public ip.


NAT Instance
============

1.NAT Instance= single Ec2 instance.
2.to enable it we have to disable the Source/Destination checks.
3. Can do port forwarding
4. Can use NACL to control the traffic coming to and from the subnet where NAT Ins resides.
5. Can be used as a bastion hosts.
6. Security group can be attached the nat instance as well as resources behind nat instance to control inbound and outbound traffic.


Note: Security group cannot be associated with a NAT gateway.
port forwarding, bastion hosts can be done via nat gateway.

Note:
1. NAT is not required for IPV6 address.
2. Its primary goal is to provide ipv4 private address public internet.
3. All IPV6 address in AWS are publicly routable. IGW works with all IPV6 IP's directly.
4. ::/0 + IGW = IPV6 public www


EC2:

1. Ec2 instances are virtual machines (OS+ allocation of resources)
2. They are running on Ec2 hosts. These hosts can be shared or dedicated.
3. EC2 are AZ resilient. 1Host=1AZ, If AZ fails then hosts fail and ec2 instance also fails.
4. They do have local storage(instance store), some CPU+memory+disk and network usage.
5. EC2 is completely AZ specific. 
6. They have instance store which is physical storage, EBS volume which is also per AZ. So if AZ is affected then
also of these get affected. Hardware, Networking ,Storage are all locked inside specific AZ.

EC2 Hosts are physical Hosts which run in a single AZ --> Ec2 machines which are running
in this AZ or inside subnets are logical V-machines which are mapped or running on top of this hosts.

Instance store are physical local storage device attached to a host. Ec2 machines running on hosts utilise the instance store
for storage. Once the instance is lost then storage is lost.

Two types of networking: 
1. Storage networking: EC2 instance can have remote storage. They are provisioned EBS volumes.These volumes are allocated on a persistant
storage. But must be present in same az of ec2 instance

2. Data networking : When an EC2 Instance is launched in an subnet, ENI (Elastic Network Interphase) is provisioned
in a subnet and they map the ec2 instance to the physical hardware 	on the ec2 hosts. ENI is attached to EC2.
Ec2 instance can have multiple ENI only iff they are in same az. ENI are Primary network interface.
One or many ENI can be attached to a Single EC2 but same az.

Interface ID eni-00ecaf0f7c4dc477f
VPC ID vpc-058cda024d985462b
Attachment Owner 395080422158


Note:
1. EC2 is suitable for traditional OS + Application Computer service.
2. Ec2 is best for long running workloads and long compute needs.
3. If you application is running on virtual servers can be migrated to ec2 for disaster recovery.
4. For steady state workload ,server style applications or if you need occasional burst then ec2 is good
5. In AWS default compute option is ec2. 

Ec2 Categories:
1. General purpose : diverse workload
2. Compute optimized : HPC, ML , gaming
3. Memory optimized : database workload, in memory large dataset processing
4. storage optimized : random & sequential IO, DW, elastic search , analytic workloads
5. Accelerated computing : hardware GPU ,field programmable gate arrays (FPGA)

Instance type:
R5dn.8xlarge 
|___|
R type instance family of 5 generation. 8 times xtra large size of memory or size. 
it has micro, mini,large, x-large options in a family of instance.

1. General Purpose :  T types (T2,T3) , M Type(M5,M6),A Type(A1)  -> MAT
2. Compute optimized : C type(C5) --> C type
3. Storage optimized : I type IOPS(I3), D Type(data warehouse, distributed, hadoop) , H Type[H1](high througput, HDFS,Kafka) -->HID
4. Memory optimized : R type(R5), X type(x1) , Z type(z1), U type(u-xtb1)  -> ZUXR

5. Accelerated computing: P type, G type , F type, INF1  -> PGF-INF1

Storage Key terms:
1. Direct (local) attached storage: Instance store. They are directly attached to the ec2 hosts. ( Ephemeral storage: temporary)
2. Network attached storage: EBS volumes. Volumes allocated over network. They are remote storage ( persistent storage , permanent)

3. Block storage: Mountable and bootable. Blocks are allocated to OS over network.
4. File storage : File share has structure. Mountable and not bootable.
5. Object storage(S3): Not mountable or bootable. Highly scalable.

Storage performance is highly impacted by three factors: IO (block size) + IOPS(No of read write per second) + Throughput(Speed of data WRITE
per second)

IO * IOPS = Througput

IO = Size of storage blocks. For example 4 KB, 16 KB, 1MEG
IOPS= no of read and write per second , 100 IOPS for example
Throughput = Write done at xx mb/sec in disk , for example 16 * 100 IOPS = 1.6 MB/S

But it is not necessarily mean increasing IO will increase IOPS for a machine. There are Cap on number 
of throughput can be achieved for a storage type. So we have to choose based on resources.


Elastic Block Store (EBS):

1. EBS is used to allocate block storage (volumes)to instance. The block can be of different size (IO). EBS are network drives.
2. EC2 instances are backed by EBS and they are restriced to AZ's. However they are highly available and resilient
as they replicated inside AZ. Only if entire AZ fails they we lost ebs volumes.
3. Different physical storage options disk(SSD/HDD)
4. Based on the drive performance vary(IOPS/throughput)
5. They are billed in month , GB/month
6. Since they are network drives they can be detach from one instance and attached to other. so those volumes
survives past the lifetime of an instance.

Different types of Volume types:
Volume types are categorised into SSD based and HDD based.

SSD based:
1. GP2 (General purpose SSD)
2. Provisioned IOPS SSD(Io1)

HDD based:
1. Throughput Optimized	HDD(st1)
2. Cold HDD (sc1)

Dominant performance attribute: 
IOPS requirement : SSD
Throughput requirement : HDD (MiB/s)


SSD's:

GP2 Storage: ( 1 volume : 1 instance)
=============

1. GP2 storage are default and good for regular workloads.
2. Its architecture is based on Volume performance bucket.
3. Whenever a volume is provisioned a volume performance bucket is allocated to it of size 5.4 million burst credits.
As the volume starts consuming the IOPS the bucket replenish. The replenishment rate depends on the size of volume.

So whatever be the volume size initially you have got bucket of contents 5.4 million burst credits.
You can consume all at once or gradually. once burst credits is empties it fills based on burst line performance.


It has got 3IOPS/GB ( min:100, max:16000 IOPS) , 3000 IOPS is base line performance
Calculation:

You will always get 100 minimum IOPS even if your size if less <= 33 GB ( 33*3=99)
33 GB= IOPS 100 / 3000 (Baseline of 3 IOPS per GiB with a minimum of 100 IOPS, burstable to 3000 IOPS)

Once the volume size cross 33 gb you start getting IOPS on the baseline calculation:
34 GB=  IOPS102/3000 (Baseline of 3 IOPS per GiB with a minimum of 100 IOPS, burstable to 3000 IOPS)

So till your volume size is less then 1000 GB you can burst till 3000 IOPS.
Once it crosses 1000GB you get 1000*3=3000 IOPS which is base line performance and no more burst is available now.
Calculation goes till 5333GB where max IOPS of 16000 is achieved.
5334 GB= IOPS 16000 (16000 IOPS for volume sizes greater than 5333 GiB)
IOPS wont be incremented any further thats the limitation irrespective of volume size.

1. max Volume size : 1 GB to 16 TB ( 16384 GiB in size)
2. max IOPS per vol: min 100 to 16000 
3. max Throughput : 250 MiB/s

4.max IOPS per instance : 80000 (Nitro instance)
5.max Throughput per instance : 2375 Mb/s(Nitro instance)

Usage: Great for bootable volumes and dev/test.

Provisioned IOPS SSD (Io1): Now has multi-attach feature. Ie. Io1 can be attached to multi instance.

1. Capability to separate IOPS from Volume size.
2. You get IOPS in ratio of 50:1 ie. 50 iops per GB. Maximum ratio of 50:1 is permitted between IOPS and volume size.
3. Minimum volume size is 4 gb to 16tb 
4. Suitable for IOPS intensive workload where heavy iops ,throughput requirement and low latency.
for small volume more iops can be provisioned. 

Calculation:
4Gb= 200 IOPS ( 4*50)
100 GB= 5000 IOPS 
Highest is 64000 IOPS for 16tb


1. max Volume size : 4 GB to 16 TB ( 16384 GiB in size)
2. max IOPS per vol: 64000 (IOPS independent)
3. max Throughput per volume : 1000 MiB/s
4.max IOPS per instance : 80000 (Nitro instance)
5.max Throughput per instance : 2375 Mb/s(Nitro instance)

Usage: Suitable for IOPS intensive workload where heavy iops ,throughput requirement and low latency.Cassandra, MongoDB


HDD: 

1. Suitable for sequential access not random access. Writing sequentially and reading sequentially
big blocks of data. Streaming data, big data, log analytics 
2. Both are great value and throughput based and not IOPS.
3. Cannot be used for boot volume.
4. (Min: 500 GiB, Max: 16384 GiB)


Throughput optimised st1:

Throughput (MB/s) 500/500(Baseline: 40 MB/s per TiB) 
Burst: 250MB/s per TiB

500:40:250

Cold HDD sc1:

Throughput (MB/s) 192/250 (Baseline: 12 MB/s per TiB) 
Burst: 80MB/s per TiB

250:12:80


Instance store volumes:

1. Block storage device.
2. They are physically attached to EC2 host.
3. Instances launched on that host can use them. No modification can be done to instance store
post launch.
4. High performance in throughput and storage as physically attached, whereas EBS were network attached.
5. They are included in instance price.
6. They are Ephemeral storage and temporary in nature . Not persistant. an instance having stored
data in instance store will lose data if the instance is stopped and restarted. On
restart it will be assigned a new ephemeral store with new storage.
Old ins-store will be wiped up and will be allocated to new instance hosted in that host.

EBS vs Instance store:

If the requirement is not satisfied by EBS then go for instance store
Any requirement for 80000 IOPS 2375 MB/s throughput then Instance store.
EBS is great for boot volume, persistant storage, high availability,resilient across AZ.
and can be multi-attach across instances to form clusters.
instance store is for high performance Iops and throughput anything which is temporary Storage and can risk persistant.
Value for money as they are included in Cost. Stateless service. 
One instance store=1 host .

EBS snapshots:
1. They are Incremental volume copy or backups of EBS volume in S3.
2. The first snapshot is the full data copy of the EBS volume. The next snapshot is the incremental copy of the
changed volume or data out of original storage or volume. We are only billed for the consumption of volume in snapshot
ie. how much copy of data is occupied in a snapshot and not the total volume of EBS storage.

In EBS volume we were billed for the volume allocation ie. if we are allocated 40GB and we use 10GB still then we are charged
GiB/month for 40GB, whereas in snapshots we are billed for 10gb as that is original size of data.
further snapshots only capture the change data by comparing old snapshots and current state of snapshot. For old data
they hold a reference to old snapshots and for new change data they store it.
Snapshots are stored in S3 so Region resilient Whereas EBS where AZ resilient. Snapshot can be move to AZ and volume
can be created from them. They can be copied to other region too.

They restore lazily. We can forced pull the data from s3 if we require quick initiation or restoration .
The blocks will be fetched immediately as forced read happens.
Fast Snapshot restore(FSR): Immediate restore. They are 50 per region on combination of Snap & AZ.
if you create 4 snap in a AZ then it is 46 left now total for that region.

EBS Encryption:

1. EBS is a network drive which is remote storage for ec2. it can be attached
to ec2 as ebs volume for block allocation of storage.
2. When an EC2 instance is created it is present on ec2 host and ebs volume is attached to it.
If we wish to encrypt the volume then during creating we can choose to encrypt volume.

Process of encryption:
1. For Encryption AWS use KMS. We can use AWS managed CMK (aws/ebs) or customer managed CMK.
2. CMK generated DataEncryptionKey(DEK) is used to encrypt the volume and stored with EBS volume
in physical storage. 
3. If any ec2 instance is attached to it. The KMS decrypts the DEK and store the plain-text decrypted key
in physical ec2 hosts on which ec2 machine is running.
4. Whenever ec2 machine had to read data or write data to volumne it will use the decrypted key for
operations.
5. if the instance is stopped or terminated or moved to different host then the decrypted key is discarded.
6. The encrypted key + volume can be mounted for another ec2 machine in host.
7. If snapshots are taken from the encrypted volume then snapshots are also encrypted with the key.
if volume is created from that snapshot then volume is also in encrypted form from that snapshot.
The dek is physically present with volume.

Important: The encryption has no impact on performance of OS. The encryption AES-256 is between 
ec2 host and volume and OS is not aware of encryption. 
There is an option of fulldisk encryption which has cpu performance penalty. We can do full disk
encryption with encrypted volume or plain volume.

Note: we can't un-encrypt or remove encryption option from an already encrypted volume or snapshot.
However only thing we can change is master key during launching volume from a snapshot.

We also have an options in settings to apply default encryption to all new EBS volume.
And use default encryption key.

ENI (Elastic Network Interface)
1.Each Ec2 instance can have 
a. One primary ENI
b. one secondary ENI
Each ENI has following attributes:
1.MAC address
2. 1 private ipv4 address
3. 0 or more secondary private ipv4 address
4. 1 public ipv4 address
5. 1 elastic IP per private ipV4.
6. Security group
7. Source/Destination check disable to use it as NAT instance.
8. DNS hostname for private primary IP
9. DNS hostname for public ipv4 address which resolve to private IP inside VPC
and public IP outside.

10. When an elastic IP get attached to a ec2 instance, the public ipv4 address is removed.
the static elastic ip is attached. There can be total of 5 static elastic IP
On removal of elastic IP new public ipv4 address gets allocated to ec2.

Note:
1. MAC address are static in nature. So as we can use secondary eni for attaching to different ec2 instance.
So the mac_adress also goes with it. So the licensing used with it also floats
ENI+MAC = licensing
2. Each ENI has different security group.
3. Internal to OS only known private IP.
4. Public ipv4 address resolves to private ip internally. Public ipv4 are dynamic , changes start and stop.
5. As we can have multiple secondary ENI so we can place it in different subnet with different rules of security groups.


AMI (Amazon machine Image) {baked by ebs snapshots}
1. AMI are images which is used to launch ec2 instances from it.
2. Each AMI has an AMI ID and they are region based.
3. AMI can be aws based or community based with distributions.
4. AMi's are also available in marketplace with applications stack installed and various custom AMI and flavours.
5. AMI can have permission for Public, Your account, Other AwS ACCOUNTS.

LIFECYCLE OF AMI:
1. LAUNCH: In this phase AMI is used to launch ec2 instance. The ec2 instance has root volumne and ebs volume attached to it.
the boot volume is /dev/xvda and ebs volume is /dev/xvdf. 
2. Config: Once Instance are launched in above phase now we can customize our AMI to install and setup our
application suite.
3. Create Image: Now to implement automation or reduce overhead of redundant time installations, we create an image out
of the the customize Ec2 instance which we can copy to same region, other region, same az to ease installations.
Whice creating the IMAGE AMI from EC2 instances . The AMI is just a container with no real data in it.
while image creation it creates snapshots from the block of device attached in EC2 machines.
It creates a block-device mapping file and put it in AMI. The block device mapping contains the snapshots ID both
Full snapshot+Incremental snapshot ID mapped to its corresponding Root Volume + EBS data volume Device ID's as reference.
4. Launch: Now when we create a real ec2 machine from the AMI image created, it creates an exact same structure of
volumes attached to ec2 using the block-device mapping. So it launches new AMI's with exact same data and volumes.

Note:
1. AMI = 1 region.
2. AMI can be copied to other region or AZ (includes its snapshots).
3. AMI are un-editable. you can launch An instance from AMi, update configuration, create image and re-use new instance
4. AMi baking : Creating image from configured instance + application.
5. Default permission= your account.

Instance Billing method:
1. On-Demand instance
2. Spot instance
3. Reserved Instance
4. Dedicated hosts

On-Demand: 
1. Short term, Spiky, Unpredictable workloads,performance, can't tolerate disruption.
2. Instances have Hourly-hate.
3. No long term commitments or upfront payment. 
4. Billed in seconds(60s minimum) or hourly.

Spot Instance:
1. 90% cheaper then On-Demand.
2. Spare Machines by AWS are offered having a spot price.
3. You set your maximum price and bid for it. As long as the spot price is less than your bid price
you get to use the resources.
4. Instance gets terminated once the spot price goes up
5. suitable for workloads which can be recreated and can tolerate failure.
6.Suitable for use case where application can withstand sudden termination/disruption.

Reserved Instances:
1. Offered at 75% discount vs On-Demand with commitment. 
2. They are reserved for 1-3years with All upfront, Partial Upfront,No-upfront.
3. They are reserved for steady workloads which requires always up and running and
known in advance.
a. Reserved Instances
b. Scheduled Reserved Instances.
c. Convertible reserved Instances.

4. Reserved in Region or AZ with capacity reservation.
5. Need reserved capacity.


Instance Status Checks( 2/2):
1. System Status checks. : Anything related to AWS Side. ec2 Host issue, loss of networking, power, 
connectivity, hardware,sotware.

2. Instance status checks : Related to EC2 like corrupt file system, OS kernel issues etc.
All should pass for an EC2 Instance.

We can set status checks alarm based on cloudwatch. it will send notification to sns topic.
we can recover instance, reboot, stop & terminate.
Auto recovery - which moves entire instance and launch it in new host with exact same configuration.
Automatically recovers fully from any staus check issues.
Works only with EBS volume attached instance not instance store.


Horizontal and Vertical Scaling
=============================

Scaling is the response to the increasing load to the application.

V-Scaling
=========
1. Vertical scaling is resizing of Ec2 instance based on increasing load on the application
running on Ec2 machine.
2. Vertical scaling can cause disruption as each re-size requires instance reboot.
3. larger instance will have a larger premium and there is a performance cap as we go up
in sizes. 
4. Benefits: No application modification is required. Works for all even monolithic application
where single application code base is running on single machine.

H-Scaling
=========
1. We increase the number of instance running an application and each small compute instance
must work together and share the incoming load.

2. So for redirecting the load and incoming request we require a load-balancer. When customer interacts with
the balancer , the load gets redistributed across the backend machines in a fair amount of share.
So each request might go to same machine or different machine.

3.Horizontal scaling is great but Sessions are key. it requires application support or off-host sessions.
4.Off-host sessions are when you require an external application to store your session data. So
there is no disruption on existing instances when an request/application interacts with machine.
5.It is advisable to add smaller instances as it adds granularity and there is no limit
to scaling. 
6. less expensive as no large instance premium.

Instance metadata
=================
1. Ec2 service provides data to instances.
2. The Ec2 service is accessible inside all instances.
3. http:169.254.269.254/latest/meta-data/
4. It has information about networking, authentication and user-data.
5. The metadata is not encrypted and not authenticated. The metadata helps the instance to learn about themselves.
6. The ec2-metadata services gives application the awareness of the environment they are running in.

Login to ec2 instance
execute ifconfig
execute curl http://169.254.169.254/latest/meta-data/

execute curl http://169.254.169.254/latest/meta-data/public-ipv4
execute curl http://169.254.169.254/latest/meta-data/public-hostname

Ec2-Metadata tool:

$ wget http://s3.amazonaws.com/ec2metadata/ec2-metadata
$ls -l ec2-metadata
	-rwxr-xr-x 1 root root 10912 2008-10-23 19:07 ec2-metadata
$ chmod u+x ec2-metadata

$ ec2-metadata --help

	$ec2-metadata -a
	ami-id: ami-xxxxxxx

./ec2-metadata-z
./ec2-metadata-s 

Containers and ECS
==================

In virtualisation, base is the hardware on top of that hypervisor runs.
on top of hypervisor lot of OS+ environment runs as virtual machines and each are isolated from each other.
However duplication happens as same OS might be running as individual VM's with different OS+environ.

OS takes up majority of memory

In Containerization we have the base as hardware and on top of that Guest OS runs.
container engine runs on top of OS and it has multiple container processes running in.
Each container process is one individual applications with runtime environments.
And each container processes can be sub processes. 

EC2 machines are basically isolated VMs running copy of ebs volumes. EBS volumnes might have guest os.
Docker image are created from base image or scratch.
docker image is made up of different stacks or layers/FS layers and image is built from docker file.
each line of docker file creates a filesystem layer. 

1. Images contain read-only layers. Any changes are layered onto the image
as a differential architecture. i.e any changes between old and new layers
are written as new layer to the image.

OS -> Container Engine -> Docker container+ -> Docker Image+

So Docker images are built from docker files. Each docker image is built of many FS layers.
The Docker container containing the image consist of extra 1 layer called as R/W layer.
We can create many images of same docker image launched in separate container. However the common
stacks is remain same and shared whereas the difference is the R/W layers

Container registry
==================
A docker container registry consist of docker images. The images are dumped into docker hub containers.
Whenever we want to mount an image we simply mount the docker images to docker hosts.

A docker host is one type of container host and docker hub is type of container hub.

1.Portable and consistency are main benefits of containerized application.
we can create images of our application, library files . Then make a docker container.
Now can mount the container and image any docker host. It will behave same in all environment.
2. Since each fs layers are shared in containerized app, so many containers running same images share the
base fs layers. This leads to resource and capacity save.
3. They are lightweight and use parent os for heavy task.

4. We can have multi-container architecture in an application which consist of various tiers.
5. If you dont need a full fledged application then container is best option.

ECS (Elastic container service)

ECS is managed container-based compute service.
ECS lets you create a cluster. It runs in two modes : EC2 and Fargate.

ECS cluster lets you run the container images. But in AWS We can store images in ECR (Elastic container registry)
which is aws product and works fine. To tell cluster about images
and where the image is located we have two definitions:
1.Container definition
2. Task definition.

1. Container definition simply is the reference to the image and port details in the ecr . It defines which ports
are exposed from that container.
2. Task definition is rest all details of the image and application resource such as memory, network , the task mode
whether ec2 or fargate.It contains single container definition or multiple container definition.

3. A task definition also stores the task role which is the IAM role that the containers inside a task cane 
assume to interace with AWS resources.
4.A task role assumed by task basically gives container in ECS the permission to access aws products and services.
5.A task can include one or more containers. A task definition creation in UI also created a container definition.

Service definition: A service definition is the wrapper around task definition.
It lets you scale, provide high availability and resilience. We can run multiple task copy of an application
but the way we scale and availability is all managed by service definition.
We create cluster and We deploy tasks in that cluster.
note : how many copies, ha, restarts, monitoring:Service definition.

ECS Modes:
1. EC2 Mode: In EC2 mode we have the ECS cluster defined inside a VPC so multi-az feature is leverage.
2. We have ec2 instances in the vpc and each instance acts as an ec2-host. The Images are stored in ECR.
Auto-scaling is used to manage the HA.
3. Task definition uses image in ECR and using service and task definition , containers are deployed in ECS.
4. it is a great middle ground as ecs manage admin overhead but we have to manage the ec2 provision, availability
and scaling part. We have to pay price even if no container or task is running as ec2 hosts are already provisioned.
5. EC2 hosts as ecs cluster is benefit as we can use reserved/on-spot instances for business scenario.

Fargate mode:
It removes even much more admin overhead of using containers within AWS.

1. In Fargate we run the container inside a shared fargate infrastructure along with other users
from the shared pool similar to ec2 hosts shared hardware. However here task containing containers are deployed in
fargate shared infrastructure.
2. The containers running in fargate shared infra are then injected into ECS cluster VPC using an ENI network component.
each ENI in VPC can have public ipv4 address / private based on how vpc in configured.

Summary:
EC2 vs ECS(EC2) vs Fargate:
==========================
1. EC2 : In EC2 we run applications as virtual machines in ec2 instances.
2. ECS(EC2): Running applications as ECS in ec2 hosts container.
3. Fargate: Running applications in ecs cluster in fargate container mode.

4. If you use container then ECS to be used.
4. EC2 mode is favourable when large workload and price conscious.
Fargate mode to be used when large workload but overhead conscious.
5. In EC2 mode we have to pay for the containers even if the containers is not running any applications
or task . But in fargate mode we pay just for the resource usage.
Farge is best suitable when we have inconsistent workload of small/burst type. 
Batch /periodic workloads.

*fargate mode requires a vpc.

modes in ecs:
1. Fargate(Network Only)
2. EC2Linux + networking
3. EC2Windows + networking

Ec2 bootstrapping:
1. Bootstrapping allows ec2 build automation.
2. Ec2 user data is accessed via meta-data IP. http://169.254.169.254/latest/user-data
3. EC2 User data are only executed once at launch time of the instance via by the instance OS.
4. EC2 doesn't validate or verify the commands being passed in user-data . The mechanism in OS simply
executes the user-data as the root user.
5. EC2 service in an ec2 instance provides user-data and meta-data . Whenever an instance is launched
the ec2 service looks for user-data and meta-data in the IP. If available it executes just like any other script.
6. if the user-data is not understood by OS then it will be bad-config.
7. It is just a block of data opaque to ec2 and not secure to store password etc.
8. 16kb is the user data limited size and are used for pre-configuring/post-launch of an instance.

Boot-Time-To-Service-Time: The time taken for the ec2 from boot time of an ec2 instance to service time 
of the instance.
We can minimise the post launch installation or configuration of the machine by combining
ami-baking + bootstrapping. if the configuration includes 90% installation + 10% configuration.
Then we can configure an AMI and bake AMI then 10% pass to ec2-user data when launching instance.

CFN-INIT and Creation Policies: (Enhanced automation for bootstrapping)
1. CFN-INIT command is passed in user-data which refesh to CFN INIT declaration in cfn template.
it creates a stack and capture any change in updates so that we can update the stack and re-execute cfn-init command any number of times.

2. cfn-signal : CFN-init command status is passed to cfn-signal and it then change the instance to desired state.
The creation policy is mentioned in cfn template with time-out value. If the signal received is correct and okay
it updates the ec2 instance to desired state else error etc.
Basically these are the ways to inform cloud formation that bootstrap config is done correctly and response is sent back.

          /opt/aws/bin/cfn-init -v --stack ${AWS::StackId} --resource EC2Instance --configsets wordpress_install --region ${AWS::Region}
          /opt/aws/bin/cfn-signal -e $? --stack ${AWS::StackId} --resource EC2Instance --region ${AWS::Region}
Basically CFN-SIGNAL receives Success signal from cfn-init and then move the cfn template to 'create complete'
Status

EC2 Instance role:
===================

The IAM role has the permission policy attached to it. 
EC2 Instance role is the IAM role that EC2 assumes and ec2 gains the same permission to the resources
mentioned in the permission policy of the iam role.
The application running inside the ec2 instance gets the permission and temp credentials to access the resources.

InstanceProfile is way of passing this role inside ec2 to application. InstanceProfile is a wrapper
around Ec2 IAM role. When we attach an ec2 instance role we are basically attaching profile to the ec2 instance
which is of same name.
The temp credentials are delivered to application via instance-metadata. Ec2 and secure token service
lease with each other so that they keep renewing credentials time to time as long application
keep checking creds in metadata such that they never expire. Automatically rotated and always valid as long as the
instance role is attached to the ec2 instance.


SSM parameter store:
====================

1. Used for storing configuration and secrets.
2. It has parameter name and value. Value is configuration .
3. SSM is integrated with many AWS products.

3 types of parameter store:
a. SecureString
b. StringList
c. string
We can store license codes, Database strings, full configs and password.
It can store plain-text and cipher-text. It also supports hierarchies and versioning.

SSM is an aws product and it is a public service. So external applications have to connect to aws public end-points to connect with SSM.
Application, lambda function, ec2 all can integrate and use SSM but SSM has an IAM role which must allow them.
also if SSM needs to store something in ciphertext or encrypted form them KMS + CMK is used.

As it support hierarchy tree so we can specify full path of exact parameter name 
or just the parent path
Note: Any change in SSM parameter has the Capability of triggering events.

System and application logging on EC2:

CloudWatch is for metrics and cloudWatch logs is for capturing ec2 logs.
However cloud watch can capture external facing part of ec2 metrics and wont capture internal logs
and events happening in an OS.
To capture OS level details we have to install cloudWatch agent in the instances.
cloudWatch agent + required permission can log events and logs to cloudwatch .

EC2 Placement groups:
Default placement of an ec2 instance is selected by AWS based on nearest and available good performing AZ. We dont have any control on
the placement.
In EC2 placement group we can alter the placement group to place instance close to each other or not place
close to each other.

3 types of group:
=================

1. Cluster - Pack instances close together.
2. Spread - keep instances separated.
3. Partition placement group - groups of instances are spread apart.

Cluster Placement Groups (PERFORMANCE) : The group is locked down to an AZ. All instances launched as present in same az and same rack.
often they are also present in same host depends on number of instance you launch.
1. Super low latency and packet per second (PPS) possible in AWS.
2. 10GB of single stream possible whereas normally we have 5 gb.
3. All members have direct connections with each other.

Drawback : All are locked down to 1 AZ , so any hardware failure will take down all instances. offers no resilience.
Notes:
1. Locked down to 1AZ.
2. Can span across VPC peering but impacts performance.
3. Not all instance type supports this. Requires supported instance type and they should have 10gbps transfer speed.
USP: Performance, High throughput, low latency, 10 GBPS of single stream performance.


Spread Placement Groups (Resilience)
1. Provides infrastructure isolation. Each instance is in separate rack with separate power supply.
2. Hard limit of 7 instance per AZ. 
3. Suitable for higher availability and resilience.
4. Not supported for dedicated instance type or dedicated hosts.

Partition Placement Groups (Topology Awareness)

1. 7 partitions per AZ.
2. Each partitions are infrastructure isolated from other partitions . Each partitions are having
own rack and no sharing between partitions.
3. We can have any-number of instances inside a partition.
4. AWS can manage and we also have option to choose which instance goes to which partition.
5. Topology aware applications where applications need to be aware of replicating instances.
HDFS,Cassandra, HBase.
6. Massively large scale application where we can design our own infra or aws auto-placed.
7. Dedicated hosts  are not supported.

Ec2 Dedicated hosts
===================
1. Ec2 host is dedicated to you.
2. They belong to specific family of instances e.g: a1,c5,M5
3. You pay for the host and not instances running on it. You pay for the resource consumption of hosts.
4. payment can be on-demand instances or reserved instances.
5. host hardware has physical socket and cores.
5. Hosts can be shared with other accounts in ORG. 

use case : mostly for licensing production usage scenario.
drawbacks: specific to certain family of instances. No RDS, placement group supported.

Enhanced networking 
===================

Enhanced networking is the AWS implementation of SR-IOV, a standard allowing a physical host network card to 
present many logical devices which can be directly utilized by instances.
The NIC card is Virtualization aware. each logical instance card is dedicated to an instance.

This means lower host CPU usage, better throughput, lower and consistent latency , Higher PPS.

EBS optimised
=============
EBS optimisation on instances means dedicated bandwidth for storage networking - separate from data networking.
Mostly comes enabled enabled by default for modern instances with no extra cost.
They are dedicated capacity for ebs.


Route 53
=======

1. Route53 hosted zone is a DNS DB(zone file) for a domains example : animals4life.org
2. R53 public hosted zone means the DB is hosted in R53 public DNS Nameservers (NS)
3. Hosted zones can be private and public. Each time a domain registration happens R53 allocated 4 name servers distributed globally to them.
4. Queries for domains are done to these name servers. The hosted zones store /host dns records (A, AAAA, MX, NS, TXT..) along with IP.
5. DNS hosted zones are globally resilient as they name servers are accessible publicly and are in multiple.
6. Hosted zones are deligated or authoritative for a DNS system. They are single source of truth for a domain.

Summary:
Domain registration -> Hosted zone created -> Domain names in Hosted zone are pointed to Name servers -> These Hosted zone contain DNS records
and are publicly accessible from public internet-> DNS system sees these Hosted zones as authoritative or deligates for the domain.

architecture:
client executes DNS query to DNS resolver(Name servers) -> DNS resolver checks Zone file in Hosted zone DB for the DNS
records containing mapping of domain to IP. -> records are fetched and returned to client and they are re-directed to Site.
This is also supported in VPC through private hosted zone. If name servers are accessible inside vpc via setting then same architecture prevails for subnet inside VPC to access.
The domain is accessible only when it is associated with a particular VPC.


Private hosted zone
A private hosted zone determines how traffic is routed within an Amazon VPC.

Public hosted zone
A public hosted zone determines how traffic is routed on the internet.

Domain registered in DNS system of references -> DNS system contains DNS name servers-> The name servers hosts hosted zone(private/public)-> The hosted
zones contains zone file for storing/mapping of DNS records.

Health checks:
1. Health checks are used to report any bug in end-point or target address.
2. Route 53 health checks are triggered every 10s or 30s . 10s comes at a cost.
3. There are many health checkers globally which starts querying the IP for checking the status.
4. Status can be healthy or unhealthy. 
so test are done in below ways:
1. TCP protocol.
2. HTTP/ HTTPS system where it is expected to return in response of 200 or 300 codes.
3. HTTP/ HTTPS with string matching : the body of response returned is checked for a particular message in first 5120 bytes.
If the endpoint returns the string expected then it is deemed healthy and it pass the check.

HTTP/ HTTPS verify network stacks, web-servers and with string matching we ensure we are getting the content we expected.
What to monitor  : Endpoint, state of cloudWatch alarm, Status of other health checks (calculated health check)

Routing policies
=================
1. Simple
2. Weighted
3. Failover
4. Geolocation
5. latency-based
6. multi-value

Simple Routing policy:
1. a single dns record set for a domain with 3 IP 
A - Ip1,ip2,ip3
2.Client gets domain resolve and fetches 3 ip for the query. Randomly chooses a IP and connects to web session
No health checks associated. However if the ip is down or in case of any bug. It will not be redirected or failover to
other ips. Lead to bad customer experience.

Multi-value routing :
Diffence with simplerouting:
Multiple records with same name and different IP and health check associated.
But any unhealthy IP is removed from the response to client. So client didnt get the bad IP.
whereas in simple routing as is the response were passed same as DNS records.

Failover routing :
The DNS records contain two set of records of same name. One is primary and other is secondary.
Health check is associated with primary and if unhealthy then secondary is put in service.
May be secondary is an s3 bucket with an error page.
It follows active-passive methodology.

Weighted routing policy:
Percentage of weights are allocated to each record of same name.
Total weight is the total of individual weight. Health check is associated
But if weight is 90 and other weight is 10 then the 90 record is selected 90% of time again and again
until other record is fetched which is healthy. 
Good for traffic distribution in EC2 instances and load-balancing . Also during testing phase of an application it is useful.

Latency based routing;
The DNS records contain multiple set of records of same name and different region is associated with them.
AWS maintains a latency based table from source to destination and its latency.
Health checks are configured against each record and if all are healthy then the lowest latency is chosen for the request.

Geolocation based routing:
It is applicable for providing localised content to the the people for a certain geographic region.
Country > continent > default
It checks the country of requestor and tries to match a record present in same country.
If not matches then searches for continent else default is returned.
useful for restricting content within a geo-region or licensing and restricting access.


Database:

1.key value Database
2.Wide column store database : Dynamo DB- we have one partition key + other key. No attribute schema Any/all/none
3.Column database : Redshift , all columns are grouped together and stored together. ( reporting style querying) OLAP
4.document orieted DB: JSON and document store. Suitable for nested documents.
5.graph db: nepture,complex relationship between nodes and objects. social media.

Using Database on EC2:
a. Favourable scenario:
1. DB instance OS level access.
2. DBRoot level access.
3. Requirement for particular DB version or database which is not supported or provided by aws managed db.
4. Combination of os + db not supported by aws.
5. Vendor needs or architecture not supported in Aws.

b.Not using database on ec2:
1. Admin overhead
2. Backup and DR management
3. replication involves admin overhead
4. Ec2 backed by ebs runs in single AZ. you have to handle ebs snapshots and store in s3.
5. Performance limitations and AWS managed db products are amazing in features you are missing out on that.
6. Ec2 is ON or OFF , no serverless and scaling needs to be done by self. no easy scaling.


RDS:

Database as-a-service(DBas)
RDS is database server as a service product from AWS which allows created of managed DB instances.
Basically it provides DB instances where we can host the databases managed by aws. So we are renting databaseserver and paying for it.
Supports enginers mysql,sqlserver,postgresql,mariadb,oracle.
Aurora is proprietary product from aws.

RDS instance: Just like ec2 instance we have rds instance. Each RDS instance host database.They come in different sizes and cpu & memory with prefix db like
db.t3,db.r5,db.m5.
Each rds instance is provisioned in single az backed by EBS volume. So any failure in AZ affects RDS instance.
We can have storage like gp2,io1 and magnetic and storage is billed as gb/month.

To access rds we require CNAMe(a4lwordpress.caves7twjdtm.us-east-1.rds.amazonaws.com) and there is security group tied to rds instance which controlled access.


a4lwordpress.caves7twjdtm.us-east-1.rds.amazonaws.com

RDS Multi-AZ:
============
RDS Multi-AZ architecture contains two instance primary and standby in two AZ in same region.
Primary instance is accessible via cname. It is always pointing to primary instance.
When writes happen to primary, synchronous replication happens to secondary and both commit
to their ebs volume storage. If primary instance is down then CNAMe is pointing to standby replica.
Failover happens 10-60 sec . Standby doesnt provide any benefits except for high-availability.
snapshots can be taken from standby without any perf issues.
failover happens in case of primary instance issue,az outage, instance type change, manual failover 

synchronous replications means MULTI-AZ. Writes to primary and secondary same time with small lag and they both
commit to their disc separately together.


RTO vs RPO
==========

RPO : it is the time between last successful backup taken and the incident/failure occurrence. 
Amount of maximum data loss  
influence technical soln and cost.
Lower the RPO, more expensive and costly the solutions to meet.

RTO: it is the time between the incident/failure occurrence and the full recovery/service back.
Influences documentation,processes, tech staff
Lower RTO, higher cost.

RDS Backups: 
1. Two types - Automatic backups and manual snapshots.
2. The snapshots are stored in S3 and not visible. Region resilience and replicated across all az in that region.
3. The snapshots are full copy at first then incremental ones only capture change data capture (CDC).
4. Snapshots are always taken on standby replica not primary so performance wise no impacts on read and writes on primary.
5. Automatic backups are also scheduled to dump in s3. It affects the RPO time so schedule must be set correctly.
6. Also transaction logs that are executed in database are written to s3 every 5 mins. Whenever restore happens the logs are replayed
on top of backups to bring back the state of db to a specific point in time.
7. Note: manual Snapshots are not deleted automatically they live past the life of db instance. you have to manually delete it.
also before deletion aws provides to take one final snapshot, this snapshot is the backup of entire rds db instance and not on database.So all database
in that instance are covered.
8. Retention period of automatic backup is 0-35 days. within that backup+trans log. even if you retain snapshots during db instance deletion
they get deleted after retention days. So only way is to take final snapshot which need manual delete.

RDS restores
===========
1. restores always creates a new rds instance with new endpoint. So application has to point to it.
2. Restores are n't fast, RTO is affected.


Read-replicas
=============
1.Read replica has primary instance where write happens and many read replica which serves only read.
2.Read replica can be in same region as primary or cross-region. the data transmit in encrypted.
3.Asynchronous replications means Read-replicas. Writes to primary happen first and stored/committed to disc.
then data is replicated asynchronously to read replicas with small lag time depend on network. 
4.read replica is used for scaling out the reads and improves performance as additional instance is present for read.
5. 5x read replica per db instance and massive global performance benefits/globally resilience. We can provide read replicas
to certain consumers to do read only may be in cross-region.
6. They provide near 0 RPO and low RTO as read replicas can be promoted to primary instance to serve both read and write.
Note this process is irreversible. we have to create new read replica.
7. They are read-only until promoted and watch out for failure, in case of failure if data is corrupt
we should not promote replicas for primary as bad data is already replicated instead rely on snapshot and backups.
8. Only scaling out reads read replicas.
9. MULTI-AZ for high availability + Read replicas for scaling reads same/cross region is the architecture together


Note
=====
Replication and backup/snapshot are two different thing.
In Replica they dont save you from corruption. Any data corruption also gets replicated to read replica instance.
whereas backup/snapshot can help to restore back in point in time. Both affects RPO. They lower RPO.
snapshots represents a discrete point in time. They represent a time where the snapshot was taken. So restores to that particular 
hour and minutes.
Restoring snapshots creates a new RDS instance.

RPO means snapshots and backup. More lower RPO by more time to time snapshots and automated backups.
RTO means read replicas that is fully recovered instance. This can be done by promoting RR as primary.

5 min rpo can't be provided by snapshots it takes time we can use read replica but replica doesnt provide safety against data corruption
be careful on choosing and deciding between RPO and RTO.

Aurora
======

1. Aurora is based on cluster concept and use shared cluster storage across all AZ.
2. The cluster has a primary instance and rest all read replicas across all AZ upto a total of 15 read replicas.
3. In RDS Multi-AZ we used to have 1 primary + 1 standby replica for high availability but standby cant take reads.
in Multi-AZ replica's could take only read. In Aurora the replicas provided both benefits they provide reads as well as can act as primary during
failover.
4. The shared cluster volume is SSD disk(High IOPS,low latency) and max to 64 tib with 6 replica , 2 in each AZ . So whenever writes happen to a 
primary instance the data is written synchronously to disk. The data is then shared and replicated across all disk
and replicas start reading from it. It repairs immediately for any corrupt disk.
5. Billed for high watermark and not usage of GB. Usage of cluster storage for a highest GB is billed.
6. It has multiple end points : Cluster endpoints which points to primary and used for read and write.
reader endpoint : points to primary if no replicas else point to all read replicas and load balance.
7. Provides 100% backup size as cluster size.
8. Backtracking feature: Configured per cluster basis, using this we can backtrack to a specific point by rolling back to
timestamp before the corruption of data occurs without change in application side.
9. restore creates a new cluster.
10. fast clones to create brand new database but the storage is not full copy. It just stores the changed/update data
between source db and cloned db.

Note: restore snapshot uses same engine  whereas migrate snapshot uses new engine.
When migrating the snapshot to a new DB. USE ARN of snapshot if migration is from non-aurora to Aurora else use snapshot iD
when we migrate snapshot a new DB engine or db instance we lost the media files as media files are stored locally in ec2instance.
Thats where EFS comes into rescue.


Aurora Serverless
=================

1. We dont have to provision database instances in advance like provisioned Aurora.
2. It uses concept of ACU: Aurora capacity units. it is scalable and massive removal of admin overhead.
3. Since it is serverless the cluster has min and max ACU. Cluster scales based on load.
4. We only pay for resources consumed per second billing.
5. Same cluster shared storage 6 copies 3 AZ replication.

Architecture
============
Provisioned servers are replaced via ACU. we scale based on ACU we are allocated.
ACU are allocated from a pool of ACU managed by AWS.
Now based on load we are allocated ACU within a range of defined min and max acu for a cluster.If the load is high
then more ACU are allocated but max upto the defined max range and once the max acu instance is activated
rest all small acus are discarded and we pay for consumption per second billing + cluster shared storage.

On top of ACU we have the wrapper known as proxy fleet. This is between the client+Application and ACU.
Client or application dont interact with ACU directly. They communicate with proxy fleet and these fleet scales based on demand
and shrink and interact with ACU.

Use case:
Infrequently used applications : Less user base so lower min acu and lower max acu. Less cost for ACU allocation.
Unpredictable workloads: You dont know the time of day or week where the load spikes
so to learn more of it we can have lowest min ACU and higher max ACU. No only when load
happens we can study it and take decision on correct acu.
variable workloads : few minutes within a hour of times a day is good as we pay for those minutes ACU only.
New applications and dev and test db's


Aurora Global database:
=======================

It is global replication across max 5 secondary region from primary region 
We set up replication from primary region to secondary region with synchronous replication of 1sec across storage layer.

In Primary region we have 1 R/W instance and 15 read replicas whereas in secondary region we have all 16 read replicas no write happens

Use case: 
a.Cross region DR and Business continuity.
b.Global read scaling - low latency performance improvements.
c.Secondary region replicas can be promoted to R/W in case of failover.

Multi Master Writes
===================

In Multi-master mode all instances are capable of R/W.
1. There is no concept of writer and read end point. All instances in the cluster can take R/W.
2. Application can connect to one or more instances and can give write request.
3. When an instance receive write request it propose the change to other instances that makes up the cluster.
It requires quorum of agreement before it can commit write to the shared cluster storage. 
4. If quorum happens and write succeeds then along with committing data to storage it replicates the data to other instances
for in-memory caching so that upto-date data access is there for quick read request.

Benefits and key points:
Since all instances are writers so the application can connect to one or both of them at same time and issue write.
In case of failover if one of them is down then application still has connectivity and access to other instance
and writes happen.
BUT in Aurora default single master archi if primary fails then replica is promoted to primary to serve R/W but this process takes time
and cause some disruption.

Database migration  service 
===========================
a. A managed database migration service.
b. It has a replication instance running on top of EC2 instance with a dms software.
c. The replication instance has the replication task which stores the source and destination end-point(config,credential)
as well as source and target db details. 
d. It migrates database from source to destination endpoint using endpoint details and the jobs can be of 3 types:
1. full load
2. full load + CDC
3. CDC only

SCT : Schema conversion tool , it is used for schema conversion during dms to modify the schema. 

Elastic File System (EFS)
=========================

1. EFS is an implementation of NFSv4 and is available inside VPC with file system having POSIX permissions.
2. They can be mounted on Linux and /nfs/media path
3. It is a shared mount storage between ec2 instances. The media stored in EFS are not lost when ec2 instance
is lost or shutdown. EC2 stores data locally where EFS stores it in nfs mount shared between rest all mounted
instances . The storage is separately from ec2.
4. EBS attached to ec2 was block storage whereas efs is file-storage.
5. EFS is a private service and they are available via mount targets inside VPC.
The mount targets are present inside VPC in every AZ just like nat gateways. They take the IP range of subnet they are present inside the VPC.
However we can access EFS via hybrid network using VPN or DX(AWS direct connect service) from on-premises network to AWS if configured.

6. EC2 instances launched in the AZ's in subnets connect to EFS via the mount targets.
Also any on-premise	infra connect to EFS using same mount targets IP. The configuration has to be done to enable
hybrid networking to connect on-premises to vpc using VPN or DX/Direct connect.

EFS more:
1. Linux only POSIX permission file system.
2. Performance modes :
	a. General purpose
	b. MAX I/O
3. Throughput Modes:
	a. Burstling
	b. Provisioned 
4. Storage classes:
	a.Infrequent
	b.Standard
Lifecycle policies can be used with classes.



Each capacity unit is equivalent to a specific compute and memory configuration
1ACU= 2gb or RAM
32ACU= 64 GB of RAM
64 ACU = 122 GB of RAM

Load Balancer:

ALB
===

1. It is Layer 7 . Understand HTTP and HTTPS.
2. Scalable and highly available.
3. Listens outside and sends to target groups.
4. Public facing or internal.
An Internet-facing load balancer routes requests from clients over the Internet to targets. 
An internal load balancer routes requests from clients to targets using private IP addresses.
5. Hourly rate + LCU units (load balancer capacity units)	

Load balancer has a DNS name to which client interacts. LB places its node in AZ's and then distributes traffic from LB Nodes to the backend
instances based on 100% or 50%.

LB gets load 100%. it has two nodes
A (AZ A) and B(AZ B) which gets 50-50 load. Now A has 4 instances on az-A so 50%/4 so each instances get 12.5
in Node B we have 1 instances on AZ-B So we get 50% load on it .

Cross zone load-balancing : it is the feature that allow load balancing to occur across cross-zone not limited to a single AZ only on which the node
and backend instance is present. Now load balancing can occur across AZ-A and AZ-B simultaneously 

Target groups : Load balancer has target groups consisting of ec2 instances or lambda functions or ecs .
Each instances has to be registered to a target group and load is sent to TG.
Load balancer can path or host rules.
if we specify balancer based on path requested then it will send to specific tg for a path.
if in multiple domain names are present in same LB. Then it is host based.

Notes:
1. Target groups are addressed via rules.
2. Rules can be path based or host.
3. ALB supports SNI for multiple SSL certificates upload for host based rules.
If we have multiple host name and for each host we want to upload SSL cert then we can do it having layer 7 support.
4. Load balancers have listeners points http/https which client interacts or send request.
A listener is a process that checks for connection requests, using the protocol and port that you configured.
HTTP: 80, HTTS: 443

5.Specify the Availability Zones to enable for your load balancer. 
The load balancer routes traffic to the targets in these Availability Zones only. You can specify only one subnet per Availability Zone. 
You must specify subnets from at least two Availability Zones to increase the availability of your load balancer.

6. The load balancer are enabled in AZ in subnet.
For internet-facing load balancers, the IPv4 addresses of the nodes are assigned by AWS. 
For internal load balancers, the IPv4 addresses are assigned from the subnet CIDR.

2-AZ , 2-Subnet Internal facing IPV4 Address is assigned from subnet CIDR (example 
Assigned from CIDR 172.31.80.0/20) or Internet facing (IPV4 is assigned by aws)

7.Your load balancer routes requests to the targets in this target group using the protocol and port that you specify, 
and performs health checks on the targets using these health check settings. Note that each target group can be associated with only one load balancer.
8. Target type can be instance, IP or lambda function. 
Health check is specified to TG based on protocol (HTTP/S) and port to check.

Launch configuration and launch templates
=========================================
These features allow to define to the configuration of ec2 instances which can be reused and shared
for creating new ec2 instances in bulk as well as launching ec2 instances as part of auto-scaling group
It lets you define the AMI type, instance type, sg,network, storage, key pair to be used by ec2's.
However both LC and LT are not editable and versioning is not supported.
Launch templates are newer as they support t2/t3 burstable instances and also supported by CLI.

Note:
Launch templates can be used to directly launch instances
Launch templates supports versioning.


Auto-scaling
============
1. It allows automatic scaling and self-healing of Ec2 instances.
2. It uses scaling policies to automate the process based on metrics.
3. It has a minimum-desired-maximum instances defined. (1-2-4). it always tries to match the desired level
of instances using provisioning or termination.
4. It uses the Launch config/templates to launch similar instances during scaling.
5. Auto-scaling group tries to even the launching instances across the subnets. If the desired capacity is 3 then
it tries to launch 3 instance in 3 AZ in 3 subnet as 1subnet=1AZ.

Scaling policies:
1. Manual scaling : Manually adjust the desired capacity
2. Schedule scaling : Scaling based on timeframe or events 
3. Dynamic scaling
	a. Simple : Scaling based on a metric value for example CPU utilisation 50% +1 instance. Or scale in when <50% -1
	b. Stepped Scaling: More Indepth metric with bigger+/- based on difference.
	c. Target Tracking: We define a target and scaling policy tries to achieve or maintain it as consolidated groups
	for example desired aggregate CPU:40% , ASG handle it.
	
Cooldown period: time in seconds the gap between a previos sccaling and next scaling can take place. Within the cooldown period
no scaling will take place. this is prevent cost rise and wont lead to immediate scale out .

ASG are automatically added or removed from target groups of ALB.
ASG can use the load balancer health check rather then ec2 status checks. this leads to application awareness.

ALB + ASG together = elasticity for an application. Always use for client facing app.
ASG defines when and where and LC/LT defines what 
when means when to launch instances what leads to either cpu metric etc.
where defines which VPC,subnet,az
What defines what to launch, what instances having what ami,sg,storage,keypair etc.

It is called self-healing because it provisions or terminates instances based
on scaling and desired capacity so subnets and az without manual intervention to keep application running.

Network Load Balancer (NLB)
==========================
1. NLB's are layer 4 and only understand TCP and UDP.
2. Since the network stack are lighter they have much faster latency of ~100ms vs 400ms of ALB.
3. Rapid scaling as they can serve millions of request per second.
4. They are the only load balancer in family which on defined gets a static IP per AZ. Also we can
use elastic IPs(whitelisting) for firewall.
5. Only load balance anything other HTTP/S like TCP/UDP/FTP. Anything over these are just bypassed.
It doesn't care about them.

SSL Offload
===========
There are three ways load balancer can handle secure connections:

1. Bridging
2. Pass-through
3. Offload


Bridging (Default mode) [ HTTPS listener, ELB]
==============================================

Client -> ELB:

1. One or more clients makes one or more SSL connections to the ELB.
2. The connections is a HTTPS protocol i.e HTTP with a Secured wrapper on it.
3. The ELB decrypts the connection using the SSL certificate stored on it after matching it with
the domain name for which client has requested.
4. AWS gets to see the certificate.
5. The decryption operation is performed and connection is terminated.

ELB -> Backend EC2:
1. Since the ELB was able to decrypt the connection it gets to see the plain-text format of HTTP request.
2. It creates a new Secure SSL connection from ELB to backend EC2 client.
3. EC2 instance decrypts and perform cryptographic operations to remove the SSL wrapper and matches
the certificate with domain name and perform computation.

Positives:
a.Since the ELB was able to decrypt the connection it gets to see the plain-text format of HTTP request.
And based on that it can take action.
Negatives:
a.Admin overhead of cryptographic operation both at ELB and EC2.
b.Certificates need to be located at EC2 and ELB.

Pass-through [ TCP protocol listener, Network Load Balancer]
===================================================

Client -> ELB:
1. One or more clients makes one or more SSL connections to the ELB.
2. The connections is a TCP protocol and uses Network load balancer.
3. No encryption or decryption happens at NLB. The client request passes as is straight to
backend ec2 instance.
4. Each ec2 instance need to have a SSL certificate to perform decryption and cryptographic operations
and performing computation.

Positives:
a.No Admin overhead at ELB.
b.Self managed and secured as AWS doesn't get to see the certificate or touch encryption.

Negatives:
a.No decryption by ELB so we dont get to see what contents pass through.

Offload [ HTTPS listener, ELB]
==============================
1. One or more clients makes one or more SSL connections to the ELB.
2. The connections is a HTTPS protocol i.e HTTP with a Secured wrapper on it.
3. The ELB decrypts the connection using the SSL certificate stored on it after matching it with
the domain name for which client has requested.
4. AWS gets to see the certificate.
5. The decryption operation is performed and connection is terminated.

ELB -> Backend EC2:
1. Since the ELB was able to decrypt the connection it gets to see the plain-text format of HTTP request.
2. It creates a new HTTP connection from ELB to backend EC2 client.
3. Since this is a HTTP connection request so it is in decrypted plaintext form.
4. EC2 instance doesn't required cryptographic operations or any decryption to perform for computation.

Positives:
a.Since the ELB was able to decrypt the connection it gets to see the plain-text format of HTTP request.
And based on that it can take action.
b. EC2 doesn't need to take any burden or overhead except maintaining hTTP traffic.
c. Smaller ec2 instances can be used.

Negatives:
a.Request goes across AWS network in HTTP format unencrypted. 

Connection stickiness [ Enabled at Load Balancer with a duration]
=====================
1. Session is maintained and stored in application server for the client request.
2. Whenever a user makes a new connection it is assigned a cookie by ELB . The cookies
until expiration is passed with the subsequent request to ELB.
3. Same user is routed to same backend instance each-time it makes a request unless
the cookie is expired or server is terminated.

Disadvantages:
a. This makes a stateful connection as server is managing the session details.
b. Can lead to uneven load balancing if the same user is redirected to particular instance
for subsequent load.
c. Fair distribution doesn't happen as session is stored in server level. To make it stateless
we should store the session details somewhere else using REDIS/MEMCached, DynamoDB.



Note: Auto-scaling and Load balancer both run inside a VPC in one more subnets.
Then comes whether they have public-facing or internal.




SERVERLESS AND APPLICATION SERVICES
===================================
Serverless doesn't mean there are no servers. It means there are no servers provisioning to take place manually by us.
No admin overhead. It will scale , provision, terminate based on load, request and call.

Event driven architecture:

1. No constant running or waiting for things.
2. Produce produce events when something happens and consumer consumes the events
does processing as events gets delivered to them.
3. No constant allocation of resources to them. They scale in once processing completes.
4. No dependency between application tiers and no waiting for any tiers to respond.
5. They can scale in and out independently based on event or load they receive or triggered
6. Between Producer and consumer there is a event router and event bus.
Event router takes event from producer and queue them in event bus. Events get routed to correct
consumer from event bus by router. Once events are consumer they are deleted from bus.

AWS Lambda
==========
1. Function as a service (FaaS)
2. They execution function or invoke function.
3. Lambda function are key component of serverless architecture. You are only billed for
the execution of time of the function.
4. Functions are written in a language and they uses a run time(python 3.6, java)
5. Functions runs in a runtime environment.
6. Lambda functions always runs in a clean runtime environment with allocates resources such as cpu.
No matter whatever be the number of functions invoked in parallel, all runs as clean separate environment.
7. Lambda has no persistent storage.
8. Lambda functions runs in a container and assumes an execution role.
9. The execution role has the privilege to connect to AWS services to put data into Lambda
and redirect output from lambda to AWS products.
10. Lambda is event-driven or aws service,api-gateway or manual invocation.
11. Lambda function max execution limit is 15minutes.
12. 1M free request per month and 400 000 GB-seconds of computer per month.


CloudWatch Events and EventBridge
=================================
1. If X(software of event producer) happens , or at Y time(s) then do z(Consumer , event consumer)
2. EventBridge is super set of CloudWatch. It is version2. 
3. AWS is pushing for EventBridge for newer deployment.It has got all features of cloudWatch+ additional
features.
4. Every AWS account has a default account event bus which captures stream of events occuring.
5. CloudWatch Event has this bus as the implicit , not visible outside.
6. EventBridge is used to create additional explicit buses. 
7. We create rules or pattern which matches incoming events. We can also create
schedule to route the events to 1+ targets (example lambda func)

Flow.
1. Event Bridge runs on top of eventbus, whenever it sees any events flow in eventbus
it triggers the targets based on below rules

a. Pattern Rule (Invoke targets when event matches a specified rule or pattern)
b. Schedule rule (Invoke targets on a schedule).

2.The rules invokes the targets to consume the events and process them.
3. The events are in the form of JSON structure with data in them.
4. The targets parse the json read the data.


API Gateway
===========

API's are small piece of code running or hosted in a server.
To Interact with API we require endpoints. They are basically entrypoints to an API. 
API also provides an API Interface which consist of all data points. All services are interacted via an API. 
So API for a large scale system must be highly available and scalable. 

1. API Gateway is an AWS managed API endpoint Service.
2. Create , publish, monitor and secure API's as a Service.
3. API's are billed based on number of API calls, data transfer and additional performance features
such as caching.
4. They can be used directly for serverless architecture.

API Gateway is a core product for building serverless application in AWS.
API gateway can be used directly with some aws services to pull data without using compute service.
They can be used with all types of architecture as an evolution from monolithic to Microservice to
Serverless.

Serverless Architecture
=======================

1. We manage servers if at exist very few but less admin overhead.
2. Applications are a collection of small & specialised functions.
3. Stateless and ephemeral environments- duration billing.
4. event-driven so consumption only when being used.
5. for computation mostly FaaS and managed services are used wherever possible.

Simple Notification Service(SNS)
================================
1. SNS is a public AWS service which has connectivity to public endpoint.
2. Coordinates the sending and delivery of messages.
3. The messages are <=256 kb payloads. 
4. SNS Topics are the base entity of SNS where permissions and configuration are done.
5. It is based on pub/sub model where publisher sends a message to topic
and subscriber who has subscribed to the topic receives it.
6. The subscriber end point can be HTTP, SQS, email,push message, lambda etc.
7. SNS is a widely used default notification services inside AWS.
8. It is also widely known for FAN-OUT pattern where events or messages
are dropped to topic as single payload entity then multiple SQS queues are the subscriber 
who consume the messages in topics and process them individually.
9. We can also filter message from SQS topic to lambda functions.
10. They can also send delivery status for few endpoints like http,SQS ,lambda.
11.They can retry deliveries for reliable delivery.
12. They are region resilient so HA and scalable.
13. Also supports SSE for encrypting data message on disk.
14. We can have cross-account  via topic policy. 

Step functions (State machine :the base entity)
==============================================

1. Step functions lets you create state machines. The state machines consist 
of start and End but in between many states which can be time based, directional/choice based
which lets you create long running serverless workflow based applications with AWS integrating
with many AWS services.

2. They aim to overcome limitations or design decisions of lambda functions. The maximum
execution limit time is 15min for lambda so any applications which require longer processing
is not permitted by lambda. 

3. However one way to achieve using lambda is to chain many lambda functions together but that would lead
to a messy applications at scale. Also they are stateless and cant commit or store any state of processing.
In a order processing system we need to store state of order during workflow execution.

4. Lambda is a FaaS so it is good for single isolated functions which executes a certain objective
and they are stateless as each invocation creates an isolated environment with new compute resources.

5. Step functions has state machines. These state machines lets you create serverless workflow.
Each state machine has a START and END and in between are states. States are the things that occur.
START---->state---->END.

6. Maximum duration for state machine execution with step functions is 1 year.

7. There are two types of workflow available in step functions:

a. Standard workflow - max duration 1 year for long running workflow.
b. Express workflow - Transaction, processing guarantees,event based. Runs for 5 minutes

8. State machines can be invoked via API Gateway,lambda, EventBridge, IOT rules.
9. We can export our state machine configuration knows as ASL ( AMAZON STATES LANGUAGLE). It is a
a json based template.

10. Like all AWS services we need IAM role permissions to interface with other aws products and services.

11. Different type of States:
a. SUCCEED & FAIL
b. WAIT  - Pause a processing of workflow for certain duration or specific point in time.
c. CHOICE - To take a choice in workflow based on certain imports/input
d. PARALLEL - To execute parallel operations tasks for a choice we made.
e. MAP - Imagine having a list [] of orders having 10 order. Now since we have 10 order we have to execute
10 times the processing.
f. TASK - Represent single unit of work. It actually does thing and integrates with lots of
different services like GLUE, BATCH, SQS, SNS ,EMR etc.

So State machines has several states of a workflow and a task which  coordinates with other external
services which does the actual work.


Simple Queue Service (SQS)
==========================

1. Public ,Fully managed , Highly available queues.
2. Queue are of two types : Standard Queue or FIFO queue.
3. In Fifo queue ordering is preserved whereas standard queue no ordering guarantee.
4. Messages in queue can be upto 256kb in size.
5. Received messages are hidden till a visibility time-out. Once the time-out
is expired they reappear again for processing. So they should be explicitly deleted once processed.
6. SQS has the concept of Dead-Letter queues for problem messages.
Any message which is unable to be processed and keep coming can be configured to push to dead letter queue.
Different workload can be designed to deal with DLQ.
7.  SQS can scale based on queue length: ASG and lambda func.
8. Standard queue: At-least once delivery but can be duplicates with different ordering.
FIFO Queue: exactly once , no duplicates.
9. Standard queue scales so fast as they dont have any limitations on delivery and ordering.
whereas fifo queue scales to 3000 messages per second with batching else 300 without.
10. SQS is billed based on request. Request means number of time you request to poll messages
from queue. Polling can be Short(immediate) polling and long (waitTimeSec) polling.
10. Short polling is not cost effective as there are changes of 0 messages and you keep polling
and getting billed for the request whereas long polling polls when when there are messages and wait
for certain duration . They poll 1-10 messages upto 64kb total.
11. Message can be encryption at rest using KMS or transit.
12. Just like bucket policy, topic policy we have queue policy for giving permission to other accounts
to use SQS.
13. 14 days retention of messages.

SQS Fan Out pattern. 1 SNS Topic and multiple SQS queue subscribing to the topic
for independent scaling and processing.


Kinesis [ Ingestion of data at scala, large throughput]
=======

1. Kinesis is a managed scalable streaming service.
2. Producer pushes or ingest data to streams and the streams can scale to near or infinite data rates.
3. We can have multiple producer and multiple consumers ingesting and reading reading from streams.
4. Once the kinesis ingest data to streams , it can retain for 24 hrs only. Within 24 hrs the consumers
can read data do analytics or processing. Post 24hrs it is replaced by new data.
5. Public service and highly available by design.

6. Kinesis stream scales based on Shards. We can have 1 shard, 2 shard to N shard.
And for each shard we can have ingestion at 1MB/S and consumption at 2MB/S.
7. As we have 24hr moving data window for streams, we can increase the window to 7 days with additional
cost.
8. Each kinesis streams has shards and each shards and kinesis data record(1MB)
9. Kinesis firehose is the product which is used to streaming/ingesting data 
from Kinesis streams to other AWS products like S3. May be you want to store
the data for long term so S3 storage can be used while using firehose for transfer.

Producer send data -> Stream consist of Shards(1..N number)-> Kinesis ingest data into shards-> Each shards persist
data within it as kinesis data record(1MB)-> Then from the stream consumer reads data.

SQS vs Kinesis [Point of Difference]
==============
Kinesis: 
1.Huge scale Ingestion of data, large throughput. Rolling time window as multiple consumers can consume at real time or within
window.
2.Retention of data /persistence: 24hrs/7 days.
3.Designed for data ingestion, analytics, Monitoring, App Clicks.

SQS: 
1.Worker pools, decoupling, Asynchronous communication.
2.SQS usually contains 1 producer group, 1 consumer group . 
It is designed for asynchronous communication where the producer or consumer doesn't bother about the functional state of each other.
3.SQS has no persistence of message, message are polled and deleted. Gone forever , no time window or retention of 24hrs /7 day concept.

CloudFront
==========
1. CloudFront is a global object cache(CDN)
2. Content are cached in locations close to customers.
3. Low latency and higher throughput. Less load on the content server.
4. Can handle both static and dynamic content.
5. Can integrate with ACM (AWS Certificate Manager) to provide HTTPS website as static hosting in s3.
6. Caching only works download and not in upload. Upload is always origin direct connection.

Terms:
1. Distribution: It is the configuration unit of cloudfront.
2. Origin: The source location of your content.
3. Edge locations: The local infrastructure which holds the cache of your data.
4. Regional edge cache: Large version of edge locations. Provides another layer of cache.

Flow:
1. User uploads an Image to s3 bucket and configure distribution for edge locations.
2. Cloudfront URL is generated.
3. The URL is accessed by other users in the world to request for the image.
4. CFN first checks the local edge locations of the users for the image. If the image is found then it is 
returned to the requestor immediately with super performance benefit in speed.
In case the image is not found, it next check the regional cache . 
5. If the image is not found in region cache , it does an origin fetch and catch data both in Region cache and edge locations
and returns the image to requestor.
6. Next time new requestor requesting for image if belong to same locations then returned from edge locations. Else
checks the region cache and returns the image. No origin fetch occurs.

Caching optimisation:
1. Caching optimisation can be done to improve speed and performance of request.
2. If simple content is requested without use of any query string parameters then it is fast
and full original content as is cached.
3. If we specify query string parameters with the request then the content is fetched based on certain URL pattern condition and cached.
Not more performance benefit as all user wont worry with exact query pattern. 
4. Also querying for a distinct id etc wont help in caching as every time new ID are requested and edge locations will run out of memory.
So things to be kept in mind while caching and planning for distributions.

We have the option of caching all query string parameters or certain pattern. Also forward to origin yes or no.

AWS Certificate Manager[ ACM ]
==============================

1. HTTP websites are simple and insecure.
2. HTTP/S : HTTP websites are made more secure by adding a layer of security in form of SSL and TLS certificate.
3. ACM is used to create , renew and deploy certificate.
4  The certificate are signed by a trusted authority and hosted in server.
5. Browser trust the trusted authority and as certificate are signed by trusted authority it trusts the certificate too.
6. HTTPS enabled secured encrypted data in transit and certificate proves identity.
7. It supports only few services (Cloudfront, API Gateway,ALB and not ec2) . Means website running on ec2 machines server can't be
assigned a trusted certificate by ACM.

FLOW:
1. ACM created certificate and deploy into CFN distribution and origin and supported services.
2. Any client request to edge locations using HTTPS + DNS name on the certificate.
The certificate is checked at end-point server and matched with DNS name. No self signed certificate is allowed.
It has to be signed by trusted identity to reach origin for fetch.


AWS Region that You Request a Certificate In (for AWS Certificate Manager)

If you want to require HTTPS between viewers and CloudFront, you must change the AWS Region to US East (N. Virginia) in the AWS Certificate Manager console before you request or import a certificate.
If you want to require HTTPS between CloudFront and your origin, and you're using an ELB load balancer as your origin, you can request or import a certificate in any Region.

Origin Access Identity (OAI)
============================
This is feature to secure our S3 bucket from direct access by user bypassing CloudFront knows as OAI.
1. OAI is a virtual identity. It is created and associated with cloudfront distribution.
2. The distribution is a combination of DNS name + associated edge locations . So, the OAI is installed and configured at the 
edge locations also.
3. Now we configure the bucket policy of s3 bucket to explicit allow only access for the OAI coming from edge locations.
Rest all is implicit Deny.
4. Now any user requesting to access the s3 bucket resource will be allowed only if it is coming via edge locations with OAI.
Rest all direct access is blocked via implicit deny.

Note: If the CloudFront is made private then it's restrictions wont apply and user will be able to direct access the s3 bucket
ARN/DNS directly unless OAI is configured. So OAI configuration is a must even if CFN is configured to be private.


Lambda@Edge
===========

1.Lambda@Edge allows cloudfront to run lambda function at CloudFront edge locations to modify 
traffic between the viewer and edge location and edge locations and origins.
2. Runs is AWS public space.
3. Different Limits vs normal lambda functions.

4 parts : Viewer request -> Origin request -> Origin response -> Viewer response.
Viewer request: After CloudFront receives the request from viewer.
Origin request : Before CloudFront forward the request to origin.
Origin response: After CloudFront receives origin response.
Viewer response : Before CloudFront forwards the response to viewer.

Use case:
========
a/b testing
Migrating between s3 origin
Content by country
different objects based on device.

AWS Global Accelerator
======================
Global accelerator edge network.

AWS Global Accelerator is designed to improve global network performance by offering entry point onto the global AWS transit network as 
close to customers as possible using ANycast IP addresses

1. AWS Global Accelerator uses the concept of Global accelerator edge network across AWS REGIONS.
2. Customer when tries to reach a AWS Infra they had to go through public network with lot of performance and network issues
due to distance and hops. So performance is not good as distance increases.
3. Normally we have a unicast IP address assign to a single network device and no two device can share the same IP.
But in Global accelerator product we share two anyCast IP between multiple locations. So when user tries to reach AWS infra
they are routed to closes edge locations.
4. In Global accelerator product we created accelerator and assign two AnyCast IP address (1.2.3.4 & 4.3.2.1) to AWS global edge locations.
These locations are present widely in AWS regions globally. When customer tries to reach either of this IP they are redirected
to nearest edge locations over a public internet. 
4. Once they reach the edge locations they enter the AWS global network zone. From here on AWS uses the global private
backbone network to transmit user request to the AWS infrastructure with significantly high performance. These private global backbone are
fiber network. 

CloudFront vs Global Accelerator product
========================================
1. Global Accelerator product is a network product and can only server layer 4 request (TCP, UDP). It cannot understand HTTP/S 
protocol.
2. CloudFront is used for caching content across edge locations.
3. GA is the entry point to AWS global backbone. It's objective is to move AWS network close to customers where as cloudfront objective
is to move content closer to customers.
4. Any requirement for caching, endpoint edge caching choose CloudFront. If requirement is for TCP/UDP,
AWS global private backbone network, routing close to global edge locations using AnyCast IP then choose AWS global accelerator.


VPC Flow Logs
=============
1. VPC flow logs only capture "Packet Metadata" and not "Packet contents".
2. They can be applied to VPC level : all interface in that VPC.
3. Can be applied Subnet level : All interfaces in that subnet.
4. Can be applied to instance/interface directly.
5. Flow logs are not real-time. We can't rely on them for real time traffic or monitoring.
6. We can dump flow logs to S3 Bucket or CloudWatch logs for analytics.

Parts of the flow logs are :
<srcaddr>
<dstaddr>
<srcport>
<dstport>
<protocol>
<action>
If the action is OK means accepted it is often security group as they are stateful , if traffic is allowed IN
then they are allowed out i.e outbound.
Whereas if you see two lines of VPC flow logs accept and reject action then perhaps NACL is present at subnet level
which is stateless ie. it evaluates both inbound and outbound traffic. Such logs are of two lines.
ICMP=1,TCP=6,UDP=17 [Protocols]

Note: it only captures traffic metadata details about sender and receiver
but never capture what the contents of packet. Also Not real-time.

Egress-Only Internet Gateway [ Only for IPV6 Address]
=====================================================
1. All IPV4 IP address are classified to public and private IP's.
2. In a VPC private IP address are not accessible to public internet or AWS public space. To make an private IP
publicly routable we use NAT gateways in VPC level and configure route to redirect all ipv4 traffic coming
from internal AWS to nat gateway.
In this method All Outbound connections are allowed + Inbound Response are allowed But Inbound Connections from
public Internet are not allowed blocked.

3. In IPV6 version 6 IP address inside AWS , ALL IPV6 IP Addresses are publicly accessible. There is no
concept of public or private IP's. It allows both Outbound Connections+InBound Response as well as
InBound Connections + Outbound response. 
To put a restriction to the InBound Connection request and trying to make architecture similar to IPV4 we have
egress-only Internet Gateway. It don't allow any incoming connection request from public internet /aws public space
to internal VPC.
Setup - ::/0 added to Router with destination as eigw-id.

4. Egress-Only-Gateway are HA across all AZ in the region and scales when required.


VPC Endpoints(Gateway) [ S3 and dynamo db access only . Same region access only no cross region]
======================================================

1. Provide private access to S3 and DynamoDB.
Private access means they allow a private only resource inside a VPC or any resource inside a private only VPC
to S3 and DynamoDB.
2. S3 and DynamoDB are public space means they are not associated or present inside a VPC. They require a public version 4 address to be accessed by a resource.
Any resource or service which is hosted inside a VPC or private network or lacks ipv4 public address is private by nature.
To make it public infrastructure we have to give access to private resource via a nat gateway present at public subnet.
The nat gateway assign the public ipv4 address and route to VPC router then internet gateway to public space/public AWS service.
Or we can assign IPV6 address to the private resource as in AWS IPV6 is publicly routable.

3.For implementing Gateway endpoints we have to create endpoints for a service in a region and associate it to a VPC.
For example s3 in us-east-1. We have to attach the gateway endpoint to a subnet inside VPC.
4. Prefix list is added to the route table of the VPC where the endpoint is attached.
Prefix list uses the gateway endpoint as target.
Prefix list is a list of IP address for the service and added to route table . It is the destination and 
target is endpoint.
for example any traffic destined for s3 goes via gateway endpoint rather than the Internet gateway.

A rule with destination pl-63a5400a (com.amazonaws.us-east-1.s3) and a target with this endpoints' ID (e.g. vpce-12345678) will be added to the route tables you select below.
Subnets associated with selected route tables will be able to access this endpoint.


5. Endpoint gateway is region and VPC level. it does not goes to subnet or AZ. While setup we have to
choose subnet to be associated. AWS does the routing. It is Region so highly available across all AZ in a region by default.
Region resilient.

6. Endpoint policy : We can configure endpoint policy in gateway to restrict the usage of all parts of Resource
and restrict to certain areas/service/prefix only. For example all outbound request via the endpoint
can access only certain bucket only not all bucket present in s3 for a region.

7. Prevent leaky buckets: S3 bucket are private by default. However we can configure the bucket policy
to allow access only via the gateway endpoint and restricting all other access. S3 bucket are having implicit deny as default.

8. Remember gateway endpoint are logical objects and limitations are they are only accessible from the VPC only
on which they are associated.

9. 
Use case : Private VPC == Public access to a public resource(S3 bucket only,dynamodb)
Confident VPC and doesn't want to give public internet access in anyways so we would just set up
gateway endpoint at VPC level to allow access to only s3 and dynamo db.

10 . Flow:
Gateway endpoint attached to VPC ->Prefix list attached to route table of the subnet-> traffic
from private resource goes to router -> Endpoint -> S3/DynamoDb.
No NatGateway or Internet gateway is required.


VPC Endpoints(Interface) [ Anything except S3 and dynamo db access , PrivateLink]
=================================================================================
1. Provide private access to AWS services.
2. They provide access to anything except S3 and dynamoDB which are provided by Gateway endpoint.
3. Interface endpoints are attached to subnet level. 1subnet = 1AZ . So they are AZ resilience.
Not highly available. You have to associate as much interface endpoint to each subnet for availability.
4. Network access are controlled by security groups and they support TCP and IPV4 only.
5. Endpoint policy can be configured just like gateway endpoint.
6. They use privateLink to inject external third party services inside VPC.
7. In Interface endpoint we have a endpoint specific DNS name which we want to resolve to access that service
for example SNS: vpce-*****sns.us-east-1.vpce.amazonaws.com
There is other DNS name which is private DNS - 	sns.us-east-1.vpce.amazonaws.com
If you want to resolve the private DNS(default service DNS) to the interface endpoint IP then we have to
associate it in route53 private hosted zone.
8.All private or public instance in VPC when tries to reach the private DNS for the service it will resolve to the 
interface endpoint IP address then network flow directly to the Service/resource.

VPC Peering
===========
1. Direct encrypted network link between two VPC's.
2. Uses AWS Global network backbone when using cross-region peering connections. Communication is encrypted.
3. No transitive connection. Peering is always between two VpC.
VPC A - VPC B , VPC B- VPC C. It doesn't mean A and C have connections. To establish connections we have to make
3 VPC peering connections A-B , B-C , A-C. 
4. SG and NACL can be used to filter connections
5. Route table configuration is required at both end of VPC peering connections
to allow traffic from a-b and b-a. 
6. VPC peering can be same account ,cross account , same region, cross-region.
7. No overlapping CIDR. Address range must be different. 


AWS Site-to-Site VPN [ Speed limitations 1.25Gbps]
====================

1. AWS site to site vpn is the quickest way to establish VPN connection between VPC(Virtual private cloud)
and On-premises network or another cloud network.
2. It is a logical connection between VPC and on-premises network encrypted using IPSec, running over the
public internet. Remember it is not a physical network connection.
There is an exception where we can establish site-to-site VPN over direct-connect network and not over public
internet.
3. Components of a VPN network:
a. Virtual Private Gateway(VGW)
b. Customer Gateway(CGW)
c. VPN Configuration

Virtual Private Gateway is the logical entity on the AWS side and is the target for the router. It is associated
in the public space of AWS and connected to public Internet.
Customer Gateway is the logical entity on the AWS side which maps or represent the physical device router on the customer On-premises side.
VPN configuration involves setup and configuration link establishment between VGW and CGW. 

4. Site-to-Site VPN are Highly available provided configuration and architecture and done correctly
to support this.
5. Quick to provision in a hour unlike direct connect or other network where it takes months and weeks
for physical connection to setup.

Architecture and flow:

1. Partial HA with common point of failure as CGW:

										 Endpoint1--------------------\
											|						   \
VPC Side with subnet -> Router -> VGW ---VPN Connection			Customer gateway(CGW) [ On-premises]
								[AWS SIDE]			|				   /			
										 Endpoint2---------------------
										
2.Highly available design with multiple CGW and multiple VPN connection in AWS side.
									
											
										 Endpoint1(AZ A)-----------------------------------------------
											|													|	
											|--Endpoint1(AZ B)----|			   						|
VPC Side with subnet -> Router -> VGW ---VPN Connection	 Customer gateway(CGW 1 in Building1) Customer gateway(CGW2 in Building2) [ On-premises]
								[AWS SIDE]			|				|	  |								|
											|--Endpoint2(AZ B)----									|
											|				   									|
										 Endpoint2(AZ A)-----------------------------------------------
										
a. VGW is created in AWS side and associated with a VPC Connection.
b. Each VGW has two physical endpoints in different AZ with IPv4 public addressing.
c. VPN connection is established between two endpoints and other connection between Endpoints and CGW.
d. VPN set up can be static and dynamic. If we are using static then IP static config has to be updated
in both side of router to set the destination of flow from VPC to On-premise via VGW 
or On-premises to VpC via VGW.
e. As the VPN tunnels are established between endpoint and CGW if HA set up is present
then failure of one tunnel wont affect the data movement and connectivity as other tunnel is active.
f. Static VPN: Route for remote side are added route tables as static routes.
Networks for remote side are configured on the VPN connection.
Doesn't support load balancing and multi connection failover
g. Dynamic VPN: Uses BGP(Borded gateway protocol) between VGW and CGW.
Route propagation if enabled then route table at AWS Side can learn about VPN network connected
to VGW and dynamically added routes. To enable dynamic vpn , CGW must be supporting BGP.

6. VPN have speed limitations of 1.25 GBPS also total cap on VGW for all
connections made is 1.25 gbps.
7. Latency is more as more hops due to public internet transit.
8. Hourly cost, gb out cost, datacap(on-premisis)
9. Benefit is Fast setup.
10.Direct connect (DX) is fast so Site-to-Site VPN can be used as a secondary backup to DX
Also they can used along with DX.

AWS Direct Connect(DX)
=====================

1. AWS Direct connect assigns you a port in one of the DX location.
2. The port is capable of delivering speeds of 1 GBPS(1000-base-lx) or 10GBPS(10GBASE-LR) to the customer router
present in Dx location which requires (VLANS/BGP) protocol.
3. Once the port is set up in DX location , infrastructure physical dedicated
fibre cable has to be setup to extend that connection to the business premises.
4. It can take months weeks for physical cable setup. Also the cross connect setup
from DX port to the customer router has to be done by a human and can take days.
5.Once the DX is setup we can setup multiple VIF(virtual interfaces) over one DX network.
The VIF can be public or private.
In public VIF it allows customer/on-premisis to access aws public service only via
DX and there is no encryption in dx data transit by default.
In private VIF it allows to connect to a single VPC and no encryption.
6. To enable IPSEC encryption we can set up a public VIF and make that VIF as site-to-site VPN
so encryption can be enabled from the virtual gateway endpoints and customer side via setting up
site-to-site vpn upon VIF on DX network.
7. DX is faster so 40GBPS speed for 4 ports in aggregate.No encryption.



AWS Transit Gateway(TGW)
========================

1. Transit gateway is a highly scalable , routing networking device used to connect various vpc's 
and on-premisis VPN network.
2. We need route table configuration to add routes.
3. In Transit gateway it is a hub and all VPN can be connected to the gateway without establishing separate peering connections.
4. It is highly available and scalable network object which significantly reduces latency.
5. we can create a complex architecture of VPC + Site-to-Site VPN + Direct Connect.
6. It supports multiple route tables allowing complex routing architecture.
7. It can peer with same account, cross-account ,same region and different region connections and leverage aws global network features.
share transit gateway with other account.
8. Can integrate with DX using transit VIF.

1. Create a transit Gateway
2. Create transit gateway attachments for each vpc in each subnet of the region.
3. Once both attachments are up, transit gateway route tables must show both
as associations and in Routes both VPC CIDR will be there.
4. Go to each subnet of the VPC add the route subnet for the VPC to talk to with destination as transit Gateway.
Association route table ID tgw-rtb-0b3c53ea0ab08fbd5
Propagation route table ID tgw-rtb-0b3c53ea0ab08fbd5
Do the same for reverse vpc also means A subnet add B route and B subnet add A route.
10.16.0.0/16 tgw-0d1cb59e1205e6735	 active  No

Public service means they are hosted in AWS public space and not within a VPC.
They have public endpoint and anyone can interact with the service provided the resource policy
and Permission policy grants them. For example S3,SNS is a public service but resources are private
by default due to implicit deny.

Private services are the ones which require a VPC to be hosted upon. They can be connected only via a VPC which
may be default VPC or customer managed VPC. They are present in AWS private space.


Storage Gateway [ File gateway, volume gateway[cached,stored], Tape[VTL] gateway]
=================================================================================
1. Storage gateway are hybrid virtual appliance may be present in On-premises or aws.
2. Provides extension of file and volume storage in AWS.
3. Volume storage backups into AWS.
4. Tape backups into AWS and migration of existing Infrastructure into AWS.
5. Basically they provide extension of the storage when your local infrastructure storage is limited so you store
the cached data only in locally and primary copy in AWS.
Store the primary copy in local storage but take asynchronous backups to aws as ebs snapshots.
Large tape library archive storage or backup is s3 glacier.

It is of 3 types:
1. Tape Gateway(VTL): virtual tapes in s3 and glacier
2. File gateway: File storage backed by s3 objects. It uses NFS and SMB protocol
3. Volume gateway : Stored/cached mode using iSCSI . Block storage backed by s3 and EBS snapshots.

File-Gateway: Storage gateway hosted in On-premises having file server running. Connected to it 
are systems and servers using NFS and SMB protocol. File gateway transfers all the storage
via a HTTPS protocol endpoint to s3. The objects are stored in s3 and lifecycle policies can be attached to them
for transition to IA , OneZone-IA. Also it can have Active directory integration for authorisation to file-server in
gateway.

Suitable for decommissioning on-premises files and moving to s3.

Tape-Gateway[iSCSI]: 
Storage gateway is hosted on-premises and facing a backup server. The backup server
was initially connected to tapedrives.Now the gateway behaves to have media-changer and tape-drive using iSCSI.
Transmits all data over https-endpoint to S3 and then to glacier. Active tapes are stored in S3 and Archive
tapes are moved to Glacier. Each tape locally 1PB. Virtual tape 100GIB - TiB.

Suitable for infrastructure which had tape/magnetic tape as storage.

Volume-Gateway[iSCSI]:

a. Volume Gateway(Stored mode): The Gateway is present in on-premises with local storage and upload buffer.
The primary copy is stored on-premise and network servers access the data from gateway using iSCSI.
Now it transmits Asynchronous backups to AWS using HTTPS endpoints to a storage gateway which creates EBS snapshots.
Later the snapshots can be used to create volumes for migration.

b. Volume Gateway(Cached mode): In contract to above, local storage is replaced by cached storage.
Data is not stored locally. Only frequently accessed data is stored in local on-premise. In AWS
data is stored in S3 backed volumes and EBS snapshots.

Snowball / edge/ Snowmobile
============================
1. Snowball

Ordered from AWS , physical storage as device delivered.
50 TB to 80TB capacity and on-premise must have 1 GBPS.
Data encryption at rest using KMS.
Multiple device to multiple premises.
Economical range is 10TB to 10PB. Only storage no computation.

2. Snowball Edge
Both Storage and compute.
Larger capacity then snowball . Fast network 10GBPS.
3 types:
Storage optimised(with ec2) - 1 TB SSD
Compute optimised
Computer optimised with GPU
ideal for remote site where lambda computation or some processing is required while data is moving.

3. Snowmobile:
Portable datacenter truck.
ideal for 10PB+ data. Not multi-site only single location.
100PB per snowmobile.

Directory
=========


The Directory service is a product which provides managed directory service instances within AWS
Must be used within VPC. Supports windows environments.
Can be used in isolation, Integrated with On-premise , Can proxy back request from on-premise.

it functions in three modes

Simple AD - An implementation of Samba 4 (compatibility with basics AD functions)
AWS Managed Microsoft AD - An actual Microsoft AD DS Implementation
AD Connector which proxies requests back to an on-premises directory.


Simple AD[Simple , default, isolation]
Isolated architecture uses open source Samba 4 mode.
Here we deploy the Active directory inside VPC and services like amazon workspace integrates and
provide login to virtual users.
cannot integrate with on-premise AD like microsoft AD.

AWS Managed Microsoft AD [ MS AD DomainService or trust AD DS, Integrated with On-premises]:
Here we deploy the actual microsoft AD service inside the AWS. For establishing trust relationship with On-premises AD
we can use private service like VPN,DX.
Completely microsoft AD DS implementation. Resilient if vpn fails still aws services can use directory services as Actual AD is inside
VPC aws.

AD Connector[ Doesn't store any directory info in cloud, Just a proxy of on-premises]:
Suppose you have just one aws service which require AD services. In this case you don't deploy AD directory inside AWS
instead you just put a AD connector there which then proxies to the AD services deployed in On-premises via a private network.
AWS service will see the connector present but doesnt care whether present in AWS ZONE or on-premises.
Not resilient as if private network fails then service fails due to On-premise has the actual implementation.
Required when just one service needs directory service and no need of deploying brand new AD service.

AWS DataSync
============

1. It transfers the data TO and FROM AWS. Bi-directional transfer of data.
2. Support validation of copied data by default.
3. Use case : migration, datatransfers, dataprocessing,archival, DR/BC.
4. Designed to work at huge scale.
5.It has agent installed which supports 10GBPS per agent.
6. supports encryption,compression,incremental and scheduled transfers.
7. bandwidth throttlers or limiters, automatic recovery of transit errors.
8. Integration of data transfers using endpoint to s3, efs,FSX windows.

Flow.
1. install datasync agent at On-premises which connects to NAS/SAN Storage using SMB,NFS protocol.
2. The datasync agent runs in VMware and communicates to datasync endpoint in AWS side.
3. The endpoint transmits data to S3, fx windows, efs.
4. Can limit transfer limit, throttle and schedule data for transfer of data or avoid transfer at certain time periods.

components:
1.task (job)
2.agent
3.location.

Amazon FSx for Windows[NTFS protocol]
=====================================
Amazon has the EFS shared file system for linux or ec2 machines which uses NFS protocol.
For windows environment support Amazon has come up with a native shared file system server/share known as FSx for windows

1. It is designed for Integration with windows environment and support Managed ActiveDirectory, self AD integration.
2. It is available within a single AZ or multi-AZ within a VPC and provides on-demand or scheduled backups.
3. It is accessible via VPC,VPN,Peering,DX.
4. On-premises connect the file share over SMB protocol. The files are shared as <\\file_id.domain_name\<File_name>
5. Encryption at Rest and transit. Supports Distributed file system(DFS) scale out, File level versioning so that we can restore
old version of file, deduplication.
6. VSS: user driven restores and managed product which had no admin overhead.

Amazon FSx for Lustre [ High performance computing HPC for Linux cluster, POSIX]
==========================================================================

1. It is a managed lustre file system designed for HPC-Linux clients having POSIX
2. Used for bigdata, ML, Financial modelling where high performance computing is required.
3. 100GB throughput and sub-millisecond latency.
4. Two types of deployment :
a. Scratch : These are pure performance deployments. No HA, No Replication. If hardware fails then file-system and
data lost.
b. Persistent - long term storage, HA within a single AZ, self healing.
5. Fsx for lustre is accessible within VPC. ENI is injected to VPC 
and is accessible using VPN,DX.

Architecture.
1. FSx for lustre is backed by s3. All objects are stored in s3 and appear to be present in filesystem.
It has the concept of lazy loading where data is made available in FS only when it is accessed. 
Any modifications to file or archive can be pushed to S3 again using hsm_archive.
2. Reading from and writing to disk based on throughput and IOPS. Also it can read from cache.
3. Backup to s3 available both manual or automatic (0-35 days retention)

AWS Secrets Manager [ Secret rotation, Product Integration]
===========================================================

1. It shares functionality with parameter store.
2. It is designed solely for storing secrets [ password, API keys].
3. usable via SDK(integration), console,CLI,API
4. Supports automatic rotation of keys via AWS lambda provided lambda has a execution role.
5. Integrates with AWS products (RDS) for automatic syncup of secrets when updated in secrets manager.
Lambda having execution role can rotate the key in secrets manager and same updated key of the database instance
can be updated in RDS seamlessly by secrets manager without admin overhead.
6. Secrets are stored in KMS at rest.

Parameter store are useful for storing hierarchical structure storage of keys but cannot rotate keys.
Secrets manager can rotate keys as well as update in all integrated services as managed product.

AWS Shield [ protects against DDOS attacks, Layer 3 and 4, Route53,EC2,ELB,CloudFront and global accelerator]
==========
1. Provides AWS resources with DDoS protection. Protects against layer3 and layer4 DDOS attacks.
2. it comes with two variants: Shield Standard and Shield Advanced.
Shield standard comes free with route53 and cloudFront
Shield advanced comes with a fee of $3000 p.m
It provides additional protection from EC2,ELB,global accelerator along with Route53 and cloudFront.
Also it has specialised DDoS response team and financial insurance for all increasing AWS cost due to attacks.

AWS Web Application Firewall(WAF) [ WEBACL's are associated to ELB ,API Gateway and CloudFront]
==============================================================================================
1. Provides protection against Layer 7 (HTTP/S) firewall.
2  Protection against layer 7 exploits: SQL injection, cross-site scripting, geo-block,rate awareness
3. WAF Rules are defined and included in WEBACL's which is associated with a cloudFront distributed deployed at all edge locations
to filter incoming traffic based on WAF rules.
4. Integrates with API Gateway, ALB, CloudFront.

CloudHSM [ FIPS 140-2 Level 3]
==============================
HSM standards for hardware security module.
HSM can be CloudHSM hosted in Cloud or on-premises HSM Device.

1. KMS is AWS managed and shared service between account but separated.
2. CloudHSM is a True "Single tenant" Hardware security module (HSM)
3. It follows a processing standard of Fully FIPS 140-2 Level 3 . KMS is L2 overall and some l3.
4. It has industry standard API : PKCS#11, java cryptography extension(JCE), cryptoNG(CNG)
5. Cloud HSM is AWS provisioned but managed by customer. AWS has no access to keys. Customer has the
access to tempered proof device. If key lost too difficult to retrieve.
6.KMS integrates with CloudHSM for custom key store service.

Architecture flow of CloudHSM
1. CloudHSM are only deployed in AWS Managed cloudHSM VPC and not in customer managed VPC.
2. In AWS Managed VPC we install multiple cloud HSM device in two AZ in two separate subnets 
and together make a cluster.
3. Now both cloudHSM device can syncup policies,keys in sync when new nodes are added or removed.
4. We have customer managed VPC when ec2 instances are provisioned in two AZ and two subnets.
To inject and provide CloudHSM interface we install ENI's in them . So two ENI in two subnet.
Then we have to install cloudHSM agent in instances to interact.
5. CLoudHSM agent uses the industry standard API CryptoNG, PKCS, JCE to connect to HSM cluster in aws managed vpc.
6. Note AWS has no access to secure area of HSM where keys are stored. It can only run update on the device
bt no access to keys.

Remember:
1. CloudHSM no integration with AWS products eg No S3 SSE.
2. Enable Trasparent data encryption (TDE) for oracle database.
3. If you are using any certicate issuing authority CA then it can protect private keys.
4. Also manage SSL/TLS encryption for web-servers.

So Anything that required cloudHSM device , no integration with AWS services, industry standard API access,
hardware module then CLouhSM is the choice for rest AWS KMS is the idea choice.

Amazon DynamoDB
===============

1. DynamoDB is a wide column store NOSQL database.
2. It is a public NOSQL database-as-a-service(DBaaS) product with key/value document structure.
Fast and single digit latency.
3. It is a managed service and no self-managed service or infrastructure is required.
4. It has manual or automatic provisioned of performance for scaling IN and out or On-demand mode
5. Point in time restore(PITR) and backups are available. Encryption in rest.
6. Capacity means Speeds in dynamoDB which comes at two options RCU and WCU
Read capacity Units(RCU) - 4KB per second
Write capacity Units(WCU) - 1KB per second.
7. Highly available and resilient across AZ.

Architecture:
A DynamoDB table consists of items. Each item can be 400KB in size include data+attribute name.
A Primarykey is defined per table and a primary key can be only partition key
or composite key i.e combination of primary key+sort key.
No rigid schema. Capacity of RCU and WCU is set per table.

On-Demand Backup: 
1. Option of taking a full table copy of a table and restore it to same region or cross-region with index or without index.
2. Also modify encryption settings.

Point in time restore:
1. All changes to the DynamoDB are captured in a series of backups with 1 sec granularity.
We can replay the changes are revert the state of DB to that point within 35 day recovery window
to a new DynamoDB table.

Highlights:
1.NOSQL, Key/value store, Billed on RCU/WCU, storage and features consumed.
2.Can be accessed via CLI, API and Console.

Reading and Writing:
1. Query can only be performed on Partition key + Sort key or only partition key.
2. Inefficient query and full table scan can lead to capacity and RCU consumption whereas the data intended
or returned is less than the RCU consumed.
3. On-demand capacity provisioning is done for unpredictable workloads,unknown,low admin work.
But On-demand capacity can lead to 5 times price against provisioned one.
4. Provisioned capacity can lead to set RCU and WCU per table basis.
5. Every table has burst pool of 300 seconds. So we should rely on this pool
and provision less capacity . If all pool is consumed then it can lead to throttle
and provisioned capacity error.

Read and Write Consistency:
1. In DynamoDb Architecture we have storage nodes among them one is leader node.
Assume we have 3 Storage Nodes in AZ A, AZ B and AZ C. The Storage node in AZ B is the leader node.

Now Consistency in DynamoDB is two types:
1. Eventual Consistency 
2. Strong Consistency

Suppose a user updates removes 4th column of an item, so the write is performed to the leader node.
The leader node takes milliseconds to replicate to other storage nodes. In eventual consistency only 1/3 nodes are checked
for data retrieval. If replication is finished between AZ B and AZ C but AZ A replication is not finished yet, then
any reads from A will lead to stale data retrieval. This is 50% less cost than strong consistency.
In Strong consistency reads are always from leader storage node. Data model to be chosen and query pattern 
depends on the use case. Not only application supports stale data so strong consistency might be the option.
In eventual consistency multiple records are returned for 1 RCU whereas in Strong consistency
exact 1 record is returned.

Calculation of WCU:
Suppose 10 items to be written with each each 2.5KB then
no of WCU needed ?
1 WCU = 1 KB/S.
So No of WCU = Round(size/1)3
(2.5/1)=2.5 Round to 3 size.
No of items =10 , So WCU = 3*10= 30 WCU

Calculation of RCU:
Suppose 10 items to be read with each each 2.5KB then
no of RCU needed ?
1 RCU = 4kb/s
No of RCU = Round(size/4 kb)1
(2.5/4)1= 0.6 Round to 1KB.
For 10 items is 10 RCU (Strong consistency)
For eventual consistency its 5 RCU i.e 50%

DynamoDB Streams and Triggers
=============================

DynamoDB Streams:
1. Streams are ordered list of items changes that occurred to a table.
2. The streams are generated for a 24 hour rolling window.
3. Steams are enabled per table basis. They capture the insert,update and deletes.
4. To the view stream changes we have 4 types:
a. Key only b. Old image c.New Image d. Old + New image.
Key only has the items partition key only or some items partition key + sort key if it's part of composite primary key.
Old Image : It is the old state of record. This view type is somewhere good and efficient as we can query to see the new state of record
and compare the old image presented to us by view.
New Image : This is the new state of record.
old + new image : Both the new and old state are represented.

Triggers:
Triggers are implemented in oracle database but DynamoDB also implements such feature as NOSQL.
1. Any changes in the item of table generates an event.
2. These events are generated from the dynamoDB stream. Stream captures the 24 hour rolling window changes
and any changes generates an event called triggers i.e an action.
3. In AWS triggers are implemented using Streams + lambda i.e; AWS = STREAMS + LAMBDA
4. Lambda function is invoked in response to a change in stream to perform some computation.
The view data is sent as an event to the lambda function which in turn does something as an action.
5. Use-case : Aggregation , Reporting and analytics.

Dynamo DB Local and Global secondary index 
==========================================
Indexes are alternative access pattern. They are views built on top of base table which allows querying on columns which are
prohibited on base table as those attributes are not part of primary key in base table. In Indexes we have the liberty of choosing
our own attributes based on our business scenario access pattern and building our attributes for efficient reading of data.

1. Index are alternative views on table data.
2. DynamoDB allow two types of indexes : 
a. Local Secondary index (LSI)
b. Global Secondary Index(GSI)

Local Secondary Index : 
1.They allow to create a view on top of base table with a different sort key.
2.It has to be created during creation of table.
3.Local secondary index has the strong consistency as they are created same time as base table.
4.Option to choose attribute for view creation : All, keys_only , Include
5.Limitations to 5 LSI per table and they share the WCU and RCU with the table.
6.Indexes are sparse i.e null value for a attribute are not considered and included in the view. So the read 
on the LSI doesn't consume the WCU for the attribute for those items.

Global Secondary Index:
1. Limitations to 20 indexes per table.
2. Present a different perspective of the base table.
3. Allow to choose a different partition key + sort key.
4. Can be created after the base table is created so it has eventual consistency. 
5. DynamoDB suggest to use GSI as default. They have their own RCU and WCU.
6. Attributes can be chosen for All, include_only, keys_only.
7. GSI are sparse same as LSI. Null values omitted for index attributes.

Considerations: Choose the index attributes wisely deciding on the access pattern, if the queries are executed
for non-index attribute that can lead to expensive operation as fetched from base table and cost of RCU.

DynamoDB Global Tables [ Multi Master cross-region replication]
===============================================================
1. They provide the multi master cross-region replication
2. No master-slave or leader architecture. 
3. There is one global table and all other tables are created in multiple region and added to global table configuration.
These tables are replica tables to global tables.
4. Last writer wins for conflict resolution and provides sub-millisecond latency for writes.
5. The reads for strong consistency for same region and eventual consistency for cross-region.
6. since all are master table so reads and writes can occur to any table.
7. Use case: Best suited for large global application as DR/BC and HA is provided.

DynamoDB Accelerator (DAX)
==========================
Traditional system : Application queries for a data, search in cache , if cache miss then separate SDK call to fetch data
from the Dynamodb then update the cache for subsequent reads which will lead to cache hit.

Using DAX : DynamoDB DAX provides ease of single API SDK call to dynamoDB without maintaining separate cache.
It abstracts away the implementation and user feels like querying dynamoDB Database. DAX is tightly integrated with DynamoDB
So whenever user queries DAX it checks it cache and if cache miss occurs it fetches from DynamoDB and updates it cache and returns to user
in sub-milliseconds latency. No admin overhead.
Less complexity for App Developer as the Application just need to install DAX SDK.

Architecture
============

1. DAX is a VPC product. If you to set up DAX cluster inside a VPC with primary cluster in one AZ
and replica cluster in other AZ's for high availability.
2. Writes are done to primary and replicated to replicas. Replicas are for Reads.
3. EC2 instance running an application + DAX SDK. DAX SDK maintain two caches: Query cache and item cache.
Query cache has the complete query along with query/scan parameters.
Item cache hold the batch(getItem)results specifically only the primary key i.e partition key and sort key.
4. Any writethrough operations DAX SDK will connect to dynamoDB and perform write operations then cache it in DAX for efficient reads.
5. It has eventual consistency and not suitable for strong consistency.
6. Any failure of primary node will lead to election and selection of another primary node. They are highly available.
7. It can scale up and out means bigger instances for DAX and addition of multiple instances for DAX.
8. DAX is in-memory cache for fast efficient scaling and reads.

Amazon Athena
=============
1. Athena is super-powerful serverless adhoc querying service.
2. It lets you query data stores in S3 without modifying the source data.
3. We have to define schema in advance and table in data catalog.
It is called schema on read because whenever data is read from S3 it transforms or converts the data into the form defined in schema.
Data is projected through schema. So the original table remains un-modified.
4. Only pay for the data consumed for querying. 
5. Supports many formats of data( csv, json,parquet,avro etc) and data can be structured, unstructured,semi-structured.
Also supports ELB, Cloudtrail and VPC flow logs from AWS services.


ElastiCache [ In memory cache database, Application Code change required, High performance, Redis & MemcacheD]
=================

1. ElastiCache is In-memory database for high-performance.
2. Two types : MemcacheD and Redis.
3. It provides cache of data for read-intensive workloads significantly reducting cost
and workload on database.
4. Can be used to store session data by making server stateless.
5. Cache are managed and stored in external elasticache instead of applications instances
so makes the application fault tolerant and user can continue their session and application access in the same way
without losing session data.
6. But to implement ElastiCache it requires application level code change.
So the application must know how to use elastiCache, perform cache invalidation etc.
Memcached vs Redis:

MemcacheD   				|  Redis
1. Simple DataStructures    1. Advanced DataStructures
2. No Replication			2. Multi-AZ
3. Multiple nodes(sharding) 3. Replication enabled so scale at reads
4. No backup and restore	4. Backup and restore present
5. Best for Multi-threaded 	5. Best for Transactions,ACID, Consistency.
architecture.


Amazon Redshift [ Petabyte Scale DataWarehouse, OLAP, Enhanced VPC routing]
=====================================================

1. Column based Petabyte Scale DataWarehouse designed for OLAP. It is cluster based.
2. Database-as-a-Service similar structure to RDS.
3. Suitable for analysis, aggregation of data.
4. Two important features:
a. Redshift spectrum : Query directly to S3
b. Federated query: Integrated with other DB's,remote db's for performing query on other DB's.
5. Supports integration with tools like Quiksight.
6. JDBC/ODBC connections, sql like interface.
7. Redshift enhanced VPC routing to be enabled for any network related demands.
Redshift uses public internet to interact with external sources but with vpc routing it uses all VPC based routes
to reach external sources.
8. It is not serverless and not highly-available. It is tied to one AZ in a VPC.
9. It has a leader node[Query input,planning & aggregation] . Client/Applications connect to 
leader node first. Compute node[ performing queries of data]. 
The compute node is further divided into slices of 2,4,16,32 which runs parallel to query execution. Each compute node slices
has local storage.
10. VPC security, IAM permission, KMS at rest, CW monitoring.
11. Automatic backups to s3(8hours,5gb), manual backups managed by admin.

Important regarding intergrations:
1. S3 can load and unload data to leader node.
2. Firehose can stream data into Redshift.
3. DynamoDB can copy data.
4. DMS can migrate database
5. Snapshots from S3 backup can be copied to cross-region and new redshift cluster can spin up from the snapshots in other region.

Redshift resilience and Recovery
================================
1. Redshift supports two types of backups:
a. Automated backup : Every 8 hours 5 GB of data to S3 as snapshots.
These snapshots are incremental so only CDC changes. 
Has default retention of 1 day and can be configured to 35 days. 
b. Manual backups : By admin , manual copy of data and creation of snapshots.

As Redshift is tied to VPC and single AZ so incase entire AZ fails , entire region fails
in those cases we can use s3 snapshots as the recovery. As S3 is highly available across region and replicated to 3+ region.
So if one AZ fails we can spin up brand new redshift cluster in another working region. If entire region fails then we can
setup cross region copy of snapshots and provision a new cluster.




