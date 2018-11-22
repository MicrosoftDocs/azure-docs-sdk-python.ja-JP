---
title: Python 用 Azure ネットワーク ライブラリ
description: Python 用 Azure ネットワーク ライブラリのリファレンス
keywords: Azure, Python, SDK, API, ネットワーク
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f468f844becc2f0fe07d892c725c00df4669f4e3
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273028"
---
# <a name="azure-network-libraries-for-python"></a><span data-ttu-id="38806-104">Python 用 Azure ネットワーク ライブラリ</span><span class="sxs-lookup"><span data-stu-id="38806-104">Azure Network libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="38806-105">概要</span><span class="sxs-lookup"><span data-stu-id="38806-105">Overview</span></span>

<span data-ttu-id="38806-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) を使用して、Azure リソースに接続できます。また、それらのリソースをオンプレミスのネットワークに接続することもできます。</span><span class="sxs-lookup"><span data-stu-id="38806-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) allows you to connect Azure resources, and also connect them to your on-premises network.</span></span>

<span data-ttu-id="38806-107">Azure Virtual Network の概要については、「[最初の仮想ネットワークの作成](/azure/virtual-network/virtual-network-get-started-vnet-subnet)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="38806-107">To get started with Azure Virtual Network, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-apis"></a><span data-ttu-id="38806-108">管理 API</span><span class="sxs-lookup"><span data-stu-id="38806-108">Management APIs</span></span>

<span data-ttu-id="38806-109">Management API を使用して、Azure 仮想ネットワークを検査、管理、構成します。</span><span class="sxs-lookup"><span data-stu-id="38806-109">Inspect, manage, and configure Azure virtual networks with the management APIs.</span></span>

<span data-ttu-id="38806-110">他の Azure Python API とは異なり、ネットワーク API は個別のパッケージに明示的にバージョン管理されています。</span><span class="sxs-lookup"><span data-stu-id="38806-110">Unlike other Azure python APIs, the networking APIs are explicitly versioned into separage packages.</span></span> <span data-ttu-id="38806-111">パッケージ情報はクライアント コンストラクターで指定されているため、これらのパッケージを個別にインポートする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="38806-111">You do not need to import them individually since the package information is specified in the client constructor.</span></span>

<span data-ttu-id="38806-112">pip を使用して管理パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="38806-112">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-network
```

### <a name="example"></a><span data-ttu-id="38806-113">例</span><span class="sxs-lookup"><span data-stu-id="38806-113">Example</span></span>

<span data-ttu-id="38806-114">仮想ネットワークと関連するサブネットを作成します。</span><span class="sxs-lookup"><span data-stu-id="38806-114">Create a virtual network and an associated subnet.</span></span>

```python
from azure.mgmt.network import NetworkManagementClient

GROUP_NAME = 'resource-group'
VNET_NAME = 'your-vnet-identifier'
LOCATION = 'region'
SUBNET_NAME = 'your-subnet-identifier'

network_client = NetworkManagementClient(credentials, 'your-subscription-id')

async_vnet_creation = network_client.virtual_networks.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    {
        'location': LOCATION,
        'address_space': {
            'address_prefixes': ['10.0.0.0/16']
        }
    }
)
async_vnet_creation.wait()

# Create Subnet
async_subnet_creation = network_client.subnets.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    SUBNET_NAME,
    {'address_prefix': '10.0.0.0/24'}
)
subnet_info = async_subnet_creation.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="38806-115">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="38806-115">Explore the Management APIs</span></span>](/python/api/overview/azure/network/management)

### <a name="samples"></a><span data-ttu-id="38806-116">サンプル</span><span class="sxs-lookup"><span data-stu-id="38806-116">Samples</span></span>

* [<span data-ttu-id="38806-117">Python でのロード バランサー用の Azure Resource Manager の概要</span><span class="sxs-lookup"><span data-stu-id="38806-117">Getting started with Azure Resource Manager for load balancers in Python</span></span>](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

<span data-ttu-id="38806-118">Azure Virtual Network のサンプルの[完全な一覧](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)を表示します。</span><span class="sxs-lookup"><span data-stu-id="38806-118">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) of Azure Virtual Network samples.</span></span>
