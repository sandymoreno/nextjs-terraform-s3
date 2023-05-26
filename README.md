# Project Next.js Deployment with Terraform

This project utilizes Next.js and leverages Terraform to automate the deployment infrastructure on AWS. The deployment includes setting up an AWS S3 bucket to store the site assets, configuring AWS CloudFront as a content delivery network (CDN) for improved performance, and integrating with AWS Route 53 for domain management.

## Terraform Configuration

The `terraform` block defines the required provider and version for AWS.

## AWS Provider Configuration

The `provider "aws"` block configures the AWS provider, specifying the desired AWS region.

## Local Variables

The `locals` block defines a local variable `s3_origin_id` used as an identifier for the S3 bucket origin in AWS CloudFront.

## AWS Resources

### AWS S3 Bucket

The `aws_s3_bucket` resource creates an S3 bucket to store the Next.js site assets. The `bucket` attribute specifies the bucket name.

### AWS S3 Bucket Policy

The `aws_s3_bucket_policy` resource sets up a bucket policy for the S3 bucket. It allows AWS CloudFront service principal to retrieve objects from the bucket using the `s3:GetObject` action. The policy ensures that only CloudFront can access the objects in the bucket.

### AWS S3 Object

The `aws_s3_object` resource uploads the `index.html` file to the S3 bucket. It sets the `content_type` attribute to "text/html" to ensure proper interpretation by web browsers.

### AWS CloudFront Origin Access Control

The `aws_cloudfront_origin_access_control` resource configures access control for the S3 bucket origin in AWS CloudFront. It specifies the `name`, `description`, `origin_access_control_origin_type`, `signing_behavior`, and `signing_protocol` attributes.

### AWS ACM Certificate

The `aws_acm_certificate` resource creates an ACM (AWS Certificate Manager) certificate for the specified domain name. It uses DNS validation to verify ownership of the domain.

### AWS Route 53 Record

The `aws_route53_record` resource creates a DNS record in AWS Route 53 to associate the ACM certificate validation with the domain. It creates an "A" record for the domain and uses the certificate's domain validation options for configuration.

### AWS ACM Certificate Validation

The `aws_acm_certificate_validation` resource validates the ACM certificate by associating the certificate ARN with the DNS record created in AWS Route 53.

### AWS CloudFront Distribution

The `aws_cloudfront_distribution` resource sets up a CloudFront distribution, which acts as a CDN for the Next.js site. It configures the distribution with the S3 bucket as the origin and enables IPv6 support. It specifies cache behaviors, SSL/TLS settings, comment, default root object, aliases, caching, and tagging.

### AWS Route 53 Record for CloudFront

The `aws_route53_record` resource creates an "A" record in AWS Route 53 to associate the domain with the CloudFront distribution. It uses an alias record that points to the CloudFront distribution domain name.

## Deployment

To deploy the Next.js project with the configured infrastructure, run Terraform commands after setting up your AWS credentials and configuring the variables (`var`) used in the Terraform code. The deployment process will create the necessary resources on AWS, including the S3 bucket, CloudFront distribution, ACM certificate, DNS records, and associated configurations.

Make sure to replace the following variables in the code:

-   `var.region`: Set your desired AWS region.
-   `var.bucket_name`: Set the desired name for the S3 bucket.
-   `var.domain`: Set the domain name for the Next.js project.

Feel free to adjust any other settings or add more resources as per your project requirements.

This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Next.js Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.
