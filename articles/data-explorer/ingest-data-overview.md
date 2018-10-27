---
title: Azure Veri Gezgini veri alımı
description: Azure veri Gezgini'nde (yükle) veri alabilen farklı yolları hakkında bilgi edinin
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f1df22c505bffdfaf60bf9c6eec3ad4e698fff02
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139535"
---
# <a name="azure-data-explorer-data-ingestion"></a>Azure Veri Gezgini veri alımı

Veri alımı oluşturulacak veya güncelleştirilecek bir tablo Azure veri Gezgini'nde bir veya daha fazla kaynaktan alınan verileri kayıtlarını yüklemek için kullanılan bir işlemdir. Alınan sonra verileri sorgu için kullanılabilir hale gelir. Azure veri alımı dahil olmak üzere, veri Gezgini'nin çalışmak için uçtan uca akışı aşağıdaki diyagramda gösterilmektedir **(2)**.

![Genel veri akışı](media/ingest-data-overview/overall-data-flow.png)

Veri alımı için sorumlu olan Azure Veri Gezgini veri yönetim hizmeti, aşağıdaki işlevleri sağlar:

1. **Veri çekme**: dış kaynaklardan (Event Hubs) veri çekme veya bir Azure kuyruktan alma istekleri okuyun.

1. **Toplu işleme**: toplu verileri aynı veritabanı ve tablo alımı aktarım hızını iyileştirmek için giden.

1. **Doğrulama**: gerekli olursa ön doğrulama ve biçim dönüştürme.

1. **Veri işleme**: düzenleme, dizin oluşturma, kodlama ve verileri sıkıştırma eşleşen şema.

1. **Kalıcılık alım akışını belirli bir noktadaki**: alma yükü altyapısı ve tutamacı yeniden deneme sırasında geçici hataları yönetme.

1. **Veri alma işleme**: verileri sorgu için kullanılabilir hale getirir.

> [!NOTE]
> Alınan veri geçerli saklama İlkesi, veritabanının saklama İlkesi'nden elde edilir. Bkz: [Bekletme İlkesi](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy) Ayrıntılar için. Veri alma gerektirir **tablo çıkışlara** veya **veritabanı çıkışlara** izinleri.

## <a name="ingestion-methods"></a>Alma yöntemi

Azure Veri Gezgini, her biri kendi hedef senaryoları, avantajları ve dezavantajları birkaç alma yöntemini destekler. Azure Veri Gezgini, ortak Hizmetleri, araştırma amacıyla SDK'lar ve doğrudan erişim altyapısı kullanılarak programlı alımı bağlayıcılar sağlar.

### <a name="ingestion-using-connectors"></a>Bağlayıcıları kullanarak alımı

Azure Veri Gezgini, şu anda Azure portalında yönetim sihirbazını kullanarak yönetilen bir olay hub'ı bağlayıcıyı destekler. Daha fazla bilgi için [hızlı başlangıç: Azure veri Gezgini'ne olay Hub'ından veri alma](ingest-data-event-hub.md).

### <a name="programmatic-ingestion"></a>Programlı alma

Azure Veri Gezgini, sorgu ve veri alımı için kullanılabilir SDK'lar sağlar. Programlı alımı (COGs) alma maliyetleri azaltmak için depolama işlemleri sırasında ve alma işlemi izleyerek en aza indirerek iyileştirilmiştir.

**Kullanılabilir SDK'lar ve açık kaynak projeleri**:

Kusto istemci alma ve verileri sorgulamak için kullanılan SDK'sı sunar:

