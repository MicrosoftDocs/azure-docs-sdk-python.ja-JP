---
title: Python 用 Azure CDN ライブラリ
description: Python 用 Azure CDN ライブラリのリファレンス
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
ms.openlocfilehash: 06e6c8786ebbd88b7d3996b640af96a23cd5689b
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275536"
---
# <a name="azure-cdn-libraries-for-python"></a><span data-ttu-id="1d3f5-104">Python 用 Azure CDN ライブラリ</span><span class="sxs-lookup"><span data-stu-id="1d3f5-104">Azure CDN libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="1d3f5-105">概要</span><span class="sxs-lookup"><span data-stu-id="1d3f5-105">Overview</span></span>

<span data-ttu-id="1d3f5-106">[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) を使用すると、世界中で高帯域幅の可用性を実現するための Web コンテンツ キャッシュを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="1d3f5-106">[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) allows you to provide web content caches to ensure high-bandwidth availability across the globe.</span></span>

<span data-ttu-id="1d3f5-107">Azure CDN を導入するには、「[Azure CDN の概要](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d3f5-107">To get started with Azure CDN, see [Getting started with Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-apis"></a><span data-ttu-id="1d3f5-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="1d3f5-108">Management APIs</span></span>

<span data-ttu-id="1d3f5-109">Management API で Azure CDN を作成、照会、管理します。</span><span class="sxs-lookup"><span data-stu-id="1d3f5-109">Create, query, and manage Azure CDNs with the management API.</span></span>

<span data-ttu-id="1d3f5-110">pip で管理パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1d3f5-110">Install the management package via pip.</span></span>

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a><span data-ttu-id="1d3f5-111">例</span><span class="sxs-lookup"><span data-stu-id="1d3f5-111">Example</span></span>

<span data-ttu-id="1d3f5-112">単一の定義済みエンドポイントを備えた CDN プロファイルを作成しています。</span><span class="sxs-lookup"><span data-stu-id="1d3f5-112">Creating a CDN profile with a single defined endpoint:</span></span>

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
> [<span data-ttu-id="1d3f5-113">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="1d3f5-113">Explore the Management APIs</span></span>](/python/api/overview/azure/cdn/management)
