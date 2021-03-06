---
title: 多集群应用
description: 通常，大多数应用都部署在单个 Kubernetes 集群上，但是有时您可能希望跨不同的集群和/或项目部署同一个应用的多个副本。在 Rancher 中多集群应用使用 Helm Chart ，并可以跨多个集群部署应用。因为能够跨多个集群部署相同的应用，因此可以避免在对每个集群上重复执行相同的操作期间引入的人为错误。使用多集群应用，您可以确保应用在所有项目/集群中具有相同的配置，并能够根据目标项目来覆盖不同的参数。由于多集群应用被视为单个应用，因此易于管理和维护。
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
  - 多集群应用
---

_自 v2.2.0 起可用_

通常，大多数应用都部署在单个 Kubernetes 集群上，但是有时您可能希望跨不同的集群和/或项目部署同一个应用的多个副本。在 Rancher 中**多集群应用**使用 Helm Chart ，并可以跨多个集群部署应用。因为能够跨多个集群部署相同的应用，因此可以避免在对每个集群上重复执行相同的操作期间引入的人为错误。使用多集群应用，您可以确保应用在所有项目/集群中具有相同的配置，并能够根据目标项目来覆盖不同的参数。由于多集群应用被视为单个应用，因此易于管理和维护。

[全局应用商店](/docs/rancher2/catalog/_index)中的任何 Helm Chart 均可作为多集群应用来进行部署和管理。

创建多集群应用之后，可以对[全局 DNS](/docs/rancher2/catalog/globaldns/_index) 进行编程，以使其更易于访问该应用。

## 先决条件

要在 Rancher 中创建多集群应用，您必须至少具有以下权限之一：

- 目标集群中的目标项目的[项目成员角色](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)，使您能够创建，读取，更新和删除工作负载。
- 目标项目的集群的[集群所有者角色](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)。

## 启动多集群应用

1. 从**全局**视图中，在导航栏中选择**多集群应用**。点击**启动**。

2. 找到您要启动的应用，然后单击**查看详细信息**。

3. （可选）查看详细说明，这些详细说明来自于 Helm Chart 的**README**。

