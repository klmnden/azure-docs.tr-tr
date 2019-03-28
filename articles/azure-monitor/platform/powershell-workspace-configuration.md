---
title: Oluşturma ve Log Analytics çalışma alanı yapılandırma için PowerShell kullanma | Microsoft Docs
description: Log Analytics çalışma alanları Azure İzleyici'de, sunuculardan verileri şirket içinde depolamak veya altyapı bulut. Azure tanılama tarafından oluşturulmuş bir Azure depolama makine verilerini toplayabilir.
services: log-analytics
author: richrundmsft
ms.service: log-analytics
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: richrund
ms.openlocfilehash: f37c8290defa5e7c9baa3b705393aba376936fd8
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539386"
---
# <a name="manage-log-analytics-workspace-in-azure-monitor-using-powershell"></a>PowerShell kullanarak Azure İzleyici'de log Analytics çalışma alanını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Kullanabileceğiniz [Log Analytics PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) bir Log Analytics çalışma alanına Azure İzleyici'de bir komut satırından veya betik bir parçası olarak çeşitli işlevleri gerçekleştirmek için.  PowerShell ile gerçekleştirebileceğiniz görevler örnekleri şunlardır:

* Çalışma alanı oluşturma
* Çözüm Ekle Kaldır
* İçeri ve dışarı aktarma kayıtlı aramalar
* Bir bilgisayar grubu oluşturun
* Windows aracısının yüklü olduğu IIS günlükler bilgisayarlardan koleksiyonunu etkinleştir
* Linux ve Windows bilgisayarlardan performans sayaçlarını Topla
* Linux Bilgisayarları'nda syslog olaylarını Topla
* Windows olay günlüklerinden olaylarını Topla
* Özel olay günlüklerini toplar
* Bir Azure sanal makinesi için log analytics aracısını ekleme
* Azure Tanılama'yı kullanarak toplanan dizin verileri log analytics'e yapılandırın

Bu makalede, Powershell'den gerçekleştirebileceğiniz işlevlerin bazılarını göstermeyi iki kod örneği sağlanmıştır.  Başvurabilirsiniz [Log Analytics PowerShell cmdlet başvurusu](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) diğer işlevleri için.

> [!NOTE]
> Log Analytics, daha önce yer alan cmdlet'ler kullanılan adı, bu yüzden operasyonel İçgörüler olarak adlandırılıyordu.

## <a name="prerequisites"></a>Önkoşullar
Bu örnekler, sürüm 1.0.0 ile veya Az.OperationalInsights modülün daha sonra çalışır.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Oluşturma ve Log Analytics çalışma alanı yapılandırma
Aşağıdaki betik örneğinde gösterilmiştir nasıl yapılır:

