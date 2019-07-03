---
title: Python 用 Azure ライブラリ
description: Python 用 Azure 管理/サービス ライブラリの概要
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: ''
ms.openlocfilehash: bb17295de2f0272f0525fda5edab87e840764478
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534465"
---
# <a name="azure-libraries-for-python"></a><span data-ttu-id="723b0-104">Python 用 Azure ライブラリ</span><span class="sxs-lookup"><span data-stu-id="723b0-104">Azure libraries for Python</span></span>

<span data-ttu-id="723b0-105">Python 用 Azure ライブラリを使うと、アプリケーションのコードから Azure サービスを使ったり Azure リソースを管理したりできます。</span><span class="sxs-lookup"><span data-stu-id="723b0-105">The Azure libraries for Python let you use Azure services and manage Azure resources from your application code.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="723b0-106">Azure のリソースを管理する</span><span class="sxs-lookup"><span data-stu-id="723b0-106">Manage Azure resources</span></span>

<span data-ttu-id="723b0-107">Python 用 Azure ライブラリを使用すると、Python アプリケーションから Azure リソースを作成したり管理したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="723b0-107">Create and manage Azure resources from Python applications using the Azure libraries for Python.</span></span>

<span data-ttu-id="723b0-108">たとえば、SQL Server インスタンスを作成するには、次のコードを使用できます。</span><span class="sxs-lookup"><span data-stu-id="723b0-108">For example, to create a SQL Server instance, you can use the following code:</span></span>

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

<span data-ttu-id="723b0-109">ライブラリの全一覧とプロジェクトへのインポート方法については、[インストール手順](python-sdk-azure-install.md)を参照してください。そのうえで[概要の記事](python-sdk-azure-get-started.yml)を参照し、自分の Azure サブスクリプションに対して認証を設定したり、サンプル コードを実行したりする方法を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="723b0-109">Review the [install instructions](python-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](python-sdk-azure-get-started.yml) to set up your authentication and run sample code against your own Azure subscription.</span></span>

## <a name="connect-to-azure-services"></a><span data-ttu-id="723b0-110">Azure サービスへの接続</span><span class="sxs-lookup"><span data-stu-id="723b0-110">Connect to Azure services</span></span>

<span data-ttu-id="723b0-111">Python ライブラリを使用すると、Azure 内のリソースを作成したり管理したりするだけでなく、アプリ内からそれらのリソースに接続して利用することができます。</span><span class="sxs-lookup"><span data-stu-id="723b0-111">In addition to using Python libraries to create and manage resources within Azure, you can also use Python libraries to connect and use those resources in your apps.</span></span> <span data-ttu-id="723b0-112">たとえば、SQL Database のテーブルを更新したり、Azure Storage にファイルを格納したりすることもできます。</span><span class="sxs-lookup"><span data-stu-id="723b0-112">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="723b0-113">ライブラリの全一覧から、特定のサービスに必要なライブラリをお選びください。また、それらのライブラリをアプリ内で使用するためのチュートリアルやサンプル コードは、Python デベロッパー センターから入手できます。</span><span class="sxs-lookup"><span data-stu-id="723b0-113">Select the library you need for a particular service from the complete list of libraries and visit the Python developer center for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="723b0-114">たとえば、単純な HTML ページを BLOB にアップロードして、URL を取得するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="723b0-114">For example, to upload a simple HTML page on a blob and get the Url:</span></span>

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

## <a name="sample-code-and-reference"></a><span data-ttu-id="723b0-115">サンプル コードとリファレンス</span><span class="sxs-lookup"><span data-stu-id="723b0-115">Sample code and reference</span></span>
<span data-ttu-id="723b0-116">以下のサンプルには、Python 用 Azure 管理ライブラリを使った一般的な自動化タスクが紹介されており、自分のアプリですぐに利用できるコードも用意されています。</span><span class="sxs-lookup"><span data-stu-id="723b0-116">The following samples cover common automation tasks with the Azure management libraries for Python and have code ready to use in your own apps:</span></span>
- [<span data-ttu-id="723b0-117">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="723b0-117">Virtual Machines</span></span>](python-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="723b0-118">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="723b0-118">Web apps</span></span>](python-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="723b0-119">SQL Database</span><span class="sxs-lookup"><span data-stu-id="723b0-119">SQL Database</span></span>](python-sdk-azure-sql-database-samples.md)

<span data-ttu-id="723b0-120">サービス ライブラリと管理ライブラリの全パッケージについて、[リファレンス](/python/api/overview/azure)が公開されています。</span><span class="sxs-lookup"><span data-stu-id="723b0-120">A [reference](/python/api/overview/azure) is available for all packages in both the service an management libraries.</span></span> <span data-ttu-id="723b0-121">新機能、重大な変更、以前のバージョンからの移行手順については、[リリース ノート](python-sdk-azure-release-notes.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="723b0-121">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](python-sdk-azure-release-notes.md).</span></span> 

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="723b0-122">質問とフィードバック</span><span class="sxs-lookup"><span data-stu-id="723b0-122">Get help and give feedback</span></span>

<span data-ttu-id="723b0-123">[Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) のコミュニティに質問を投稿し、[GitHub プロジェクト](https://github.com/Azure/azure-sdk-for-python)で SDK に対して問題を開いてください。</span><span class="sxs-lookup"><span data-stu-id="723b0-123">Post questions to the community on [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) and open issues against the SDK on the [project GitHub](https://github.com/Azure/azure-sdk-for-python).</span></span>
