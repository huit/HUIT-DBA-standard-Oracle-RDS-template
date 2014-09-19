HUIT DBA Oracle RDS CloudStack Template
=========================================

Welcome to the HUIT DBA Oracle RDS CloudStack Template github repository.  This repository contains the cloudformation
and scripts needed to build an HUIT DBA standard Oracle database.

The default characteristics of the database are as follows:

DBName (Oracle SID):  oracledb
Oracle adminuser:  oraadmin  (this is a user with privileges like SYS and SYSTEM)
Listener port:  8003 		  (standard secure port recommended by HUIT DBA)
Oracle SGA size:  8G
Allocated db storage: 10G
MultiAZ database: Yes
Database engine:  Oracle Enterprise Edition (oracle-ee)
DBInstanceClass: db.m2.xlarge (possible values are:  db.t1.micro | db.m1.small | db.m1.medium |
                               db.m1.large | db.m1.xlarge | db.m2.xlarge |db.m2.2xlarge | db.m2.4xlarge
BackupRetentionPeriod:  35 days (this is the max allowed in AWS)
CharacterSet: AL32UTF8  (this is the default when DB is created via CloudFormation and is recommended by HUIT DBA)
LicenseType: bring-your-own-license (presumes database is built under Harvard Oracle site license)


By default, the template creates a CIDR db security group allowing inbound access to the listener port from the
HUIT DBA VPN tunnel IP range:  128.103.150.0/24


## Quickstart

To use this repo, start off by setting the required parameters for the CloudFormation template in your environment as follows:

export KeyName=my-key                  (your SSH key name)
export Label=mystack                   (the appropriate name of your stack)
export DBPassword=mypassword           (the password for the oraadamin Oracle user)


Then when ready, you can start the CloudFormation stack using the deploy script:

$> ./deploy create-stack

Note that the deploy.sh script assumes that you have a profile called "HUIT DBA" which is used by the AWS CLI to connect to AWS.


If there are missing parameters, set them as environment variables, and then start again. 
The script will also leave a copy of the generated cloudformation template in the current directory named `template.json`.

To see the list of known parameters you can set in your environment, you can use the helper script

$> ./bin/find_cf_params ./resources/cf.json

For example:

$ ./bin/find_cf_params ./resources/cf.json
DBAllocatedStorage
MultiAZDatabase
Label
DBName
DBClass
DBPassword
DBUsername
KeyName

The helper tools need python 2.7 (which is also a requirement for the AWS cli tools)


There are two helper utilities in `bin/`

* `find_cf_params` : given a cloudformation template, prints out the Parameter names
* `pprint_json` pretty prints out the CF json (or any JSON)





