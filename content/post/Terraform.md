+++ 
draft = true
date = 2019-01-21T19:15:42+08:00
title = "Terraform"
slug = "" 
tags = []
categories = []
+++


# What is Terraform?

* Terraform是一个能安全有效地构建，变更和版本化基础设施的工具。主流云服务商都有其对应的接口(provider)，当然你也可以自己实现它的接口来管理你自己的基础设施。
* 你只需要在它的配置文件中定义一个应用或者整个数据中心运行所需的内容。它就会生成一个可执行的计划(方案)来描述它将如何实现配置文件中所定义的内容，并根据计划构建你的基础设施。如果你的配置文件有变化，它也会自动的根据变化增量地调整。
* 它可以管理地基础设施有很多，比如：计算实例，存储资源，网络资源等基础应用以及DNS, Saas等高级应用。

# 入门教程

[官方教程](https://learn.hashicorp.com/terraform/getting-started/install.html)

# 使用案列

* [All](https://www.terraform.io/docs/providers/index.html)  
* [腾讯云](https://www.terraform.io/docs/providers/tencentcloud/index.html)  
* [阿里云](https://www.terraform.io/docs/providers/alicloud/index.html)  

# 自定义

[terraform-provider-libvirt](https://github.com/dmacvicar/terraform-provider-libvirt)


## 
```go
func main() {
    fmt.Println("Hello, World!")
}
```