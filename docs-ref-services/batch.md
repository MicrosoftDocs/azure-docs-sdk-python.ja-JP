---
title: Python 用 Azure Batch ライブラリ
description: Python Batch ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, Batch, 処理, スケジューリング, 長時間実行
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: fb9528c449d197440590bfc3b1991065cfe13357
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376835"
---
# <a name="azure-batch-libraries-for-python"></a><span data-ttu-id="03255-104">Python 用 Azure Batch ライブラリ</span><span class="sxs-lookup"><span data-stu-id="03255-104">Azure Batch libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="03255-105">概要</span><span class="sxs-lookup"><span data-stu-id="03255-105">Overview</span></span>

<span data-ttu-id="03255-106">[Azure Batch](/azure/batch/batch-technical-overview) を使用すると、大規模な並列コンピューティングやハイパフォーマンス コンピューティングのアプリケーションをクラウドで効率的に実行できます。</span><span class="sxs-lookup"><span data-stu-id="03255-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>

<span data-ttu-id="03255-107">Azure Batch の概要については、「[Azure Portal で Batch アカウントを作成する](/azure/batch/batch-account-create-portal)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03255-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="03255-108">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="03255-108">Install the libraries</span></span>

## <a name="client-library"></a><span data-ttu-id="03255-109">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="03255-109">Client library</span></span>
<span data-ttu-id="03255-110">Azure Batch クライアント ライブラリを使用すると、コンピューティング ノードとプールを構成したり、タスクを定義してそれらをジョブで実行するように構成したり、ジョブ マネージャーを設定してジョブの実行を制御または監視したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="03255-110">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="03255-111">これらのオブジェクトを使って大規模な並列コンピューティング ソリューションを実行する方法について詳しくは、[こちら](/azure/batch/batch-api-basics)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03255-111">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

```bash
pip install azure-batch
```
### <a name="example"></a><span data-ttu-id="03255-112">例</span><span class="sxs-lookup"><span data-stu-id="03255-112">Example</span></span>

<span data-ttu-id="03255-113">Batch アカウントで Linux コンピューティング ノードのプールを設定する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="03255-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```python
# create the batch client for an account using its URI and keys
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

## <a name="management-api"></a><span data-ttu-id="03255-114">管理 API</span><span class="sxs-lookup"><span data-stu-id="03255-114">Management API</span></span>
<span data-ttu-id="03255-115">Batch アカウントの作成と削除、Batch アカウント キーの読み取りと再生成、Batch アカウントのストレージ管理は、Azure Batch 管理ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="03255-115">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="03255-116">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="03255-116">Explore the Client APIs</span></span>](/python/api/overview/azure/batch/client)

### <a name="example"></a><span data-ttu-id="03255-117">例</span><span class="sxs-lookup"><span data-stu-id="03255-117">Example</span></span>
<span data-ttu-id="03255-118">Azure Batch アカウントを作成し、そのアカウントで使用する新しいアプリケーションと Azure ストレージ アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="03255-118">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```python
from azure.mgmt.batch import BatchManagementClient
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

LOCATION ='eastus'
GROUP_NAME ='batchresourcegroup'
STORAGE_ACCOUNT_NAME ='batchstorageaccount'

# Create Resource group
print('Create Resource Group')
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})

# Create a storage account
storage_async_operation = storage_client.storage_accounts.create(
    GROUP_NAME,
    STORAGE_ACCOUNT_NAME,
    StorageAccountCreateParameters(
        sku=Sku(SkuName.standard_ragrs),
        kind=Kind.storage,
        location=LOCATION
    )
)
storage_account = storage_async_operation.result()

# Create a Batch Account, specifying the storage account we want to link
storage_resource = storage_account.id
batch_account = azure.mgmt.batch.models.BatchAccountCreateParameters(
    location=LOCATION,
    auto_storage=azure.mgmt.batch.models.AutoStorageBaseProperties(storage_resource)
)
creating = batch_client.account.create('MyBatchAccount', LOCATION, batch_account)
creating.wait()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="03255-119">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="03255-119">Explore the Management APIs</span></span>](/python/api/overview/azure/batch/management)
