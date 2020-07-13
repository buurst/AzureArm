# CloudFormation
## CloudFormation Template Examples for Launching SoftNAS

There are two templates. One is a template for a single instance and the other template is for HA pair. The required parameters used in the template are located at the top of the JSON inside the ```Parameters {}``` section. Most values are empty. You can either define them as the defaults in the parameters section or pass them in at template creation time using the aws cli. You can also use the CloudFormation console in AWS to upload the JSON and visually edit the parameters and launch from there.

Once the template launch completes you can browse to the private IP of the instance via  HTTPS and log in. The default password to log into the SoftNAS instance Web UI will be the instance-id. The default username will be 'softnas'

### Parameter Keys Explained
```
KeyName : The SSH key that will be used to access via ssh (if ever required).
MyRegion : The AWS region that you want to launch in.
NasType : The AWS instance size to launch the SoftNAS image on.
AllowedSG : Primary security group id that should have access, will be added as default.
MySubnet1 : Subnet ID to launch primary NAS in.
MySubnet2 : Subnet ID to launch secondary NAS in (for HA template only)
SoftnasAMI : The AMI ID for SoftNAS in your region (get from marketplace page).
AlertEmail : Email address where the mionitoring alerts will be sent to.
NewIamRole : Create the IAM role if you do not have it created? (yes or no)
LicenseCapacity : 1TB, 10TB, 20TB, 50TB (Will create that much storage)
Prefix : This string will prefix all the resource names so you can easily identify them.
```
<br/>

### Example of passing in parameters for an HA pair
```
# give it a name that will also serve as a prefix tag for all the resources
name="SoftnasTest"
```
### Now launch the template using aws CLI and pass in all the parameters using the --parameters falg:
```
aws cloudformation create-stack --region us-west-2 --stack-name $name --template-body file://Softnas_Private_HA_Pair.json --capabilities CAPABILITY_NAMED_IAM --parameters\
 ParameterKey=KeyName,ParameterValue=my_test_key\
 ParameterKey=MyRegion,ParameterValue=us-west-2\
 ParameterKey=NasType,ParameterValue=r5.xlarge\
 ParameterKey=AllowedSG,ParameterValue=sg-0a1f0496a31d6c713\
 ParameterKey=MySubnet1,ParameterValue=subnet-089eaeefe23a044c0\
 ParameterKey=MySubnet2,ParameterValue=subnet-0660fb4fe82c5a7b2\
 ParameterKey=SoftnasAMI,ParameterValue=ami-0453c7da5fbe82bb9\
 ParameterKey=AlertEmail,ParameterValue=MyAlertEmailAddress@mydomain.com\
 ParameterKey=NewIamRole,ParameterValue=no\
 ParameterKey=LicenseCapacity,ParameterValue=1TB\
 ParameterKey=Prefix,ParameterValue="$name"
```
<br/>

### Example of passing in parameters for a single instance

```
# give it a name that will also serve as a prefix tag for all the resources
name="SoftnasSingle"
```
### Now launch the template using aws CLI and pass in all the parameters using the --parameters falg:
```
aws cloudformation create-stack --region us-west-2 --stack-name $name --template-body file://Softnas_Private_Instance.json --capabilities CAPABILITY_NAMED_IAM --parameters\
 ParameterKey=KeyName,ParameterValue=my_test_key\
 ParameterKey=MyRegion,ParameterValue=us-west-2\
 ParameterKey=NasType,ParameterValue=r5.xlarge\
 ParameterKey=AllowedSG,ParameterValue=sg-0a1f0496a31d6c713\
 ParameterKey=MySubnet1,ParameterValue=subnet-089eaeefe23a044c0\
 ParameterKey=SoftnasAMI,ParameterValue=ami-0453c7da5fbe82bb9\
 ParameterKey=AlertEmail,ParameterValue=MyAlertEmailAddress@mydomain.com\
 ParameterKey=NewIamRole,ParameterValue=no\
 ParameterKey=LicenseCapacity,ParameterValue=1TB\
 ParameterKey=Prefix,ParameterValue="$name"
```

Reach out to Buurst support if you have any questions

