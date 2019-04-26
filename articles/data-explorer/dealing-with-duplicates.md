---
title: Azure veri Gezgini'nde yinelenen veri işleme
description: Bu konuda, Azure Veri Gezgini'ni kullanarak, yinelenen veri ile ilgilenmeniz çeşitli yaklaşımlar gösterilmektedir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 8f55b6dfb7b5bc9eda675aca4ed80a66b8a25a7f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445779"
---
# <a name="handle-duplicate-data-in-azure-data-explorer"></a>Azure veri Gezgini'nde yinelenen veri işleme

Cihazlar bulut için veri gönderen bir yerel veri önbelleği korur. Veri boyutu bağlı olarak yerel önbellek verileri için gün veya hatta aylar depoluyor. Analitik veritabanlarınızı önbelleğe alınmış verileri de yeniden göndermeniz ve analitik veritabanında veri çoğaltma neden yapıyor cihazlardan korumak istiyorsunuz. Bu konuda, bu tür senaryolar için yinelenen veri işleme için en iyi uygulamalar açıklanmaktadır.

Veri çoğaltma için en iyi çözüm çoğaltma engelliyor. Veri işlem hattı boyunca veri taşıma ile ilişkili maliyetler kaydeder ve karşılanan sisteme alınan yinelenen veri ile şirket kaynaklarına harcama önler daha önce veri ardışık düzeninde Mümkünse sorunu düzeltin. Bununla birlikte, burada kaynak sistemi değiştirilemez durumda bu senaryoyla yarayan çeşitli yolları vardır.

## <a name="understand-the-impact-of-duplicate-data"></a>Yinelenen verileri etkisini anlama

Yinelenen verileri yüzdesini izleyin. Yinelenen verileri yüzdesi bulununca sorunu ve iş üzerindeki etkilerine kapsamını çözümleme ve uygun çözümü seçebilirsiniz.

Örnek Sorgu: yinelenen kayıtları yüzdesini belirlemek için

```kusto
let _sample = 0.01; // 1% sampling
let _data =
DeviceEventsAll
| where EventDateTime between (datetime('10-01-2018 10:00') .. datetime('10-10-2018 10:00'));
let _totalRecords = toscalar(_data | count);
_data
| where rand()<= _sample
| summarize recordsCount=count() by hash(DeviceId) + hash(EventId) + hash(StationId)  // Use all dimensions that make row unique. Combining hashes can be improved
| summarize duplicateRecords=countif(recordsCount  > 1)
| extend duplicate_percentage = (duplicateRecords / _sample) / _totalRecords  
```

## <a name="solutions-for-handling-duplicate-data"></a>Yinelenen veri işleme yönelik çözümler

### <a name="solution-1-dont-remove-duplicate-data"></a>Çözüm #1: Yinelenen verileri kaldırma

İş gereksinimlerinize ve yinelenen veri tolerans anlayın. Bazı veri kümeleri, yinelenen verileri belirli bir yüzdesi ile yönetebilirsiniz. Yinelenen verileri büyük etkiye sahip değilse, varlığı yoksayabilirsiniz. Yinelenen verileri kaldırma değil avantajı ortadan kalkar alma işlemi veya sorgu performansı ' dir.

### <a name="solution-2-handle-duplicate-rows-during-query"></a>Çözüm #2: Sorgu sırasında yinelenen satırları işleme

Sorgu sırasında veri yinelenen satırları filtrelemek başka bir seçenektir. [ `arg_max()` ](/azure/kusto/query/arg-max-aggfunction) Toplu işlev, yinelenen kayıtları süzer ve zaman damgası (veya başka bir sütuna) göre en son kaydını döndürmek için kullanılabilir. Bu yöntemi kullanmanın avantajı daha hızlı alma olduğu yinelenenleri kaldırma sorgu süre boyunca ortaya çıkar. Ayrıca, Denetim ve sorun giderme için tüm kayıtları (yinelenenler de dahil) kullanılabilir. Kullanılarak dezavantajı `arg_max` işlevi, ek sorgu süresini ve CPU üzerindeki yükü veriler sorgulanır her zaman kullanılabilir. Sorgulanan veri miktarına bağlı olarak bu çözüm, işlevsel olmayan veya bellek kullanan durdurabilir ve diğer seçenekleri değiştirme gerektirir.

