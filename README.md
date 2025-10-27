# End of Life

This repository is being phased out. Some of the modules have been moved to
their own repository, and others will be moved as changes become necessary.
The following is a list of the new module sources:

- [sil-org/alb/aws](https://github.com/sil-org/terraform-aws-alb) replaces aws/alb
- [sil-org/backup/aws](https://github.com/sil-org/terraform-aws-backup) replaces aws/backup/rds
- [sil-org/cloudtrail/aws](https://github.com/sil-org/terraform-aws-cloudtrail) replaces aws/cloudtrail
- [sil-org/ecs-service/aws](https://github.com/sil-org/terraform-aws-ecs-service) replaces aws/ecs/service-only
- [sil-org/vpc/aws](https://github.com/sil-org/terraform-aws-vpc) replaces aws/vpc

# Terraform Modules
This repository contains Terraform modules that implement conventions we use
on AWS and other providers. You're welcome to use them, however be aware
that they may not be the best available or suit your needs exactly right. We
have opinions and conventions we like to follow and these modules follow them.

## Documentation
Each module provides its own documentation, so browse the source to read more
about each.

## MIT License
Copyright (c) 2017 SIL Global

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
