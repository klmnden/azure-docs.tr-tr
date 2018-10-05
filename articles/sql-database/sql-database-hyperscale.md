---
title: Azure SQL veritabanı hiper ölçekli genel bakış | Microsoft Docs
description: Bu konuda Azure SQL veritabanı'ndaki sanal çekirdek tabanlı satın alma modeli hiper ölçekli hizmet katmanında ve genel amaçlı ve iş açısından kritik hizmet katmanı arasından farklı nasıl açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/23/2018
ms.openlocfilehash: be7c0b80b8f735a5eefe4e8345cd4d4a16885ec4
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803571"
---
# <a name="what-is-the-hyperscale-service-tier-preview-in-azure-sql-database"></a>Azure SQL veritabanı'nda hiper ölçekli hizmet Katmanı (Önizleme) nedir?

Azure SQL veritabanı'nda hiper ölçekli hizmet katmanı sanal çekirdek tabanlı satın alma modeli en yeni hizmet katmanında ' dir. Bu hizmet katmanında yüksek düzeyde ölçeklenebilir depolama ve Azure mimarisini depolama ve önemli ölçüde kullanılabilir sınırları aşan bir Azure SQL veritabanı için genel amaçlı ve iş için bilgi işlem kaynaklarını yararlanır işlem performans katmanı olan Kritik hizmet katmanları.

