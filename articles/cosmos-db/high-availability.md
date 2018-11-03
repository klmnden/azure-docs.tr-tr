---
title: Azure Cosmos DB'de yüksek kullanılabilirlik
description: Bu makalede Azure Cosmos DB yüksek kullanılabilirliği nasıl sağladığını açıklar.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: fb7de4a913fa1edb3329e25276f83be2fb6cecd8
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961815"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Azure cosmos DB'de yüksek kullanılabilirlik

Azure Cosmos DB, verilerinizi Cosmos hesabınızla ilişkili tüm Azure bölgeleri arasında şeffaf biçimde çoğaltır. Cosmos DB, aşağıdaki görüntüde gösterildiği gibi birden çok yedekleme verileriniz için katmanı kullanır:

![Kaynak bölümleme](./media/high-availability/figure1.png) 

- Verileri Cosmos kapsayıcıları içinde yatay olarak bölümlenir.

- Her bölge içinde çoğaltılır ve çoğaltmaları çoğunluğu tarafından dizinlendiğini tüm yazma işlemlerini ile her bölüm, çoğaltma kümesi tarafından korunur. Çoğaltmaları 10 20 adede kadar hata etki alanlarına dağıtılır.

- Her bölüm tüm bölgeler arasında çoğaltılır. Her bölgede bir Cosmos kapsayıcının tüm veri bölümleri içerir ve yazma kabul edebilir ve okuma hizmet.  

Cosmos hesabınızı N Azure bölgeleri arasında dağıtılırsa, olacaktır en az 4 tüm verilerinizin kopyalarını x N. Düşük gecikme süreli veri erişimi sağlayan ve Cosmos hesabınızla ilişkili bölgeler arasında yazma/okuma aktarım hızı ölçeklendirmenin yanı sıra, ayrıca daha fazla bölge (üst N) sahip kullanılabilirliğini artırır.  

## <a name="slas-for-availability"></a>Kullanılabilirlik SLA'ları   

Global olarak dağıtılmış bir veritabanı olarak Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayabilir ve kapsamlı SLA sağlar. Aşağıdaki tabloda, tek ve çok bölgeli hesaplar için Cosmos DB tarafından sağlanan yüksek kullanılabilirlik için ilgili garanti açıklanmaktadır. Yüksek kullanılabilirlik için birden fazla yazma bölgeleri için Cosmos hesaplarınızı yapılandırın.

|İşlem türü  | Tek bölge |Çok bölgeli (tek bölge yazar)|Çok bölgeli (çok bölgeli yazar) |
|---------|---------|---------|-------|
|Yazar    | % 99,99    |% 99,99   |99,999|
|Okur     | % 99,99    |99,999  |99,999|

> [!NOTE]
> Uygulamada, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modeli gerçek yazma kullanılabilirliği yayımlanan Sla'lardan önemli ölçüde büyük/küçük harf yüksektir. Gerçek okuma kullanılabilirliği için tüm tutarlılık düzeyi yayımlanan SLA'lar yüksektir.

## <a name="regional-outages"></a>Bölgesel kesintiler

Bölgesel kesintiler nadir değildir ve Azure Cosmos DB, veritabanının her zaman kullanılabilir olduğundan emin olur. Aşağıdaki ayrıntıları Cosmos hesabınızın yapılandırmasına bağlı olarak bir kesinti sırasında Cosmos DB davranışı Yakala: 

- Çok bölgeli hesaplar birden çok yazma bölgeleri ile yapılandırılmış, yazma ve okuma için yüksek oranda kullanılabilir kalır. Bölgesel yük devretme anlıktır ve değişiklikleri uygulamadan gerektirmez.

