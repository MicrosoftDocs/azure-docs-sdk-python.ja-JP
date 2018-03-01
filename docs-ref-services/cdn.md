---
title: "Python 用 Azure CDN ライブラリ"
description: "Python 用 Azure CDN ライブラリのリファレンス"
keywords: Azure, python, SDK, API, CDN
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 92d6dd6ce55964bc48bb9bc654e8dec25f6b8344
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-cdn-libraries-for-python"></a>Python 用 Azure CDN ライブラリ

## <a name="overview"></a>概要

[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) を使用すると、世界中で高帯域幅の可用性を実現するための Web コンテンツ キャッシュを提供することができます。

Azure CDN を導入するには、「[Azure CDN の概要](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)」を参照してください。

## <a name="management-apis"></a>管理 API

Management API で Azure CDN を作成、照会、管理します。

pip で管理パッケージをインストールします。

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a>例

単一の定義済みエンドポイントを備えた CDN プロファイルを作成しています。

```python
from azure.mgmt.cdn import CdnManagementClient

cdn_client = CdnManagementClient(credentials, 'your-subscription-id')
profile_poller = cdn_client.profiles.create('my-resource-group',
                                            'cdn-name',
                                            {
                                                "location": "some_region", 
                                                "sku": {
                                                    "name": "sku_tier"
                                                } 
                                            })
profile = profile_poller.result()

endpoint_poller = client.endpoints.create('my-resource-group',
                                          'cdn-name',
                                          'unique-endpoint-name', 
                                          { 
                                              "location": "any_region", 
                                              "origins": [
                                                  {
                                                      "name": "origin_name", 
                                                      "host_name": "url"
                                                  }]
                                          })
endpoint = endpoint_poller.result()
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/cdn/management)
