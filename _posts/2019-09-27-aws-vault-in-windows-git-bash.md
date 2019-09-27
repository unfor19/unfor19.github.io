---
layout: post
title: aws-vault in Windows Git Bash
author: Meir Gabay
publication_date: 2019-Sep-27
---

## Install aws-vault

1. Download latest **Windows** version of aws-vault from here: https://github.com/99designs/aws-vault/releases
2. Rename the file `aws-vault-windows-386.exe` to `aws-vault.exe`
3. Copy `aws-vault.exe` to `C:\Program Files\Git\usr\bin\`
4. Open a new Git Bash terminal, and execute `aws-vault` :)

This trick is useful for executing any package in Windows Git Bash

## Add profile to vault

Profile name: **admin**

```
$ aws-vault add admin
Enter Access Key ID: KtPRUkRNtR6ccWmDjFRi
Enter Secret Access Key: u4zCTXgAbTL2VdKhMLwDcTpdhGv9M79PHFWRDxuL
Added credentials to profile "admin" in vault
```

## (Optional) Add MFA-Serial

Add `mfa_serial=arn:aws:iam::123456789012:mfa/admin` to relevant profile

```
$ vim ~/.aws/config

[profile admin]
mfa_serial=arn:aws:iam::123456789012:mfa/admin
```

## Execute AWS-CLI commands

Profile name: **admin**
You'll be prompted for MFA token only if you add mfa_serial to the config file

```
$ aws-vault exec admin -- aws s3 ls
Enter token for arn:aws:iam::123456789012:mfa/admin: 123456
2019-08-15 16:37:23 first-bucket
2019-09-01 12:32:43 second-bucket
```

Note: The command `aws sts get-session-token` doesn't work when using aws-vault.

## Execute Terraform commands

Profile name: **admin**

```
$ aws-vault exec admin -- terraform plan
Enter token for arn:aws:iam::123456789012:mfa/admin: 123456
Refreshing Terraform state in-memory prior to plan...
```

## Remove session credentials (cache)

Profile name: **admin**

```
$ aws-vault remove admin --sessions-only
Deleted 1 sessions.
```

Inspired by: [https://blog.gruntwork.io/authenticating-to-aws-with-environment-variables-e793d6f6d02e](https://blog.gruntwork.io/authenticating-to-aws-with-environment-variables-e793d6f6d02e)

Learn more about aws-vault: [https://github.com/99designs/aws-vault/blob/master/USAGE.md](https://github.com/99designs/aws-vault/blob/master/USAGE.md)
