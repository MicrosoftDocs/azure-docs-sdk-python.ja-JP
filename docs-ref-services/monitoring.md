---
title: "Python 用 Azure 監視ライブラリ"
description: "Python 用 Azure 監視ライブラリのリファレンス"
keywords: "Azure, python, SDK, API, 監視"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 6a073f9943b1f5af962546931e9d13372720e193
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="azure-monitoring-libraries-for-python"></a><span data-ttu-id="8a5e6-104">Python 用 Azure 監視ライブラリ</span><span class="sxs-lookup"><span data-stu-id="8a5e6-104">Azure Monitoring libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="8a5e6-105">概要</span><span class="sxs-lookup"><span data-stu-id="8a5e6-105">Overview</span></span> 
<span data-ttu-id="8a5e6-106">監視では、アプリケーションを正常な状態で稼働させ続けるためのデータを取得できます。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-106">Monitoring provides data to ensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="8a5e6-107">また、潜在的な問題を防止したり、発生した問題をトラブルシューティングするのにも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-107">It also helps you to stave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="8a5e6-108">さらに、監視データを使用して、アプリケーションに関する深い洞察を得ることもできます。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-108">In addition, you can use monitoring data to gain deep insights about your application.</span></span> <span data-ttu-id="8a5e6-109">そのような知識は、アプリケーションのパフォーマンスや保守容易性を向上させたり、手作業での介入が必要な操作を自動化したりするうえで役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-109">That knowledge can help you to improve application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>

