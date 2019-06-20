---
title: Azure SQL veritabanı hiper ölçekli SSS | Microsoft Docs
description: Genellikle hiper ölçekli bir veritabanı adı hiper ölçekli hizmet katmanındaki - bir Azure SQL veritabanı hakkında genel soruların müşterilere yanıtlar isteyin.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: 535ae91abc04b2fdcebb6a2083db95ec50f61798
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275593"
---
# <a name="faq-about-azure-sql-hyperscale-databases"></a>Azure SQL hiper ölçekli veritabanları hakkında SSS

Bu makalede, genellikle bir hiper ölçekli veritabanının adı Azure SQL veritabanı hiper ölçekli bir hizmet katmanındaki bir veritabanı dikkate müşteriler için sık sorulan soruların yanıtlarını sağlar. Bu makalede destekleyen hiper ölçekli senaryolar ve çapraz özelliği hizmetleri genel ile SQL veritabanı Hiperölçekli uyumludur.

- Bu SSS, Hiper ölçekli Hizmet katmanını kısa bir anlayışa sahip ve kendi özel sorular ve sorularınızın kaygıları arıyorsanız okuyucular için tasarlanmıştır.
- Bu SSS, bir kılavuz kitabı olması veya bir SQL veritabanı hiper ölçekli veritabanını kullanma hakkında soruları yanıtlamak üzere tasarlanmamıştır. Bunun için başvurmak için öneririz [Azure SQL veritabanı hiper ölçekli](sql-database-service-tier-hyperscale.md) belgeleri.

## <a name="general-questions"></a>Genel sorular

### <a name="what-is-a-hyperscale-database"></a>Bir hiper ölçekli veritabanı nedir

Bir Azure SQL veritabanı hiper ölçekli genişleme depolama teknolojisi tarafından desteklenen hiper ölçekli hizmet katmanındaki bir hiper ölçekli veritabanıdır. Bir hiper ölçekli veritabanı en fazla 100 TB veri destekler ve yüksek aktarım hızı ve performansı, ek olarak iş yükü gereksinimlerine uyum sağlamak için hızla ölçeklendirme sağlar. Ölçeklendirme, uygulama – bağlantısı, herhangi bir SQL veritabanı gibi sorgu işleme ve benzeri, iş için saydamdır.

### <a name="what-resource-types-and-purchasing-models-support-hyperscale"></a>Hangi kaynak türlerinin ve satın alma modeli, Hiper ölçekli desteği

Hiper ölçekli hizmet katmanı, yalnızca sanal çekirdek tabanlı satın alma modelini kullanarak Azure SQL veritabanı'nda tek veritabanları için kullanılabilir.  

### <a name="how-does-the-hyperscale-service-tier-differ-from-the-general-purpose-and-business-critical-service-tiers"></a>Hiper ölçekli Hizmet katmanını genel amaçlı ve iş açısından kritik hizmet katmanları farkı nedir

Kullanılabilirlik, depolama türü ve IOPS temel sanal çekirdek tabanlı hizmet katmanları, öncelikli olarak ayırt edilen temel.

- Genel amaçlı hizmet katmanı, dengeli bir g/ç gecikme veya yük devretme süreleri öncelik olmadığı işlem ve depolama seçenekleri sunduğundan çoğu iş yükü için uygundur.
- Hiper ölçekli Hizmet katmanını, çok büyük veritabanı iş yükleri için optimize edilmiştir.
- İş açısından kritik Hizmet katmanını GÇ gecikmesine öncelikli olduğu iş yükü için uygundur.

| | Kaynak türü | Genel Amaçlı |  Hiper Ölçek | İş Açısından Kritik |
|:---|:---:|:---:|:---:|:---:|:---:|
| **En iyi** |Tümü|  Çoğu iş yükü. Teklifler yönlendirilmiş dengeli işlem ve depolama seçenekleri bütçe. | Otomatik ölçeklendirme depolama ve ölçeklendirme ile büyük veri kapasitesi gereksinimlerini özelliği veri uygulamalarınızı sorunsuzca işlem. | Yüksek işlem oranına ve düşük gecikme süresi GÇ uygulamalarla OLTP. En yüksek düzeyde dayanıklılık sunar birkaç kullanarak hataları için yalıtılmış çoğaltmaları.|
|  **Kaynak türü** ||Tek veritabanı / elastik havuz / yönetilen örnek | Tek veritabanı | Tek veritabanı / elastik havuz / yönetilen örnek |
| **İşlem boyutu**|Tek veritabanı / elastik havuzu * | 1 ila 80 sanal çekirdekler | 1 ila 80 sanal çekirdekler * | 1 ila 80 sanal çekirdekler |
| |Yönetilen örnek | 8, 16, 24, 32, 40, 64, 80 sanal çekirdekler | Yok | 8, 16, 24, 32, 40, 64, 80 sanal çekirdekler |
| **Depolama türü** | Tümü |Premium uzaktaki depolama birimi (örnek başına) | Yerel SSD önbellek (örnek başına) ile serbest bağlı depolama | Süper hızlı yerel SSD depolaması (örnek başına) |
| **Depolama boyutu** | Tek veritabanı / elastik havuz | 5 GB – 4 TB | En fazla 100 TB | 5 GB – 4 TB |
| | Yönetilen örnek  | 32 GB – 8 TB | Yok | 32 GB – 4 TB |
| **G/ç aktarım hızı** | Tek veritabanı ** | 7000 maksimum IOPS ile sanal çekirdek başına 500 IOPS | Henüz bilinmiyor | 200.000 maksimum IOPS ile 5000 IOPS|
| | Yönetilen örnek | Dosya boyutuna bağlıdır | Yok | Yönetilen örnek: Dosya boyutuna bağlıdır|
|**Kullanılabilirlik**|Tümü|1 çoğaltma, hiçbir okuma ölçeği, Hayır yerel önbellek | Lrs'de kadar 15 okuma ölçeği, kısmi yerel önbellek | 3 çoğaltma, 1 okuma ölçeği, bölgesel olarak yedekli HA tam yerel önbellek |
|**Yedekleri**|Tümü|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)| RA-GRS, 7-35 gün (varsayılan olarak 7 gün), sabit zaman-belirli bir noktaya Kurtarma (PITR) | RA-GRS, 7-35 gün (varsayılan olarak 7 gün) |

