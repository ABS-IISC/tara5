# Complete AWS Deployment Guide for Non-Technical Users
## AI Document Analysis Tool - Step-by-Step Deployment

**Last Updated**: November 23, 2025
**Repository**: https://github.com/ABS-IISC/tara5
**Difficulty**: Beginner-Friendly (No coding knowledge required!)

---

## Table of Contents
1. [What is This Tool?](#what-is-this-tool)
2. [Why Deploy to AWS?](#why-deploy-to-aws)
3. [AWS Deployment Options Comparison](#deployment-options-comparison)
4. [Prerequisites (What You Need)](#prerequisites)
5. [Option 1: AWS App Runner (EASIEST - RECOMMENDED)](#option-1-aws-app-runner-easiest)
6. [Option 2: AWS Elastic Beanstalk](#option-2-aws-elastic-beanstalk)
7. [Option 3: AWS EC2 (Traditional Server)](#option-3-aws-ec2-traditional-server)
8. [Option 4: AWS Lambda + API Gateway](#option-4-aws-lambda-api-gateway)
9. [Cost Comparison](#cost-comparison)
10. [Troubleshooting](#troubleshooting)

---

## What is This Tool?

This is an **AI-powered document analysis tool** that:
- Reads Word documents (.docx files)
- Analyzes them using advanced Claude AI models from Anthropic
- Provides intelligent feedback and suggestions
- Generates reviewed documents with comments
- Stores everything securely in AWS cloud storage

Think of it like having an AI assistant that reviews your documents automatically!

---

## Why Deploy to AWS?

AWS (Amazon Web Services) is like renting a super-powerful computer in the cloud. Benefits:

✅ **Always Available**: Your tool runs 24/7, accessible from anywhere
✅ **Secure**: Enterprise-grade security and backups
✅ **Scalable**: Handles 1 user or 1000 users automatically
✅ **No Maintenance**: AWS manages servers, updates, and infrastructure
✅ **Professional**: Get a real web URL like `https://your-app.awsapprunner.com`

---

## Deployment Options Comparison

Let me explain each option in simple terms:

| Option | Difficulty | Best For | Monthly Cost | Setup Time |
|--------|-----------|----------|--------------|------------|
| **AWS App Runner** | ⭐ Easiest | Beginners, Quick Start | $50-100 | 15 minutes |
| **Elastic Beanstalk** | ⭐⭐ Medium | Growing Applications | $30-80 | 30 minutes |
| **EC2 Server** | ⭐⭐⭐ Hard | Full Control Needed | $20-50 | 1-2 hours |
| **Lambda (Serverless)** | ⭐⭐⭐⭐ Very Hard | Cost Optimization | $10-30 | 2-3 hours |

**🎯 RECOMMENDATION**: Start with **AWS App Runner** - it's the easiest and perfect for beginners!

---

## Prerequisites

Before you start, you need:

### 1. AWS Account (Free to Create)
- Go to: https://aws.amazon.com/
- Click "Create an AWS Account"
- You'll need:
  - Email address
  - Credit card (for verification only - we'll use free tier when possible)
  - Phone number

### 2. GitHub Account (Free)
- Go to: https://github.com/
- Sign up if you don't have an account
- Your code is already at: https://github.com/ABS-IISC/tara5

### 3. Enable Claude AI Access in AWS
This is CRITICAL - without this, the tool won't work!

**Step-by-Step to Enable Bedrock:**

1. **Sign in to AWS Console**
   - Go to: https://console.aws.amazon.com/
   - Enter your email and password

2. **Go to AWS Bedrock Service**
   - In the search bar at top, type "Bedrock"
   - Click "Amazon Bedrock" from results
   - **IMPORTANT**: Change region to "US East (N. Virginia)" using dropdown at top-right

3. **Request Model Access**
   - In left sidebar, click "Model access"
   - Click orange "Manage model access" or "Modify model access" button
   - Find **Anthropic** section
   - Check the box next to:
     - ✅ **Claude 3.5 Sonnet** (Primary model - MUST HAVE)
     - ✅ **Claude 3 Opus** (Optional - premium)
     - ✅ **Claude 3 Haiku** (Optional - fast)
     - ✅ **Claude Sonnet 4.5** (Optional - newest)
   - Click "Request model access" at bottom
   - Wait 2-5 minutes for "Access granted" status (usually instant)

4. **Verify Access Granted**
   - Refresh the page
   - Status should show "Access granted" in green
   - If denied, click "Request again" and wait

**💡 TIP**: Model access is FREE to request. You only pay when you USE the models (pay-per-use pricing).

---

## Option 1: AWS App Runner (EASIEST - RECOMMENDED)

### What is App Runner?
App Runner is like a "magic deploy button" - you point it to your GitHub code, and AWS automatically:
- Builds your application
- Runs it on servers
- Gives you a URL
- Handles scaling and updates

### Step-by-Step Deployment

#### STEP 1: Prepare Your GitHub Repository

1. **Verify Repository Access**
   - Go to: https://github.com/ABS-IISC/tara5
   - Make sure you can see the code
   - Note: The repository is already set up with everything you need!

#### STEP 2: Create AWS App Runner Service

1. **Open AWS App Runner Console**
   - Go to AWS Console: https://console.aws.amazon.com/
   - Search for "App Runner" in the top search bar
   - Click "AWS App Runner"
   - Click orange "Create service" button

2. **Configure Source Code**
   - **Repository type**: Select "Source code repository"
   - **Connect to GitHub**:
     - Click "Add new" next to GitHub connection
     - Name it: `github-connection`
     - Click "Install another"
     - Authorize AWS in GitHub popup window
     - Select your GitHub username `ABS-IISC`
     - Click "Connect"
   - **Repository**: Select `ABS-IISC/tara5`
   - **Branch**: Select `main`
   - **Deployment trigger**: Choose "Automatic" (deploys on every code push)
   - Click "Next"

3. **Configure Build**
   - **Configuration source**: Select "Use a configuration file"
   - (This uses the `apprunner.yaml` file already in your repository)
   - Click "Next"

4. **Configure Service**
   - **Service name**: Enter `ai-document-analyzer` (or your preferred name)
   - **Virtual CPU**: Select `1 vCPU`
   - **Memory**: Select `2 GB` (minimum recommended)
   - **Environment variables**:
     - The `apprunner.yaml` file already has most variables
     - You may need to add:
       - `SECRET_KEY`: Generate at https://randomkeygen.com/ (use "CodeIgniter Encryption Keys")
       - Paste: `3afca7db04822768a2a2eae4744f8521ab49605123deae3ae4a30f9dab5d503e`
   - Click "Next"

5. **Configure Security**
   - **Instance role**:
     - If you have one, select it
     - If not, click "Create new service role"
     - Name it: `AppRunnerAIPrismRole`
   - Click "Next"

6. **Review and Create**
   - Review all settings
   - **Estimated cost**: ~$0.064/hour = ~$45-50/month
   - Click "Create & deploy"

#### STEP 3: Create IAM Role with Permissions

While App Runner is deploying (takes 5-10 minutes), let's set up permissions:

1. **Go to IAM Console**
   - Search "IAM" in AWS Console
   - Click "Roles" in left sidebar
   - Find your role: `AppRunnerAIPrismRole`
   - Click on it

2. **Add Bedrock Permissions**
   - Click "Add permissions" → "Create inline policy"
   - Click "JSON" tab
   - Paste this code:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "BedrockAccess",
         "Effect": "Allow",
         "Action": [
           "bedrock:InvokeModel",
           "bedrock:InvokeModelWithResponseStream",
           "bedrock:ListFoundationModels"
         ],
         "Resource": [
           "arn:aws:bedrock:us-east-1::foundation-model/anthropic.claude-*",
           "arn:aws:bedrock:us-east-1::foundation-model/us.anthropic.claude-*"
         ]
       }
     ]
   }
   ```
   - Click "Review policy"
   - Name: `BedrockAccessPolicy`
   - Click "Create policy"

3. **Add S3 Permissions**
   - Click "Add permissions" → "Create inline policy" again
   - Click "JSON" tab
   - Paste this:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "S3Access",
         "Effect": "Allow",
         "Action": [
           "s3:PutObject",
           "s3:GetObject",
           "s3:DeleteObject",
           "s3:ListBucket"
         ],
         "Resource": [
           "arn:aws:s3:::felix-s3-bucket",
           "arn:aws:s3:::felix-s3-bucket/*"
         ]
       }
     ]
   }
   ```
   - Name: `S3AccessPolicy`
   - Click "Create policy"

#### STEP 4: Create S3 Bucket for Storage

1. **Go to S3 Console**
   - Search "S3" in AWS Console
   - Click "Create bucket"

2. **Configure Bucket**
   - **Bucket name**: `felix-s3-bucket` (must be globally unique)
   - **Region**: `US East (N. Virginia) us-east-1`
   - **Block Public Access**: Keep ALL boxes CHECKED (secure)
   - **Bucket Versioning**: Enable (recommended for backup)
   - Click "Create bucket"

#### STEP 5: Wait for Deployment & Test

1. **Monitor Deployment**
   - Go back to App Runner console
   - Click on your service: `ai-document-analyzer`
   - Wait for status to show "Running" (green)
   - This takes 5-10 minutes

2. **Get Your Application URL**
   - Once running, you'll see a URL like:
   - `https://abc123xyz.us-east-1.awsapprunner.com`
   - **THIS IS YOUR APP'S PUBLIC URL!**

3. **Test the Application**
   - Click the URL to open in browser
   - You should see the AI Document Analyzer homepage
   - Try uploading a test document
   - If it works: **🎉 CONGRATULATIONS! You're deployed!**

#### STEP 6: Troubleshooting Common Issues

**Issue: "Model access denied"**
- Go back to Bedrock console
- Verify "Access granted" status for Claude models
- Wait a few more minutes and try again

**Issue: "S3 bucket not found"**
- Verify bucket name is exactly `felix-s3-bucket`
- Verify bucket is in `us-east-1` region
- Check IAM role has S3 permissions

**Issue: App won't start**
- Go to App Runner logs (Logs tab in service)
- Look for error messages
- Common fix: Update environment variables

---

## Option 2: AWS Elastic Beanstalk

### What is Elastic Beanstalk?
Elastic Beanstalk is like App Runner but with more control. It's good if you:
- Want more customization
- Need to manage multiple environments (dev, staging, prod)
- Have some technical knowledge

### Quick Setup Steps

1. **Install EB CLI (Command Line Tool)**
   ```bash
   pip install awsebcli --upgrade
   ```

2. **Initialize Elastic Beanstalk**
   ```bash
   cd /path/to/tara5
   eb init
   ```
   - Select region: `us-east-1`
   - Application name: `ai-document-analyzer`
   - Platform: `Python 3.11`
   - Set up SSH: `Yes` (optional)

3. **Create Environment**
   ```bash
   eb create ai-doc-analyzer-prod
   ```
   - Wait 5-10 minutes for environment creation

4. **Set Environment Variables**
   ```bash
   eb setenv AWS_REGION=us-east-1 \
     S3_BUCKET_NAME=felix-s3-bucket \
     BEDROCK_MODEL_ID=anthropic.claude-3-5-sonnet-20240620-v1:0 \
     SECRET_KEY=your-generated-key
   ```

5. **Deploy**
   ```bash
   eb deploy
   ```

**Cost**: ~$30-80/month depending on instance size

**More Details**: Full Elastic Beanstalk guide available in documentation folder.

---

## Option 3: AWS EC2 (Traditional Server)

### What is EC2?
EC2 is like renting a virtual computer in the cloud. You have full control but need to:
- Install and configure everything manually
- Handle security updates
- Manage the operating system

### Quick Setup Steps

1. **Launch EC2 Instance**
   - Go to EC2 Console
   - Click "Launch Instance"
   - Choose Amazon Linux 2023
   - Instance type: `t3.small` (2 vCPU, 2 GB RAM)
   - Key pair: Create new or use existing
   - Security group: Allow HTTP (80), HTTPS (443), SSH (22)
   - Click "Launch"

2. **Connect to Server**
   ```bash
   ssh -i your-key.pem ec2-user@your-ec2-public-ip
   ```

3. **Install Dependencies**
   ```bash
   sudo yum update -y
   sudo yum install python3 pip git -y
   git clone https://github.com/ABS-IISC/tara5.git
   cd tara5
   pip3 install -r requirements.txt
   ```

4. **Configure and Run**
   ```bash
   export AWS_REGION=us-east-1
   export S3_BUCKET_NAME=felix-s3-bucket
   python3 app.py
   ```

**Cost**: ~$20-50/month for t3.small instance

**⚠️ Warning**: Requires more technical knowledge. Not recommended for beginners.

---

## Option 4: AWS Lambda + API Gateway

### What is Lambda?
Lambda is "serverless" - you only pay when someone uses your app. Good for:
- Low traffic applications
- Cost optimization
- Apps that run occasionally

### Why It's Complex
Lambda requires:
- Breaking app into small functions
- Setting up API Gateway
- Configuring triggers
- Managing layers and dependencies

**Cost**: ~$10-30/month (cheapest option)

**⚠️ Warning**: Very technical. Requires significant code changes. Only for experienced developers.

---

## Cost Comparison

Here's what you'll actually pay per month:

### AWS App Runner (Recommended)
- **Base cost**: $0.064/hour × 730 hours = ~$47/month
- **AI Model usage**: ~$3-10/month (depends on document count)
- **S3 storage**: $0.023/GB = ~$1-2/month (for 50-100 GB)
- **Data transfer**: First 1 GB free, then $0.09/GB = ~$1-5/month
- **TOTAL**: **$50-65/month**

### Elastic Beanstalk
- **EC2 instance**: $20-40/month
- **Load balancer**: $16/month (optional)
- **AI + S3**: Same as above (~$4-12/month)
- **TOTAL**: **$40-70/month**

### EC2 (t3.small)
- **Instance**: $0.021/hour × 730 = $15/month
- **Storage**: $0.10/GB × 30 GB = $3/month
- **AI + S3**: ~$4-12/month
- **TOTAL**: **$22-30/month** (cheapest, but requires maintenance)

### AWS Free Tier (First Year)
New AWS accounts get 12 months of free tier:
- EC2: 750 hours/month of t2.micro or t3.micro (enough for 1 small instance)
- S3: 5 GB storage, 20,000 GET requests
- Lambda: 1 million requests/month
- **BUT**: Bedrock AI models are NOT free - you pay per use

**💡 Money-Saving Tip**: Start with App Runner for ease, monitor costs in AWS Cost Explorer, optimize later if needed.

---

## Monitoring Your Deployed Application

### 1. Check Application Health

**AWS App Runner**:
- Go to App Runner console
- Click your service
- Check "Health" tab
- Green = Good, Red = Problem

### 2. View Logs

**To see what's happening**:
- In App Runner service page
- Click "Logs" tab
- Click "View in CloudWatch"
- See real-time application logs

**What to look for**:
- ✅ `Server started on port 8080`
- ✅ `7 Claude models loaded`
- ✅ `AWS credentials loaded`
- ❌ `Model access denied` = Check Bedrock permissions
- ❌ `S3 bucket not found` = Check bucket name

### 3. Monitor Costs

**Set up billing alerts**:
1. Go to Billing Dashboard
2. Click "Budgets"
3. Click "Create budget"
4. Set monthly budget: $100
5. Set alert at 80% ($80)
6. Enter your email
7. Click "Create"

You'll get email when costs reach $80!

---

## Troubleshooting

### Problem: "Application keeps crashing"

**Solution**:
1. Check logs in CloudWatch
2. Look for error messages
3. Common issues:
   - Missing environment variables
   - Wrong Bedrock model ID
   - Insufficient memory (increase to 2 GB)

### Problem: "Model access denied"

**Solution**:
1. Go to AWS Bedrock console
2. Check "Model access" in left sidebar
3. Verify "Access granted" for Claude 3.5 Sonnet
4. If not granted, request access again
5. Wait 5 minutes and restart app

### Problem: "S3 upload failed"

**Solution**:
1. Verify S3 bucket exists: `felix-s3-bucket`
2. Check IAM role has S3 permissions
3. Verify bucket region is `us-east-1`
4. Check bucket permissions (should be private, but IAM role should have access)

### Problem: "High costs"

**Solutions**:
1. Enable auto-scaling limits in App Runner
2. Set max instances to 2-3
3. Use Claude 3 Haiku instead of Sonnet (10x cheaper, but less capable)
4. Enable request caching
5. Delete old files from S3 bucket

### Problem: "Slow response times"

**Solutions**:
1. Increase instance memory to 3-4 GB
2. Enable caching in application
3. Use faster Claude models (Haiku)
4. Check network latency (change region if needed)

---

## Security Best Practices

### 1. Never Commit Secrets to GitHub
- Don't put passwords, API keys, or `SECRET_KEY` in code
- Use AWS Secrets Manager or environment variables
- Check `.gitignore` file includes `.env`

### 2. Use IAM Roles (Not Access Keys)
- App Runner/EC2: Use IAM roles (automatic, secure)
- Never create long-term access keys for production
- Rotate keys every 90 days if you must use them

### 3. Enable Encryption
- S3: Enable bucket encryption (default in AWS)
- App Runner: HTTPS only (automatic)
- Bedrock: Encrypted by default

### 4. Monitor Access
- Enable CloudTrail (logs all AWS actions)
- Set up billing alerts
- Review IAM policies monthly

### 5. Keep Software Updated
- App Runner updates automatically
- EC2: You must update manually (`sudo yum update`)

---

## Next Steps After Deployment

### 1. Test Thoroughly
- Upload sample documents
- Verify AI analysis works
- Check document download
- Test S3 export functionality

### 2. Set Up Monitoring
- Enable CloudWatch alarms
- Set up billing alerts
- Monitor application logs

### 3. Create Backups
- Enable S3 bucket versioning
- Export important data regularly
- Test restore process

### 4. Optimize Costs
- Review AWS Cost Explorer monthly
- Delete unused resources
- Use cheaper models for testing

### 5. Plan for Growth
- Monitor user count
- Increase instances if needed
- Consider caching for better performance

---

## Getting Help

### AWS Support
- Free tier: Community forums only
- Developer support: $29/month (email response in 24 hours)
- Business support: $100/month (phone support, 1-hour response)

### Community Resources
- AWS Forums: https://forums.aws.amazon.com/
- Stack Overflow: Tag questions with `aws-app-runner`, `aws-bedrock`
- AWS Documentation: https://docs.aws.amazon.com/

### Application Issues
- GitHub Issues: https://github.com/ABS-IISC/tara5/issues
- Check logs in CloudWatch
- Review deployment guide in repository

---

## Summary - Quick Decision Guide

**Choose AWS App Runner if**:
- ✅ You want the easiest deployment
- ✅ You're okay with ~$50-65/month cost
- ✅ You want automatic updates and scaling
- ✅ You don't want to manage servers
- **BEST FOR**: Beginners, quick deployment, production use

**Choose Elastic Beanstalk if**:
- ✅ You want more control than App Runner
- ✅ You need multiple environments (dev/staging/prod)
- ✅ You're comfortable with command line
- **BEST FOR**: Growing applications, team projects

**Choose EC2 if**:
- ✅ You want lowest cost (~$22-30/month)
- ✅ You have technical knowledge
- ✅ You need full control over everything
- ✅ You can handle manual updates
- **BEST FOR**: Cost-sensitive projects, experienced users

**Choose Lambda if**:
- ✅ You have advanced AWS knowledge
- ✅ You need ultra-low cost for low traffic
- ✅ You can refactor the application code
- **BEST FOR**: Experts only, optimization projects

---

## Conclusion

🎉 **Congratulations!** You now have a complete guide to deploying your AI document analysis tool to AWS!

**My Recommendation for Beginners**:
1. Start with **AWS App Runner** (15 minutes to deploy)
2. Get it working and test thoroughly
3. Monitor costs for first month
4. Optimize later if needed

**Remember**:
- Start small, scale up as needed
- Monitor costs weekly
- Enable billing alerts immediately
- Keep backups of important data
- Ask for help in AWS forums when stuck

**Your application URL will look like**:
`https://your-app-name.us-east-1.awsapprunner.com`

Share this URL with users, and they can access your AI document analyzer from anywhere in the world!

---

**Questions?** Review the troubleshooting section or check AWS documentation at: https://docs.aws.amazon.com/apprunner/

**Good luck with your deployment!** 🚀

---

*Last Updated: November 23, 2025*
*Repository: https://github.com/ABS-IISC/tara5*
*Contact: Check GitHub repository for support*
