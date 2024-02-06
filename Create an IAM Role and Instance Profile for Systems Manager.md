To perform the instructions you've provided using the AWS CLI, we will break down the process into CLI commands corresponding to each step. This will involve using the `aws iam` command set to create a role and attach the necessary permissions policy for Systems Manager managed instances.

1. **Create the IAM Role**: This step involves creating a role with a trust relationship that allows EC2 instances to assume this role.

```shell
aws iam create-role \
    --role-name SM-Workshop-ManagedInstancesRole \
    --assume-role-policy-document file://trust-policy.json
```

For this command to work, you need to have a file named `trust-policy.json` in your current directory with the following trust relationship policy for EC2:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

2. **Attach the AmazonSSMManagedInstanceCore Policy**: Once the role is created, attach the `AmazonSSMManagedInstanceCore` policy to grant the necessary permissions for Systems Manager.

```shell
aws iam attach-role-policy \
    --role-name SM-Workshop-ManagedInstancesRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
```

3. **Create an Instance Profile**: IAM roles by themselves cannot be directly associated with EC2 instances. You need to create an instance profile and add the role to it.

```shell
aws iam create-instance-profile \
    --instance-profile-name SM-Workshop-ManagedInstancesProfile
```

4. **Add the Role to the Instance Profile**: Finally, add the role to the instance profile created in the previous step.

```shell
aws iam add-role-to-instance-profile \
    --instance-profile-name SM-Workshop-ManagedInstancesProfile \
    --role-name SM-Workshop-ManagedInstancesRole
```

After executing these commands, you will have successfully created an IAM role with the necessary permissions for Systems Manager managed instances and associated it with an instance profile. This profile can then be attached to new or existing EC2 instances to manage them through AWS Systems Manager.
