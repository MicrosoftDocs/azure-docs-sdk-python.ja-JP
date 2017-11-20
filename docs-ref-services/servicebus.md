---
title: "Python 用 Service Bus ライブラリ"
description: "Service Bus 用の Python クライアント ライブラリと管理ライブラリのリファレンス ドキュメント"
keywords: "Azure, Python, SDK, API, メッセージング, pubsub, pub-sub, メッセージ ブローカー"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="7dcbb-104">Python 用 Service Bus ライブラリ</span><span class="sxs-lookup"><span data-stu-id="7dcbb-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="7dcbb-105">概要</span><span class="sxs-lookup"><span data-stu-id="7dcbb-105">Overview</span></span>

<span data-ttu-id="7dcbb-106">Microsoft Azure Service Bus は、信頼性の高いメッセージ キュー機能や永続的なパブリッシュ/サブスクライブ メッセージング機能など、クラウドベースのメッセージ指向ミドルウェアの一連のテクノロジをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="7dcbb-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="7dcbb-107">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="7dcbb-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a><span data-ttu-id="7dcbb-108">例</span><span class="sxs-lookup"><span data-stu-id="7dcbb-108">Example</span></span>
<span data-ttu-id="7dcbb-109">メッセージをキューに送信する</span><span class="sxs-lookup"><span data-stu-id="7dcbb-109">Send messages to a queue.</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="7dcbb-110">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="7dcbb-110">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/managementlibrary)

