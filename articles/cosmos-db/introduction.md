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
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 804b20111ea99892201079657d6d3602ececdd28
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Azure Cosmos DB, Microsoft’un genel olarak dağıtılan çok modelli veritabanıdır. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Diğer veritabanı hizmetlerinin sunamadığı kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA) ile birlikte aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantileri sunar. Yapabilecekleriniz [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) gider ve taahhüt bir Azure aboneliği boş.

![Azure Cosmos DB ile esnek düşük gecikme süresi, beş tutarlılık modelleri ve SLA garanti kapsamlı garanti genişleme, Microsoft'un Genel dağıtılmış veritabanı hizmetidir.](./media/introduction/azure-cosmos-db.png)

> [!div class="nextstepaction"]
> [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)

## <a name="key-capabilities"></a>Temel işlevler
Genel olarak dağıtılan bir veritabanı hizmeti olarak Azure Cosmos DB, ölçeklenebilir, son derece hızlı yanıt veren uygulamalar oluşturmanıza yardımcı olacak aşağıdaki özellikleri sağlar:

* **Anahtar teslimi genel dağıtım**
    * [Tek bir düğmeye tıklayarak](tutorial-global-distribution-documentdb.md) istediğiniz kadar çok [Azure bölgesine](https://azure.microsoft.com/regions/) [verilerinizi dağıtabilirsiniz](distribute-data-globally.md). Bu, verilerinizi kullanıcılarınızın bulunduğu yere yerleştirerek müşterileriniz için mümkün olan en düşük gecikme süresini sağlamanıza olanak tanır. 
    * Azure Cosmos veritabanı kullanarak çok girişli API'leri, uygulama her zaman burada en yakın bölgeyi ve en yakın veri merkezinin istekleri gönderir bilir. Tüm olası hiçbir yapılandırma değişiklikleri ile. Write-bölgenizi ayarlamak ve aynı sayıda okuma bölgeler istediğiniz ve rest sizin için gerçekleştirilir.

* **Çoklu veri modelleri ve verilere erişim ile veri sorgulamaya yönelik popüler API’ler**
    * Azure Cosmos DB'nin üzerine yapılandırıldığı ve atom kayıt sırasını (ARS) temel alan veri modeli; belge, grafik, anahtar-değer, tablo ve sütunlu veri modelleri de dahil olmak üzere ancak bunlarla sınırlı kalmamak kaydıyla birçok veri modelini yerel olarak destekler.
    * Aşağıdaki veri modellerinin API'leri, birden çok dilde sağlanan SDK'larla desteklenir:
        * [DocumentDB API](documentdb-introduction.md): özellikleri sorgulama SQL Şeması daha az JSON veritabanı altyapısıyla.
        * [MongoDB API](mongodb-introduction.md): Cosmos DB üstünde oluşturulmuş bir MongoDB veritabanı hizmeti. Varolan MongoDB kitaplıkları, sürücüleri, Araçlar ve uygulamaları ile uyumludur.
        * [Tablo API](table-introduction.md): Azure Table depolama uygulamaları için premium özellikleri sağlamak için yerleşik bir anahtar-değer veritabanı hizmeti.
        * [(Gremlin) API grafik](graph-introduction.md): grafik veritabanı yerleşik hizmeti aşağıdaki [Apache TinkerPop belirtimi](http://tinkerpop.apache.org/).
        * Ek veri modelleri çok yakında!

* **Aktarım hızı ve depolamayı dünyanın dört bir yanında, talep üzerine esnek bir şekilde ölçeklendirin**
    * Veritabanı verimliliği en kolay ölçeklendirmenizi bir [saniyede](request-units.md) ayrıntı düzeyi ve istediğiniz zaman değiştirebilirsiniz. 
    * Boyut gereksinimlerinizi şimdi ve sonsuza dek karşılamak için depolama boyutunuzu [şeffaf ve otomatik bir şekilde](partition-data.md) ölçeklendirin.

* **Yüksek hızda yanıt veren ve görev açısından kritik uygulamalar oluşturun**
    * Azure Cosmos DB, müşterilerine 99. yüzdebirlik dilimde uçtan uca düşük gecikme süresi garanti etmektedir. 
    * Cosmos DB, tipik bir 1 KB’lik öğe için aynı Azure bölgesindeki 99. yüzdebirlik dilimde 10 ms’nin altında okumalar ve 15 ms’nin altında dizini oluşturulmuş yazmalar için uçtan uca gecikme süresi garanti eder. Ortalama gecikme süreleri çok daha düşüktür (5 ms’nin altında).

* **"Her zaman açık" özelliği ile kullanılabilirlik endişeniz olmaz**
    * Tek bir bölge içinde %99,99 kullanılabilirlik.
    * Daha yüksek kullanılabilirlik için dilediğiniz sayıda [Azure bölgesine](https://azure.microsoft.com/regions) dağıtın.
    * Sıfır veri kaybı garantisi ile bir veya daha fazla bölgenin [hata benzetimini gerçekleştirin](regional-failover.md). 

* **Genel olarak dağıtılan uygulamaları, doğru şekilde yazın**
    * Beş [tutarlılık modeli](consistency-levels.md), NoSQL benzeri nihai tutarlılığa kadar giden ve aradaki her şeyi içeren, güçlü SQL benzeri tutarlılık spektrumu sağlar. 
  
* **Para iadesi garantisi**
    * Verileriniz gitmesi gereken yere hızlı bir şekilde ulaşmazsa paranız iade edilir. 
    * Kullanılabilirlik, gecikme süresi, aktarım hızı ve tutarlılık için [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla). 

* **Veritabanı şema/dizin yönetimi yok**
    * Veritabanı şema ve dizinlerinizi uygulamanızın şemasıyla eşitleme konusunda endişelenmeyi bırakın. Bizde şema yok. 
    * Azure Cosmos DB’nin veritabanı altyapısı tam olarak şemadan bağımsızdır; aldığı tüm verileri, herhangi bir şema veya dizin gerektirmeden otomatik olarak dizine alır ve ışık hızında sorgular sunar. 

* **Düşük sahip olma maliyeti**
    * Beş ila 10 kat [daha düşük maliyetli](https://aka.ms/cosmos-db-tco-paper) yönetilmeyen bir çözümden.
    * DynamoDB’den üç kat daha ucuz.

## <a name="capability-comparison"></a>Özellik karşılaştırması

Azure Cosmos DB, ilişkisel ve ilişkisel olmayan veritabanlarının en iyi özelliklerini sağlar.

| Özellikler | İlişkisel veritabanları   | İlişkisel olmayan (NoSQL) veritabanları |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Genel dağıtım | Hayır | Hayır | Evet, çok girişli API'lerle 30'dan fazla bölgede anahtar teslimi dağıtım|
| Yatay ölçeklendirme | Hayır | Evet | Evet, depolama ve aktarım hızını bağımsız olarak ölçeklendirebilirsiniz | 
| Gecikme süresi garantileri | Hayır | Evet | Evte, %99 oranında okumalar <10 ms ve yazmalar <15 ms | 
| Yüksek kullanılabilirlik | Hayır | Evet | Evet, Cosmos DB her zaman açıktır, PACELC dengelemeleri vardır, ayrıca otomatik ve el ile yük devretme seçenekleri sağlar|
| Veri modeli + API | İlişkisel + SQL | Çoklu model + OSS API’si | Çoklu model + SQL + OSS API’si (yakında daha fazlası sunulacak) |
| SLA’lar | Evet | Hayır | Evet, gecikme süresi, aktarım hızı, tutarlılık ve kullanılabilirlik için kapsamlı SLA’lar |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çeşitli veriler için düşük yanıt süreleri içinde [küresel](distribute-data-globally.md) ölçekte muazzam miktarlarda okuma ve yazma işlemlerini işlemesi gereken tüm [web, mobil, oyun ve IoT uygulamaları](use-cases.md) Azure Cosmos DB'nin [garantili](https://azure.microsoft.com/support/legal/sla/cosmos-db/) kullanılabilirliğinden, yüksek aktarım hızından, düşük gecikme süresinden ve ayarlanabilir tutarlılığından yararlanır. Nasıl CosmosDB uygulanabilir hakkında bilgi edinin [IOT ve telematik](use-cases.md#iot-and-telematics), [perakende ve pazarlama](use-cases.md#retail-and-marketing), [oyun](use-cases.md#gaming) ve [Web ve mobil uygulamaları](use-cases.md#web-and-mobile-applications) .

## <a name="next-steps"></a>Sonraki adımlar
Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB'nin DocumentDB API’si ile çalışmaya başlama](create-documentdb-dotnet.md)
* [Azure Cosmos DB'nin MongoDB API’si ile çalışmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB'nin Grafik API’si ile çalışmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB'nin Tablo API’si ile çalışmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
