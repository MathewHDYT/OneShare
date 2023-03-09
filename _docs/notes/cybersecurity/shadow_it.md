---
layout: default
title: Shadow IT
parent: CyberSecurity
grand_parent: Notes
---

## Shadow IT

### Meaning:

Sometimes business units go around corporate IT, procurement, legal, and security when they need to get the job done quickly. This leads to the security team not knowing what they need to protect and systems not built to IT or Security standards.

**Public Cloud** is an easy way for business units to engage in Shadow IT. And the most accessible public cloud to get started with is `AWS`. 

### Amazon AWS:

Amazon `AWS` is a public cloud service provider. Most major enterprises leverage `AWS` is some form or another for Computer Services, Big Data or Machine learning, Data Archive, Video Streaming, IoT, etc.

#### These are all the services AWS supports:

![AWS](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/aws.png) 

`AWS` divides its infrastructure into Regions, mostly independent clusters of datacenters.
Within each region are `availability zones` (`AZ`). Each `AZ` in a region leverages separate power grids and usually are located in different flood plains.
This redundancy allows you to establish highly resilient architectures to withstand significant weather or geological event, or more frequently, hardware or facility failures. 

Because regions are independent, you will get different answers to question depending on the region you are querying. You can specify a region with the `--region` option.

### Amazon S3:

Amazon S3 (`Simple Storage Service`) is their hosted object storage service.
`Objects` are stored in Buckets, meaning key-value stores with the Object key being a full path name for a file and the value being the contents of the file.
S3 is a publicly hosted service, it doesn't exist behind a corporate firewall, making it convenient for hosting public content.

AWS Buckets use a global namespace. Only one AWS customer can create a bucket named `bestfesivalcompany-images`.

### Discovering Bucket Names:

One of the easiest ways to discover buckets is when a company embeds content hosted in S3 on their website. **Images**, **PDFs**, etc., can all be hosted cheaply in S3 and linked from another site. 

These links will look like this: 

```
http://BUCKETNAME.s3.amazonaws.com/FILENAME.ext
```

or

```
http://s3.amazonaws.com/BUCKETNAME/FILENAME.ext
```

### Listing the Contens of Buckets:

Amazon S3 is one of AWS's oldest services. Meaning that it has two different methods of access control:
- Bucket Policies
- S3 ACLs

