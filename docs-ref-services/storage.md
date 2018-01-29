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
ms.openlocfilehash: e00e821ff3e806a994fa8d96aae50c35eeeb8392
ms.sourcegitcommit: 5ab15a7214082d16f339a13e4ae7735b3a57a9aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="b77c5-103">Python 用 Azure Storage ライブラリ</span><span class="sxs-lookup"><span data-stu-id="b77c5-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="b77c5-104">概要</span><span class="sxs-lookup"><span data-stu-id="b77c5-104">Overview</span></span>
- <span data-ttu-id="b77c5-105">[Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage) との間でオブジェクトとファイルの読み取りと書き込みを行う</span><span class="sxs-lookup"><span data-stu-id="b77c5-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="b77c5-106">クラウドに接続されているアプリケーションの間で [Azure Queue Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage) を使ってメッセージを送受信する</span><span class="sxs-lookup"><span data-stu-id="b77c5-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="b77c5-107">[Azure Table Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) で大きな構造化データの読み取りと書き込みを行う</span><span class="sxs-lookup"><span data-stu-id="b77c5-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="b77c5-108">[Azure File Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage) で、ストレージをアプリ間で共有する</span><span class="sxs-lookup"><span data-stu-id="b77c5-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="b77c5-109">Python コードから管理ライブラリを使って、Azure Storage アカウントの作成、更新、管理のほか、アクセス キーの照会と再生成を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="b77c5-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="b77c5-110">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="b77c5-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="b77c5-111">クライアント</span><span class="sxs-lookup"><span data-stu-id="b77c5-111">Client</span></span>

<span data-ttu-id="b77c5-112">Azure Storage クライアント ライブラリは、Blob、File、Queue、Table の 4 つのパッケージで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b77c5-112">Azure Storage Client Libraries consist of 4 packages: Blob, File, Queue and Table.</span></span> <span data-ttu-id="b77c5-113">Blob パッケージをインストールするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b77c5-113">To install the blob package, run:</span></span>

```bash
pip install azure-storage-blob
```

### <a name="management"></a><span data-ttu-id="b77c5-114">管理</span><span class="sxs-lookup"><span data-stu-id="b77c5-114">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="b77c5-115">例</span><span class="sxs-lookup"><span data-stu-id="b77c5-115">Example</span></span>
```python
blob_service = BlockBlobService(account_name, account_key)

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

## <a name="samples"></a><span data-ttu-id="b77c5-116">サンプル</span><span class="sxs-lookup"><span data-stu-id="b77c5-116">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="b77c5-117">Python で Azure Blob Storage を使用する</span><span class="sxs-lookup"><span data-stu-id="b77c5-117">Get started with Azure Blob Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | <span data-ttu-id="b77c5-118">Azure Storage 内のファイルとオブジェクトの作成、読み取り、更新、制限付きアクセス、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="b77c5-118">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="b77c5-119">Python で Azure Queue Storage を使用する</span><span class="sxs-lookup"><span data-stu-id="b77c5-119">Get started with Azure Queue Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | <span data-ttu-id="b77c5-120">Azure Storage キューからのメッセージの挿入、ピーク、取得、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="b77c5-120">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="b77c5-121">Azure Storage アカウントを管理する</span><span class="sxs-lookup"><span data-stu-id="b77c5-121">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="b77c5-122">ストレージ アカウントの作成、更新、削除を行います。</span><span class="sxs-lookup"><span data-stu-id="b77c5-122">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="b77c5-123">ストレージ アカウント アクセス キーを取得および再生成します。</span><span class="sxs-lookup"><span data-stu-id="b77c5-123">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="b77c5-124">アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。</span><span class="sxs-lookup"><span data-stu-id="b77c5-124">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>
