---
title: Python 用 Azure リソース ライブラリ
description: Python 用 Azure リソース ライブラリのリファレンス
keywords: Azure, Python, SDK, API, リソース
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f331a88ceac73449c9c1aff7bccbbaff4c4ba7be
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277379"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="5a4d2-104">Python 用 Azure リソース ライブラリ</span><span class="sxs-lookup"><span data-stu-id="5a4d2-104">Azure Resources libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="5a4d2-105">概要</span><span class="sxs-lookup"><span data-stu-id="5a4d2-105">Overview</span></span> 
<span data-ttu-id="5a4d2-106">[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) を使用して、グループ内のリソースをデプロイ、監視、管理します。</span><span class="sxs-lookup"><span data-stu-id="5a4d2-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="5a4d2-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="5a4d2-107">Management API</span></span>
<span data-ttu-id="5a4d2-108">Management API を使用して、リソース グループを作成し、テンプレートからリソースをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="5a4d2-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a><span data-ttu-id="5a4d2-109">例</span><span class="sxs-lookup"><span data-stu-id="5a4d2-109">Example</span></span> 
<span data-ttu-id="5a4d2-110">米国東部の Azure リージョンに新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="5a4d2-110">Create a new resource group in the Azure Eastern US region.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5a4d2-111">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="5a4d2-111">Explore the Management APIs</span></span>](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a><span data-ttu-id="5a4d2-112">サンプル</span><span class="sxs-lookup"><span data-stu-id="5a4d2-112">Samples</span></span>
[<span data-ttu-id="5a4d2-113">Azure のリソースとリソース グループを管理する</span><span class="sxs-lookup"><span data-stu-id="5a4d2-113">Manage Azure resources and resource groups</span></span>](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

<span data-ttu-id="5a4d2-114">Azure Resource Manager のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5a4d2-114">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) of Azure Resource Manager samples.</span></span>
