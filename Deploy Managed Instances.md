
### Step 1: Find the Amazon Linux 2 AMI ID

First, find the Amazon Linux 2 AMI ID for your AWS region. You can do this by using the AWS CLI to describe images. Here's an example command to find the AMI ID in the `us-east-1` region:

```shell
aws ec2 describe-images \
    --owners amazon \
    --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
    --query 'Images[0].ImageId' \
    --output text \
    --region us-east-1
```

### Step 2: Launch Instances

Use the AMI ID obtained in Step 1 to launch four t2.micro instances with the specified configurations. You will need the ARN of the `SM-Workshop-ManagedInstancesRole` IAM role. If you don't have the ARN, you can retrieve it with:

```shell
aws iam get-role --role-name SM-Workshop-ManagedInstancesRole --query 'Role.Arn' --output text
```

Launch instances using the following command template. Replace `<AMI-ID>` with the actual AMI ID and `<ROLE-ARN>` with the ARN of your IAM role:

```shell
aws ec2 run-instances \
    --image-id <AMI-ID> \
    --count 4 \
    --instance-type t2.micro \
    --iam-instance-profile Name=SM-Workshop-ManagedInstancesProfile \
    --associate-public-ip-address \
    --security-groups default \
    --no-cli-pager
```

### Step 3: Tagging Instances

After launching, you'll receive instance IDs in the output. Use these IDs to tag your instances accordingly. Replace `<instance-id>` with your actual instance IDs:

```shell
# Tag for App1
aws ec2 create-tags \
    --resources <instance-id-1> \
    --tags Key=Name,Value=App1

# Tag for App2
aws ec2 create-tags \
    --resources <instance-id-2> \
    --tags Key=Name,Value=App2

# Tag for Web1
aws ec2 create-tags \
    --resources <instance-id-3> \
    --tags Key=Name,Value=Web1

# Tag for Web2
aws ec2 create-tags \
    --resources <instance-id-4> \
    --tags Key=Name,Value=Web2
```

### Notes

- The `--associate-public-ip-address` flag automatically assigns a public IP to your instances, assuming your subnet is set to do so.
- For security groups, this example uses the default VPC security group. Ensure the `default` security group allows inbound SSH (port 22) traffic for SSH access, or use Session Manager for remote access without SSH.
- You might need to adjust the `--security-groups` parameter based on your VPC's setup. If you're using a non-default VPC or security group, specify the appropriate security group ID with `--security-group-ids`.
- The `--no-cli-pager` option prevents the AWS CLI from paging output for commands that produce a lot of outputs, such as `run-instances`.

Remember to replace placeholders with actual values for your AWS environment.
