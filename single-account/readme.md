# Lambda Runtime Monitor

This project deploys an automated system to monitor and report AWS Lambda functions using deprecated runtimes across your AWS account.

## Architecture
![Lambda Runtime Monitor Architecture](./lambda-runtime-monitor-architecture.mmd)

## Features

- 🔍 Daily automated scanning of Lambda functions
- 📊 Identifies functions using deprecated runtimes
- 🏷️ Includes APPID tag information (optional)
- 📧 Email notifications with detailed reports
- ⏱️ Configurable scheduling
- 📝 Comprehensive logging via CloudWatch

## Prerequisites

- AWS Account with Business or Enterprise Support (for Trusted Advisor API access)
- Verified email address in Amazon SES
- AWS CLI installed and configured
- Required IAM permissions to deploy CloudFormation stacks

## Files Structure

```
.
├── README.md
├── cloudformation/
│   └── lambda-runtime-monitor-ses-notifier.yml
├── diagrams/
│   └── lambda-runtime-monitor-architecture.mmd
└── docs/
    └── deployment-guide.md
```

## Quick Start

1. **Verify SES Email**
```bash
aws ses verify-email-identity --email-address your-sender@example.com
```

2. **Deploy Stack**
```bash
aws cloudformation create-stack \
  --stack-name deprecated-runtime-checker \
  --template-body file://cloudformation/lambda-runtime-monitor-ses-notifier.yml \
  --capabilities CAPABILITY_IAM \
  --parameters \
    ParameterKey=EmailRecipient,ParameterValue=recipient@example.com \
    ParameterKey=EmailSender,ParameterValue=sender@example.com
```

## Configuration Parameters

| Parameter | Description | Default |
|-----------|------------|---------|
| FunctionName | Name of the Lambda function | check-deprecated-lambda-runtimes |
| EmailRecipient | Email address to receive notifications | - |
| EmailSender | Verified SES email address to send from | - |

## Email Report Format

The daily email report includes:
- Function Name
- Runtime Version
- AWS Region
- Status
- APPID Tag (if enabled)

## Enabling APPID Tag Checking

To enable APPID tag checking:
1. Open the Lambda function code in the CloudFormation template
2. Uncomment the tag checking code block
3. Deploy or update the stack

## IAM Permissions

The solution creates an IAM role with the following permissions:
- `support:DescribeTrustedAdvisorCheckResult`
- `ses:SendEmail`
- `ses:SendRawEmail`
- `lambda:ListTags`
- `sts:GetCallerIdentity`
- CloudWatch Logs permissions

## Monitoring and Logging

- All function executions are logged to CloudWatch Logs
- Stack creation events are available in CloudFormation console
- Email delivery status can be monitored in SES console

## Troubleshooting

Common issues and solutions:

1. **Email not received**
   - Verify SES email address
   - Check SES sending limits
   - Review CloudWatch Logs

2. **Function execution failed**
   - Verify AWS Support plan level
   - Check IAM permissions
   - Review CloudWatch Logs

3. **Missing APPID tags**
   - Confirm tag checking code is uncommented
   - Verify IAM permissions for tag access

## Cost Considerations

- Lambda invocation (1 daily)
- CloudWatch Logs storage
- SES email sending
- Trusted Advisor API calls

## Security Considerations

- Email addresses should be verified in SES
- IAM roles follow least privilege principle
- All actions are logged to CloudWatch
- No sensitive data is stored

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Submit pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Authors

Your Name/Organization

## Acknowledgments

- AWS Documentation
- AWS Support
- Community Contributors

## Support

For support, please:
1. Check existing GitHub issues
2. Review CloudWatch Logs
3. Open a new GitHub issue

---

**Note**: This system requires AWS Business or Enterprise Support for Trusted Advisor API access.
