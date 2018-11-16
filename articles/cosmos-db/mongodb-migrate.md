---
title: MongoDB için Azure Cosmos DB API’si ile mongoimport ve mongorestore kullanma | Microsoft Docs
description: MongoDB hesabı için bir API’ye verileri içeri aktarmak için mongoimport ve mongorestore kullanmayı öğrenin
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 13422434e6392ec7681ec4478533c45a84f40c9a
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706985"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>Öğretici: Verilerinizi Azure Cosmos DB MongoDB API hesabına geçirme

Bu öğreticide MongoDB'de depolanan verileri Azure Cosmos DB MongoDB API'si hesabına geçirme adımları açıklanmaktadır. MongoDB’den verileri içeri aktarıyor ve bunları Azure Cosmos DB SQL API’siyle kullanmayı planlıyorsanız, verileri içeri aktarmak için [Veri Geçiş aracını](import-data.md) kullanmanız gerekir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Geçiş planlaması
> * Geçiş önkoşulları
> * mongoimport'u kullanarak veri geçirme
> * mongorestore'u kullanarak veri geçirme

Verileri MongoDB API'si hesabına geçirmeden önce örnek MongoDB verilerine sahip olduğunuzdan emin olun. Örnek MongoDB veritabanınız yoksa [MongoDB community server](https://www.mongodb.com/download-center) sürümünü indirip yükleyebilir, örnek bir veritabanı oluşturabilir ve örnek verileri mongoimport.exe veya mongorestore.exe ile karşıya yükleyebilirsiniz. 

## <a name="plan-for-migration"></a>Geçiş planlaması

1. Koleksiyonlarınızı önceden oluşturup ölçeklendirin:
        
   * Varsayılan olarak, Azure Cosmos DB yeni bir MongoDB koleksiyonun saniyede 1.000 istek birimiyle (RU/sn) sağlar. Mongoimport veya mongorestore'u kullanarak geçişi başlatmadan önce, [Azure portal](https://portal.azure.com)'dan veya MongoDB sürücülerinden ve araçlarından tüm koleksiyonlarınızı önceden oluşturun. Veri boyutu 10 GB'ın üzerindeyse uygun bir parça anahtarıyla [bölümlenmiş koleksiyon](partition-data.md) oluşturduğunuzdan emin olun. İçindeki koleksiyonlar varlık verilerini depolamak için MongoDB önerir. Azure Cosmos veritabanı düzeyinde karşılaştırılabilir boyutu ve sağlama aktarım hızının varlıklarına genel birlikte bulundurabilirsiniz.

   * Gelen [Azure portalında](https://portal.azure.com), tek bölümlü bir koleksiyon için 1000 RU/sn ve 2.500 RU/sn geçiş süresince yalnızca bir parçalı koleksiyon için koleksiyon aktarım hızınızı artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Maliyetlerden tasarruf etmek için geçişten hemen sonra aktarım hızını düşürebilirsiniz.

   * Koleksiyon düzeyinde RU/sn sağlamaya ek olarak, üst veritabanı düzeyinde bir koleksiyon kümesi için RU/sn de sağlayabilirsiniz. Bu, veritabanı ve koleksiyonların önceden oluşturulmasını ve her koleksiyon için bir parça anahtarı tanımlanmasını gerektirir.

   * En sevdiğiniz araç, sürücü veya SDK üzerinden parçalı koleksiyonlar oluşturabilirsiniz. Bu örnekte, parçalı bir koleksiyon oluşturmak için Mongo Shell kullanacağız:

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Sonuçlar:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Tek bir belge yazma için yaklaşık RU ücretini hesaplayın:

   a. MongoDB Kabuğu'ndan Azure Cosmos DB MongoDB API hesabınıza bağlanın. [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md) bölümünde yönergeleri bulabilirsiniz.
    
   b. MongoDB Kabuğu’ndan örnek belgelerinizden birini kullanarak örnek bir ekleme komutu çalıştırın:
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. ```db.runCommand({getLastRequestStatistics: 1})``` çalıştırdığınızda aşağıdaki gibi bir yanıt alırsınız:
     
      ```bash
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
      ```
        
    d. İstek ücretini not edin.
    
1. Makinenizden Azure Cosmos DB bulut hizmetine gecikmeyi belirleyin:
    
    a. Bu komutu kullanarak MongoDB Kabuğu’ndan ayrıntılı günlüğe yazmayı etkinleştirin: ```setVerboseShell(true)```
    
    b. Veritabanına karşı basit bir sorgu çalıştırma: ```db.coll.find().limit(1)```. Aşağıdaki gibi bir yanıt alırsınız:

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. Yinelenen belgeler olmadığından emin olmak için geçişten önce eklenen belgeyi kaldırın. Bu komutu kullanarak belgeleri kaldırabilirsiniz: ```db.coll.remove({})```

1. Yaklaşık *batchSize* ve *numInsertionWorkers* değerlerini hesaplayın:

    * *batchSize* için, toplam sağlanan RU’yu 3. adımdaki tek belge yazmanızda kullanılan RU’ya bölün.
    
    * Hesaplanan *batchSize* <= 24 ise, bu sayıyı *batchSize* değeriniz olarak kullanın.
    
    * Hesaplanan *batchSize* > 24 ise, *batchSize* değerini 24 olarak ayarlayın.
    
    * *numInsertionWorkers* için bu eşitliği kullanın:   *numInsertionWorkers =  (sağlanan aktarım hızı * saniye cinsinden gecikme) / (toplu iş boyutu * tek bir yazma için kullanılan RU)*.
        
    |Özellik|Değer|
    |--------|-----|
    |batchSize| 24 |
    |Sağlanan RU’lar | 10000 |
    |Gecikme süresi | 0,100 s |
    |1 belge yazma için ücretlendirilen RU | 10 RU |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RU x 0,1 s) / (24 x 10 RU) = 4,1666*

