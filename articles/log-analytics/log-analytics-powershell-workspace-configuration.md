---
title: Oluşturma ve bir günlük analizi çalışma alanı yapılandırmak için PowerShell kullanın | Microsoft Docs
description: Altyapı Bulut veya şirket içi sunuculardan Analytics kullanım verilerini oturum. Azure tanılama tarafından oluşturulan zaman Azure depolama biriminden makine verilerini toplayabilir.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: 6a3f91323a017533d2d012f1e81760396c17a643
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30179098"
---
# <a name="manage-log-analytics-using-powershell"></a>PowerShell kullanarak Log Analytics’i yönetme
Kullanabileceğiniz [günlük analizi PowerShell cmdlet'leri](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) bir komut satırından veya bir komut dosyasının parçası olarak günlük analizi çeşitli işlevleri gerçekleştirmek için.  PowerShell ile gerçekleştirebileceğiniz görevler örnekleri şunlardır:

* Çalışma alanı oluşturma
* Bir çözüm Ekle Kaldır
* İçeri ve dışarı aktarma Kaydedilmiş aramaları
* Bir bilgisayar grubu oluşturun
* Windows aracısının yüklü olduğu IIS günlüklerinin bilgisayarlardan toplamayı etkinleştir
* Linux ve Windows bilgisayarlarından performans sayaçlarını Topla
* Linux bilgisayarlarda syslog olaylarını Topla 
* Windows olay günlüklerini olaylarını Topla
* Özel olay günlüklerini toplayın
* Bir Azure sanal makinesi için günlük analizi Aracısı Ekle
* Azure Tanılama'yı kullanarak toplanan dizin verileri için günlük analizi yapılandırın

Bu makale, bazı Powershell'den gerçekleştirebileceğiniz işlevlerin göstermek iki kod örnekleri sağlar.  Başvurabilirsiniz [günlük analizi PowerShell cmdlet başvurusunun](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) diğer işlevleri için.

> [!NOTE]
> Günlük analizi daha önce yer alan cmdlet'ler kullanılan ad olmasının olan Operational Insights çağrıldı.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu örnekler 2.3.0 sürümüyle ya da daha sonra AzureRm.OperationalInsights modülün çalışır.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Oluşturma ve yapılandırma günlük analizi çalışma alanı
Aşağıdaki komut dosyası örneği gösterilmektedir nasıl yapılır:

1. Çalışma alanı oluşturma
2. Var olan çözümlerinin listesi
3. Çalışma Alanı'na çözümleri Ekle
4. İçeri aktarma Kaydedilmiş aramaları
5. Dışarı aktarma Kaydedilmiş aramaları
6. Bir bilgisayar grubu oluşturun
7. Windows aracısının yüklü olduğu IIS günlüklerinin bilgisayarlardan toplamayı etkinleştir
8. Mantıksal Disk performans sayaçlarını Linux bilgisayarından toplar (% kullanılan Inode; Boş megabayt; % Kullanılan alan; Disk aktarımı/sn; Disk Okuma/sn; Disk Yazma/sn)
9. Linux bilgisayarlardan Syslog olaylarını Topla
10. Uygulama olay günlüğü'ndeki Windows bilgisayarlardan hata ve uyarı olaylarını Topla
11. Windows bilgisayarlardan bellek kullanılabilir MBayt performans sayacı Topla
12. Özel günlük Topla 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up to 5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Azure tanılama dizin için yapılandırma günlük analizi
Azure kaynaklarını aracısız izleme için kaynakları Azure tanılama etkin ve günlük analizi çalışma alanına yazmak için yapılandırılmış olması gerekir. Bu yaklaşım, doğrudan günlük analizi veri gönderir ve bir depolama hesabına yazılması için veri gerektirmez. Desteklenen kaynaklar şunlardır:

| Kaynak Türü | Günlükler | Ölçümler |
| --- | --- | --- |
| Application Gatewayler    | Evet | Evet |
| Automation hesapları     | Evet | |
| Batch hesapları          | Evet | Evet |
| Data Lake analizi     | Evet | | 
| Veri Gölü deposu         | Evet | |
| SQL esnek havuzu        |     | Evet |
| Olay Hub'ı ad alanı     |     | Evet |
| IoT Hub                |     | Evet |
| Anahtar Kasası               | Evet | |
| Yük Dengeleyiciler          | Evet | |
| Logic Apps              | Evet | Evet |
| Ağ Güvenlik Grupları | Evet | |
| Redis Önbelleği             |     | Evet |
| Hizmet ara         | Evet | Evet |
| Service Bus ad alanı   |     | Evet |
| SQL (v12)               |     | Evet |
| Web Siteleri               |     | Evet |
| Web sunucu grupları        |     | Evet |

Kullanılabilir ölçümler ayrıntılarını başvurmak [ölçümleri Azure İzleyicisi ile desteklenen](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

Kullanılabilir günlüklerini ayrıntılarını başvurmak [desteklenen Hizmetleri ve şema için tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Farklı Aboneliklerde bulunan kaynaklar günlükleri toplamak için önceki cmdlet'ini de kullanabilirsiniz. Cmdlet günlükleri ve günlükleri gönderilen çalışma alanı oluşturma her iki kaynağın kimliği sağlama abonelikler arasında çalışmaya yapabiliyor.


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a>Günlük analizi depolama biriminden Azure tanılama dizin için yapılandırma
Klasik bulut hizmeti ya da bir service fabric kümesi çalışan bir örneğini günlük verilerini toplamak için önce verileri Azure depolama alanına yazmak gerekir. Günlük analizi depolama hesabından günlükleri toplamak için sonra yapılandırılır. Desteklenen kaynaklar şunlardır:

* Klasik bulut Hizmetleri (web ve çalışan rolleri)
* Service fabric kümeleri

Aşağıdaki örnekte gösterildiği nasıl yapılır:

1. Günlük analizi veri dizinini oluşturacak konumları ve var olan depolama hesaplarını Listele
2. Bir depolama hesabından okumak için bir yapılandırma oluşturmak
3. Yeni oluşturulan yapılandırma veri dizini oluşturmak için ek konumlardan güncelleştirin.
4. Yeni oluşturulan yapılandırmasını silme

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

Yukarıdaki komut dosyası, farklı Aboneliklerdeki depolama hesaplarına günlükleri toplamak için de kullanabilirsiniz. Komut dosyası, depolama hesabı kaynak kimliği ile ilgili erişim tuşu sağlayarak abonelikler arasında çalışmaya yapabiliyor. Erişim anahtarı değiştirdiğinizde, yeni anahtar depolama Insight güncelleştirmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar
* [Günlük analizi PowerShell cmdlet'leri gözden](https://msdn.microsoft.com/library/mt188224\(v=azure.300\).aspx) günlük analizi yapılandırması için PowerShell kullanma hakkında ek bilgi için.