1. Çalışma alanı oluşturma
2. Kullanılabilir çözümler listesi
3. Çözüm çalışma alanına ekleme
4. İçeri aktarma kaydedilen aramalar
5. Dışarı aktarma kaydedilen aramalar
6. Bir bilgisayar grubu oluşturun
7. Windows aracısının yüklü olduğu IIS günlükler bilgisayarlardan koleksiyonunu etkinleştir
8. Mantıksal Disk performans sayaçları Linux bilgisayarından toplar (% kullanılan Inode'ları; Boş megabayt; % Kullanılan alan; Disk aktarımı/sn; Disk Okuma/sn; Disk Yazma/sn)
9. Linux bilgisayarlardan Syslog olaylarını Topla
10. Windows bilgisayarlardan uygulama olay günlüğü'ndeki hata ve uyarı olaylarını Topla
11. Windows bilgisayarlardan bellek kullanılabilir MBayt performans sayacı Topla
12. Özel günlük toplama

```powershell

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
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json

# Create Computer Group based on a query
New-AzOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernel syslog collection"
Enable-AzOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs

New-AzOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```
Yukarıdaki örnekte regexDelimiter olarak tanımlanan "\\n" için yeni satır. Günlük sınırlayıcı bir zaman damgası da olabilir.  Desteklenen biçimler şunlardır:

| Biçimlendir | JSON normal ifade biçimi kullanan iki \\ için her \ standart bir normal ifade, bunu bir normal ifade uygulamasında test azaltılırsa \\ için \ | | |
| --- | --- | --- | --- |
| `YYYY-MM-DD HH:MM:SS` | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |
| `M/D/YYYY HH:MM:SS AM/PM` | `(([0-1]\\\\d)\|[0-9])/(([0-3]\\\\d)\|(\\\\d))/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]\\\\s(AM\|PM\|am\|pm)` | | |
| `dd/MMM/yyyy HH:MM:SS` | `((([0-3]\\\\d)\` | `(\\\\d))/(Jan\|Feb\|Mar\|May\|Apr\|Jul\|Jun\|Aug\|Oct\|Sep\|Nov\|Dec\|jan\|feb\|mar\|may\|apr\|jul\|jun\|aug\|oct\|sep\|nov\|dec)/((\\\\d{2})\|(\\\\d{4}))\\\\s((\\\\d)\` | `([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9])` |
| `MMM dd yyyy HH:MM:SS` | `(((?:Jan(?:uary)?\|Feb(?:ruary)?\|Mar(?:ch)?\|Apr(?:il)?\|May\|Jun(?:e)?\|Jul(?:y)?\|Aug(?:ust)?\|Sep(?:tember)?\|Sept\|Oct(?:ober)?\|Nov(?:ember)?\|Dec(?:ember)?)).*?((?:(?:[0-2]?\\\\d{1})\|(?:[3][01]{1})))(?![\\\\d]).*?((?:(?:[1]{1}\\\\d{1}\\\\d{1}\\\\d{1})\|(?:[2]{1}\\\\d{3})))(?![\\\\d]).*?((?:(?:[0-1][0-9])\|(?:[2][0-3])\|(?:[0-9])):(?:[0-5][0-9])(?::[0-5][0-9])?(?:\\\\s?(?:am\|AM\|pm\|PM))?))` | | |
| `yyMMdd HH:mm:ss` | `([0-9]{2}([0][1-9]\|[1][0-2])([0-2][0-9]\|[3][0-1])\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `ddMMyy HH:mm:ss` | `(([0-2][0-9]\|[3][0-1])([0][1-9]\|[1][0-2])[0-9]{2}\\\\s\\\\s?([0-1]?[0-9]\|[2][0-3]):[0-5][0-9]:[0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s?([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0-1]?[0-9]\|[2][0-3]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM  d HH:mm:ss` <br> MMM sonra iki boşluk | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `MMM d HH:mm:ss` | `(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\s([0]?[1-9]\|[1-2][0-9]\|[3][0-1])\\\\s([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])` | | |
| `dd/MMM/yyyy:HH:mm:ss +zzzz` <br> Burada + + veya - <br> Burada zzzz saat uzaklığı | `(([0-2][1-9]\|[3][0-1])\\\\/(Jan\|Feb\|Mar\|Apr\|May\|Jun\|Jul\|Aug\|Sep\|Oct\|Nov\|Dec)\\\\/((19\|20)[0-9][0-9]):([0][0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\\\s[\\\\+\|\\\\-][0-9]{4})` | | |
| `yyyy-MM-ddTHH:mm:ss` <br> Bir değişmez değer Harf T T olduğu | `((\\\\d{2})\|(\\\\d{4}))-([0-1]\\\\d)-(([0-3]\\\\d)\|(\\\\d))T((\\\\d)\|([0-1]\\\\d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]` | | |

## <a name="configuring-log-analytics-to-send-azure-diagnostics"></a>Log Analytics, Azure Tanılama verileri gönderecek şekilde yapılandırma
Azure kaynaklarını aracısız izleme için kaynakları etkin ve Log Analytics çalışma alanına yazmak için yapılandırılmış Azure tanılama olması gerekir. Bu yaklaşım, verileri doğrudan çalışma alanınıza gönderir ve bir depolama hesabına yazılmasına izin gerektirmez. Desteklenen kaynaklar şunlardır:

| Kaynak Türü | Günlükler | Ölçümler |
| --- | --- | --- |
| Uygulama Ağ Geçitleri    | Evet | Evet |
| Automation hesapları     | Evet | |
| Batch hesapları          | Evet | Evet |
| Data Lake analytics     | Evet | |
| Data Lake store         | Evet | |
| SQL esnek havuzu        |     | Evet |
| Olay hub'ı ad alanı     |     | Evet |
| IoT Hub                |     | Evet |
| Key Vault               | Evet | |
| Yük Dengeleyiciler          | Evet | |
| Logic Apps              | Evet | Evet |
| Ağ Güvenlik Grupları | Evet | |
| Redis için Azure Önbelleği             |     | Evet |
| Hizmet ara         | Evet | Evet |
| Service Bus ad alanı   |     | Evet |
| SQL (v12)               |     | Evet |
| Web Siteleri               |     | Evet |
| Web sunucu grupları        |     | Evet |

Kullanılabilir ölçümler ayrıntılarını başvurmak [ölçümleri Azure İzleyici ile desteklenen](../../azure-monitor/platform/metrics-supported.md).

Kullanılabilir günlükleri ayrıntılarını başvurmak [desteklenen Hizmetleri ve şema için tanılama günlüklerini](../../azure-monitor/platform/diagnostic-logs-schema.md).

```powershell
$workspaceId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Ayrıca, farklı Aboneliklerde olması kaynaklardan günlükleri toplamak için önceki cmdlet'ini de kullanabilirsiniz. Cmdlet'ini her iki kaynağın günlükleri ve günlüklerde gönderilir çalışma alanı oluşturma kimliği sunuyorsunuz olduğundan, abonelikler arasında iş kuramıyor.


## <a name="configuring-log-analytics-workspace-to-collect-azure-diagnostics-from-storage"></a>Depolama alanından Azure tanılama verilerini toplamak için Log Analytics çalışma alanını yapılandırma
Bir Klasik bulut hizmetini veya service fabric kümesi çalışan bir örnek günlük verilerini toplamak için önce verileri Azure depolama alanına yazmak gerekir. Bir Log Analytics çalışma alanı, ardından depolama hesabından günlükleri toplamak için yapılandırılır. Desteklenen kaynaklar şunlardır:

* Klasik cloud services (web ve çalışan rolleri)
* Service fabric kümeleri

Aşağıdaki örnekte gösterildiği nasıl yapılır:

1. Verileri çalışma dizinini oluşturacak konumları ve var olan depolama hesaplarını Listele
2. Bir depolama hesabından okumak için bir yapılandırma oluşturun
3. Yeni oluşturulan yapılandırma veri dizini oluşturmak için ek konumlardan güncelleştirin.
4. Yeni oluşturulan yapılandırmasını Sil

```powershell
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable"
$workspace = (Get-AzOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want the workspace to index
$storageId = "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight"

```

Önceki komut, farklı Aboneliklerdeki depolama hesaplarından günlük toplama için de kullanabilirsiniz. Depolama hesabı kaynak kimliği'ni ve karşılık gelen bir erişim anahtarı sağlayarak bu yana, abonelikler arasında çalışabilmek için betiğidir. Erişim anahtarı değiştirdiğinizde, yeni anahtar sağlamak için depolama öngörüsü güncelleştirmeniz gerekiyor.


## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics PowerShell cmdlet'leri gözden](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) Log analytics'in bir yapılandırma için PowerShell kullanma hakkında ek bilgi için.

