---
title: "Azure Cosmos DB’ye Giriş | Microsoft Belgeleri"
description: "Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik ve yüksek kullanılabilirlik için oluşturulmuştur."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 12/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: e8b1454583e52f2c7a38b375df259a8b66f7d24f
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB, Microsoft’un genel olarak dağıtılan çok modelli veritabanıdır. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Diğer veritabanı hizmetlerinin sunamadığı kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA) ile birlikte aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantileri sunar. [Azure Cosmos DB’yi ücretsiz olarak](https://azure.microsoft.com/try/cosmosdb/) bir Azure aboneliği olmadan, ücretsiz ve herhangi bir taahhütte bulunmadan deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB, Microsoft'un esnek ölçek genişletme, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

## <a name="key-capabilities"></a>Temel işlevler
Genel olarak dağıtılan, çok modelli bir veritabanı hizmeti olarak Azure Cosmos DB, ölçeklenebilir, küresel ölçekte son derece hızlı yanıt veren uygulamalar oluşturmayı kolaylaştırır:

* **Anahtar teslimi genel dağıtım**
    * [Tek bir düğmeye tıklayarak](tutorial-global-distribution-sql-api.md) istediğiniz kadar çok [Azure bölgesine](https://azure.microsoft.com/regions/) [verilerinizi dağıtabilirsiniz](distribute-data-globally.md). Bu, verilerinizi kullanıcılarınızın bulunduğu yere yerleştirerek müşterileriniz için mümkün olan en düşük gecikme süresini sağlamanıza olanak tanır. 
    * Azure Cosmos DB'nin çok girişli API'lerini kullanarak, uygulama her zaman en yakın bölgenin yerini bilir ve istekleri en yakın veri merkezine gönderir. Bunların tümü yapılandırma değişikliğine gerek kalmadan yapılabilir. Yazma bölgenizi ve istediğiniz kadar çok okuma bölgesini ayarlarsınız, kalan işlemler sizin yerinize yapılır.
    * Cosmos DB veritabanınıza bölgeleri ekleyip kaldırırken uygulamanızın yeniden dağıtılması gerekmez ve birden çok girişli API özelliği sayesinde üst düzeyde kullanılabilir olmaya devam eder.

* **Çoklu veri modelleri ve verilere erişim ile veri sorgulamaya yönelik popüler API’ler**
    * Azure Cosmos DB'nin üzerine yapılandırıldığı ve atom kayıt sırasını (ARS) temel alan veri modeli; belge, grafik, anahtar-değer, tablo ve sütun ailesi veri modelleri de dahil olmak üzere ancak bunlarla sınırlı kalmamak kaydıyla birçok veri modelini yerel olarak destekler.
    * Aşağıdaki veri modellerinin API'leri, birden çok dilde sağlanan SDK'larla desteklenir:
        * [SQL API](sql-api-introduction.md): Zengin SQL sorgulama özelliklerine sahip şemasız bir JSON veritabanı.
        * [MongoDB API](mongodb-introduction.md): Azure Cosmos DB platformu tarafından desteklenen üst düzeyde ölçeklenebilir bir *Hizmet olarak MongoDB*. Mevcut MongoDB kitaplıkları, sürücüleri, araçları ve uygulamalarıyla uyumludur.
        * [Cassandra API](cassandra-introduction.md): Azure Cosmos DB platformu tarafından desteklenen, genel olarak dağıtımı yapılan bir Hizmet olarak Cassandra. Mevcut [Apache Cassandra](https://cassandra.apache.org/) kitaplıkları, sürücüleri, araçları ve uygulamalarıyla uyumludur.
        * [Graph (Gremlin) API](graph-introduction.md): Open Graph API'leri desteğine sahip üst düzeyde bağlantılı veri kümeleriyle çalışan uygulamaların oluşturulmasını ve çalıştırılmasını kolaylaştıran, tümüyle yönetilen ve yatay olarak ölçeklenebilen graf veritabanı hizmeti ([Apache TinkerPop belirtimi](http://tinkerpop.apache.org/), Apache Gremlin temelinde).
        * [Tablo API](table-introduction.md): Hiçbir uygulama değişikliği yapmadan mevcut Azure Tablosu uygulamalarına üstün özellikler (örneğin, otomatik dizin oluşturma, garantili düşük gecik süresi, genel dağıtım) sağlamak için tasarlanan bir anahtar-değer veritabanı hizmeti.
        * Çok yakında başka veri modelleri de sağlanacaktır!

* **Aktarım hızı ve depolamayı dünyanın dört bir yanında, talep üzerine esnek ve bağımsız bir şekilde ölçeklendirin**
    * Veritabanı aktarım hızını [saniye başına](request-units.md) ayrıntı düzeyinde kolayca ölçeklendirin ve dilediğiniz zaman değiştirin. 
    * Boyut gereksinimlerinizi şimdi ve sonsuza dek karşılamak için depolama boyutunuzu [şeffaf ve otomatik bir şekilde](partition-data.md) ölçeklendirin.

* **Yüksek hızda yanıt veren ve görev açısından kritik uygulamalar oluşturun**
    * Azure Cosmos DB, müşterilerine 99. yüzdebirlik dilimde uçtan uca düşük gecikme süresi garanti etmektedir. 
    * Cosmos DB, tipik bir 1 KB’lik öğe için aynı Azure bölgesindeki 99. yüzdebirlik dilimde 10 ms’nin altında okumalar ve 15 ms’nin altında dizini oluşturulmuş yazmalar için uçtan uca gecikme süresi garanti eder. Ortalama gecikme süreleri çok daha düşüktür (5 ms’nin altında).

* **"Her zaman açık" özelliği ile kullanılabilirlik endişeniz olmaz**
    * Tek tek tüm bölge veritabanı hesapları için %99,99 kullanılabilirlik SLA'sı ve çok bölgeli tüm veritabanı hesaplarında tümüyle %99,999 okunabilirlik.
    * Daha yüksek kullanılabilirlik ve daha iyi bir performans için dilediğiniz sayıda [Azure bölgesine](https://azure.microsoft.com/regions) dağıtın.
    * Bölgelere dinamik olarak öncelikler ayarlayın ve uygulamanın tamamında (yalnızca veritabanının ötesinde) uçtan uça kullanılabilirliği test etmek için sıfır veri kaybı garantisiyle bir veya birden çok bölgede [hata benzetimi yapın](regional-failover.md). 

* **Genel olarak dağıtılan uygulamaları, doğru şekilde yazın**
    * İyi tanımlanmış, pratik ve sezgisel beş [tutarlılık modeli](consistency-levels.md), rahatlatılmış NoSQL benzeri nihai tutarlılığa kadar giden ve aradaki her şeyi içeren, güçlü SQL benzeri tutarlılık spektrumu sağlar. 
  
* **Para iadesi garantisi**
    * Görev açısından kritik verileriniz için kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılıkla ilgili, mali olarak desteklenen, endüstri lideri ve kapsamlı [hizmet düzeyli sözleşmeleri](https://aka.ms/acdbsla). 

* **Veritabanı şema/dizin yönetimi yok**
    * Veritabanı şeması ve/veya dizin yönetimi konusunda kaygılanmadan uygulamanızın şemasını hızla yineleyin.
    * Azure Cosmos DB’nin veritabanı altyapısı tam olarak şemadan bağımsızdır; aldığı tüm verileri, herhangi bir şema veya dizin gerektirmeden otomatik olarak dizine alır ve ışık hızında sorgular sunar. 

* **Düşük sahip olma maliyeti**
    * Yönetilmeyen bir çözüme veya şirket içi bir NoSQL çözümüne göre beş ila on kat [daha uygun maliyetli](https://aka.ms/cosmos-db-tco-paper).
    * AWS DynamoDB veya Google Spanner'dan üç kat ucuz.

## <a name="capability-comparison"></a>Özellik karşılaştırması

Azure Cosmos DB, ilişkisel ve ilişkisel olmayan veritabanlarının en iyi özelliklerini sağlar.

| Özellikler | İlişkisel veritabanları   | İlişkisel olmayan (NoSQL) veritabanları |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Genel dağıtım | Hayır | Hayır | Evet, çok girişli API'lerle 30'dan fazla bölgede anahtar teslimi dağıtım|
| Yatay ölçeklendirme | Hayır | Yes | Evet, depolama ve aktarım hızını bağımsız olarak ölçeklendirebilirsiniz | 
| Gecikme süresi garantileri | Hayır | Yes | Evte, %99 oranında okumalar <10 ms ve yazmalar <15 ms | 
| Yüksek kullanılabilirlik | Hayır | Yes | Evet, Azure Cosmos DB her zaman açıktır, iyi tanımlanmış PACELC dengelemeleri vardır, ayrıca otomatik ve el ile yük devretme seçenekleri sunar|
| Veri modeli + API | İlişkisel + SQL | Çoklu model + OSS API’si | Çoklu model + SQL + OSS API’si (yakında daha fazlası sunulacak) |
| SLA’lar | Yes | Hayır | Evet, gecikme süresi, aktarım hızı, tutarlılık ve kullanılabilirlik için kapsamlı SLA’lar |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çeşitli veriler için düşük yanıt süreleri içinde [küresel](distribute-data-globally.md) ölçekte muazzam miktarlarda veri okuma ve yazma işlemlerini işlemesi gereken herhangi bir [web, mobil, oyun ve IoT uygulaması](use-cases.md) Azure Cosmos DB'nin [garantili](https://azure.microsoft.com/support/legal/sla/cosmos-db/) yüksek kullanılabilirliğinden, yüksek aktarım hızından, neredeyse gerçek zamanlı yanıt süresinden ve ayarlanabilir tutarlılığından yararlanır. Azure Cosmos DB’nin [IoT ve telematik](use-cases.md#iot-and-telematics), [Perakende ve pazarlama](use-cases.md#retail-and-marketing), [Oyun](use-cases.md#gaming) ve [Web ve mobil uygulamalara](use-cases.md#web-and-mobile-applications) nasıl uygulanabileceğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [Azure Cosmos DB MongoDB API’yi kullanmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
