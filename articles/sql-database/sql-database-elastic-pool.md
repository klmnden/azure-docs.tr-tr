---
title: Azure elastik havuzu nedir? | Microsoft Belgeleri
description: "Yüzlerce veya binlerce veritabanının bir havuz kullanarak yönetin. Havuz üzerindeki bir dizi performans birimi için tek fiyat dağıtılabilir. Veritabanlarını istediğiniz zaman içeri veya dışarı taşıyın."
keywords: "elastik havuz, sql veritabanları"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 01/11/2017
ms.author: CarlRabeler
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2681dd3792a351fecc0c72eb7fe546113a451d24
ms.openlocfilehash: 412c3f9c34e399ecdffd939b5b72e687a847b8e1


---
# <a name="what-is-an-azure-elastic-pool"></a>Azure elastik havuzu nedir?
SQL Veritabanı Elastik havuzları, son derece farklı ve öngörülemeyen kullanım düzenlerine sahip birden çok veritabanına ilişkin performans hedeflerini yönetmek için basit ve uygun maliyetli bir çözüm sağlar.

> [!NOTE]
> Esnek havuzlar şu anda önizleme aşamasında oldukları Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgede esnek havuz GA’sı olabildiğince çabuk ortaya çıkar.
>
>

## <a name="how-it-works"></a>Nasıl çalışır?
Yaygın bir SaaS uygulama modeli ise tek kiracılı veritabanı modelidir: her müşteriye kendi veritabanı verilir. Her müşteri (veritabanı) bellek, G/Ç ve CPU için öngörülemeyen kaynak gereksinimlerine sahiptir. Talepteki bu iniş çıkışlarla birlikte kaynakları verimli ve uygun maliyetli bir şekilde nasıl ayırabilirsiniz? Geleneksel olarak iki seçeneğiniz vardır: (1) en yüksek kullanımı temel alarak kaynakları fazladan sağlama ve fazla ödeme yapma ya da (2) en yüksek kullanım dönemlerinde performans ve müşteri memnuniyetini riske atarak maliyetten tasarruf etmek için eksik sağlama. Elastik havuzlar, veritabanlarının gerektiğinde gerekli olan performans kaynaklarını almasını sağlayarak bu sorunu çözmektedir. Bunlar, tahmin edilebilir bir bütçe içinde basit bir kaynak ayırma mekanizması sağlar. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

SQL Veritabanında bir veritabanının kaynak taleplerini ele alma becerisinin göreli ölçümü, tek veritabanları için Veritabanı İşlem Birimleri (DTU), elastik havuzdaki veritabanları içinse elastik DTU (eDTU) olarak ifade edilir. DTU ve eDTU’lar hakkında daha fazla bilgi için bkz. [SQL Veritabanına Giriş](sql-database-technical-overview.md).

Havuza belirli bir fiyat karşılığında, belirli bir sayıda eDTU verilir. Havuz içerisinde tek tek veritabanlarına belirli parametreler içinde otomatik olarak ölçeklendirme esnekliği tanınır. Veritabanı, yoğun bir yük altındayken talebi karşılamak üzere daha fazla eDTU kullanabilir. Yükü az olan veritabanları daha az eDTU kullanır ve yükü bulunmayan veritabanları eDTU kullanmaz. Tek tek veritabanları yerine tüm havuz için kaynak sağlamak, yönetim görevlerinizi basitleştirir. Ayrıca, havuza yönelik bütçeniz tahmin edilebilir bir hale gelir.

Var olan havuza veritabanı kesintisi yaşanmadan ek eDTU’lar eklenebilir ancak veritabanlarının yeni eDTU ayrımı için ek işlem kaynaklarını sunmak üzere taşınması gerekebilir. Benzer şekilde, ek eDTU’lara artık ihtiyaç yoksa bunlar mevcut bir havuzdan ne zaman isterseniz kaldırılabilir.

Ayrıca havuza veritabanları ekleyebilir veya havuzdan veritabanları kaldırabilirsiniz. Bir veritabanı kaynakları tahmin edilebilir bir şekilde normalden az kullanıyorsa bu veritabanını havuzdan çıkarın.