4. 在**配置选项**下，为多集群应用输入**名称**。默认情况下，该名称还用于在多集群应用的每个[目标项目](#目标项目)中创建 Kubernetes 命名空间。该命名空间名称为`<MULTI-CLUSTER_APPLICATION_NAME>-<PROJECT_ID>。

5. 选择一个**模板版本**。

6. 完成[多集群应用特定的配置选项](#多集群应用配置选项)以及[应用配置选项](#应用配置选项)。

7. 选择可以与多集群应用交互的**成员**。

8. 添加需要根据项目的不同，使用非默认应用配置的应答。请参阅，[指定针对项目的特定配置选项](#指定针对项目的特定配置选项)。

9. 可以在**预览**部分中查看 Chart 中到 YAML 文件。如果确认，请单击**启动**。

**结果：** 您的应用已部署到了所选的命名空间。您可以从多集群应用页面查看应用状态。

## 多集群应用配置选项

Rancher 将多集群应用的配置选项分为几个部分。

### 目标项目

在**目标**部分中，选择要在其中部署应用的[项目](/docs/rancher2/cluster-admin/projects-and-namespaces/_index)。项目列表基于您有权访问哪些项目。对于您选择的每个项目，它将被添加到列表中，其中显示了所选的集群名称和项目名称。要删除目标项目，请单击 **-**。

### 升级选项

在决定升级应用时，在**升级**部分中，选择要使用的升级策略。

- **滚动更新（批量）：** 选择此升级策略时，可以配置一次升级的应用数量**批量大小**，**间隔**是指定要等待多少秒开始下一批更新。

- **同时升级所有应用：** 选择此升级策略时，所有项目中的所有应用将同时升级。

### 角色

在**角色**部分中，定义多集群应用的角色。通常，当用户[启动应用商店应用](/docs/rancher2/catalog/launching-apps/_index)时，该特定用户的权限将用于创建该应用所需的所有工作负载/资源。

对于多集群应用，该应用由**系统用户**部署，并被分配为所有基础资源的创建者。使用系统用户是因为实际用户有可能会被从目标项目中移除。比如，如果直接使用实际用户的话，如果实际用户被从某个项目中移除了，这个用户就不能管理多集群应用在这个项目中的副本了。

Rancher 将让您从**角色**的两个选项中进行选择：**项目**和**集群**。Rancher 将会根据用户的权限判断是否可以允许使用选中的角色进行创建。

- **项目** - 这等效于[项目成员](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)。如果选择此角色，Rancher 将检查在所有目标项目中，用户是否至少具有[项目成员](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)角色。虽然可能未明确授予用户项目成员角色，但如果用户是[系统管理员](/docs/rancher2/admin-settings/rbac/global-permissions/_index)，或[集群所有者](/docs/rancher2/admin-settings/rbac/global-permissions/_index)或[项目所有者](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)，则都认为该用户拥有适当的权限级别。

- **集群** - 这等效于[集群所有者](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)。如果选择此角色，Rancher 将检查所有目标项目中的用户是否具有该项目所在的集群的[集群所有者](/docs/rancher2/admin-settings/rbac/cluster-project-roles/_index)角色。尽管可能未明确授予下游集群所有者角色，但是如果用户是[系统管理员](/docs/rancher2/admin-settings/rbac/global-permissions/_index)，则认为该用户具有适当的权限级别。

在点击创建应用的时候，Rancher 将检查您是否在目标项目中具有这些权限。

> **注意：** 有些应用（例如 **Grafana** 或 **Datadog**）需要访问特定的集群范围资源。所以这些应用将需要**集群**角色。如果以后发现应用需要集群角色，则可以升级多集群应用以更新角色。

## 应用配置选项

对于每个 Helm Chart，必须输入所需答案列表才能成功部署 Chart。输入答案时，必须遵守[使用 Helm：--set 的格式和限制](https://helm.sh/docs/intro/using_helm/#the-format-and-limitations-of-set)，因为 Rancher 将其作为`--set`标志传递给 Helm。

> 例如，输入包含两个用逗号分隔的值的答案（即`abc,bcd`）时，请用双引号将这些值引起来（即`"abc,bcd"`）。

### 使用`questions.yml`文件

如果您要部署的 Helm 应用包含了一个`questions.yml`文件，则 Rancher UI 将转换该文件，并允许用户以表单的形式来填写答案。

### 使用原生 Helm Chart 键值对

对于原生的 Helm Chart(即，来自**Helm Stable**或**Helm Incubator**的应用或没有配置`questions.yml`文件的[自定义的应用商店](/docs/rancher2/catalog/adding-catalogs/_index)，答案在“答案”部分中需要通过键值对的方式进行配置。这些答案用于覆盖应用的默认值。

### 成员

默认情况下，多集群应用只能由创建它的用户管理。在**成员**部分中，可以添加其他用户，以便他们可以帮助管理或查看多集群应用。

1. 通过在**成员**搜索框中键入成员的名称，找到要添加的用户。

2. 选择该成员的**访问类型**。多集群项目有三种访问类型，请仔细阅读以了解这些访问类型的含义。

   - **所有者**：此访问类型可以管理多集群应用的任何配置部分，包括模板版本，多集群应用[特定的配置选项](#多集群应用配置选项)，应用的[配置选项](#应用配置选项)，可以与多集群应用交互的[成员](#成员)和[指定针对项目的特定配置选项](#指定针对项目的特定配置选项)。由于创建的多集群应用具有与用户不同的权限集，因此多集群应用的任何**所有者**都可以管理/删除应用中的[目标项目](#目标项目)，而无需显式访问这些项目。仅应为受信任的用户提供此访问类型。

   - **成员**：此访问类型只能修改模板版本，多集群应用[特定的配置选项](#多集群应用配置选项)和[指定针对项目的特定配置选项](#指定针对项目的特定配置选项)。由于使用来自用户的不同权限集创建了多集群应用，因此多集群应用的任何**成员**都可以修改该应用，而无需显式访问这些项目。仅应为受信任的用户提供此访问类型。

   - **只读**：此访问类型无法修改多集群应用的任何配置选项。用户只能查看这些应用。

   > **注意：** 请确保仅向受信任的用户授予**所有者**或**成员**访问权限，因为他们将能够管理多集群应用在目标项目中的部署的应用，即使他们可能没有访问这个项目的权限。

### 指定针对项目的特定配置选项

使用相同配置在多个集群/项目中部署相同应用的能力是多集群应用的主要优势之一。可能有一个特定的项目需要稍有不同的配置选项，但是您希望把它和其他项目在一起作为多集群应用的目标项目来处理。您可以创建特定项目的特定[应用配置选项](#应用配置选项)，而不是创建全新的应用。

1. 在**应答参数**部分中，单击**添加应答参数**。

2. 对于每个答案覆盖，您可以选择以下内容：

   - **区域**： 在配置选项中选择要覆盖答案的目标项目。

   - **问题**： 选择要覆盖的问题。

   - **答案**： 输入您要改用的答案。

## 升级多集群应用的角色和项目

- **更改现有多集群应用上的角色**

  创建者和有**所有者**访问权限的用户都可以升级多集群应用的角色。修改角色时，我们将检查用户在当前所有目标项目中是否具有该确切角色。Rancher 将根据多集群应用的**角色**字段，相应的来检查用户是否有系统管理员，集群所有者或项目所有者的权限。

- **添加/删除目标项目**

  1. 创建者和有**所有者**访问权限的用户都可以添加或删除其目标项目。在添加新项目时，我们检查此请求的调用者是否在要添加的新项目中具有多集群应用上配置的有角色。Rancher 将根据多集群应用的**角色**字段，相应的来检查用户是否有系统管理员，集群所有者或项目所有者的权限。
  2. 删除目标项目时，我们不执行这些成员资格检查。这是因为调用者可能已经失去了对某个目标项目的权限，或者该项目可能已被删除了，因此调用者希望将其从目标列表中删除。

## 多集群应用管理

与同类型的多个单个应用相比，使用多集群应用的好处之一是易于管理。可以克隆，升级或回滚多集群应用。

1. 从**全局**视图中，在导航栏中选择**多集群应用**。

2. 选择要对其执行以下操作之一的多集群应用，然后单击**垂直省略号（...）**。选择以下选项之一：

   - **克隆**： 使用相同的配置创建另一个多集群应用。通过使用此选项，您可以轻松地复制多集群应用。

   - **升级**： 升级您的多集群应用以更改配置的某些部分。对多集群应用执行升级时，如果您具有正确的[访问类型](#成员)，则可以修改[升级策略](#升级选项)。

   - **回滚**： 将您的应用回滚到特定版本。如果升级后，有一个或多个[目标项目](#目标项目)中的多集群应用出现了问题，并且 Rancher 已存储了多达 10 个修订版本的多集群应用。您可以选择回滚多集群应用。它将还原**所有**目标项目中的应用，而不仅仅是受升级问题影响的目标项目。

## 删除多集群应用

1. 从**全局**视图中，在导航栏中选择**多集群应用**。

2. 选择要删除的多集群应用，然后单击**垂直省略号（...）>删除**。删除多集群应用时，所有目标项目中的所有应用和命名空间都将被删除。

> **注意：** 目标项目中为多集群应用创建的应用不能单独删除。仅当删除多集群应用时才能删除应用。
