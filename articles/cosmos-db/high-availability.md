---
title: Azure Cosmos DB'de yüksek kullanılabilirlik
description: Bu makalede Azure Cosmos DB yüksek kullanılabilirliği nasıl sağladığını açıklar.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 74e2d7901d127c9dd7edd8509e5bba082c4ad220
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978965"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Azure Cosmos DB ile yüksek kullanılabilirlik

Azure Cosmos DB, verilerinizi Cosmos hesabınızla ilişkili tüm Azure bölgeleri arasında şeffaf biçimde çoğaltır. Cosmos DB, aşağıdaki görüntüde gösterildiği gibi birden çok yedekleme verileriniz için katmanı kullanır:

![Fiziksel bölümleme](./media/high-availability/cosmosdb-data-redundancy.png)

- Cosmos kapsayıcıları içinde veri [yatay olarak bölünmüş](partitioning-overview.md).

- Her bölge içinde çoğaltılır ve çoğaltmaları çoğunluğu tarafından dizinlendiğini tüm yazma işlemlerini ile her bölüm, çoğaltma kümesi tarafından korunur. Çoğaltmaları 10 20 adede kadar hata etki alanlarına dağıtılır.

- Her bölüm tüm bölgeler arasında çoğaltılır. Her bölgede bir Cosmos kapsayıcının tüm veri bölümleri içerir ve yazma kabul edebilir ve okuma hizmet.  

Cosmos hesabınızı dağıtılır, *N* Azure bölgeleri olacaktır en az *N* 4 tüm verilerinizin kopyalarını x. Düşük gecikme süreli veri erişimi sağlayan ve Cosmos hesabınızla ilişkili bölgeler arasında yazma/okuma aktarım hızı ölçeklendirmenin yanı sıra, daha fazla bölge sahip (yüksek *N*) kullanılabilirliğini daha da geliştirir.  

## <a name="slas-for-availability"></a>Kullanılabilirlik SLA'ları

Global olarak dağıtılmış bir veritabanı olarak Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayabilir ve kapsamlı SLA sağlar. Aşağıdaki tabloda, tek ve çok bölgeli hesaplar için Cosmos DB tarafından sağlanan yüksek kullanılabilirlik garantisi gösterir. Yüksek kullanılabilirlik için birden fazla yazma bölgeleri için Cosmos hesaplarınızı her zaman yapılandırın.

|İşlem türü  | Tek bölge |Çok bölgeli (tek bölge yazar)|Çok bölgeli (çok bölgeli yazar) |
|---------|---------|---------|-------|
|Yazar    | 99.99    |99.99   |99.999|
|Okur     | 99.99    |99.999  |99.999|

> [!NOTE]
> Uygulamada, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modelleri için gerçek yazma kullanılabilirliği yayımlanan Sla'lardan önemli ölçüde büyük/küçük harf yüksektir. Gerçek okuma kullanılabilirliği için tüm tutarlılık düzeyi yayımlanan SLA'lar yüksektir.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Bölgesel kesintiler yaşanması durumunda Cosmos DB ile yüksek kullanılabilirlik

Bölgesel kesintiler nadir değildir ve Azure Cosmos DB her zaman veritabanınızı yüksek oranda kullanılabilir olmasını sağlar. Aşağıdaki ayrıntıları Cosmos hesabı yapılandırmanıza bağlı olarak bir kesinti sırasında Cosmos DB davranışı Yakala:

- Bir yazma işlemi, istemci için Onaylandı önce Cosmos DB ile veri yazma işlemleri kabul eden bir bölgedeki bir çekirdeği tarafından dizinlendiğini.

- Çok bölgeli hesaplar birden çok yazma bölgeleri ile yapılandırılmış, yazma ve okuma için yüksek oranda kullanılabilir olacaktır. Bölgesel yük devretme anlıktır ve değişiklikleri uygulamadan gerekmez.

