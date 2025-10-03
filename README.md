<<<<<<< HEAD
# AWS Lambda CI/CD Pipeline

This project demonstrates a complete CI/CD pipeline for AWS Lambda functions using AWS CodeBuild and CodeDeploy. The pipeline automatically builds, packages, and deploys Lambda functions with version management and alias updates.

## 🚀 Overview

This CI/CD solution provides:
- **Automated Lambda deployment** using AWS CodeBuild
- **Version management** with automatic version publishing
- **Alias updates** for blue/green deployments
- **Zero-downtime deployments** using AWS CodeDeploy
- **Build artifact management** with S3 storage

## 📁 Project Structure

```
lambda-cicd-main/
├── index.js                 # Lambda function code
├── buildspec.yml           # CodeBuild configuration
├── appspec.yml             # CodeDeploy configuration
├── scripts/
│   └── await-update-code.sh # Script to wait for Lambda updates
└── README.md               # This file
```

## 🛠️ Components

### Lambda Function (`index.js`)
Simple Node.js Lambda function that returns a success message:
```javascript
exports.handler = async (event) => {
  return {
    statusCode: 200,
    body: JSON.stringify('FUNCIONOU !!'),
  };
};
```

### CodeBuild Configuration (`buildspec.yml`)
- Packages the Lambda function code
- Updates the Lambda function code
- Publishes a new version
- Prepares CodeDeploy artifacts

### CodeDeploy Configuration (`appspec.yml`)
- Defines deployment target (Lambda function)
- Manages alias updates for blue/green deployments
- Uses placeholders that are replaced during build

## 🚀 Step-by-Step Setup

### Prerequisites

1. **AWS Account** with appropriate permissions
2. **AWS CLI** configured with credentials
3. **Lambda function** already created in AWS
4. **S3 bucket** for storing build artifacts
5. **IAM roles** for CodeBuild and CodeDeploy

### Step 1: Create Lambda Function

1. Go to AWS Lambda Console
2. Click "Create function"
3. Choose "Author from scratch"
4. Set function name (e.g., `my-lambda-function`)
5. Choose Node.js runtime
6. Create function

### Step 2: Create S3 Bucket

```bash
aws s3 mb s3://your-lambda-deployments-bucket
```

### Step 3: Create IAM Roles

#### CodeBuild Service Role
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "codebuild.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Attach policies:
- `AWSCodeBuildDeveloperAccess`
- `AWSLambdaFullAccess`
- `AmazonS3FullAccess`

#### CodeDeploy Service Role
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "codedeploy.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Attach policies:
- `AWSCodeDeployRoleForLambda`
- `AWSLambdaFullAccess`

### Step 4: Create CodeBuild Project

1. Go to AWS CodeBuild Console
2. Click "Create build project"
3. Configure:
   - **Project name**: `lambda-cicd-build`
   - **Source**: GitHub/CodeCommit (your repository)
   - **Environment**: 
     - OS: Ubuntu
     - Runtime: Standard
     - Image: aws/codebuild/standard:5.0
   - **Service role**: Use the CodeBuild role created above
   - **Buildspec**: Use buildspec.yml in source code root

### Step 5: Create CodeDeploy Application

1. Go to AWS CodeDeploy Console
2. Click "Create application"
3. Configure:
   - **Application name**: `lambda-cicd-app`
   - **Compute platform**: AWS Lambda

### Step 6: Create Deployment Group

1. In your CodeDeploy application, click "Create deployment group"
2. Configure:
   - **Deployment group name**: `lambda-cicd-group`
   - **Service role**: Use the CodeDeploy role created above
   - **Deployment type**: Blue/Green
   - **Environment**: Lambda function
   - **Function name**: Your Lambda function name
   - **Alias**: `PROD` (or your preferred alias)

### Step 7: Configure Environment Variables

In your CodeBuild project, add these environment variables:
- `LAMBDA_NAME`: Your Lambda function name
- `LAMBDA_ALIAS`: Your Lambda alias (e.g., `PROD`)
- `S3_BUCKET`: Your S3 bucket name

### Step 8: Create Lambda Alias

```bash
aws lambda create-alias \
  --function-name your-lambda-function \
  --name PROD \
  --function-version 1
```

## 🔄 How It Works

### Build Process (CodeBuild)

1. **Pre-build Phase**:
   - Creates build directory
   - Copies Lambda function code
   - Creates deployment package (ZIP file)

2. **Build Phase**:
   - Updates Lambda function code
   - Waits for update completion
   - Publishes new version
   - Updates appspec.yml with current and target versions

### Deployment Process (CodeDeploy)

1. **Blue/Green Deployment**:
   - Creates new Lambda version
   - Updates alias to point to new version
   - Maintains zero-downtime deployment

## 🚀 Running the Pipeline

### Manual Trigger
1. Go to CodeBuild Console
2. Select your project
3. Click "Start build"
4. Monitor the build logs

### Automatic Trigger
Set up webhook or CloudWatch Events to trigger builds on code changes.

## 📊 Monitoring

### CodeBuild Logs
- View build logs in CodeBuild Console
- Check for errors and warnings

### Lambda Function
- Monitor function metrics in CloudWatch
- Check function logs for execution details

### CodeDeploy
- View deployment status in CodeDeploy Console
- Monitor deployment history

## 🔧 Customization

### Modify Lambda Function
Edit `index.js` to change the function behavior:
```javascript
exports.handler = async (event) => {
  // Your custom logic here
  return {
    statusCode: 200,
    body: JSON.stringify({
      message: 'Hello from Lambda!',
      timestamp: new Date().toISOString()
    }),
  };
};
```

### Update Build Process
Modify `buildspec.yml` to:
- Add testing steps
- Include additional dependencies
- Change packaging process

### Environment-Specific Deployments
Create multiple deployment groups for different environments (dev, staging, prod).

## 🐛 Troubleshooting

### Common Issues

1. **Permission Denied**:
   - Check IAM roles and policies
   - Ensure CodeBuild has Lambda update permissions

2. **Lambda Update Fails**:
   - Verify function name and alias exist
   - Check Lambda function configuration

3. **CodeDeploy Fails**:
   - Ensure alias exists
   - Check CodeDeploy service role permissions

### Debug Steps

1. Check CodeBuild logs for detailed error messages
2. Verify environment variables are set correctly
3. Test Lambda function manually
4. Check CloudWatch logs for Lambda execution errors

## 📚 Additional Resources

- [AWS CodeBuild Documentation](https://docs.aws.amazon.com/codebuild/)
- [AWS CodeDeploy Documentation](https://docs.aws.amazon.com/codedeploy/)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [YouTube Tutorial](https://youtu.be/VeJt62_2azY)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the pipeline
5. Submit a pull request
=======
# lambda-cicd-aws
>>>>>>> eb2c4b9644827c1b8cbe7f212ce83d42c037e066
