---
title: Python 用 Azure Key Vault ライブラリ
description: Python 用 Azure Key Vault クライアント ライブラリのリファレンス ドキュメント
author: sptramer
manager: carmonm
ms.author: sttramer
ms.date: 06/10/2019
ms.topic: conceptual
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: f4661ee389c13ce8546e7b5cc8866ab7b216d3b0
ms.sourcegitcommit: 92fa5dbcfd9a20f4a49da5f4bdc03045783d3495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/15/2019
ms.locfileid: "67149334"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="5e8e4-103">Python 用 Azure Key Vault ライブラリ</span><span class="sxs-lookup"><span data-stu-id="5e8e4-103">Azure Key Vault libraries for Python</span></span>

<span data-ttu-id="5e8e4-104">[Azure Key Vault](/azure/key-vault/) は、暗号化キー、シークレット、および証明書の管理用の Azure のストレージと管理のシステムです。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-104">[Azure Key Vault](/azure/key-vault/) is Azure's storage and management system for cryptographic keys, secrets, and certificate management.</span></span> <span data-ttu-id="5e8e4-105">Key Vault 用の Python SDK API は、クライアント ライブラリと管理ライブラリに分割されます。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-105">The Python SDK API for Key Vault is split between client libraries and management libraries.</span></span>

<span data-ttu-id="5e8e4-106">以下の場合にクライアント ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-106">Use the client library to:</span></span>
- <span data-ttu-id="5e8e4-107">Azure Key Vault に格納されている項目のアクセス、更新、または削除</span><span class="sxs-lookup"><span data-stu-id="5e8e4-107">Access, update, or delete items stored in an Azure Key Vault</span></span>
- <span data-ttu-id="5e8e4-108">格納されている証明書のメタデータの取得</span><span class="sxs-lookup"><span data-stu-id="5e8e4-108">Get metadata for stored certificates</span></span>
- <span data-ttu-id="5e8e4-109">Key Vault 内の対称キーに対する署名の確認</span><span class="sxs-lookup"><span data-stu-id="5e8e4-109">Verify signatures against symmetric keys in Key Vault</span></span>

<span data-ttu-id="5e8e4-110">以下の場合に管理ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-110">Use the management library to:</span></span>
- <span data-ttu-id="5e8e4-111">新しい Key Vault ストアの作成、更新、または削除</span><span class="sxs-lookup"><span data-stu-id="5e8e4-111">Create, update, or delete new Key Vault stores</span></span>
- <span data-ttu-id="5e8e4-112">コンテナーのアクセス ポリシーの制御</span><span class="sxs-lookup"><span data-stu-id="5e8e4-112">Control vault access policies</span></span>
- <span data-ttu-id="5e8e4-113">サブスクリプションまたはリソース グループによるコンテナーの一覧表示</span><span class="sxs-lookup"><span data-stu-id="5e8e4-113">List vaults by subscription or resource group</span></span>
- <span data-ttu-id="5e8e4-114">コンテナー名の利用可能性の確認</span><span class="sxs-lookup"><span data-stu-id="5e8e4-114">Check for vault name availability</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="5e8e4-115">ライブラリをインストールする</span><span class="sxs-lookup"><span data-stu-id="5e8e4-115">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="5e8e4-116">クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="5e8e4-116">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="5e8e4-117">例</span><span class="sxs-lookup"><span data-stu-id="5e8e4-117">Examples</span></span>

<span data-ttu-id="5e8e4-118">次の例では、Azure に接続するアプリケーションの推奨されるサインイン方式である、サービス プリンシパル認証を使用しています。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-118">The following examples use service principal authentication, which is the recommended sign in method for applications that connect to Azure.</span></span> <span data-ttu-id="5e8e4-119">サービス プリンシパル認証の詳細については、「[Python 用 Azure 管理ライブラリを使用した認証](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-119">To learn about service principal authentication, see [Authenticate with the Azure SDK for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span></span>

<span data-ttu-id="5e8e4-120">コンテナーからの非対称キーの公開部分を取得します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-120">Retrieve the public portion of an asymmetric key from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# KEY_VERSION is required, and can be obtained with the KeyVaultClient.get_key_versions(self, vault_url, key_name) API
key_bundle = client.get_key(VAULT_URL, KEY_NAME, KEY_VERSION)
key = key_bundle.key
```

<span data-ttu-id="5e8e4-121">コンテナーからシークレットを取得します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-121">Retrieve a secret from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# SECRET_VERSION is required, and can be obtained with the KeyVaultClient.get_secret_versions(self, vault_url, secret_id) API
secret_bundle = client.get_secret(VAULT_URL, SECRET_ID, SECRET_VERSION)
secret = secret_bundle.value
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5e8e4-122">クライアント API を探す</span><span class="sxs-lookup"><span data-stu-id="5e8e4-122">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a><span data-ttu-id="5e8e4-123">管理ライブラリ</span><span class="sxs-lookup"><span data-stu-id="5e8e4-123">Management library</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="5e8e4-124">例</span><span class="sxs-lookup"><span data-stu-id="5e8e4-124">Example</span></span>

<span data-ttu-id="5e8e4-125">次の例は、Azure Key Vault を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-125">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient
from azure.common.credentials import ServicePrincipalCredentials


credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

# Even when using service principal credentials, a subscription ID is required. For service principals,
# this should be the subscription used to create the service principal. Storing a token like a valid
# subscription ID in code is not recommended and only shown here for example purposes.
SUBSCRIPTION_ID = '...'
client = KeyVaultManagementClient(credentials, SUBSCRIPTION_ID)

# The object ID and organization ID (tenant) of the user, application, or service principal for access policies.
# These values can be found through the Azure CLI or the Portal.
ALLOW_OBJECT_ID = '...'
ALLOW_TENANT_ID = '...'

RESOURCE_GROUP = '...'
VAULT_NAME = '...'

# Vault properties may also be created by using the azure.mgmt.keyvault.models.VaultCreateOrUpdateParameters
# class, rather than a map. 
operation = client.vaults.create_or_update(
    RESOURCE_GROUP,
    VAULT_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': TENANT_ID,
            'access_policies': [{
                'object_id': OBJECT_ID,
                'tenant_id': ALLOW_TENANT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)

vault = operation.result()
print(f'New vault URI: {vault.properties.vault_uri}')
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="5e8e4-126">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="5e8e4-126">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="5e8e4-127">サンプル</span><span class="sxs-lookup"><span data-stu-id="5e8e4-127">Samples</span></span>
* <span data-ttu-id="5e8e4-128">[Azure Key Vault の管理][1]</span><span class="sxs-lookup"><span data-stu-id="5e8e4-128">[Manage Azure Key Vaults][1]</span></span> 
* <span data-ttu-id="5e8e4-129">[Azure Key Vault の回復][2]</span><span class="sxs-lookup"><span data-stu-id="5e8e4-129">[Azure Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="5e8e4-130">Azure Key Vault のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)を表示します。</span><span class="sxs-lookup"><span data-stu-id="5e8e4-130">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 
