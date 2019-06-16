---
title: Azure Cosmos DB'de yüksek kullanılabilirlik
description: Bu makalede Azure Cosmos DB yüksek kullanılabilirliği nasıl sağladığını açıklar.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 23273084826775b47170753dff3e5cf5ed8ae45f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063570"
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
|Yazma    | 99.99    |99.99   |99.999|
|Okuma     | 99.99    |99.999  |99.999|

> [!NOTE]
> Uygulamada, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modelleri için gerçek yazma kullanılabilirliği yayımlanan Sla'lardan önemli ölçüde büyük/küçük harf yüksektir. Gerçek okuma kullanılabilirliği için tüm tutarlılık düzeyi yayımlanan SLA'lar yüksektir.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Bölgesel kesintiler yaşanması durumunda Cosmos DB ile yüksek kullanılabilirlik

Bölgesel kesintiler nadir değildir ve Azure Cosmos DB her zaman veritabanınızı yüksek oranda kullanılabilir olmasını sağlar. Aşağıdaki ayrıntıları Cosmos hesabı yapılandırmanıza bağlı olarak bir kesinti sırasında Cosmos DB davranışı Yakala:

- Bir yazma işlemi, istemci için Onaylandı önce Cosmos DB ile veri yazma işlemleri kabul eden bir bölgedeki bir çekirdeği tarafından dizinlendiğini.

- Çok bölgeli hesaplar birden çok yazma bölgeleri ile yapılandırılmış, yazma ve okuma için yüksek oranda kullanılabilir olacaktır. Bölgesel yük devretme anlıktır ve değişiklikleri uygulamadan gerekmez.