* [Python SDK'sı](https://docs.microsoft.com/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](https://docs.microsoft.com/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](https://docs.microsoft.com/azure/kusto/api/java/kusto-java-client-library)

* [Node SDK]

* [REST API](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Programlı alma tekniklerini**:

* Azure Veri Gezgini veri yönetim hizmeti (yüksek performanslı ve güvenilir alımı) aracılığıyla veri alma

  * [**Batch alımı** ](https://docs.microsoft.com/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (SDK'sı tarafından sağlanan): istemci verileri Azure Blob Depolama (Azure Veri Gezgini veri yönetim hizmeti tarafından atanan) yükler ve bir Azure kuyruğuna bir bildirim gönderir. Yüksek hacimli, güvenilir ve ucuz veri alımı için önerilen yöntem budur.

* Azure Veri Gezgini altyapısı (en keşfi ve prototip oluşturma için uygun) doğrudan veri alma:

  * **Satır içi alımı**: bant dışı verileri içeren denetim komutu (.ingest satır içi) için geçici sınama amacıyla tasarlanmıştır.

  * **Sorgudan alma**: sorgu sonuçlarını gösteren denetim komutu (.set, .set veya ekleme işlemlerinde, .set veya değiştirme), raporlar veya küçük geçici tablolar oluşturmak için kullanılır.

  * **Depolama alanından alma**: denetim komutu (.ingest içine) harici olarak depolanan veriler (örneğin, Azure Blob Depolama) ile veri alımı verimli toplu sağlar.

**Gecikme süresi farklı yöntemler**:

| Yöntem | Gecikme süresi |
| --- | --- |
| **Satır içi alımı** | Hemen |
| **Sorgudan alma** | Sorgu süresini + işlem süresi |
| **Depolama alanından alma** | Karşıdan yükleme süresi + işlem süresi |
| **Kuyruğa alınmış alma** | Toplu işleme süresi + işleme süresi |
| |

İşleme süresi, genellikle küçüktür birkaç saniye veri boyutuna bağlıdır. Toplu işlem süresi varsayılan olarak 5 dakika.

## <a name="choosing-the-most-appropriate-ingestion-method"></a>En uygun alma yöntemi seçme

Veri alımı başlamadan önce kendiniz aşağıdaki soruları sormaya.

* Verilerim nerede bulunur? 
* Veri biçimi nedir ve onu değiştirilebilir? 
* Sorgulanacak gerekli alanlar nelerdir? 
* Beklenen veri hacmi ve hız nedir? 
* Kaç tane olay türleri (tablo sayısı yansıtılır) beklenen? 
* Ne sıklıkla olay şeması değiştirilmesi beklenmeyen? 
* Verileri ne kadar düğümü oluşturur? 
* İşletim sistemi kaynağı nedir? 
* Gecikme süresi gereksinimleri nelerdir? 
* Mevcut yönetilen alım işlem hatları biri kullanılabilir mi? 

Olay hub'ı gibi bir ileti sistemi hizmeti dayalı mevcut bir altyapınız var olan kuruluşlar için bir bağlayıcı kullanarak büyük olasılıkla en uygun çözümüdür. Sıraya alınan alımı, büyük veri hacimleri için uygundur.

## <a name="supported-data-formats"></a>Desteklenen veri biçimleri

Azure Veri Gezgini, ayrıştırabilmesi için bu yöntemleri dışında sorgudan alma tüm alımı için verileri desteklenen veri biçimlerinden birini biçimlendirilmiş olması gerekir.

* CSV, TSV, PSV, SCSV, SOH SÜRE SONU
* JSON (satır ayrılmış, çok satırlı), Avro
* ZIP ve GZIP 

> [!NOTE]
> Veriler alınır, hedef tablo sütunları temel alan veri türleri algılanır. Bir kaydı eksik ya da bir alanın gerekli veri türü olarak ayrıştırılamaz karşılık gelen tablo sütunları null değerlerle doldurulur.

## <a name="schema-mapping"></a>Şema eşleme

Şema eşleme belirleyici kaynak veri alanları için hedef tablo sütun bağlama yardımcı olur.

* [CSV eşleme](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#csv-mapping) sıra tabanlı biçimler (isteğe bağlı) çalışır ve alma komutunun parametre olarak geçirilen veya [tablosunda önceden oluşturulmuş](https://docs.microsoft.com/azure/kusto/management/tables?branch=master#create-ingestion-mapping) ve başvurulan alma komutunun parametre.
* [JSON eşleme](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#json-mapping) (zorunlu) ve [Avro eşleme](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master#avro-mapping) (zorunlu) alma komut parametresi geçirilebilir veya [tablosunda önceden oluşturulmuş](https://docs.microsoft.com/azure/kusto/management/tables#create-ingestion-mapping) ve başvurulan alma komutunun parametre.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Verileri Event Hub'dan Azure Veri Gezgini'ne alma](ingest-data-event-hub.md)

[Hızlı başlangıç: Azure Veri Gezgini Python kitaplığını kullanarak verileri alma](python-ingest-data.md)

