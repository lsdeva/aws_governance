
### Step 1: Create the KMS Key

First, create a symmetric KMS key:

```shell
aws kms create-key \
    --description "Key for session manager" \
    --tags TagKey=Purpose,TagValue=SessionManager \
    --output text \
    --query 'KeyMetadata.Arn'
```

This command creates a new KMS key with a description and tags, then outputs the ARN of the created key. Note this ARN for later use.

### Step 2: Create an Alias for the Key

After creating the key, assign an alias to it for easier reference:

```shell
aws kms create-alias \
    --alias-name alias/session-manager \
    --target-key-id <key-id>
```

Replace `<key-id>` with the key ID part of the ARN returned from the previous command. The key ID is the part of the ARN after `key/`.

### Step 3: Define Key Administrative Permissions

To allow an IAM role (`TeamRole`) administrative permissions to the key, attach a policy to the role that grants these permissions. Here's an example policy statement that you would add to the role's policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:*"
            ],
            "Resource": "<kms-key-arn>"
        }
    ]
}
```

You need to replace `<kms-key-arn>` with the ARN of the KMS key you created. You can attach this policy directly to the `TeamRole` IAM role using the AWS Management Console or the AWS CLI.

### Step 4: Define Key Usage Permissions

Similarly, to grant the EC2 IAM instance profile role (`SM-Workshop-ManagedInstancesRole`) usage permissions on the key, attach a policy to this role that includes the necessary KMS actions, such as `kms:Encrypt`, `kms:Decrypt`, `kms:ReEncrypt*`, `kms:GenerateDataKey*`, and `kms:DescribeKey`.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:ReEncrypt*",
                "kms:GenerateDataKey*",
                "kms:DescribeKey"
            ],
            "Resource": "<kms-key-arn>"
        }
    ]
}
```

Replace `<kms-key-arn>` with your KMS key's ARN. Attach this policy to the `SM-Workshop-ManagedInstancesRole`.

### Note

The AWS CLI does not directly support defining specific administrative or usage permissions during key creation. Instead, you manage permissions by attaching policies to IAM roles or users that specify the ARN of the KMS key. Ensure you have the necessary permissions to create keys and manage policies for roles in your AWS account.
