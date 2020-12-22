---
title: 管理项目级别应用
description: 部署应用之后，与管理单独的工作负载/资源相比，使用应用商店的好处之一是能够轻松统一管理许多工作负载和资源。您可以克隆，升级或回滚应用。部署应用后，您可以轻松升级到其他模板版本。升级应用后，您可以轻松回滚到之前的某个修订版本。
keywords:
  - rancher 2.0中文文档
  - rancher 2.x 中文文档
  - rancher中文
  - rancher 2.0中文
  - rancher2
  - rancher教程
  - rancher中国
  - rancher 2.0
  - rancher2.0 中文教程
  - 应用商店
  - 管理项目级别应用
---

部署应用之后，与管理单独的工作负载/资源相比，使用应用商店的好处之一是能够轻松统一管理许多工作负载和资源。您可以克隆，升级或回滚应用。

## 克隆应用

部署应用后，您可以轻松的克隆它，以使用几乎相同的配置创建另一个应用。它节省了您手动填写重复信息的时间。

## 升级应用

部署应用后，您可以轻松升级到其他模板版本。

1. 从**全局**视图中，导航到要升级的应用所在项目。

1. 在主导航栏中，选择**应用商店**，单击**启动**。

1. 找到要升级的应用，然后单击省略号以找到**升级**。

1. 选择要部署的**模板版本**。

1. （可选）更新您的**配置选项**。

1. （可选）通过选中**在升级过程中需要时，删除并重新创建资源**。复选框，选择是否要强制升级应用。

   > 在 Kubernetes 中，某些字段被设计为不可变的或无法直接更新。从 v2.2.0 开始，无论这些字段如何，您现在都可以强制更新应用商店的应用。Helm 会根据需要判断是否应该删除并重新创建资源。

1. 可以通过**预览**部分，查看 Chart 中的 YAML 文件。如果确认，请单击**启动**。

**结果：** 您的应用已更新。您可以从项目的以下位置查看应用状态：

- **资源 > 工作负载**
- **应用商店 > 应用列表**

## 回滚应用

升级应用后，您可以轻松回滚到之前的某个修订版本。

1. 从**全局**视图中，导航到要回滚的应用所在的项目。

1. 在主导航栏中，选择**应用商店**。

1. 找到要回滚的应用，然后单击省略号以找到**回滚**。

1. 选择要回滚的**版本**。默认情况下，Rancher 最多保存最近的 10 个修订版本。

1. （可选）通过选中**在回滚过程中需要时，删除并重新创建资源**复选框，选择是否要强制回滚应用。

1. 确定并单击**回滚**.

**结果**：您的应用已更新。您可以从项目的以下位置查看应用状态：

- **资源 > 工作负载**
- **应用商店 > 应用列表**

## 删除应用

为了防止您无意中删除共享命名空间的其他应用，删除应用本身不会删除应用所在的命名空间。

因此，如果要删除一个应用和其部署的命名空间，应分别删除该应用和命名空间：

1. 使用应用的**删除**功能来删除应用。

1. 从**全局**视图中，导航到要删除的应用所在的项目。

1. 从主菜单中，选择**命名空间**。

1. 查找运行您的应用的命名空间。选择它，然后单击**删除**。

**结果：** 应用的部署及其命名空间被删除。