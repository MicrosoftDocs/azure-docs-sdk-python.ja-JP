---
title: Python 用 Azure Container Instances ライブラリ
description: Python 用 Azure Container Instances ライブラリのリファレンス
keywords: Azure, python, SDK, API, ACI, container, instances
author: dlepow
manager: jeconnoc
ms.date: 04/15/2019
ms.author: danlep
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: container-instances
ms.openlocfilehash: 88df9443efb98bc5cec26c5eb4b01a4956141d40
ms.sourcegitcommit: 1b45953f168cbf36869c24c1741d70153b88b9fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59675937"
---
# <a name="azure-container-instances-libraries-for-python"></a>Python 用 Azure Container Instances ライブラリ

Python 用 Microsoft Azure Container Instances ライブラリを使用して、Azure コンテナー インスタンスを作成し、管理します。 詳細については、[Azure Container Instances の概要](/azure/container-instances/container-instances-overview)に関するページをご覧ください。

## <a name="management-apis"></a>管理 API

管理ライブラリを使用して、Azure で Azure コンテナー インスタンスを作成して管理します。

pip で管理パッケージをインストールします。

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a>サンプル ソース

以下のコード例をコンテキスト内で確認するには、次の GitHub リポジトリでサンプルを検索してください。

[Azure-Samples/aci-docs-sample-python](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a>Authentication

SDK クライアント (次の例の Azure Container Instances クライアント、Resource Manager クライアントなど) を認証する最も簡単な方法の 1 つは、[ファイル ベースの認証](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)を使用することです。 ファイル ベースの認証では、資格情報ファイルへのパスを表す `AZURE_AUTH_LOCATION` 環境変数を照会します。 ファイル ベースの認証を使用するには:

1. [Azure CLI](/cli/azure) または [Cloud Shell](https://shell.azure.com/) で資格情報ファイルを作成します。

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   [Cloud Shell](https://shell.azure.com/) を使用して資格情報ファイルを生成する場合は、その内容を、お使いの Python アプリケーションがアクセスできるローカル ファイルにコピーします。

2. `AZURE_AUTH_LOCATION` 環境変数を、生成された資格情報ファイルの完全なパスに設定します。 次に例を示します (Bash の場合)。

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

資格情報ファイルを作成し、`AZURE_AUTH_LOCATION` 環境変数を設定したら、[client_factory][client_factory] モジュールの `get_client_from_auth_file` メソッドを使用して、[ResourceManagementClient][ResourceManagementClient] オブジェクトと [ContainerInstanceManagementClient][ContainerInstanceManagementClient] オブジェクトを初期化します。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

Azure の Python 管理ライブラリで使用できる認証方法の詳細については、「[Python 用 Azure 管理ライブラリを使用した認証](/python/azure/python-sdk-azure-authenticate)」を参照してください。

## <a name="create-container-group---single-container"></a>コンテナー グループを作成する - 1 つのコンテナー

この例では、1 つのコンテナーを含むコンテナー グループが作成されます

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L141 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a>コンテナー グループを作成する - 複数のコンテナー

この例では、アプリケーション コンテナーとサイドカー コンテナーという 2 つのコンテナーを含むコンテナー グループが作成されます。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L144-L197 "Create multi-container group")]

## <a name="create-task-based-container-group"></a>タスク ベースのコンテナー グループを作成する

この例では、1 つのタスク ベースのコンテナーを含むコンテナー グループが作成されます。 この例では、複数の機能を示します。

* [コマンド ライン オーバーライド](/azure/container-instances/container-instances-restart-policy#command-line-override) - コンテナーの Dockerfile の `CMD` 行で指定されているものとは異なるカスタム コマンド ラインが指定されます。 コマンド ライン オーバーライドを使用すると、コンテナーの起動時に実行するカスタム コマンド ラインを指定して、コンテナーに組み込まれている既定のコマンド ラインをオーバーライドできます。 コンテナーの起動時に複数のコマンドが実行される場合は、次が適用されます。

   `echo FOO BAR` など、**1 つのコマンド**を、複数のコマンド ライン引数を指定して実行する場合、その引数は、1 つの文字列リストとして [Container][Container] の `command` プロパティに指定する必要があります。 例: 

   `command = ['echo', 'FOO', 'BAR']`

   ただし、**複数のコマンド**を、複数の (可能性がある) 引数を指定して実行する場合は、シェルを実行し、チェーンされたコマンドを 1 つの引数として渡す必要があります。 たとえば、これにより `echo` と `tail` コマンドの両方が実行されます。

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* [環境変数](/azure/container-instances/container-instances-environment-variables) - 2 つの環境変数が、コンテナー グループ内のコンテナーに対して指定されています。 環境変数を使用して、実行時のスクリプトまたはアプリケーションの動作を変更するか、動的な情報を、コンテナー内で実行されているアプリケーションに渡します。
* [再起動ポリシー](/azure/container-instances/container-instances-restart-policy) - コンテナーは再起動ポリシー "Never" を使用して構成されます。これは、バッチ ジョブの一環として実行されるタスク ベースのコンテナーに有用です。
* [AzureOperationPoller][AzureOperationPoller] による操作のポーリング - create メソッドの呼び出し後、その操作はポーリングされます。これにより、操作が完了したことを判断し、コンテナー グループのログを取得できます。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L200-L276 "Run a task-based container")]