\* Elastik havuzlar için hiper ölçekli hizmet katmanında desteklenmiyor

### <a name="who-should-use-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı kullanan

Hiper ölçekli Hizmet katmanını, öncelikle şirket içi SQL Server veritabanlarını ve buluta taşıyarak uygulamalarını modernleştirin isteyen müşterilere veya Azure SQL veritabanı zaten kullanıyorsunuz ve önemli ölçüde genişletmek isteyen müşteriler için tasarlanmıştır Veritabanı büyümesini olası. Hiper ölçekli, ayrıca hem yüksek performanslı ve yüksek ölçeklenebilirlik oluşturanlar müşterilere yöneliktir. Hiper ölçekli olursunuz:

- Veritabanı boyutu en fazla 100 TB için destek
- Veritabanı boyutu (yedeklemeler dosya anlık görüntülerine dayalı) bağımsız olarak hızlı veritabanı yedeklemeleri
- (Geri yükleme, dosya anlık görüntülerden olan) veritabanı boyutundan bağımsız olarak hızlı veritabanını geri yükler
- Hızlı hareket işleme sürelerini veritabanı boyutundan bağımsız olarak daha yüksek günlük işleme sonuçları
- Okuma ölçeği genişletme anında yedek yanı sıra, okuma, iş yükü boşaltma için bir veya daha fazla salt okunur düğümlerine.
- Bilgi işlem yoğun iş yükü uyum sağlayacak ve ardından, Sabit sürede ölçeği daha güçlü olmasını Sabit sürede hızlı ölçeği artırma. Örneğin, bu bir biçimde P6 olarak bir P11 arasında yukarı ve aşağı ölçeklendirme ile benzer ancak değil bir veri işlemi boyutu olarak çok daha hızlı gerçekleşir.

### <a name="what-regions-currently-support-hyperscale"></a>Hangi bölgeler şu anda hiper ölçekli destekler

