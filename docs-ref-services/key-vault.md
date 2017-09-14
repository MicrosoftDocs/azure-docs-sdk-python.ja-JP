---
title: "Python 用 Azure Key Vault ライブラリ"
description: "Python 用 Azure Key Vault クライアント ライブラリのリファレンス ドキュメント"
author: lisawong19
keywords: "Azure, Python, SDK, API, キー, Key Vault, 認証, シークレット, キー, セキュリティ"
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3eac46eb4d5d19273ead9f19b739f6fb6d72e5cc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="0d200-104">Python 用 Azure Key Vault ライブラリ</span><span class="sxs-lookup"><span data-stu-id="0d200-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="0d200-105">概要</span><span class="sxs-lookup"><span data-stu-id="0d200-105">Overview</span></span>

<span data-ttu-id="0d200-106">Azure Key Vault に格納されるキーとシークレットの作成、更新、削除は、クライアント ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="0d200-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="0d200-107">キー コンテナーの作成、アプリケーションの承認、アクセス許可の管理は、Azure Key Vault 管理ライブラリを使って行います。</span><span class="sxs-lookup"><span data-stu-id="0d200-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="0d200-108">[Azure Key Vault](/azure/key-vault/key-vault-whatis) の詳細はこちらです。</span><span class="sxs-lookup"><span data-stu-id="0d200-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="0d200-109">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="0d200-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="0d200-110">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="0d200-110">Client library</span></span>
```bash
pip install azure-keyvault
```

## <a name="example"></a><span data-ttu-id="0d200-111">例</span><span class="sxs-lookup"><span data-stu-id="0d200-111">Example</span></span>
<span data-ttu-id="0d200-112">キー コンテナーから [JSON Web キー](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18)を取得します。</span><span class="sxs-lookup"><span data-stu-id="0d200-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[<span data-ttu-id="0d200-113">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="0d200-113">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/clientlibrary)

### <a name="management-api"></a><span data-ttu-id="0d200-114">Management API</span><span class="sxs-lookup"><span data-stu-id="0d200-114">Management API</span></span>
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="0d200-115">例</span><span class="sxs-lookup"><span data-stu-id="0d200-115">Example</span></span>
<span data-ttu-id="0d200-116">次の例は、Azure Key Vault を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0d200-116">The following example shows how to create an Azure Key Vault.</span></span> 

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
> [<span data-ttu-id="0d200-117">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="0d200-117">Explore the Management APIs</span></span>](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [<span data-ttu-id="0d200-118">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="0d200-118">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/managementlibrary)

## <a name="samples"></a><span data-ttu-id="0d200-119">サンプル</span><span class="sxs-lookup"><span data-stu-id="0d200-119">Samples</span></span>
* <span data-ttu-id="0d200-120">[キー コンテナーの管理][1]</span><span class="sxs-lookup"><span data-stu-id="0d200-120">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="0d200-121">[キー コンテナーの回復][2]</span><span class="sxs-lookup"><span data-stu-id="0d200-121">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="0d200-122">Azure Key Vault のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)を表示します。</span><span class="sxs-lookup"><span data-stu-id="0d200-122">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 