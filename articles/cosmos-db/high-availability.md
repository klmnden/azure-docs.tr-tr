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
ms.openlocfilehash: fdf1bf227ed21111c1aa0754a8423c1ee6aa151b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244146"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Azure cosmos DB'de yüksek kullanılabilirlik

Azure Cosmos DB, verilerinizi Cosmos hesabınızla ilişkili Azure bölgeleri arasında şeffaf biçimde çoğaltır.  Cosmos DB, birden çok yedekleme verileriniz için katmanı kullanır. Aşağıdaki şekilde görüldüğü gibi:

- Verileri Cosmos kapsayıcıları içinde yatay olarak bölümlenir.
- Her bölge içinde çoğaltılır ve çoğaltmaları çoğunluğu tarafından dizinlendiğini tüm yazma işlemlerini ile her bölüm, çoğaltma kümesi tarafından korunur. Çoğaltmaları 10 20 adede kadar hata etki alanlarına dağıtılır.
- Tüm bölgelerde tüm bölümleri çoğaltılır. Her bölgede tüm Cosmos kapsayıcı veri bölümlerini içerir ve yazma kabul edin ve okuma hizmet.  

![Kaynak bölümleme](./media/high-availability/figure1.png) 

Cosmos hesabınızı N Azure bölgeleri arasında dağıtılırsa, olacaktır en az 4 tüm verilerinizin kopyalarını x N. Daha fazla bölge (üst N) de sahip Cosmos hesabınızla ilişkili bölgeler arasında düşük gecikme süreli erişim verilerinize ve ölçeklendirme yazma ve okuma işlemesi sağlamanın yanı sıra kullanılabilirliğini artırır.  

## <a name="availability-slas"></a>Kullanılabilirlik SLA'ları  

Global olarak dağıtılmış bir veritabanı olarak Cosmos DB, aktarım hızı, düşük gecikme süresi 99. yüzdebirlik dilimde, tutarlılık ve yüksek kullanılabilirlik kapsayan kapsamlı SLA'lar sağlar. Aşağıdaki tabloda, tek ve çok bölgeli hesaplar için Cosmos DB tarafından sağlanan yüksek kullanılabilirlik için ilgili garanti açıklanmaktadır. En yüksek kullanılabilirlik için birden fazla yazma bölgeleri için Cosmos hesaplarınızı yapılandırma öneririz.

