---
title: Performans önerileri - Azure SQL veritabanı | Microsoft Docs
description: Azure SQL veritabanı, geçerli sorgu performansını iyileştirebilir SQL veritabanlarınız için öneriler sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: d09adbfa7cb2782d710ef3116cbd7bc68ee247b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61417592"
---
# <a name="performance-recommendations-for-sql-database"></a>SQL veritabanı için performans önerileri

Azure SQL veritabanı öğrenir ve uygulamanızla birlikte uyum sağlar. Bu, SQL veritabanlarınızın performansını en üst düzeye olanak tanıyan özelleştirilmiş önerileri sağlar. SQL veritabanı, sürekli olarak değerlendirir ve Kullanım Geçmişi SQL veritabanlarınızın analiz eder. Sağlanan öneriler, veritabanı benzersiz iş yükü düzenleri temelinde ve performansının artırılmasına yardımcı.

> [!TIP]
> [Otomatik ayarlama](sql-database-automatic-tuning.md) en yaygın veritabanı performans sorunlarından bazıları otomatik olarak ayarlamak için önerilen yöntemdir. [Sorgu performansı öngörüleri](sql-database-query-performance.md) temel Azure SQL veritabanı performans izleme ihtiyaçları için önerilen yöntemdir. [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md) Gelişmiş otomatik performans sorunlarını gidermek için yerleşik zeka sayesinde, uygun ölçekte, veritabanı performansını izleme için ve önerilen yöntem budur.
>

## <a name="create-index-recommendations"></a>Dizin önerileri oluşturma
SQL veritabanı, sürekli olarak çalışan sorguları izler ve performans geliştirebileceğimiz dizinleri tanımlar. Belirli bir dizin eksik olduğunu yeterli güvenle sonra yeni bir **Create Index** öneri oluşturulur.

 Azure SQL veritabanı dizini aracılığıyla sunacağı performans kazanç tahmin ederek güvenle oluşturur. Tahmini performans kazancı bağlı olarak, öneriler, yüksek, Orta veya düşük olarak kategorilere ayrılır. 

Önerileri kullanılarak oluşturulan dizinleri her zaman otomatik olarak oluşturulan dizinleri işaretlenir. Hangi dizin otomatik olarak oluşturulmuş sys.indexes görünümünde bakarak görebilirsiniz. Otomatik oluşturulan dizinleri Değiştir/yeniden adlandırma komutları engellemez. 

Otomatik olarak oluşturulmuş bir dizin üzerinde olan sütunu çalışırsanız, komut geçirir. Otomatik olarak oluşturulmuş dizini komutuyla bırakılır. Normal dizin dizinlenir sütunları ALTER/yeniden adlandır komut engelleyin.

Create Index öneri uygulandıktan sonra Azure SQL veritabanı sorgulama performansının taban çizgisi performansı ile karşılaştırır. Yeni bir dizin Gelişmiş performans, öneri başarılı olarak işaretlenir ve etkisi raporu kullanılabilir. Dizin performansını alamadık, otomatik olarak döndürüldü. SQL veritabanı, önerileri veritabanı performansını artırmak emin olmak için bu işlemi kullanır.

Tüm **dizin oluşturma** havuz veya veritabanı kaynak kullanımı yüksekse, öneriyi uygulama izin vermeyen bir geri alma ilkesinin önerisi vardır. Geri alma İlkesi, hesap CPU, veri GÇ, günlük GÇ ve kullanılabilir depolama alanı alır. 

CPU, veri GÇ ve günlük GÇ önceki 30 dakika içerisinde % 80 ' daha yüksek olması durumunda, create Index öneri ertelenir. Dizin oluşturulduktan sonra kullanılabilir depolama alanı % 10 olacaksa, öneri, bir hata durumuna geçtiğinde. Birkaç gün sonra otomatik ayarlama hala dizin yararlı olduğunu düşündüğü, işlemi yeniden başlatır. 

Bu işlem, bir dizin oluşturmak için yeterli kullanılabilir depolama alanı oluncaya kadar veya dizin olarak yararlı artık görülen değil kadar yinelenir.

## <a name="drop-index-recommendations"></a>Bırakma dizin önerileri
Eksik dizinleri algılama yanı sıra SQL veritabanı sürekli olarak mevcut dizinleri performansını analiz eder. Dizin kullanılmıyorsa, Azure SQL veritabanı bırakmadan önerir. Bir dizini bırakmadan, iki durumda da önerilir:
* Dizini (aynı dizine ve sütun bölüm şeması ve filtreler dahil) başka bir dizinin yineleniyor.
* Dizin uzun bir süre (93 gün) için önce kullanılmıştır.

Doğrulamadan sonra uygulama da açılan dizin önerileri inceleyin. Etkisi raporu performansını artırır, kullanılabilir. Performans düşerse, öneri geri döndürüldü.


## <a name="parameterize-queries-recommendations"></a>Öneriler sorguları Parametreleştirme
*Sorguları parametrele* önerileri sürekli son yedekleme aynı sorgu yürütme planı ile derlenen bir veya daha fazla sorgu olduğunda görünür. Bu durum, zorlamalı Parametreleştirme uygulamak için bir fırsat oluşturur. Zorlanmış Parametreleştirme sırayla, performansı geliştirir ve kaynak kullanımını azaltır önbelleğe alınır ve gelecekte yeniden için sorgu planlarına sağlar. 

