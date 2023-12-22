
JQ cheatsheet:


https://gist.github.com/lukeplausin/b64c10f8b524bb310e0083756c42caf6

Ref: https://github.com/mdminhazulhaque/aws-cli-cheatsheet


Ref: https://medium.com/circuitpeople/aws-cli-with-jq-and-bash-9d54e2eabaf1

## AWS Code Commit:

#aws #versioncontrol #git #ssh 

Ref: https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html?icmpid=docs_acc_console_connect_np

Create code commit ssh key - need to use cli next time

```
create config file in ssh directory
Host git-codecommit.*.amazonaws.com
  User APKAEIBAERJR2EXAMPLE
  IdentityFile ~/.ssh/codecommit_rsa
```

Then test connection:
```bash
chmod 600 config

ssh git-codecommit.us-east-2.amazonaws.com - sub region as needed
```

Then you can clone the repo as usual and do your thing:
```bash
git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/MyDemoRepo my-demo-repo

```

## Cloudfront

```bash

# get list of cloudfront distros

aws cloudfront list-distributions | jq -r '.DistributionList.Items[] | .DomainName+" "+.Origins.Items[0].DomainName'
```

## Config

```bash
ConfigService get rule names that do not contain a string

aws configservice describe-config-rules --profile $PROFILE_NAME --region us-east-1 | jq -c '.ConfigRules .[] select(.ConfigRuleName test("AWSControlTower") not) .ConfigRuleName'
```


## DynamoDB

```bash

# get list of tables
aws dynamodb list-tables | jq -r '.TableNames[]'
```
## EC2

## ECR

```bash

# list of repos

aws ecr describe-repositories | jq -r '.repositories[] | .repositoryName'
```


```bash
# list instances with instanceId, launchTime, state and stateReason

aws ec2 describe-instances --region us-east-2 jq "[.Reservations .[] .Instances .[] {instanceId: .InstanceId, launchTime: .LaunchTime, state: .State, stateReason: .StateReason}]"

# list instances with State and stateReason and launchTime

aws ec2 describe-instances --region us-east-2 jq "[.Reservations .[] .Instances .[] 
{instanceId: .InstanceId, launchTime: .LaunchTime, state: .State, stateReason: .StateReason}]"


# list instances that are running with instanceId and launchTime

aws ec2 describe-instances --region us-east-2 "select(.State.Name == \"running\")" jq "[ .Reservations .[] .Instances .[] {instanceId: .InstanceId, launchTime: .LaunchTime, state: .State, stateReason: .StateReason} ]"

# list VPC's and CIDRs

aws ec2 describe-vpcs | jq "[.Vpcs .[] .CidrBlock ]"

# list subnets with subnetId and AZ

aws ec2 describe-subnets | jq "[.Subnets .[]{Subnet:.SubnetId,AZ:.AvailabilityZone}]"

# list amiId , instanceId and privateIp

aws ec2 describe-instances --profile $PROFILE_NAME --region us-east-1 jq -c '.Reservations .[] .Instances .[] select (.ImageId == ${AMIID}) .InstanceId,.PrivateIpAddress'


# describe-key-pairs that start with string

aws ec2 describe-key-pairs --profile $PROFILE_NAME --region us-east-1 | jq -c '.KeyPairs .[] select( .KeyName startswith("test-string"))'

aws ec2 describe-instances select "Name" tags, launchTime, PubDNSName


aws ec2 describe-instances --profile $PROFILE_NAME --region us-east-1 | jq '.Reservations .[] .Instances .[] [(.Tags[]select(.Key=="Name").Value), .LaunchTime, .PublicDnsName ]'


__ [
____ "instance1",
____ "2021-07-30T18:55:23+00:00",
____ "ec2-3-86-245-113.compute-1.amazonaws.com"
__ ]
__ [
____ "instance2",
____ "2021-07-30T18:55:23+00:00",
____ "ec2-52-91-167-140.compute-1.amazonaws.com"
__ ]

# EC2 Name Tags

aws ec2 describe-instances --profile ${PROFILE_NAME} --region us-east-1 jq 
'.Reservations .[] .Instances .[] [(.Tags[]select(.Key=="Name").Value)]'

# PrivateIpAddress

aws ec2 describe-instances --profile $PROFILE_NAME --region us-east-1 jq '.Reservations .[] .Instances .[] [(.Tags[]select(.Key=="Name").Value), .LaunchTime, .PrivateIpAddress ]'

# KeyName and PrivateIpAddress

aws ec2 describe-instances --profile $PROFILE_NAME --region us-east-1 jq '.Reservations .[] .Instances .[] [(.Tags[]select(.Key=="Name").Value), .KeyName, .PrivateIpAddress ]'


# Search for instances with Name tags

aws ec2 describe-instances --profile $PROFILE_NAME --region us-east-1 jq -c '.Reservations .[] .Instances .[] [(.Tags[]select(.Key=="Name").Value), .LaunchTime, .PublicDnsName ]'

# Filter for runing instances

aws ec2 describe-instances --query "Reservations[*].Instances[*].{PublicIP:PublicIpAddress,PrivateIP:PrivateIpAddress,Name:Tags[?Key=='Name']|[0].Value,Type:InstanceType,Status:State.Name,VpcId:VpcId,Id:InstanceId}" --filters "Name=instance-state-name,Values=running" --output table

# List first 10 images owned by AWS:


aws ec2 describe-images --owner amazon --query 'Images[0:10].[ImageId,Name]' --output text

# List the first 5 images with ubuntu in the name (useful for demos):

aws ec2 describe-images --owner amazon \
  --query 'Images[?starts_with(Name, `ubuntu`) == `true`]|[0:5].[ImageId,Name]' \
  --output text

# publicly exposed EBS snapshots


aws ec2 describe-snapshots --restorable-by-user-ids all

aws ec2 describe-snapshots --restorable-by-user-ids all --owner-ids 106560262141

# Find the right ami

export AMI_ID=$(aws ec2 describe-images --owners amazon | jq -r ".Images[] | { id: .ImageId, desc: .Description } | select(.desc?) | select(.desc | contains(\"Amazon Linux 2\")) | select(.desc | contains(\".NET 6\")) | .id")


export AMI_ID=$(aws ec2 describe-images --owners amazon | jq -r ".Images[] | { id: .ImageId, desc: .Description } | select(.desc?) | select(.desc | contains(\"Kali Linux\")) | select(.desc | contains(\".NET 6\")) | .id")

#GBs of volumes present in account

aws ec2 describe-volumes | jq -r '.Volumes | [ group_by(.State)[] | { (.[0].State): ([.[].Size] | add) } ] | add'


```

