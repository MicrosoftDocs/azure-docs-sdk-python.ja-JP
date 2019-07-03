---
title: Python 用 Azure リソース ライブラリ
description: Python 用 Azure リソース ライブラリのリファレンス
keywords: Azure, Python, SDK, API, リソース
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: cef1f2bad7dcb3ff73aeae9c56000fb949541df9
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534223"
---
# <a name="azure-resources-libraries-for-python"></a>Python 用 Azure リソース ライブラリ

## <a name="overview"></a>概要 
[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) を使用して、グループ内のリソースをデプロイ、監視、管理します。

## <a name="management-api"></a>管理 API
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

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a>サンプル
[Azure のリソースとリソース グループを管理する](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

Azure Resource Manager のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=resource)を参照してください。
