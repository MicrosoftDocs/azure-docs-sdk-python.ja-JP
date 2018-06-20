---
title: Python 用 Service Bus ライブラリ
description: Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 6c0bc66fbe8194b5b8f34ee8e29945b03ba8c242
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551595"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="b1f66-104">Python 用 Service Bus ライブラリ</span><span class="sxs-lookup"><span data-stu-id="b1f66-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="b1f66-105">概要</span><span class="sxs-lookup"><span data-stu-id="b1f66-105">Overview</span></span>

<span data-ttu-id="b1f66-106">Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="b1f66-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="b1f66-107">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="b1f66-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a><span data-ttu-id="b1f66-108">Service Bus キュー</span><span class="sxs-lookup"><span data-stu-id="b1f66-108">ServiceBus Queues</span></span>
<span data-ttu-id="b1f66-109">プッシュ型配信 (ロング ポーリング) を使用した高度なメッセージング機能が要求されるシナリオでは、ストレージ キューよりも Service Bus キューの方が適している場合があります。より大きなメッセージ サイズへの対応、メッセージの順序付け、単一操作での破壊的な読み取り、スケジュール指定配信は、そのようなシナリオの例です。</span><span class="sxs-lookup"><span data-stu-id="b1f66-109">ServiceBus Queues are an alternative to Storage Queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

<span data-ttu-id="b1f66-110">このサービスでは、Shared Access Signature 認証または ACS 認証を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-110">The service can use Shared Access Signature authentication, or ACS authentication.</span></span>

<span data-ttu-id="b1f66-111">2014 年 8 月より後に Azure Portal を使用して作成された Service Bus 名前空間では、ACS 認証がサポートされません。</span><span class="sxs-lookup"><span data-stu-id="b1f66-111">Service bus namespaces created using the Azure portal after August 2014 no longer support ACS authentication.</span></span> <span data-ttu-id="b1f66-112">ACS に対応した名前空間は、Azure SDK で作成してください。</span><span class="sxs-lookup"><span data-stu-id="b1f66-112">You can create ACS compatible namespaces with the Azure SDK.</span></span>

### <a name="shared-access-signature-authentication"></a><span data-ttu-id="b1f66-113">Shared Access Signature 認証</span><span class="sxs-lookup"><span data-stu-id="b1f66-113">Shared Access Signature Authentication</span></span>

<span data-ttu-id="b1f66-114">Shared Access Signature 認証を使用するには、次のコードで Service Bus サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b1f66-114">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a><span data-ttu-id="b1f66-115">ACS 認証</span><span class="sxs-lookup"><span data-stu-id="b1f66-115">ACS Authentication</span></span>

<span data-ttu-id="b1f66-116">ACS 認証を使用するには、次のコードで Service Bus サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="b1f66-116">To use ACS authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a><span data-ttu-id="b1f66-117">メッセージの送受信</span><span class="sxs-lookup"><span data-stu-id="b1f66-117">Sending and Receiving Messages</span></span>

<span data-ttu-id="b1f66-118">キューは、**create\_queue** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-118">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="b1f66-119">そのキューにメッセージを挿入するには、**send\_queue\_message** メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b1f66-119">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="b1f66-120">**send\_queue\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-120">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="b1f66-121">その後、**receive\_queue\_message** メソッドを呼び出すことで、メッセージをデキューすることができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-121">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a><span data-ttu-id="b1f66-122">Service Bus トピック</span><span class="sxs-lookup"><span data-stu-id="b1f66-122">ServiceBus Topics</span></span>

<span data-ttu-id="b1f66-123">Service Bus トピックは、Service Bus キュー上の発行/サブスクライブのシナリオを実装しやすくする抽象概念です。</span><span class="sxs-lookup"><span data-stu-id="b1f66-123">ServiceBus topics are an abstraction on top of ServiceBus Queues that make pub/sub scenarios easy to implement.</span></span>

<span data-ttu-id="b1f66-124">サーバー側のトピックは、**create\_topic** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-124">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="b1f66-125">トピックには、**send\_topic\_message** メソッドを使用してメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-125">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="b1f66-126">**send\_topic\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-126">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="b1f66-127">Python 3 では文字列型のメッセージが UTF-8 でエンコーディングされますが、Python 2 ではエンコーディングを独自に行う必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b1f66-127">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="b1f66-128">その後クライアントは、**create\_subscription** メソッドを呼び出した後、**receive\_subscription\_message** メソッドを呼び出すことで、サブスクリプションを作成し、メッセージを取り出すことができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-128">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="b1f66-129">サブスクリプションが作成される前に送信されたメッセージは受信されないので注意してください。</span><span class="sxs-lookup"><span data-stu-id="b1f66-129">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a><span data-ttu-id="b1f66-130">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="b1f66-130">Event Hub</span></span>

<span data-ttu-id="b1f66-131">Event Hubs を使用すると、デバイスとサービスの多様なセットから高速でイベント ストリームを収集できます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-131">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="b1f66-132">イベント ハブは、**create\_event\_hub** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-132">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="b1f66-133">イベントを送信するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="b1f66-133">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="b1f66-134">イベントの内容は、複数のメッセージを含んだ JSON エンコード文字列またはイベント メッセージです。</span><span class="sxs-lookup"><span data-stu-id="b1f66-134">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

## <a name="advanced-features"></a><span data-ttu-id="b1f66-135">高度な機能</span><span class="sxs-lookup"><span data-stu-id="b1f66-135">Advanced features</span></span>

### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="b1f66-136">ブローカーのプロパティとユーザーのプロパティ</span><span class="sxs-lookup"><span data-stu-id="b1f66-136">Broker Properties and User Properties</span></span>

<span data-ttu-id="b1f66-137">ここでは、[こちら](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)に定義されているブローカーのプロパティとユーザーのプロパティの使い方について説明します。</span><span class="sxs-lookup"><span data-stu-id="b1f66-137">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
<span data-ttu-id="b1f66-138">datetime、int、float、boolean を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="b1f66-138">You can use datetime, int, float or boolean</span></span>

```python
props = {'hello': 'world',
            'number': 42,
            'active': True,
            'deceased': False,
            'large': 8555111000,
            'floating': 3.14,
            'dob': datetime(2011, 12, 14),
            'double_quote_message': 'This "should" work fine',
            'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```
<span data-ttu-id="b1f66-139">このライブラリの旧バージョンとの互換性を確保するために、`broker_properties` を JSON 文字列として定義してもかまいません。</span><span class="sxs-lookup"><span data-stu-id="b1f66-139">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="b1f66-140">その場合は、有効な JSON 文字列をご自身で確実に記述してください。RestAPI を送信する前に、Python によるチェックは実行されません。</span><span class="sxs-lookup"><span data-stu-id="b1f66-140">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="b1f66-141">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="b1f66-141">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/management)