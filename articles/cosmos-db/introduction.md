---
title: Azure Cosmos DB’ye giriş
description: Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik, yüksek kullanılabilirlik için oluşturulmuştur ve NoSQL verileri için yerel destek sunar.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 04/08/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 50e2e36058e025e87302352eaa492f5cf78eb31b
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536432"
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Azure Cosmos DB, Microsoft’un genel olarak dağıtılan çok modelli veritabanıdır. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Diğer veritabanı hizmetlerinin sunamadığı kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA) ile birlikte aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantileri sunar. [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz olarak deneyin](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB, Microsoft'un esnek ölçek genişletme, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

## <a name="key-capabilities"></a>Temel işlevler
Genel olarak dağıtılan, çok modelli bir veritabanı hizmeti olarak Azure Cosmos DB, ölçeklenebilir, küresel ölçekte son derece hızlı yanıt veren uygulamalar oluşturmayı kolaylaştırır:

* **Anahtar teslimi genel dağıtım**
    * [Tek bir düğmeye tıklayarak](tutorial-global-distribution-sql-api.md) istediğiniz kadar çok [Azure bölgesine](https://azure.microsoft.com/regions/) [verilerinizi dağıtabilirsiniz](distribute-data-globally.md). Bu, verilerinizi kullanıcılarınızın bulunduğu yere yerleştirerek müşterileriniz için mümkün olan en düşük gecikme süresini sağlamanıza olanak tanır. 
    * Azure Cosmos DB'nin çok girişli API'lerini kullanarak, uygulama her zaman en yakın bölgenin yerini bilir ve istekleri en yakın veri merkezine gönderir. Bunların tümü yapılandırma değişikliğine gerek kalmadan yapılabilir. Yazma bölgenizi ve istediğiniz kadar çok okuma bölgesini ayarlarsınız, kalan işlemler sizin yerinize yapılır.
    * Azure Cosmos DB veritabanınıza bölgeleri ekleyip kaldırırken uygulamanızın yeniden dağıtılması gerekmez ve birden çok girişli API özelliği sayesinde üst düzeyde kullanılabilir olmaya devam eder.

* **Çoklu veri modelleri ve verilere erişim ile veri sorgulamaya yönelik popüler API’ler**
    * Azure Cosmos DB'nin üzerine yapılandırıldığı ve atom kayıt sırasını (ARS) temel alan veri modeli; belge, grafik, anahtar-değer, tablo ve sütun ailesi veri modelleri de dahil olmak üzere ancak bunlarla sınırlı kalmamak kaydıyla birçok veri modelini yerel olarak destekler.
    * Aşağıdaki veri modellerinin API'leri, birden çok dilde sağlanan SDK'larla desteklenir:
        * [Azure Cosmos DB API SQL](sql-api-introduction.md): Zengin SQL sorgulama özelliklerine sahip şemasız bir JSON veritabanı altyapısı.
        * [MongoDB için Azure Cosmos DB API](mongodb-introduction.md): Yüksek düzeyde ölçeklenebilir *hizmet olarak MongoDB* Azure Cosmos DB platformu tarafından desteklenen. Mevcut MongoDB kitaplıkları, sürücüleri, araçları ve uygulamalarıyla uyumludur.
        * [Cassandra için Azure Cosmos DB API](cassandra-introduction.md): Bir Global olarak dağıtılmış Cassandra olarak-Azure Cosmos DB platformu tarafından desteklenen hizmet. Mevcut [Apache Cassandra](https://cassandra.apache.org/) kitaplıkları, sürücüleri, araçları ve uygulamalarıyla uyumludur.
        * [Gremlin için Azure Cosmos DB API](graph-introduction.md): Tam olarak yönetilen, yüksek oranda çalışan uygulamaların oluşturulmasını ve çalıştırılmasını kolaylaştırır yatay olarak ölçeklenebilen graf veritabanı hizmeti bağlı açık Gremlin API'lerini destekleyen veri kümeleri (temel [Apache TinkerPop belirtimi](https://tinkerpop.apache.org/), Apache Gremlin).
        * [Tablo için Azure Cosmos DB API](table-introduction.md): Hiçbir uygulama değişikliği yapmadan mevcut Azure tablo depolama uygulamaları için premium özellikler (örneğin, otomatik dizin oluşturma, garantili düşük gecikme süresi, genel dağıtım) sağlamak için tasarlanan bir anahtar-değer veritabanı hizmeti.
        * Çok yakında başka veri modelleri ve API’ler de kullanıma sunulacaktır!

* **Aktarım hızı ve depolamayı dünyanın dört bir yanında, talep üzerine esnek ve bağımsız bir şekilde ölçeklendirin**
    * Veritabanı aktarım hızını [saniye başına](request-units.md) ayrıntı düzeyinde kolayca ölçeklendirin ve dilediğiniz zaman değiştirin. 
    * Boyut gereksinimlerinizi şimdi ve sonsuza dek karşılamak için depolama boyutunuzu [şeffaf ve otomatik bir şekilde](partition-data.md) ölçeklendirin.

* **Yüksek hızda yanıt veren ve görev açısından kritik uygulamalar oluşturun**
    * Azure Cosmos DB, müşterilerine 99. yüzdebirlik dilimde uçtan uca düşük gecikme süresi garanti etmektedir. 
    * Cosmos DB, tipik bir 1 KB'lik öğe için 10 MS'nin altında okumalar uçtan uca gecikme süresi garanti eder ve aynı Azure bölgesindeki 99. yüzdebirlik 10 MS'nin altında dizini oluşturulmuş yazmalar. Ortalama gecikme süreleri çok daha düşüktür (5 ms’nin altında).

* **"Her zaman açık" özelliği ile kullanılabilirlik endişeniz olmaz**
    * Tek tek tüm bölge veritabanı hesapları için %99,99 kullanılabilirlik SLA'sı ve çok bölgeli tüm veritabanı hesaplarında tümüyle %99,999 okunabilirlik.
    * Daha yüksek kullanılabilirlik ve daha iyi bir performans için dilediğiniz sayıda [Azure bölgesine](https://azure.microsoft.com/regions) dağıtın.
    * Bölgelere dinamik olarak öncelikler ayarlayın ve uygulamanın tamamında (yalnızca veritabanının ötesinde) uçtan uça kullanılabilirliği test etmek için sıfır veri kaybı garantisiyle bir veya birden çok bölgede [hata benzetimi yapın](high-availability.md). 

* **Genel olarak dağıtılan uygulamaları, doğru şekilde yazın**
    * İyi tanımlanmış, pratik ve sezgisel beş [tutarlılık modeli](consistency-levels.md), rahatlatılmış NoSQL benzeri nihai tutarlılığa kadar giden ve aradaki her şeyi içeren, güçlü SQL benzeri tutarlılık spektrumu sağlar. 
  
* **Para iadesi garantisi**
    * Görev açısından kritik verileriniz için kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılıkla ilgili, endüstri lideri, mali olarak desteklenen, ve kapsamlı [hizmet düzeyli sözleşmeleri](https://aka.ms/acdbsla) (SLA’lar). 

* **Veritabanı şema/dizin yönetimi yok**
    * Veritabanı şeması ve/veya dizin yönetimi konusunda kaygılanmadan uygulamanızın şemasını hızla yineleyin.
    * Azure Cosmos DB’nin veritabanı altyapısı tam olarak şemadan bağımsızdır; aldığı tüm verileri, herhangi bir şema veya dizin gerektirmeden otomatik olarak dizine alır ve ışık hızında sorgular sunar. 

* **Düşük sahip olma maliyeti**
    * Yönetilmeyen bir çözüme veya şirket içi bir NoSQL çözümüne göre beş ila on kat daha uygun maliyetli.
    * AWS DynamoDB veya Google Spanner'dan üç kat ucuz.

## <a name="capability-comparison"></a>Özellik karşılaştırması

Azure Cosmos DB, geleneksel ilişkisel ve ilişkisel olmayan veritabanlarının en iyi özelliklerini sağlar.

| Özellikler | İlişkisel veritabanları   | İlişkisel olmayan (NoSQL) veritabanları |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Genel dağıtım | Hayır | Hayır | Evet, çok girişli API'lerle 30'dan fazla bölgede anahtar teslimi dağıtım|
| Yatay ölçeklendirme | Hayır | Evet | Evet, depolama ve aktarım hızını bağımsız olarak ölçeklendirebilirsiniz | 
| Gecikme süresi garantileri | Hayır | Evet | Evet, 99 oranında okumalar < 10 ms ve yazmalar < 10 ms | 
| Yüksek kullanılabilirlik | Hayır | Evet | Evet, Azure Cosmos DB her zaman açıktır, iyi tanımlanmış PACELC dengelemeleri vardır, ayrıca otomatik ve el ile yük devretme seçenekleri sunar|
| Veri modeli + API | İlişkisel + SQL | Çoklu model + OSS API’si | Çoklu model + SQL + OSS API'si |
| SLA’lar | Evet | Hayır | Evet, gecikme süresi, aktarım hızı, tutarlılık ve kullanılabilirlik için kapsamlı SLA’lar |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çeşitli veriler için düşük yanıt süreleri içinde [küresel](distribute-data-globally.md) ölçekte muazzam miktarlarda veri okuma ve yazma işlemlerini işlemesi gereken herhangi bir [web, mobil, oyun ve IoT uygulaması](use-cases.md) Azure Cosmos DB'nin [garantili](https://azure.microsoft.com/support/legal/sla/cosmos-db/) yüksek kullanılabilirliğinden, yüksek aktarım hızından, neredeyse gerçek zamanlı yanıt süresinden ve ayarlanabilir tutarlılığından yararlanır. Azure Cosmos DB’nin [IoT ve telematik](use-cases.md#iot-and-telematics), [Perakende ve pazarlama](use-cases.md#retail-and-marketing), [Oyun](use-cases.md#gaming) ve [Web ve mobil uygulamalara](use-cases.md#web-and-mobile-applications) nasıl uygulanabileceğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB API'si ile çalışmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