SQL Server karşı başlangıçta verilen her bir sorgu yürütme planı oluşturmak için derlenmesi gerekir. Oluşturulan her plan planı önbelleğe eklenir. Bu plan önbelleğinden ek derleme ihtiyacını ortadan kaldırır aynı sorgunun sonraki yürütmeleri yeniden kullanabilirsiniz. 

Yürütme planını forceseek farklı değerler her zaman yeniden derlenen çünkü değerlerle parametreleştirilmemiş sorgular performans yükü neden olabilir. Çoğu durumda, aynı sorgu farklı parametre değerleri ile aynı yürütme planlarını oluşturur. Bu planları, ancak yine de ayrı olarak planı önbelleğe eklenir. 

Yürütme planlarını yeniden derleme işlemi veritabanı kaynakları kullanır, sorgu süresini artırır ve planı önbellek taşıyor. Bu olayların sırayla planları önbellekten çıkarılmasına neden. Bu SQL Server davranışı, veritabanındaki zorlamalı Parametreleştirme seçeneğini ayarlayarak değiştirilebilir. 

(Öneri uygulandıysa gibi) bu öneriyi etkisini tahmin etmenize yardımcı olmak için gerçek CPU kullanımı ve tahmini CPU kullanımı arasında bir karşılaştırma sağlanır. Bu öneri, CPU tasarrufu kazanmanıza yardımcı olabilir. Ayrıca sorgu süresini azaltmak yardımcı olur ve ek yükü planı önbellek için planların daha fazla önbellekte kalabilir ve yeniden anlamına gelir. Seçerek bu öneriyi hızlı bir şekilde uygulayabilirsiniz **Uygula** komutu. 

Bu öneri uygulandıktan sonra veritabanınıza dakikalar içinde zorunlu Parametreleştirme sağlar. Yaklaşık 24 saat boyunca sürer izleme işlemi başlar. Bu süre bittikten sonra doğrulama raporu görebilirsiniz. Bu rapor, 24 saat önce ve öneri uygulandıktan sonra veritabanı CPU kullanımını gösterir. SQL veritabanı Danışmanı performans regresyon algılanırsa, otomatik olarak uygulanan öneri döner bir güvenlik mekanizması vardır.

## <a name="fix-schema-issues-recommendations-preview"></a>Şema sorunlarını önerileri (Önizleme) Düzelt

> [!IMPORTANT]
> Microsoft şu anda önerileri "şema sorunu" kullanımdan kaldırılıyor. Kullanmanızı öneririz [Intelligent Insights](sql-database-intelligent-insights.md) daha önce "şema sorunu" önerileri ele şema sorunları da dahil olmak üzere, veritabanı performans sorunlarını izlemek için.
> 

**Şema sorunlarını düzelt** öneriler, SQL veritabanı hizmeti SQL veritabanı'nda oluşmasını şema ile ilgili SQL hataları sayısı bir anomali istediğinde görünür. Bu öneri, genellikle veritabanınızı bir saat içinde birden çok şema ile ilgili hataları (geçersiz sütun adı, geçersiz nesne adı vb.) karşılaştığında görünür.

"Şema sorunları" söz dizimi hataları SQL Server'daki bir sınıfa dahildir. SQL sorgu tanımı hem de veritabanı şeması tanımı hizalanmamış bunlar ortaya çıkar. Örneğin, sorgu tarafından beklenen sütunlardan birinin, hedef tabloda veya tam tersi eksik olabilir. 

Azure SQL veritabanı hizmeti SQL veritabanı'nda oluşmasını şema ile ilgili SQL hataları sayısı bir anomali istediğinde "şema sorunu" öneri görünür. Aşağıdaki tablo şema sorunlarıyla ilgili hataları gösterir:

| SQL hata kodu | `Message` |
| --- | --- |
| 201 |Yordamı veya işlevi ' *'parametresini bekliyor'* ', hangi sağlanmadı. |
| 207 |Geçersiz sütun adı ' *'. |
| 208 |Geçersiz nesne adı ' *'. |
| 213 |Sütun adı veya numarası sağlanan değerlerin tablo tanımı eşleşmiyor. |
| 2812 |Saklı yordamı bulunamadı. ' *'. |
| 8144 |Yordamı veya işlevi * çok fazla bağımsız değişken belirtildi. |

## <a name="custom-applications"></a>Özel uygulamalar

Geliştiriciler, Azure SQL veritabanı için performans önerisi kullanan özel uygulamalar geliştirmeye göz önünde bulundurabilirsiniz. Tüm önerilerin bir veritabanı erişilebilir portalda listelenen [Get-AzSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) API.

## <a name="next-steps"></a>Sonraki adımlar
Önerilerinizi izleyin ve performansı iyileştirmek için bunları uygulanmaya devam eder. Veritabanı iş yüklerini, dinamik ve sürekli olarak değiştirin. SQL veritabanı Danışmanı, büyük olasılıkla veritabanınızın performansını iyileştirebilir önerileri sağlamak ve izlemek devam eder. 

* Veritabanı dizinleri ve sorgu yürütme planlarını otomatik ayarlama hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı otomatik ayarlama](sql-database-automatic-tuning.md).
* Otomatik olarak otomatik tanılama ve performans sorunlarını kök neden Analizi ile veritabanı performansını izleme hakkında daha fazla bilgi için bkz. [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md).
*  Performans önerilerini Azure portalında kullanma hakkında daha fazla bilgi için bkz. [performans önerilerini Azure portalında](sql-database-advisor-portal.md).
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) hakkında bilgi edinin ve sık kullandığınız sorguların performans etkisini görüntüleyin.


