---
title: Azure Cosmos DB'de yüksek kullanılabilirlik
description: Bu makalede Azure Cosmos DB yüksek kullanılabilirliği nasıl sağladığını açıklar.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fc818d2d7db60a8def99c2ad635580253dc795e0
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109767"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Azure Cosmos DB ile yüksek kullanılabilirlik

Azure Cosmos DB, verilerinizi Cosmos hesabınızla ilişkili tüm Azure bölgeleri arasında şeffaf biçimde çoğaltır. Cosmos DB, aşağıdaki görüntüde gösterildiği gibi birden çok yedekleme verileriniz için katmanı kullanır:

![Fiziksel bölümleme](./media/high-availability/cosmosdb-data-redundancy.png)

- Verileri Cosmos kapsayıcıları içinde yatay olarak bölümlenir.

- Her bölge içinde çoğaltılır ve çoğaltmaları çoğunluğu tarafından dizinlendiğini tüm yazma işlemlerini ile her bölüm, çoğaltma kümesi tarafından korunur. Çoğaltmaları 10 20 adede kadar hata etki alanlarına dağıtılır.

- Her bölüm tüm bölgeler arasında çoğaltılır. Her bölgede bir Cosmos kapsayıcının tüm veri bölümleri içerir ve yazma kabul edebilir ve okuma hizmet.  

Cosmos hesabınızı N Azure bölgeleri arasında dağıtılırsa, olacaktır en az dört tüm verilerinizin kopyalarını x N. Düşük gecikme süreli veri erişimi sağlayan ve Cosmos hesabınızla ilişkili bölgeler arasında yazma/okuma aktarım hızı ölçeklendirmenin yanı sıra, ayrıca daha fazla bölge (üst N) sahip kullanılabilirliğini artırır.  

## <a name="slas-for-availability"></a>Kullanılabilirlik SLA'ları

Global olarak dağıtılmış bir veritabanı olarak Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayabilir ve kapsamlı SLA sağlar. Aşağıdaki tabloda, tek ve çok bölgeli hesaplar için Cosmos DB tarafından sağlanan yüksek kullanılabilirlik garantisi gösterir. Yüksek kullanılabilirlik için birden fazla yazma bölgeleri için Cosmos hesaplarınızı yapılandırın.

|İşlem türü  | Tek bölge |Çok bölgeli (tek bölge yazar)|Çok bölgeli (çok bölgeli yazar) |
|---------|---------|---------|-------|
|Yazma    | 99.99    |99.99   |99.999|
|Okuma     | 99.99    |99.999  |99.999|

> [!NOTE]
> Uygulamada, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modeli gerçek yazma kullanılabilirliği yayımlanan Sla'lardan önemli ölçüde büyük/küçük harf yüksektir. Gerçek okuma kullanılabilirliği için tüm tutarlılık düzeyi yayımlanan SLA'lar yüksektir.

## <a name="high-availability-with-cosmos-db-in-the-face-of-regional-outages"></a>Bölgesel kesintiler karşılaşıldığında Cosmos DB ile yüksek kullanılabilirlik

Bölgesel kesintiler nadir değildir ve Azure Cosmos DB, veritabanının her zaman kullanılabilir olduğundan emin olur. Aşağıdaki ayrıntıları Cosmos hesabınızın yapılandırmasına bağlı olarak bir kesinti sırasında Cosmos DB davranışı Yakala:

- Bir yazma işlemi, istemci için Onaylandı önce Cosmos DB ile veri yazma işlemleri kabul eden bir bölgedeki bir çekirdeği tarafından dizinlendiğini.

- Çok bölgeli hesaplar birden çok yazma bölgeleri ile yapılandırılmış, yazma ve okuma için yüksek oranda kullanılabilir olacaktır. Bölgesel yük devretme anlıktır ve değişiklikleri uygulamadan gerekmez.

