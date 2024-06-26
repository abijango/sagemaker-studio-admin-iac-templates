To make sure you do not incur additional charges when you no longer need the to experiment with the jumpstart model, follow these steps to remove the resources you created when following the blog post [Using Generative AI foundation models in VPC mode with no internet connectivity using SageMaker JumpStart]().


### 1. Make note of the Amazon Resource Name (ARN) for the SageMaker Studio domain
You will need the full ARN of the SageMaker Studio domain in step 3.
- Go to the CloudFormation console, select the second stack you created (named StudioAndUserInternetOnly if you used the suggested name in the deploy CFN script).
- Go to the Output tab, copy the value of the output named `StudioDomainArn` and keep it somewhere safe.

NOTE: You can run the following make command to perform steps 2-4:
```bash
cleanup-cfn FOLDER=studio-classic-vpc-only STUDIO_ARN=<StudioDomainArn>
```

### 2. Delete the SageMaker Studio and associated resources
- Go to the CloudFormation console.
- Locate the second stack you created (named SageMaker-Studio-VPC-No-Internet if you used the suggested name in the blog) and delete the stack.
- When the stack is deleted, move to the next step.

### 3. Remove the mount points (ENIs) for the EFS volume, ENI for the inference endpoint, and security groups created by SageMaker Studio
In this step, you will delete the resources that were automatically created when you created the SageMaker Studio domain, including the EFS mount points and security groups.

Make sure the dependencies are installed.

`python3 -m pip install --user -r requirements.txt`

Run `cleanup-efs.py`, passing in the the ARN of the SageMaker Studio domain as an argument.

`python3 cleanup.py --sagemaker-studio-domain [ARN for the SageMaker Studio domain from step 1]`

To prevent accidental data loss, the script does not delete the volume . We ask you to delete the EFS volume yourself once you have made sure you do not need any data or code you might have created in SageMaker Studio. The script writes the File System ID for the EFS volume to the output. Make a note of the file system ID as you will need it in step 6.

### 4. Delete the VPC and other networking resources
You can now delete the VPC and its associated resources such as VPC endpoints and subnets. In the CloudFormation console, locate the first stack you created (named No-Internet if you used the suggested name) and delete it. If the stack fails to delete after a while, make sure you have completed step 4.

### 5. Delete the EFS volume
Once you have made sure you do not need the data in the EFS volume, go to EFS console, locate the EFS volume using the file system ID you noted earlier, and delete it.
