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
ms.openlocfilehash: 19e0e629253462f77d58740857b853d1c94d53cf
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534320"
---
# <a name="azure-container-instances-libraries-for-python"></a><span data-ttu-id="9a7ee-104">Python 用 Azure Container Instances ライブラリ</span><span class="sxs-lookup"><span data-stu-id="9a7ee-104">Azure Container Instances libraries for Python</span></span>

<span data-ttu-id="9a7ee-105">Python 用 Microsoft Azure Container Instances ライブラリを使用して、Azure コンテナー インスタンスを作成し、管理します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-105">Use the Microsoft Azure Container Instances libraries for Python to create and manage Azure container instances.</span></span> <span data-ttu-id="9a7ee-106">詳細については、[Azure Container Instances の概要](/azure/container-instances/container-instances-overview)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-106">Learn more by reading the [Azure Container Instances overview](/azure/container-instances/container-instances-overview).</span></span>

## <a name="management-apis"></a><span data-ttu-id="9a7ee-107">管理 API</span><span class="sxs-lookup"><span data-stu-id="9a7ee-107">Management APIs</span></span>

<span data-ttu-id="9a7ee-108">管理ライブラリを使用して、Azure で Azure コンテナー インスタンスを作成して管理します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-108">Use the management library to create and manage Azure container instances in Azure.</span></span>

<span data-ttu-id="9a7ee-109">pip で管理パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-109">Install the management package via pip:</span></span>

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a><span data-ttu-id="9a7ee-110">サンプル ソース</span><span class="sxs-lookup"><span data-stu-id="9a7ee-110">Example source</span></span>

<span data-ttu-id="9a7ee-111">以下のコード例をコンテキスト内で確認するには、次の GitHub リポジトリでサンプルを検索してください。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-111">If you'd like to see the following code examples in context, you can find them in the following GitHub repository:</span></span>

