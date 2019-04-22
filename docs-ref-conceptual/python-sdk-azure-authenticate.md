---
title: Python 用 Azure 管理ライブラリを使用した認証
description: Python 用 Azure 管理ライブラリへのサービス プリンシパルを使用して認証を行います
keywords: Azure, Python, SDK, API, 認証, active directory, サービス プリンシパル
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/11/2019
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 51f26b120cefffd2d7f4af9c2b6b2cb532bc6006
ms.sourcegitcommit: 375a1f9180eb1323fe2af0a7e28fd4676973c68e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59586810"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="bc587-104">Python 用 Azure 管理ライブラリを使用した認証</span><span class="sxs-lookup"><span data-stu-id="bc587-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="bc587-105">Python 管理ライブラリを使用してリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては、いくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="bc587-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="bc587-106">トークン資格情報による認証</span><span class="sxs-lookup"><span data-stu-id="bc587-106">Authenticate with token credentials</span></span>

<span data-ttu-id="bc587-107">構成ファイル、レジストリ、または Azure Key Vault に、資格情報を安全に格納します。</span><span class="sxs-lookup"><span data-stu-id="bc587-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="bc587-108">次の例では、認証に[サービス プリンシパル](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)を使用します。</span><span class="sxs-lookup"><span data-stu-id="bc587-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="bc587-109">Azure CLI でサービス プリンシパルを作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="bc587-109">To create a service principal with the Azure CLI, use the following command:</span></span>
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> <span data-ttu-id="bc587-110">CLI を使用したサービス プリンシパルの設定については、「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bc587-110">To learn more about setting up service princpals with the CLI, see [Create an Azure service principal with Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials

# Tenant ID for your Azure subscription
TENANT_ID = '<Your tenant ID>'

# Your service principal App ID
CLIENT = '<Your service principal ID>'

# Your service principal password
KEY = '<Your service principal password>'

