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
# <a name="managed-disks"></a><span data-ttu-id="9f118-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="9f118-103">Managed Disks</span></span>

<span data-ttu-id="9f118-104">Azure Managed Disks は、ディスク管理の簡素化、スケーラビリティの強化、セキュリティとスケールの向上を実現します。</span><span class="sxs-lookup"><span data-stu-id="9f118-104">Azure Managed Disks provide a simplified disk Management, enhanced Scalability, better Security and Scale.</span></span> <span data-ttu-id="9f118-105">ディスクのストレージ アカウントの概念を取り払い、顧客がストレージ アカウントに関連する制限を気にすることなくスケーリングを行えるようにします。</span><span class="sxs-lookup"><span data-stu-id="9f118-105">It takes away the notion of storage account for disks, enabling customers to scale without worrying about the limitations associated with storage accounts.</span></span> <span data-ttu-id="9f118-106">この記事では、Python のサービスを利用する際の概要とリファレンスを簡単に紹介します。</span><span class="sxs-lookup"><span data-stu-id="9f118-106">This post provides a quick introduction and reference on consuming the service from Python.</span></span>

<span data-ttu-id="9f118-107">開発者の視点から見ると、Azure CLI の Managed Disks エクスペリエンスは、他のクロスプラットフォーム ツールの CLI と比べて独特です。</span><span class="sxs-lookup"><span data-stu-id="9f118-107">From a developer perspective, the Managed Disks experience in Azure CLI is idomatic to the CLI experience in other cross-platform tools.</span></span> <span data-ttu-id="9f118-108">[Azure Python](https://azure.microsoft.com/develop/python/) SDK と [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) を使用して、Managed Disks を管理できます。</span><span class="sxs-lookup"><span data-stu-id="9f118-108">You can use the [Azure Python](https://azure.microsoft.com/develop/python/) SDK and the [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) to administer Managed Disks.</span></span> <span data-ttu-id="9f118-109">この[チュートリアル](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)を使用して、コンピューティング クライアントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9f118-109">You can create a compute client using this [tutorial](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python).</span></span>

## <a name="standalone-managed-disks"></a><span data-ttu-id="9f118-110">スタンドアロンの管理ディスク</span><span class="sxs-lookup"><span data-stu-id="9f118-110">Standalone Managed Disks</span></span>

<span data-ttu-id="9f118-111">スタンドアロンの管理ディスクは、さまざまな方法で簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="9f118-111">You can easily create standalone Managed Disks in a variety of ways.</span></span>

### <a name="create-an-empty-managed-disk"></a><span data-ttu-id="9f118-112">空のマネージド ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="9f118-112">Create an empty Managed Disk</span></span>

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

### <a name="create-a-managed-disk-from-blob-storage"></a><span data-ttu-id="9f118-113">Blob Storage からマネージド ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="9f118-113">Create a Managed Disk from blob storage</span></span>

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

### <a name="create-an-image-from-blob-storage"></a><span data-ttu-id="9f118-114">BLOB ストレージからイメージを作成する。</span><span class="sxs-lookup"><span data-stu-id="9f118-114">Create an image from blob storage</span></span>

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

### <a name="create-a-managed-disk-from-your-own-image"></a><span data-ttu-id="9f118-115">独自のイメージからマネージド ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="9f118-115">Create a Managed Disk from your own image</span></span>

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

## <a name="virtual-machine-with-managed-disks"></a><span data-ttu-id="9f118-116">Managed Disks を使用した仮想マシン</span><span class="sxs-lookup"><span data-stu-id="9f118-116">Virtual machine with Managed Disks</span></span>

<span data-ttu-id="9f118-117">特定のディスク イメージの暗黙的な管理ディスクを使用した仮想マシンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9f118-117">You can create a Virtual Machine with an implicit Managed Disk for a specific disk image.</span></span> <span data-ttu-id="9f118-118">マネージド ディスクを暗黙的に作成することで、作成が簡素化されます。ディスクの詳細をすべて指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9f118-118">Creation is simplified with implicit creation of managed disks without specifying all the disk details.</span></span> <span data-ttu-id="9f118-119">ストレージ アカウントの作成と管理についても気にする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9f118-119">You do not have to worry about creating and managing Storage Accounts.</span></span>

<span data-ttu-id="9f118-120">管理ディスクは、Azure の OS イメージから VM を作成するときに暗黙的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="9f118-120">A Managed Disk is created implicitly when creating VM from an OS image in Azure.</span></span> <span data-ttu-id="9f118-121">``storage_profile`` パラメーターでは、現在 ``os_disk`` は省略可能になっており、仮想マシン作成の必須の前提条件としてストレージ アカウントを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9f118-121">In the ``storage_profile`` parameter, ``os_disk`` is now optional and you don't have to create a storage account as required precondition to create a Virtual Machine.</span></span>

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

<span data-ttu-id="9f118-122">この ``storage_profile`` パラメーターは現在有効になっています。</span><span class="sxs-lookup"><span data-stu-id="9f118-122">This ``storage_profile`` parameter is now valid.</span></span> <span data-ttu-id="9f118-123">Python で VM を作成する方法 (ネットワークなどを含む) の完全な例を取得するには、[Python の VM チュートリアル](https://github.com/Azure-Samples/virtual-machines-python-manage)のページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9f118-123">To get a complete example on how to create a VM in Python (including network, etc), check the full [VM tutorial in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).</span></span>

<span data-ttu-id="9f118-124">また、独自のイメージを使用して ``storage_profile`` を作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="9f118-124">You can also create a ``storage_profile`` from your own image:</span></span>

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

<span data-ttu-id="9f118-125">以前にプロビジョニングした管理ディスクを簡単にアタッチできます。</span><span class="sxs-lookup"><span data-stu-id="9f118-125">You can easily attach a previously provisioned Managed Disk.</span></span>

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

## <a name="virtual-machine-scale-sets-with-managed-disks"></a><span data-ttu-id="9f118-126">Managed Disks を使用した仮想マシン スケール セット</span><span class="sxs-lookup"><span data-stu-id="9f118-126">Virtual machine Scale Sets with Managed Disks</span></span>

<span data-ttu-id="9f118-127">管理ディスクを使用する前は、スケール セットに含めるすべての VM に対してストレージ アカウントを手動で作成し、その後、リスト パラメーター ``vhd_containers`` を使用してすべてのストレージ アカウント名をスケール セットの RestAPI に渡す必要がありました。</span><span class="sxs-lookup"><span data-stu-id="9f118-127">Before Managed Disks, you needed to create a storage account manually for all the VMs you wanted inside your Scale Set, and then use the list parameter ``vhd_containers`` to provide all the storage account name to the Scale Set RestAPI.</span></span> <span data-ttu-id="9f118-128">正式な移行ガイドについては、こちらの記事 (`<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9f118-128">The official transition guide is available in this article `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.</span></span>

<span data-ttu-id="9f118-129">管理ディスクを使用するようになって、ストレージ アカウントを管理する必要はなくなりました。</span><span class="sxs-lookup"><span data-stu-id="9f118-129">Now with Managed Disk, you don't have to manage any storage account at all.</span></span> <span data-ttu-id="9f118-130">VMSS Python SDK を使い慣れている場合は、``storage_profile`` は VM の作成に使用したものとまったく同じと考えてください。</span><span class="sxs-lookup"><span data-stu-id="9f118-130">If you're are used to the VMSS Python SDK, your ``storage_profile`` can now be exactly the same as the one used in VM creation:</span></span>

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

<span data-ttu-id="9f118-131">完全なサンプル:</span><span class="sxs-lookup"><span data-stu-id="9f118-131">The full sample being:</span></span>

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

## <a name="other-operations-with-managed-disks"></a><span data-ttu-id="9f118-132">Managed Disks を使用したその他の操作</span><span class="sxs-lookup"><span data-stu-id="9f118-132">Other operations with Managed Disks</span></span>

### <a name="resizing-a-managed-disk"></a><span data-ttu-id="9f118-133">マネージド ディスクのサイズ変更を行う</span><span class="sxs-lookup"><span data-stu-id="9f118-133">Resizing a Managed Disk</span></span>

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

### <a name="update-the-storage-account-type-of-the-managed-disks"></a><span data-ttu-id="9f118-134">マネージド ディスクのストレージ アカウントの種類を更新する</span><span class="sxs-lookup"><span data-stu-id="9f118-134">Update the storage account type of the Managed Disks</span></span>

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

### <a name="create-an-image-from-nlob-storage"></a><span data-ttu-id="9f118-135">BLOB ストレージからイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="9f118-135">Create an image from nlob storage</span></span>

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

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a><span data-ttu-id="9f118-136">現在仮想マシンにアタッチされているマネージド ディスクのスナップショットを作成する</span><span class="sxs-lookup"><span data-stu-id="9f118-136">Create a snapshot of a Managed Disk that is currently attached to a virtual machine</span></span>

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
