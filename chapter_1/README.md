## 安装 for mac
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

## 验证
terraform version
Terraform v1.7.3

## 模拟云服务
### [LocalStack](https://www.localstack.cloud/)
```bash
docker run \
  --rm -it \
  -p 4566:4566 \
  -p 4510-4559:4510-4559 \
  localstack/localstack
```
### LocalStack ec2 资源类型
data部分的name和owners使用的类型资源参考[ec2 types](https://github.com/localstack/moto/blob/v5.0.0a3.post2/moto/ec2/resources/amis.json)

## commands
### init
会产生`.terraform.lock.hcl`文件，包含在仓库内，保证未来的执行通过lock文件得到统一的资源

### plan
将文件对应的资源变更列出

### apply
执行计划。当执行命令之后，terraform会重新执行一下 `terraform plan`，将变更说明，需要手动确认输入`yes`即可继续。

### destroy
无论是不是真正的云资源，一定完成测试之后执行清理命令，保持好习惯。