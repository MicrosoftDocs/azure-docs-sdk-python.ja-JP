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
# <a name="azure-key-vault-libraries-for-python"></a>Python 用 Azure Key Vault ライブラリ

[Azure Key Vault](/azure/key-vault/) は、暗号化キー、シークレット、および証明書の管理用の Azure のストレージと管理のシステムです。 Key Vault 用の Python SDK API は、クライアント ライブラリと管理ライブラリに分割されます。

以下の場合にクライアント ライブラリを使用します。
- Azure Key Vault に格納されている項目のアクセス、更新、または削除
- 格納されている証明書のメタデータの取得
- Key Vault 内の対称キーに対する署名の確認

以下の場合に管理ライブラリを使用します。
- 新しい Key Vault ストアの作成、更新、または削除
- コンテナーのアクセス ポリシーの制御
- サブスクリプションまたはリソース グループによるコンテナーの一覧表示
- コンテナー名の利用可能性の確認

## <a name="install-the-libraries"></a>ライブラリをインストールする

### <a name="client-library"></a>クライアント ライブラリ

```bash
pip install azure-keyvault
```

## <a name="examples"></a>例

次の例では、Azure に接続するアプリケーションの推奨されるサインイン方式である、サービス プリンシパル認証を使用しています。 サービス プリンシパル認証の詳細については、「[Python 用 Azure 管理ライブラリを使用した認証](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)」を参照してください。

コンテナーからの非対称キーの公開部分を取得します。

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

コンテナーからシークレットを取得します。

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
> [クライアント API を探す](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a>管理ライブラリ

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>例

次の例は、Azure Key Vault を作成する方法を示します。 

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
> [Management API を探す](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>サンプル
* [Azure Key Vault の管理][1] 
* [Azure Key Vault の回復][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

Azure Key Vault のサンプルの[完全な一覧](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault)を表示します。 