## <a name="list-container-groups"></a>コンテナー グループの一覧を表示する

この例では、リソース グループ内のコンテナー グループの一覧が表示され、そのプロパティがいくつか出力されます。

コンテナー グループの一覧を表示すると、返された各グループの [instance_view][instance_view] は `None` になっています。 コンテナー グループ内のコンテナーの詳細を取得するには、コンテナー グループに対して [get][containergroupoperations_get] を使用します。これにより、`instance_view` プロパティが設定されたグループが返されます。 コンテナー グループのコンテナーをその `instance_view` で反復処理する例については、次の「[既存のコンテナー グループを取得する](#get-an-existing-container-group)」を参照してください。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L279-L293 "List container groups")]

## <a name="get-an-existing-container-group"></a>既存のコンテナー グループを取得する

この例では、リソース グループから特定のコンテナー グループが取得され、そのプロパティ (そのコンテナーを含む) とその値がいくつか出力されます。

[取得操作][containergroupoperations_get]によって、[instance_view][instance_view] が設定されたコンテナー グループが返されます。これにより、グループ内の各コンテナーを反復処理できます。 コンテナー グループの `instance_vew` プロパティは `get` 操作によってのみ設定されます。サブスクリプションまたはリソース グループ内のコンテナー グループを一覧表示しても、この操作は性質上負荷がかかる可能性があるため、インスタンス ビューは設定されません (たとえば、数百のコンテナー グループを一覧表示すると、それぞれのグループに複数のコンテナーが含まれる可能性があります)。 前に「[コンテナー グループの一覧を表示する](#list-container-groups)」で説明したように、`list` の実行後、特定のコンテナー グループのコンテナー インスタンスの詳細を取得するには、そのグループに対して `get` を実行する必要があります。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L296-L325 "Get container group")]

## <a name="delete-a-container-group"></a>コンテナー グループを削除する

この例では、リソース グループの複数のコンテナー グループと、リソース グループが削除されます。

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a>次の手順

* 前の例のソース コードは GitHub で確認できます。

  [Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]

* Azure Container Instances のその他のコード サンプル:

  [Azure のコード サンプル][samples-aci]

* アプリで使用できるその他の[サンプル Python コード][samples-python]を確認してください。

> [!div class="nextstepaction"]
> [Management API を探す](/python/api/overview/azure/containerinstance/management)

<!-- LINKS - External -->
[aci-docs-sample-python]: https://github.com/Azure-Samples/aci-docs-sample-python
[samples-aci]: https://azure.microsoft.com/resources/samples/?sort=0&term=ACI
[samples-python]: https://azure.microsoft.com/resources/samples/?platform=python

<!-- TYPES -->
[AzureOperationPoller]: /python/api/msrestazure.azure_operation.AzureOperationPoller
[client_factory]: /python/api/azure.common.client_factory
[Container]: /python/api/azure.mgmt.containerinstance.models.container
[ContainerGroupInstanceView]: /python/api/azure.mgmt.containerinstance.models.containergrouppropertiesinstanceview
[containergroupoperations_get]: /python/api/azure.mgmt.containerinstance.operations.containergroupsoperations#get
[ContainerInstanceManagementClient]: /python/api/azure.mgmt.containerinstance.containerinstancemanagementclient
[instance_view]: /python/api/azure.mgmt.containerinstance.models.containergroup#variables
[ResourceManagementClient]: /python/api/azure.mgmt.resource.resources.resourcemanagementclient