Therefore, many buckets contain public information which furthermore allow you to list the contents of the bucket with the command. To use this download [curl](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

```
curl http://irs-form-990.s3.amazonaws.com/
```

Additionally, AWS CLI also provides the ability to list the contents of a bucket, which additionally parses the XML to make it more readable or tools like Beautify XML can be used to convert the result into something more readable.

```
aws s3 ls s3://irs-form-990/ --no-sign-request
```

The option `--no-sign-request` allows you to request data from S3 without being and AWS Customer.

### Downloading Objects:

Downloading an object from S3 is also easy. 

You can use curl: 

```
curl http://irs-form-990.s3.amazonaws.com/201101319349101615_public.xml
```

or the `AWS` CLI: 

```
aws s3 cp s3://irs-form-990/201101319349101615_public.xml . --no-sign-request
```

Note the two different URIs for an object. Objects can be addressed with `http://` or via `s3://`

Additionally, if `curl` can't show the file directly (`.zip`), we can use the `--ouput` option to copy the output into a new file.

```
curl http://s3.amazonaws.com/images.bestfestivalcompany.com/wp-backup.zip --output output.zip
```

### The different levels of Amazon S3 Authentication:

In `Amazon S3, Object` permissions are different from `Bucket` permissions. `Bucket` permissions allow you to list the `Objects` in a `Bucket`, while the Object's permissions will enable you to download the Object. In the case of the irs-form-990 bucket, both the bucket and all the objects in the bucket are publicly readable.
But that isn't always the case. `Objects` can be readable while the `Bucket` is not, or the `Bucket` can be publicly readable, but the `Objects` inside are not. 

Additionally, you can also have public write permission to a `Bucket`. This is generally a bad idea and has been the vector of several crypto mining incidents. 

There are generally two levels of public `Buckets` and `Objects`:
- **Anyone**, you can just simply hit the `URL` from your local browser
- **Public**, you can just simply hit the `URL` from your local browser as long as you are a `AWS Customer` (what `AWS` foolishly called `AuthenticatedUsers` for many years). Anyone with a credit card can create a `AWS` account, therefore Authenticated Users doesn't provide much data protection either

**ACL NAME ** | **BUCKET** | **OBJECT** |
------------- | ---------- | ---------- |
Anyone | Anonymously lists contents of the bucket via curl or with `aws s3 ls --no-sign-request` | Ability to download via curl or `aws s3 cp --no-sign-request` |
AuthenticatedUsers | Can only list the bucket with active AWS keys via `aws s3 ls` | You can only download the object with active AWS Keys via `aws s3 cp` |

### AWS IAM: 

Excluding a few older services like Amazon S3, all requests to AWS services must be signed. This is typically done behind the scenes by the AWS CLI or the various Software development Kits that AWS provides. The signing process leverages IAM Access Keys. These access keys are on of the primary ways an AWS account is compromised.

### IAM Access Keys: 

IAM Access Keys consist of an **Access Key ID** and the **Secret Access Key**.

**Access Key IDs** always begin with the letters AKIA and are 20 characters long. These act as a username for the AWS API. The Secret Access Key is 40 characters long, AWS generates both strings, however AWS doesn0t make the Secret Access Key available to download after the initial generation.

There is also another type of credentials called short-term credentials, where the **Access Key ID** begins with the letters ASIA and includes, and additional string called the Session Token.

To find **Access Key IDs** the File Explorer can be adjusted to search for text inside a given file extension or to always search for text inside. As well as other ways to search directories recursively for files can be used.

### Conducting Reconnaissance with IAM:

When you find credentials to AWS, you can add them to your `AWS` Profile in the `AWS` `CLI` for this, you use the command:

```
aws configure --profile PROFILENAME
```

This command will add entries to the `.aws/config` and `.aws/credentials` file in the users home directory.

Once the profile is configured with the new access keys, you can execute any command using this other set of credentials.
For example, to list all the S3 Buckets in the `AWS` account you have found credentials for: 

```
aws s3 ls --profile PROFILENAME
```

**ProTip**: Never store a set of access keys in the `[default]` profile.
Doing so forces you always to specify a profile and never accidentally run a command against an account you don't intend to.

#### A few other common AWS reconnaissance techniques are:
1. Finding the Account ID belonging to an access key: `aws sts get-access-key-info --access-key-id AKIAEXAMPLE  --profile PROFILENAME`
2. Determining the Username the access key you're using belongs to: `aws sts get-caller-identity --profile PROFILENAME`
3. Listing all the EC2 instances running in an account: `aws ec2 describe-instances --output text --profile PROFILENAME`
4. Listing all the EC2 instances running in an account in a different region: `aws ec2 describe-instances --output text --region us-east-1 --profile PROFILENAME`
5. Listing all the Secrets running in an account in a different region: `aws secretsmanager list-secrets --region us-east-1` or getting the region from a profile: `aws secretsmanager list-secrets --profile PROFILENAME`
6. Listing the value of a Secret running in an account in a different region: `aws secretsmanager get-secret-value --secret-id SECRETID --profile PROFILENAME --region us-east-1`

### AWS ARNs: 

An Amazon `ARN` is Amazons way of generating a unique identifier for all resources in the `AWS` Cloud.
It consists of multiple strings separated by colons. 

#### The format is:
```
arn:aws:<service>:<region>:<account_id>:<resource_type>/<resource_name>
```

### AWS Elastic Container Registry (ECR):

Is an online registry for public and private container images.
