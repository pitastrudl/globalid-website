# GlobalID website

A new shiny websiste for the globalID company. The website is hosted via CloudFront from a statically hosted S3 Bucket. Website template was borrowed from https://github.com/designmodo/html-website-templates.

## Getting Started

This application is primarily designed to be deployed via S3 Website endpoint hosting fronted by AWS CloudFront. You will need an AWS account. Of course this can be served via any modern webserver.

### Prerequisites

What things you need to deploy:

```
- Provisioned S3 Bucket with custom bucket policies and enabled static website hosting
- CloudFront distribution with S3 as the origin and OAC
- A DNS record in Route53
- Provisioned SSL certificate in ACM
- Custom policies for deployment of website
```

### Installing

A simple s3 sync of the `website/` folder to the root of the S3 bucket is enough.

To sync the website:

```bash
aws s3 sync website/ s3://bucket-name/ --delete
```

And invalidate the cache:

```bash
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

You may also use this repo to automate the deployments. The deploy actions will act on merged PRs if the relevant files are changed.

## Improvements/TODO

### AWS
- The AWS infrastructure could be provisioned via Terraform
  - Automate S3, Cloudfront, Route 53 creation
- Decrease caching TTL on HTML files in CloudFront, so users get the most fresh version
- Disable compression on non-compressible files

### Git/Deployments

- Add linting job to GH Actions
- Access for the Github Actions to AWS resources could be done by using an IAM role
- Use conventional commits
- Add releases on builds from main