Aşağıdaki örnekte, biz benzersiz kayıtları belirlemek sütun kümesi için alınan son kaydını sorgu:

```kusto
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize hint.strategy=shuffle arg_max(EventDateTime, *) by DeviceId, EventId, StationId
```

Bu sorgu, tablonun doğrudan sorgulama yerine bir işlev içinde de yerleştirilebilir:

```kusto
.create function DeviceEventsView
{
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize arg_max(EventDateTime, *) by DeviceId, EventId, StationId
}
```

### <a name="solution-3-filter-duplicates-during-the-ingestion-process"></a>Çözüm #3: Alma işlemi sırasında yinelenenleri Filtrele

Yinelenen filtre alma işlemi sırasında başka bir çözümdür. Kusto tablolara alımı sırasında yinelenen verileri yoksayar. Veriler bir hazırlama tablosuna alınan ve yinelenen satırlar kaldırdıktan sonra başka bir tabloya kopyalar. Bu çözümün avantajı, sorgu performansını önemli ölçüde karşılaştırıldığında önceki çözümü artırır olmasıdır. Dezavantajlarından artan alma süresi ve ek veri depolama maliyetini içerir.

Bu yöntem aşağıdaki örnekte gösterilmiştir:

1. Aynı şemaya başka bir tablo oluşturun:

    ```kusto
    .create table DeviceEventsUnique (EventDateTime: datetime, DeviceId: int, EventId: int, StationId: int)
    ```

1. Yinelenen kayıtları göre filtrelemek için bir işlev oluşturma hologram katılma daha önce alınan fiyatlarla yeni kayıtlar.

    ```kusto
    .create function RemoveDuplicateDeviceEvents()
    {
    DeviceEventsAll
    | join hint.strategy=broadcast kind = anti
        (
        DeviceEventsUnique
        | where EventDateTime > ago(7d)   // filter the data for certain time frame
        | limit 1000000   //set some limitations (few million records) to avoid choking-up the system during outage recovery

        ) on DeviceId, EventId, StationId
    }
    ```

    > [!NOTE]
    > Birleştirmeler CPU bağlantılı işlemler ve sisteme ek yük ekleyin.

1. Ayarlama [güncelleştirme ilkesi](/azure/kusto/management/update-policy) üzerinde `DeviceEventsUnique` tablo. Yeni veri uygulamasına gittiğinde güncelleştirme ilkesi etkinleştirilir `DeviceEventsAll` tablo. Kusto altyapısı otomatik olarak yeni olarak işlev yürütülür [kapsamlarını](/azure/kusto/management/extents-overview) oluşturulur. Yeni oluşturulan veri işleme kapsamlıdır. Aşağıdaki komut, kaynak tablosu bitiştirir (`DeviceEventsAll`), hedef tablo (`DeviceEventsUnique`) ve işlev `RemoveDuplicatesDeviceEvents` birlikte bir güncelleştirme ilkesi oluşturmak için.

    ```kusto
    .alter table DeviceEventsUnique policy update
    @'[{"IsEnabled": true, "Source": "DeviceEventsAll", "Query": "RemoveDuplicateDeviceEvents()", "IsTransactional": true, "PropagateIngestionProperties": true}]'
    ```

    > [!NOTE]
    > Güncelleştirme ilkesi, veri alımı sırasında filtrelenmiş ve iki kez alınan alma süresini genişletir (için `DeviceEventsAll` tablo ve `DeviceEventsUnique` tablo).

1. (İsteğe bağlı) Daha düşük bir veri saklama ayarlamak `DeviceEventsAll` verilerin kopyaları depolanmasını önlemek için tablo. Veri hacmine bağlı olarak gün sayısı ve sorun giderme verilerini saklamak istediğiniz süreyi seçin. Ayarlayabilirsiniz `0d` gün saklama COGS kaydetmek ve veri depolama alanına yüklenir olmadığından bu yana, performansı artırmak için.

    ```kusto
    .alter-merge table DeviceEventsAll policy retention softdelete = 1d
    ```

## <a name="summary"></a>Özet

Veri çoğaltma, birden çok yolla işlenebilir. Seçenekler, işletmeniz için doğru yöntem belirlemek için hesap fiyat ve performans, alma dikkatlice değerlendirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Veri Gezgini için sorgu yazma](write-queries.md)
