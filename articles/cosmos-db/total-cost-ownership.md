---
title: Azure Cosmos DB ile Ownership(TCO) toplam maliyeti
description: Bu makalede Azure Cosmos DB ile Iaas sahipliğini toplam maliyeti karşılaştırır ve şirket içi veritabanları
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: c3a3305197802906077dab330a6b51c1195c6c36
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58879480"
---
# <a name="total-cost-of-ownershiptco-with-azure-cosmos-db"></a>Azure Cosmos DB ile Ownership(TCO) toplam maliyeti

Azure Cosmos DB ile hassas çok kiracılılık ve kaynak İdaresi tasarlanmıştır. Bu tasarım, Azure Cosmos DB'ın önemli ölçüde daha düşük maliyet ve Yardım kullanıcıları çalışmasını sağlar. Şu anda Azure Cosmos DB ile sürekli artan yoğunluğu ve müşteri iş yükleri bir küme içinde binlerce tek bir makinede 280'den fazla müşteri iş yüklerini destekler. Bunu daha fazla bilgi edinmek için bkz: bir veri merkezindeki birden fazla küme ve kümedeki farklı makineler arasında yük dengeleyen müşteri iş yüklerinin kopyaları [Azure Cosmos DB: Global olarak dağıtılmış veritabanı sınır gönderme](https://azure.microsoft.com/blog/azure-cosmos-db-pushing-the-frontier-of-globally-distributed-databases/). Kaynak İdaresi, çok kiracılı modeli ve Azure altyapı geri kalanı ile yerel tümleştirme nedeniyle ortalama 4 ila 6 kat ucuz MongoDB, Cassandra ya da diğer OSS Iaas veritabanı 10 kat ucuz kadar çalıştırıldığı NoSQL Azure Cosmos DB Şirket içinde çalışan altyapıları. Kağıt bakın [(olmayan) bir NoSQL veritabanı bulut hizmeti sahipliğini toplam maliyeti](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf).

OSS NoSQL veritabanı çözümleri, Apache Cassandra, MongoDB, HBase, altyapıları gibi şirket içi için tasarlanmıştır. Yönetilen bir hizmet olarak sunulan bir Resource Manager şablonu ile sağlanan kümeler yönetme ve izleme desteği için bir kiracı veritabanı eşdeğerdir. OSS NoSQL mimarileri önemli işletimsel ek yük gerektirir ve uzmanlığa zor ve pahalı bulmak olabilir. Öte yandan, Azure Cosmos DB işletmeniz için yeniliklerin yerine yönetme ve veritabanı altyapısına bakım uygulama geliştiricilerin odaklanmasını sağlar bir tam olarak yönetilen bir bulut hizmetidir. 

Bir buluta özgü veritabanı hizmeti Azure Cosmos DB, OSS NoSQL veritabanı altyapıları değil tasarlanmış ve kaynak İdaresi veya temel mimari ilkeleri olarak ayarlanmış çok kiracılı modeli ile oluşturulmuş. Cassandra ve MongoDB gibi OSS NoSQL veritabanı altyapıları, üzerinde çalıştıkları sanal makinenin tüm kaynaklar kullanımları için kullanılabilir bir temel varsayılmaktadır. Bu veritabanı altyapılarını birçoğu, kaynakların miktarını belirli bir eşiğin altına düşmesi durumunda çalışamaz. Örneğin, küçük bir VM için örnekleri ve daha yüksek sağlama maliyeti genellikle büyük ölçekli Vm'lerle önerme satıcı önerilen yapılandırmaları ile kullanılabilir. Bu nedenle, OSS NoSQL barındırmak mümkün değil veya tüm diğer şirket içi veritabanı altyapısı ve ikinci veya tüketilen depolama alanı başına istek sayısı gibi tüketim tabanlı şarj model kullanarak kullanılabilir hale.

## <a name="total-cost-of-ownership-of-azure-cosmos-db"></a>Azure Cosmos DB sahipliğini toplam maliyeti 

Sunucusuz sağlama modelinin Azure Cosmos DB'nin veritabanı altyapısı işlemi, fazladan sağlama ihtiyacını ortadan kaldırır. Azure Cosmos DB kaynaklarını özel yapılandırmaları gerek kalmadan sağlanmadı veya lisans. Sonuç olarak, Azure Cosmos DB tarafından desteklenen uygulamaların OSS NoSQL veritabanlarına kıyasla sahipliği tasarruf toplam maliyeti kadar bir yüzde 70 ile çalıştırabilirsiniz. Gerçek zamanlı bazı örnekler için bkz. [müşteri kullanım örnekleri](https://customers.microsoft.com/en-us/search?sq=Cosmos%20DB&ff=&p=0&so=story_publish_date%20desc). Azure Cosmos DB fiyatlandırma modelinin diğer avantajları şunlardır:

* **Fiyat için mükemmel bir değer:** Pazar analistleri, müşteriler ve iş ortakları, Azure Cosmos DB ne müşteriler bu çözümler, kendi ya da diğer satıcılardan uygularken alabilir için kıyasla çok daha düşük bir fiyatla sunduğu tüm özelliklerden büyük bir değer doğrulanmıştır. Veritabanı özellikleri gibi küresel dağıtım, birden çok yöneticili ve sezgisel, iyi tanımlanmış tutarlılık modeli, otomatik dizin oluşturma büyük ölçüde basitleştirilmiş Azure Cosmos DB ile herhangi bir ek yükü, karmaşık veya kapalı kalma süresi olmadan.

* **Hiç NoSQL DevOps yönetim gereklidir:** Azure Cosmos DB ile bir DevOps dağıtımları yönetmek için bakım, ölçek ve düzeltme eki gerçekleştirmek görevlendirmek gerekmez. OSS NoSQL küme barındırılan şirket içi veya Bulut altyapısı üzerinde yaptığınız tüm iş yüklerini yürütebilir.

![Azure Cosmos DB sahip olma maliyeti](./media/total-cost-ownership/tco.png)

* **Esnek bir şekilde ölçeklendirme olanağı:** Azure Cosmos DB aktarım hızını, böylece yoğun olmayan zamanlarda sahip olma maliyetini azaltmak yukarı ve aşağı ölçeklendirilebilir. Bir bulut altyapısında dağıtılmış OSS NoSQL kümelerdeki sınırlı esneklik sunar ve şirket içi dağıtımlar tanımına göre esnek değildir. Daha fazla aktarım hızı sağlarsanız doğrusal olarak ölçeklendirmek için Azure Cosmos DB'de aktarım hızınızı garanti edilir. Bu garanti, finansal SLA'ları ve dilediğiniz ölçekte 99. yüzdebirlik yedeklenir.

* **Ekonomik ölçeklendirme:** Azure Cosmos DB çok sayıda düğüm çalışır gibi yönetilen bir hizmet tümleşik yerel ağ, depolama ve işlemler. Azure Cosmos DB'nin büyük ölçekli nedeniyle, Standardizasyon maliyetlerinden tasarruf edebilirler.

* **Bulut için iyileştirilmiş:** Azure Cosmos DB, en başından başlayarak ayrıntılı çok kiracılılık ve performans yalıtımı birlikte gelen tasarlanmıştır. Bu en uygun şekilde yerleştirme, yürütme ve kümeler ve veri merkezleri arasında binlerce kiracılar ve iş yüklerini Dengeleme sağlar. Buna karşılık, OSS NoSQL veritabanları geçerli nesil şirket içi tek bir kiracının iş yükünü çalıştırmak için kabul tüm sanal makine ile çalışır. Bu veritabanları, ayrıca bir bulut sağlayıcısının altyapı ve tam kapsamı için donanım yararlanmak için tasarlanmamıştır. Örneğin, bir rutin görüntüsü bir OSS NoSQL veritabanı altyapısı olan bir sanal makine Vs arasındaki farkların farkında değil yükseltebilir veya olgu, premium disk zaten üç yönlü çoğaltılır. Bu avantajlarından yararlanın ve müşterilerine avantajlar ve tasarruf geçirin.

* **Saat bazında ödeme yaparsınız:** Herhangi bir noktada zaman içinde ölçeklendirmek için gereken, büyük ölçekli iş yükleri için yalnızca saatlik olarak ücretlendirilir. Uygulama iş yükünü, genellikle yılın ve tarafından sorgulanan veriler arasında farklılık gösterir. Azure Cosmos DB ile yukarı veya aşağı gerekir ve yalnızca ihtiyacınız olan kadarını ödersiniz olarak ölçeklendirebilirsiniz. Donanım saatte yetkisini almak için bir yol olmadığından bu model, şirket içi veya Iaas tarafından barındırılan sistemler ile aynı olamaz. Böyle durumlarda, büyük olasılıkla arasında 14 10 kez Azure Cosmos DB ile bir ortalama kaydedebilirsiniz.

* **Çok sayıda özelliğe Ücretsiz edinin:** Azure Cosmos DB'de iş yüklerinin önemli ölçüde daha ucuz alternatif veritabanı hizmetleri karşılaştırılır yazın. Ayrıca, Azure Cosmos DB özellikleri gibi gibi sunar [otomatik dizin oluşturma](indexing-policies.md), [yaşam süresi (TTL)](time-to-live.md), [değişiklik akışı](change-feed.md) ve diğer hiçbir ek bir ücret olmadan diğer veritabanı hizmetlerinin genellikle ücret bir şey.

* **Kullanır, çeşitli iş yükleri için para birimi birleşik:** Alternatif teklifleri, Azure Cosmos DB'de aksine, segment iş yükleri için örneğin, okuma ve yazma işlemleri ihtiyacınız yoktur. Veya sağlama aktarım hızını bir yazma üretimi ve aktarım hızı okuma iş yükü türüne göre. Azure Cosmos DB'de istek birimleri birleşik ve normalleştirilmiş bir para kullanarak sağlanan aktarım hızı ayrılmış ya da RU/sn'ye Azure Cosmos DB, iş yüklerinize öncelik atayın, kapasite planlaması gerçekleştirmek veya her tür kapasitesi için ödeme zorlamaz ayrı olarak. Bu tür bir yaklaşım, aynı RU/sn çeşitli işlemleri ve iş yükü türleri arasında kolayca değişim olanak tanır.

* **Ölçeklendirme için VM sağlama gerektirmez:** Çoğu işletimsel veritabanı ölçek istiyorsanız gürültücü Komşuları önlemek için büyük sanal makineler ve gevşek kaynak İdaresi için Git gerektirir. Bu, müşterilerin yükü ve maliyeti, ön taahhüt koyar. Azure Cosmos DB ile küçük başlayın ve sorunsuz bir şekilde ve olmadan herhangi bir kapalı kalma süresi veya veri kullanılabilirliği üzerindeki etkisini büyük ölçekli iş yükü boyutları halinde büyütün.

* **Sağlanan aktarım hızı üst sınırı için kullanabilir:** Azure Cosmos DB'de alt çekirdek çoğullama virtue tarafından sağlanan işleme seçenekleri veya üçüncü taraf teklifler Iaas barındırılan daha büyük ölçüde karşılayabilir. Bu yöntem, çok fazla birden çok alternatif çözümler kaydeder.

* **Diğer Azure hizmetleriyle derin tümleştirme Azure Cosmos DB'nin.** Azure Cosmos DB, ağ, hesaplama, Azure işlevleri (sunucusuz), Azure IOT ve diğer Azure Hizmetleri ile yerel bir tümleştirmesi vardır. Bu tümleştirme sayesinde, güçlü garanti ile dünya genelinde veri çoğaltma hızını en iyi performansı alırsınız. Üçüncü taraf çözümleri, eşleştirilecek mümkün olmayacaktır veya genellikle gibi özellikler sunmak için bir premium ücretlendirme.

* **Otomatik olarak yüksek kullanılabilirlik, en az 10-20 hata etki alanları varsayılan olarak sahip olursunuz:** Azure Cosmos DB, yüksek kullanılabilirlik için kritik bir özellik hata etki alanlarında iş yüklerinin dağıtımını destekler. 99. yüzdebirlik dilimde arasında herhangi bir yazma ve okuma için 99.999 yüksek kullanılabilirlik sunar dünyanın. Şunun gibi kendi sunucunuzda veya bir üçüncü taraf çözümü ile uygulama maliyeti olur yüksek.

* **Otomatik olarak tüm kurumsal özellikleri ek maliyet olmaksızın alın.** Azure Cosmos DB, en kapsamlı uyumluluk sertifikaları, güvenlik ve şifreleme bekleyen ve Hareket halindeki (rakibimizi için karşılaştırıldığında) ek maliyet olmadan sunar. Otomatik olarak dünyanın herhangi bir yere bölgesel kullanılabilirlik alın. Veritabanınızı herhangi sayıda Azure bölgesine yayılan ekleyebilir veya herhangi bir noktada bölgeleri kaldırın.

* **Ayrılmış bir kapasiteye sahip en fazla % 65'e maliyetlerin kaydedebilirsiniz:** Azure Cosmos DB [ayrılan kapasite](cosmos-db-reserved-capacity.md) para'ı kaydetmek için Azure Cosmos DB kaynaklarını bir yıl veya üç yıl için önceden ödeme yaparak yardımcı olur. Önemli ölçüde, bir yıllık veya üç yıllık ön taahhüt ile maliyetlerinizi azaltın ve normal fiyatlandırma karşılaştırıldığında 20 %65 indirimleri arasında kaydedin. Görev açısından kritik iş yüklerinizi kapasite sağlama açısından daha iyi SLA elde edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [nasıl Azure Cosmos fiyatlandırma modeli DB, müşterilerin uygun maliyetli](total-cost-ownership.md)
* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)
* Daha fazla bilgi edinin [toplam bir NoSQL veritabanı bulut hizmetinin (olmayan) sahip olma maliyeti](https://documentdbportalstorage.blob.core.windows.net/papers/11.15.2017/NoSQL%20TCO%20paper.pdf)
