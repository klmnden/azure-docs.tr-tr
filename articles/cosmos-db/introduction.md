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
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: 600894bffe367ee1412df6a82f668143829688cc
ms.contentlocale: tr-tr
ms.lasthandoff: 07/22/2017

---

# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Azure Cosmos DB, Microsoft’un genel olarak dağıtılan çok modelli veritabanıdır. Azure Cosmos DB, bir düğmeye tıklayarak aktarım hızı ile depolamayı dilediğiniz sayıda Azure coğrafi bölgesinde esnek ve birbirinden bağımsız bir şekilde ölçeklendirmenize olanak tanır. Diğer veritabanı hizmetlerinin sunamadığı kapsamlı [hizmet düzeyi sözleşmeleri](https://aka.ms/acdbsla) (SLA) ile birlikte aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık garantileri sunar.

![Azure Cosmos DB, Microsoft'un esnek ölçek artırma, garantili düşük gecikme süresi, beş tutarlılık modeli ve kapsamlı SLA garantisi ile genel olarak dağıtılmış veritabanı hizmetidir](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çeşitli veriler için düşük yanıt süreleri içinde [küresel](distribute-data-globally.md) ölçekte muazzam miktarlarda okuma ve yazma işlemlerini işlemesi gereken tüm [web, mobil, oyun ve IoT uygulamaları](use-cases.md) Azure Cosmos DB'nin [garantili](https://azure.microsoft.com/support/legal/sla/cosmos-db/) kullanılabilirliğinden, yüksek aktarım hızından, düşük gecikme süresinden ve ayarlanabilir tutarlılığından yararlanır.

## <a name="key-capabilities"></a>Temel işlevler
Genel olarak dağıtılan bir veritabanı hizmeti olarak Azure Cosmos DB, ölçeklenebilir, son derece hızlı yanıt veren uygulamalar oluşturmanıza yardımcı olacak aşağıdaki özellikleri sağlar:

* **Anahtar teslimi genel dağıtım**
    * [Tek bir düğmeye tıklayarak](tutorial-global-distribution-documentdb.md) istediğiniz kadar çok [Azure bölgesine](https://azure.microsoft.com/regions/) [verilerinizi dağıtabilirsiniz](distribute-data-globally.md). Bu, verilerinizi kullanıcılarınızın bulunduğu yere yerleştirerek müşterileriniz için mümkün olan en düşük gecikme süresini sağlamanıza olanak tanır. 
    * Azure Cosmos DB'nin çok girişli API'lerini kullanarak, uygulama her zaman en yakın bölgenin yerini bilir ve istekleri en yakın veri merkezine gönderir. Bunların tümü yapılandırma değişikliğine gerek kalmadan yapılabilir; yazma bölgenizi ve istediğiniz kadar çok okuma bölgesini ayarlarsınız, kalan işlemler sizin yerinize yapılır.

* **Çoklu veri modelleri ve verilere erişim ile veri sorgulamaya yönelik popüler API’ler**
    * Azure Cosmos DB'nin üzerine yapılandırıldığı ve atom kayıt sırasını (ARS) temel alan veri modeli; belge, grafik, anahtar-değer, tablo ve sütunlu veri modelleri de dahil olmak üzere ancak bunlarla sınırlı kalmamak kaydıyla birçok veri modelini yerel olarak destekler.
    * Aşağıdaki veri modellerinin API'leri, birden çok dilde sağlanan SDK'larla desteklenir:
        * [DocumentDB API'si](documentdb-introduction.md)
        * [MongoDB API’si](mongodb-introduction.md)
        * [Tablo API’si](table-introduction.md)
        * [Graph (Gremlin) API'si](graph-introduction.md)
        * Çok yakında başka veri modelleri de sağlanacaktır 

* **Aktarım hızı ve depolamayı dünyanın dört bir yanında, talep üzerine esnek bir şekilde ölçeklendirin**
    * Veritabanı aktarım hızını [saniye](request-units.md) ve [dakika](request-units-per-minute.md) ayrıntı düzeylerinde kolayca ölçeklendirin ve dilediğiniz zaman değiştirin. 
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
    * Yönetilmeyen bir çözüme göre beş ila on kat [daha uygun maliyetli](https://aka.ms/cosmos-db-tco-paper).
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


## <a name="next-steps"></a>Sonraki adımlar
Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB'nin DocumentDB API’si ile çalışmaya başlama](create-documentdb-dotnet.md)
* [Azure Cosmos DB'nin MongoDB API’si ile çalışmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB'nin Grafik API’si ile çalışmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB'nin Tablo API’si ile çalışmaya başlama](create-table-dotnet.md)

