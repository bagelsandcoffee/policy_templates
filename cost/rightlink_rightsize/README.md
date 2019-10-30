## RightLink Rightsize Instances

### What it does

This Policy Template uses data from the RightLink 10 monitoring metrics api to rightsize the  instance after user approval

### Functional Details

The policy leverages the CMP API monitoring metics api to rightsize instances using the provided CPU and Memory thresholds.  If the instance can be rightsized the incident escalation will automatically change the instance size after the users approval.

There are two policy templates required to support this policy, `RightLink Rightsize Instances` and `RightLink Rightsize Instances Add Tags`.

The RightLink Rightsize is used to actually downsize instances based on RightLink monitoring metrics. This policy will  resize the instance. This required the instance to be **stopped**.
If a server is marked `N/A`, no action will be taken and only the resize tag will be removed. You will need to manually move that instance to another family type.
**_This policy requires [RightLink 10](http://docs.rightscale.com/rl10/getting_started.html) with monitoring enabled and collecting metrics_**, see [Installation](http://docs.rightscale.com/rl10/about.html)


### Input Parameters

#### RightLink Rightsize Instances

- Average free memory percent to allow for downsize - Value: 0-100, -1 disables this metric
- Maximum free memory percent to allow for downsize - Value: 0-100, -1 disables this metric
- Maximum cpu idle percent to allow for downsize - Value: 0-100, -1 disables this metric
- Average cpu idle percent to allow for downsize - Value: 0-100, -1 disables this metric
- Instance tags used to filter instances that must validate policy. Example: rs_monitoring:resize=1
- Email address to send escalation emails to - Example: noreply@example.com
- Days to cooldown between checks of same machine - Number of days to cooldown between checks of the same instance. This drives the `RightLink Rightsize Instances Add Tags`

#### Policy Actions

The following policy actions are taken on any resources found to be out of compliance.

- Downsize instances after approval
- Send an email report

#### RightLink Rightsize Instances Add Tags

- Instance tags used to filter instances that must validate policy. Example: rs_monitoring:resize=1
- Email address to send escalation emails to - Example: noreply@example.com

#### Policy Actions

The following policy actions are taken on any resources found to be out of compliance.

- Add or remove tags for rightsizing
- Send an email report


### Required Permissions

This policy requires permissions to access RightScale resources (clouds, instances and tags).  Before applying this policy add the following roles to the user applying the policy.  The roles should be applied to all Accounts where the policy will run or the Organization. For more information on modifying roles visit the [Governance Docs](https://docs.rightscale.com/cm/ref/user_roles.html)

- Cloud Management - Observer

### Supported Clouds

- AWS
- Azure
- Google
- VMWare w/ RCA-v

### Cost

This Policy Template does not launch any instances, and so does not incur any cloud costs.