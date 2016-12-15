---
title: "Elastik veritabanı havuzu ne zaman kullanılmalıdır?"
description: "Elastik veritabanı havuzu, bir elastik veritabanı grubu tarafından paylaşılan kullanılabilir kaynaklar koleksiyonudur. Bu belgede bir veritabanı grubu için elastik veritabanı havuzu kullanmanın uygunluğunu değerlendirmeye yardımcı olan yönergeler verilmektedir."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 3d3941d5-276c-4fd2-9cc1-9fe8b1e4c96c
ms.service: sql-database
ms.custom: sharded databases pool; app development
ms.devlang: NA
ms.date: 08/08/2016
ms.author: sstein
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 408cf315f8b44c9ebf852c3ccbf2ed93c70ef22f


---
# <a name="when-should-an-elastic-database-pool-be-used"></a>Elastik veritabanı havuzu ne zaman kullanılmalıdır?
Elastik veritabanı havuzu kullanımının uygun maliyetli olup olmadığını değerlendirmek için veritabanı kullanım modellerini ve elastik veritabanı havuzu ile tek veritabanları arasındaki fiyatlandırma farklarını göz önünde bulundurun. Var olan bir SQL veritabanı kümesi için gereken geçerli havuz boyutunu belirlemeye yardımcı olmak üzere ek yönergeler de verilmektedir.  

* Havuzlara genel bakış için bkz. [SQL Veritabanı elastik veritabanı havuzları](sql-database-elastic-pool.md).

> [!NOTE]
> Esnek havuzlar şu anda önizleme aşamasında oldukları Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgedeki elastik havuzların genel kullanım durumları en kısa sürede bildirilecektir.
> 
> 

## <a name="elastic-database-pools"></a>Esnek veritabanı havuzları
SaaS geliştiricileri, birden fazla veritabanından oluşan büyük ölçekli veri katmanlarının üzerinde uygulamalar oluşturur. Her müşteri için tek veritabanı sağlanması yaygın bir uygulama modelidir. Ancak, farklı müşteriler genellikle değişen ve tahmin edilemeyen kullanım modellerine sahiptir ve her veritabanı kullanıcısının kaynak gereksinimlerini tahmin etmek zordur. Bu nedenle geliştirici, tüm veritabanları için üretilen işi ve yanıt sürelerini iyileştirmek amacıyla yüksek maliyetler karşılığında kaynakları fazladan sağlayabilir. Veya geliştirici daha az harcama yapıp müşterileri için performans deneyimini riske atabilir. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Azure SQL Veritabanındaki elastik havuzlar, SaaS geliştiricilerinin bir veritabanı grubuna ait fiyat performansını belirtilen bütçe dahilinde iyileştirmesini ve aynı zamanda her veritabanı için performans Elastikliği sunmasını sağlar. Havuzlar, geliştiricinin tek veritabanları tarafından öngörülemez süreler boyunca kullanımı sağlamak amacıyla birden fazla veritabanı tarafından paylaşılan bir havuz için elastik Veritabanı İşlem Birimleri (eDTU) satın almasına olanak tanır. Bir havuza yönelik eDTU gereksinimi, veritabanlarının toplam kullanımına göre belirlenir. Havuz için kullanılabilen eDTU miktarı, geliştirici bütçesine göre denetlenir. Havuzlar, geliştiricinin havuz için performans üzerindeki bütçe etkisini ve bütçe üzerindeki performans etkisini değerlendirmesini kolaylaştırır. Geliştirici, veritabanlarını havuza ekler, veritabanları için en düşük ve en yüksek eDTU’ları ayarlar ve ardından bütçeyi temel alarak havuzun eDTU değerini ayarlar. Geliştirici, hizmetini zayıf bir başlangıçtan sürekli artan ölçekte olgun bir işletmeye sorunsuzca büyütmek için havuzları kullanabilir.  

## <a name="when-to-consider-a-pool"></a>Ne zaman havuz düşünülmelidir
Havuzlar, belirli kullanım düzenlerine sahip çok sayıda veritabanı bulunan durumlar için çok uygundur. Söz konusu kullanım düzeni, belirli bir veritabanı için ortalama düşük düzeyde kullanım ile nispeten nadir zamanlarda kullanımın ani olarak artması şeklindedir.

