# WebApp for AWS

**WebApp for AWS** is a CloudFormation template to help setting up the infrastructure needed for a typical Single Page Application (SPA) based on AWS services.

![layout](./layout.png)

The following AWS services are used:
* [IAM](https://aws.amazon.com/iam/)
* [S3](https://aws.amazon.com/s3/)
* [CloudFront](https://aws.amazon.com/cloudfront/)
* [Lambda](https://aws.amazon.com/lambda/)

## Getting started

To quickly create a stack with the latest version click on the button below.

[<img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?templateURL=https://s3.amazonaws.com/supercharge-templates/webapp/latest/webapp.template)

---

# Contents

* [Parameters](#parameters)
  * [CloudFront Settings](#cloudfront-settings)
  * [Access Control & Authentication](#access-control--authentication)
* [Outputs](#outputs)
* [Authors](#authors)
* [License](#license)

# Parameters

## CloudFront Settings

You can specify the crucial parameters for the generated CloudFront distribution with the options below. Other settings have smart defaults: e.g. caching, error page handling, etc.

| Name                   | Description                             |
| ---------------------- | --------------------------------------- |
| `PriceClass`           | Price class (replication regions)       |
| `ViewerProtocolPolicy` | HTTP / HTTPS and redirect configuration |
| `CNAMEs`               | List of custom domain names             |
| `SSLCertificateARN`    | ACM certificate identifier for TLS      |

## Access Control & Authentication

With **WebApp for AWS** you can easily set up access control and authentication using a [Lambda@Edge](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html) function under the hood.

| Name              | Description                                    |
| ----------------- | ---------------------------------------------- |
| `VersionId`       | Versioning parameter (see note below)          |
| `AccessWhitelist` | IP ranges to enable access for                 |
| `AuthEnabled`     | HTTP Basic authentication enabled or not       |
| `AuthCredentials` | List of `<username>:<password>` pairs          |
| `AuthWhitelist`   | IP ranges that does not require authentication |

> **IMPORTANT:** Due to technical limitations in CloudFormation and [Lambda Function Versioning](https://docs.aws.amazon.com/lambda/latest/dg/versioning-aliases.html) you need to increment the `VersionId` counter during a stack update every time you change any of the parameters below!

### Access Control

With the `AccessWhitelist` parameter you can specify a comma separated list of CIDRs to allow accessing your app. Any client that is not on the whitelist will receive a `403 Forbidden` error.

*Examples:*

* If you want your app to be publicly accessible keep the default `0.0.0.0/0`
* If you want to restrict access to one specific IP address use `<IP address>/32`

### Authentication

You can also enable additional HTTP Basic authentication for your app on the top of [Access Control](#access-control). Use the `AuthEnabled` parameter with `yes` or `no` values to enable or disable HTTP Basic authentication. Unauthenticated users will receive a `401 Unauthorized` error.

In the `AuthCredentials` parameter you can define a comma separated list of `<username>:<password>` pairs, and `AuthWhitelist` is a comma separated list of CIDRs which does not require password authentication.

*Examples:*

* If you want a **public app** with password protection use:

> | Parameter         | Value                      |
> | ----------------- | -------------------------- |
> | `AccessWhitelist` | `0.0.0.0/0`                |
> | `AuthEnabled`     | `yes`                      |
> | `AuthCredentials` | `user1:pass1, user2:pass2` |

* If you want a **public app** but passwordless access for e.g. your internal IP range use:

> | Parameter         | Value                      |
> | ----------------- | -------------------------- |
> | `AccessWhitelist` | `0.0.0.0/0`                |
> | `AuthEnabled`     | `yes`                      |
> | `AuthCredentials` | `user1:pass1, user2:pass2` |
> | `AuthWhitelist`   | `<your IP range>`          |

* If you want a **private app** (not available for the public) and don't need additional password protection:

> | Parameter         | Value             |
> | ----------------- | ----------------- |
> | `AccessWhitelist` | `<your IP range>` |
> | `AuthEnabled`     | `no`              |

# Outputs

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| `AccessKeyId`        | IAM access key of bucket owner                  |
| `AccessKeySecret`    | IAM key secret of bucket owner                  |
| `BucketName`         | Name of S3 bucket                               |
| `DistributionId`     | ID of the CloudFront distribution               |
| `DistributionDomain` | Domain name for the CloudFront the distribution |

# Authors

[redcancode](https://github.com/redcancode) & [exoszajzbuk](https://github.com/exoszajzbuk) @ [![Supercharge](https://supercharge.io/assets/logo-small-with-text.png)](https://supercharge.io/)

# License

[MIT](LICENSE.md)

```
Copyright (c) 2018 Supercharge

MIT License

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
