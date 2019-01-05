---
title: Azure Cosmos DB’ye giriş
description: Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik, yüksek kullanılabilirlik için oluşturulmuştur ve NoSQL verileri için yerel destek sunar.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 12/18/2018
ms.author: sngun
ms.openlocfilehash: b384bc51ac371ef75f5128c92f7e4b8d7f45ecc6
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034956"
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Günümüzün uygulamaları yüksek derecede yanıt veren ve her zaman çevrimiçi olması gerekir. Düşük gecikme süresi ve yüksek kullanılabilirlik elde etmek için bu uygulamaların örnekleri kendi kullanıcılara yakın olan veri merkezlerinde dağıtılması gerekir. Uygulamaları yoğun saatlerde kullanım büyük değişiklikleri gerçek zamanlı olarak yanıt hacimlerinin veri depolamak ve bu verileri milisaniye cinsinden kullanıcılar için kullanılabilir hale.

Azure Cosmos DB, Microsoft'un Global olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Esnek, aktarım hızını ve depolamayı ölçeklendirin ve en sevdiğiniz API'nizi arasında SQL, MongoDB, Cassandra, tablo ve Gremlin kullanarak hızlı, tek haneli milisaniye veri erişimi avantajlarından yararlanın. Cosmos DB sağlayan kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA'lar) aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantisi için bir şey başka bir veritabanı hizmet sunabilir.

[Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz olarak deneyin](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB, Microsoft'un esnek ölçek genişletme, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Önemli Avantajlar

### <a name="turnkey-global-distribution"></a>Anahtar teslim küresel dağıtım

Cosmos DB, hızlı yanıt veren katı oluşturmanızı ve dünya çapında yüksek kullanılabilirliğe sahip uygulamalar sağlar. Kullanıcılarınız nerede cosmos DB, kullanıcılarınızın bunları en yakın olanında verilerin bir çoğaltması ile etkileşim kurabilmesi verilerinizin şeffaf biçimde çoğaltır.

Cosmos DB veya Cosmos hesabınıza Azure bölgelerinden birini bir düğmeye tıklayarak herhangi bir zamanda kaldırılamıyor sağlar. Cosmos DB, verilerinizi, uygulamanızın yüksek oranda kullanılabilir hizmet çok girişli yeteneklerini olmaya devam ederken, Cosmos hesabınızla ilişkili tüm bölgeler için sorunsuz bir şekilde çoğaltır.

Daha fazla bilgi için [genel dağıtım](distribute-data-globally.md) makalesi.

### <a name="always-on"></a>Her zaman "açıktır"

Azure altyapısıyla kapsamlı tümleştirme sayesinde ve [saydam çok yöneticili çoğaltma](global-dist-under-the-hood.md), Cosmos DB sağlar % 99,999 [yüksek kullanılabilirlik](high-availability.md) hem okur ve yazar. Cosmos DB ayrıca programlı olarak (veya Portal aracılığıyla) Cosmos hesabınızın bölgesel yük devretme çağırma yeteneği sağlar. Bu özellik, Cosmos veritabanı, otomatik olarak yük devretme olabilir, ancak bölgesel bir olağanüstü durum varsa, uygulamanızın rest de yük devretme için tasarlandığını sağlamanıza yardımcı olur.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Aktarım hızını ve depolamayı dünya çapında elastik ölçeklenebilirliği

Cosmos DB saydam yatay bölümleme ve çok yöneticili çoğaltma ile tasarlanmış, dünya çapındaki tüm okuma ve yazma işlemleri için esnek görülmemiş boyutta ölçeklenebilirlik sunar. Esnek, yüz milyonlarca İsteği/sn, tek bir API çağrısı ile dünya çapındaki binlerce gelen ölçeği artırma ve yalnızca ihtiyacınız aktarım hızı (ve depolama için) ödeme yaparsınız. Bu özellik iş yüklerinizi beklenmedik artışları için yoğun işlemi, fazladan sağlama yapmak zorunda kalmadan başa çıkmanıza yardımcı olur. Bkz: [Cosmos DB'de bölümleme](partitioning-overview.md), [kapsayıcılar ve veritabanları sağlanan aktarım hızını](set-throughput.md), ve [sağlanan aktarım hızı küresel olarak ölçekleme](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Dünya çapında 99. yüzdebirlik dilimde düşük gecikme süresi garanti edilir

Oluşturabileceğinizi Cosmos DB'yi kullanarak yüksek derecede yanıt veren, çok büyük ölçekli uygulamalar. Yeni çok yöneticili çoğaltma protokolü ve Mandal ve [yazma için iyileştirilmiş veritabanı altyapısı](index-policy.md), Cosmos DB her ikisi için de 10 MS'den, yazmalarda sayısından az garanti okur ve (dizini oluşturulmuş yazmalar, tüm dünyada 99. yüzdebirlik) . Bu özellik, veri ve İnanılmaz derecede hızlı sorgular yüksek derecede yanıt veren uygulamalar için sürekli alımı sağlar.

### <a name="precisely-defined-multiple-consistency-choices"></a>Tam olarak tanımlı, birden çok tutarlılık seçenekleri

Extreme yapmak zorunda [tutarlılık, kullanılabilirlik, gecikme süresi ve programlama bir denge](consistency-levels-tradeoffs.md). Cosmos DB'nin çok yöneticili çoğaltma Protokolü sunmak üzere tasarlanmış dikkatle [beş iyi tanımlanmış tutarlılık seçeneği](consistency-levels.md) - güçlü, sınırlanmış eskime durumu, tutarlı önek, oturum ve nihai — için sezgisel programlama modeli Düşük gecikme süresi ve yüksek kullanılabilirlik için global olarak dağıtılmış uygulamanızı ile.

### <a name="no-schema-or-index-management"></a>Şema veya dizin yönetimi yok

Veritabanı şema ve dizinlerinizi tutma küresel olarak dağıtılan uygulamalar için özellikle sıkıntılı bir uygulamanın şema ile eşitlenmiş durumda. Ancak, Cosmos DB ile şema veya dizin gerekmez. Veritabanı altyapısı tam olarak şemadan bağımsızdır.  Şema ve dizin yönetimi yok gerekli olduğundan, ayrıca uygulama kapalı kalma süresi hakkında endişelenmek zorunda olmadığınız şemaları geçirirken. Cosmos DB [otomatik olarak tüm verilerin dizinini oluşturur](index-policy.md) – hiçbir şema, gerekli bir dizin yok – ve hızlı sorgular sunar.

### <a name="battle-tested-database-service"></a>Zorlu koşullarda test edilmiştir veritabanı hizmeti

Cosmos DB, azure'da temel bir hizmettir. Neredeyse on yıl için Cosmos DB birçok Microsoft ürünlerinin tarafından Skype, Xbox, Office 365, Azure ve diğer birçok dahil olmak üzere küresel ölçekte görev açısından kritik uygulamalar için kullanıldı. Bugün, Cosmos DB çok sayıda dış müşterileri ve esnek ölçek ve/veya anahtar teslim çoklu veri merkezi/çoklu bölge, çok yöneticili çoğaltma için düşük gecikme süresi ve hem yüksek kullanılabilirlik gerektiren uygulamalar tarafından kullanılan Azure üzerinde en hızlı gelişen hizmetlerden biri Okuma ve yazma işlemleri.

### <a name="ubiquitous-regional-presence"></a>Her yerde bulunan bölgesel varlığı

Cosmos DB, dünya 54 fazla bölgenin genel bulut, Azure Çin 21Vianet, Azure Almanya, Azure kamu ve Savunma Bakanlığı için Azure kamu da dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Bkz: [Cosmos DB'nin bölgesel varlığı](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Varsayılan ve kurumsal kullanıma hazır tarafından güvenli hale getirme

Cosmos DB için sertifikalı bir [çeşit uyumluluk standartlarını](compliance.md). Ayrıca, Cosmos DB'deki tüm veriler şifrelenir, bekleyen ve Hareket halindeki. Cosmos DB, satır düzeyinde yetkilendirme sağlar ve katı güvenlik standartlarına uyar.

### <a name="significant-tco-savings"></a>Toplam sahip olma Maliyetini önemli ölçüde tasarruf

Cosmos DB tamamen yönetilen bir hizmet olduğundan, artık ihtiyacınız olmayan yönetmek ve karmaşık çoklu veri merkezi dağıtımları ve yükseltme işlemleri sırasında veritabanı yazılım, destek, lisans veya işlemleri için ödeme yaparsınız. Bkz: [maliyet Cosmos DB ile en iyi duruma](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Sektör lideri kapsamlı SLA'lar

Cosmos DB, ilk ve tek hizmettir sunmak olduğundan [sektör lideri kapsamlı SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/) % 99,999 yüksek kullanılabilirlik denetimine, okuma ve yazma aktarım hızı ve tutarlılık garantisi 99. yüzdebirlik dilimde düşük gecikme süresi.

### <a name="apache-spark--cosmos-db--operational-analytics-at-global-scale"></a>Apache Spark ve Cosmos DB küresel ölçekte operasyonel analiz =

Çalıştırabileceğiniz [Spark](spark-connector.md) Cosmos DB'de depolanan veriler. Bu özellik, doğrudan Cosmos DB'de işletim işlemsel iş yüklerini etkilemeden küresel ölçekte düşük gecikme süreli, operasyonel analiz gerçekleştirmenize olanak tanır.

### <a name="develop-applications-for-cosmos-db-using-popular-nosql-apis"></a>Popüler NoSQL API kullanarak Cosmos DB için uygulamalar geliştirin

Cosmos DB, güncelleştirme ve Cosmos veritabanınızda depolanan verilerinizi sorgulayın API'leri bir seçenek sunar. Varsayılan olarak, [SQL kullanabileceğiniz](how-to-sql-query.md) güncelleştirmek ve Cosmos veritabanı, verilerinizi sorgulayın.

Cosmos DB ayrıca uygulayan [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) ve [Azure tablo depolama](table-introduction.md) protokolleri hizmette doğrudan bağlayabilirsiniz. Bu, istemci sürücüleri (ve araçlar) işaret edecek şekilde doğrudan Cosmos veritabanınız için yaygın olarak kullanılan NoSQL API'leri sağlar. Sık kullanılan NoSQL API kablo protokolleri destekleyerek, Cosmos DB sağlar:

* Uygulama mantığınızın önemli kısımları korurken Cosmos DB uygulamanıza kolayca geçirin.
* Uygulamanızı taşınabilir tutun ve bulut satıcısı belirsiz durumda kalır.
* Sektör ortak NoSQL API için önde gelen, mali olarak desteklenen SLA'ları alın. 
* Esnek bir şekilde sağlanan aktarım hızı ve depolama için veritabanlarınızı ihtiyaçlarınıza göre ölçeklendirin ve yalnızca aktarım hızını ve ihtiyacınız olan depolama için ödeme yaparsınız. Bu, önemli maliyet tasarrufları yol açar.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çeşitli veriler için düşük yanıt süreleri içinde [küresel](distribute-data-globally.md) ölçekte muazzam miktarlarda veri okuma ve yazma işlemlerini işlemesi gereken herhangi bir [web, mobil, oyun ve IoT uygulaması](use-cases.md) Azure Cosmos DB'nin [garantili](https://azure.microsoft.com/support/legal/sla/cosmos-db/) yüksek kullanılabilirliğinden, yüksek aktarım hızından, neredeyse gerçek zamanlı yanıt süresinden ve ayarlanabilir tutarlılığından yararlanır. Azure Cosmos DB’nin [IoT ve telematik](use-cases.md#iot-and-telematics), [Perakende ve pazarlama](use-cases.md#retail-and-marketing), [Oyun](use-cases.md#gaming) ve [Web ve mobil uygulamalara](use-cases.md#web-and-mobile-applications) nasıl uygulanabileceğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB hakkında daha fazla bilgiyi [genel dağıtım](distribute-data-globally.md) ve [bölümleme](partitioning-overview.md) özellikleri.

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB'nin API'sini kullanmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
