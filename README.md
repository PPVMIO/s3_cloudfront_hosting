# S3 React

Terraform module for bringing up a react app in an s3 bucket.

## Build the infrastructure

1. Create `main.tf`
```
module "anxiety_frontend" {
  source = "<path to github module>"
  alias_name = "mytestsite"
  domain_name = "domain.com"
  index_document = "index.html"
  error_document = "error.html"
}

output "s3_bucket_name" {
    value = module.anxiety_frontend.s3_bucket_name
} 

output "cloudfront_distribution_id" {
    value = module.anxiety_frontend.cloudfront_distribution_id
} 
```

2. Run

```tf init```

3. Run

```tf apply```

4. Successful output will look like

```
Outputs:

cloudfront_distribution_id = <cloudfront id>
s3_bucket_name = <bucket name/url>
```

## Deploy a react app


1. Create a react app 
2. Build the react app 
3. Sync the react app with the s3 bucket

Useful `.env` file

```
export BUILD_FOLDER="./build"
export BUCKET_NAME="<bucket name from terraform output>"
alias build="yarn install && yarn build --prod"
alias deployapp="aws s3 sync ${BUILD_FOLDER} s3://${BUCKET_NAME}"
alias deployinf="tf apply"
```





