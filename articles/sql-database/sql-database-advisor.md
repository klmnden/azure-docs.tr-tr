---
title: Performans önerileri - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı geçerli sorgu performansını artırmak, SQL veritabanları için öneriler sağlar.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 4cee9fd9072e2db9a5f90a84b7683a76bba2504d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644027"
---
# <a name="performance-recommendations-for-sql-database"></a>SQL veritabanı için performans önerileri

Azure SQL Database öğrenir ve uygulamanız ile uyum sağlar. SQL veritabanlarının performansını en üst düzeye çıkarmanıza olanak tanıyan özelleştirilmiş önerileri sağlar. SQL veritabanı sürekli olarak değerlendirir ve SQL veritabanlarının kullanım geçmişini analiz eder. Sağlanan öneriler, veritabanı benzersiz iş yükü düzenlerini esas alarak ve performansının artırılmasına yardımcı.

> [!TIP]
> [Otomatik ayarlama](sql-database-automatic-tuning.md) performans ayarlaması için önerilen yöntemdir. [Akıllı Öngörüler](sql-database-intelligent-insights.md) performansı izlemek için önerilen yöntemdir. 
>

## <a name="create-index-recommendations"></a>Dizin önerileri oluşturma
SQL veritabanı sürekli olarak çalışan sorguları izler ve performansı artırmak dizinleri tanımlar. Belirli bir dizin eksik olduğunu yeterli güvenirlik sonra yeni bir **Create Index** öneri oluşturulur.

 Azure SQL veritabanı dizin tamamlanışında sunacağı performans kazancı tahminlerle güven oluşturur. Tahmini performans kazancını bağlı olarak öneriler yüksek, Orta veya düşük olarak kategorilere ayrılır. 

Önerileri kullanarak oluşturulan dizinleri her zaman otomatik olarak oluşturulan dizinleri işaretlenir. Hangi dizinleri otomatik oluşturulan sys.indexes Sergi bakarak görebilirsiniz. Otomatik oluşturulan dizinleri ALTER/yeniden adlandırma komutları engelleme. 

Otomatik oluşturulan dizin üzerinde sahip sütun bırakma çalışırsanız, komut geçirir. Otomatik oluşturulan dizini komutuyla bırakılır. Normal dizin dizini sütunlar üzerinde ALTER/RENAME komutu engelleyin.

Create Index öneri uygulandıktan sonra Azure SQL Database sorguları performans taban çizgisi performansı ile karşılaştırır. Yeni dizin performans geliştirilmiş, öneri başarılı olarak işaretlenir ve etkisi raporu kullanılabilir. Dizin performansı alamadık, otomatik olarak döndü. SQL veritabanı önerileri veritabanı performansını iyileştirmek emin olmak için bu işlemi kullanır.

Tüm **dizin oluşturma** önerisi havuzu veya veritabanı kaynak kullanımı yüksekse, öneri uygulama izin vermeyen bir geri alma İlkesi yok. Geri alma İlkesi, hesap CPU, veri g/ç, günlük GÇ ve kullanılabilir depolama alanı alır. 

Önceki 30 dakika içinde % 80 ' daha yüksek ise, CPU, veri g/ç veya günlük GÇ oluşturma dizin önerisi ertelenir. Dizin oluşturulduktan sonra kullanılabilir depolama alanı % 10 olacaksa öneri bir hata durumuna geçtiğinde. Birkaç gün sonra otomatik ayarlama hala dizini yararlı olacaktır inanırsa, işlemi yeniden başlatır. 

Bu işlem, dizin oluşturmak için yeterli kullanılabilir depolama alanı kadar ya da dizin olarak yararlı artık görülen değil kadar yinelenir.

## <a name="drop-index-recommendations"></a>Dizin kaldırma önerileri
Eksik dizinler algılama yanı sıra, SQL veritabanı sürekli varolan dizinleri performansını analiz eder. Bir dizin kullanılmazsa, Azure SQL veritabanı bırakmadan önerir. Bir dizini bırakmadan iki durumda da önerilir:
* Dizin başka bir dizin (aynı dizine ve sütun, bölüm şeması ve filtreleri dahil) bir kopyası.
* Dizin uzun süren bir süre (93 gün) için kullanılmamış.

Dizin kaldırma önerileri de uygulama sonra doğrulama geçer. Performansı artırır, etkisi raporu kullanılabilir. Performans düşerse, öneri geri döndürüldü.


## <a name="parameterize-queries-recommendations"></a>Sorguları önerileri Parametreleştirme
*Sorguları Parametreleştirme* önerileri sürekli son yukarı ile aynı sorgu yürütme planı derlenir bir veya daha fazla sorgular varsa görünür. Bu durum zorlanmış parametrelemeyi uygulamak için bir fırsat oluşturur. Zorlanmış parametrelemeyi performansını artırır ve kaynak kullanımını azaltır önbelleğe ve gelecekte yeniden sorgu planlarını sırayla izin verir. 