credentials = ServicePrincipalCredentials(
    client_id = CLIENT,
    secret = KEY,
    tenant = TENANT_ID
)
```

> <span data-ttu-id="bc587-111">[注!] いずれかの Azure ソブリン クラウドに接続するには、`cloud_environment` パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="bc587-111">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>
>
> ```python
> from azure.common.credentials import ServicePrincipalCredentials
> from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
> 
> # Tenant ID for your Azure Subscription
> TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
> 
> # Your Service Principal App ID
> CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'
> 
> # Your Service Principal Password
> KEY = 'password'
> 
> credentials = ServicePrincipalCredentials(
>     client_id = CLIENT,
>     secret = KEY,
>     tenant = TENANT_ID,
>     cloud_environment = AZURE_CHINA_CLOUD
> )
> ```

<span data-ttu-id="bc587-112">より詳細な制御が必要な場合は、[ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) および SDK ADAL ラッパーを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bc587-112">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="bc587-113">利用可能なすべてのシナリオの一覧とサンプルについては、ADAL の Web サイトを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bc587-113">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="bc587-114">たとえば、サービス プリンシパル認証の場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="bc587-114">For instance for service principal authentication:</span></span>

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

<span data-ttu-id="bc587-115">すべての有効な ADAL 呼び出しは、`AdalAuthentication` クラスで使用できます。</span><span class="sxs-lookup"><span data-stu-id="bc587-115">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="bc587-116">次に、API の操作を開始するために、クライアント オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bc587-116">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="bc587-117">[注!] Azure ソブリン クラウドを使用する場合は、管理クライアントを作成するときに (`msrestazure.azure_cloud` の定数を通じて) 適切なベース URL も指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc587-117">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="bc587-118">たとえば、Azure China Cloud の場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="bc587-118">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="bc587-119">ファイル ベースの認証</span><span class="sxs-lookup"><span data-stu-id="bc587-119">File based authentication</span></span>

<span data-ttu-id="bc587-120">最も簡単な認証方法は、Azure サービス プリンシパルの資格情報が含まれた JSON ファイルを作成することです。</span><span class="sxs-lookup"><span data-stu-id="bc587-120">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="bc587-121">次の CLI コマンドを使用して、新しいサービス プリンシパルとこのファイルを同時に作成できます。</span><span class="sxs-lookup"><span data-stu-id="bc587-121">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="bc587-122">このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。</span><span class="sxs-lookup"><span data-stu-id="bc587-122">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="bc587-123">ご利用のシェルから、このファイルの完全なパスを保持する環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="bc587-123">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="bc587-124">自分でファイルを作成する場合は、次の形式に従ってください。</span><span class="sxs-lookup"><span data-stu-id="bc587-124">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "<Service principal ID>",
    "clientSecret": "<Service principal secret/password>",
    "subscriptionId": "<Subscription associated with the service principal>",
    "tenantId": "<The service principal's tenant>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="bc587-125">その後、クライアント ファクトリを使用して任意のクライアントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="bc587-125">You can then create any client using the client factory:</span></span>

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="bc587-126">Azure マネージド ID を使用して認証する</span><span class="sxs-lookup"><span data-stu-id="bc587-126">Authenticate with Azure Managed Identities</span></span>
<span data-ttu-id="bc587-127">Azure マネージド ID により、Azure のリソースで SDK/CLI を簡単に使用できます。特定の資格情報の作成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="bc587-127">Azure Managed Identity is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="bc587-128">マネージド ID を使用するには、Azure リソース (Azure 関数や、Azure で実行中の VM など) から Azure に接続している必要があります。</span><span class="sxs-lookup"><span data-stu-id="bc587-128">To use managed identities, you must be connecting to Azure from an Azure resource, such as an Azure Function or a VM running in Azure.</span></span> <span data-ttu-id="bc587-129">リソースにマネージド ID を構成する方法については、[Azure リソースのマネージド ID の構成](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)に関するページ、および[Azure リソースのマネージド ID を使用する方法](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="bc587-129">To learn how to configure a managed identity for a resource, see [Configure managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) and [How to use managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).</span></span>

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

## <a name="mgmt-auth-cli"></a><span data-ttu-id="bc587-130">CLI ベースの認証</span><span class="sxs-lookup"><span data-stu-id="bc587-130">CLI-based authentication</span></span>

<span data-ttu-id="bc587-131">SDK では、Azure CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="bc587-131">The SDK is able to create a client using the Azure CLI's active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc587-132">これは、すばやく開始できる開発者エクスペリエンスとして使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="bc587-132">This should be used as quick start developer experience.</span></span> <span data-ttu-id="bc587-133">運用環境では、[ADAL](#mgmt-auth-legacy) か独自の資格情報システムを使用してください。</span><span class="sxs-lookup"><span data-stu-id="bc587-133">For production purposes, use [ADAL](#mgmt-auth-legacy) or your own credentials system.</span></span>
> <span data-ttu-id="bc587-134">CLI 構成に変更を加えると、SDK の実行に影響します。</span><span class="sxs-lookup"><span data-stu-id="bc587-134">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="bc587-135">アクティブな資格情報を定義するには、[az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) を使用します。</span><span class="sxs-lookup"><span data-stu-id="bc587-135">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="bc587-136">既定のサブスクリプション ID は、1 つしかない場合はその値であり、そうでない場合は [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) を使用して定義できます。</span><span class="sxs-lookup"><span data-stu-id="bc587-136">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="bc587-137">トークン資格情報による認証 (従来の方法)</span><span class="sxs-lookup"><span data-stu-id="bc587-137">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="bc587-138">SDK の以前のバージョンでは、ADAL はまだ利用できず、`UserPassCredentials` クラスが用意されていました。</span><span class="sxs-lookup"><span data-stu-id="bc587-138">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="bc587-139">これは非推奨とされており、今後は使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="bc587-139">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="bc587-140">このサンプルでは、ユーザー/パスワードのシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="bc587-140">This sample shows user/password scenario.</span></span> <span data-ttu-id="bc587-141">ここでは 2FA はサポートされません。</span><span class="sxs-lookup"><span data-stu-id="bc587-141">This does not support 2FA.</span></span>

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