## IAM

```bash
# Add All Users to a Specific Group
$ for i in `aws iam list-users --profile $PROFILE_NAME jq ".[] .[] .UserName" sed 's/"//g'`; do aws iam add-user-to-group --user-name ${i} --group-name ReadOnly --profile $PROFILE_NAME; done



# Search for an IAM user across multiple profiles
$ for i in dev qa production; do echo $i; aws iam list-users --profile $i jq '.[] .[] select(.UserName startswith("jdoe"))'; done

#list IAM users

aws iam list-users --output table

# when was my IAM account created

aws iam get-user | jq -r ".User.CreateDate[:4]"


# Which account am I using

{ aws sts get-caller-identity & aws iam list-account-aliases; } | jq -s ".|add"

# userid & username
aws iam list-users | jq -r '.Users[]|.UserId+" "+.UserName'

#create access key quick

aws iam create-access-key --user-name audit-temp | jq -r '.AccessKey | .AccessKeyId+" "+.SecretAccessKey'


# list Groups

aws iam list-groups | jq -r '.Groups[].GroupName'




```


## MFA

```bash
# To use mfa in the AWS CLI: 

# copy the MFA serial arn to mfa_serial in .aws/config

mfa_serial=arn:aws:iam::ACCOUNT:mfa/IAM_USERNAME
```



## Route53

```bash
# list of hosted zones

aws route53 list-hosted-zones | jq -r '.HostedZones[]|.Id+" "+.Name'
```
## S3

```bash

# filter for bucket names:
aws s3api list-buckets | jq -r '.Buckets[].Name'


```


## Tagging

```bash
# view resources with the name tag - useful for clean-up

aws resourcegroupstaggingapi get-resources --tag-filters Key=name,Values=KEY --output=table

```
```

Turn off less output from CLI:
#awscli #terminal #commandline #environment-variables

```bash
add to .zshrc or .bashrc

export AWS_PAGER=
```


## VPC


```bash

#Filter for VPC CIDR

aws ec2 describe-vpcs --output text --query "Vpcs[*].{VpcId:VpcId,Name:Tags[?Key=='Environment'].Value|[0],CidrBlock:CidrBlock}" 

# filter to vpc id based on specified tag
aws ec2 describe-vpcs --filter Name=tag:User,Values=account-admin | jq -r ".Vpcs[].VpcId"

```
## AWS CLI in Docker 
#cli #awscli #docker 

```
AWS in Docker (using aws_vault):

alias aws='docker run --rm -it -a stdout -a stdin -e $AWS_DEFAULT_REGION -e AWS_ACCESS_KEY_ID -e AWS_SECRET_ACCESS_KEY -e AWS_SESSION_TOKEN -e AWS_SESSION_EXPIRATION amazon/aws-cli:latest'
```


## MISC commands

Get current number of aws services

```
curl -s https://raw.githubusercontent.com/boto/botocore/develop/botocore/data/endpoints.json | jq -r '.partitions[0].services | keys[]' | wc -l

```