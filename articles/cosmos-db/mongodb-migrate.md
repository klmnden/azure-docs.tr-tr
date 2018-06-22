---
title: MongoDB için Azure Cosmos DB API’si ile mongoimport ve mongorestore kullanma | Microsoft Docs
description: MongoDB hesabı için bir API’ye verileri içeri aktarmak için mongoimport ve mongorestore kullanmayı öğrenin
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: cacda277082f62c9d98a7459cb5dbf74375bfd87
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795355"
---
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: MongoDB verilerini içeri aktarma 

MongoDB’deki verileri, MongoDB’ye yönelik bir API ile kullanılacak bir Azure Cosmos DB hesabına geçirmek için şunları yapmalısınız:

* [MongoDB İndirme Merkezi](https://www.mongodb.com/download-center)’nden topluluk sürücüsünü indirip yükleyin.
* "Yükleme klasörü/bin" dizinindeki mongoimport.exe veya mongorestore.exe dosyasını kullanın. 
* [MongoDB için API bağlantı dizenizi](connect-mongodb-account.md) alın.

MongoDB’den verileri içeri aktarıyor ve bunları Azure Cosmos DB SQL API’siyle kullanmayı planlıyorsanız, verileri içeri aktarmak için [Veri Geçiş aracını](import-data.md) kullanmanız gerekir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bağlantı dizenizi alma
> * Mongoimport kullanarak MongoDB verilerini içeri aktarma
> * Mongorestore kullanarak MongoDB verilerini içeri aktarma

## <a name="prerequisites"></a>Ön koşullar

* Aktarım hızını artırma: Veri geçişinizin süresi, tek bir koleksiyon veya bir koleksiyon kümesi için ayarladığınız aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. [Azure portalında](https://portal.azure.com) aktarım hızını artırma hakkında daha fazla bilgi için bkz. [Azure Cosmos DB’de performans düzeyleri ve fiyatlandırma katmanları](performance-levels.md).

* SSL’yi etkinleştir: Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. Makalenin devamındaki yordamlar mongoimport ve mongorestore için SSL’yi etkinleştirmeyi içerir.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Bağlantı dizesi bilgilerinizi bulun (konak, bağlantı noktası, kullanıcı adı ve parola)

1. [Azure portalında](https://portal.azure.com), sol bölmeden **Azure Cosmos DB** girdisine tıklayın.
2. **Abonelikler** bölmesinde, hesabınızın adını seçin.
3. **Bağlantı Dizesi** dikey penceresinde **Bağlantı Dizesi**’ne tıklayın.

   Sağ bölme, hesabınıza başarıyla bağlanmak için gereken tüm bilgileri içerir.

   ![Bağlantı Dizesi dikey penceresi](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a>Mongoimport kullanarak verileri MongoDB için API’ye aktarma

Azure Cosmos DB hesabınıza verileri içeri aktarmak için, aşağıdaki şablonu kullanın. *Konak*, *kullanıcı adı* ve *parola* alanlarını hesabınıza özgü değerlerle doldurun.  

Şablon:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Örnek:  

    mongoimport.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\admin\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a>Mongorestore kullanarak verileri MongoDB için API’ye aktarma

MongoDB hesabı için API’nize verileri geri yüklemek için, içeri aktarmayı yürütmek üzere aşağıdaki şablonu kullanın. *Konak*, *kullanıcı adı* ve *parola* alanlarını hesabınıza özgü değerlerle doldurun.

Şablon:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Örnek:

    mongorestore.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Başarılı geçiş kılavuzu

1. Koleksiyonlarınızı önceden oluşturup ölçeklendirin:
        
    * Varsayılan olarak, Azure Cosmos DB yeni bir MongoDB koleksiyonun saniyede 1.000 istek birimiyle (RU/sn) sağlar. Mongoimport, mongorestore veya mongomirror kullanarak geçişe başlamadan önce, [Azure portaldan](https://portal.azure.com) veya MongoDB sürücüleri ve araçlarından tüm koleksiyonlarınızı önceden oluşturun. Koleksiyonunuz 10 GB’den büyükse, uygun parça anahtarına sahip bir [parçalı/bölümlenmiş koleksiyon](partition-data.md) oluşturduğunuzdan emin olun.

    * [Azure portalından](https://portal.azure.com), yalnızca geçiş için koleksiyonunuzun aktarım hızını tek bir bölüm için 1.000 RU/sn’den parçalı koleksiyon için 2.500 RU/sn’ye artırın. Daha yüksek aktarım hızı ile, azaltmayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Azure Cosmos DB’de saatlik faturalandırma ile, maliyet tasarrufu sağlamak için geçiş işleminden hemen sonra aktarım hızını azaltabilirsiniz.

    * Koleksiyon düzeyinde RU/sn sağlamaya ek olarak, üst veritabanı düzeyinde bir koleksiyon kümesi için RU/sn de sağlayabilirsiniz. Bu, veritabanı ve koleksiyonların önceden oluşturulmasını ve her koleksiyon için bir parça anahtarı tanımlanmasını gerektirir.

    * En sevdiğiniz araç, sürücü veya SDK üzerinden parçalı koleksiyonlar oluşturabilirsiniz. Bu örnekte, parçalı bir koleksiyon oluşturmak için Mongo Shell kullanacağız:

        ```
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

2. Tek bir belge yazma için yaklaşık RU ücretini hesaplayın:

    a. MongoDB Kabuğu’ndan Azure Cosmos DB MongoDB veritabanınıza bağlanın. [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md) bölümünde yönergeleri bulabilirsiniz.
    
    b. MongoDB Kabuğu’ndan örnek belgelerinizden birini kullanarak örnek bir ekleme komutu çalıştırın:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. ```db.runCommand({getLastRequestStatistics: 1})``` çalıştırdığınızda aşağıdaki gibi bir yanıt alırsınız:
     
        ```
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
    
3. Makinenizden Azure Cosmos DB bulut hizmetine gecikmeyi belirleyin:
    
    a. Bu komutu kullanarak MongoDB Kabuğu’ndan ayrıntılı günlüğe yazmayı etkinleştirin: ```setVerboseShell(true)```
    
    b. Veritabanına karşı basit bir sorgu çalıştırma: ```db.coll.find().limit(1)```. Aşağıdaki gibi bir yanıt alırsınız:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Yinelenen belgeler olmadığından emin olmak için geçişten önce eklenen belgeyi kaldırın. Bu komutu kullanarak belgeleri kaldırabilirsiniz: ```db.coll.remove({})```

5. Yaklaşık *batchSize* ve *numInsertionWorkers* değerlerini hesaplayın:

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

6. Son geçiş komutunu çalıştırın:

   ```
   mongoimport.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Alternatif olarak mongorestore kullanabilirsiniz (tüm koleksiyonlarda aktarım hızının önceki hesaplamalarda kullanılan miktarda veya daha fazla RU olarak ayarlandığından emin olun):
   
   ```
   mongorestore.exe --host comsosdb-mongodb-account.documents.azure.com:10255 -u comsosdb-mongodb-account -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki öğreticiye geçip Azure Cosmos DB kullanarak MongoDB verilerini sorgulamayı öğrenebilirsiniz. 

> [!div class="nextstepaction"]
>[MongoDB verileri nasıl sorgulanır?](../cosmos-db/tutorial-query-mongodb.md)