SQL Server karşı başlangıçta verilen her sorgu yürütme planı oluşturmak için derlenmesi gerekiyor. Oluşturulan her plan planı önbelleğe eklenir. Sonraki yürütmelerde aynı sorgunun önbelleğinden ek derleme gereksinimini ortadan kaldırır, bu plan yeniden kullanabilirsiniz. 

Yürütme planı parametresiz farklı değerler her zaman yeniden derlenmesi için parametreli olmayan değerleri sorgularıyla performansa için yol açabilir. Çoğu durumda, aynı sorguları farklı parametre değerleri ile aynı yürütme planları oluşturun. Bu planlar ancak hala ayrı olarak planı önbelleğe eklenir. 

Yürütme planları yeniden derlenmesi işlemi veritabanı kaynaklarını kullanır, sorgu süresini artırır ve plan önbelleğinin taşar. Bu olaylar sırayla planları önbellekten çıkarılmasına neden. Bu SQL Server davranış veritabanında zorlanmış parametrelemeyi seçeneğini ayarlayarak değiştirilebilir. 

(Öneri uygulandıysa, gibi) Bu öneri etkisini tahmin etmenize yardımcı olmak için gerçek CPU kullanımı ve tahmini CPU kullanımı arasında bir karşılaştırma sağlanır. Bu öneri, CPU tasarrufu kazanmanıza yardımcı olabilir. Ayrıca sorgu süresini azaltın yardımcı olur ve ek yükü planı önbelleği için hangi planları birkaçını önbellekte kalır ve yeniden anlamına gelir. Seçerek bu öneri hızla uygulayabilirsiniz **Uygula** komutu. 

Bu öneri uyguladıktan sonra veritabanınızdaki dakika içinde zorlanmış parametrelemeyi sağlar. Yaklaşık 24 saat boyunca sürer izleme işlemi başlatır. Bu süre, doğrulama raporunu görebilirsiniz. Bu rapor, 24 saat önce ve öneri uygulandıktan sonra veritabanınız CPU kullanımını gösterir. SQL veritabanı Danışmanı performans regresyon algıladıysa, otomatik olarak uygulanan öneri döner bir güvenlik mekanizması vardır.

## <a name="fix-schema-issues-recommendations-preview"></a>Şema sorunları önerileri (Önizleme) Düzelt

> [!IMPORTANT]
> Microsoft, şu anda "şema sorunu düzeltin" önerileri kaldırmaktadır. Kullanmanızı öneririz [akıllı Öngörüler](sql-database-intelligent-insights.md) "şema sorunu düzeltin" önerileri daha önce ele alınan şema sorunları da dahil olmak üzere, veritabanı performans sorunlarını izlemek için.
> 

**Şema sorunları giderin** önerileri SQL veritabanı hizmetinin bir anomali SQL veritabanınız gerçekleştiği şema ile ilgili SQL hataları sayısındaki bildirimler olduğunda görüntülenir. Bu öneri, genellikle veritabanınızı bir saat içinde birden çok şema ile ilgili hataları (geçersiz sütun adı, geçersiz nesne adı vb.) karşılaştığında görüntülenir.

"Şema" SQL Server'da sözdizimi hataları sınıfının sorunlardır. SQL sorgusu tanımını ve veritabanı şeması tanımı hizalanmamış bunlar oluşur. Örneğin, sorgu tarafından beklenen sütunlardan biri hedef tablo ya da tam tersini eksik olabilir. 

Azure SQL veritabanı hizmetinin bir anomali SQL veritabanınız gerçekleştiği şema ile ilgili SQL hataları sayısındaki bildirimler "şema sorunu düzeltin" öneri görünür. Aşağıdaki tabloda şema sorunlarıyla ilgili hataları gösterilmektedir:

| SQL hata kodu | İleti |
| --- | --- |
| 201 |Yordamı veya işlevi '*'parametresini beklemektedir'*', hangi değil sağlandı. |
| 207 |Geçersiz sütun adı ' *'. |
| 208 |Geçersiz nesne adı ' *'. |
| 213 |Sütun adı veya numarası sağlanan değerlerin tablo tanımı eşleşmiyor. |
| 2812 |Saklı yordamı bulunamadı. ' *'. |
| 8144 |Yordam veya işlev * belirtilen çok fazla bağımsız değişkenlere sahiptir. |

## <a name="next-steps"></a>Sonraki adımlar
Önerilerinizi izleyebilir ve bunları performansı iyileştirmek için uygulanmaya devam eder. Veritabanı iş yüklerini, dinamik ve sürekli olarak değiştirin. SQL veritabanı Danışmanı izlemek ve veritabanınızın performansı artırmak öneriler sunmak devam eder. 

* Veritabanı dizinlerini ve sorgu yürütme planları otomatik ayarlama hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md).
* Otomatik olarak otomatik tanılama ve performans sorunu kök neden Analizi ile veritabanı performansını izleme hakkında daha fazla bilgi için bkz: [Azure SQL akıllı Öngörüler](sql-database-intelligent-insights.md).
*  Azure portalında performans önerileri kullanma hakkında daha fazla bilgi için bkz: [performans önerileri Azure portalında](sql-database-advisor-portal.md).
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) hakkında bilgi edinin ve üst sorgularınızı performans etkisini görüntülemek için.


