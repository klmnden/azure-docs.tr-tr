---
title: Azure Veri Gezgini veri alımı
description: Azure veri Gezgini'nde (yükle) veri alabilen farklı yolları hakkında bilgi edinin
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 6c9cb7282ba9d99ea643bbd023d876b8fbe3904c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045603"
---
# <a name="azure-data-explorer-data-ingestion"></a>Azure Veri Gezgini veri alımı

Veri alımı oluşturulacak veya güncelleştirilecek bir tablo Azure veri Gezgini'nde bir veya daha fazla kaynaktan alınan verileri kayıtlarını yüklemek için kullanılan bir işlemdir. Alınan sonra verileri sorgu için kullanılabilir hale gelir. Aşağıdaki diyagramda, veri alımı dahil olmak üzere Azure Veri Gezgini içinde çalışma için uçtan uca akışı gösterilmektedir.

![Veri akışı](media/ingest-data-overview/data-flow.png)

Veri alımı için sorumlu olan Azure Veri Gezgini veri yönetim hizmeti, aşağıdaki işlevleri sağlar:

1. **Veri çekme**: (Event Hubs) dış kaynaklardan veri çekmek veya bir Azure kuyruktan alma istekleri okuyun.

1. **Toplu işleme**: Toplu veriler aynı veritabanı ve tablo alımı aktarım hızını iyileştirmek için giden.

1. **Doğrulama**: Ön doğrulama ve biçim dönüştürme gerekirse.

1. **Veri işleme**: Şema, düzenleme, dizin oluşturma, kodlama ve verileri sıkıştırma eşleşen.

1. **Kalıcılık alım akışını belirli bir noktadaki**: Alma yük altyapısını yönetme ve bağlı geçici hataları yeniden deneme işlemlerini.

1. **Veri alma işleme**: Verileri sorgu için kullanılabilir hale getirir.

## <a name="ingestion-methods"></a>Alma yöntemi

Azure Veri Gezgini, her biri kendi hedef senaryoları, avantajları ve dezavantajları birkaç alma yöntemini destekler. Azure Veri Gezgini, işlem hatları ve ortak Hizmetleri, araştırma amacıyla SDK'lar ve doğrudan erişim altyapısı kullanılarak programlı alımı bağlayıcılar sağlar.

### <a name="ingestion-using-pipelines-connectors-and-plugins"></a>Alım işlem hatları, bağlayıcılar ve eklentileri

Şu anda Azure Veri Gezgini destekler:

* Azure portalında Yönetim Sihirbazı kullanılarak yönetilebilir event Grid kanal. Daha fazla bilgi için [alma Azure Blobları Azure Veri Gezgini içinde](ingest-data-event-grid.md).

* Azure portalında yönetim sihirbazını kullanarak yönetilen olay hub'ı kanal. Daha fazla bilgi için [olay hub'ı Azure veri Gezgini'ne alabilen](ingest-data-event-hub.md).

* Logstash eklentisi bkz [Logstash Azure veri Gezgini'ne alabilen](ingest-data-logstash.md).

* Kafka bağlayıcı bkz [Azure veri Gezgini'ne kafka'dan veri alma](ingest-data-kafka.md).

### <a name="ingestion-using-integration-services"></a>Tümleştirme hizmetlerini kullanarak alımı

* Bir tam olarak yönetilen bir veri tümleştirme hizmeti ve Azure Veri Gezgini'ni kullanarak verileri kopyalamak amacıyla Analiz iş yükleri için Azure Data Factory (ADF) [desteklenen veri depoları ve biçimler](/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats). Daha fazla bilgi için [ya da Azure veri Gezgini'nde Azure Data Factory kullanarak veri kopyalama](/azure/data-factory/connector-azure-data-explorer).

### <a name="programmatic-ingestion"></a>Programlı alma

Azure Veri Gezgini, sorgu ve veri alımı için kullanılabilir SDK'lar sağlar. Programlı alımı (COGs) alma maliyetleri azaltmak için depolama işlemleri sırasında ve alma işlemi izleyerek en aza indirerek iyileştirilmiştir.

**Kullanılabilir SDK'lar ve açık kaynaklı projelerin**:

Kusto istemci alma ve verileri sorgulamak için kullanılan SDK'sı sunar:

