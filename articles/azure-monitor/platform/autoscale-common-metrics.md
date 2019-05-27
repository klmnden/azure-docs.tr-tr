---
title: Otomatik ölçeklendirme ortak ölçümleri
description: Hangi ölçümleri otomatik ölçeklendirme için yaygın olarak kullanıldığı hakkında bilgi edinin, bulut Hizmetleri, sanal makineler ve Web uygulamaları.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/6/2016
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 9da8e5fb88ff34e561b579b760973ecd23c884a3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66129741"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure İzleyici otomatik ölçeklendirme ortak ölçümleri

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyici otomatik ölçeklendirmesini (ölçüler) telemetri verilerini temel alarak çalışan örnek sayısı yukarı veya aşağı ölçeklendirmenize olanak tanıyor. Bu belgede kullanmak isteyebileceğiniz ortak ölçümler açıklanmıştır. Azure portalında göre ölçeklendirmek için kaynak ölçüm seçebilirsiniz. Ancak, göre ölçeklendirmek için farklı bir kaynaktan herhangi bir ölçüm seçebilirsiniz.

Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts). Diğer Azure Hizmetleri farklı ölçeklendirme yöntemlerini kullanın.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Ölçümler, Resource Manager tabanlı VM'ler için işlem
Resource Manager tabanlı sanal makineler ve sanal makine ölçek kümeleri, varsayılan olarak, temel (konak düzeyinde) ölçümler gösterin. Ayrıca, bir Azure VM ve VMSS için tanılama veri toplamayı yapılandırırken, Azure tanılama uzantısı ("ölçümleri konuk işletim sistemi" yaygın olarak bilinir) konuk işletim sistemi performans sayaçları da yayar.  Bu ölçümleri otomatik ölçeklendirme kuralları kullanın.

Kullanabileceğiniz `Get MetricDefinitions` VMSS kaynağınız için kullanılabilir ölçümleri görüntülemek için API/PoSH/CLI.

VM ölçek kümeleri kullanıyorsanız ve listelenen belirli bir ölçüm görmüyor varsa büyük olasılıkla *devre dışı* , tanılama uzantısı'nda.

Belirli bir ölçüm değil olup olmadığını örneklenen veya istediğiniz sıklıkta aktarılan tanılama yapılandırması güncelleştirebilirsiniz.

Yukarıdaki her iki durumda da true ise, daha sonra gözden [PowerShell kullanarak Windows çalıştıran bir sanal makine Azure tanılamayı etkinleştirerek](../../virtual-machines/extensions/diagnostics-windows.md) yapılandırmak ve ölçüm etkinleştirmek için Azure VM tanılama uzantınızın güncelleştirmek için PowerShell hakkında. Bu makalede ayrıca örnek tanılama yapılandırma dosyası içerir.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Resource Manager tabanlı Windows ve Linux VM'ler için konak ölçümleri
Aşağıdaki konak düzeyinde ölçümler, varsayılan olarak Azure VM ve VMSS için hem Windows hem de Linux örnekleri gönderilir. Bu ölçümler Azure VM açıklamaktadır, ancak Azure VM konağı yerine Konuk sanal Makinede yüklü bir aracı üzerinden toplanır. Otomatik ölçeklendirme kuralları, bu ölçümleri kullanabilirsiniz.