Bir havuza ekleyebileceğiniz veritabanı sayısı arttıkça, tasarruflarınız artar. Uygulama kullanım modelinize bağlı olarak, yalnızca iki S3 veritabanı ile tasarruf edildiğini görmek mümkündür.  

Aşağıdaki bölümler veritabanı koleksiyonunuzun bir havuzda olmasının yararlarını nasıl değerlendireceğini anlamanıza yardımcı olur. Örneklerde Standart havuzlar kullanılmaktadır, ancak aynı ilkeler Temel ve Premium havuzlar için de geçerlidir.

### <a name="assessing-database-utilization-patterns"></a>Veritabanı kullanım modellerini değerlendirme
Aşağıdaki şekilde zamanın büyük bölümünü boşta geçiren, ancak düzenli olarak ani etkinlikler sergileyen bir veritabanı örneğini göstermektedir. Bu model bir havuz için uygun olan kullanım modelidir:

   ![havuz için uygun bir tek veritabanı](./media/sql-database-elastic-pool-guidance/one-database.png)

Yukarıda gösterilen beş dakikalık süre boyunca Veritabanı1, 90 DTU’ya kadar yükselir, ancak genel ortalama kullanım beş DTU’dan azdır. Bu iş yükünü tek veritabanında çalıştırmak için S3 performans düzeyi gereklidir, ancak bu düzey düşük etkinlik dönemlerinde kaynakların çoğunu kullanılmamış halde bırakır.

Havuz bu kullanılmayan DTU’ların birden fazla veritabanında paylaşılmasına olanak tanır ve böylece gereken toplam DTU miktarı ile genel maliyeti azaltır.

Önceki örnekten devam ederek, Veritabanı1 ile benzer kullanım modellerine sahip ek veritabanları olduğunu varsayalım. Aşağıdaki ilk iki şekilde, kullanımın zaman içinde örtüşmeyen niteliğini göstermek üzere dört veritabanı ile 20 veritabanının kullanımı aynı grafiğe yerleştirilmiştir:

   ![bir havuz için uygun kullanım modeli ile dört veritabanı](./media/sql-database-elastic-pool-guidance/four-databases.png)

   ![bir havuz için uygun kullanım modeli ile yirmi veritabanı](./media/sql-database-elastic-pool-guidance/twenty-databases.png)

20 veritabanının tamamındaki toplam DTU kullanımı, yukarıdaki şekilde siyah çizgi ile gösterilmiştir. Bu şekil, toplam DTU kullanımının 100 DTU’yu hiçbir zaman aşmadığını ve 20 veritabanının bu süre boyunca 100 eDTU’yu paylaşabileceğini gösterir. Bu durum, tek veritabanları için S3 performans düzeylerindeki veritabanlarının her birini yerleştirmeye kıyasla DTU sayısında 20 kat azalmaya ve 13 kat fiyat azalmasına yol açar.

Bu örnek aşağıdaki nedenlerle idealdir:

* Bir veritabanındaki en yüksek kullanım ile ortalama kullanım arasında büyük farklar mevcuttur.  
* Bir veritabanının en yüksek kullanımı zamanın farklı noktalarında gerçekleşir.
* eDTU’lar çok sayıda veritabanı arasında paylaşılır.

Bir havuzun fiyatı, havuz eDTU'larının bir işlevidir. Bir havuzun eDTU birim fiyatı, tek veritabanının DTU birim fiyatından 1,5 kat fazlayken, **havuz eDTU’ları çok sayıda veritabanı tarafından paylaşılabilir ve bu nedenle birçok durumda toplam eDTU sayısı gereklidir**. Fiyatlandırma ve eDTU paylaşımındaki bu farklılıklar, havuzların sağlayabileceği tasarruf potansiyelinin temelini oluşturur.  

Veritabanı sayısı ve veritabanı kullanımıyla ilgili aşağıdaki temel kurallar, bir havuzun tek veritabanları için kullanılan performans düzeylerine kıyasla daha az maliyet doğurmasını sağlar.