1. Geçiş komutunu çalıştırın. Veri geçirme seçenekleri aşağıdaki bölümlerde açıklanmıştır.

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Alternatif olarak mongorestore kullanabilirsiniz (tüm koleksiyonlarda aktarım hızının önceki hesaplamalarda kullanılan sayıda veya daha fazla RU olarak ayarlandığından emin olun):
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>Geçiş önkoşulları

* **Aktarım hızını artırma:** Veri geçişinizin süresi, tek bir koleksiyon veya bir koleksiyon kümesi için ayarladığınız aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. [Azure portalında](https://portal.azure.com) aktarım hızını artırma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB’de performans düzeyleri ve fiyatlandırma katmanları](performance-levels.md).

* **SSL’yi etkinleştirme:** Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. Makalenin devamındaki yordamlar mongoimport ve mongorestore için SSL’yi etkinleştirmeyi içerir.

* **Azure Cosmos DB kaynaklarını oluşturma:** Veri geçişini başlatmadan önce Azure portaldan tüm koleksiyonlarınızı oluşturun. Veritabanı düzeyinde aktarım hızına sahip olan bir Azure Cosmos DB hesabına geçiş yapıyorsanız Azure Cosmos DB koleksiyonlarını oluştururken bölüm anahtarı girdiğinizden emin olun.

## <a name="get-your-connection-string"></a>Bağlantı dizenizi alma 

1. [Azure portalında](https://portal.azure.com), sol bölmeden **Azure Cosmos DB** girdisine tıklayın.
1. **Abonelikler** bölmesinde, hesabınızın adını seçin.
1. **Bağlantı Dizesi** dikey penceresinde **Bağlantı Dizesi**’ne tıklayın.

   Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.

   ![Bağlantı Dizesi dikey penceresi](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>mongoimport'u kullanarak veri geçirme

Azure Cosmos DB hesabınıza verileri içeri aktarmak için, aşağıdaki şablonu kullanın. *Konak*, *kullanıcı adı* ve *parola* alanlarını hesabınıza özgü değerlerle doldurun.  

Şablon:

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Örnek:  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>mongorestore'u kullanarak veri geçirme

MongoDB hesabı için API’nize verileri geri yüklemek için, içeri aktarmayı yürütmek üzere aşağıdaki şablonu kullanın. *Konak*, *kullanıcı adı* ve *parola* alanlarını hesabınıza özgü değerlerle doldurun.

Şablon:

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Örnek:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki öğreticiye geçip Azure Cosmos DB kullanarak MongoDB verilerini sorgulamayı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
>[MongoDB verileri nasıl sorgulanır?](../cosmos-db/tutorial-query-mongodb.md)