|İşlem Türü  | Tek bölge |Çok bölgeli (tek bölge yazar)|Çok bölgeli (çok bölgeli Yazar|
|---------|---------|---------|-------|
|Yazar    | % 99,99    |% 99,99   |99,999|
|Okur     | % 99,99    |99,999  |99,999|

> [!NOTE]
> Uygulamada, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modeli gerçek yazma kullanılabilirliği yayımlanan Sla'lardan önemli ölçüde büyük/küçük harf yüksektir. Ve gerçek okuma kullanılabilirliği için tüm tutarlılık düzeyi uygulamada, yayımlanan Sla'lardan önemli ölçüde daha yüksek.

## <a name="regional-outages"></a>Bölgesel kesintiler

Bölgesel kesintiler nadir değildir ve Azure Cosmos DB, veritabanının her zaman kullanılabilir olduğundan emin olur. Aşağıda, Cosmos hesabı yapılandırmanıza bağlı olarak bir kesinti sırasında Cosmos DB davranışı yakalar: 

- Her ikisi de yazar ve bölgesel bir kesinti sırasında okur ile birden çok yazma bölgeleri yapılandırılmış çok bölgeli hesaplar yüksek oranda kullanılabilir kalır. Bölgesel yük devretme anlıktır ve değişiklikleri uygulamadan gerektirmez.

- Yazma bölgesi kesinti sırasında bir yazma tek bölge ile çok bölgeli hesaplar için okuma yüksek oranda kullanılabilir olarak kalır. Ancak, yazma işlemleri için "otomatik yük devretme Cosmos hesabınızdaki Cosmos DB için etkilenen bölge Cosmos hesabınızla ilişkili başka bir bölgeye yük devretme için etkinleştirmeniz gerekir". Yük devretme, belirttiğiniz bölge öncelik sırasına göre gerçekleştirilir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, kesinti sırasında etkilenen yazma bölgesinde mevcut çoğaltılmamış veri akışı çakışmalar arasında kullanılabilir hale getirilir. Uygulama, çakışmaları akışı, uygulamaya özgü mantığı temelinde çakışmaları çözün ve güncelleştirilmiş veriler uygun şekilde Cosmos kapsayıcıya geri yazma okuyabilir. Daha önce etkilenen yazma bölgesi kurtarır sonra otomatik olarak bir okuma bölgesi kullanılabilir hale gelir. Elle yük devretme çağırmak ve etkilenen bölgeyi yazma bölgesi olarak geri getirin. Azure CLI veya Azure portalını kullanarak bunu el ile bir yük devretme yapabilirsiniz.  

- Okuma ve yazma işlemleri için çok bölgeli hesaplar okuma bölgesi kesinti sırasında bir yazma tek bölge ile yüksek oranda kullanılabilir kalır. Etkilenen bölgeyi yazma bölgesi otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. Cosmos DB SDK'ları okuma çağrıları tercih edilen bölge listedeki sonraki kullanılabilir bölgeye yönlendirir. Tercih edilen bölge listesi bölgelerde hiçbiri kullanılabilir haldeyse, çağrıları otomatik olarak geçerli yazma bölgesine dönmesi. Değişiklik uygulama kodunuzda okuma bölgesi kesinti işlemek için gerekli değildir. Sonuç olarak, etkilenen bölge yeniden çevrimiçi olduğunda, daha önce etkilenen okuma bölgesi ile geçerli yazma bölgesine otomatik olarak eşitler ve yeniden okuma hizmet vermek için kullanılabilir. Sonraki Okuma, uygulama kodunuz için herhangi bir değişikliğe gerek kalmadan kurtarılan bölgeye yönlendirilir. Hem yük devretme ve daha önce başarısız bölgesi aşamalarını sırasında okuma tutarlılığı garantilerini Cosmos DB tarafından kabul devam eder.

- Tek bölgeli hesaplar, bölgesel bir kesinti durumunda kullanılabilirlik kaybedebilir. Yüksek oranda Cosmos hesabınızla her zaman yüksek kullanılabilirlik sağlamak için en az iki bölgeleri (tercihen en az iki yazma bölgeleri) ayarlamak için öneririz.

## <a name="building-highly-available-applications"></a>Yüksek düzeyde erişilebilir uygulamalar oluşturma

- Yüksek yazma emin olun ve Okunabilirlik için birden çok yazma bölgeleri ile en az iki bölgeleri yayılmasını Cosmos hesabınızı yapılandırın. Bu yapılandırma, en iyi kullanılabilirlik, düşük gecikme süresi ve ölçeklenebilirlik için hem okuma hem de yazma işlemleri SLA'lar ile desteklenen sağlayacaktır. Bkz. nasıl [Cosmos hesabınız ile birden çok yazma bölgeleri yapılandırma](tutorial-global-distribution-sql-api.md).

- Bir yazma tek bölge ile yapılandırılmış olan çok bölgeli Cosmos hesapları için "otomatik yük devretme" Azure CLI veya Azure portalını kullanarak etkinleştirin.  Bölgesel bir olağanüstü durumda olduğunda otomatik yük devretme etkinleştirdikten sonra Cosmos DB devreder otomatik olarak hesabınızı.  

- Cosmos hesabınızı yüksek oranda kullanılabilir olsa bile, uygulamanızın doğru bir şekilde yüksek oranda kullanılabilir kalmasını tasarlanmamış olabilir. Uygulamanız için uçtan uca yüksek kullanılabilirlik sağlamak için düzenli aralıklarla uygulamayı test etmek veya olağanüstü durum kurtarma (DR) bir parçası olarak Azure CLI veya Azure portal'ı kullanarak "el ile yük devretme" çağırma ayrıntılı açıklanmıştır. Daha fazla bilgi için Cosmos hesabınız için bölgesel önceliklerini değiştirmek bakın).  

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makalede aktarım hızını ölçeklendirme hakkında bilgi edinebilirsiniz:

* [Ölçeklendirme aktarım hızı](scaling-throughput.md)

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)

* [Aktarım hızı küresel olarak ölçekleme](scaling-throughput.md)

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)

* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)