- **Çok bölgeli hesaplar tek yazma bölgesi (yazma bölgesi kesinti) ile:** Yazma bölgesi kesinti sırasında okuma için bu hesapları yüksek oranda kullanılabilir olarak kalır. Ancak, yazma için şunları yapmalısınız **"otomatik yük devretmeyi etkinleştir"** , Cosmos üzerinde başka bir bölgeye etkilenen bölgeye yük devretme için hesap. Yük devretme, belirttiğiniz bölge öncelik sırasına göre gerçekleştirilir. Etkilenen bölge yeniden çevrimiçi olduğunda, kesinti sırasında etkilenen yazma bölgesinde mevcut çoğaltılmamış veriler aracılığıyla kullanılabilir hale getirileceğini [akışı çakışmaları](how-to-manage-conflicts.md#read-from-conflict-feed). Uygulama, çakışmaları akışı, uygulamaya özgü mantığı temelinde çakışmaları çözün ve güncelleştirilmiş veriler uygun şekilde Cosmos kapsayıcıya geri yazma okuyabilir. Daha önce etkilenen yazma bölgesi kurtarır sonra otomatik olarak bir okuma bölgesi kullanılabilir hale gelir. Elle yük devretme çağırmak ve etkilenen bölgeyi yazma bölgesi yapılandırın. Kullanarak el ile bir yük devretme yeniden yapabilirsiniz [Azure CLI veya Azure portalında](how-to-manage-database-account.md#manual-failover). Var. **veri veya kullanılabilirliği kaybı olmadan** öncesinde, sırasında veya sonrasında el ile yük devretme. Uygulamanız, yüksek oranda kullanılabilir olmaya devam eder. 

- **Çok bölgeli hesaplar tek yazma bölgesi (okuma bölgesi kesinti) ile:** Okuma bölgesi kesinti sırasında bu hesapları okuma ve yazmalar için yüksek oranda kullanılabilir kalır. Etkilenen bölgeyi yazma bölgesi otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. [Cosmos DB SDK'ları](sql-api-sdk-dotnet.md) yeniden yönlendirme tercih edilen bölge listesindeki bir sonraki kullanılabilir bölgeye çağrıları okur. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda okuma bölgesi kesinti işlemek için gerekli değildir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi ile geçerli yazma bölgesine otomatik olarak eşitler ve yeniden okuma isteklerine hizmet vermeye kullanıma sunulacaktır. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir. Hem yük devretme ve daha önce başarısız bölgesi aşamalarını sırasında tutarlılık garantileri Cosmos DB tarafından kabul devam okuyun.

- Tek bölgeli hesaplar, bölgesel bir kesintinin ardından kullanılabilirlik kaybedebilir. Her zaman ayarlanan önerilir **en az iki bölgeleri** (tercihen en az iki bölgeleri yazma) Cosmos hesabınızla her zaman yüksek kullanılabilirlik sağlamak için.

- Bile oldukça nadir ve talihsiz olayda Azure bölgesi kalıcı olarak kurtarılamaz olduğunda olup olmadığını veri kaybı olmadan, çok bölgeli Cosmos hesabınızın varsayılan tutarlılık düzeyi ile yapılandırılır *güçlü*. Sınırlanmış eskime durumu tutarlılık ile yapılandırılmış çok bölgeli Cosmos hesaplar için bir kalıcı olarak kurtarılamaz yazma bölgesi olması durumunda olası veri kaybı penceresini eskime penceresine sınırlıdır (*K* veya *T*); oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri için olası veri kaybı penceresini en fazla beş saniye sınırlıdır.

## <a name="building-highly-available-applications"></a>Yüksek düzeyde erişilebilir uygulamalar oluşturma

- Yüksek yazma emin olun ve Okunabilirlik için birden çok yazma bölgeleri ile en az iki bölgeleri yayılmasını Cosmos hesabınızı yapılandırın. Bu yapılandırma sağlayacak en düşük gecikme, yüksek kullanılabilirlik ve her ikisi için de en iyi ölçeklenebilirlik okur ve SLA'lar ile desteklenen yazar. Daha fazla bilgi için bkz. nasıl [Cosmos hesabınız ile birden çok yazma bölgeleri yapılandırma](tutorial-global-distribution-sql-api.md).

- Bir yazma tek bölge ile yapılandırılmış olan çok bölgeli Cosmos hesapları için [otomatik yük devretme, Azure CLI veya Azure portalını kullanarak etkinleştirmeniz](how-to-manage-database-account.md#automatic-failover). Bölgesel bir olağanüstü durumda olduğunda otomatik yük devretme etkinleştirdikten sonra Cosmos DB devreder otomatik olarak hesabınızı.  

- Cosmos hesabınızı yüksek oranda kullanılabilir olsa bile, uygulamanızın doğru bir şekilde yüksek oranda kullanılabilir kalmasını tasarlanmamış olabilir. Uygulamanızın uçtan uca yüksek kullanılabilirliğini sınamak için düzenli aralıklarla çağırma [Azure CLI veya Azure portalını kullanarak el ile yük devretme](how-to-manage-database-account.md#manual-failover), uygulamayı test etmek veya olağanüstü durum kurtarma (DR) bir parçası olarak gidilmesini sağlar.

- Bir Global olarak dağıtılmış veritabanı ortam içinde bir bölge çapında kesinti varsa tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki yoktur. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır önce kabul edilebilen maksimum süre anlamanız gerekir. Bir uygulamanın tamamen kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) bilinir. Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin maksimum süreyi anlamanız gereken bir kesintiden sonra kurtarılırken. Zaman dilimi kaybetmeyi göze güncelleştirmeleri, kurtarma noktası hedefi (RPO) bilinir. Azure Cosmos DB için RTO ve RPO için bkz [tutarlılık düzeyleri ve veri dayanıklılığı](consistency-levels-tradeoffs.md#rto)

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makaleleri okuyabilirsiniz:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Genel olarak sağlanan aktarım hızı ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)
* [Cosmos hesabınızın birden çok yazma bölgeleri ile yapılandırma](how-to-multi-master.md)
