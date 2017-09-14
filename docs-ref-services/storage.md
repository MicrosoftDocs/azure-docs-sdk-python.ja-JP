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
# <a name="azure-storage-libraries-for-python"></a>Python 用 Azure Storage ライブラリ

## <a name="overview"></a>概要
- [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage) との間でオブジェクトやファイルの読み取りと書き込みを行う
- クラウドに接続されているアプリケーションの間で [Azure Queue Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage) を使ってメッセージを送受信する
- [Azure Table Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) で大きな構造化データの読み取りと書き込みを行う 
- [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage) で、ストレージをアプリ間で共有する

Python コードから管理ライブラリを使って、Azure Storage アカウントの作成、更新、管理のほか、アクセス キーの照会と再生成を行うことができます。

## <a name="install-the-libraries"></a>ライブラリをインストールする

### <a name="client"></a>クライアント

```bash
pip install azure-storage
```

### <a name="management"></a>管理

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>例
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

## <a name="samples"></a>サンプル

| | |
|--|--|
| [Python で Azure Blob Storage を使用する](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | Azure Storage 内のファイルとオブジェクトの作成、読み取り、更新、制限付きアクセス、削除を行います。 |
| [Python で Azure Queue Storage を使用する](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | Azure Storage キューからのメッセージの挿入、ピーク、取得、削除を行います。 | 
| [Azure Storage アカウントを管理する](https://azure.microsoft.com/resources/samples/storage-python-manage) | ストレージ アカウントの作成、更新、削除を行います。 ストレージ アカウント アクセス キーを取得および再生成します。

アプリで使用できるその他の[サンプル Python コード](https://azure.microsoft.com/resources/samples/?platform=python)も参照してください。