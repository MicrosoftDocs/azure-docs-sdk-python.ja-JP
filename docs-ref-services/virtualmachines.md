---
title: "Python 用 Azure 仮想マシン ライブラリ"
description: 
keywords: "Azure, Python, SDK, API, コンピューティング, 仮想マシン"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: c25665e19adb44c7112bf1533097ce1e6c739cb8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="111ca-103">Azure 仮想マシン ライブラリ</span><span class="sxs-lookup"><span data-stu-id="111ca-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="111ca-104">概要</span><span class="sxs-lookup"><span data-stu-id="111ca-104">Overview</span></span>

<span data-ttu-id="111ca-105">Linux または Windows を実行するオンデマンドのスケーラブルなコンピューティング リソースです。</span><span class="sxs-lookup"><span data-stu-id="111ca-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="111ca-106">Azure 仮想マシンの概要については、「[Azure Portal で Linux 仮想マシンを作成する](/azure/virtual-machines/linux/quick-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="111ca-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="111ca-107">Management API</span><span class="sxs-lookup"><span data-stu-id="111ca-107">Management API</span></span>

<span data-ttu-id="111ca-108">Azure の Windows 仮想マシンと Linux 仮想マシンは、コードから Management API を使って作成、構成、管理、スケーリングを行えます。</span><span class="sxs-lookup"><span data-stu-id="111ca-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="111ca-109">pip を使ってライブラリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="111ca-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a><span data-ttu-id="111ca-110">例</span><span class="sxs-lookup"><span data-stu-id="111ca-110">Example</span></span>

<span data-ttu-id="111ca-111">既存の Azure リソース グループに新しい Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="111ca-111">Create a new Linux virtual machine in an existing Azure resource group.</span></span>

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
> [<span data-ttu-id="111ca-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="111ca-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a><span data-ttu-id="111ca-113">サンプル</span><span class="sxs-lookup"><span data-stu-id="111ca-113">Samples</span></span>

* <span data-ttu-id="111ca-114">[仮想マシンを管理する][1]</span><span class="sxs-lookup"><span data-stu-id="111ca-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="111ca-115">[ロード バランサーを管理する][2]</span><span class="sxs-lookup"><span data-stu-id="111ca-115">[Manage a load balancer][2]</span></span>
* <span data-ttu-id="111ca-116">[管理ディスクを作成して構成する][3]</span><span class="sxs-lookup"><span data-stu-id="111ca-116">[Create and configure managed disks][3]</span></span>
* <span data-ttu-id="111ca-117">[イメージを一覧表示する][4]</span><span class="sxs-lookup"><span data-stu-id="111ca-117">[List images][4]</span></span> 
* <span data-ttu-id="111ca-118">[仮想マシンを監視する][5]</span><span class="sxs-lookup"><span data-stu-id="111ca-118">[Monitor virtual machines][5]</span></span>

<span data-ttu-id="111ca-119">仮想マシン サンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="111ca-119">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[3]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[4]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md