Azure SQL veritabanı hiper ölçekli katmanı altında listelenen bölgede şu anda kullanılabilir [Azure SQL veritabanı hiper ölçekli genel bakış](sql-database-service-tier-hyperscale.md#regions).

### <a name="can-i-create-multiple-hyperscale-databases-per-logical-server"></a>Mantıksal sunucu başına birden fazla hiper ölçekli veritabanı oluşturabilirim

Evet. Daha fazla bilgi ve hiper ölçekli veritabanlarının her mantıksal sunucu sayısına yönelik sınırlar için bkz. [bir mantıksal sunucuda tek ve havuza alınmış veritabanları için SQL veritabanı kaynak limitleri](sql-database-resource-limits-logical-server.md).

### <a name="what-are-the-performance-characteristics-of-a-hyperscale-database"></a>Bir hiper ölçekli veritabanı performans özellikleri nelerdir

SQL veritabanı hiper ölçekli mimarisi, büyük veritabanı boyutları desteklerken yüksek performanslı ve aktarım hızı sağlar. 

### <a name="what-is-the-scalability-of-a-hyperscale-database"></a>Bir hiper ölçekli veritabanının ölçeklenebilirliği nedir

SQL veritabanı hiper ölçekli, iş yükü talebe göre hızlı ölçeklenebilirlik sağlar.

- **Yukarı/Aşağı ölçeklendirme**

  İle hiper ölçekli, CPU, bellek gibi kaynakların açısından birincil işlem boyutu ölçeğini ve ardından, Sabit sürede ölçeği. Depolama paylaşıldığından, büyütme ve ölçeği veri işleminin bir boyut değil.  
- **/ Out ölçeklendirme**

  İle hiper ölçekli, ayrıca, okuma isteklerinin sunmak için kullanabileceğiniz bir veya daha fazla ek işlem düğümleri sağlamasını yapma özelliği edinin. Bu, bu ek işlem düğümleri salt okunur düğüm olarak birincil işlemden iş yükünüz okuma boşaltmak için kullanabileceğiniz anlamına gelir. Ek olarak salt okunur, bu düğümler olarak etkin bekleme Ayrıca hizmet bir hata olması durumunda üzerinden birincil güçlendirin.

  Bu ek işlem düğümleri Sabit sürede yapılabilir ve çevrimiçi bir işlem olduğundan her biri sağlama. Ayarlayarak bu ek salt okunur işlem düğümlerine bağlanabilirsiniz `ApplicationIntent` bağlantı dizenizi bağımsız değişkenine `readonly`. Herhangi bir bağlantı ile işaretlenen `readonly` ek salt okunur işlem düğümlerinden biri için otomatik olarak yönlendirilir.

## <a name="deep-dive-questions"></a>Derin Dalış sorular

### <a name="can-i-mix-hyperscale-and-single-databases-in-a-single-logical-server"></a>Tek veritabanlarını tek bir mantıksal sunucu ve hiper ölçekli karıştırabilir miyim

Evet, uygulayabilirsiniz.

### <a name="does-hyperscale-require-my-application-programming-model-to-change"></a>Hiper ölçekli uygulama programlama modeli değiştirmek için benim gerektiriyor mu

Hayır, uygulama programlama modeli, olduğu gibi kalır. Azure SQL veritabanınızı ile etkileşim kurmak için bağlantı dizenizi normal ve diğer normal modu kullanın.

### <a name="what-transaction-isolation-levels-are-going-to-be-default-on-sql-database-hyperscale-database"></a>Hangi işlem yalıtım düzeyleri varsayılan veritabanında SQL veritabanı hiper ölçekli olacak

Birincil düğüm üzerinde işlem yalıtım düzeyi RCSI (okuma kaydedilen anlık görüntü yalıtımı) vardır. İkincil okuma ölçek düğümlerde, yalıtım düzeyini anlık görüntü.

### <a name="can-i-bring-my-on-premises-or-iaas-sql-server-license-to-sql-database-hyperscale"></a>SQL veritabanı hiper ölçekli için kendi şirket içi veya Iaas SQL Server lisansınızı getirebilirsiniz

Evet, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) hiper ölçekli için kullanılabilir. Her SQL Server Standard çekirdeği için 1 hiper ölçekli sanal çekirdekler eşleyebilirsiniz. Her SQL Server Enterprise çekirdeği için 4 hiper ölçekli sanal çekirdek eşleyebilirsiniz. İkincil çoğaltmalarda SQL lisansına sahip olması gerekmez. Azure hibrit avantajı Fiyat (ikincil) çoğaltmalara okuma ölçeği otomatik olarak uygulanır.

### <a name="what-kind-of-workloads-is-sql-database-hyperscale-designed-for"></a>SQL veritabanı hiper ölçekli hangi türdeki iş yüklerini tasarlanmıştır

Tüm SQL Server iş yüklerinizi SQL veritabanı hiper ölçekli destekler, ancak temelde OLTP için iyileştirilmiştir. Karma (HTAP) ve Analytical (veri reyonu) iş yükleri de getirebilirsiniz.

### <a name="how-can-i-choose-between-azure-sql-data-warehouse-and-sql-database-hyperscale"></a>SQL veritabanı hiper ölçekli Azure SQL veri ambarı arasında nasıl seçebilirim

Şu anda olarak veri ambarı, SQL Server'ı kullanarak etkileşimli analiz sorguları çalıştırma (örneğin, birkaç TB 10's TB'a kadar) görece küçük veri ambarları barındırabilir SQL veritabanı hiper ölçekli bir harika daha düşük bir maliyet karşılığında seçenektir; çünkü ve verilerinizi geçirebilirsiniz.  T-SQL kod değişikliği yapmadan ambarı iş yükü için SQL veritabanı hiper ölçekli.

Karmaşık sorgular ile büyük bir ölçekte veri analizi çalıştıran ve paralel veri ambarı'nı (PDW), Teradata veya diğer yüksek düzeyde paralel işlemci (MPP) kullanarak varsa) veri ambarları, SQL veri ambarı en iyi seçenek olabilir.
  
## <a name="sql-database-hyperscale-compute-questions"></a>SQL veritabanı hiper ölçekli işlem soruları

### <a name="can-i-pause-my-compute-at-any-time"></a>Ben my dilediğiniz zaman duraklatabilirsiniz

İşlem ve aşağı çoğaltmaların sayısı ölçeklendirebilirsiniz ancak şu anda değil, yoğun olmayan zamanlarda maliyetini düşürün.

### <a name="can-i-provision-a-compute-with-extra-ram-for-my-memory-intensive-workload"></a>My bellek kullanımı yoğun iş yükü için ek RAM ile bir işlem sağlayabilir miyim

