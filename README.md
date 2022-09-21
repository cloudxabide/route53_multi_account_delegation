# route53_multi_account_delegation
An example of how to delegate DNS management across accounts

## Status
This is a work in progress.  
I'll be adding some images and more explanation, but this should get the point across to someone who has already been in this situation.

## Issue
Let's say your organization owns a domain, in this case: clouditoutloud.com.  This domain was registered via AWS and is managed in that same AWS account.  
After some time, the "OCP Developer team" asks the "CORP IT team" for the ability to manage their own frequent DNS updates from their own AWS account - in this particular case, to test OpenShift Container Platform.  
Then, another team wants to test AWS EKS, you'll repeat the process using the "EKS Developer team" account


## Steps (high-level)
* Register domain (clouditoutloud.com)
* Create a Hosted Zone (public) for clouditoutloud.com in "CORP IT account"
* Create a Hosted Zone (public) for ocp.clouditoutloud.com in "OCP Developer account"
  *  Note the NS records to provide to the CORP IT team
* Create Record in the CORP IT account:
  *  Record Name:  "ocp.clouditoutloud.com"
  *  Record Type:  NS
  *  Value(s):  4 x NS servers selected when you created the "ocp.clouditoutloud.com" Hosted Zone in the OCP Developer account

To create an OpenShift 4 Cluster using this new domain, use the following values (in your install-config.yaml):
```
export OCP_BASE_DOMAIN=ocp.clouditoutloud.com
export OCP_CLUSTER_NAME=testcluster
```

install-config.yaml
  * baseDomain: $OCP_BASE_DOMAIN
  * metadata:name: $OCP_CLUSTER_NAME

## Discussion Points
* why not just use [Resource Access Manager (RAM)](https://aws.amazon.com/ram/)?  

## References

