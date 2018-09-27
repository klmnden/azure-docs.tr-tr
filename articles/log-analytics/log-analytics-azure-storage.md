---
title: Azure Hizmet Günlükleri ve ölçümleri Log Analytics'e toplama | Microsoft Docs
description: Tanılama günlükleri ve ölçümleri Log Analytics'e yazmak için Azure kaynaklarını yapılandırın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 297b3f4c9ef110f8adc9dcb5cd9eac9e30729a5d
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47180134"
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Azure Hizmet Günlükleri ve Log Analytics kullanım ölçümlerini Topla

Toplama günlükleri ve ölçümleri Azure Hizmetleri için dört farklı yolu vardır:

1. Azure tanılama verilerini doğrudan Log Analytics'e (*tanılama* aşağıdaki tabloda)
2. Log Analytics için Azure depolama için Azure tanılama (*depolama* aşağıdaki tabloda)
3. Azure Hizmetleri için bağlayıcılar (*Bağlayıcılar* aşağıdaki tabloda)
4. Toplamak ve daha sonra Log Analytics (boş olanları aşağıdaki tabloda ve listelenmeyen Hizmetleri) içine veri göndermek için komut dosyaları


| Hizmet                 | Kaynak Türü                           | Günlükler        | Ölçümler     | Çözüm |
| --- | --- | --- | --- | --- |
| Uygulama ağ geçitleri    | Microsoft.Network/applicationGateways   | Tanılama | Tanılama | [Azure uygulama ağ geçidi analizi](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application Insights    |                                         | Bağlayıcı   | Bağlayıcı   | [Application Insights Bağlayıcısı](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Önizleme) |
| Automation hesapları     | Microsoft.Automation/AutomationAccounts | Tanılama |             | [Daha fazla bilgi](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch hesapları          | Microsoft.Batch/batchAccounts           | Tanılama | Tanılama | |
| Klasik bulut Hizmetleri  |                                         | Depolama     |             | [Daha fazla bilgi](log-analytics-azure-storage-iis-table.md) |
| Bilişsel hizmetler      | Microsoft.CognitiveServices/accounts    |             | Tanılama | |
| Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Tanılama |             | |
| Data Lake store         | Microsoft.DataLakeStore/accounts        | Tanılama |             | |
| Olay Hub'ı ad alanı     | Microsoft.EventHub/namespaces           | Tanılama | Tanılama | |
| IoT Hub                | Microsoft.Devices/ıothubs               |             | Tanılama | |
| Key Vault               | Microsoft.KeyVault/vaults               | Tanılama |             | [Anahtar kasası analizi](log-analytics-azure-key-vault.md) |
| Yük Dengeleyiciler          | Microsoft.Network/loadBalancers         | Tanılama |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Tanılama | Tanılama | |
| Ağ Güvenlik Grupları | Microsoft.Network/networksecuritygroups | Tanılama |             | [Azure ağ güvenlik grubu analizi](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Kurtarma kasaları         | Microsoft.RecoveryServices/vaults       |             |             | [Azure kurtarma Hizmetleri analizi (Önizleme)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Hizmet ara         | Microsoft.Search/searchServices         | Tanılama | Tanılama | |
| Service Bus ad alanı   | Microsoft.ServiceBus/namespaces         | Tanılama | Tanılama | [Service Bus analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Depolama     |             | [Service Fabric analizi (Önizleme)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Tanılama | [Azure SQL Analytics (Önizleme)](log-analytics-azure-sql.md) |
| Depolama                 |                                         |             | Betik      | [Azure depolama analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtual Machines        | Microsoft.Compute/virtualMachines       | Dahili numara   | Dahili numara <br> Tanılama  | |
| Sanal makine ölçek kümeleri | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Tanılama | |
| Web sunucu grupları        | Microsoft.Web/serverfarms               |             | Tanılama | |
| Web Siteleri               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Tanılama | [Azure Web Apps Analytics (Önizleme)](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-web-apps-analytics) |


> [!NOTE]
> Azure sanal makineleri (Linux ve Windows) izlemek için yüklemenizi öneririz [Log Analytics VM uzantısı](log-analytics-azure-vm-extension.md). Aracı, sanal makinelerinizin içinde toplanan Öngörüler sağlar. Uzantı sanal makine ölçek kümeleri için de kullanabilirsiniz.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Azure tanılama verilerini doğrudan Log Analytics'e bağlanma
Birçok Azure kaynağı tanılama günlükleri yazabiliyor ve doğrudan Log Analytics ve bunun için ölçümleri analiz için veri toplama tercih edilen yoludur. Azure Tanılama'yı kullanarak, verileri Log Analytics'e hemen yazılır ve ilk veri depolama alanına yazmaya gerek yoktur.

Destekleyen azure kaynak [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) kendi günlükleri ve ölçümleri doğrudan Log Analytics'e gönderebilirsiniz.

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla Log Analytics’e gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, ölçüm tamamında tüm gelen iletiler halinde gösterilir tanılama ayarları aracılığıyla dışarı aktardığınızda olay hub'ı sıralar.
>
>

* Kullanılabilir ölçümler ayrıntılarını başvurmak [ölçümleri Azure İzleyici ile desteklenen](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Kullanılabilir günlükleri ayrıntılarını başvurmak [desteklenen Hizmetleri ve şema için tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>PowerShell ile tanılamayı etkinleştirme
Kasım 2016 (v2.3.0) gereksinim veya sonraki sürümünün [Azure PowerShell](/powershell/azure/overview).

Aşağıdaki PowerShell örneği kullanma işlemini gösterir [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) bir ağ güvenlik grubu tanılamayı etkinleştirmek için. Desteklenen tüm kaynaklar için aynı yaklaşımı çalışır - Ayarla `$resourceId` tanılama için etkinleştirmek istediğiniz kaynağının kaynak kimliği.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Resource Manager şablonları ile tanılamayı etkinleştirme

Oluşturulduktan ve Log Analytics çalışma alanınıza tanılama gönderdiğiniz bir kaynak üzerinde tanılamayı etkinleştirmek için aşağıdaki gibi bir şablon kullanabilirsiniz. Bu örnek bir Otomasyon hesabı, ancak tüm desteklenen kaynak türleri için çalışır içindir.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Azure tanılama depolama sonra Log Analytics

Bazı kaynaklar içinde günlüklerinden toplamak için Azure depolama birimine günlük gönderme ve ardından Log Analytics'i depolamadan günlükleri okuyacak şekilde yapılandırmak mümkündür.

Log Analytics, günlükleri ve aşağıdaki kaynaklar için Azure depolama biriminden tanılama toplamak için bu yaklaşımı kullanabilirsiniz:

| Kaynak | Günlükler |
| --- | --- |
| Service Fabric |ETWEvent <br> Operasyonel olay <br> Güvenilir aktör olay <br> Güvenilir hizmet olayı |
| Virtual Machines |Linux Syslog <br> Windows olay <br> IIS günlüğü <br> Windows ETWEvent |
| Web rolleri <br> Çalışan rolleri |Linux Syslog <br> Windows olay <br> IIS günlüğü <br> Windows ETWEvent |

> [!NOTE]
> Normal Azure veri ücretleri depolama ve bir depolama hesabına tanılama gönderdiğinizde, işlemler için ve ne zaman Log Analytics depolama hesabınızdan veri okuma için ücretlendirilir.
>
>

Bkz: [olaylar için IIS ve tablo depolama için blob depolama alanını kullanın](log-analytics-azure-storage-iis-table.md) nasıl Log Analytics Bu günlükleri toplayabilir hakkında daha fazla bilgi edinmek için.

## <a name="connectors-for-azure-services"></a>Azure Hizmetleri için bağlayıcılar

Application ınsights, Log Analytics'e gönderilmesi için Application Insights tarafından toplanan verileri sağlayan bir bağlayıcı vardır.

Daha fazla bilgi edinin [Application Insights Bağlayıcısı](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics"></a>Komut toplamak ve Log Analytics'e gönderme verisi

Günlükleri ve ölçümleri Log Analytics'e göndermek için doğrudan bir yol sağlamaz Azure Hizmetleri için günlük ve ölçümleri toplamak için bir Azure Otomasyonu betiği kullanabilirsiniz. Betik daha sonra verileri Log Analytics kullanarak gönderebilirsiniz [veri toplayıcı API'si](log-analytics-data-collector-api.md)

Azure şablonu galeri sahip [Azure Otomasyonu kullanma örnekleri](https://azure.microsoft.com/resources/templates/?term=OMS) Hizmetleri ve Log Analytics'e gönderme verileri toplamak için.

## <a name="next-steps"></a>Sonraki adımlar

* [Olaylar için IIS ve tablo depolama için BLOB Depolama kullanma](log-analytics-azure-storage-iis-table.md) Azure tablo depolama veya BLOB depolamaya yazılan IIS günlükler, yazma tanılama Hizmetleri için günlüklerini okumak için.
* [Çözümlerle](log-analytics-add-solutions.md) veri Öngörüler sağlar.
* [Arama sorguları kullanılır](log-analytics-log-searches.md) verileri çözümlemek için.
