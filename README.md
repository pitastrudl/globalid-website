# GlobalID website

A new shiny websiste for the globalID company. The website is hosted via CloudFront from a statically hosted S3 Bucket. Website template was borrowed from https://github.com/designmodo/html-website-templates.
> P.S. this a mock repository.
## Getting Started

This application is primarily designed to be deployed via S3 Website endpoint hosting fronted by AWS CloudFront. You will need an AWS account. Of course this can be served via any modern webserver.

### Prerequisites

What things you need to deploy:

```
- Provisioned S3 Bucket with custom bucket policies and enabled static website hosting
- CloudFront distribution with S3 as the origin, OAC enabled and index.html as root
- A DNS record in Route53
- Provisioned SSL certificate in ACM
- Custom policies for deployment of website
- Provisioned IAM role (Least privelage approach) with a separate user for deployment
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
  - Automate S3, Cloudfront, Route 53 creation with custom policies
- Decrease caching TTL on HTML files in CloudFront, so users get the most fresh version of the website
- Disable compression on non-compressible files (tar,zips etc)
- Use GitHub OIDC for setting up AWS credentials in the Github Actions

### Git/Deployments

- Add linting job and code scanning to GH Actions
- Use conventional commits
- Add releases on builds from main branch
- For better security posture, custom scripts could be developed for CloudFront invalidations and AWS credentials setup instead of using GH premade Actions
- Make deployments only on merged PRs and protect the main branch
- If ingress data would be a concern, s3 sync could be done with an --update switch.