- Çok bölgeli hesaplar tek yazma bölgesi ile: Yazma bölgesi kesinti sırasında okuma için bu hesapları yüksek oranda kullanılabilir olarak kalır. Ancak, yazma işlemleri için "otomatik yük devretme Cosmos hesabınızda etkilenen bölge ilişkili başka bir bölgeye yük devretme için etkinleştirmeniz gerekir". Yük devretme, belirttiğiniz bölge öncelik sırasına göre gerçekleştirilir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, kesinti sırasında etkilenen yazma bölgesinde mevcut çoğaltılmamış veri akışı çakışmalar arasında kullanılabilir hale getirilir. Uygulama, çakışmaları akışı, uygulamaya özgü mantığı temelinde çakışmaları çözün ve güncelleştirilmiş veriler uygun şekilde Cosmos kapsayıcıya geri yazma okuyabilir. Daha önce etkilenen yazma bölgesi kurtarır sonra otomatik olarak bir okuma bölgesi kullanılabilir hale gelir. Elle yük devretme çağırmak ve etkilenen bölgeyi yazma bölgesi yapılandırın. Elle yük devretme kullanarak yapabileceğiniz [Azure CLI veya Azure portalında](how-to-manage-database-account.md#manual-failover). Var. **veri veya kullanılabilirliği kaybı olmadan** öncesinde, sırasında veya sonrasında el ile yük devretme. Uygulamanız, yüksek oranda kullanılabilir olmaya devam eder. 

- Çok bölgeli hesaplar tek yazma bölgesi ile: Okuma bölgesi kesinti sırasında bu hesapları okuma ve yazmalar için yüksek oranda kullanılabilir kalır. Etkilenen bölgeyi yazma bölgesi otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. Cosmos DB SDK'ları okuma çağrıları tercih edilen bölge listedeki sonraki kullanılabilir bölgeye yönlendirir. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda okuma bölgesi kesinti işlemek için gerekli değildir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi ile geçerli yazma bölgesine otomatik olarak eşitler ve yeniden okuma isteklerine hizmet vermeye kullanıma sunulacaktır. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir. Hem yük devretme ve daha önce başarısız bölgesi aşamalarını sırasında okuma tutarlılığı garantilerini Cosmos DB tarafından kabul devam eder.

- Tek bölgeli hesaplar, bölgesel bir kesintinin ardından kullanılabilirlik kaybedebilir. Her zaman yüksek kullanılabilirlik sağlamak için en az iki bölgeleri (tercihen en az iki yazma bölgeleri) Cosmos hesabınızla ayarlamak için önerilir.

- Bile oldukça nadir ve talihsiz olayda Azure bölgesi kalıcı olarak kurtarılamaz olduğunda olup olmadığını olası veri kaybı olmadan çok bölgeli Cosmos hesabınızı güçlü, varsayılan tutarlılık düzeyi ile yapılandırılır. Sınırlanmış eskime durumu tutarlılık ile yapılandırılmış çok bölgeli Cosmos hesaplar için bir kalıcı olarak kurtarılamaz yazma bölgesi olması durumunda olası veri kaybı penceresini sınırlanmış eskime durumu penceresine; oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri için olası veri kaybı penceresini en fazla beş saniye sınırlıdır.

## <a name="building-highly-available-applications"></a>Yüksek düzeyde erişilebilir uygulamalar oluşturma

- Yüksek yazma emin olun ve Okunabilirlik için birden çok yazma bölgeleri ile en az iki bölgeleri yayılmasını Cosmos hesabınızı yapılandırın. Bu yapılandırma veririz en düşük gecikme süresi, kullanılabilirlik ve ölçeklenebilirlik için her ikisi de okur ve SLA'lar ile desteklenen yazar. Daha fazla bilgi için bkz. nasıl [Cosmos hesabınız ile birden çok yazma bölgeleri yapılandırma](tutorial-global-distribution-sql-api.md). Çok yöneticili uygulamalarınızda yapılandırmak için bkz [çok ana yapılandırma](how-to-multi-master.md).

- Bir yazma tek bölge ile yapılandırılmış olan çok bölgeli Cosmos hesapları için [otomatik yük devretme, Azure CLI veya Azure portalını kullanarak etkinleştirmeniz](how-to-manage-database-account.md#automatic-failover). Bölgesel bir olağanüstü durumda olduğunda otomatik yük devretme etkinleştirdikten sonra Cosmos DB devreder otomatik olarak hesabınızı.  

- Cosmos hesabınızı yüksek oranda kullanılabilir olsa bile, uygulamanızın doğru bir şekilde yüksek oranda kullanılabilir kalmasını tasarlanmamış olabilir. Uçtan uca yüksek kullanılabilirlik için uygulamanızı test etmek için düzenli aralıklarla çağırma [Azure CLI veya Azure portalını kullanarak el ile yük devretme](how-to-manage-database-account.md#manual-failover), uygulamayı test etmek veya olağanüstü durum kurtarma (DR) bir parçası olarak gidilmesini sağlar.


İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır önce kabul edilebilen maksimum süre anlamanız gerekir. Bir uygulamanın tamamen kurtarmak için gereken süre, Kurtarma süresi hedefi (RTO) bilinir. Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin maksimum süreyi anlamanız gereken bir kesintiden sonra kurtarılırken. Zaman dilimi kaybetmeyi göze güncelleştirmeleri, kurtarma noktası hedefi (RPO) bilinir.

Aşağıdaki tabloda en yaygın senaryolar için RTO ve RPO gösterilmektedir.

|Sayı, bölgeler |Yapılandırma |Tutarlılık Düzeyi|RPO |RTO |
|---------|---------|---------|-------|-------|
|1    | *    |*   | < 240 dakika | < 1 hafta |
|>1     | Tek yöneticili çoğaltma | Oturum, tutarlı ön ek, nihai | < 15 dakika | < 15 dakika |
|>1     | Tek yöneticili çoğaltma | Sınırlanmış Eskime Durumu | K &AMP; T | < 15 dakika |
|>1     | Çok yöneticili çoğaltma | Oturum, tutarlı ön ek, nihai | < 15 dakika | 0 |
|>1     | Çok yöneticili çoğaltma | Sınırlanmış Eskime Durumu | K &AMP; T | 0 |
|>1     | * | Güçlü | 0 | < 15 dakika |

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makalede aktarım hızını ölçeklendirme hakkında bilgi edinebilirsiniz:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Genel olarak sağlanan aktarım hızı ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)
* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md)
