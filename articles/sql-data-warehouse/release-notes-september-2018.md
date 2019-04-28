---
title: Eylül 2018'den Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 10/08/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: bc559a1224aace2ee599c24c8dce07a6d55173fd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61474989"
---
# <a name="whats-new-in-azure-sql-data-warehouse-september-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Eylül 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Eylül 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="new-lower-entry-point-for-sql-data-warehouse-gen2"></a>SQL veri ambarı Gen2 için yeni alt giriş noktası
Nisan 2018'de [Microsoft tarafından Duyuruldu](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) Azure SQL veri ambarı Gen2 sunduğu performans x 5, 5 x işlem ölçeği, 4 x eşzamanlılık ve sınırsız depolama. Belirtilen [Kıyaslama bulut veri ambarında](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) Gigaom, SQL veri ambarı Gen2'ye göre **%42 Amazon Redshift çok daha iyi**.

2. nesil bir alt giriş noktası, daha küçük boyutlu veri ambarı veya geliştirme/test ortamları tüm yapılan en son hizmet geliştirmeleri ile çalıştırmanıza izin vererek DWU500c en genel kullanıma sunulmuştur. Yeni giriş noktası da dahil olmak üzere 2. nesil özelliklerin tümünü korur [Uyarlamalı önbelleğe alma](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/), [aydınlatma hızlı veri karıştırma](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/), için ve Destek [gerçek zamanlı veri ambarı](https://azure.microsoft.com/blog/enabling-real-time-data-warehousing-with-azure-sql-data-warehouse/).

## <a name="sql-vulnerability-assessment"></a>SQL Güvenlik Açığı Değerlendirmesi
[SQL güvenlik açığı değerlendirmesi (VA)](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/25/sql-vulnerability-assessment-now-supports-azure-sql-data-warehouse-and-azure-sql-database-managed-instance/) veri Ambarınızı sürekli olarak izleyen bir kullanımı kolay hizmetidir. Her zaman yüksek düzeyde güvenlik olmanıza yardımcı olur ve kuruluş ilkelerinizin karşılanması. Bu, bulunan her sorun için bir eyleme dönüştürülebilir düzeltme adımlarının yanı sıra kapsamlı güvenlik raporu sağlar. Bir güvenlik olmasanız bile bu rapor, proaktif olarak, veritabanı güvenliği stature yönetmek ve en yüksek etkisi eylemleri ilgilenmeniz odaklanmak için uzman kolaylaştırır. Dinamik ortamlarda değişiklikleri sık ve izlemek zor olduğu, VA veri Ambarınızı saldırılarına karşı savunmasız bırakabilir ayarları algılamada her zaman benzersizdir.

## <a name="improved-availability-with-query-restartability"></a>Sorgu restartability ile geliştirilmiş kullanılabilirlik
Sorgu yürütme sırasında herhangi bir sayıda sorunları ortaya çıkabilir sorgunun başarısız olmasına neden olabilir. Bir ağ kesintisi, bir donanım hatası veya diğer bağlantı kesilmesi bir kesintiye neden olabilir. SQL veri ambarı sorgu restartability adım veya deyimi düzeyi SELECT sorgusu için artık desteklemektedir. 

Sorgu Restartability ile bir hata nedeniyle kesintiye uçuştaki bir sorgu otomatik olarak yeniden başlatılır. Adımlar sorgu şeklini sayısına bağlı olarak veya sorgu yürütülürken durduruldu olduğunda, SQL veri ambarı altyapısını tam sorgu ya da yeniden başlatılır veya son tamamlanan sorgu adımından devam edecek. Bir son kullanıcı bakış açısından sorgu yalnızca tamamlar. 

## <a name="maintenance-scheduling-preview"></a>Bakım zamanlaması (Önizleme)
Azure SQL veri ambarı bakım zamanlama artık Önizleme aşamasındadır. Bu yeni özellik, hizmet durumu planlı bakım bildirimlerini, kaynak sistem durumu İzleyicisi'ni kontrol edin ve Azure SQL veri ambarı bakım zamanlama hizmeti sorunsuzca tümleştirilir. Bakım zamanlaması, yeni özellikleri, yükseltmeleri almak uygun olduğunda, bir zaman penceresi zamanlamanıza olanak sağlar ve düzeltme ekleri.

Bakım zamanlama, Azure İzleyici yararlanır ve yaklaşan Bakımı olayları almak istedikleri ve hangi akışları otomatik kapalı kalma süresi yönetmek ve işlemlerini etkisini en aza indirmek için tetiklenmesinin gerekip nasıl uyacaklarını belirlemelerini sağlar. Bildirimler, bir e-posta veya metin içerebilir. 

## <a name="wide-row-support-in-polybase"></a>Polybase çok satır desteği
SQL Data Warehouse'a veri yükleme, genel kılavuz kullanmaktır [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading#options-for-loading-with-polybase) olan paralel veri yükleme desteği. Bu sürüm, daha geniş 32 K sütunlarından 1 MB - geniş satır boyutu tablolarla yüklemenize izin verme için desteği etkinleştirir. Veri yükleme işlemi geniş bir satır içeren tablolar için geniş bir satır için destek basitleştirir.

> [!Note]
> Toplam satır boyutu 1 MB boyutunda aşamaz.

## <a name="additional-language-support"></a>Ek dil desteği
Bu sürüm aşağıdaki T-SQL dil öğeleri için destek sunar:

### <a name="stringsplit"></a>STRING_SPLIT
[STRING_SPLIT](https://docs.microsoft.com/sql/t-sql/functions/string-split-transact-sql) işlevi, belirtilen ayırıcıyı kullanarak bir karakter dizesi ayırır. Bir sütun ayrıştırma ve başka bir tabloya yerleştirmek üzere birden çok değer olduğu senaryoları veri yükleme STRING_SPLIT faydalıdır.

#### <a name="example"></a>Örnek
```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike';

SELECT
    value
FROM
    STRING_SPLIT(@tags, ',')
WHERE
    RTRIM(value) <> '';
```

### <a name="compressdecompress-functions"></a>SIKIŞTIR/DECOMPRESS işlevleri
[SIKIŞTIRMA](https://docs.microsoft.com/sql/t-sql/functions/compress-transact-sql) / [DECOMPRESS](https://docs.microsoft.com/sql/t-sql/functions/decompress-transact-sql) işlevleri sıkıştırabilir veya bir dize GZIP algoritmasıyla giriş sıkıştırmasını olanak sağlar.

#### <a name="example"></a>Örnek

```sql
SELECT
    name [name_original]
    , COMPRESS(name) [name_compressed]
    , CAST(DECOMPRESS(COMPRESS(name)) AS NVARCHAR(MAX)) [name_decompressed]
FROM
    sys.objects;
```

### <a name="if-exists-clause-for-dropping-views"></a>IF EXISTS yan tümcesinden görünümleri silmek için
Eğer mevcut yan tümcesinde eklenmesini [açılan VIEW](https://docs.microsoft.com/sql/t-sql/statements/drop-view-transact-sql) deyimi bir görünüm veri ambarından kaldırmak için gerekli T-SQL kodunu basitleştirir. Mevcut ya da deyim görünümü mevcut değilse yoksayar açılan VIEW ifadesine uygulandığında Eğer mevcut sözdizimini, görünüm kaldıracağız.

#### <a name="example"></a>Örnek
```sql
DROP VIEW IF EXISTS dbo.TestView;
```
```
Message
--------------------------------------------------
Commands completed successfully.

```

## <a name="improved-compilation-time-for-singleton-inserts-and-ddl-statements"></a>Geliştirilmiş derleme zamanı tekil ekler ve DDL deyimleri 
Veri ekleme, genellikle bir singleton INSERT çalışan Liderleri için geleneksel bir ayıklama, dönüştürme ve yükleme (ETL) modeli aşağıdaki ([DEĞERLERİN EKLENECEĞİ](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql)) veritabanındaki bir tabloya. Bu sürüm, bu tür bir deyimi yürütmek için gerekli olan derleme zamanı azaltarak tekil ekleme işlemlerinin performansını artırır. Bazı durumlarda, gözlemlenen derleme geliştirme 3 kata kadar daha hızlı olur. Bu geliştirme, tekil INSERT deyimini yürütmek için gereken süreyi azaltır. 

Geliştirme, veri ambarları ile çok sayıda benzer şekilde azaltarak nesneleri oluşturun, dahil olmak üzere veri tanımlama dili (DDL) bildirimleri için sorgu derleme zamanı ALTER ve silme işlemleri de fayda sağlar. 

Son olarak, geliştirme, geniş tabloların - büyük sayıda sütuna sahip tablolar üzerinde yürütülen deyimleri genel yürütülmesini azaltır. Geliştirme genel sorgular yürütme süresini azaltır, sorgu derleme adımı sürede azalmaya neden olur.

## <a name="bug-fixes"></a>Hata düzeltmeleri

| Unvan | Açıklama |
|:---|:---|
| **Benzersiz kısıtlamalar için dağıtımlarında istatistikleri oluşturma düzeltildi** | Bu düzeltme çalışan UPDATE STATISTICS yalnızca tablo ile belirtilen, tabloda tanımlanan benzersiz kısıtlama sahip olduğunda kullanıcıların karşılaştığınız bir hatayı ele alır. |
| **Sorguları dış tablolar derlenirken düzeltme** | Bu düzeltme, derleme zamanı dış tablolar içeren sorgular için etkilenen bir kusur yöneliktir.|
| **Büyük türleriyle deyimi yürütüp düzeltildi** | Hazırlanmış deyim derleme bir üründe biri olarak bildirilen parametreleri ele *büyük* türleri (nvarchar(max), varchar(max) ve varbinary(max)). |
| **Bir hata oluştuğunda derin şekilde iç içe geçmiş sorgular için düzeltme** | İç içe bir sorgu sistem sınırı aştığında bir hata iletisini Temizle sağlar.|
| **Bağıntılı alt sorgu ve yürütme süresi sabit bir ifade içerdiğinde, derleme zamanı hatalarını düzeltme** |Derleme zamanı hatası bağıntılı alt sorgular ve yürütme zamanı sabitleri (gibi GETDATE()). belirli bir birleşimini içeren sorgular için adresleri|
| **PDW nesne kilitleri ve autostats için eşzamanlılık yuvası almak için adresi zaman aşımı** |Düzeltme autostats istek uzun bir süredir kaynak isteklerini engellemesini önleyecek kilit zaman aşımı ekler.|

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