- Bir tek yazma bölgesi ile çok bölgeli hesaplar: bir yazma bölgesi kesinti sırasında bu hesaplar okuma için yüksek oranda kullanılabilir kalır. Ancak, yazma işlemleri için "otomatik yük devretme Cosmos hesabınızda etkilenen bölge ilişkili başka bir bölgeye yük devretme için etkinleştirmeniz gerekir". Yük devretme, belirttiğiniz bölge öncelik sırasına göre gerçekleştirilir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, kesinti sırasında etkilenen yazma bölgesinde mevcut çoğaltılmamış veri akışı çakışmalar arasında kullanılabilir hale getirilir. Uygulama, çakışmaları akışı, uygulamaya özgü mantığı temelinde çakışmaları çözün ve güncelleştirilmiş veriler uygun şekilde Cosmos kapsayıcıya geri yazma okuyabilir. Daha önce etkilenen yazma bölgesi kurtarır sonra otomatik olarak bir okuma bölgesi kullanılabilir hale gelir. Elle yük devretme çağırmak ve etkilenen bölgeyi yazma bölgesi olarak geri getirin. Elle yük devretme kullanarak yapabileceğiniz [Azure CLI veya Azure portalında](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account).  

- Bir tek yazma bölgesi ile çok bölgeli hesaplar: Okuma bölgesi kesinti sırasında bu hesapları okuma ve yazmalar için yüksek oranda kullanılabilir kalır. Etkilenen bölgeyi yazma bölgesi otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. Cosmos DB SDK'ları okuma çağrıları tercih edilen bölge listedeki sonraki kullanılabilir bölgeye yönlendirir. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda okuma bölgesi kesinti işlemek için gerekli değildir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi ile geçerli yazma bölgesine otomatik olarak eşitler ve yeniden okuma isteklerine hizmet vermeye kullanıma sunulacaktır. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir. Hem yük devretme ve daha önce başarısız bölgesi aşamalarını sırasında okuma tutarlılığı garantilerini Cosmos DB tarafından kabul devam eder.

- Tek bölgeli hesaplar, bölgesel bir kesinti durumunda kullanılabilirlik kaybedebilir. Bu en az iki bölgeleri (tercihen en az iki yazma bölgeleri) Kurulum Cosmos hesabınızla her zaman yüksek kullanılabilirlik sağlamak için önerilir.

## <a name="building-highly-available-applications"></a>Yüksek düzeyde erişilebilir uygulamalar oluşturma

- Yüksek yazma emin olun ve Okunabilirlik için birden çok yazma bölgeleri ile en az iki bölgeleri yayılmasını Cosmos hesabınızı yapılandırın. Bu yapılandırma sağlayacak düşük gecikme süresi, kullanılabilirlik ve ölçeklenebilirlik her ikisi için de okur ve SLA'lar ile desteklenen yazar. Daha fazla bilgi için bkz. nasıl [Cosmos hesabınız ile birden çok yazma bölgeleri yapılandırma](tutorial-global-distribution-sql-api.md).

- Bir yazma tek bölge ile yapılandırılmış olan çok bölgeli Cosmos hesapları için [otomatik yük devretme, Azure CLI veya Azure portalını kullanarak etkinleştirmeniz](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account). Bölgesel bir olağanüstü durumda olduğunda otomatik yük devretme etkinleştirdikten sonra Cosmos DB devreder otomatik olarak hesabınızı.  

- Cosmos hesabınızı yüksek oranda kullanılabilir olsa bile, uygulamanızın doğru bir şekilde yüksek oranda kullanılabilir kalmasını tasarlanmamış olabilir. Uygulamanız için uçtan uca yüksek kullanılabilirlik sağlamak için düzenli aralıklarla çağırma [Azure CLI veya Azure portalını kullanarak el ile yük devretme](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account), uygulamayı test etmek veya olağanüstü durum kurtarma (DR) bir parçası olarak gidilmesini sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makalede aktarım hızını ölçeklendirme hakkında bilgi edinebilirsiniz:

* [Ölçeklendirme aktarım hızı](scaling-throughput.md)

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)

* [Aktarım hızı küresel olarak ölçekleme](scaling-throughput.md)

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)

* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)


