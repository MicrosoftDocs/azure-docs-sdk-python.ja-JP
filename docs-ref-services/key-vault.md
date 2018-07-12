---
title: Python 用 Azure Key Vault ライブラリ
description: Python 用 Azure Key Vault クライアント ライブラリのリファレンス ドキュメント
author: lisawong19
keywords: Azure, Python, SDK, API, キー, Key Vault, 認証, シークレット, キー, セキュリティ
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 555f55dcf7355a1a82dc3ca5826e0d0bd3fad414
ms.sourcegitcommit: 8476146ae9bcd1533db47adbe2524b27b93aaba0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2018
ms.locfileid: "37925943"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="c3a48-104">Python 用 Azure Key Vault ライブラリ</span><span class="sxs-lookup"><span data-stu-id="c3a48-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="c3a48-105">概要</span><span class="sxs-lookup"><span data-stu-id="c3a48-105">Overview</span></span>

<span data-ttu-id="c3a48-106">Azure Key Vault に格納されるキーとシークレットの作成、更新、削除は、クライアント ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="c3a48-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="c3a48-107">キー コンテナーの作成、アプリケーションの承認、アクセス許可の管理は、Azure Key Vault 管理ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="c3a48-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="c3a48-108">[Azure Key Vault](/azure/key-vault/key-vault-whatis) の詳細はこちらです。</span><span class="sxs-lookup"><span data-stu-id="c3a48-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="c3a48-109">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="c3a48-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="c3a48-110">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="c3a48-110">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="c3a48-111">例</span><span class="sxs-lookup"><span data-stu-id="c3a48-111">Examples</span></span>

<span data-ttu-id="c3a48-112">キー コンテナーから [JSON Web キー](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)を取得します。</span><span class="sxs-lookup"><span data-stu-id="c3a48-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```

<span data-ttu-id="c3a48-113">同様に、次のスニペットを使用して、コンテナーからシークレットを取得できます。</span><span class="sxs-lookup"><span data-stu-id="c3a48-113">Similarly, you can use the following snippet to retrieve a secret from the vault:</span></span>

```
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '',
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

secret_bundle = client.get_secret("https://VAULT_ID.vault.azure.net/", "SECRET_ID", "SECRET_VERSION")

print(secret_bundle.value)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3a48-114">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="c3a48-114">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a><span data-ttu-id="c3a48-115">管理 API</span><span class="sxs-lookup"><span data-stu-id="c3a48-115">Management API</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="c3a48-116">例</span><span class="sxs-lookup"><span data-stu-id="c3a48-116">Example</span></span>
<span data-ttu-id="c3a48-117">次の例は、Azure Key Vault を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c3a48-117">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="c3a48-118">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="c3a48-118">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3a48-119">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="c3a48-119">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="c3a48-120">サンプル</span><span class="sxs-lookup"><span data-stu-id="c3a48-120">Samples</span></span>
* <span data-ttu-id="c3a48-121">[キー コンテナーの管理][1]</span><span class="sxs-lookup"><span data-stu-id="c3a48-121">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="c3a48-122">[キー コンテナーの回復][2]</span><span class="sxs-lookup"><span data-stu-id="c3a48-122">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="c3a48-123">Azure Key Vault のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)を表示します。</span><span class="sxs-lookup"><span data-stu-id="c3a48-123">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
