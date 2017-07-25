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

## CodeDeploy

### Deployment Logs

```bash
cd /opt/codedeploy-agent/deployment-root/deployment-logs
tail codedeploy-agent-deployments.log
```

## Elastic Beanstalk

### App Directory

```bash
/var/app/current
```

### Logs

```bash
less /var/log/eb-activity.log
```
