---
title: Python 用 Service Bus ライブラリ
description: Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント
keywords: Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 540a2a248f7a2abcc83517bc4a4ef96ef03e8a9f
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747742"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="5680a-104">Python 用 Service Bus ライブラリ</span><span class="sxs-lookup"><span data-stu-id="5680a-104">Service Bus libraries for Python</span></span>

<span data-ttu-id="5680a-105">Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="5680a-105">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span>

* [<span data-ttu-id="5680a-106">SDK のソース コード</span><span class="sxs-lookup"><span data-stu-id="5680a-106">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="5680a-107">SDK のリファレンス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="5680a-107">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a><span data-ttu-id="5680a-108">v0.50.0 の新機能</span><span class="sxs-lookup"><span data-stu-id="5680a-108">What's new in v0.50.0?</span></span>
<span data-ttu-id="5680a-109">バージョン 0.50.0 では、メッセージの送受信に、新しい AMQP ベースの API を使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5680a-109">As of version 0.50.0 a new AMQP-based API is available for sending and receiving messages.</span></span> <span data-ttu-id="5680a-110">この更新には、**破壊的変更**が含まれています。</span><span class="sxs-lookup"><span data-stu-id="5680a-110">This update involves **breaking changes**.</span></span>
<span data-ttu-id="5680a-111">現時点でのアップグレードが適切であるかどうかを判断するには、「[v0.21.1 から v0.50.0 への移行](#migration-from-v0211-to-v0500)」をお読みください。</span><span class="sxs-lookup"><span data-stu-id="5680a-111">Please read [Migration from v0.21.1 to v0.50.0](#migration-from-v0211-to-v0500) to determine if upgrading is right for you at this time.</span></span>

<span data-ttu-id="5680a-112">新しい AMQP ベースの API では、メッセージの受け渡しの信頼性やパフォーマンスが向上し、今後の機能サポートも強化されます。</span><span class="sxs-lookup"><span data-stu-id="5680a-112">The new AMQP-based API offers improved message passing reliability, performance and expanded feature support going forward.</span></span>
<span data-ttu-id="5680a-113">また、新しい API では、メッセージの送受信や処理に対する非同期操作 (asyncio に基づく) もサポートされます。</span><span class="sxs-lookup"><span data-stu-id="5680a-113">The new API also offers support for asynchronous operations (based on asyncio) for sending, receiving and handling messages.</span></span>

<span data-ttu-id="5680a-114">従来の HTTP ベースの操作については、「[HTTP ベースの操作による従来の API の使用](#using-http-based-operations-of-the-legacy-api)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5680a-114">For documentation on the legacy HTTP-based operations please see [Using HTTP-based operations of the legacy API](#using-http-based-operations-of-the-legacy-api).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="5680a-115">前提条件</span><span class="sxs-lookup"><span data-stu-id="5680a-115">Prerequisites</span></span>

* <span data-ttu-id="5680a-116">Azure サブスクリプション - [無料アカウント](https://azure.microsoft.com/free/)を作成できます</span><span class="sxs-lookup"><span data-stu-id="5680a-116">Azure subscription - [Create a free account](https://azure.microsoft.com/free/)</span></span>
* <span data-ttu-id="5680a-117">Azure [Service Bus 名前空間と管理資格情報](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span><span class="sxs-lookup"><span data-stu-id="5680a-117">Azure [Service Bus namespace and management credentials](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span></span>
* [<span data-ttu-id="5680a-118">Python 2.7 から 3.7</span><span class="sxs-lookup"><span data-stu-id="5680a-118">Python 2.7-3.7</span></span>](https://www.python.org/downloads/)


## <a name="installation"></a><span data-ttu-id="5680a-119">インストール</span><span class="sxs-lookup"><span data-stu-id="5680a-119">Installation</span></span>
```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a><span data-ttu-id="5680a-120">Azure Service Bus に接続する</span><span class="sxs-lookup"><span data-stu-id="5680a-120">Connect to Azure Service Bus</span></span>

### <a name="get-credentials"></a><span data-ttu-id="5680a-121">資格情報の取得</span><span class="sxs-lookup"><span data-stu-id="5680a-121">Get credentials</span></span>
<span data-ttu-id="5680a-122">次の Azure CLI スニペットを使用して、環境変数に Service Bus 接続文字列を設定します (この値は、Azure portal で見つけることもできます)。</span><span class="sxs-lookup"><span data-stu-id="5680a-122">Use the Azure CLI snippet below to populate an environment variable with the Service Bus connection string (you can also find this value in the Azure portal).</span></span> <span data-ttu-id="5680a-123">このスニペットは Bash シェル用にフォーマットされています。</span><span class="sxs-lookup"><span data-stu-id="5680a-123">The snippet is formatted for the Bash shell.</span></span>

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a><span data-ttu-id="5680a-124">クライアントの作成</span><span class="sxs-lookup"><span data-stu-id="5680a-124">Create client</span></span>
<span data-ttu-id="5680a-125">`SB_CONN_STR` 環境変数を設定したら、ServiceBusClient を作成できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-125">Once you've populated the `SB_CONN_STR` environment variable, you can create the ServiceBusClient.</span></span>
```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```
<span data-ttu-id="5680a-126">非同期操作を使用する場合は、`azure.servicebus.aio` 名前空間を使用してください。</span><span class="sxs-lookup"><span data-stu-id="5680a-126">If you wish to use asynchronous operations, please use the `azure.servicebus.aio` namespace.</span></span>
```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```


## <a name="service-bus-queues"></a><span data-ttu-id="5680a-127">Service Bus キュー</span><span class="sxs-lookup"><span data-stu-id="5680a-127">Service Bus queues</span></span>
<span data-ttu-id="5680a-128">プッシュ型配信 (ロング ポーリング) を使用した高度なメッセージング機能が要求されるシナリオでは、Storage キューよりも Service Bus キューの方が適している場合があります。より大きなメッセージ サイズへの対応、メッセージの順序付け、単一操作での破壊的な読み取り、スケジュール指定配信は、そのような機能の例です。</span><span class="sxs-lookup"><span data-stu-id="5680a-128">Service Bus queues are an alternative to Storage queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

### <a name="create-queue"></a><span data-ttu-id="5680a-129">キューの作成</span><span class="sxs-lookup"><span data-stu-id="5680a-129">Create queue</span></span>
<span data-ttu-id="5680a-130">これにより、Service Bus 名前空間内に新しいキューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5680a-130">This creates a new queue within the Service Bus namespace.</span></span> <span data-ttu-id="5680a-131">同じ名前のキューが既に名前空間内に存在する場合は、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5680a-131">If a queue of the same name already exists within the namespace an error will be raised.</span></span> 
```python
sb_client.create_queue("MyQueue")
```
<span data-ttu-id="5680a-132">省略可能なパラメーターを指定して、キューの動作を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="5680a-132">Optional parameters to configure the queue behaviour can also be specified.</span></span>
```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a><span data-ttu-id="5680a-133">キュー クライアントの取得</span><span class="sxs-lookup"><span data-stu-id="5680a-133">Get a queue client</span></span>
<span data-ttu-id="5680a-134">キューとの間でのメッセージの送受信や、その他の操作を実行するには、QueueClient を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-134">A QueueClient can be used to send and receive messages from the queue, along with other operations.</span></span>
```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a><span data-ttu-id="5680a-135">メッセージの送信</span><span class="sxs-lookup"><span data-stu-id="5680a-135">Sending messages</span></span>
<span data-ttu-id="5680a-136">キュー クライアントでは、一度に 1 つ以上のメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-136">The queue client can send one or more messages at a time:</span></span>
```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```
<span data-ttu-id="5680a-137">QueueClient.send に対する呼び出しごとに、新しいサービス接続が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5680a-137">Each call to QueueClient.send will create a new service connection.</span></span> <span data-ttu-id="5680a-138">複数の送信呼び出しに同じ接続を再利用するために、送信側を開くことができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-138">To reuse the same connection for multiple send calls, you can open a sender:</span></span>
```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```
<span data-ttu-id="5680a-139">非同期クライアントを使用している場合は、上述の操作で非同期の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="5680a-139">If you are using an asynchronous client, the above operations will use async syntax:</span></span>
```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```


### <a name="receiving-messages"></a><span data-ttu-id="5680a-140">メッセージの受信</span><span class="sxs-lookup"><span data-stu-id="5680a-140">Receiving messages</span></span>
<span data-ttu-id="5680a-141">メッセージは、継続的な反復子としてキューから受信できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-141">Messages can be received from a queue as a continuous iterator.</span></span> <span data-ttu-id="5680a-142">メッセージ受信の既定のモードは [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read) です。この場合、メッセージがキューから削除されるように、各メッセージを明示的に完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5680a-142">The default mode for message receiving is [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), which requires each message to be explicitly completed in order that it be removed from the queue.</span></span>
```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```
<span data-ttu-id="5680a-143">サービス接続は、反復子全体にわたって開かれたままになります。</span><span class="sxs-lookup"><span data-stu-id="5680a-143">The service connection will remain open for the entirety of the iterator.</span></span>
<span data-ttu-id="5680a-144">メッセージ ストリームが部分的に反復処理されている場合は、`with` ステートメントで受信側を実行して、接続を確実に閉じる必要があります。</span><span class="sxs-lookup"><span data-stu-id="5680a-144">If you find yourself only partially iterating the message stream, you should run the receiver in a `with` statement to ensure the connection is closed:</span></span>
```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
<span data-ttu-id="5680a-145">非同期クライアントを使用している場合は、上述の操作で非同期の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="5680a-145">If you are using an asynchronous client, the above operations will use async syntax:</span></span>
```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a><span data-ttu-id="5680a-146">Service Bus トピックとサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="5680a-146">Service Bus topics and subscriptions</span></span>

<span data-ttu-id="5680a-147">Service Bus のトピックとサブスクリプションは、発行とサブスクライブのパターンで一対多形式の通信を提供する Service Bus キュー上の抽象化です。</span><span class="sxs-lookup"><span data-stu-id="5680a-147">Service Bus topics and subscriptions are an abstraction on top of Service Bus queues that provide a one-to-many form of communication, in a publish/subscribe pattern.</span></span> <span data-ttu-id="5680a-148">メッセージはトピックに送信された後、関連付けられている 1 つ以上のサブスクリプションに配信されますが、この方法は多数の受信者にスケーリングする場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="5680a-148">Messages are sent to a topic and delivered to one or more associated subscriptions, which is useful for scaling to large numbers of recipients.</span></span>

### <a name="create-topic"></a><span data-ttu-id="5680a-149">トピックの作成</span><span class="sxs-lookup"><span data-stu-id="5680a-149">Create topic</span></span>
<span data-ttu-id="5680a-150">これにより、Service Bus 名前空間内に新しいトピックが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5680a-150">This creates a new topic within the Service Bus namespace.</span></span> <span data-ttu-id="5680a-151">同じ名前のトピックが既に存在する場合は、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="5680a-151">If a topic of the same name already exists an error will be raised.</span></span> 
```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a><span data-ttu-id="5680a-152">トピック クライアントの取得</span><span class="sxs-lookup"><span data-stu-id="5680a-152">Get a topic client</span></span>
<span data-ttu-id="5680a-153">トピックへのメッセージの送信や、その他の操作を実行するには、TopicClient を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-153">A TopicClient can be used to send messages to the topic, along with other operations.</span></span>
```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a><span data-ttu-id="5680a-154">サブスクリプションの作成</span><span class="sxs-lookup"><span data-stu-id="5680a-154">Create subscription</span></span>
<span data-ttu-id="5680a-155">これにより、Service Bus 名前空間内に、指定したトピックに対する新しいサブスクリプションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5680a-155">This creates a new subscription for the specified topic within the Service Bus namespace.</span></span>
```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a><span data-ttu-id="5680a-156">サブスクリプション クライアントの取得</span><span class="sxs-lookup"><span data-stu-id="5680a-156">Get a subscription client</span></span>
<span data-ttu-id="5680a-157">トピックからのメッセージの受信や、その他の操作を実行するには、SubscriptionClient を使用できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-157">A SubscriptionClient can be used to receive messages from the topic, along with other operations.</span></span>
```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a><span data-ttu-id="5680a-158">v0.21.1 から v0.50.0 への移行</span><span class="sxs-lookup"><span data-stu-id="5680a-158">Migration from v0.21.1 to v0.50.0</span></span>
<span data-ttu-id="5680a-159">バージョン 0.50.0 には、重要な破壊的変更が導入されました。</span><span class="sxs-lookup"><span data-stu-id="5680a-159">Major breaking changes were introduced in version 0.50.0.</span></span>
<span data-ttu-id="5680a-160">元の HTTP ベースの API はv0.50.0 でも引き続き使用できますが、新しい名前空間 `azure.servicebus.control_client` に存在します。</span><span class="sxs-lookup"><span data-stu-id="5680a-160">The original HTTP-based API is still available in v0.50.0 - however it now exists under a new namesapce: `azure.servicebus.control_client`.</span></span>

### <a name="should-i-upgrade"></a><span data-ttu-id="5680a-161">アップグレードの必要性について</span><span class="sxs-lookup"><span data-stu-id="5680a-161">Should I upgrade?</span></span>
<span data-ttu-id="5680a-162">新しいパッケージ (v0.50.0) を v0.21.1 と比較した場合、HTTP ベースの操作に関しては改善点はありません。</span><span class="sxs-lookup"><span data-stu-id="5680a-162">The new package (v0.50.0) offers no improvements in HTTP-based operations over v0.21.1.</span></span> <span data-ttu-id="5680a-163">HTTP ベースの API は、新しい名前空間に存在するという点を除いては同じです。</span><span class="sxs-lookup"><span data-stu-id="5680a-163">The HTTP-based API is identical except that it now exists under a new namespace.</span></span> <span data-ttu-id="5680a-164">このため、HTTP ベースの操作 (`create_queue`、`delete_queue` など) のみを使用する場合は、現時点でアップグレードを行うメリットはありません。</span><span class="sxs-lookup"><span data-stu-id="5680a-164">For this reason if you only wish to use HTTP-based operations (`create_queue`, `delete_queue` etc) - there will be no additional benefit in upgrading at this time.</span></span>


### <a name="how-do-i-migrate-my-code-to-the-new-version"></a><span data-ttu-id="5680a-165">コードを新しいバージョンに移行する方法</span><span class="sxs-lookup"><span data-stu-id="5680a-165">How do I migrate my code to the new version?</span></span>
<span data-ttu-id="5680a-166">v0.21.0 に対して作成したコードは、次のようにインポートの名前空間を変更するだけで、バージョン 0.50.0 に移植できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-166">Code written against v0.21.0 can be ported to version 0.50.0 by simply changing the import namespace:</span></span>

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a><span data-ttu-id="5680a-167">HTTP ベースの操作による従来の API の使用</span><span class="sxs-lookup"><span data-stu-id="5680a-167">Using HTTP-based operations of the legacy API</span></span>
<span data-ttu-id="5680a-168">以下の説明は従来の API についてのもので、追加変更を加えずに既存のコードを v0.50.0 に移植することを希望するユーザーを対象としています。</span><span class="sxs-lookup"><span data-stu-id="5680a-168">The following documentation describes the legacy API and should be used for those wishing to port existing code to v0.50.0 without making any additional changes.</span></span> <span data-ttu-id="5680a-169">このリファレンスは、v0.21.1 を使用しているユーザーもガイダンスとしても使用できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-169">This reference can also be used as guidance by those using v0.21.1.</span></span>
<span data-ttu-id="5680a-170">新しいコードを作成するユーザーについては、上で説明した新しい API の使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5680a-170">For those writing new code, we recommend using the new API described above.</span></span>

### <a name="service-bus-queues"></a><span data-ttu-id="5680a-171">Service Bus キュー</span><span class="sxs-lookup"><span data-stu-id="5680a-171">Service Bus queues</span></span>

#### <a name="shared-access-signature-sas-authentication"></a><span data-ttu-id="5680a-172">Shared Access Signature (SAS) 認証</span><span class="sxs-lookup"><span data-stu-id="5680a-172">Shared Access Signature (SAS) authentication</span></span>

<span data-ttu-id="5680a-173">Shared Access Signature 認証を使用するには、次のコードで Service Bus サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5680a-173">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a><span data-ttu-id="5680a-174">Azure Access Control Service (ACS) 認証</span><span class="sxs-lookup"><span data-stu-id="5680a-174">Access Control Service (ACS) authentication</span></span>
<span data-ttu-id="5680a-175">新しい Service Bus 名前空間では、ACS がサポートされなくなりました。</span><span class="sxs-lookup"><span data-stu-id="5680a-175">ACS is no longer supported on new Service Bus namespaces.</span></span> <span data-ttu-id="5680a-176">[アプリケーションを SAS 認証に移行する](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas)ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5680a-176">We recommend [migrating applications to SAS authentication](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span></span>
<span data-ttu-id="5680a-177">以前の Service Bus 名前空間内で ACS 認証を使用するには、以下を使用して ServiceBusService を作成します。</span><span class="sxs-lookup"><span data-stu-id="5680a-177">To use ACS authentication within an older Service Bus namesapce, create the ServiceBusService with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
#### <a name="sending-and-receiving-messages"></a><span data-ttu-id="5680a-178">メッセージの送受信</span><span class="sxs-lookup"><span data-stu-id="5680a-178">Sending and receiving messages</span></span>

<span data-ttu-id="5680a-179">キューは、**create\_queue** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-179">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="5680a-180">そのキューにメッセージを挿入するには、**send\_queue\_message** メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5680a-180">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="5680a-181">**send\_queue\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-181">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="5680a-182">その後、**receive\_queue\_message** メソッドを呼び出すことで、メッセージをデキューすることができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-182">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a><span data-ttu-id="5680a-183">Service Bus トピック</span><span class="sxs-lookup"><span data-stu-id="5680a-183">Service Bus topics</span></span>

<span data-ttu-id="5680a-184">サーバー側のトピックは、**create\_topic** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-184">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="5680a-185">トピックには、**send\_topic\_message** メソッドを使用してメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-185">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="5680a-186">**send\_topic\_message_batch** メソッドを使用すると、複数のメッセージを一度に送信することができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-186">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="5680a-187">Python 3 では文字列型のメッセージが UTF-8 でエンコーディングされますが、Python 2 ではエンコーディングを独自に行う必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5680a-187">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="5680a-188">その後クライアントは、**create\_subscription** メソッドを呼び出した後、**receive\_subscription\_message** メソッドを呼び出すことで、サブスクリプションを作成し、メッセージを取り出すことができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-188">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="5680a-189">サブスクリプションが作成される前に送信されたメッセージは受信されないので注意してください。</span><span class="sxs-lookup"><span data-stu-id="5680a-189">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a><span data-ttu-id="5680a-190">イベント ハブ</span><span class="sxs-lookup"><span data-stu-id="5680a-190">Event Hub</span></span>

<span data-ttu-id="5680a-191">Event Hubs を使用すると、デバイスとサービスの多様なセットから高速でイベント ストリームを収集できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-191">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="5680a-192">イベント ハブは、**create\_event\_hub** メソッドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="5680a-192">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="5680a-193">イベントを送信するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="5680a-193">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="5680a-194">イベントの内容は、複数のメッセージを含んだ JSON エンコード文字列またはイベント メッセージです。</span><span class="sxs-lookup"><span data-stu-id="5680a-194">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

### <a name="advanced-features"></a><span data-ttu-id="5680a-195">高度な機能</span><span class="sxs-lookup"><span data-stu-id="5680a-195">Advanced features</span></span>

#### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="5680a-196">ブローカーのプロパティとユーザーのプロパティ</span><span class="sxs-lookup"><span data-stu-id="5680a-196">Broker properties and user properties</span></span>

<span data-ttu-id="5680a-197">ここでは、[こちら](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties)に定義されているブローカーのプロパティとユーザーのプロパティの使い方について説明します。</span><span class="sxs-lookup"><span data-stu-id="5680a-197">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```
<span data-ttu-id="5680a-198">datetime、int、float、boolean を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5680a-198">You can use datetime, int, float or boolean</span></span>

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
<span data-ttu-id="5680a-199">このライブラリの旧バージョンとの互換性を確保するために、`broker_properties` を JSON 文字列として定義してもかまいません。</span><span class="sxs-lookup"><span data-stu-id="5680a-199">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="5680a-200">その場合は、有効な JSON 文字列をご自身で確実に記述してください。RestAPI を送信する前に、Python によるチェックは実行されません。</span><span class="sxs-lookup"><span data-stu-id="5680a-200">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a><span data-ttu-id="5680a-201">次の手順</span><span class="sxs-lookup"><span data-stu-id="5680a-201">Next Steps</span></span>
* [<span data-ttu-id="5680a-202">Service Bus のドキュメント</span><span class="sxs-lookup"><span data-stu-id="5680a-202">Service Bus documentation</span></span>](https://docs.microsoft.com/azure/service-bus-messaging)
* [<span data-ttu-id="5680a-203">SDK のソース コード</span><span class="sxs-lookup"><span data-stu-id="5680a-203">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="5680a-204">SDK のリファレンス ドキュメント</span><span class="sxs-lookup"><span data-stu-id="5680a-204">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [<span data-ttu-id="5680a-205">その他のサンプル</span><span class="sxs-lookup"><span data-stu-id="5680a-205">Additional samples</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)
