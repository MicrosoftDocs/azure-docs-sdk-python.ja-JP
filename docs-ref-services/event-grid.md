---
title: Python 用 Azure Event Grid ライブラリ
description: ''
keywords: Azure, Python, SDK, API, Event Grid
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: e5df1078116f13f959923eac3e0c7b5789545278
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534298"
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="e4997-103">Python 用 Event Grid ライブラリ</span><span class="sxs-lookup"><span data-stu-id="e4997-103">Event Grid libraries for Python</span></span>


<span data-ttu-id="e4997-104">Azure Event Grid は、パブリッシュ/サブスクライブ モデルを使用した画一的なイベントの使用を可能にする、完全に管理されたインテリジェントなイベント ルーティング サービスです。</span><span class="sxs-lookup"><span data-stu-id="e4997-104">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

<span data-ttu-id="e4997-105">Azure Event Grid の[詳細を確認](/azure/event-grid/overview)し、[Azure Blob Storage イベントのチュートリアル](/azure/storage/blobs/storage-blob-event-quickstart)を開始してください。</span><span class="sxs-lookup"><span data-stu-id="e4997-105">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="publish-sdk"></a><span data-ttu-id="e4997-106">発行 SDK</span><span class="sxs-lookup"><span data-stu-id="e4997-106">Publish SDK</span></span>

<span data-ttu-id="e4997-107">Azure Event Grid 発行 SDK を使用して、イベントの認証、作成、処理、トピックへの発行を行います。</span><span class="sxs-lookup"><span data-stu-id="e4997-107">Authenticate, create, handle, and publish events to topics using the Azure Event Grid publish SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="e4997-108">インストール</span><span class="sxs-lookup"><span data-stu-id="e4997-108">Installation</span></span> 

<span data-ttu-id="e4997-109">[pip](https://pip.pypa.io/en/stable/quickstart/) を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e4997-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-eventgrid
```

### <a name="example"></a><span data-ttu-id="e4997-110">例</span><span class="sxs-lookup"><span data-stu-id="e4997-110">Example</span></span> 

<span data-ttu-id="e4997-111">次のコードでは、イベントをトピックに発行します。</span><span class="sxs-lookup"><span data-stu-id="e4997-111">The following code publishes an event to a topic.</span></span> <span data-ttu-id="e4997-112">トピック キーとエンドポイントは、Azure Portal または Azure CLI を使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="e4997-112">You can retrieve the topic key and endpoint through the Azure Portal or through the Azure CLI:</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

```python
from datetime import datetime
from azure.eventgrid import EventGridClient
from msrest.authentication import TopicCredentials

def publish_event(self):

        credentials = TopicCredentials(
            self.settings.EVENT_GRID_KEY
        )
        event_grid_client = EventGridClient(credentials)
        event_grid_client.publish_events(
            "your-endpoint-here",
            events=[{
                'id' : "dbf93d79-3859-4cac-8055-51e3b6b54bea",
                'subject' : "Sample subject",
                'data': {
                    'key': 'Sample Data'
                },
                'event_type': 'SampleEventType',
                'event_time': datetime(2018, 5, 2),
                'data_version': 1
            }]
        )
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e4997-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="e4997-113">Explore the Client APIs</span></span>](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="e4997-114">管理 SDK</span><span class="sxs-lookup"><span data-stu-id="e4997-114">Management SDK</span></span>

<span data-ttu-id="e4997-115">管理 SDK を使用して、Event Grid のインスタンス、トピック、サブスクリプションを作成、更新、削除します。</span><span class="sxs-lookup"><span data-stu-id="e4997-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the management SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="e4997-116">インストール</span><span class="sxs-lookup"><span data-stu-id="e4997-116">Installation</span></span> 

<span data-ttu-id="e4997-117">[pip](https://pip.pypa.io/en/stable/quickstart/) を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e4997-117">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="e4997-118">例</span><span class="sxs-lookup"><span data-stu-id="e4997-118">Example</span></span>

<span data-ttu-id="e4997-119">次のコードでは、カスタム トピックを作成し、エンドポイントをそのトピックにサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="e4997-119">The following creates a custom topic and subscribes an endpoint to the topic.</span></span> <span data-ttu-id="e4997-120">その後、HTTPS 経由でイベントをトピックに送信します。</span><span class="sxs-lookup"><span data-stu-id="e4997-120">The code then sends an event to the topic through HTTPS.</span></span>
<span data-ttu-id="e4997-121">RequestBin は、エンドポイントを作成してそこに送信された要求を確認することができるオープン ソースのサードパーティ ツールです。</span><span class="sxs-lookup"><span data-stu-id="e4997-121">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="e4997-122">[RequestBin](https://requestbin.com) にアクセスし、 **[Create a RequestBin]\(RequestBin の作成\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e4997-122">Go to [RequestBin](https://requestbin.com), and click **Create a RequestBin**.</span></span> <span data-ttu-id="e4997-123">Bin URL をコピーしてください。トピックをサブスクライブするときにこの URL が必要になります。</span><span class="sxs-lookup"><span data-stu-id="e4997-123">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

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
<span data-ttu-id="e4997-124">送信されたばかりのイベントを表示するために、前に作成した RequestBin URL の場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="e4997-124">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="e4997-125">リソースのクリーンアップ</span><span class="sxs-lookup"><span data-stu-id="e4997-125">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e4997-126">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="e4997-126">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a><span data-ttu-id="e4997-127">詳細情報</span><span class="sxs-lookup"><span data-stu-id="e4997-127">Learn more</span></span>

[<span data-ttu-id="e4997-128">Event Grid SDK を使用してイベントを受信する</span><span class="sxs-lookup"><span data-stu-id="e4997-128">Receive events using the Event Grid SDK</span></span>](/azure/event-grid/receive-events)
