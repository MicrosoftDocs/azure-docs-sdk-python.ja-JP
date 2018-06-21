---
title: Python 用 Azure Storage ライブラリ
description: ''
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
ms.openlocfilehash: e45b12af9e026e0f6390556813385d86784feaa4
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2018
ms.locfileid: "35720063"
---
# <a name="azure-storage-libraries-for-python"></a>Python 用 Azure Storage ライブラリ

## <a name="overview"></a>概要
- [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage) との間でオブジェクトとファイルの読み取りと書き込みを行う
- クラウドに接続されているアプリケーションの間で [Azure Queue Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage) を使ってメッセージを送受信する
- [Azure Table Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) で大きな構造化データの読み取りと書き込みを行う 
- [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage) で、ストレージをアプリ間で共有する

Python コードから管理ライブラリを使って、Azure Storage アカウントの作成、更新、管理のほか、アクセス キーの照会と再生成を行うことができます。

## <a name="install-the-libraries"></a>ライブラリをインストールする

### <a name="client"></a>クライアント

Azure Storage クライアント ライブラリは、Blob、File、Queue、Table の 4 つのパッケージで構成されます。 Blob パッケージをインストールするには、次のコマンドを実行します。

```bash
pip install azure-storage-blob
```

### <a name="management"></a>管理

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>例
```python
from azure.storage.blob import BlockBlobService

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

## <a name="samples"></a>サンプル

| | |
|--|--|
| [Python で Azure Blob Storage を使用する](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | Azure Storage 内のファイルとオブジェクトの作成、読み取り、更新、制限付きアクセス、削除を行います。 |
| [Python で Azure Queue Storage を使用する](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | Azure Storage キューからのメッセージの挿入、ピーク、取得、削除を行います。 | 
| [Azure Storage アカウントを管理する](https://azure.microsoft.com/resources/samples/storage-python-manage) | ストレージ アカウントの作成、更新、削除を行います。 ストレージ アカウント アクセス キーを取得および再生成します。

アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。
