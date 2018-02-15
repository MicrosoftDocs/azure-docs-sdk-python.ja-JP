---
title: "Python 用 Azure 管理ライブラリを使用した認証"
description: "Python 用 Azure 管理ライブラリへのサービス プリンシパルを使用して認証を行います"
keywords: "Azure, Python, SDK, API, 認証, active directory, サービス プリンシパル"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 271722eee1ef982d1f091b3d3af29069917f3e17
ms.sourcegitcommit: 97e5d660eb4a006f969c3010087e1386cc6eb482
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2018
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="13e4f-104">Python 用 Azure 管理ライブラリを使用した認証</span><span class="sxs-lookup"><span data-stu-id="13e4f-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="13e4f-105">Python 管理ライブラリを使用してリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては、いくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="13e4f-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="13e4f-106">トークン資格情報による認証</span><span class="sxs-lookup"><span data-stu-id="13e4f-106">Authenticate with token credentials</span></span>

<span data-ttu-id="13e4f-107">構成ファイル、レジストリ、または Azure Key Vault に、資格情報を安全に格納します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="13e4f-108">次の例では、認証に[サービス プリンシパル](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)を使用します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="13e4f-109">Azure CLI 2.0 を通じてサービス プリンシパルを作成できます</span><span class="sxs-lookup"><span data-stu-id="13e4f-109">You can create a Service Principal via the Azure CLI 2.0</span></span>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```

```python
    from azure.common.credentials import ServicePrincipalCredentials

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID
    )
```

> <span data-ttu-id="13e4f-110">[注!] いずれかの Azure ソブリン クラウドに接続するには、`cloud_environment` パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-110">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>

```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID,
        cloud_environment = AZURE_CHINA_CLOUD
    )
```

<span data-ttu-id="13e4f-111">より詳細な制御が必要な場合は、[ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) および SDK ADAL ラッパーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="13e4f-111">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="13e4f-112">利用可能なすべてのシナリオの一覧とサンプルについては、ADAL の Web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="13e4f-112">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="13e4f-113">たとえば、サービス プリンシパル認証の場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="13e4f-113">For instance for service principal authentication:</span></span>

```python
    import adal
    from msrestazure.azure_active_directory import AdalAuthentication
    from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
    RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

    context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
    credentials = AdalAuthentication(
        context.acquire_token_with_client_credentials,
        RESOURCE,
        CLIENT,
        KEY
    )
```

<span data-ttu-id="13e4f-114">すべての有効な ADAL 呼び出しは、`AdalAuthentication` クラスで使用できます。</span><span class="sxs-lookup"><span data-stu-id="13e4f-114">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="13e4f-115">次に、API の操作を開始するために、クライアント オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-115">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="13e4f-116">[注!] Azure ソブリン クラウドを使用する場合は、管理クライアントを作成するときに (`msrestazure.azure_cloud` の定数を通じて) 適切なベース URL も指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="13e4f-116">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="13e4f-117">たとえば、Azure China Cloud の場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="13e4f-117">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="13e4f-118">ファイル ベースの認証</span><span class="sxs-lookup"><span data-stu-id="13e4f-118">File based authentication</span></span>

<span data-ttu-id="13e4f-119">最も簡単な認証方法は、Azure サービス プリンシパルの資格情報が含まれた JSON ファイルを作成することです。</span><span class="sxs-lookup"><span data-stu-id="13e4f-119">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="13e4f-120">次の CLI コマンドを使用して、新しいサービス プリンシパルとこのファイルを同時に作成できます。</span><span class="sxs-lookup"><span data-stu-id="13e4f-120">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="13e4f-121">このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。</span><span class="sxs-lookup"><span data-stu-id="13e4f-121">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="13e4f-122">ご利用のシェルから、このファイルの完全なパスを保持する環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-122">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="13e4f-123">自分でファイルを作成する場合は、次の形式に従ってください。</span><span class="sxs-lookup"><span data-stu-id="13e4f-123">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "ad735158-65ca-11e7-ba4d-ecb1d756380e",
    "clientSecret": "b70bb224-65ca-11e7-810c-ecb1d756380e",
    "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
    "tenantId": "c81da1d8-65ca-11e7-b1d1-ecb1d756380e",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="13e4f-124">その後、クライアント ファクトリを使用して任意のクライアントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="13e4f-124">You can then create any client using the client factory:</span></span>
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="13e4f-125">管理対象のサービス ID (MSI) による認証</span><span class="sxs-lookup"><span data-stu-id="13e4f-125">Authenticate with Managed Service Identity(MSI)</span></span> 
<span data-ttu-id="13e4f-126">MSI を使えば、Azure のリソースで SDK/CLI を簡単に使用できます。特定の資格情報の作成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="13e4f-126">MSI is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

    # Create MSI Authentication
    credentials = MSIAuthentication()


    # Create a Subscription Client
    subscription_client = SubscriptionClient(credentials)
    subscription = next(subscription_client.subscriptions.list())
    subscription_id = subscription.subscription_id

    # Create a Resource Management client
    resource_client = ResourceManagementClient(credentials, subscription_id)

    
    # List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
    for resource_group in resource_client.resource_groups.list():
        print(resource_group.name)

```

## <a name="mgmt-auth-cli"></a><span data-ttu-id="13e4f-127">CLI ベースの認証</span><span class="sxs-lookup"><span data-stu-id="13e4f-127">CLI-based authentication</span></span>

<span data-ttu-id="13e4f-128">SDK では、CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="13e4f-128">The SDK is able to create a client using your CLI active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="13e4f-129">これは、すばやく開始できる開発者エクスペリエンスとして使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="13e4f-129">This should be used as quick start developer experience.</span></span> <span data-ttu-id="13e4f-130">運用環境では、[ADAL](#authenticate-with-token-credentials) か独自の資格情報システムを使用してください。</span><span class="sxs-lookup"><span data-stu-id="13e4f-130">For production purposes, use [ADAL](#authenticate-with-token-credentials) or your own credentials system.</span></span>
> <span data-ttu-id="13e4f-131">CLI 構成に変更を加えると、SDK の実行に影響します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-131">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="13e4f-132">アクティブな資格情報を定義するには、[az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) を使用します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-132">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="13e4f-133">既定のサブスクリプション ID は、1 つしかない場合はその値であり、そうでない場合は [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) を使用して定義できます。</span><span class="sxs-lookup"><span data-stu-id="13e4f-133">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="13e4f-134">トークン資格情報による認証 (従来の方法)</span><span class="sxs-lookup"><span data-stu-id="13e4f-134">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="13e4f-135">SDK の以前のバージョンでは、ADAL はまだ利用できず、`UserPassCredentials` クラスが用意されていました。</span><span class="sxs-lookup"><span data-stu-id="13e4f-135">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="13e4f-136">これは非推奨とされており、今後は使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="13e4f-136">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="13e4f-137">このサンプルでは、ユーザー/パスワードのシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="13e4f-137">This sample shows user/password scenario.</span></span> <span data-ttu-id="13e4f-138">ここでは 2FA はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="13e4f-138">This does not support 2FA.</span></span>

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```
