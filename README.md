# aws-soa
# AWS IAM – Create and Attach a Customer-Managed IAM Policy

## Objective

To create a customer-managed IAM policy that provides read-only access to a specific Amazon S3 bucket, attach the policy to an IAM user, and verify allowed and denied permissions using the AWS IAM Policy Simulator.

## AWS Resources

| Parameter | Value |
|---|---|
| AWS Service | IAM and Amazon S3 |
| S3 Bucket Name | `meghanadh-2410080031` |
| IAM Policy Name | `S3ReadOnlyCustomPolicy` |
| IAM Username | `S3ReadOnlyUser` |
| Policy Type | Customer-Managed |
| Access Type | Read-Only |

---

## Step 1: Open AWS IAM

1. Log in to the AWS Management Console.
2. Search for IAM in the AWS search bar.
3. Open Identity and Access Management (IAM).
4. Select Policies from the left navigation panel.

## Step 2: Create an S3 Bucket

1. Search for Amazon S3 in the AWS Console.
2. Open the S3 service.
3. Click Create bucket.
4. Enter the bucket name:

   `meghanadh-2410080031`

5. Select the required AWS region.
6. Keep Block all public access enabled.
7. Keep the default configuration.
8. Click Create bucket.

The S3 bucket will be used as the resource for the customer-managed IAM policy.

## Step 3: Create a Customer-Managed IAM Policy

1. Navigate to IAM → Policies.
2. Click Create policy.
3. Select the JSON editor.
4. Remove the existing JSON.
5. Paste the following policy.

### IAM Policy JSON

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

### Explanation of the Policy

- **Version:** Specifies the IAM policy language version.
- **Effect:** Allow specifies that the listed actions are permitted.
- **s3:ListBucket:** Allows the user to view the contents of the specified bucket.
- **s3:GetObject:** Allows the user to download and read objects from the bucket.
- **Resource:** Restricts the permissions to the specified S3 bucket and its objects.

6. Click Next.
7. Enter the policy name:

   `S3ReadOnlyCustomPolicy`

8. Enter the description:

   `Allows read-only access to the meghanadh-2410080031 S3 bucket.`

9. Review the policy permissions.
10. Click Create policy.

The customer-managed IAM policy is now created.

## Step 4: Create an IAM User

1. Navigate to IAM → Users.
2. Click Create user.
3. Enter the username:

   `S3ReadOnlyUser`

4. Click Next.
5. Create the user without attaching additional permissions.
6. Complete the user creation process.

This IAM user will be used to test the customer-managed policy.

## Step 5: Attach the Customer-Managed Policy

1. Navigate to IAM → Users.
2. Select `S3ReadOnlyUser`.
3. Open the Permissions tab.
4. Click Add permissions.
5. Select Attach policies directly.
6. Search for:

   `S3ReadOnlyCustomPolicy`

7. Select the checkbox beside the policy.
8. Click Add permissions.

The customer-managed policy is now attached to the IAM user.

## Step 6: Enable IAM User Console Access

1. Open IAM → Users → S3ReadOnlyUser.
2. Navigate to the Security credentials tab.
3. Locate the AWS Management Console access section.
4. Enable or create console access using the available account workflow.
5. Set a console password.
6. Save the configuration.

The IAM user can now sign in to the AWS Management Console.

## Step 7: Log In as the IAM User

1. Open a new incognito or private browser window.
2. Navigate to the AWS IAM user sign-in page.
3. Enter the AWS Account ID.
4. Enter the username:

   `S3ReadOnlyUser`

5. Enter the configured password.
6. Sign in to the AWS Management Console.

The user will now access AWS with the permissions granted by the attached customer-managed policy.

## Step 8: Verify S3 Bucket Access

1. Search for Amazon S3.
2. Open the S3 service.
3. Navigate to Buckets.
4. Locate:

   `meghanadh-2410080031`

5. Open the bucket.
6. Verify that the user can view the bucket contents.
7. If objects are available, try downloading an object.

The user should be able to list the bucket and read its objects.

## Step 9: Verify Permissions Using IAM Policy Simulator

1. Open the IAM Policy Simulator:

   https://policysim.aws.amazon.com/

2. Select the IAM user:

   `S3ReadOnlyUser`

3. Select Amazon S3 as the service.
4. Select the appropriate S3 bucket resource.
5. Test the following actions.

### Allowed Permissions

| Action | Description | Expected Result |
|---|---|---|
| `s3:ListBucket` | View bucket contents | Allowed |
| `s3:GetObject` | Download or read objects | Allowed |

### Denied Permissions

| Action | Description | Expected Result |
|---|---|---|
| `s3:PutObject` | Upload objects | Denied |
| `s3:DeleteObject` | Delete objects | Denied |

6. Select the required actions.
7. Click Run Simulation.
8. Verify the results.

**Note:** The IAM Policy Simulator evaluates the selected policy configuration. Other policies, explicit denies, permissions boundaries, service control policies, and bucket policies may affect actual access.

## Step 10: Verify the Final Configuration

Verify the following:

1. The S3 bucket `meghanadh-2410080031` exists.
2. The customer-managed policy `S3ReadOnlyCustomPolicy` is created.
3. The IAM user `S3ReadOnlyUser` is created.
4. The custom policy is attached to the IAM user.
5. The user can list and read objects from the specified bucket.
6. Upload and delete operations are not permitted by the custom policy.
7. IAM Policy Simulator results match the expected permissions.

---

## IAM Managed Policies vs Customer-Managed Policies

AWS provides two common types of managed IAM policies.

| AWS-Managed Policy | Purpose |
|---|---|
| `AmazonS3ReadOnlyAccess` | Provides broad read-only access to S3 resources |
| `AmazonS3FullAccess` | Provides broad full access to S3 |
| `ReadOnlyAccess` | Provides read-only access across supported AWS services |
| `AdministratorAccess` | Provides broad administrative access to AWS |

A customer-managed policy such as `S3ReadOnlyCustomPolicy` allows administrators to define permissions for specific resources.

For this practical, the custom policy is preferred because it restricts access to one named S3 bucket instead of granting broad S3 permissions.

---

## Final Result

Successfully created an Amazon S3 bucket named `meghanadh-2410080031`, created a customer-managed IAM policy named `S3ReadOnlyCustomPolicy`, and attached it to the IAM user `S3ReadOnlyUser`.

The policy grants read-only access through `s3:ListBucket` and `s3:GetObject` permissions while not granting upload or delete permissions.

The configuration was designed to restrict access to the specified S3 bucket, and the allowed and denied actions were verified using the AWS IAM Policy Simulator.

## Conclusion

Customer-managed IAM policies provide fine-grained access control by allowing administrators to define specific permissions for AWS resources. By attaching a restricted policy to an IAM user, access can be controlled according to the user's responsibilities while following the principle of least privilege.

**Practical completed: AWS IAM – Create and Attach a Customer-Managed IAM Policy.**
