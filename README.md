# S3 React

Terraform module for bringing up a statically hosted website via AWS cloudfront/s3/route53.

## Features
* Static Website Hosting
* S3 Bucket
* HTTPS Certificate Provisioning
* CNAME Creation
* Cloudfront Distribution
* Restricted access to bucket via Cloudfront

## Getting Started

### Prerequisites
* [Route53 Domain Name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html)
* [Terraform CLI](https://www.terraform.io/downloads.html)

### Building Infrastructure

1. Create `main.tf`
```
module "static_hosted_site" {
  source = "<path to github module>"
  alias_name = "mytestsite"
  domain_name = "domain.com"
  index_document = "index.html"
}

output "s3_bucket_name" {
    value = module.static_hosted_site.s3_bucket_name
} 

output "cloudfront_distribution_id" {
    value = module.static_hosted_site.cloudfront_distribution_id
} 
```

2. Run

```terraform init```

3. Run

```terraform apply```

4. Successful run will supply output like

```
Outputs:

cloudfront_distribution_id = <cloudfront id>
s3_bucket_name = <bucket name/url>
```

## Next Steps

1. Create a new folder for your website

```
mkdir web
touch web/index.html
echo "<p>Hello World<p>" > web/index.html
```

2. Sync the folder with your s3 bucket

```
export BUCKET_NAME=<bucket name from terraform output>
aws s3 sync web s3://${BUCKET_NAME}

```

3. Default ttl is set to `3600s`, if you want to force cloudfront to invalidate the cache
```
export CLOUDFRONT_ID=<cloudfront id from terraform output>
aws cloudfront create-invalidation --distribution-id $CLOUDFRONT_ID --paths /*
```