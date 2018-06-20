---
title: Python 用 Azure リソース ライブラリ
description: ''
keywords: Azure, Python, SDK, API, リソース
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: 32e13bee27db091f0bca12c7d9ae4fc62165f4c0
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
ms.locfileid: "20909395"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="820d8-103">Python 用 Azure リソース ライブラリ</span><span class="sxs-lookup"><span data-stu-id="820d8-103">Azure Resources libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="820d8-104">概要</span><span class="sxs-lookup"><span data-stu-id="820d8-104">Overview</span></span> 
<span data-ttu-id="820d8-105">リソース グループ内の Azure リソースを管理します。</span><span class="sxs-lookup"><span data-stu-id="820d8-105">Manage Azure resources in resource groups.</span></span>

| <span data-ttu-id="820d8-106">パッケージ</span><span class="sxs-lookup"><span data-stu-id="820d8-106">Package</span></span>  |  <span data-ttu-id="820d8-107">Description</span><span class="sxs-lookup"><span data-stu-id="820d8-107">Description</span></span> |
|---|---|
|<span data-ttu-id="820d8-108">[azure.mgmt.resource.features][1]</span><span class="sxs-lookup"><span data-stu-id="820d8-108">[azure.mgmt.resource.features][1]</span></span>|<span data-ttu-id="820d8-109">Azure Feature Exposure Control (AFEC) は、リソース プロバイダーがユーザーへの機能の公開を制御するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="820d8-109">Azure Feature Exposure Control (AFEC) provides a mechanism for the resource providers to control feature exposure to users.</span></span>|
|<span data-ttu-id="820d8-110">[azure.mgmt.resource.links][2]</span><span class="sxs-lookup"><span data-stu-id="820d8-110">[azure.mgmt.resource.links][2]</span></span>|<span data-ttu-id="820d8-111">Azure リソースは、論理的な関係を形成するために互いをリンクすることができます。</span><span class="sxs-lookup"><span data-stu-id="820d8-111">Azure resources can be linked together to form logical relationships.</span></span> <span data-ttu-id="820d8-112">異なるリソース グループに属しているリソースの間にリンクを確立することができます。</span><span class="sxs-lookup"><span data-stu-id="820d8-112">You can establish links between resources belonging to different resource groups.</span></span>|
|<span data-ttu-id="820d8-113">[azure.mgmt.resource.locks][3]</span><span class="sxs-lookup"><span data-stu-id="820d8-113">[azure.mgmt.resource.locks][3]</span></span>|<span data-ttu-id="820d8-114">Azure リソースは、組織内の他のユーザーによるリソースの削除または変更を防ぐために、ロックすることができます。</span><span class="sxs-lookup"><span data-stu-id="820d8-114">Azure resources can be locked to prevent other users in your organization from deleting or modifying resources.</span></span>|
|<span data-ttu-id="820d8-115">[azure.mgmt.resource.managedapplications][4]</span><span class="sxs-lookup"><span data-stu-id="820d8-115">[azure.mgmt.resource.managedapplications][4]</span></span>|<span data-ttu-id="820d8-116">ARM マネージ アプリケーション (アプライアンス)。</span><span class="sxs-lookup"><span data-stu-id="820d8-116">ARM managed applications (appliances).</span></span>|
|<span data-ttu-id="820d8-117">[azure.mgmt.resource.policy][5]</span><span class="sxs-lookup"><span data-stu-id="820d8-117">[azure.mgmt.resource.policy][5]</span></span>|<span data-ttu-id="820d8-118">リソースへのアクセスを管理および制御するには、カスタマイズしたポリシーを定義して一定のスコープで割り当てます。</span><span class="sxs-lookup"><span data-stu-id="820d8-118">To manage and control access to your resources, you can define customized policies and assign them at a scope.</span></span>|
|<span data-ttu-id="820d8-119">[azure.mgmt.resource.resources][6]</span><span class="sxs-lookup"><span data-stu-id="820d8-119">[azure.mgmt.resource.resources][6]</span></span>| <span data-ttu-id="820d8-120">リソースとリソース グループを扱うための操作を提供します。</span><span class="sxs-lookup"><span data-stu-id="820d8-120">Provides operations for working with resources and resource groups.</span></span>|
|<span data-ttu-id="820d8-121">[azure.mgmt.resource.subscriptions][7]</span><span class="sxs-lookup"><span data-stu-id="820d8-121">[azure.mgmt.resource.subscriptions][7]</span></span>|<span data-ttu-id="820d8-122">すべてのリソース グループとリソースは、サブスクリプション内に存在します。</span><span class="sxs-lookup"><span data-stu-id="820d8-122">All resource groups and resources exist within subscriptions.</span></span> <span data-ttu-id="820d8-123">これらの操作では、サブスクリプションとテナントに関する情報を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="820d8-123">These operation enable you get information about your subscriptions and tenants.</span></span>|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a><span data-ttu-id="820d8-124">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="820d8-124">Install the libraries</span></span> 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="820d8-125">例</span><span class="sxs-lookup"><span data-stu-id="820d8-125">Example</span></span>
<span data-ttu-id="820d8-126">次の例は、リソース グループの作成方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="820d8-126">The following is an example of how to create a resource group.</span></span> 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

<span data-ttu-id="820d8-127">アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。</span><span class="sxs-lookup"><span data-stu-id="820d8-127">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span> 