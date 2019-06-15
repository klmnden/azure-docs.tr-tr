---
title: Azure İzleyici'de Azure etkinlik günlüğü olaylarını görüntüle
description: Azure İzleyici'de Azure Etkinlik günlüğünü görüntüleyin ve PowerShell, CLI ve REST API ile alır.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 32578f77f2b3f30d80953bdd1099d22c945c640b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248122"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Azure etkinlik günlüğü olayları almak ve görüntüleme

[Azure etkinlik günlüğü](activity-logs-overview.md) oluşan Azure'da abonelik düzeyindeki olayların hakkında Öngörüler sağlar. Bu makalede, görüntüleme ve etkinlik günlüğü olaylarını almak için farklı yöntemler hakkında ayrıntılı bilgi sağlar.

## <a name="azure-portal"></a>Azure portal
Tüm kaynaklardan için Etkinlik günlüğünü görüntüleyin **İzleyici** Azure portalındaki menü. Belirli bir kaynak için Etkinlik günlüğünü görüntüleyin **etkinlik günlüğü** o kaynağın menü seçeneği.

![Etkinlik Günlüğü Görüntüle](./media/activity-logs-overview/view-activity-log.png)

Etkinlik günlüğü olayları aşağıdaki alanlara göre filtreleyebilirsiniz:

* **TimeSpan**: Olaylar için başlangıç ve bitiş zamanı.
* **Kategori**: Olay kategorisi açıklandığı [etkinlik günlüğünde kategorileri](activity-logs-overview.md#categories-in-the-activity-log).
* **Abonelik**: Bir veya daha fazla Azure aboneliği adları.
* **Kaynak grubu**: Seçili abonelik içinde bir veya daha fazla kaynak grupları.
* **Kaynak (ad)** :-belirli bir kaynağın adı.
* **Kaynak türü**: Örneğin, kaynak türünü _Microsoft.Compute/virtualmachines_.
* **İşlem adı** -adı, örneğin, bir Azure Resource Manager işlem _Microsoft.SQL/servers/Write_.
* **Önem derecesi**: Olay önem derecesi. Kullanılabilir değerler _bilgilendirici_, _uyarı_, _hata_, _kritik_.
* **Olayı başlatan tarafından**: İşlemi gerçekleştiren kullanıcı.
* **Açık arama**: Tüm olaylarda tüm alanlar boyunca o dize için arama metin arama kutusunu açın.

### <a name="view-change-history"></a>Değişiklik geçmişi görüntüle

Etkinlik günlüğünü gözden geçirirken, değişiklikleri sırasında olduğunu görmek için yardımcı olur, olay saati. Bu bilgiyle görüntüleyebilirsiniz **değişiklik geçmişini**. Daha ayrıntılı olarak incelemek istediğiniz etkinlik günlüğünden bir olay seçin. Seçin **değişiklik geçmişini (Önizleme)** herhangi görüntülemek için sekmesinde değişiklikler bu olay ile ilişkili.

![Bir olay için değişiklik geçmişi listesi](media/activity-logs-overview/change-history-event.png)

Olay ile ilişkili herhangi bir değişiklik varsa, seçebileceğiniz değişikliklerin bir listesini görürsünüz. Bu açılır **değişiklik geçmişini (Önizleme)** sayfası. Bu sayfada kaynak değişiklikleri bakın. Aşağıdaki örnekte görebileceğiniz gibi biz yalnızca VM boyutları, ancak değişiklikten önce önceki VM boyutu ne olduğunu ve ne şekilde değiştirilmiştir değiştiğini görebilirsiniz.

![Farkları gösteren değişiklik geçmişi sayfası](media/activity-logs-overview/change-history-event-details.png)

Değişiklik geçmişi hakkında daha fazla bilgi için bkz: [alma kaynak değişiklikleri](../../governance/resource-graph/how-to/get-resource-changes.md).


## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Tıklayın **günlükleri** en üstündeki **etkinlik günlüğü** sayfasını açmak için [Activity Log Analytics, izleme çözümü](activity-log-collect.md) abonelik için. Bu, etkinlik günlüğü için analytics görüntülemek ve çalıştırmak için sağlayacak [oturum sorguları](../log-query/log-query-overview.md) ile **AzureActivity** tablo. Etkinlik günlüğünüzü bir Log Analytics çalışma alanına bağlı değilse, bu yapılandırma gerçekleştirmeniz istenir.



## <a name="powershell"></a>PowerShell
Kullanım [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) Powershell'den etkinlik günlüğü almak için cmdlet'i. Bazı genel örnekleri aşağıda verilmiştir.

> [!NOTE]
> `Get-AzLog` yalnızca 15 günlük geçmişi sağlar. Kullanım **- MaxEvents** 15 gün ötesinde son N olayları sorgulamak için parametre. 15 gün sayısından önceki olayların erişmek için REST API veya SDK'sını kullanın. Dahil etmezseniz **StartTime**, varsayılan değer ise **EndTime** eksi bir saat. Dahil etmezseniz **EndTime**, geçerli zamanı varsayılan değeridir. Tüm saatler UTC biçimindedir.


Belirli bir tarih saatten sonra oluşturulup günlük girişlerini alın:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Bir tarih saat aralık günlük girişlerini alın:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Günlük girişlerini belirli bir kaynak grubundan alın:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Bir tarih saat aralık belirli kaynak sağlayıcısından günlük girişlerini alın:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Belirli bir çağıranın ile günlük girişlerini alın:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Son 1000 olayları alın:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
Kullanım [az İzleyici etkinlik günlüğü](cli-samples.md#view-activity-log-for-a-subscription) CLI'dan etkinlik günlüğü alınamadı. Bazı genel örnekleri aşağıda verilmiştir.


Tüm kullanılabilir seçenekleri görüntüleyin.

```azurecli
az monitor activity-log list -h
```

Günlük girişlerini belirli bir kaynak grubundan alın:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Belirli bir çağıranın ile günlük girişlerini alın:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Bir kaynak türü, bir tarih aralığındaki çağıran tarafından günlükleri alın:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>REST API
Kullanım [Azure İzleyici REST API](https://docs.microsoft.com/rest/api/monitor/) etkinlik günlüğü bir diğer istemciden alınacak. Bazı genel örnekleri aşağıda verilmiştir.

Etkinlik günlükleri ile filtre alın:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Etkinlik günlükleri, filtre ve select alın:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Etkinlik günlüklerini Seç alın:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Etkinlik günlükleri, filtre veya select alın:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>Sonraki Adımlar

* [Etkinlik günlüğüne genel bakış okuyun](activity-logs-overview.md)
* [Event Hubs'ta akışa veya depolama için Etkinlik günlüğünü arşivleme](activity-log-export.md)
* [Azure etkinlik günlüğünün Event Hubs'a Stream](activity-logs-stream-event-hubs.md)
* [Arşiv Depolama'ya Azure etkinlik günlüğü](archive-activity-log.md)