Hayır. Daha fazla RAM almak için daha yüksek bir işlem boyutu için yükseltmeniz gerekir. Daha fazla bilgi için [hiper ölçekli depolama ve işlem boyutları](sql-database-vcore-resource-limits-single-databases.md#hyperscale-service-tier).

### <a name="can-i-provision-multiple-compute-nodes-of-different-sizes"></a>Farklı boyutlardaki birden fazla işlem düğümünde sağlayabilirim

Hayır.

### <a name="how-many-read-scale-replicas-are-supported"></a>Kaç tane okuma ölçeği yinelemeler desteklenir

Hiper ölçekli veritabanları varsayılan olarak, varsayılan olarak bir okuma ölçeği çoğaltması (toplam iki çoğaltmalar) ile oluşturulur. 0 ile 4 kullanarak arasında salt okunur çoğaltmaların sayısı ölçeklendirebilirsiniz [Azure portalında](https://portal.azure.com), [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current), [Powershell](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [CLI](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update).

### <a name="for-high-availability-do-i-need-to-provision-additional-compute-nodes"></a>Yüksek kullanılabilirlik için miyim sağlamak için ek işlem düğümleri

Hiper ölçekli veritabanlarında depolama düzeyinde dayanıklılık sağlanır. Bir çoğaltma dayanıklılık sağlamak için yeterlidir. İşlem çoğaltma kapalı olduğunda, yeni bir çoğaltma ile veri kaybı olmadan otomatik olarak oluşturulur.

Ancak, yalnızca bir çoğaltma varsa, yük devretme sonrasında yeni çoğaltma olarak yerel önbellek oluşturmak için biraz zaman alabilir. Önbellek yeniden oluşturma aşamasında veritabanı doğrudan sayfası sunucularından, IOPS ve sorgu performans veri getirir.

Yüksek kullanılabilirlik gerektiren iş açısından önemli uygulamaları için birincil bilgi işlem düğümü (varsayılan) dahil olmak üzere en az 2 işlem düğümleri, sağlamanız gerekir. Bu sayede, etkin bekleme yük devretme söz konusu olduğunda kullanılabilir.

## <a name="data-size-and-storage-questions"></a>Veri boyutu ile depolama soruları

### <a name="what-is-the-max-db-size-supported-with-sql-database-hyperscale"></a>SQL veritabanı Hiperölçekli ile desteklenen en büyük veritabanı boyutu nedir

100 TB

### <a name="what-is-the-size-of-the-transaction-log-with-hyperscale"></a>Büyük ölçekli işlem günlüğüyle boyutu nedir

Büyük ölçekli işlem günlüğüyle neredeyse sonsuzdur. Günlük alanı bir günlük yüksek aktarım hızına sahip bir sistemde bitmesi konusunda endişelenmeniz gerekmez. Ancak, günlük oluşturma hızını sürekli agresif iş yükleri için kısıtlanmış. En yüksek sürekli günlük oluşturulması yaklaşık 100 MB/sn oranıdır.

### <a name="does-my-temp-db-scale-as-my-database-grows"></a>My tempdb, Veritabanım büyüdükçe ölçeğinizi artırın

`tempdb` Veritabanı yerel SSD depolama alanında bulunan ve sağladığınız işlem boyutuna göre yapılandırılır. `tempdb` En iyi duruma getirilmiş ve maksimum performans avantajlarından sağlamak için normal bir kutudur. `tempdb` Boyutu yapılandırılabilir değildir ve sizin için depolama alt sistemi tarafından yönetilir.

### <a name="does-my-database-size-automatically-grow-or-do-i-have-to-manage-the-size-of-the-data-files"></a>Veritabanı boyut otomatik olarak Büyüt yok veya veri dosyalarının boyutunu yönetmek zorunda değilsiniz

INSERT/daha fazla veri alma gibi veritabanı boyutunuz otomatik olarak büyür.

### <a name="what-is-the-smallest-database-size-that-sql-database-hyperscale-supports-or-starts-with"></a>SQL veritabanı hiper ölçekli destekler veya ile başlayan küçük veritabanı boyutu nedir

10 GB

### <a name="in-what-increments-does-my-database-size-grow"></a>Hangi artışlarla my veritabanı boyutunu büyütün

1 GB

### <a name="is-the-storage-in-sql-database-hyperscale-local-or-remote"></a>Yerel veya uzak SQL veritabanı hiper ölçekli depolamada mı

Hiper ölçekli veri dosyalarını Azure standart depolama alanında depolanır. Veri, yoğun işlem düğümlerine yakın makinelerde yerel SSD depolama önbelleğe alınır. Ayrıca, işlem düğümleri, yerel SSD ve bellek içi (arabellek havuzu vb.), uzak düğümlerinden veri getirme sıklığını azaltmak için bir önbelleğe sahip.

### <a name="can-i-manage-or-define-files-or-filegroups-with-hyperscale"></a>Yönetmek veya miyim dosyaları veya hiper ölçekli içeren dosya grupları tanımlar

Hayır
  
### <a name="can-i-provision-a-hard-cap-on-the-data-growth-for-my-database"></a>Veritabanım için sabit bir sınır üzerinde veri artışı sağlayabilir miyim

Hayır

### <a name="how-are-data-files-laid-out-with-sql-database-hyperscale"></a>Nasıl veri dosyaları ile SQL veritabanı Hiperölçekli yerleştirilir

Veri dosyalarını sayfa sunucuları tarafından denetlenir. Veri boyutu büyüdükçe, veri dosyaları ve ilişkili sayfasında sunucu düğümlerini eklenir.

### <a name="is-database-shrink-supported"></a>Veritabanı küçültme destekleniyor mu

Hayır

### <a name="is-database-compression-supported"></a>Veritabanı sıkıştırması destekleniyor mu

Evet

### <a name="if-i-have-a-huge-table-does-my-table-data-get-spread-out-across-multiple-data-files"></a>Ben büyük bir tablonuz varsa, Tablo verilerimi arasında birden çok veri dosyası yayılmasını

Evet. Belirli bir tabloyla ilişkili veri sayfaları, aynı dosya grubunun tüm parçası olan birden çok veri dosyalarında sonlandırabilirsiniz. SQL Server kullanan bir [orantılı dolgu stratejisi](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups#file-and-filegroup-fill-strategy) verileri veri dosyalarını dağıtma.

## <a name="data-migration-questions"></a>Veri geçiş soruları

### <a name="can-i-move-my-existing-azure-sql-databases-to-the-hyperscale-service-tier"></a>Uygulamam var olan Azure SQL veritabanları için hiper ölçekli Hizmet katmanını taşıyabilir miyim

Evet. Mevcut Azure SQL veritabanlarınızı için hiper ölçekli taşıyabilirsiniz. Bu, tek yönlü bir geçiş olur. Başka bir hizmet katmanı için hiper ölçekli veritabanları taşıyamazsınız. Üretim veritabanlarınızı bir kopyasını alın ve (kanıtları) kavram kanıtı için hiper ölçekli için geçirme öneririz.
  
### <a name="can-i-move-my-hyperscale-databases-to-other-editions"></a>My hiper ölçekli veritabanları için diğer sürümleri taşıyabilir miyim

Hayır. Şu anda başka bir hizmet katmanına bir hiper ölçekli veritabanı taşıyamazsınız.

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanına geçişten sonra herhangi bir işlevselliğini veya özellikleri kaybetmeden miyim

Evet. Bazı Azure SQL veritabanı özellikleri dahil olmak üzere hiper ölçekli henüz desteklenmiyor ancak uzun süreli bekletme yedeklemesi sınırlı değildir. Bu özellikler için hiper ölçekli veritabanlarınızı geçirdikten sonra durabilir.  Bu sınırlamalar, geçici olmasını bekliyoruz.

### <a name="can-i-move-my--on-premises-sql-server-database-or-my-sql-server-virtual-machine-database-to-hyperscale"></a>Hiper ölçekli için şirket içi SQL Server veritabanını veya SQL Server sanal makine Veritabanım taşıyabilirim

Evet. BACPAC, işlem çoğaltma, mantıksal veri yükleme dahil olmak üzere hiper ölçekli için geçirmek için mevcut olan tüm geçiş teknolojileri kullanabilirsiniz. Ayrıca bkz: [Azure veritabanı geçiş hizmeti](../dms/dms-overview.md).

### <a name="what-is-my-downtime-during-migration-from-an-on-premises-or-virtual-machine-environment-to-hyperscale-and-how-can-i-minimize-it"></a>Bir şirket içi geçiş sırasında my kapalı kalma süresi nedir veya sanal makine hiper ölçekli ortama ve nasıl miyim en aza indirmek,

Azure SQL veritabanı'nda tek bir veritabanı için veritabanlarınızı geçiş sırasında kapalı kalma süresi kapalı kalma süresi ile aynıdır. Kullanabileceğiniz [işlemsel çoğaltma](replication-to-sql-database.md#data-migration-scenario
) boyutu birkaç TB'a kadar veritabanları için kapalı kalma süresiyle geçiş en aza indirmek için. Çok büyük bir veritabanı için (10'dan fazla TB), ADF, Spark veya diğer veri taşıma teknolojilerini kullanarak veri geçirmeyi düşünebilirsiniz.

### <a name="how-much-time-would-it-take-to-bring-in-x-amount-of-data-to-sql-database-hyperscale"></a>Ne kadar süre olduğu Al X getirmek için SQL veritabanı hiper ölçekli veri miktarı

Hiper ölçekli yeni/değiştirilmiş veri 100 MB/sn kullanan yeteneğine sahiptir.

### <a name="can-i-read-data-from-blob-storage-and-do-fast-load-like-polybase-and-sql-data-warehouse"></a>Verileri okumak blob depolama ve yükleme (örneğin, Polybase ve SQL veri ambarı) hızlı

Azure Depolama'dan veri okumak ve veri yükleme (yalnızca normal tek bir veritabanı ile yapabileceğiniz gibi) bir hiper ölçekli veritabanına yükleyin. Polybase, Azure SQL veritabanı üzerinde şu anda desteklenmiyor. Polybase kullanarak yapabileceğiniz [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) ya da bir Spark işi çalıştırma [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) ile [SQL için Spark Bağlayıcısı](sql-database-spark-connector.md). Spark Bağlayıcısı-SQL toplu ekleme destekler.

Basit kurtarma veya toplu günlük modeli hiper ölçekli desteklenmiyor. Tam kurtarma modeli, yüksek kullanılabilirlik sağlamak için gereklidir. Ancak, daha büyük ölçekli sağlar alabilen yeni günlük mimarisi nedeniyle tek bir Azure SQL veritabanına kıyasla oranı.

### <a name="does-sql-database-hyperscale-allow-provisioning-multiple-nodes-for-ingesting-large-amounts-of-data"></a>SQL veritabanı hiper ölçekli, büyük miktarlarda veri almak için birden çok düğüm sağlama izin veriyor mu

Hayır. SQL veritabanı hiper ölçekli bir SMP mimaridir ve asimetrik bir çoklu işlem ya da çok yöneticili mimari değildir. Yalnızca salt okunur iş yüklerinin ölçeğini genişletebilir için birden fazla çoğaltma oluşturabilirsiniz.

### <a name="what-is-the-oldest-sql-server-version-will-sql-database-hyperscale-support-migration-from"></a>Eski SQL Server sürümü, SQL veritabanı hiper ölçekli destek geçiş olur nedir

SQL Server 2005. Daha fazla bilgi için [tek bir veritabanı veya havuza alınmış bir veritabanı geçiş](sql-database-single-database-migrate.md#migrate-to-a-single-database-or-a-pooled-database). Uyumluluk sorunları için bkz: [veritabanı geçişi uyumluluk sorunlarını çözme](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues).

### <a name="does-sql-database-hyperscale-support-migration-from-other-data-sources-such-as-aurora-mysql-oracle-db2-and-other-database-platforms"></a>SQL veritabanı hiper ölçekli Aurora, MySQL, Oracle, DB2 ve diğer veritabanı platformlarına gibi diğer veri kaynaklarından geçiş destekliyor mu

Evet. SQL Server dışındaki farklı veri kaynaklarından gelen mantıksal geçiş gerektirir. Kullanabileceğiniz [Azure veritabanı geçiş hizmeti](../dms/dms-overview.md) mantıksal geçiş için.

## <a name="business-continuity-and-disaster-recovery-questions"></a>İş sürekliliği ve olağanüstü durum kurtarma sorular

### <a name="what-slas-are-provided-for-a-hyperscale-database"></a>Ne için hiper ölçekli bir veritabanı SLA sağlanır

Varsayılan 1 okunabilir ikincil birincil % 99,95 oranında kullanılabilirlik SLA'sı var.  Daha fazla çoğaltma ile en fazla % 99,99 oranında SLA gider.  

### <a name="are-the-database-backups-managed-for-me-by-the-azure-sql-database-service"></a>Veritabanı yedeklerini benim için Azure SQL veritabanı hizmeti tarafından yönetilir

Evet

### <a name="how-often-are-the-database-backups-taken"></a>Veritabanı yedeklerinin ne sıklıkta alınır

Hiçbir geleneksel tam, değişiklik ve günlük yedekleri SQL veritabanı hiper ölçekli veritabanları için vardır. Bunun yerine, normal bir anlık görüntü veri dosyalarının ve oluşturulan günlük saklama süresi, yapılandırılmış veya sizin için kullanılabilir olduğundan yalnızca korunur.

### <a name="does-sql-database-hyperscale-support-point-in-time-restore"></a>SQL veritabanı hiper ölçekli destek zaman içinde nokta geri yüklemesi

Evet

### <a name="what-is-the-recovery-point-objective-rporecovery-time-objective-rto-with-backuprestore-in-sql-database-hyperscale"></a>Kurtarma noktası hedefi (RPO) nedir / SQL veritabanı hiper ölçekli, yedekleme/geri yükleme ile kurtarma süresi hedefi (RTO)

RPO, 0 en düşük olur. RTO, 10 dakikadan az, veritabanı boyutundan bağımsız olarak hedeftir. 

### <a name="do-backups-of-large-databases-affect-compute-performance-on-my-primary"></a>Büyük veritabanlarının yedeklerini my birincil işlem performansı etkiler mi

Hayır. Yedeklemeler depolama alt sistemi tarafından yönetilir ve dosya anlık görüntüleri yararlanın. Kullanıcı iş yükünü birincil etkilemez.

### <a name="can-i-perform-geo-restore-with-a-sql-database-hyperscale-database"></a>SQL veritabanı hiper ölçekli bir veritabanı ile coğrafi geri yükleme yapabilir miyim

Evet.  Coğrafi geri yükleme tam olarak desteklenir.

### <a name="can-i-setup-geo-replication-with-sql-database-hyperscale-database"></a>Coğrafi çoğaltma ile SQL veritabanı hiper ölçekli veritabanı Kurulum

Şu anda değil.

### <a name="do-my-secondary-compute-nodes-get-geo-replicated-with-sql-database-hyperscale"></a>My ikincil işlem düğümleri, SQL veritabanı Hiperölçekli ile coğrafi olarak çoğaltılmış elde ederim

Şu anda değil.

### <a name="can-i-take-a-sql-database-hyperscale-database-backup-and-restore-it-to-my-on-premises-server-or-sql-server-in-vm"></a>SQL veritabanı hiper ölçekli bir veritabanı yedeğinin ve miyim kendi şirket içi sunucuda veya VM'de SQL Server için bir geri yükleme mi

Hayır. Hiper ölçekli veritabanları için depolama biçimi, geleneksel SQL Server'dan farklıdır ve yedeklemeleri denetlemek veya bunları erişiminiz yoksa. Bir SQL veritabanı hiper ölçekli veritabanı verilerinizden yararlanmak için dışarı aktarma hizmetini kullanmak veya BCP komut dosyası kullanın.

## <a name="cross-feature-questions"></a>Özellik sorular arası

### <a name="do-i-lose-any-functionality-or-capabilities-after-migration-to-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanına geçişten sonra herhangi bir işlevselliğini veya özellikleri kaybetmeden miyim

Evet. Azure SQL veritabanı özelliklerden bazıları dahil bunlarla sınırlı olmamak uzun süreli bekletme yedeklemesi hiper ölçekli içinde desteklenmez. Bu özellikler için hiper ölçekli veritabanlarınızı geçirdikten sonra durabilir.

### <a name="will-polybase-work-with-sql-database-hyperscale"></a>SQL veritabanı hiper ölçekli olacak Polybase çalışma

Hayır. Polybase, Azure SQL veritabanı'nda desteklenmiyor.

### <a name="does-the-compute-have-support-for-r-and-python"></a>İşlem, R ve python desteği var mı

Hayır. R ve Python Azure SQL veritabanı'nda desteklenmez.

### <a name="are-the-compute-nodes-containerized"></a>İşlem düğümlerine kapsayıcılı hale

Hayır. Bir işlem sanal makine ve kapsayıcı olmayan veritabanı bulunur.

## <a name="performance-questions"></a>Performans sorular

### <a name="how-much-throughput-can-i-push-on-the-largest-sql-database-hyperscale-compute"></a>Ne kadar verimlilik ı üzerinde en büyük SQL veritabanı hiper ölçekli işlem gönderebilir

Değişiklik verilerini (işlem günlüğü verileri oluşturma), tutarlı bir 100 MB/sn gördük

### <a name="how-many-iops-do-i-get-on-the-largest-sql-database-hyperscale-compute"></a>Kaç tane IOPS üzerinde en büyük SQL veritabanı hiper ölçekli işlem alabilir miyim

IOPS ve g/ç gecikme süresi, iş yükü düzenleri bağlı olarak değişir.  Erişilecek gerek kalmadan verileri compute'nın önbellek için yerel ise, yerel SSD olarak aynı GÇ desenleri olacaktır.   

### <a name="does-my-throughput-get-affected-by-backups"></a>My aktarım hızı yedeklemeler tarafından etkilenen

Hayır. İşlem, depolama katmanından işlem üzerindeki etkiyi önlemek için ayrılmıştır.

### <a name="does-my-throughput-get-affected-as-i-provision-additional-compute-nodes"></a>Ek işlem düğümleri sağlayabilirim gibi my aktarım hızı etkilenir

Paylaşılan depolama birimi ve birincil ve ikincil işlem düğümleri arasında'olmuyor doğrudan fiziksel çoğaltma olduğundan teknik olarak, aktarım hızı birincil düğüm üzerinde okuma ölçeği düğüm ekleyerek etkilenmez. Ancak biz günlük izin vermek için sürekli agresif iş yükünü azaltma ikincil düğüm ve bilgi edinmek için sayfa sunucularda uygulayın ve ikincil düğümlerinde hatalı okuma performansı kaçının.

## <a name="scalability-questions"></a>Ölçeklenebilirlik soruları

### <a name="how-long-would-it-take-to-scale-up-and-down-a-compute-node"></a>İşlem düğümü yukarı ve aşağı ölçeklendirme süresini

İşlem ölçeklendirme veya aşağı veri boyutundan bağımsız olarak 5-10 dakika sürer.

### <a name="is-my-database-offline-while-the-scaling-updown-operation-is-in-progress"></a>Yukarı/Aşağı işlemi ölçeklendirme işlemi devam ederken Veritabanım çevrimdışı olur.

Hayır. Ölçeklendirme yukarı ve aşağı bu kadar çevrimiçi olacak.

### <a name="should-i-expect-connection-drop-when-the-scaling-operations-are-in-progress"></a>Ölçeklendirme işlemleri devam ederken bağlantı bırakma beklemeliyim

Artırma veya işlem düğümü ile hedef boyutu için yük devretme gerçekleştiğinde bırakılmakta varolan bağlantılar sonuçlarında azaltma. Salt okunur çoğaltmalar ekleyerek bağlantı bırakmaları sonuçlanmaz.

### <a name="is-the-scaling-up-and-down-of-compute-nodes-automatic-or-end-user-triggered-operation"></a>Ölçeklendirme ve aşağı son kullanıcı veya işlem düğümlerini otomatik tetiklenen işlemi

Son kullanıcı. Otomatik değil.  

### <a name="does-my-tempb-also-grow-as-the-compute-is-scaled-up"></a>Mu my `tempb` Ayrıca işlem ölçeği kaydedildikçe.

Evet. İşlem büyüdükçe geçici db otomatik olarak ölçeklenir.  

### <a name="can-i-provision-multiple-primary-computes-such-as-a-multi-master-system-where-multiple-primary-compute-heads-can-drive-a-higher-level-of-concurrency"></a>Eşzamanlılık daha yüksek bir düzeyde birden fazla birincil işlem heads burada yönlendirebilirsiniz çok yöneticili bir sistem gibi birden çok birincil hesaplar sağlayabilirim

Hayır. Yalnızca birincil işlem düğümü okuma/yazma isteklerini kabul eder. İkincil işlem düğümleri yalnızca salt okunur istekler kabul edin.

## <a name="read-scale-questions"></a>Okuma ölçeği sorular

### <a name="how-many-secondary-compute-nodes-can-i-provision"></a>Kaç tane ikincil işlem düğümleri sağlayabilirim

Varsayılan olarak hiper ölçekli veritabanları için 2 çoğaltması oluştururuz. Yineleme sayısı ayarlamak istiyorsanız, bunu kullanarak yapabilirsiniz [Azure portalında](https://portal.azure.com).

### <a name="how-do-i-connect-to-these-secondary-compute-nodes"></a>Bu ikincil işlem düğümlerine nasıl bağlanabilirim

Ayarlayarak bu ek salt okunur işlem düğümlerine bağlanabilirsiniz `ApplicationIntent` bağlantı dizenizi bağımsız değişkenine `readonly`. Herhangi bir bağlantı ile işaretlenen `readonly` ek salt okunur işlem düğümlerinden biri için otomatik olarak yönlendirilir.  

### <a name="can-i-create-a-dedicated-endpoint-for-the-read-scale-replica"></a>Okuma ölçeği çoğaltma için ayrılmış bir uç noktası oluşturabilir miyim

Hayır. Belirterek yalnızca okuma ölçeği çoğaltmaya bağlanabilirsiniz `ApplicationIntent=ReadOnly`.

### <a name="does-the-system-do-intelligent-load-balancing-of-the-read-workload"></a>Sistem, okuma iş yükü akıllı yük dengeleme işe yarar

Hayır. Okuma yalnızca bir rastgele okuma ölçeği çoğaltmaya yeniden yönlendirilmiş iş yüküdür.

### <a name="can-i-scale-updown-the-secondary-compute-nodes-independently-of-the-primary-compute"></a>Ben yukarı/aşağı ikincil işlem düğümlerine birincil işlem bağımsız olarak ölçeklendirebilirsiniz

Hayır. Bunların aynı yapılandırmanın bir yük devretme durumunda, birincil olarak olması gerekir böylece ikincil işlem düğümlerine HA için de kullanılır.

### <a name="do-i-get-different-temp-db-sizing-for-my-primary-compute-and-my-additional-secondary-compute-nodes"></a>Farklı tempdb boyutlandırma my birincil işlem ve benim ek ikincil işlem düğümleri için elde ederim

Hayır. `tempdb` Yapılandırılmış işlem sağlama boyutuna bağlı olarak, ikincil işlem düğümleriniz birincil işlem aynı boyutta olan.

### <a name="can-i-add-indexes-and-views-on-my-secondary-compute-nodes"></a>Dizinler ve görünümleri my ikincil işlem düğümleri ekleyebilir miyim

Hayır. Hiper ölçekli veritabanları paylaşılan depolama, tüm işlem düğümleri aynı tablolar, dizinler ve görünümler görmek anlamına gelir. İkincil – okuma için iyileştirilmiş ek dizinler istiyorsanız, bunları birincil ilk eklemeniz gerekir.

### <a name="how-much-delay-is-there-going-to-be-between-the-primary-and-secondary-compute-node"></a>Birincil ve ikincil işlem düğümü arasında olacak şekilde ne kadar gecikme oraya giden

Bir işlem günlüğü oluşturma hızını bağlı olarak birincil kararlıdır andan itibaren ya da anlık veya düşük milisaniye cinsinden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Hiper ölçekli hizmet katmanı hakkında daha fazla bilgi için bkz: [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md).
