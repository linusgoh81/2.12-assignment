# 2.12-assignment

Given a Lambda function that is triggered upon the creation of files in an S3 bucket, answer the following:
1. What is the purpose of the execution role on the Lambda function?

The Lambda function's execution role is an IAM role that grants the Lambda function permissions to access other AWS services.  Think of it as the identity the Lambda function assumes when it runs.  Without an execution role (or with an insufficiently configured one), the Lambda function won't be able to interact with any other AWS resources, including S3 (for uploading, in your example), DynamoDB, databases, or anything else.

2. What is the purpose of the resource-based policy on the Lambda function?

A resource-based policy is a policy attached directly to the resource itself (in this case, the Lambda function). It controls who (which AWS principals) can invoke the resource (the Lambda function).  It's an authorization mechanism.  For a Lambda function triggered by S3, the resource-based policy allows S3 to invoke the Lambda function when a file is uploaded.  Without it, S3 wouldn't be permitted to trigger the function.

3. If the function is needed to upload a file into an S3 bucket, describe (i.e no need for the actual policies)
   - What is the needed update on the execution role? You need to add permissions to the Lambda function's execution role that allow it to write objects to the destination S3 bucket. 
   - What is the new resource-based policy that needs to be added (if any)? You likely won't need to update the resource-based policy on the Lambda function itself for this specific scenario (uploading to a different bucket). The existing resource-based policy that allows S3 to invoke the Lambda function (when a file is uploaded to the triggering bucket) remains in place.  The Lambda function's execution role handles the permissions for the upload to the destination bucket.

If, however, you wanted to allow other AWS principals (e.g., other AWS accounts or specific IAM users) to directly invoke the Lambda function to perform the upload, then you would need to add statements to the resource-based policy to grant those principals lambda:InvokeFunction permission.  But for the core requirement of the function being triggered by one S3 bucket and uploading to another, the execution role update is sufficient.
  