- **Çok bölgeli hesaplar tek yazma bölgesi (yazma bölgesi kesinti) ile:** Yazma bölgesi kesinti sırasında okuma için bu hesapları yüksek oranda kullanılabilir olarak kalır. Ancak, yazma için şunları yapmalısınız **"otomatik yük devretmeyi etkinleştir"** , Cosmos üzerinde başka bir bölgeye etkilenen bölgeye yük devretme için hesap. Yük devretme, belirttiğiniz bölge öncelik sırasına göre gerçekleştirilir. Etkilenen bölge yeniden çevrimiçi olduğunda, kesinti sırasında etkilenen yazma bölgesinde mevcut çoğaltılmamış veriler aracılığıyla kullanılabilir hale getirileceğini [akışı çakışmaları](how-to-manage-conflicts.md#read-from-conflict-feed). Uygulama, çakışmaları akışı, uygulamaya özgü mantığı temelinde çakışmaları çözün ve güncelleştirilmiş veriler uygun şekilde Cosmos kapsayıcıya geri yazma okuyabilir. Daha önce etkilenen yazma bölgesi kurtarır sonra otomatik olarak bir okuma bölgesi kullanılabilir hale gelir. Elle yük devretme çağırmak ve etkilenen bölgeyi yazma bölgesi yapılandırın. Kullanarak el ile bir yük devretme yeniden yapabilirsiniz [Azure CLI veya Azure portalında](how-to-manage-database-account.md#manual-failover). Var. **veri veya kullanılabilirliği kaybı olmadan** öncesinde, sırasında veya sonrasında el ile yük devretme. Uygulamanız, yüksek oranda kullanılabilir olmaya devam eder. 

- **Çok bölgeli hesaplar tek yazma bölgesi (okuma bölgesi kesinti) ile:** Okuma bölgesi kesinti sırasında bu hesapları okuma ve yazmalar için yüksek oranda kullanılabilir kalır. Etkilenen bölgeyi yazma bölgesi otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. [Cosmos DB SDK'ları](sql-api-sdk-dotnet.md) yeniden yönlendirme tercih edilen bölge listesindeki bir sonraki kullanılabilir bölgeye çağrıları okur. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda okuma bölgesi kesinti işlemek için gerekli değildir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi ile geçerli yazma bölgesine otomatik olarak eşitler ve yeniden okuma isteklerine hizmet vermeye kullanıma sunulacaktır. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir. Hem yük devretme ve daha önce başarısız bölgesi aşamalarını sırasında tutarlılık garantileri Cosmos DB tarafından kabul devam okuyun.

- Tek bölgeli hesaplar, bölgesel bir kesintinin ardından kullanılabilirlik kaybedebilir. Her zaman ayarlanan önerilir **en az iki bölgeleri** (tercihen en az iki bölgeleri yazma) Cosmos hesabınızla her zaman yüksek kullanılabilirlik sağlamak için.

- Hatta nadir ve talihsiz olayda Azure bölgesi kalıcı olarak kurtarılamaz olduğunda olup olmadığını veri kaybı olmadan, çok bölgeli Cosmos hesabınızın varsayılan tutarlılık düzeyi ile yapılandırılır *güçlü*. Sınırlanmış eskime durumu tutarlılık ile yapılandırılmış çok bölgeli Cosmos hesaplar için bir kalıcı olarak kurtarılamaz yazma bölgesi olması durumunda olası veri kaybı penceresini eskime penceresine sınırlıdır (*K* veya *T*); oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri için olası veri kaybı penceresini en fazla beş saniye sınırlıdır. 

## <a name="availability-zone-support"></a>Kullanılabilirlik alanı desteği

Azure Cosmos DB bölgesel kesintiler sırasında yüksek kullanılabilirlik ve dayanıklılık sağlayan bir çok yöneticili Global olarak dağıtılmış veritabanı hizmetidir. Buna ek olarak çapraz bölge dayanıklılığı için artık etkinleştirebilirsiniz **bölge yedekliliği** , Azure Cosmos veritabanı ile ilişkilendirmek için bir bölge seçerken. 

Kullanılabilirlik alanı desteği sayesinde, Azure Cosmos DB, çoğaltmalar yüksek kullanılabilirlik ve dayanıklılığı, bölgesel hatalar sırasında sağlamak için belirli bir bölge içinde birden çok bölge arasında yerleştirilir sağlayacaktır. Gecikme süresi ve bu yapılandırmada diğer SLA'ları için bir değişiklik bulunmamaktadır. Tek bir bölge arıza durumunda, tam veri dayanıklılığı ile RPO bölge artıklığı sağlar olay = 0 ve RTO kullanılabilirlikle = 0. 

Bölge artıklık bir *ek özellik* için [çok yöneticili çoğaltma](how-to-multi-master.md) özelliği. Tek başına bölge artıklığı üzerine bölgesel dayanıklılık elde etme dayanan olamaz. Örneğin, bölgesel kesintilerden veya bölgeler arasında düşük gecikme süreli erişim olması durumunda, bölge artıklığı yanı sıra birden çok yazma bölgeleri için tavsiye edilir. 

Azure Cosmos hesabınız için çok bölgeli yazma yapılandırma sırasında bölge yedekliliği olmadan kabul edebileceğiniz ek bir maliyet. Aksi halde, Lütfen bölge yedekliliği desteği için fiyatlandırma ile ilgili aşağıdaki nota bakın. Azure Cosmos hesabınızın mevcut bir bölgeyi bölge yedekliliği bölgesini kaldırmak ve geri etkin bölge yedekliliği ile ekleyerek etkinleştirebilirsiniz.

Bu özellik aşağıdaki Azure bölgelerinde kullanılabilir:

* Birleşik Krallık Güney
* Güneydoğu Asya 

> [!NOTE] 
> Kullanılabilirlik alanları tek bir bölgede Azure Cosmos hesabı etkinleştirme hesabınıza başka bir bölgede eklemeye eşdeğer ücretleri neden olur. Fiyatlandırma hakkında daha fazla bilgi için bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) ve [Azure Cosmos DB için çok bölgeli maliyetine](optimize-cost-regions.md) makaleler. 

Aşağıdaki tabloda, çeşitli hesap yapılandırmasını, yüksek kullanılabilirlik özelliği özetlenmiştir: 

|KPI  |Kullanılabilirlik alanları (AZ olmayan) olmadan tek bölge  |Tek bölge ile kullanılabilirlik alanları (AZ)  |Kullanılabilirlik alanları (AZ, 2 bölge) ile çok bölgeli Yazar – çoğu Önerilen ayar |
|---------|---------|---------|---------|
|Kullanılabilirlik SLA'sı yazma     |   %99,99      |    %99,99     |  99.999%  |
|Kullanılabilirlik SLA'sını okuyun   |   %99,99      |   %99,99      |  99.999%       |
|Fiyat  |  Tek bölge faturama yansıyan fiyat |  Tek bölge kullanılabilirlik alanı faturama yansıyan fiyat |  Çok bölgeli faturama yansıyan fiyat       |
|Bölge hataları – veri kaybı   |  Veri kaybı  |   Veri kaybı olmadan |   Veri kaybı olmadan  |
|Bölge hataları-kullanılabilirlik |  Kullanılabilirliği kaybı  | Kullanılabilirliği kaybı olmadan  |  Kullanılabilirliği kaybı olmadan  |
|Okuma gecikme süresi    |  Çapraz bölge    |   Çapraz bölge   |    Düşük  |
|Yazma gecikme süresi    |   Çapraz bölge   |  Çapraz bölge    |   Düşük   |
|Bölgesel kesinti – veri kaybı    |   Veri kaybı      |  Veri kaybı       |   Veri kaybı <br/><br/> Kullanarak sınırlanmış eskime durumu tutarlılık birden çok ana ve birden fazla bölge ile veri kaybı sınırlanmış eskime durumu, hesabınızda yapılandırılmış sınırlıdır. <br/><br/> Bölgesel bir kesinti sırasında veri kaybı ile birden çok bölgede güçlü tutarlılık yapılandırarak önlenebilir. Bu seçenek kullanılabilirliğini ve performansını etkileyen ödünler.      |
|Bölgesel kesinti-kullanılabilirlik  |  Kullanılabilirliği kaybı       |  Kullanılabilirliği kaybı       |  Kullanılabilirliği kaybı olmadan  |
|Aktarım hızı    |  Sağlanan aktarım hızı X RU/s      |  Sağlanan aktarım hızı X RU/s       |  RU/sn sağlanan aktarım hızı x 2 <br/><br/> Bu yapılandırma modunu iki kez tek bir bölge için kullanılabilirlik alanları ile karşılaştırıldığında iki bölgeleri olduğundan işleme miktarını gerektirir.   |

Yeni veya var olan Azure Cosmos hesapları bir bölge ekleme sırasında bölge yedekliliği etkinleştirebilirsiniz. Şu anda PowerShell veya Azure Resource Manager şablonlarını kullanarak bölge artıklığı yalnızca etkinleştirebilirsiniz. Azure Cosmos hesabınızda bölge yedekliliği etkinleştirmek için ayarlamalısınız `isZoneRedundant` bayrak `true` belirli bir konum. Bu bayrak konumları özelliği içinde ayarlayabilirsiniz. Örneğin, aşağıdaki powershell kod parçacığı "Güneydoğu Asya" bölgesi için bölge artıklığı sağlar:

```powershell
$locations = @( 
    @{ "locationName"="Southeast Asia"; "failoverPriority"=0; "isZoneRedundant"= "true" }, 
    @{ "locationName"="East US"; "failoverPriority"=1 } 
) 
```

## <a name="building-highly-available-applications"></a>Yüksek düzeyde erişilebilir uygulamalar oluşturma

- Yüksek yazma emin olun ve Okunabilirlik için birden çok yazma bölgeleri ile en az iki bölgeleri yayılmasını Cosmos hesabınızı yapılandırın. Bu yapılandırma sağlayacak en düşük gecikme, yüksek kullanılabilirlik ve her ikisi için de en iyi ölçeklenebilirlik okur ve SLA'lar ile desteklenen yazar. Daha fazla bilgi için bkz. nasıl [Cosmos hesabınız ile birden çok yazma bölgeleri yapılandırma](tutorial-global-distribution-sql-api.md).

- Bir yazma tek bölge ile yapılandırılmış olan çok bölgeli Cosmos hesapları için [otomatik yük devretme, Azure CLI veya Azure portalını kullanarak etkinleştirmeniz](how-to-manage-database-account.md#automatic-failover). Bölgesel bir olağanüstü durumda olduğunda otomatik yük devretme etkinleştirdikten sonra Cosmos DB devreder otomatik olarak hesabınızı.  

- Cosmos hesabınızı yüksek oranda kullanılabilir olsa bile, uygulamanızın doğru bir şekilde yüksek oranda kullanılabilir kalmasını tasarlanmamış olabilir. Uygulamanızın uçtan uca yüksek kullanılabilirliğini sınamak için düzenli aralıklarla çağırma [Azure CLI veya Azure portalını kullanarak el ile yük devretme](how-to-manage-database-account.md#manual-failover), uygulamayı test etmek veya olağanüstü durum kurtarma (DR) bir parçası olarak gidilmesini sağlar.

- Global olarak dağıtılmış veritabanı ortam içinde bir bölge çapında kesinti varsa tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki yoktur. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır önce kabul edilebilen maksimum süre anlamanız gerekir. Bir uygulamanın tamamen kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) bilinir. Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin maksimum süreyi anlamanız gereken bir kesintiden sonra kurtarılırken. Zaman dilimi kaybetmeyi göze güncelleştirmeleri, kurtarma noktası hedefi (RPO) bilinir. Azure Cosmos DB için RTO ve RPO için bkz [tutarlılık düzeyleri ve veri dayanıklılığı](consistency-levels-tradeoffs.md#rto)

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makaleleri okuyabilirsiniz:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Genel olarak sağlanan aktarım hızı ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)
* [Cosmos hesabınızın birden çok yazma bölgeleri ile yapılandırma](how-to-multi-master.md)