* [Python SDK'sı](/azure/kusto/api/python/kusto-python-client-library)

* [.NET SDK](/azure/kusto/api/netfx/about-the-sdk)

* [Java SDK](/azure/kusto/api/java/kusto-java-client-library)

* [Node SDK](/azure/kusto/api/node/kusto-node-client-library)

* [REST API](/azure/kusto/api/netfx/kusto-ingest-client-rest)

**Programlı alma tekniklerini**:

* Azure Veri Gezgini veri yönetim hizmeti (yüksek performanslı ve güvenilir alımı) aracılığıyla veri almak:

    [**Batch alımı** ](/azure/kusto/api/netfx/kusto-ingest-queued-ingest-sample) (SDK'sı tarafından sağlanan): istemci verileri Azure Blob Depolama (Azure Veri Gezgini veri yönetim hizmeti tarafından atanan) yükler ve bir Azure kuyruğuna bir bildirim gönderir. Batch alımı ucuz veri alımı ve yüksek hacimli, güvenilir, için önerilen tekniği ' dir.

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

İşlem süresi kısa birkaç saniye veri boyutuna bağlıdır. Toplu işlem süresi varsayılan olarak 5 dakika.

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

Bir ileti sistemi hizmeti gibi olay hub'ı temel alan kuruluşlar için mevcut bir altyapınız var olan bir bağlayıcı kullanarak büyük olasılıkla en uygun çözümüdür. Sıraya alınan alımı, büyük veri hacimleri için uygundur.

## <a name="supported-data-formats"></a>Desteklenen veri biçimleri

Azure Veri Gezgini, ayrıştırabilmesi için bu yöntemleri dışında sorgudan alma tüm alımı için verileri biçimlendirin. Desteklenen veri biçimlerini şunlardır:

* CSV, TSV, PSV, SCSV, SOH SÜRE SONU
* JSON (satır ayrılmış, çok satırlı), Avro
* ZIP ve GZIP 

> [!NOTE]
> Veriler alınır, hedef tablo sütunları temel alan veri türleri algılanır. Bir kaydı eksik ya da bir alanın gerekli veri türü olarak ayrıştırılamaz karşılık gelen tablo sütunları null değerlerle doldurulur.

## <a name="ingestion-recommendations-and-limitations"></a>Alma gereksinimleri ve sınırlamaları

* Alınan veri geçerli saklama İlkesi, veritabanının saklama İlkesi'nden elde edilir. Bkz: [Bekletme İlkesi](/azure/kusto/concepts/retentionpolicy) Ayrıntılar için. Veri alma gerektirir **tablo çıkışlara** veya **veritabanı çıkışlara** izinleri.
* 5 GB'lık maksimum dosya boyutu alımı destekler. Dosyalar 100 MB ve 1 GB arasında alma önerilir.

## <a name="schema-mapping"></a>Şema eşleme

Şema eşleme, kaynak veri alanları için hedef tablo sütun bağlama yardımcı olur.

* [CSV eşleme](/azure/kusto/management/mappings?branch=master#csv-mapping) tüm sıra tabanlı biçimler (isteğe bağlı) çalışır. Alma komut parametresi kullanılarak gerçekleştirilebilir veya [tablosunda önceden oluşturulmuş](/azure/kusto/management/tables?branch=master#create-ingestion-mapping) ve başvurulan alma komutunun parametre.
* [JSON eşleme](/azure/kusto/management/mappings?branch=master#json-mapping) (zorunlu) ve [Avro eşleme](/azure/kusto/management/mappings?branch=master#avro-mapping) (zorunlu) alma komut parametresi kullanılarak gerçekleştirilebilir. Ayrıca olabilirler [tablosunda önceden oluşturulmuş](/azure/kusto/management/tables#create-ingestion-mapping) ve başvurulan alma komutunun parametre.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure veri Gezgini'ne olay Hub'ından veri alma](ingest-data-event-hub.md)

> [!div class="nextstepaction"]
> [Event Grid aboneliğini Azure Veri Gezgini'ni kullanarak veri alma](ingest-data-event-grid.md)

> [!div class="nextstepaction"]
> [Azure veri Gezgini'ne kafka'dan veri alma](ingest-data-kafka.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini düğümü kitaplığını kullanarak veri alma](node-ingest-data.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma](net-standard-ingest-data.md)

> [!div class="nextstepaction"]
> [Azure Veri Gezgini Logstash verileri alma](ingest-data-logstash.md)
