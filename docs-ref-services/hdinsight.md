---
title: Azure HDInsight Python SDK (プレビュー)
description: Azure HDInsight Python SDK のリファレンス。 HDInsight Python SDK には、HDInsight クラスターの管理に使用できるクラスとメソッドが用意されています。
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 42e1e36b5854fda93188564be3ed3064b9ba4435
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277472"
---
# <a name="hdinsight-python-management-sdk-preview"></a>HDInsight Python Management SDK (プレビュー)

## <a name="overview"></a>概要

HDInsight Python SDK には、HDInsight クラスターの管理に使用できるクラスとメソッドが用意されています。 これには、スクリプト アクションを作成、削除、更新、一覧表示、スケーリング、実行したり、HDInsight クラスターのプロパティを監視、取得したりする操作が含まれます。

## <a name="prerequisites"></a>前提条件

* Azure アカウント。 所有していない場合は、[無料試用版を入手](https://azure.microsoft.com/free/)してください。
* [Python](https://www.python.org/downloads/)
* [pip](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a>SDK のインストール

HDInsight Python SDK は、[Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) にあり、次のコマンドを実行してインストールできます。 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a>Authentication

SDK は最初に Azure サブスクリプションで認証する必要があります。  以下の例に従って、サービス プリンシパルを作成し、これを使用して認証します。 その後、`HDInsightManagementClient` のインスタンスが生成されます。これには、管理操作の実行に使用できるメソッドが多数含まれています (以下のセクションで説明します)。

> [!NOTE]
> 認証方法は以下の例の他にもあり、そちらの方がご自身のニーズに適している可能性もあります。 すべての方法については、「[Python 用 Azure 管理ライブラリを使用した認証](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)」をご覧ください。

### <a name="authentication-example-using-a-service-principal"></a>サービス プリンシパルを使用した認証の例

まず、[Azure Cloud Shell](https://shell.azure.com/bash) にログインします。 現在、サービス プリンシパル作成対象のサブスクリプションを使用していることを確認します。 

```azurecli-interactive
az account show
```

ご自身のサブスクリプション情報が JSON として表示されます。

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

正しいサブスクリプションにログインしていない場合は、以下を実行して正しいサブスクリプションを選択します。 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> 他の方法で、たとえば Azure portal で HDInsight クラスターを作成する、といった方法で HDInsight リソース プロバイダーをまだ登録していない場合は、認証の前にこれを一度実行する必要があります。 これを [Azure Cloud Shell](https://shell.azure.com/bash) から実行するには、次のコマンドを実行します。
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

次に、ご自身のサービス プリンシパルの名前を選択し、次のコマンドを使用して作成します。

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

サービス プリンシパル情報が JSON として表示されます。

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
次の Python スニペットをコピーし、コマンド実行後に返された JSON の文字列を `TENANT_ID`、`CLIENT_ID`、`CLIENT_SECRET`、`SUBSCRIPTION_ID` に入力して、サービス プリンシパルを作成します。

```python
from azure.mgmt.hdinsight import HDInsightManagementClient
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.hdinsight.models import *

# Tenant ID for your Azure Subscription
TENANT_ID = ''
# Your Service Principal App Client ID
CLIENT_ID = ''
# Your Service Principal Client Secret
CLIENT_SECRET = ''
# Your Azure Subscription ID
SUBSCRIPTION_ID = ''

credentials = ServicePrincipalCredentials(
    client_id = CLIENT_ID,
    secret = CLIENT_SECRET,
    tenant = TENANT_ID
)

client = HDInsightManagementClient(credentials, SUBSCRIPTION_ID)
```


## <a name="cluster-management"></a>クラスターの管理

> [!NOTE]
> このセクションでは、`HDInsightManagementClient` インスタンスの認証と構成が既に完了していること、および `client` と呼ばれる変数にそのインスタンスが格納されていることを前提としています。 `HDInsightManagementClient` を認証および取得する方法については、上記の「認証」セクションを参照してください。

### <a name="create-a-cluster"></a>クラスターの作成

新しいクラスターを作成するには、`client.clusters.create()` を呼び出します。 

#### <a name="example"></a>例

この例は、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成する方法を示しています。

> [!NOTE]
> 次に示すように、最初にリソース グループとストレージ アカウントを作成する必要があります。 これらが既に作成済みの場合、この手順はスキップできます。

##### <a name="creating-a-resource-group"></a>リソース グループの作成

[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、リソース グループを作成できます
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>ストレージ アカウントの作成

[Azure Cloud Shell](https://shell.azure.com/bash) を使用して次を実行することで、ストレージ アカウントを作成できます。
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
ここで、次のコマンドを実行して、ストレージ アカウントに対するキーを取得します (これはクラスターを作成するときに必要になります)。
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
次の Python スニペットでは、2 つのヘッド ノードと 1 つの worker ノードを含む Spark クラスターを作成します。 コメントの説明に従って空白の変数を入力します。また、ご自身のニーズに合わせて他のパラメーターを変更します。

```python
# The name for the cluster you are creating
cluster_name = ""
# The name of your existing Resource Group
resource_group_name = ""
# Choose a username
username = ""
# Choose a password
password = ""
# Replace <> with the name of your storage account
storage_account = "<>.blob.core.windows.net"
# Storage account key you obtained above
storage_account_key = ""
# Choose a region
location = ""
container = "default"

params = ClusterCreateProperties(
    cluster_version="3.6",
    os_type=OSType.linux,
    tier=Tier.standard,
    cluster_definition=ClusterDefinition(
        kind="spark",
        configurations={
            "gateway": {
                "restAuthCredential.enabled_credential": "True",
                "restAuthCredential.username": username,
                "restAuthCredential.password": password
            }
        }
    ),
    compute_profile=ComputeProfile(
        roles=[
            Role(
                name="headnode",
                target_instance_count=2,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            ),
            Role(
                name="workernode",
                target_instance_count=1,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            )
        ]
    ),
    storage_profile=StorageProfile(
        storageaccounts=[StorageAccount(
            name=storage_account,
            key=storage_account_key,
            container=container,
            is_default=True
        )]
    )
)

client.clusters.create(
    cluster_name=cluster_name,
    resource_group_name=resource_group_name,
    parameters=ClusterCreateParametersExtended(
        location=location,
        tags={},
        properties=params
    ))
```

### <a name="get-cluster-details"></a>クラスターの詳細の取得

特定のクラスターのプロパティを取得するには:

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>例

`get` を使用すると、ご自身のクラスターを適切に作成できたことを確認できます。

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

出力は次のようになります。

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a>クラスターの一覧表示

#### <a name="list-clusters-under-the-subscription"></a>サブスクリプションのクラスターの一覧表示

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a>リソース グループ別のクラスターの一覧表示

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> `list()` と `list_by_resource_group()` の両方が `ClusterPaged` オブジェクトを返します。 `advance_page()` を呼び出すと、そのページ上にクラスターの一覧が返され、`ClusterPaged` オブジェクトが次のページに進められます。 これは、`StopIteration` 例外が発生し、それ以上ページがないことが示されるまで繰り返されます。

#### <a name="example"></a>例

次の例では、現在のサブスクリプションのすべてのクラスターのプロパティが出力されます。

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a>クラスターの削除

クラスターを削除するには:

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>クラスター タグの更新

次のように、特定のクラスターのタグを更新できます。

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a>例

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="scale-cluster"></a>クラスターのスケーリング

特定のクラスターの worker ノード数を増減するには、次のように新しいサイズを指定します。

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a>クラスターの監視

HDInsight 管理 SDK を使用して、Operations Management Suite (OMS) でご自身のクラスターの監視を管理することもできます。

### <a name="enable-oms-monitoring"></a>OMS 監視の有効化

> [!NOTE]
> OMS の監視を有効にするには、既存の Log Analytics ワークスペースが必要です。 まだ作成していない場合、その方法については、「[Azure ポータルで Log Analytics ワークスペースを作成する](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace)」を参照してください。

ご自身のクラスターで OMS 監視を有効にするには:

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a>OMS 監視の状態の表示

ご自身のクラスターに対する OMS の状態を取得するには:

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>OMS 監視の無効化

ご自身のクラスターに対する OMS を無効にするには:

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a>[スクリプト操作]

HDInsight には、クラスターをカスタマイズするためにカスタム スクリプトを呼び出すスクリプト アクションという構成方法があります。
> [!NOTE]
> スクリプト アクションを使用する方法の詳細については、「[スクリプト アクションを使用して Linux ベースの HDInsight クラスターをカスタマイズする](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)」を参照してください

### <a name="execute-script-actions"></a>スクリプト アクションの実行
特定のクラスターに対してスクリプト アクションを実行するには:

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a>スクリプト アクションの削除

特定のクラスターに対して指定した保存済みスクリプト アクションを削除するには:

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>保存済みスクリプト アクションの一覧表示

> [!NOTE]
> `list()` と `list_persisted_scripts()` は `RuntimeScriptActionDetailPaged` オブジェクトを返します。 `advance_page()` を呼び出すと、そのページ上に `RuntimeScriptActionDetailPaged` の一覧が返され、`RuntimeScriptActionDetail` オブジェクトが次のページに進められます。 これは、`StopIteration` 例外が発生し、それ以上ページがないことが示されるまで繰り返されます。 次の例を見てください。

指定したクラスターに対する保存済みスクリプト アクションを一覧表示するには:
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>例

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a>スクリプトの全実行履歴の一覧表示

指定したクラスターに対するスクリプトの実行履歴をすべて一覧表示するには:

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>例

この例では、過去のすべてのスクリプト実行の詳細が出力されます。

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