- [Resource Manager tabanlı Windows ve Linux VM'ler için konak ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)
- [Resource Manager tabanlı Windows ve Linux VM ölçek kümeleri için ana ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Konuk işletim sistemi ölçümleri Resource Manager tabanlı Windows Vm'leri
Azure'da bir VM oluşturduğunuzda, tanılama tanılama uzantısını kullanarak etkinleştirilir. Tanılama uzantısını VM içinde alınan ölçümleri bir dizi yayar. Başka bir deyişle, varsayılan olarak yayılan değil ölçümleri dışına otomatik ölçeklendirme yapabilirsiniz.

PowerShell'de aşağıdaki komutu kullanarak, ölçümlerin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Ölçüm Adı | Birim |
| --- | --- |
| \Processor(_Total)\% Processor Time |Percent |
| \Processor(_Total)\% ayrıcalıklı zaman |Percent |
| \Processor(_Total)\% kullanıcı zamanı |Percent |
| \Processor bilgi (_Total) \Processor sıklığı |Count |
| \System\Processes |Count |
| \Process (_Total) \Thread sayısı |Count |
| \Process (_Total) \Handle sayısı |Count |
| \Memory\% Kaydedilmiş Bayt yüzdesi |Percent |
| \Memory\Available Bytes |Bayt |
| \Memory\Committed bayt |Bayt |
| \Memory\Commit sınırı |Bayt |
| \Memory\Pool disk belleğine alınan bayt |Bayt |
| \Memory\Pool olmayan havuz bayt sayısı |Bayt |
| \PhysicalDisk(_Total)\% disk zamanı |Percent |
| \PhysicalDisk(_Total)\% disk okuma süresi |Percent |
| \PhysicalDisk(_Total)\% disk yazma saati |Percent |
| \PhysicalDisk (_Total) \Disk aktarımı/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Okuma/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk Yazma/sn |CountPerSecond |
| \PhysicalDisk (_Total) \Disk bayt/sn |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk Okuma Bayt/sn |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk Yazma Bayt/sn |BytesPerSecond |
| \Avg \PhysicalDisk (_Total). Disk Kuyruğu Uzunluğu |Count |
| \Avg \PhysicalDisk (_Total). Disk okuma kuyruğu uzunluğu |Count |
| \Avg \PhysicalDisk (_Total). Disk yazma kuyruğu uzunluğu |Count |
| \LogicalDisk(_Total)\% boş alanı |Percent |
| \LogicalDisk (_Total) \Free megabayt sayısı |Count |

### <a name="guest-os-metrics-linux-vms"></a>Konuk işletim sistemi ölçümleri Linux Vm'leri
Azure'da bir VM oluşturduğunuzda, tanılama tanılama uzantısını kullanarak varsayılan olarak etkindir.

PowerShell'de aşağıdaki komutu kullanarak, ölçümlerin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Ölçüm Adı | Birim |
| --- | --- |
| \Memory\AvailableMemory |Bayt |
| \Memory\PercentAvailableMemory |Percent |
| \Memory\UsedMemory |Bayt |
| \Memory\PercentUsedMemory |Percent |
| \Memory\PercentUsedByCache |Percent |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bayt |
| \Memory\PercentAvailableSwap |Percent |
| \Memory\UsedSwap |Bayt |
| \Memory\PercentUsedSwap |Percent |
| \Processor\PercentIdleTime |Percent |
| \Processor\PercentUserTime |Percent |
| \Processor\PercentNiceTime |Percent |
| \Processor\PercentPrivilegedTime |Percent |
| \Processor\PercentInterruptTime |Percent |
| \Processor\PercentDPCTime |Percent |
| \Processor\PercentProcessorTime |Percent |
| \Processor\PercentIOWaitTime |Percent |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Saniye |
| \PhysicalDisk\AverageWriteTime |Saniye |
| \PhysicalDisk\AverageTransferTime |Saniye |
| \PhysicalDisk\AverageDiskQueueLength |Count |
| \NetworkInterface\BytesTransmitted |Bayt |
| \NetworkInterface\BytesReceived |Bayt |
| \NetworkInterface\PacketsTransmitted |Count |
| \NetworkInterface\PacketsReceived |Count |
| \NetworkInterface\BytesTotal |Bayt |
| \NetworkInterface\TotalRxErrors |Count |
| \NetworkInterface\TotalTxErrors |Count |
| \NetworkInterface\TotalCollisions |Count |

## <a name="commonly-used-web-server-farm-metrics"></a>Yaygın olarak kullanılan Web (sunucu grubu) ölçümleri
Ayrıca, Http kuyruk uzunluğu gibi yaygın web sunucusu ölçümleri temel alan otomatik ölçeklendirme gerçekleştirebilir. Ölçüm adı olan **HttpQueueLength**.  Aşağıdaki bölümde, kullanılabilir bir sunucu grubu (Web uygulamaları) ölçümlerini listelenmektedir.

### <a name="web-apps-metrics"></a>Web Apps ölçümleri
PowerShell'de aşağıdaki komutu kullanarak, Web Apps ölçümlerin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Uyarı veya ölçeklendirme bu ölçümlere göre.

| Ölçüm Adı | Birim |
| --- | --- |
| CpuPercentage |Percent |
| MemoryPercentage |Percent |
| DiskQueueLength |Count |
| HttpQueueLength |Count |
| BytesReceived |Bayt |
| BytesSent |Bayt |

## <a name="commonly-used-storage-metrics"></a>Yaygın olarak kullanılan depolama ölçümleri
Depolama kuyruğundaki iletileri sayısı depolama kuyruğu uzunluğu olarak ölçeklendirebilirsiniz. Depolama kuyruk uzunluğu özel bir ölçüm ve eşik örnek başına ileti sayısı. Kuyruktaki iletilerin toplam sayısını 200 Örneğin, iki örneği varsa ve Eşiği 100'e ayarlanırsa, ölçeklendirme gerçekleşir. Örnek başına 100 iletileri, en fazla 200 veya daha fazlasını ekleyen 120 ve 80 veya herhangi bir birleşimini olabilir.

Bu ayar Azure portalında yapılandırmanız **ayarları** dikey penceresi. VM ölçek kümeleri için kullanmak üzere Resource Manager şablonu otomatik ölçeklendirme ayarı güncelleştirebilirsiniz *metricName* olarak *ApproximateMessageCount* ve depolama kuyruğu kimliği geçirin  *metricResourceUri*.

Örneğin, bir Klasik depolama hesabı ile otomatik ölçeklendirme ayarı metricTrigger aşağıdakileri içerir:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

MetricTrigger (Klasik olmayan) depolama hesabı için aşağıdakileri içerir:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Yaygın olarak kullanılan Service Bus ölçümleri
Service Bus kuyruğundaki iletileri sayısı Service Bus kuyruk uzunluğuna göre ölçeklendirebilirsiniz. Service Bus kuyruğu uzunluğu özel bir ölçüm ve eşik örnek başına ileti sayısı. Kuyruktaki iletilerin toplam sayısını 200 Örneğin, iki örneği varsa ve Eşiği 100'e ayarlanırsa, ölçeklendirme gerçekleşir. Örnek başına 100 iletileri, en fazla 200 veya daha fazlasını ekleyen 120 ve 80 veya herhangi bir birleşimini olabilir.

VM ölçek kümeleri için kullanmak üzere Resource Manager şablonu otomatik ölçeklendirme ayarı güncelleştirebilirsiniz *metricName* olarak *ApproximateMessageCount* ve depolama kuyruğu kimliği geçirin  *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Service Bus için kaynak grubu kavramını yok ancak Azure Resource Manager bölge başına varsayılan kaynak grubu oluşturur. Kaynak grubu genellikle 'Default - ServiceBus-[Bölge]' biçimindedir. Örneğin, 'Varsayılan-ServiceBus-EastUS', 'Varsayılan-ServiceBus-WestUS', 'Varsayılan-ServiceBus-AustraliaEast' vb.
>
>
