---
title: Managed Disks
description: マネージド ディスクの作成、サイズ変更、更新を行います。
author: lisawong19
manager: douge
ms.assetid: ''
ms.devlang: python
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 6/15/2017
ms.author: routlaw
ms.openlocfilehash: 8618b42a545e2e36ccca8944ef1dc6cf49966b00
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534415"
---
# <a name="managed-disks"></a>Managed Disks

Azure Managed Disks は、ディスク管理の簡素化、スケーラビリティの強化、セキュリティとスケールの向上を実現します。 ディスクのストレージ アカウントの概念を取り払い、顧客がストレージ アカウントに関連する制限を気にすることなくスケーリングを行えるようにします。 この記事では、Python のサービスを利用する際の概要とリファレンスを簡単に紹介します。

開発者の視点から見ると、Azure CLI の Managed Disks エクスペリエンスは、他のクロスプラットフォーム ツールの CLI と比べて独特です。 [Azure Python](https://azure.microsoft.com/develop/python/) SDK と [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) を使用して、Managed Disks を管理できます。 この[チュートリアル](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)を使用して、コンピューティング クライアントを作成できます。

## <a name="standalone-managed-disks"></a>スタンドアロンの管理ディスク

スタンドアロンの管理ディスクは、さまざまな方法で簡単に作成できます。

### <a name="create-an-empty-managed-disk"></a>空のマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>Blob Storage からマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-an-image-from-blob-storage"></a>BLOB ストレージからイメージを作成する。

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.images.create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
        'storage_profile': {
           'os_disk': {
              'os_type': 'Linux',
              'os_state': "Generalized",
              'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
              'caching': "ReadWrite",
           }
        }        
    }
)
image_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>独自のイメージからマネージド ディスクを作成する

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')
async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = async_creation.result()
```

## <a name="virtual-machine-with-managed-disks"></a>Managed Disks を使用した仮想マシン

特定のディスク イメージの暗黙的な管理ディスクを使用した仮想マシンを作成できます。 マネージド ディスクを暗黙的に作成することで、作成が簡素化されます。ディスクの詳細をすべて指定する必要はありません。 ストレージ アカウントの作成と管理についても気にする必要はありません。

管理ディスクは、Azure の OS イメージから VM を作成するときに暗黙的に作成されます。 ``storage_profile`` パラメーターでは、現在 ``os_disk`` は省略可能になっており、仮想マシン作成の必須の前提条件としてストレージ アカウントを作成する必要はありません。

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

この ``storage_profile`` パラメーターは現在有効になっています。 Python で VM を作成する方法 (ネットワークなどを含む) の完全な例を取得するには、[Python の VM チュートリアル](https://github.com/Azure-Samples/virtual-machines-python-manage)のページを参照してください。

また、独自のイメージを使用して ``storage_profile`` を作成することもできます。

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

以前にプロビジョニングした管理ディスクを簡単にアタッチできます。

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})
async_update = compute_client.virtual_machines.create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>Managed Disks を使用した仮想マシン スケール セット

管理ディスクを使用する前は、スケール セットに含めるすべての VM に対してストレージ アカウントを手動で作成し、その後、リスト パラメーター ``vhd_containers`` を使用してすべてのストレージ アカウント名をスケール セットの RestAPI に渡す必要がありました。 正式な移行ガイドについては、こちらの記事 (`<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`) を参照してください。

管理ディスクを使用するようになって、ストレージ アカウントを管理する必要はなくなりました。 VMSS Python SDK を使い慣れている場合は、``storage_profile`` は VM の作成に使用したものとまったく同じと考えてください。

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

完全なサンプル:

```python
naming_infix = "PyTestInfix"
vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    } 
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>Managed Disks を使用したその他の操作

### <a name="resizing-a-managed-disk"></a>マネージド ディスクのサイズ変更を行う

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>マネージド ディスクのストレージ アカウントの種類を更新する

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-nlob-storage"></a>BLOB ストレージからイメージを作成する

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>現在仮想マシンにアタッチされているマネージド ディスクのスナップショットを作成する

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
async_snapshot_creation = self.compute_client.snapshots.create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```
