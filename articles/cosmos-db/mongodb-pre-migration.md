---
title: Geçiş öncesi adımları veri geçişler için MongoDB için Azure Cosmos DB'nin MongoDB API'si
description: Bu belge, Cosmos DB'ye Mongodb'den veri geçiş için Önkoşullar genel bir bakış sağlar.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: roaror
ms.openlocfilehash: 476a143555323bbb5058541000a5b1a26d23b71a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014421"
---
# <a name="pre-migration-steps-for-data-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Geçiş öncesi adımları veri geçişler için MongoDB için Azure Cosmos DB'nin MongoDB API'si

Mongodb'deki verilerinizi geçirmeden önce (ya da şirket içi veya Bulut (Iaas)), MongoDB için Azure Cosmos DB'nin API'sine gerekir:

1. [Azure Cosmos DB hesabı oluşturma](#create-account)
2. [İş yükleriniz için gereken aktarım hızını tahmin etmek](#estimate-throughput)
3. [Verileriniz için bir en uygun bölüm anahtarı seçin](#partitioning)
4. [Verilerinizde ayarlayabileceğiniz dizin oluşturma ilkesini anlama](#indexing)

Yukarıdaki Önkoşullar geçiş zaten tamamlanmış olup [geçirme MongoDB verilerini Azure Cosmos DB'nin MongoDB API'si için](../dms/tutorial-mongodb-cosmos-db.md) gerçek veri geçiş yönergeleri için. Aksi durumda, bu belgede bu ön koşullar işlemek için yönergeler sağlar. 

## <a id="create-account"></a> Bir Azure Cosmos DB hesabı oluşturma 

Geçişi başlatmadan önce yapmanız [Azure Cosmos DB'nin MongoDB kullanarak bir Azure Cosmos hesap oluşturma](create-mongodb-dotnet.md). 

Hesap oluşturma sırasında ayarları seçebilirsiniz [Global olarak dağıtma](distribute-data-globally.md) verilerinizi. Her ikisi de bir yazma ve okuma bölgesi, bölgelerin veren seçeneğiniz etkinleştir çok bölgeli yazma (veya birden çok ana yapılandırma) de vardır.

![Hesap oluşturma](./media/mongodb-pre-migration/account-creation.png)

## <a id="estimate-throughput"></a> Aktarım hızı gereken iş yükleriniz için tahmin etme

Kullanarak geçişi başlatmadan önce [veritabanı geçiş hizmeti (DMS)](../dms/dms-overview.md), aktarım hızı sağlamak için Azure Cosmos veritabanlarınızın ve koleksiyonlarınızın miktarını tahmin etmelidir.

Aktarım hızı üzerinde sağlanabilir:

- Koleksiyon

- Database

> [!NOTE]
> Yukarıdaki birlikte bulundurabilirsiniz, aktarım hızı sağlanmış bir veritabanındaki bazı derlemeler burada ayrılmış ve diğer aktarım hızı paylaşabiliriz. Ayrıntılar için lütfen bkz [bir veritabanı ve kapsayıcı aktarım hızı ayarlama](set-throughput.md) sayfası.
>

Veritabanı veya koleksiyon düzeyi aktarım hızı veya her ikisinin bir birleşiminde sağlamak istediğinize karar verin ilk. Genel olarak, bir adanmış aktarım hızı koleksiyon düzeyinde yapılandırmanız önerilir. Koleksiyonlar için sağlanan aktarım hızına paylaşmak için veritabanı ve veritabanı düzeyinde sağlama aktarım hızı sağlar. Paylaşılan aktarım hızı ile ancak, tek tek her bir koleksiyonda belirli bir aktarım hızı için bir garanti yoktur ve herhangi belirli bir koleksiyon üzerinde tahmin edilebilir performans elde etmezsiniz.

Ne kadar verimlilik için tek tek her koleksiyonu adanmış olması gerekir hakkında emin değilseniz, veritabanı düzeyinde aktarım hızı seçebilirsiniz. Sağlanan aktarım hızını esnek bir şekilde ölçeklendirme olanağı, Azure Cosmos veritabanı, işlem kapasitesinin bir MongoDB VM veya fiziksel sunucu için bir mantıksal eşdeğer olarak yapılandırılmış, ancak daha uygun maliyetli düşünebilirsiniz. Daha fazla bilgi için [Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızını](set-throughput.md).

Aktarım hızı ve veritabanı düzeyinde sağlarsanız, bu veritabanı içinde oluşturulan tüm koleksiyonlar bölüm/parça anahtarı ile oluşturulması gerekir. Bölümleme hakkında daha fazla bilgi için bkz: [bölümleme ve yatay ölçeklendirme Azure Cosmos DB'de](partition-data.md). Geçiş sırasında bir bölüm/parça anahtarı belirtmezseniz, Azure veritabanı geçiş hizmeti parça anahtarı alanıyla otomatik olarak doldurur. bir *_kimliği* her belge için otomatik olarak oluşturulan özniteliği.

### <a name="optimal-number-of-request-units-rus-to-provision"></a>En uygun sayıda istek birimi (RU) sağlama

Azure Cosmos DB'de aktarım hızı, önceden sağlanmış ve saniye başına istek birimleri (RU's) ölçülür. Bir VM veya şirket içi MongoDB çalıştıran iş yükleriniz varsa, RU ait olarak fiziksel kaynaklar için basit bir Özet gibi bir VM veya şirket-bir şirket içi sunucu ve örneğin, bunlar sahip kaynakları boyutu için bellek düşünebilirsiniz CPU, IOPS. 

Vm'leri veya şirket içi sunucular aksine, RU'ları dilediğiniz zaman ölçeği artırma ve azaltma kolaydır. Saniyeler içinde sağlanan RU sayısı değiştirebilirsiniz ve yalnızca belirli bir bir saatlik dönemdeki sağlama RU sayısı için faturalandırılırsınız. Daha fazla bilgi için [istek birimi olarak Azure Cosmos DB'de](request-units.md).

Gerekli RU sayısını etkileyen önemli faktörlerin şunlardır:
- **Öğesi (yani, belge) boyutu**: Bir öğe/belge boyutu arttıkça okumak veya öğe/belge de artar yazmak için harcanan RU sayısı.
- **Öğe özellik sayısı**: Varsayılarak [varsayılan dizinleme](index-overview.md) tüm özelliklerde RU sayısını, bir öğe, öğe özelliği sayısı arttıkça artırır yazmak için kullanılan. Yazma işlemleri tarafından için istek birimi tüketimini azaltabilirsiniz [dizin oluşturulmuş özellikleri sayısının sınırlanması](index-policy.md).
- **Eşzamanlı işlemlerin**: Tüketilen birimler ayrıca bağlı olarak farklı hangi CRUD işlemleri (örneğin, yazma, okuma, güncelleştirme, silme) sıklığı ve daha karmaşık sorgular yürütülürken isteyin. Kullanabileceğiniz [mongostat](https://docs.mongodb.com/manual/reference/program/mongostat/) geçerli MongoDB verilerinizi eşzamanlılık ihtiyaçlarını çıkarmak için.
- **Sorgu desenleri**: Bir sorgu karmaşıklığı, sorgu tarafından kullanılan kaç tane istek birimleri etkiler.

JSON dosyalarını kullanarak dışa aktarmak istemiyorsanız [mongoexport](https://docs.mongodb.com/manual/reference/program/mongoexport/) ve kaç yazma işlemleri, okuma, güncelleştirir ve siler meydana saniyede anlamak, kullanabileceğiniz [Azure Cosmos DB kapasite Planlayıcısı](https://www.documentdb.com/capacityplanner) tahmin etmek için ilk RU sayısını sağlamak için. Kapasite Planlayıcısı daha karmaşık sorgular maliyet faktörü değil. Bu nedenle, verileriniz üzerinde karmaşık sorgular varsa, ek RU tüketilir. Hesaplayıcı, ayrıca tüm alanlar dizinlenir ve oturum tutarlılığı kullanılan varsayar. Verilerinize (veya örnek veriler), Azure Cosmos DB'ye geçirmek için sorgular maliyeti olduğunu anlamak için en iyi yolu [Cosmos DB'nin uç noktasını bağlamak](connect-mongodb-account.md) ve örnek sorgu kullanarak MongoDB Kabuğu'ndan çalıştırma `getLastRequestStastistics` almak için komutu İstek yükü, tüketilen RU sayısını çıkarır:

`db.runCommand({getLastRequestStatistics: 1})`

Bu komut aşağıdakine benzer bir JSON belgesi çıkarır:

```{  "_t": "GetRequestStatisticsResponse",  "ok": 1,  "CommandName": "find",  "RequestCharge": 10.1,  "RequestDurationInMilliSeconds": 7.2}```

Bir sorgu tarafından tüketilen RU sayısını anlamak ve bu sorgu için eşzamanlılık gerekiyor sonra sağlanan RU sayısı ayarlayabilirsiniz. Bir kerelik RU en iyi duruma getirme değil; sürekli olarak iyileştirin veya gerekir, değil, ağır bir iş yükü aksine yoğun trafik bekleniyor mı veri alma bağlı olarak RU ölçeğini.

## <a id="partitioning"></a>Bölüm anahtarı seçin
Bölümleme gibi Azure Cosmos DB Global olarak dağıtılmış bir veritabanına geçirmeden önce göz önünde bulundurarak, önemli bir noktadır. Azure Cosmos DB, uygulamanızın ölçeklenebilirlik ve performans gereksinimlerini karşılamak için bir veritabanında tek tek kapsayıcı ölçeklendirme işleminden bölümleme kullanır. Kapsayıcı öğeler Bölümlemede, mantıksal bölümler adlı ayrı altkümelere, ayrılır. Ayrıntılar ve verileriniz için doğru bölüm anahtarı seçmeyi öneriler için lütfen bkz [bir bölüm anahtarı bölüm seçme](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview#choose-partitionkey). 

## <a id="indexing"></a>Verilerinizi dizin
Varsayılan olarak, Azure Cosmos DB, tüm veri alanlarını alımdan sonra dizinler. Değiştirebileceğiniz [dizin oluşturma ilkesi](index-policy.md) dilediğiniz zaman Azure Cosmos DB'de. Aslında, genellikle veri geçişi sırasında dizin oluşturmayı kapatmak ve geri zaman verileri Cosmos DB'de zaten üzerinde etkinleştirmek için önerilir. Daha fazla bilgi dizin oluşturma hakkında daha fazla ayrıntı için bilgi [Azure Cosmos DB'de dizinleme](index-overview.md) bölümü. 

[Azure veritabanı geçiş hizmeti](../dms/tutorial-mongodb-cosmos-db.md) benzersiz dizinler MongoDB koleksiyonlarla otomatik olarak geçirir. Ancak geçiş işleminden önce benzersiz dizinler oluşturulmalıdır. Zaten mevcutsa veri koleksiyonları'nda azure Cosmos DB benzersiz dizinler oluşturulmasını desteklemiyor. Daha fazla bilgi için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Veritabanı geçiş hizmetini kullanarak, Cosmos DB için MongoDB verilerinizi geçirin.](../dms/tutorial-mongodb-cosmos-db.md) 
* [Azure Cosmos kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* [Azure Cosmos DB'de bölümleme](partition-data.md)
* [Azure cosmos DB genel dağıtım](distribute-data-globally.md)
* [Azure Cosmos DB'yi dizine ekleme](index-overview.md)
* [İstek birimleri Azure cosmos DB](request-units.md)
