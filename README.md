# serverless-domain-manager
Create custom domain names that your lambda can deploy to with serverless. Allows for base path mapping when deploying and deletion of domain names.

# About Amplify
Amplify builds innovative and compelling digital educational products that empower teachers and students across the country. We have a long history as the leading innovator in K-12 education - and have been described as the best tech company in education and the best education company in tech. While others try to shrink the learning experience into the technology, we use technology to expand what is possible in real classrooms with real students and teachers.

Learn more at https://www.amplify.com

# Getting Started

## Prerequisites
Make sure you have the following installed before starting:
* [nodejs](https://nodejs.org/en/download/)
* [npm](https://www.npmjs.com/get-npm?utm_source=house&utm_medium=homepage&utm_campaign=free%20orgs&utm_term=Install%20npm)
* [serverless](https://serverless.com/framework/docs/providers/aws/guide/installation/)

The IAM role that is deploying the lambda will need the following permissions:
```
acm: ListCertificate
apigateway: GET
apigateway: POST
route53: ListHostedZones
route53: ChangeResourceRecordSets
```

## Installing
```# From npm (recommended)
npm install serverless-domain-manager

# From github
npm install https://github.com/amplify-education/serverless-domain-manager.git
```

Then make the following edits to your serverless.yaml file:
```yaml
plugins:
  - serverless-domain-manager

custom:
  customDomain:
    basePath:
    domainName:
    stage:
```

## Running

To create the custom domain:
```
serverless create_domain
```

To deploy with the custom domain:
```
severless deploy
```

To remove the created custom domain:
```
serverless delete_domain
```
# How it works
Creating the custom domain takes advantage of Amazon's Certificate Manager to assign a certificate to the given domain name. Based on already created certificate names, the plugin will search for the certificate that resembles the custom domain's name the most and assign the ARN to that domain name. The plugin then creates the proper CNAMEs for the domain through Route 53. Once the domain name is set it takes up to 40 minutes before it is initialized. After the certificate is initialized, `sls deploy` will create the base path mapping and assign the lambda to the custom domain name through Cloudfront.

## Running Tests
To run the test:
```
npm test
```
All tests should pass.

If there is an error update the node_module inside the serverless-vpc-discovery folder:
```
npm install
```

# Known Issues
* (5/23/2017) CloudFormation does not support changing the base path from empty to something or vice a versa. You must run `sls remove` to remove the base path mapping.
* (5/23/2017) Amazon Certificate Manager only allows certificates from the `us-east-1` region certificates for use with CloudFront, and by extension, API Gateway Custom Domains (Results in a BadRequestException: Certificate name must be specified...).

# Responsible Disclosure
If you have any security issue to report, contact project maintainers privately.
You can reach us at <github@amplify.com>

# Contributing
We welcome pull requests! For your pull request to be accepted smoothly, we suggest that you:
1. For any sizable change, first open a GitHub issue to discuss your idea.
2. Create a pull request.  Explain why you want to make the change and what it’s for.
We’ll try to answer any PR’s promptly.
