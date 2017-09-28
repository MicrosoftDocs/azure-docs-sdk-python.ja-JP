---
title: "Python 用 Azure リソース ライブラリ"
description: "Python 用 Azure リソース ライブラリのリファレンス"
keywords: "Azure, Python, SDK, API, リソース"
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: d8a27bc2b5480b07728a0b3f892f5c3c70c913d9
ms.sourcegitcommit: 427b44319ae99bf19e167f427e7681b2103dd8e4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="azure-resources-libraries-for-python"></a>Python 用 Azure リソース ライブラリ

## <a name="overview"></a>概要 
[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) を使用して、グループ内のリソースをデプロイ、監視、管理します。

## <a name="management-api"></a>Management API
Management API を使用して、リソース グループを作成し、テンプレートからリソースをデプロイします。

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a>例 
米国東部の Azure リージョンに新しいリソース グループを作成します。

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagmentClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a>サンプル
[Azure のリソースとリソース グループを管理する](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

Azure Resource Manager のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)を参照してください。
