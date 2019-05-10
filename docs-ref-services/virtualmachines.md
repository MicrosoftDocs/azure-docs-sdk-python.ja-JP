---
title: Python 用 Azure 仮想マシン ライブラリ
description: ''
keywords: Azure, Python, SDK, API, コンピューティング, 仮想マシン
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: adea3dfd1e38fb8c880009d5a02ab2b8be2a67e1
ms.sourcegitcommit: a1248376a21fc9a9441ab1fb982a477bce48f565
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2019
ms.locfileid: "29478825"
---
# <a name="azure-virtual-machine-libraries"></a>Azure 仮想マシン ライブラリ

## <a name="overview"></a>概要

Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。

Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。

## <a name="management-api"></a>管理 API

Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、管理、スケーリングを行えます。

pip を使ってライブラリをインストールします。

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a>例

管理対象のサービス ID (MSI) 認証を使って既存の Azure リソース グループに新しい Linux 仮想マシンを作成します。

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/virtualmachines/management)

## <a name="samples"></a>サンプル

* [仮想マシンを管理する][1]
* [管理対象のサービス ID で認証を行う][2]
* [管理対象のサービス ID 拡張機能で仮想マシンを作成する][3]
* [ロード バランサーを管理する][4]
* [マネージド ディスクを作成して構成する][5]
* [イメージを一覧表示する][6] 
* [仮想マシンを監視する][7]

仮想マシン サンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)をご覧ください。

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://github.com/Azure-Samples/compute-python-msi-vm
[4]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[7]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md