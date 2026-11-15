# AWS IAM – Create and Attach a Customer-Managed IAM Policy

## Objective

Create a customer-managed IAM policy that provides read-only access to a specific Amazon S3 bucket, attach the policy to an IAM user, and verify allowed and denied permissions using the AWS IAM Policy Simulator.

## AWS Resources

| Parameter | Value |
|---|---|
| AWS Service | IAM and Amazon S3 |
| S3 Bucket Name | `meghanadh-2410080031` |
| IAM Policy Name | `S3ReadOnlyCustomPolicy` |
| IAM Username | `S3ReadOnlyUser` |
| Policy Type | Customer-Managed |
| Access Type | Read-Only |

## Step 1: Open AWS IAM

1. Sign in to the AWS Management Console.
2. Search for **IAM** and open **Identity and Access Management (IAM)**.
3. Select **Policies** from the left navigation panel.

## Step 2: Create an S3 Bucket

1. Open the **Amazon S3** service.
2. Click **Create bucket**.
3. Enter bucket name: `meghanadh-2410080031`.
4. Select the required AWS region.
5. Keep **Block all public access** enabled.
6. Keep other settings as defaults.
7. Click **Create bucket**.

## Step 3: Create a Customer-Managed IAM Policy

1. Go to **IAM → Policies**.
2. Click **Create policy**.
3. Open the **JSON** editor and replace the default content with:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::meghanadh-2410080031"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::meghanadh-2410080031/*"
    }
  ]
}
```

4. Click **Next**.
5. Name the policy `S3ReadOnlyCustomPolicy`.
6. Add description: `Allows read-only access to the meghanadh-2410080031 S3 bucket.`
7. Review and click **Create policy**.

## Step 4: Create an IAM User

1. Navigate to **IAM → Users**.
2. Click **Create user**.
3. Enter username: `S3ReadOnlyUser`.
4. Continue without attaching additional permissions.
5. Complete user creation.

## Step 5: Attach the Customer-Managed Policy

1. Open **IAM → Users → S3ReadOnlyUser**.
2. Go to the **Permissions** tab and click **Add permissions**.
3. Choose **Attach policies directly**.
4. Search for `S3ReadOnlyCustomPolicy`.
5. Select the policy and click **Add permissions**.

## Step 6: Enable IAM User Console Access

1. Open **IAM → Users → S3ReadOnlyUser**.
2. Go to **Security credentials**.
3. In **AWS Management Console access**, enable/create console access.
4. Set a console password and save.

## Step 7: Sign In as IAM User

1. Open a new incognito/private browser window.
2. Go to the IAM user sign-in URL for your AWS account.
3. Sign in with:
   - Username: `S3ReadOnlyUser`
   - Configured password

## Step 8: Verify S3 Bucket Access

1. Open **Amazon S3**.
2. Navigate to **Buckets** and open `meghanadh-2410080031`.
3. Verify the user can list bucket contents.
4. If objects exist, verify the user can download/read objects.

## Step 9: Verify Permissions with IAM Policy Simulator

1. Open https://policysim.aws.amazon.com/
2. Select IAM user: `S3ReadOnlyUser`.
3. Select service: **Amazon S3**.
4. Select the target S3 bucket resource.
5. Run simulation for:

### Allowed

| Action | Expected |
|---|---|
| `s3:ListBucket` | Allowed |
| `s3:GetObject` | Allowed |

### Denied

| Action | Expected |
|---|---|
| `s3:PutObject` | Denied |
| `s3:DeleteObject` | Denied |

> Note: Effective access may also be impacted by bucket policies, permissions boundaries, service control policies, or explicit denies.

## Step 10: Final Verification Checklist

- [ ] Bucket `meghanadh-2410080031` exists
- [ ] Policy `S3ReadOnlyCustomPolicy` exists
- [ ] User `S3ReadOnlyUser` exists
- [ ] Policy is attached to the user
- [ ] User can list/read from the bucket
- [ ] User cannot upload/delete objects
- [ ] Policy Simulator results match expected allow/deny outcomes

## IAM Managed Policies vs Customer-Managed Policies

| AWS-Managed Policy | Purpose |
|---|---|
| `AmazonS3ReadOnlyAccess` | Broad read-only access to S3 |
| `AmazonS3FullAccess` | Broad full access to S3 |
| `ReadOnlyAccess` | Read-only access across AWS services |
| `AdministratorAccess` | Broad administrative access |

`S3ReadOnlyCustomPolicy` is preferred for this practical because it enforces least privilege by restricting access to one named bucket.

## Final Result

Created S3 bucket `meghanadh-2410080031`, created customer-managed policy `S3ReadOnlyCustomPolicy`, and attached it to IAM user `S3ReadOnlyUser`.

The user can list the bucket and read objects (`s3:ListBucket`, `s3:GetObject`) but does not receive upload or delete permissions.

## Conclusion

Customer-managed IAM policies provide fine-grained, least-privilege access control by scoping permissions to exact resources and actions.
