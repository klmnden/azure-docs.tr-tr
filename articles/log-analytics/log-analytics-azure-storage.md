---
title: "Günlük analizi için Azure Hizmeti günlüklerini ve ölçümleri toplamak | Microsoft Docs"
description: "Tanılama günlüklerini ve ölçümleri için günlük analizi yazmak için Azure kaynaklarını yapılandırın."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a3785e39f0d1cf849dbbf0d83d89eaed58c5b0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Azure Hizmeti günlüklerini ve günlük analizi kullanımda ölçümlerini Topla

Günlükleri ve Azure Hizmetleri için ölçümleri toplama dört farklı yolu vardır:

1. Azure tanılama yönlendirmek için günlük analizi (*tanılama* aşağıdaki tabloda)
2. Azure günlük analizi depolama için Azure tanılama (*depolama* aşağıdaki tabloda)
3. Azure Hizmetleri için bağlayıcıları (*Bağlayıcılar* aşağıdaki tabloda)
4. Toplamak ve günlük analizi (boşlukları aşağıdaki tabloda ve için listelenmeyen Hizmetleri) içine veri göndermek için komut dosyaları


| Hizmet                 | Kaynak Türü                           | Günlükler        | Ölçümler     | Çözüm |
| --- | --- | --- | --- | --- |
| Uygulama ağ geçitleri    | Microsoft.Network/applicationGateways   | Tanılama | Tanılama | [Azure uygulama ağ geçidi analizi](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application ınsights    |                                         | Bağlayıcı   | Bağlayıcı   | [Uygulama Öngörüler Bağlayıcısı](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Önizleme) |
| Automation hesapları     | Microsoft.Automation/AutomationAccounts | Tanılama |             | [Daha fazla bilgi](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Toplu hesaplar          | Microsoft.Batch/batchAccounts           | Tanılama | Tanılama | |
| Klasik bulut Hizmetleri  |                                         | Depolama     |             | [Daha fazla bilgi](log-analytics-azure-storage-iis-table.md) |
| Bilişsel Hizmetler      | Microsoft.CognitiveServices/accounts    |             | Tanılama | |
| Data Lake analizi     | Microsoft.DataLakeAnalytics/accounts    | Tanılama |             | |
| Veri Gölü deposu         | Microsoft.DataLakeStore/accounts        | Tanılama |             | |
| Olay Hub'ad alanı     | Microsoft.EventHub/namespaces           | Tanılama | Tanılama | |
| IOT hub'ları                | Microsoft.Devices/IotHubs               |             | Tanılama | |
| Anahtar Kasası               | Microsoft.KeyVault/vaults               | Tanılama |             | [KeyVault analizi](log-analytics-azure-key-vault.md) |
| Yük Dengeleyici          | Microsoft.Network/loadBalancers         | Tanılama |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Tanılama | Tanılama | |
| Ağ Güvenlik Grupları | Microsoft.Network/networksecuritygroups | Tanılama |             | [Azure ağ güvenlik grubu analizi](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Kurtarma kasaları         | Microsoft.RecoveryServices/vaults       |             |             | [Analytics (Önizleme) Azure kurtarma Hizmetleri](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Hizmet ara         | Microsoft.Search/searchServices         | Tanılama | Tanılama | |
| Hizmet veri yolu ad alanı   | Microsoft.ServiceBus/namespaces         | Tanılama | Tanılama | [Hizmet veri yolu Analytics (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Depolama     |             | [Service Fabric Analytics (Önizleme)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Tanılama | [Azure SQL analizi (Önizleme)](log-analytics-azure-sql.md) |
| Depolama                 |                                         |             | Betik      | [Azure depolama çözümlemeleri (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtual Machines        | Microsoft.Compute/virtualMachines       | Dahili numara   | Dahili numara <br> Tanılama  | |
| Sanal makine ölçek kümeleri | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Tanılama | |
| Web sunucu grupları        | Microsoft.Web/serverfarms               |             | Tanılama | |
| Web Siteleri               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Tanılama | [Azure Web uygulamaları analizi (Önizleme)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) |


> [!NOTE]
> Azure sanal makine (Linux ve Windows'da) izlemek için yüklemenizi öneririz [günlük analizi VM uzantısı](log-analytics-azure-vm-extension.md). Aracı sanal makinelerinizin içinde toplanan bilgiler sağlar. Uzantı sanal makine ölçek kümeleri için de kullanabilirsiniz.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Azure tanılama günlük analizi için doğrudan
Birçok Azure kaynaklarını tanılama günlüklerini yazabilmesi ve ölçümlere doğrudan günlük analizi ve bu analiz için veri toplamanın tercih edilen yöntemdir. Azure tanılama kullanırken, veriler için günlük analizi hemen yazılır ve ilk veri depolama alanına yazmak için gerek yoktur.

Destek azure kaynaklarını [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) kendi günlüklerini ve ölçümleri doğrudan günlük analizi gönderebilirsiniz.

* Kullanılabilir ölçümler ayrıntılarını başvurmak [ölçümleri Azure İzleyicisi ile desteklenen](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Kullanılabilir günlüklerini ayrıntılarını başvurmak [desteklenen Hizmetleri ve şema için tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>PowerShell ile tanılamayı etkinleştirme
Kasım 2016 (v2.3.0) gerekir veya sonraki sürümünün [Azure PowerShell](/powershell/azure/overview).

Aşağıdaki PowerShell örneğinde nasıl kullanılacağını gösterir [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) bir ağ güvenlik grubu üzerinde tanılamayı etkinleştirmek için. Desteklenen tüm kaynaklar için aynı yaklaşımı çalışır - ayarlamak `$resourceId` tanılama için etkinleştirmek istediğiniz kaynağının kaynak kimliği.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Resource Manager şablonları ile tanılamayı etkinleştirin

Oluşturulur ve tanılama için günlük analizi çalışma alanınız gönderdiğiniz bir kaynakta tanılama etkinleştirmek için aşağıdaki benzer bir şablon kullanabilirsiniz. Bu örnek bir Otomasyon hesabı, ancak tüm desteklenen kaynak türleri için çalışır içindir.

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

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Azure tanılama depolama sonra günlük analizi

Bazı kaynaklar içinde günlüklerinden toplamak için Azure depolama alanına günlükleri göndermek ve depolama biriminden günlüklerini okumak için günlük analizi yapılandırmak da mümkündür.

Günlük analizi, tanılama günlüklerini ve aşağıdaki kaynaklar için Azure depolama toplamak için bu yaklaşım kullanabilirsiniz:

| Kaynak | Günlükler |
| --- | --- |
| Service Fabric |ETWEvent <br> İşlem olayı <br> Güvenilir aktör olay <br> Güvenilir hizmet olayı |
| Virtual Machines |Linux Syslog <br> Windows olayı <br> IIS günlüğü <br> Windows ETWEvent |
| Web rolleri <br> Çalışan rolleri |Linux Syslog <br> Windows olayı <br> IIS günlüğü <br> Windows ETWEvent |

> [!NOTE]
> Azure normal veri ücretleri depolama ve bir depolama hesabına tanılama gönderdiğinizde, işlemler ve ne zaman veri günlük analizi depolama hesabınızdan okur için sizden ücret kesilir.
>
>

Bkz: [olayları için IIS ve tablo depolama alanı için kullanım blob depolama](log-analytics-azure-storage-iis-table.md) günlük analizi Bu günlükler nasıl toplayabilirsiniz hakkında daha fazla bilgi edinmek için.

## <a name="connectors-for-azure-services"></a>Azure Hizmetleri için bağlayıcılar

Günlük analizi için gönderilecek Application Insights tarafından toplanan verilerin izin veren Application Insights için bir bağlayıcı yoktur.

Daha fazla bilgi edinmek [Application Insights Bağlayıcısı](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics"></a>Toplamak ve günlük analizi veri göndermek için komut dosyaları

Günlük analizi günlüklerini ve ölçümleri göndermek için doğrudan bir yol sağlamaz Azure Hizmetleri için günlük ve ölçümleri toplamak için bir Azure Otomasyonu komut dosyası kullanabilirsiniz. Komut dosyası sonra verileri günlük analizi kullanarak gönderebileceğini [veri toplayıcı API](log-analytics-data-collector-api.md)

Azure Şablon Galerisi sahip [Azure otomasyon kullanma örnekleri](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) Hizmetleri ve günlük analizi için gönderme veri toplamak için.

## <a name="next-steps"></a>Sonraki adımlar

* [Olaylar için IIS ve tablo depolama için BLOB storage kullanma](log-analytics-azure-storage-iis-table.md) o yazma tanılama tablo depolamaya veya blob depolamaya yazılabilir IIS günlükleri için Azure Hizmetleri için günlükleri okunamıyor.
* [Çözümlerle](log-analytics-add-solutions.md) verileri bir anlayış sağlamak için.
* [Arama sorguları kullanmak](log-analytics-log-searches.md) verileri çözümlemek için.
