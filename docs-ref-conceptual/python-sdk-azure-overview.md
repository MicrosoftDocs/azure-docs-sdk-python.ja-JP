---
title: "Python 用 Azure ライブラリ"
description: "Python 用 Azure 管理/サービス ライブラリの概要"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 2b3e6d31edd7b946664853b3478e22205ab8c92e
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-libraries-for-python"></a>Python 用 Azure ライブラリ

Python 用 Azure ライブラリを使うと、アプリケーションのコードから Azure サービスを使ったり Azure リソースを管理したりできます。 

## <a name="manage-azure-resources"></a>Azure のリソースを管理する

Python 用 Azure ライブラリを使用すると、Python アプリケーションから Azure リソースを作成したり管理したりすることができます。

たとえば、SQL Server インスタンスを作成するには、次のコードを使用できます。

```python
sql_client = SqlManagementClient(
    credentials,
    subscription_id
)

server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

ライブラリの全一覧とプロジェクトへのインポート方法については、[インストール手順](python-sdk-azure-install.md)を参照してください。そのうえで[概要の記事](python-sdk-azure-get-started.yml)を参照し、自分の Azure サブスクリプションに対して認証を設定したり、サンプル コードを実行したりする方法を確認しましょう。

## <a name="connect-to-azure-services"></a>Azure サービスへの接続

Python ライブラリを使用すると、Azure 内のリソースを作成したり管理したりするだけでなく、アプリ内からそれらのリソースに接続して利用することができます。 たとえば、SQL Database のテーブルを更新したり、Azure Storage にファイルを格納したりすることもできます。 ライブラリの全一覧から、特定のサービスに必要なライブラリをお選びください。また、それらのライブラリをアプリ内で使用するためのチュートリアルやサンプル コードは、Python デベロッパー センターから入手できます。

たとえば、単純な HTML ページを BLOB にアップロードして、URL を取得するには、次のようにします。

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

## <a name="sample-code-and-reference"></a>サンプル コードとリファレンス
以下のサンプルには、Python 用 Azure 管理ライブラリを使った一般的な自動化タスクが紹介されており、自分のアプリですぐに利用できるコードも用意されています。
- [Virtual Machines](python-sdk-azure-virtual-machine-samples.md)
- [Web アプリ](python-sdk-azure-web-apps-samples.md)
- [SQL Database](python-sdk-azure-sql-database-samples.md)

サービス ライブラリと管理ライブラリの全パッケージについて、[リファレンス](/python/api/overview/azure)が公開されています。 新機能、重大な変更、以前のバージョンからの移行手順については、[リリース ノート](python-sdk-azure-release-notes.md)を参照してください。 

## <a name="get-help-and-give-feedback"></a>質問とフィードバック

[Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに質問を投稿し、[GitHub プロジェクト](https://github.com/Azure/azure-sdk-for-python)で SDK に対して問題を開いてください。