## <a name="which-databases-go-in-a-pool"></a>Hangi veritabanları havuza eklenir?
![Bir elastik havuzda eDTU'ları paylaşan SQL veritabanları.][1]

Elastik havuzlar için mükemmel adaylar olan veritabanlarında, genellikle etkin ve pasif dönemler olur. Yukarıdaki dönemde tek veritabanının, 4 veritabanının ve son olarak 20 veritabanı içeren bir elastik havuzun etkinliği gösterilmektedir. Zaman içinde etkinlik düzeyi değişen veritabanları her zaman etkin olmadığı ve eDTU’ları paylaşabildiği için elastik havuzlara yönelik mükemmel adaylardır. Tüm veritabanları bu modele uymaz. Daha sabit bir kaynak talebine sahip veritabanları, kaynakların tek tek atandığı Temel, Standart ve Premium hizmet katmanlarına daha uygundur.

[Elastik havuzlar için fiyat ve performans ile ilgili dikkat edilmesi gerekenler](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>Elastik havuzlar için eDTU ve depolama sınırları

Aşağıdaki tabloda Temel, Standart ve Premium elastik havuzların özellikleri açıklanmaktadır.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır.  SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

## <a name="elastic-pool-properties"></a>Elastik havuz özellikleri

Aşağıdaki tablolarda elastik havuzlara ve havuza alınmış veritabanlarına ilişkin sınırlar açıklanmıştır.

### <a name="limits-for-elastic-pools"></a>Elastik havuzlar için limitler
| Özellik | Açıklama |
|:--- |:--- |
| Hizmet katmanı |Temel, Standart veya Premium. Hizmet katmanı, yapılandırılabilen performans ve depolama limitlerinin yanı sıra iş sürekliliği seçeneklerinin aralığını belirler. Bir havuzdaki her veritabanı, havuzla aynı hizmet katmanına sahiptir. “Hizmet katmanı”, “sürüm” olarak da adlandırılır. |
| Havuz başına eDTU |Havuzdaki veritabanları tarafından paylaşılabilen en fazla eDTU sayısı. Havuzdaki veritabanları tarafından kullanılan toplam eDTU sayısı, zaman içindeki aynı noktada bu sınırı aşamaz. |
| Havuz başına maks. depolama alanı (GB) |Havuzdaki veritabanları tarafından paylaşılabilen GB cinsinden en büyük depolama miktarı. Havuzdaki veritabanları tarafından kullanılan toplam depolama alanı bu sınırı aşamaz. Bu sınır, havuz başına eDTU sayısına göre belirlenir. Bu sınır aşılırsa tüm veritabanları salt okunur hale gelir. Havuz başına düşen en fazla depolama alanı, havuzdaki veri dosyalarına ayrılan en fazla depolama alanını ifade eder ve günlük dosyalarının kullandığı alanı içermez. |
| Havuz başına maks. veritabanı sayısı |Havuz başına izin verilen en fazla veritabanı sayısı. |
| Havuz başına maks. eş zamanlı çalışan |Havuzdaki tüm veritabanları için kullanılabilen en fazla eşzamanlı çalışan (istek) sayısı. |
| Havuz başına maks. eş zamanlı oturum |Havuzdaki tüm veritabanları için en fazla eşzamanlı oturum açma sayısı. |
| Havuz başına maks. eş zamanlı oturum |Havuzdaki tüm veritabanları için kullanılabilen en fazla oturum sayısı. |

### <a name="limits-for-pooled-databases"></a>Havuza alınmış veritabanları için sınırlar
| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı.  Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez.  Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım.  Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir.  Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz.  Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına maks. depolama alanı (GB) |Bir havuzdaki veritabanı için en fazla depolama alanı. Havuza alınmış veritabanları, havuz depolama alanını paylaşır. Bu nedenle veritabanı depolama alanı, kalan havuz depolama alanı ve veritabanı başına maksimum depolama alanı değerlerinin hangisi daha küçükse bu değerle sınırlıdır. Veritabanı başına düşen maksimum depolama alanı, veri dosyalarının maksimum boyutunu ifade eder ve günlük dosyalarının kullandığı alanı içermez. |

## <a name="elastic-jobs"></a>Esnek işler
Bir havuz kullanılarak **[esnek işlerde](sql-database-elastic-jobs-overview.md)** betik çalıştırma yoluyla yönetim görevleri kolaylaştırılır. Elastik iş, çok sayıda veritabanından kaynaklanan sorunların çoğunu ortadan kaldırır. Başlamak için bkz. [Elastik işlerle çalışmaya başlama](sql-database-elastic-jobs-getting-started.md).

Birden fazla veritabanıyla çalışmak için kullanılabilen diğer veritabanı araçları hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Bir havuzdaki veritabanları için iş sürekliliği özellikleri
Havuza alınan veritabanları genellikle tek veritabanları için kullanılabilen [iş sürekliliği özelliklerinin](sql-database-business-continuity.md) aynılarını destekler.

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme
Belirli bir noktaya geri yükleme işlemi, bir havuzdaki veritabanını zamandaki belirli bir noktaya geri yüklemek için otomatik veritabanı yedeklemelerini kullanır. Bkz. [Belirli Bir Noktaya Geri Yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Coğrafi Geri Yükleme
Coğrafi Geri Yükleme, bir veritabanı barındırıldığı bölgedeki bir olay nedeniyle kullanılamaz olduğunda varsayılan kurtarma seçeneğini sağlar. Bkz. [Bir Azure SQL Veritabanını geri yükleme veya ikincil veritabanına yük devretme](sql-database-disaster-recovery.md)

### <a name="active-geo-replication"></a>Etkin Coğrafi Çoğaltma
Coğrafi Geri Yüklemenin sunabileceğinden daha agresif kurtarma gereksinimleri olan uygulamalar için [Azure portalını](sql-database-geo-replication-portal.md), [PowerShell’i](sql-database-geo-replication-powershell.md) veya [Transact-SQL’i](sql-database-geo-replication-transact-sql.md) kullanarak Etkin Coğrafi Çoğaltma özelliğini yapılandırın.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure SQL Veritabanı elastiklik özellikleriyle ilgili Microsoft Virtual Academy video dersi](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png



<!--HONumber=Jan17_HO2-->


