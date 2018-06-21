---
title: Python 用 Azure 管理ライブラリを使用した認証
description: Python 用 Azure 管理ライブラリへのサービス プリンシパルを使用して認証を行います
keywords: Azure, Python, SDK, API, 認証, active directory, サービス プリンシパル
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 78b248071e4718c1ab5ad743e697eafcfb510ec5
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2018
ms.locfileid: "35720053"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>Python 用 Azure 管理ライブラリを使用した認証

Python 管理ライブラリを使用してリソースの作成と管理を行うときに、Azure に対してアプリケーションを認証する方法としては、いくつかの選択肢があります。

## <a name="mgmt-auth-token"></a>トークン資格情報による認証

構成ファイル、レジストリ、または Azure Key Vault に、資格情報を安全に格納します。

次の例では、認証に[サービス プリンシパル](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)を使用します。

> [!NOTE]
> Azure CLI 2.0 を通じてサービス プリンシパルを作成できます
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

> [注!] いずれかの Azure ソブリン クラウドに接続するには、`cloud_environment` パラメーターを使用します。

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

その後、クライアント ファクトリを使用して任意のクライアントを作成できます。
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>管理対象のサービス ID (MSI) による認証 
MSI を使えば、Azure のリソースで SDK/CLI を簡単に使用できます。特定の資格情報の作成は必要ありません。

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

SDK では、CLI のアクティブなサブスクリプションを使用してクライアントを作成できます。

> [!IMPORTANT]
> これは、すばやく開始できる開発者エクスペリエンスとして使用することをお勧めします。 運用環境では、[ADAL](#authenticate-with-token-credentials) か独自の資格情報システムを使用してください。
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
