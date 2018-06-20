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
ms.openlocfilehash: 47252ca3b2f5c6087277bac3735025f0dbabbdd8
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479075"
---
# <a name="azure-network-libraries-for-python"></a>Python 用 Azure ネットワーク ライブラリ

## <a name="overview"></a>概要

[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) を使用して、Azure リソースに接続できます。また、それらのリソースをオンプレミスのネットワークに接続することもできます。

Azure Virtual Network の概要については、「[最初の仮想ネットワークの作成](/azure/virtual-network/virtual-network-get-started-vnet-subnet)」を参照してください。

## <a name="management-apis"></a>管理 API

Management API を使用して、Azure 仮想ネットワークを検査、管理、構成します。

他の Azure Python API とは異なり、ネットワーク API は個別のパッケージに明示的にバージョン管理されています。 パッケージ情報はクライアント コンストラクターで指定されているため、これらのパッケージを個別にインポートする必要はありません。

pip を使用して管理パッケージをインストールします。

```bash
pip install azure-mgmt-network
```

### <a name="example"></a>例

仮想ネットワークと関連するサブネットを作成します。

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
> [Management API を探す](/python/api/overview/azure/network/management)

### <a name="samples"></a>サンプル

* [Python でのロード バランサー用の Azure Resource Manager の概要](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

Azure Virtual Network のサンプルの[完全な一覧](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network)を表示します。
