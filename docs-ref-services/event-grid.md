---
title: "Python 用 Azure Event Grid ライブラリ"
description: 
keywords: Azure, Python, SDK, API, Event Grid
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: 299b50ce8366d0c49ade28dfece98d6696a4f9ef
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="3602d-103">Python 用 Event Grid ライブラリ</span><span class="sxs-lookup"><span data-stu-id="3602d-103">Event Grid libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="3602d-104">概要</span><span class="sxs-lookup"><span data-stu-id="3602d-104">Overview</span></span>
<span data-ttu-id="3602d-105">Azure Event Grid は、パブリッシュ/サブスクライブ モデルを使用した画一的なイベントの使用を可能にする、完全に管理されたインテリジェントなイベント ルーティング サービスです。</span><span class="sxs-lookup"><span data-stu-id="3602d-105">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

## <a name="management-api"></a><span data-ttu-id="3602d-106">管理 API</span><span class="sxs-lookup"><span data-stu-id="3602d-106">Management API</span></span>
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="3602d-107">例</span><span class="sxs-lookup"><span data-stu-id="3602d-107">Example</span></span>
<span data-ttu-id="3602d-108">次の例は、カスタム トピックを作成し、そのトピックにサブスクライブして、イベントをトリガーし、結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="3602d-108">The following creates a custom topic, subscribes to the topic, and triggers the event to view the result.</span></span> <span data-ttu-id="3602d-109">RequestBin は、エンドポイントを作成してそこに送信された要求を確認することができるオープン ソースのサードパーティ ツールです。</span><span class="sxs-lookup"><span data-stu-id="3602d-109">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="3602d-110">[RequestBin](https://requestb.in/) にアクセスし、**[Create a RequestBin]\(RequestBin の作成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3602d-110">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="3602d-111">Bin URL をコピーしてください。トピックをサブスクライブするときにこの URL が必要になります。</span><span class="sxs-lookup"><span data-stu-id="3602d-111">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
<span data-ttu-id="3602d-112">送信されたばかりのイベントを表示するために、前に作成した RequestBin URL の場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="3602d-112">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="3602d-113">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="3602d-113">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3602d-114">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="3602d-114">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