[<span data-ttu-id="9a7ee-112">Azure-Samples/aci-docs-sample-python</span><span class="sxs-lookup"><span data-stu-id="9a7ee-112">Azure-Samples/aci-docs-sample-python</span></span>](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a><span data-ttu-id="9a7ee-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="9a7ee-113">Authentication</span></span>

<span data-ttu-id="9a7ee-114">SDK クライアント (次の例の Azure Container Instances クライアント、Resource Manager クライアントなど) を認証する最も簡単な方法の 1 つは、[ファイル ベースの認証](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file)を使用することです。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-114">One of the easiest ways to authenticate SDK clients (like the Azure Container Instances and Resource Manager clients in the following example) is with [file-based authentication](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file).</span></span> <span data-ttu-id="9a7ee-115">ファイル ベースの認証では、資格情報ファイルへのパスを表す `AZURE_AUTH_LOCATION` 環境変数を照会します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-115">File-based authentication queries the `AZURE_AUTH_LOCATION` environment variable for the path to a credentials file.</span></span> <span data-ttu-id="9a7ee-116">ファイル ベースの認証を使用するには:</span><span class="sxs-lookup"><span data-stu-id="9a7ee-116">To use file-based authentication:</span></span>

1. <span data-ttu-id="9a7ee-117">[Azure CLI](/cli/azure) または [Cloud Shell](https://shell.azure.com/) で資格情報ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-117">Create a credentials file with the [Azure CLI](/cli/azure) or [Cloud Shell](https://shell.azure.com/):</span></span>

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   <span data-ttu-id="9a7ee-118">[Cloud Shell](https://shell.azure.com/) を使用して資格情報ファイルを生成する場合は、その内容を、お使いの Python アプリケーションがアクセスできるローカル ファイルにコピーします。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-118">If you use the [Cloud Shell](https://shell.azure.com/) to generate the credentials file, copy its contents into a local file that your Python application can access.</span></span>

2. <span data-ttu-id="9a7ee-119">`AZURE_AUTH_LOCATION` 環境変数を、生成された資格情報ファイルの完全なパスに設定します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-119">Set the `AZURE_AUTH_LOCATION` environment variable to the full path of the generated credentials file.</span></span> <span data-ttu-id="9a7ee-120">次に例を示します (Bash の場合)。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-120">For example (in Bash):</span></span>

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

<span data-ttu-id="9a7ee-121">資格情報ファイルを作成し、`AZURE_AUTH_LOCATION` 環境変数を設定したら、[client_factory][client_factory] モジュールの `get_client_from_auth_file` メソッドを使用して、ResourceManagementClient オブジェクトと [ContainerInstanceManagementClient][ContainerInstanceManagementClient] オブジェクトを初期化します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-121">Once you've created the credentials file and populated the `AZURE_AUTH_LOCATION` environment variable, use the `get_client_from_auth_file` method of the [client_factory][client_factory] module to initialize the [ResourceManagementClient][ResourceManagementClient] and [ContainerInstanceManagementClient][ContainerInstanceManagementClient] objects.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

<span data-ttu-id="9a7ee-122">Azure の Python 管理ライブラリで使用できる認証方法の詳細については、「[Python 用 Azure 管理ライブラリを使用した認証](/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-122">For more details about the available authentication methods in the Python management libraries for Azure, see [Authenticate with the Azure Management Libraries for Python](/python/azure/python-sdk-azure-authenticate).</span></span>

## <a name="create-container-group---single-container"></a><span data-ttu-id="9a7ee-123">コンテナー グループを作成する - 1 つのコンテナー</span><span class="sxs-lookup"><span data-stu-id="9a7ee-123">Create container group - single container</span></span>

<span data-ttu-id="9a7ee-124">この例では、1 つのコンテナーを含むコンテナー グループが作成されます</span><span class="sxs-lookup"><span data-stu-id="9a7ee-124">This example creates a container group with a single container</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L141 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a><span data-ttu-id="9a7ee-125">コンテナー グループを作成する - 複数のコンテナー</span><span class="sxs-lookup"><span data-stu-id="9a7ee-125">Create container group - multiple containers</span></span>

<span data-ttu-id="9a7ee-126">この例では、アプリケーション コンテナーとサイドカー コンテナーという 2 つのコンテナーを含むコンテナー グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-126">This example creates a container group with two containers: an application container and a sidecar container.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L144-L197 "Create multi-container group")]

## <a name="create-task-based-container-group"></a><span data-ttu-id="9a7ee-127">タスク ベースのコンテナー グループを作成する</span><span class="sxs-lookup"><span data-stu-id="9a7ee-127">Create task-based container group</span></span>

<span data-ttu-id="9a7ee-128">この例では、1 つのタスク ベースのコンテナーを含むコンテナー グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-128">This example creates a container group with a single task-based container.</span></span> <span data-ttu-id="9a7ee-129">この例では、複数の機能を示します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-129">This example demonstrates several features:</span></span>

* <span data-ttu-id="9a7ee-130">[コマンド ライン オーバーライド](/azure/container-instances/container-instances-start-command) - コンテナーの Dockerfile の `CMD` 行で指定されているものとは異なるカスタム コマンド ラインが指定されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-130">[Command line override](/azure/container-instances/container-instances-start-command) - A custom command line, different from that which is specified in the container's Dockerfile `CMD` line, is specified.</span></span> <span data-ttu-id="9a7ee-131">コマンド ライン オーバーライドを使用すると、コンテナーの起動時に実行するカスタム コマンド ラインを指定して、コンテナーに組み込まれている既定のコマンド ラインをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-131">Command line override allows you to specify a custom command line to execute at container startup, overriding the default command line baked-in to the container.</span></span> <span data-ttu-id="9a7ee-132">コンテナーの起動時に複数のコマンドが実行される場合は、次が適用されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-132">Regarding executing multiple commands at container startup, the following applies:</span></span>

   <span data-ttu-id="9a7ee-133">`echo FOO BAR` など、**1 つのコマンド**を、複数のコマンド ライン引数を指定して実行する場合、その引数は、1 つの文字列リストとして [Container][Container] の `command` プロパティに指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-133">If you want to run a **single command** with several command-line arguments, for example `echo FOO BAR`, you must supply them as a string list to the `command` property of the [Container][Container].</span></span> <span data-ttu-id="9a7ee-134">例:</span><span class="sxs-lookup"><span data-stu-id="9a7ee-134">For example:</span></span>

   `command = ['echo', 'FOO', 'BAR']`

   <span data-ttu-id="9a7ee-135">ただし、**複数のコマンド**を、複数の (可能性がある) 引数を指定して実行する場合は、シェルを実行し、チェーンされたコマンドを 1 つの引数として渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-135">If, however, you want to run **multiple commands** with (potentially) multiple arguments, you must execute a shell and pass the chained commands as an argument.</span></span> <span data-ttu-id="9a7ee-136">たとえば、これにより `echo` と `tail` コマンドの両方が実行されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-136">For example, this executes both an `echo` and a `tail` command:</span></span>

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* <span data-ttu-id="9a7ee-137">[環境変数](/azure/container-instances/container-instances-environment-variables) - 2 つの環境変数が、コンテナー グループ内のコンテナーに対して指定されています。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-137">[Environment variables](/azure/container-instances/container-instances-environment-variables) - Two environment variables are specified for the container in the container group.</span></span> <span data-ttu-id="9a7ee-138">環境変数を使用して、実行時のスクリプトまたはアプリケーションの動作を変更するか、動的な情報を、コンテナー内で実行されているアプリケーションに渡します。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-138">Use environment variables to modify script or application behavior at runtime, or otherwise pass dynamic information to an application running in the container.</span></span>
* <span data-ttu-id="9a7ee-139">[再起動ポリシー](/azure/container-instances/container-instances-restart-policy) - コンテナーは再起動ポリシー "Never" を使用して構成されます。これは、バッチ ジョブの一環として実行されるタスク ベースのコンテナーに有用です。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-139">[Restart policy](/azure/container-instances/container-instances-restart-policy) - The container is configured with a restart policy of "Never," useful for task-based containers that are executed as part of a batch job.</span></span>
* <span data-ttu-id="9a7ee-140">[AzureOperationPoller][AzureOperationPoller] による操作のポーリング - create メソッドの呼び出し後、その操作はポーリングされます。これにより、操作が完了したことを判断し、コンテナー グループのログを取得できます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-140">Operation polling with [AzureOperationPoller][AzureOperationPoller] - After the create method is invoked, the operation is polled to determine when it has completed and the container group's logs can be obtained.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L200-L276 "Run a task-based container")]

## <a name="list-container-groups"></a><span data-ttu-id="9a7ee-141">コンテナー グループの一覧を表示する</span><span class="sxs-lookup"><span data-stu-id="9a7ee-141">List container groups</span></span>

<span data-ttu-id="9a7ee-142">この例では、リソース グループ内のコンテナー グループの一覧が表示され、そのプロパティがいくつか出力されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-142">This example lists the container groups in a resource group and then prints a few of their properties.</span></span>

<span data-ttu-id="9a7ee-143">コンテナー グループの一覧を表示すると、返された各グループの [instance_view][instance_view] は `None` になっています。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-143">When you list container groups,the [instance_view][instance_view] of each returned group is `None`.</span></span> <span data-ttu-id="9a7ee-144">コンテナー グループ内のコンテナーの詳細を取得するには、コンテナー グループに対して [get][containergroupoperations_get] を使用します。これにより、`instance_view` プロパティが設定されたグループが返されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-144">To get the details of the containers within a container group, you must then [get][containergroupoperations_get] the container group, which returns the group with its `instance_view` property populated.</span></span> <span data-ttu-id="9a7ee-145">コンテナー グループのコンテナーをその `instance_view` で反復処理する例については、次の「[既存のコンテナー グループを取得する](#get-an-existing-container-group)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-145">See the next section, [Get an existing container group](#get-an-existing-container-group), for an example of iterating over a container group's containers in its `instance_view`.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L279-L293 "List container groups")]

## <a name="get-an-existing-container-group"></a><span data-ttu-id="9a7ee-146">既存のコンテナー グループを取得する</span><span class="sxs-lookup"><span data-stu-id="9a7ee-146">Get an existing container group</span></span>

<span data-ttu-id="9a7ee-147">この例では、リソース グループから特定のコンテナー グループが取得され、そのプロパティ (そのコンテナーを含む) とその値がいくつか出力されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-147">This example gets a specific container group from a resource group, and then prints a few of its properties (including its containers) and their values.</span></span>

<span data-ttu-id="9a7ee-148">[取得操作][containergroupoperations_get]によって、returns a container group with its [instance_view][instance_view] が設定されたコンテナー グループが返されます。これにより、グループ内の各コンテナーを反復処理できます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-148">The [get operation][containergroupoperations_get] returns a container group with its [instance_view][instance_view] populated, which allows you to iterate over each container in the group.</span></span> <span data-ttu-id="9a7ee-149">コンテナー グループの `instance_vew` プロパティは `get` 操作によってのみ設定されます。サブスクリプションまたはリソース グループ内のコンテナー グループを一覧表示しても、この操作は性質上負荷がかかる可能性があるため、インスタンス ビューは設定されません (たとえば、数百のコンテナー グループを一覧表示すると、それぞれのグループに複数のコンテナーが含まれる可能性があります)。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-149">Only the `get` operation populates the `instance_vew` property of the container group--listing the container groups in a subscription or resource group doesn't populate the instance view due to the potentially expensive nature of the operation (for example, when listing hundreds of container groups, each potentially containing multiple containers).</span></span> <span data-ttu-id="9a7ee-150">前に「[コンテナー グループの一覧を表示する](#list-container-groups)」で説明したように、`list` の実行後、特定のコンテナー グループのコンテナー インスタンスの詳細を取得するには、そのグループに対して `get` を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-150">As mentioned previously in the [List container groups](#list-container-groups) section, after a `list`, you must subsequently `get` a specific container group to obtain its container instance details.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L296-L325 "Get container group")]

## <a name="delete-a-container-group"></a><span data-ttu-id="9a7ee-151">コンテナー グループを削除する</span><span class="sxs-lookup"><span data-stu-id="9a7ee-151">Delete a container group</span></span>

<span data-ttu-id="9a7ee-152">この例では、リソース グループの複数のコンテナー グループと、リソース グループが削除されます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-152">This example deletes several container groups from a resource group, as well as the resource group.</span></span>

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python -->
[!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a><span data-ttu-id="9a7ee-153">次の手順</span><span class="sxs-lookup"><span data-stu-id="9a7ee-153">Next steps</span></span>

* <span data-ttu-id="9a7ee-154">前の例のソース コードは GitHub で確認できます。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-154">The source code for the preceding examples can be found on GitHub:</span></span>

  <span data-ttu-id="9a7ee-155">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span><span class="sxs-lookup"><span data-stu-id="9a7ee-155">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span></span>

* <span data-ttu-id="9a7ee-156">Azure Container Instances のその他のコード サンプル:</span><span class="sxs-lookup"><span data-stu-id="9a7ee-156">More Azure Container Instances code samples:</span></span>

  <span data-ttu-id="9a7ee-157">[Azure のコード サンプル][samples-aci]</span><span class="sxs-lookup"><span data-stu-id="9a7ee-157">[Azure Code Samples][samples-aci]</span></span>

* <span data-ttu-id="9a7ee-158">アプリで使用できるその他の[サンプル Python コード][samples-python]も参照してください。</span><span class="sxs-lookup"><span data-stu-id="9a7ee-158">Explore more [sample Python code][samples-python] you can use in your apps.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9a7ee-159">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="9a7ee-159">Explore the management APIs</span></span>](/python/api/overview/azure/containerinstance/management)

<!-- LINKS - External -->
[aci-docs-sample-python]: https://github.com/Azure-Samples/aci-docs-sample-python
[samples-aci]: https://azure.microsoft.com/resources/samples/?sort=0&term=ACI
[samples-python]: https://azure.microsoft.com/resources/samples/?platform=python

<!-- TYPES -->
[AzureOperationPoller]: /python/api/msrestazure.azure_operation.AzureOperationPoller
[client_factory]: /python/api/azure.common.client_factory
[Container]: /python/api/azure.mgmt.containerinstance.models.container
[ContainerGroupInstanceView]: /python/api/azure.mgmt.containerinstance.models.containergrouppropertiesinstanceview
[containergroupoperations_get]: /python/api/azure.mgmt.containerinstance.operations.containergroupsoperations#get-resource-group-name--container-group-name--custom-headers-none--raw-false----operation-config-
[ContainerInstanceManagementClient]: /python/api/azure.mgmt.containerinstance.containerinstancemanagementclient
[instance_view]: /python/api/azure.mgmt.containerinstance.models.containergroup#variables
[ResourceManagementClient]: /python/api/azure.mgmt.resource.resources.resourcemanagementclient