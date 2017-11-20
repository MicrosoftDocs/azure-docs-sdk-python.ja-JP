---
title: "Python 用 Azure Storage ライブラリ"
description: 
keywords: Azure, Python, SDK, API, Storage
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 64465964d32934a6a45dec44cb92a0a8a84b9c37
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="a920b-103">Python 用 Azure Storage ライブラリ</span><span class="sxs-lookup"><span data-stu-id="a920b-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="a920b-104">概要</span><span class="sxs-lookup"><span data-stu-id="a920b-104">Overview</span></span>
- <span data-ttu-id="a920b-105">[Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage) との間でオブジェクトやファイルの読み取りと書き込みを行う</span><span class="sxs-lookup"><span data-stu-id="a920b-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="a920b-106">クラウドに接続されているアプリケーションの間で [Azure Queue Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage) を使ってメッセージを送受信する</span><span class="sxs-lookup"><span data-stu-id="a920b-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="a920b-107">[Azure Table Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) で大きな構造化データの読み取りと書き込みを行う</span><span class="sxs-lookup"><span data-stu-id="a920b-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="a920b-108">[Azure File Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage) で、ストレージをアプリ間で共有する</span><span class="sxs-lookup"><span data-stu-id="a920b-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="a920b-109">Python コードから管理ライブラリを使って、Azure Storage アカウントの作成、更新、管理のほか、アクセス キーの照会と再生成を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a920b-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="a920b-110">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="a920b-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="a920b-111">クライアント</span><span class="sxs-lookup"><span data-stu-id="a920b-111">Client</span></span>

```bash
pip install azure-storage
```

### <a name="management"></a><span data-ttu-id="a920b-112">管理</span><span class="sxs-lookup"><span data-stu-id="a920b-112">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="a920b-113">例</span><span class="sxs-lookup"><span data-stu-id="a920b-113">Example</span></span>
```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="samples"></a><span data-ttu-id="a920b-114">サンプル</span><span class="sxs-lookup"><span data-stu-id="a920b-114">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="a920b-115">Python で Azure Blob Storage を使用する</span><span class="sxs-lookup"><span data-stu-id="a920b-115">Get started with Azure Blob Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | <span data-ttu-id="a920b-116">Azure Storage 内のファイルとオブジェクトの作成、読み取り、更新、制限付きアクセス、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="a920b-116">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="a920b-117">Python で Azure Queue Storage を使用する</span><span class="sxs-lookup"><span data-stu-id="a920b-117">Get started with Azure Queue Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | <span data-ttu-id="a920b-118">Azure Storage キューからのメッセージの挿入、ピーク、取得、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="a920b-118">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="a920b-119">Azure Storage アカウントを管理する</span><span class="sxs-lookup"><span data-stu-id="a920b-119">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="a920b-120">ストレージ アカウントの作成、更新、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="a920b-120">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="a920b-121">ストレージ アカウント アクセス キーを取得および再生成します。</span><span class="sxs-lookup"><span data-stu-id="a920b-121">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="a920b-122">アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。</span><span class="sxs-lookup"><span data-stu-id="a920b-122">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>