<span data-ttu-id="8a5e6-110">Azure Monitor の詳細については、[こちら](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-110">Learn more about Azure Monitor [here](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor).</span></span> 

## <a name="installation"></a><span data-ttu-id="8a5e6-111">インストール</span><span class="sxs-lookup"><span data-stu-id="8a5e6-111">Installation</span></span>
```bash
pip install azure-mgmt-monitor
```

## <a name="example---metrics"></a><span data-ttu-id="8a5e6-112">例 - メトリック</span><span class="sxs-lookup"><span data-stu-id="8a5e6-112">Example - Metrics</span></span>
<span data-ttu-id="8a5e6-113">このサンプルは、Azure 上のリソース (VM など) のメトリックを取得します。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-113">This sample obtains the metrics of a resource on Azure (VMs, etc.).</span></span> <span data-ttu-id="8a5e6-114">このサンプルには、少なくとも Python パッケージのバージョン 0.4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-114">This sample requires version 0.4.0 of the Python package at least.</span></span>

<span data-ttu-id="8a5e6-115">フィルターに使用できるキーワードの完全な一覧については、[こちら](https://msdn.microsoft.com/library/azure/mt743622.aspx)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-115">A complete list of available keywords for filters is available [here](https://msdn.microsoft.com/library/azure/mt743622.aspx).</span></span>

<span data-ttu-id="8a5e6-116">各種のリソースでサポートされているメトリックについては、[こちら](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-116">Supported metrics per resource type is available [here](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics).</span></span>

```python
import datetime
from azure.mgmt.monitor import MonitorManagementClient

# Get the ARM id of your resource. You might chose to do a "get"
# using the according management or to build the URL directly
# Example for a ARM VM
resource_id = (
    "subscriptions/{}/"
    "resourceGroups/{}/"
    "providers/Microsoft.Compute/virtualMachines/{}"
).format(subscription_id, resource_group_name, vm_name)

# create client
client = MonitorManagementClient(
    credentials,
    subscription_id
)

# You can get the available metrics of this specific resource
for metric in client.metric_definitions.list(resource_id):
    # azure.monitor.models.MetricDefinition
    print("{}: id={}, unit={}".format(
        metric.name.localized_value,
        metric.name.value,
        metric.unit
    ))

# Example of result for a VM:
# Percentage CPU: id=Percentage CPU, unit=Unit.percent
# Network In: id=Network In, unit=Unit.bytes
# Network Out: id=Network Out, unit=Unit.bytes
# Disk Read Bytes: id=Disk Read Bytes, unit=Unit.bytes
# Disk Write Bytes: id=Disk Write Bytes, unit=Unit.bytes
# Disk Read Operations/Sec: id=Disk Read Operations/Sec, unit=Unit.count_per_second
# Disk Write Operations/Sec: id=Disk Write Operations/Sec, unit=Unit.count_per_second

# Get CPU total of yesterday for this VM, by hour

today = datetime.datetime.now().date()
yesterday = today - datetime.timedelta(days=1)

metrics_data = client.metrics.list(
    resource_id,
    timespan="{}/{}".format(yesterday, today),
    interval='PT1H',
    metric='Percentage CPU',
    aggregation='Total'
)

for item in metrics_data.value:
    # azure.mgmt.monitor.models.Metric
    print("{} ({})".format(item.name.localized_value, item.unit.name))
    for timeserie in item.timeseries:
        for data in timeserie.data:
            # azure.mgmt.monitor.models.MetricData
            print("{}: {}".format(data.time_stamp, data.total))

# Example of result:
# Percentage CPU (percent)
# 2016-11-16 00:00:00+00:00: 72.0
# 2016-11-16 01:00:00+00:00: 90.59
# 2016-11-16 02:00:00+00:00: 60.58
# 2016-11-16 03:00:00+00:00: 65.78
# 2016-11-16 04:00:00+00:00: 43.96
# 2016-11-16 05:00:00+00:00: 43.96
# 2016-11-16 06:00:00+00:00: 114.9
# 2016-11-16 07:00:00+00:00: 45.4
```

## <a name="example---alerts"></a><span data-ttu-id="8a5e6-117">例 - アラート</span><span class="sxs-lookup"><span data-stu-id="8a5e6-117">Example - Alerts</span></span>
<span data-ttu-id="8a5e6-118">この例は、リソースの作成時にそれらに関するアラートを自動的に設定して、すべてのリソースを正しく監視する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-118">This example shows how to automatically set up alerts on your resources when they are created to ensure that all resources are monitored correctly.</span></span>

<span data-ttu-id="8a5e6-119">CPU 使用率についてのアラートを作成するために、VM 上にデータ ソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-119">Create a data source on a VM to alert on CPU usage:</span></span>
```python
from azure.mgmt.monitor import MonitorMgmtClient
from azure.mgmt.monitor.models import RuleMetricDataSource

resource_id = (
    "subscriptions/{}/"
    "resourceGroups/MonitorTestsDoNotDelete/"
    "providers/Microsoft.Compute/virtualMachines/MonitorTest"
).format(self.settings.SUBSCRIPTION_ID)

# create client
client = MonitorMgmtClient(
    credentials,
    subscription_id
)

# I need a subclass of "RuleDataSource"
data_source = RuleMetricDataSource(
    resource_uri = resource_id,
    metric_name = 'Percentage CPU'
)
```
<span data-ttu-id="8a5e6-120">過去 5 分間の VM の平均 CPU 使用率が 90% を上回る場合にトリガーされるしきい値条件を作成します (上記のデータ ソースを使用)。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-120">Create a threshold condition that triggers when the average CPU usage of a VM for the last 5 minutes is above 90% (using the preceding data source):</span></span>
```python
from azure.mgmt.monitor.models import ThresholdRuleCondition

# I need a subclasses of "RuleCondition"
rule_condition = ThresholdRuleCondition(
    data_source = data_source,
    operator = 'GreaterThanOrEqual',
    threshold = 90,
    window_size = 'PT5M',
    time_aggregation = 'Average'
)
```

<span data-ttu-id="8a5e6-121">電子メール アクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-121">Create an email action:</span></span>
```python
from azure.mgmt.monitor.models import RuleEmailAction

# I need a subclass of "RuleAction"
rule_action = RuleEmailAction(
    send_to_service_owners = True,
    custom_emails = [
        'monitoringemail@microsoft.com'
    ]
)
```

<span data-ttu-id="8a5e6-122">アラートを作成します。</span><span class="sxs-lookup"><span data-stu-id="8a5e6-122">Create the alert:</span></span>
```python
rule_name = 'MyPyTestAlertRule'
my_alert = client.alert_rules.create_or_update(
    group_name,
    rule_name,
    {
        'location': 'westus',
        'alert_rule_resource_name': rule_name,
        'description': 'Testing Alert rule creation',
        'is_enabled': True,
        'condition': rule_condition,
        'actions': [
            rule_action
        ]
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="8a5e6-123">Management API を探す</span><span class="sxs-lookup"><span data-stu-id="8a5e6-123">Explore the Management APIs</span></span>](/python/api/overview/azure/monitoring/management)