### <a name="minimum-number-of-databases"></a>En az veritabanı sayısı
Tek veritabanı performans düzeyi DTU’larının toplamı, havuz için gerekli eDTU’lardan 1,5 kat fazla ise esne havuz daha uygun maliyetlidir. Kullanılabilir boyutlar için bkz. [Elastik veritabanı havuzları ve elastik veritabanları için eDTU ve depolama limitleri](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

***Örnek***<br>
100 eDTU havuzun tek veritabanı performans düzeylerini kullanmaya kıyasla daha uygun maliyetli olması için en az iki S3 veritabanı veya en az 15 adet S0 veritabanı gereklidir.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Eşzamanlı olarak en üst seviyeye çıkan en fazla veritabanı sayısı
Bir havuzdaki tüm veritabanları, eDTU’ları paylaşarak, tek veritabanı performans düzeylerini kullanırken mevcut olan sınıra kadar eDTU’ları eşzamanlı olarak kullanamaz. Eşzamanlı olarak en üst seviyeye çıkan veritabanı sayısı azaldıkça, havuz eDTU’sunun ayarlanabileceği düzey azalır ve havuz daha uygun maliyetli hale gelir. Genel olarak, havuzdaki veritabanlarının en fazla 2/3’ü (veya %67’si) eDTU sınırına eşzamanlı olarak ulaşmalıdır.

***Örnek***<br>
200 eDTU içeren bir havuzdaki üç S3 veritabanının maliyetlerini azaltmak için, bu veritabanlarının en fazla iki tanesi kullanım sırasında en üst seviyeye çıkabilir.  Aksi takdirde, bu dört S3 veritabanının ikiden fazlası eşzamanlı olarak en üst seviyeye çıkarsa, havuzun boyutu 200 eDTU’dan fazla olmak zorundadır.  Havuz 200 eDTU’dan fazlasına yeniden boyutlandırılırsa, maliyetin tek veritabanı performans düzeylerinden düşük tutulması için havuza daha fazla S3 veritabanının eklenmesi gerekir.  

Bu örnek, havuzdaki diğer veritabanlarının kullanımını dikkate almaz. Herhangi bir zamanda tüm veritabanlarının kullanımı aynı olursa, veritabanlarının 2/3’ünden (veya %67) daha azı eşzamanlı olarak en üst seviyeye çıkabilir.

### <a name="dtu-utilization-per-database"></a>Veritabanı başına DTU kullanımı
Bir veritabanının en yüksek ile ortalama kullanımı arasında büyük bir fark olması, uzun süreli düşük kullanımı ve kısa süreli yüksek kullanımı ifade eder. Bu kullanım modeli, veritabanları arasında kaynakların paylaşılması için idealdir. Bir veritabanının en yüksek kullanımı ortalama kullanımından 1,5 kat fazla olduğunda, veritabanı havuz için düşünülmelidir.

***Örnek***<br>
En yüksek kullanımı 100 DTU’ya varan ve ortalama olarak en fazla 67 DTU kullanan bir S3 veritabanı, bir havuzda eDTU paylaşmak için iyi bir adaydır.  Alternatif olarak, en yüksek kullanımı 20 DTU’ya varan ve ortalama olarak en fazla 13 DTU kullanan bir S1 veritabanı da havuz için iyi bir adaydır.

## <a name="sizing-an-elastic-pool"></a>Elastik havuzunu boyutlandırma
Bir havuz için en iyi boyut, havuzdaki tüm veritabanları için gereken toplam eDTU ve depolama kaynağı sayısına bağlıdır. Buna aşağıdakilerin belirlenmesi dahildir:

* Havuzdaki tüm veritabanları tarafından kullanılan en fazla DTU sayısı.
* Havuzdaki tüm veritabanları tarafından kullanılan en fazla depolama baytı sayısı.

Kullanılabilir boyutlar için bkz. [Elastik veritabanı havuzları ve elastik veritabanları için eDTU ve depolama limitleri](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

SQL Veritabanı, mevcut bir SQL Veritabanı sunucusundaki veritabanlarının geçmiş kaynak kullanımını otomatik olarak değerlendirir ve Azure portalda uygun havuz yapılandırmasını önerir. Önerilere ek olarak, yerleşik deneyim sunucu üzerindeki özel bir veritabanı grubu için eDTU kullanımını tahmin eder. Bu deneyim, havuza veritabanlarını etkileşimli bir şekilde ekleyerek ve değişiklikleri uygulamadan önce kaynak kullanım analizi ile boyutlandırma önerisini almak üzere veritabanlarını kaldırarak "durum" çözümlemesi yapmanıza olanak tanır. Nasıl yapılır konuları için bkz. [Elastik havuzlarını izleme, yönetme ve boyutlandırma](sql-database-elastic-pool-manage-portal.md).

V12’den önceki sunucular için geçici boyutlandırma tahminlerinin yanı sıra farklı sunuculardaki veritabanlarına yönelik boyutlandırma tahminleri sağlayan daha esnek kaynak kullanım değerlendirmeleri için bkz. [Elastik veritabanı havuzu için uygun veritabanlarını tanımlamaya yönelik Powershell betiği](sql-database-elastic-pool-database-assessment-powershell.md).

| Özellik | Portal deneyimi | PowerShell betiği |
|:--- |:--- |:--- |
| Ayrıntı düzeyi |15 saniye |15 saniye |
| Bir havuz ile tek veritabanı performans düzeyleri arasındaki fiyat farklılıklarını göz önünde bulundurur |Evet |Hayır |
| Analiz edilen veritabanları listesinin özelleştirilmesine olanak tanır |Evet |Evet |
| Analizde kullanılan sürenin özelleştirilmesine olanak tanır |Hayır |Evet |
| Farklı sunucular arasında analiz edilen veritabanları listesinin özelleştirilmesine olanak tanır |Hayır |Evet |
| v11 sunucularında analiz edilen veritabanları listesinin özelleştirilmesine olanak tanır |Hayır |Evet |

Araçları kullanamadığınız durumlarda aşağıdaki adım adım yönergeler bir havuzun tek veritabanlarından daha uygun maliyetli olup olmadığını tahmin etmenize yardımcı olabilir:

1. Havuz için gereken eDTU sayısını aşağıdaki gibi tahmin edebilirsiniz:
   
   MAKS(<*Toplam veritabanı sayısı* X *Veritabanı başına ortalama DTU kullanımı*>,<br>
   <*Eşzamanlı olarak en üst seviyeye çıkan veritabanı sayısı* X *Veritabanı başına en yüksek DTU kullanımı*)
2. Havuzdaki tüm veritabanları için gereken bayt sayısını ekleyerek havuz için gereken depolama alanını tahmin edin.  Ardından, bu depolama miktarını sağlayan eDTU havuz boyutunu belirleyin.  eDTU havuz boyutunu temel alan havuz depolama limitleri için bkz. [Elastik veritabanı havuzları ve elastik veritabanları için eDTU ve depolama limitleri](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).
3. 1. ve 2. Adımlardaki eDTU tahminlerinin büyük olanlarını alın.
4. [SQL Veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/) bakın ve 3. Adımdaki tahminden büyük olan en küçük eDTU havuz boyutunu bulun.
5. 5. Adımdaki havuz fiyatını, tek veritabanları için uygun performans düzeylerini kullanma fiyatıyla karşılaştırın.

## <a name="summary"></a>Özet
Bütün tek veritabanları havuzlar için uygun adaylar değildir. Düşük ortalama kullanım ve oldukça nadir kullanım yükseltmeleri ile nitelenen kullanım modellerine sahip veritabanları mükemmel adaylardır. Uygulama kullanım modelleri dinamik olduğu için, bir havuzun bazı veya tüm veritabanlarınıza yönelik iyi bir seçenek olup olmadığını görmek üzere ilk değerlendirmeyi yapmak için bu makalede açıklanan bilgi ve araçları kullanın. Bu makale, elastik bir havuzun uygun olup olmadığıyla ilgili karar vermenize yardımcı olmaya yönelik bir başlangıç noktasıdır. Geçmiş kaynak kullanımını sürekli olarak izlemeniz ve tüm veritabanlarınızın performans düzeylerini sürekli olarak yeniden değerlendirmeniz gerektiğini unutmayın. Veritabanlarını elastik havuzların içine ve dışına kolayca taşıyabilirsiniz ve çok sayıda veritabanınız varsa veritabanlarınızı çeşitli boyutlardaki birden fazla havuza bölebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Elastik veritabanı havuzu oluşturma](sql-database-elastic-pool-create-portal.md)
* [Elastik veritabanı havuzlarını izleme, yönetme ve boyutlandırma](sql-database-elastic-pool-manage-portal.md)
* [SQL Veritabanı seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama](sql-database-service-tiers.md)
* [Elastik veritabanı havuzu için uygun veritabanlarını tanımlamaya yönelik PowerShell betiği](sql-database-elastic-pool-database-assessment-powershell.md)




<!--HONumber=Dec16_HO1-->


