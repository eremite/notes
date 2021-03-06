# AWS

## IAM

### Setup login role for client

* Login to the client's root account.
* Go to Account and find their AWS Account ID
* Create a role for cross-account access between accounts you own
  * Get Account ID from Custom Bit
  * Name: `custombit`
  * Attach the `AdministratorAccess` policy.
* In the CustomBit IAM create a custombit@client policy.
```json
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource": "arn:aws:iam::11112222333:role/custombit"
    }
}
```
* Attach the policy to user(s)
* In the navbar user dropdown choose "Switch Roles"
* Account is the Account ID, Role is `custombit`

## Elastic Beanstalk

```bash
cd /var/app/current # Your application
less /var/log/eb-activity.log # Elastic Beanstalk Activity logs
tail -f /var/log/*.log /var/log/puma/*.log /var/log/nginx/*.log # Tail the logs
cd /opt/elasticbeanstalk/hooks/ # See the automatic hooks
/opt/elasticbeanstalk/bin/get-config container # List Elastic Beanstalk config
/opt/elasticbeanstalk/bin/get-config container -k app_staging_dir # Get a specific value
source $(/opt/elasticbeanstalk/bin/get-config container -k support_dir)/envvars # Load environment variables
```

## How to get the instance id from within an ec2 instance?

`wget -q -O - http://169.254.169.254/latest/meta-data/instance-id`

## CloudWatch Log Insights

Requests that take longer than 75 seconds

```
fields @timestamp
| parse @message " duration=* " as duration
| parse @message " path=* " as path
| parse @message "method=* " as method
| sort @timestamp asc
| filter duration > 75000
```

Count hits to a path within a 15 minute interval

```
filter @message like "path=/integrations"
| stats count(*) as hits by bin(15m)
```
