---
title: Python 用 Azure Data Factory ライブラリ
description: Python 用 Azure Data Factory ライブラリのリファレンス
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 05/10/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 05d93f7d1838c110c3ba77f7abd3967f7870774b
ms.sourcegitcommit: d65549030a0edb50d75482f79aac0962d1dacb57
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34759049"
---
# <a name="azure-data-factory-libraries-for-python"></a><span data-ttu-id="27185-103">Python 用 Azure Data Factory ライブラリ</span><span class="sxs-lookup"><span data-stu-id="27185-103">Azure Data Factory libraries for Python</span></span>

<span data-ttu-id="27185-104">[Azure Data Factory](/azure/data-factory/) を使用して、データの保管、移行、および処理を行うサービスを自動データ パイプラインにまとめます</span><span class="sxs-lookup"><span data-stu-id="27185-104">Compose data storage, movement, and processing services into automated data pipelines with [Azure Data Factory](/azure/data-factory/)</span></span>

<span data-ttu-id="27185-105">Data Factory の[詳細](/azure/data-factory/introduction)を確認し、[Python クイックスタートを使用して、データ ファクトリとパイプラインの作成](/azure/data-factory/quickstart-create-data-factory-python)を開始してください。</span><span class="sxs-lookup"><span data-stu-id="27185-105">[Learn more](/azure/data-factory/introduction) about Data Factory and get started with the [Create a data factory and pipeline using Python quickstart](/azure/data-factory/quickstart-create-data-factory-python).</span></span> 

## <a name="management-module"></a><span data-ttu-id="27185-106">管理モジュール</span><span class="sxs-lookup"><span data-stu-id="27185-106">Management module</span></span>

<span data-ttu-id="27185-107">管理モジュールを使用して、ご利用のサブスクリプションで Data Factory インスタンスを作成し、管理します。</span><span class="sxs-lookup"><span data-stu-id="27185-107">Create and manage Data Factory instances in your subscription with the management module.</span></span>

### <a name="installation"></a><span data-ttu-id="27185-108">インストール</span><span class="sxs-lookup"><span data-stu-id="27185-108">Installation</span></span>

<span data-ttu-id="27185-109">[pip](https://pip.pypa.io/en/stable/quickstart/) を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="27185-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a><span data-ttu-id="27185-110">例</span><span class="sxs-lookup"><span data-stu-id="27185-110">Example</span></span> 

<span data-ttu-id="27185-111">米国東部リージョンでご利用のサブスクリプション内に Data Factory を作成します。</span><span class="sxs-lookup"><span data-stu-id="27185-111">Create a Data Factory in your subscription on the East US region.</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}  

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="27185-112">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="27185-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datafactory/management)