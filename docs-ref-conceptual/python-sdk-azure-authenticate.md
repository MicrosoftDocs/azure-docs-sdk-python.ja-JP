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
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>Python 用 Azure 管理ライブラリを使用した認証

Python 管理ライブラリを使用してリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては、いくつかの選択肢があります。

## <a name="mgmt-auth-token"></a>トークン資格情報による認証

構成ファイル、レジストリ、または Azure Key Vault に、資格情報を安全に格納します。

次の例では、認証に[サービス プリンシパル](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)を使用します。

> [!NOTE]
> Azure CLI でサービス プリンシパルを作成するには、次のコマンドを使用します。
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> CLI を使用したサービス プリンシパルの設定については、「[Azure CLI で Azure サービス プリンシパルを作成する](/cli/azure/create-an-azure-service-principal-azure-cli)」をご覧ください。

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

> [注!] いずれかの Azure ソブリン クラウドに接続するには、`cloud_environment` パラメーターを使用します。
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

より詳細な制御が必要な場合は、[ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) および SDK ADAL ラッパーを使用することをお勧めします。 利用可能なすべてのシナリオの一覧とサンプルについては、ADAL の Web サイトを参照してください。 たとえば、サービス プリンシパル認証の場合は、次のようにします。

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

すべての有効な ADAL 呼び出しは、`AdalAuthentication` クラスで使用できます。

次に、API の操作を開始するために、クライアント オブジェクトを作成します。

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> [注!] Azure ソブリン クラウドを使用する場合は、管理クライアントを作成するときに (`msrestazure.azure_cloud` の定数を通じて) 適切なベース URL も指定する必要があります。 たとえば、Azure China Cloud の場合は、次のようにします。
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a>ファイル ベースの認証

最も簡単な認証方法は、Azure サービス プリンシパルの資格情報が含まれた JSON ファイルを作成することです。 次の CLI コマンドを使用して、新しいサービス プリンシパルとこのファイルを同時に作成できます。

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

このファイルは、コードで読み取ることができるシステム上の安全な場所に保存してください。 ご利用のシェルから、このファイルの完全なパスを保持する環境変数を設定します。

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

自分でファイルを作成する場合は、次の形式に従ってください。

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

その後、クライアント ファクトリを使用して任意のクライアントを作成できます。

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>Azure マネージド ID を使用して認証する
Azure マネージド ID により、Azure のリソースで SDK/CLI を簡単に使用できます。特定の資格情報の作成は必要ありません。

> [!IMPORTANT]
>
> マネージド ID を使用するには、Azure リソース (Azure 関数や、Azure で実行中の VM など) から Azure に接続している必要があります。 リソースにマネージド ID を構成する方法については、[Azure リソースのマネージド ID の構成](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)に関するページ、および[Azure リソースのマネージド ID を使用する方法](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)に関するページをご覧ください。

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

## <a name="mgmt-auth-cli"></a>CLI ベースの認証

SDK では、Azure CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。

> [!IMPORTANT]
> これは、すばやく開始できる開発者エクスペリエンスとして使用することをお勧めします。 運用環境では、[ADAL](#mgmt-auth-legacy) か独自の資格情報システムを使用してください。
> CLI 構成に変更を加えると、SDK の実行に影響します。

アクティブな資格情報を定義するには、[az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) を使用します。
既定のサブスクリプション ID は、1 つしかない場合はその値であり、そうでない場合は [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) を使用して定義できます。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a>トークン資格情報による認証 (従来の方法)

SDK の以前のバージョンでは、ADAL はまだ利用できず、`UserPassCredentials` クラスが用意されていました。 これは非推奨とされており、今後は使用しないでください。

このサンプルでは、ユーザー/パスワードのシナリオを示します。 ここでは 2FA はサポートされません。

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```
