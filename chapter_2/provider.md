## Provider概念
provider是terraform的核心插件类型之一。在整个过程中，terraform cli会启动一个进程，并通过 `go-plugin` 加载另一个插件进程（在go中，原生plugin支持并不友好，HashiCorp公司自己开发了一个 `go-plugin`工程，当terraform进程执行完毕结束时，插件进程会被关闭），然后通过grpc协议进行通信，插件程序通过CRUD调用各个云平台提供的sdk或者https api来操作平台资源。

## Provider插件的下载与存放
在上一节的操作里可以看到插件会被存放在以下地址：
```
/Users/suxiangdong/code/terraform/chapter_1/.terraform/providers/registry.terraform.io/hashicorp/aws/5.81.0/darwin_arm64/terraform-provider-aws_v5.81.0_x5
```

有一个问题，就是在不同的terraform项目中，每个插件都会被下载一次，非常浪费，因此最好统一存放目录，terraform启动时先查找指定路径下的provider文件，如果存在则直接复制到当前工作目录下。
### 通过环境变量配置存放路径
```
export TF_PLUGIN_CACHE_DIR="$HOME/.terraform.d/plugin-cache"
```

### 通过配置修改存放路径(推荐)
terraform启动首先查找用户目录下的.terraformrc的配置文件，如下配置即可：
```
plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
```
文件夹要手动创建，不然会报错
```
mkdir -p $HOME/.terraform.d/plugin-cache
```

## provider仓库
[仓库地址](https://registry.terraform.io/browse/providers)
既可以使用仓库里的provider，也可以贡献provider。一般每个provider里都会有相应data、resource的使用说明。


## provider声明
provider必须被声明，并且要传入一些关键信息才可以正常使用。
```yaml
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws" # 指定插件名称
      version = "~>5.0" #指定版本
    }
  }
}

provider "aws" {
  access_key                  = "test"
  secret_key                  = "test"
  region                      = "us-east-1"
  s3_use_path_style           = false
  skip_credentials_validation = true
  skip_metadata_api_check     = true
  skip_requesting_account_id  = true

  endpoints {
    apigateway     = "http://localhost:4566"
    apigatewayv2   = "http://localhost:4566"
    cloudformation = "http://localhost:4566"
    cloudwatch     = "http://localhost:4566"
    dynamodb       = "http://localhost:4566"
    ec2            = "http://localhost:4566"
    es             = "http://localhost:4566"
    elasticache    = "http://localhost:4566"
    firehose       = "http://localhost:4566"
    iam            = "http://localhost:4566"
    kinesis        = "http://localhost:4566"
    lambda         = "http://localhost:4566"
    rds            = "http://localhost:4566"
    redshift       = "http://localhost:4566"
    route53        = "http://localhost:4566"
    s3             = "http://s3.localhost.localstack.cloud:4566"
    secretsmanager = "http://localhost:4566"
    ses            = "http://localhost:4566"
    sns            = "http://localhost:4566"
    sqs            = "http://localhost:4566"
    ssm            = "http://localhost:4566"
    stepfunctions  = "http://localhost:4566"
    sts            = "http://localhost:4566"
  }
}
```