> [!IMPORTANT]
> Hiper ölçekli hizmet katmanı genel önizlemeye sunuldu ve şu anda sınırlı Azure bölgelerinde kullanılabilir. Bölge tam listesi için bkz [hiper ölçekli hizmet katmanı kullanılabildiği bölgeler](#hyperscale-regions)

> [!IMPORTANT]
> Hiper ölçekli hizmet katmanı şu anda genel Önizleme aşamasındadır. Tüm üretim iş yüklerini hiper ölçekli veritabanlarında henüz çalıştıran önerilmemektedir. Diğer hizmet katmanları için bir hiper ölçekli veritabanı güncelleştirilemiyor. Test amacıyla geçerli veritabanınızın bir kopyasını alın ve kopyasını hiper ölçekli hizmet katmanına güncelleştirmek öneririz.

## <a name="what-are-the-capabilities-of-the-hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanının özellikleri nelerdir

Azure SQL veritabanı'nda hiper ölçekli Hizmet katmanını, aşağıdaki özellikleri sağlar:

- Bir veritabanı boyutu 100 TB kadar desteği 
- Daha hızlı büyük veritabanı yedekleme (dosya anlık görüntülerine dayalı)
- Daha hızlı geri yüklemeler (dosya anlık görüntülerine dayalı) veritabanı 
- Daha fazla günlük performans ve veri birimlerini bağımsız olarak daha hızlı hareket işleme süreleri nedeniyle yüksek genel performansı 
- Hızlı ölçeği genişletme - bir veya daha fazla salt okunur düğüm, okuma iş yükü boşaltma için ve kullanımına yönelik sık erişimli-yedek sağlayabilirsiniz
- Hızlı ölçeği artırma -, Sabit sürede işlem kaynaklarınızı gerektiğinde gerekli yoğun iş yüklerine uyum sağlamak üzere ölçeklendirmek ve ardından işlem kaynaklarını değil gerektiğinde ölçeği tekrar. 

Hiper ölçekli hizmet katmanı, birçok geleneksel bulut veritabanlarında görülen pratik sınırlarına kaldırır. Tek bir düğüm kullanılabilir kaynaklar tarafından en diğer veritabanları burada sınırlıdır hiper ölçekli hizmet katmanındaki veritabanları gibi bir sınırları vardır. Esnek depolama mimarisinin ile gerektiği gibi depolama kapasitesini öğrenmek ölçeklendirebilirsiniz. Aslında, Hiper ölçekli veritabanları ile tanımlanan en fazla boyutu oluşturulmaz. Bir hiper ölçekli veritabanı gerektiğinde - büyüdükçe ve yalnızca kullandığınız kapasitesi için faturalandırılırsınız. Okuma açısından yoğun iş yükleri için hiper ölçekli Hizmet katmanını, yük boşaltma okuma iş yükleri için gerektiği gibi ek salt okunur çoğaltmalar sağlayarak hızlı ölçeklendirme sağlar. 

Ayrıca, zaman ölçeği büyütün veya veritabanı yedeklemelerini oluşturmak için gerekli veya aşağı artık veritabanında veri hacmine bağlıdır. Hiper ölçekli veritabanları neredeyse anında yedeklenebilir. Ayrıca bir veritabanında terabayt onlarca yukarı veya aşağı dakikalar içinde ölçeklendirebilirsiniz. Bu özellik ilk yapılandırma seçimlerinizi Kutu ilgili endişeleriniz öğesinden kurtarır. 

Hiper ölçekli hizmet katmanı için işlem boyutları hakkında daha fazla bilgi için bkz. [sanal çekirdek hizmet katmanı, işlem, bellek, depolama ve GÇ kaynakları seçme](sql-database-service-tiers-vcore.md).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Kimin hiper ölçekli Hizmet katmanını göz önünde bulundurmalıyım

Hizmet katmanı, öncelikle büyük veritabanları olan müşteriler için tasarlanmıştır hiper ölçekli ya da şirket içi ve buluta veya bulutta durumda ve tarafından en büyük veritabanı boyutu sınırlı olduğundan müşteriler için taşıyarak uygulamalarını modernleştirin istiyorsanız kısıtlamalar (1-4 TB). Ayrıca, yüksek performanslı ve yüksek ölçeklenebilirlik için depolama arama ve işlem müşteriler için de tasarlanmıştır.

Tüm SQL Server iş yüklerini hiper ölçekli Hizmet katmanını destekler, ancak temelde OLTP için iyileştirilmiştir. Hiper ölçekli hizmet katmanı karma de destekler ve analitik (veri reyonu) iş yükleri. 

> [!IMPORTANT]
> Elastik havuzlar, Hiper ölçekli Hizmet katmanını desteklemez.

## <a name="architecture-distributing-functions-to-isolate-capabilities"></a>Mimari: özellikleri yalıtmak için işlevleri dağıtma

Tüm veri yönetim işlevlerinin bir işlemde konum / (hatta bugün üretimde çağrılan dağıtılmış veritabanları tek parçalı veri altyapısı birden çok kopyasını böylece) merkezi geleneksel veritabanı altyapıları, bir hiper ölçekli veritabanı ayırır. Burada çeşitli veri altyapıları semantiği, uzun vadeli depolama ve dayanıklılık için veri sağlayan bileşenlerden ayırmak sorgu işleme altyapısı. Bu şekilde, depolama kapasitesini sorunsuz gerektiği kadar genişletilebilir (ilk hedef değer 100 TB). Yeni bir okunabilir çoğaltma dönmesi için hiçbir veri kopyalama gerekli şekilde salt okunur çoğaltmalar aynı işlem bileşenleri paylaşın.

Aşağıdaki diyagram, farklı türde bir hiper ölçekli veritabanında düğümleri gösterir:

![architecture](./media/sql-database-hyperscale/hyperscale-architecture.png)

Bir hiper ölçekli veritabanı aşağıdaki farklı tür düğüm içerir:
 
### <a name="compute-node"></a>İşlem düğümü

İşlem düğümü nerede ilişkisel altyapıda yaşıyor, tüm dil öğeleri, sorgu işleme ve benzeri ortaya olduğundan. Sorun bu pencerelerde hiper ölçekli veritabanına sahip tüm kullanıcı etkileşimlerine işlem düğümleri. Düğüm ağ sayısını en aza indirmek için SSD tabanlı önbellekler (etiketli RBPEX - önceki diyagramdaki dayanıklı arabellek havuzu genişletme) sahip işlem gidiş dönüş bir sayfa veri getirmek için gerekli. Tüm okuma-yazma iş yükleri ve işlemleri işleneceği bir birincil işlem düğümü vardır. (Bu işlevselliği isteniyorsa) iş yükleri okuma boşaltma için salt okunur işlem düğümleri olarak davranmak yanı sıra yük devretme amacıyla sık erişimli bekleme düğümleri olarak davranacak bir veya daha fazla ikincil işlem düğümleri vardır.

### <a name="page-server-node"></a>Sunucu düğümü sayfası

Sayfa sunucuları genişletilmiş depolama altyapısı temsil eden sistemleridir.  Her bir sayfası sunucusu, veritabanındaki sayfaları kümesini sorumludur.  Olup, her sayfa sunucusu 1 terabayt veri denetler. Veri yok (dışında yedeklilik ve kullanılabilirlik tutulur çoğaltmalar) birden fazla sayfa sunucudaki paylaşılır. Bir sayfa sunucusunun isteğe bağlı işlem düğümlerine veritabanlarının sunmak ve veri hareketlerini güncelleştir güncel sayfaları tutmak işidir. Günlük hizmeti günlük kayıtları yürüterek sayfası sunucular güncel tutulur. Sayfa sunucuları da performansı geliştirmek için SSD tabanlı önbellekler bulundurur. Uzun vadeli depolaması ve veri sayfalarını ek güvenilirlik için Azure depolama alanında tutulur.

### <a name="log-service-node"></a>Günlük hizmeti düğümü

Günlük hizmeti düğümü birincil işlem düğümü günlük kayıtları kabul eder, bunları dayanıklı bir önbellekte kalıcı ve (, önbelleklerini güncelleştirebileceğiniz) ve böylece veriler, güncelleştirilmiş t olabilir günlük kayıtlarını geri kalanı için işlem düğümlerine ilgili sayfaya sunucuları yanı sıra iletir Burada. Bu şekilde, tüm veri değişikliklerini birincil işlem düğümünden tüm sayfa sunucuları ve ikincil işlem düğümleri için günlük hizmeti aracılığıyla dağıtılır. Son olarak, günlük kayıtları sonsuz depolama havuzu bir Azure depolama, uzun vadeli depolama için atılır. Bu mekanizma, sık sık günlük kesme için yeterlilikte kaldırır. Günlük hizmeti erişimi hızlandırmak üzere yerel önbelleği de vardır.

### <a name="azure-storage-node"></a>Azure Depolama düğümü

Azure depolama, veri sayfası sunucularından son hedefi düğümüdür. Bu depolama, Azure bölgeleri arasında çoğaltmayı hem de yedekleme amacıyla kullanılır. Yedekleme anlık görüntüsünü veri dosyalarının oluşur. Geri yükleme işlemi bu anlık görüntülerden hızlı olan ve veri geri zaman içinde herhangi bir noktasına yükleyebilirsiniz. 

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Temel dosya anlık görüntüsü yedekleri olan ve bu nedenle neredeyse anında. Depolama ve işlem ayrımı yedekleme/geri yükleme işlemi, birincil bir işlem düğümünde işleme yükünü azaltmak depolama katmanına gönderme etkinleştirin. Sonuç olarak, büyük bir veritabanının yedeğini birincil işlem düğümü performansını etkilemez. Benzer şekilde, geri yüklemeler dosya anlık görüntüsü kopyalayarak yapılır ve bu nedenle bir veri işlemi boyutunu değildir. Aynı depolama hesabında geri yüklemeler için geri yükleme işlemi hızlıdır.

## <a name="scale-and-performance-advantages"></a>Ölçek ve performans avantajları

Hızlı bir şekilde ek salt okunur işlem düğümleri Yukarı/Aşağı Döndür olanağı, mimari önemli sağlayan hiper ölçekli ölçeklendirme özelliklerinden okuma ve ayrıca daha fazla yazma isteklerine hizmet için birincil işlem düğümü boş. Ayrıca, işlem düğümlerine yukarı/aşağı hızla hiper ölçekli mimarisi paylaşılan depolama mimarisi nedeniyle ölçeklendirilebilir. 

### <a name="a-namehyperscale-regions-hyperscale-service-tier-available-regions"></a><a name="hyperscale-regions"> Hiper ölçekli hizmet katmanı kullanılabildiği bölgeler
Hiper ölçekli hizmet katmanı şu anda genel Önizleme aşamasındadır ve aşağıdaki Azure bölgeleri içinde kullanılabilir: EastUS1 EastUS2, WestUS2, CentralUS, NorthCentralUS, WestEurope, NorthEurope, UKWest, AustraliaEast, AustraliaSouthEast, SouthEastAsia, JapanEast, KoreaCentral

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet katmanları hakkında daha fazla bilgi için bkz: [hizmet katmanları](sql-database-service-tiers.md)
- Bkz: [kaynak bakış sınırlayan bir mantıksal sunucuda](sql-database-resource-limits-logical-server.md) sunucusu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
- Model sınırları tek bir veritabanı için satın almak için bkz: [Azure SQL veritabanı sanal çekirdek tabanlı satın alma modeli sınırları tek bir veritabanı için](sql-database-vcore-resource-limits-single-databases.md).
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
