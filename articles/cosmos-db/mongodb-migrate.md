---
title: 'Öğretici: MongoDB için Azure Cosmos DB API’si ile mongoimport ve mongorestore kullanma | Microsoft Docs'
description: Bu öğreticide, veya ve mongorestore MongoDB hesabı için bir API veri almak için nasıl kullanılacağını öğrenin.
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: sngun
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB workloads to a MongoDB API account in Azure Cosmos DB, so that the overhead to manage resources is handled by Azure Cosmos DB.
ms.openlocfilehash: b40ca7cf62127a95fe277e205e5a5c5d36618828
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753855"
---
# <a name="tutorial-migrate-your-data-to-a-mongodb-api-account-in-azure-cosmos-db"></a>Öğretici: Bir Azure Cosmos DB MongoDB API hesabı, verilerinizi geçirme

Bir geliştirici olarak, NoSQL belge verileri kullanan uygulamalar olabilir. Azure Cosmos DB MongoDB API hesabı, depolamak ve bu belge verilere erişmek için kullanabilirsiniz. MongoDB API'si için var olan uygulamalarınızdan verileri de geçirebilirsiniz. Bu öğretici, önceden bir Azure Cosmos DB MongoDB API hesabı için depolanan verileri geçirme hakkında yönergeler açıklanmaktadır. Mongodb'deki verileri içeri aktarın ve Azure Cosmos DB SQL API ile kullanmayı planlıyorsanız, kullanmanız gereken [veri geçiş aracı](import-data.md) verileri içeri aktarma.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Bir geçiş planı hazırlayın.
> * Veya'ı kullanarak verileri geçirin.
> * Verileri mongorestore kullanarak geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Gözden geçirin ve geçiş işlemini başlatmadan önce aşağıdaki önkoşulları tamamlayın.

### <a name="plan-for-the-migration"></a>Geçiş planlaması

Bu bölümde, veri geçişini planlama açıklar. Biz RU ücretleri tahmin, bulut hizmetine makinenizden gecikme süresini belirleme ve toplu iş boyutu ve ekleme çalışan sayısını hesaplamak.


#### <a name="pre-create-and-scale-your-collections"></a>Önceden oluşturma ve koleksiyonlarınız ölçeklendirin

Varsayılan olarak, Azure Cosmos DB yeni bir MongoDB koleksiyonun saniyede 1.000 istek birimiyle (RU/sn) sağlar. Veya veya mongorestore geçirmeden önce tüm koleksiyonlardan önceden oluştur [Azure portalında](https://portal.azure.com) veya MongoDB sürücüleri ve araçlar. Veri boyutu 10 GB'den fazla ise, bölünmüş bir koleksiyona bir uygun parça anahtarı ile oluşturun.

Gelen [Azure portalında](https://portal.azure.com), tek bölümlü bir koleksiyon için 1.000 RU/sn ve 2.500 RU/sn parçalı koleksiyon için geçiş için koleksiyonları aktarım hızınızı artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Maliyetlerden tasarruf etmek için geçişten hemen sonra aktarım hızını düşürebilirsiniz.

Sağlama RU/sn ek olarak koleksiyon düzeyinde, ana veritabanı düzeyinde koleksiyonları kümesi için RU/sn sağlayabilirsiniz. Üst düzeyde veritabanı ve koleksiyon önceden oluşturma ve her bir koleksiyon için bir parça anahtarı tanımlayın.

Parçalı koleksiyonlar, tercih edilen araç, sürücü veya SDK kullanarak oluşturabilirsiniz. Bu örnekte, parçalı bir koleksiyon oluşturmak için Mongo Shell kullanacağız:

```bash
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Komutu aşağıdaki sonuçları verir:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

#### <a name="calculate-the-approximate-ru-charge-for-a-single-document-write"></a>Bir tek belge yazma için yaklaşık RU Ücret hesaplama

Azure Cosmos DB MongoDB API hesabınızda MongoDB Kabuğu'ndan bağlanın. [Azure Cosmos DB’ye MongoDB uygulaması bağlama](connect-mongodb-account.md) bölümünde yönergeleri bulabilirsiniz.

Ardından, örnek belgelerinizi birini kullanarak örnek bir Ekle komutu çalıştırın:
   
```bash
db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
```
        
Komutunu çalıştırın `db.runCommand({getLastRequestStatistics: 1})`.

Aşağıdaki çıktı benzer bir yanıt alırsınız:
     
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
        
İstek ücretini not edin.
    
#### <a name="determine-the-latency-from-your-machine-to-the-azure-cosmos-db-cloud-service"></a>Azure Cosmos DB bulut hizmeti için makinenizden gecikme süresini belirleme
    
Komutu ile MongoDB Kabuğu'ndan ayrıntılı günlüğe yazmayı etkinleştir `setVerboseShell(true)`.
    
Temel sorgu komutu veritabanında çalıştırın `db.coll.find().limit(1)`.

Aşağıdaki çıktı benzer bir yanıt alırsınız:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Geçiş işlemini çalıştırmadan önce yinelenen belge olmadığından emin olmak için eklenen belge kaldırın. Belgeler komutuyla kaldırabilirsiniz `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>BatchSize ve numInsertionWorkers özelliklerinin yaklaşık değerleri hesaplama

İçin **batchSize** özelliği, toplam RU, tek bir belge yazma izninden "makinenizden Azure Cosmos DB bulut hizmeti gecikme süresini belirler." bölümünde tamamlandı olarak tüketilen RU miktarı tarafından sağlanan Böl Hesaplanan değer 24'e eşit veya daha az ise, bu sayıyı özellik değeri olarak kullanın. Özellik değeri, hesaplanan değer 24'ten büyükse, 24'e ayarlayın.
    
Değeri için **numInsertionWorkers** özelliği, bu eşitlik kullanın:

`numInsertionWorkers = (Provisioned RUs throughput * Latency in seconds) / (batchSize * Consumed RUs for a single write)`

Aşağıdaki değerleri için bir değer hesaplamak için kullanabiliriz **numInsertionWorkers** özelliği:

| Özellik | Değer |
|--------|-----|
| **batchSize** | 24 |
| Sağlanan ru | 10,000 |
| Gecikme süresi | 0,100 s |
| Tüketilen ru | 10 RU |
| **numInsertionWorkers** | (0.100 x 10.000 Ru'ya s) / (24 x 10 RU) = **4.1666** |

Çalıştırma **monogoimport** geçiş komutu. Komut parametreleri, bu makalenin sonraki bölümlerinde açıklanmıştır.

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

Ayrıca **monogorestore** komutu. Tüm koleksiyonlar aktarım hızı veya önceki hesaplamalarda kullanılan RU sayısı üzerindeki kümesi olduğundan emin olun.
   
```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
```

### <a name="complete-the-prerequisites"></a>Önkoşulları tamamlayın

Geçiş için planladıktan sonra aşağıdaki adımları tamamlayın: 

* **Örnek veri alma**: Geçişe başlamadan önce bazı örnek MongoDB verilerini olduğundan emin olun. Örnek MongoDB veritabanınız yoksa [MongoDB community server](https://www.mongodb.com/download-center) sürümünü indirip yükleyebilir, örnek bir veritabanı oluşturabilir ve örnek verileri mongoimport.exe veya mongorestore.exe ile karşıya yükleyebilirsiniz. 

* **Üretilen iş hacmini artırmak**: Veri geçiş süresi, aktarım hızı, tek bir koleksiyon için ayarlama miktarı veya koleksiyonları kümesi göre değişir. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamlandıktan sonra maliyet tasarrufu için Aktarım azaltabilirsiniz. 

* **SSL'yi**: Azure Cosmos DB, katı güvenlik gereksinimleri ve standartları vardır. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. Bu makalede yer alan yordamları veya ve mongorestore komutlar için SSL'yi etkinleştirecek şekilde nasıl içerir.

* **Azure Cosmos DB kaynaklarını oluşturan**: Geçişe başlamadan önce tüm koleksiyonlarınız Azure portalından önceden oluşturun. Veritabanı düzeyinde aktarım hızına sahip bir Azure Cosmos DB hesabına geçirirseniz, Azure Cosmos DB koleksiyonları oluştururken bir bölüm anahtarı sağlamak emin olun.

* **Bağlantı dizenizi alma**: İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Cosmos DB** soldaki girişi. Altında **abonelikleri**, hesabınızın adını seçin. Altında **bağlantı dizesi**seçin **bağlantı dizesi**. Portalın sağ tarafındaki bilgileri gösterir, hesabınıza bağlanmanız gerekir:

    ![Bağlantı Dizesi bilgileri](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Veya kullanın

Azure Cosmos DB hesabınıza verileri içeri aktarmak için, aşağıdaki şablonu kullanın.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Değiştirin \<your_hostname >, \<your_username >, ve \<your_password > hesabınız için belirli değerleri içeren parametre. Aşağıdaki örnekte **sampleDB** değeri olarak \<your_database >, ve **sampleColl** değeri olarak \<your_collection >:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Mongorestore kullanın

MongoDB hesabı için API’nize verileri geri yüklemek için, içeri aktarmayı yürütmek üzere aşağıdaki şablonu kullanın.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Değiştirin \<your_hostname >, \<your_username >, ve \<your_password > hesabınız için belirli değerleri içeren parametre. Aşağıdaki örnekte **./dumps/dump-2016-12-07** değeri olarak \<path_to_backup >:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklara artık ihtiyacınız olmadığında, kaynak grubu, Azure Cosmos DB hesabı ve tüm ilgili kaynakları silin. Kaynak grubunu silmek için aşağıdaki adımları kullanın:

1. Azure Cosmos DB hesabı oluşturduğunuz kaynak grubuna gidin.
1. **Kaynak grubunu sil**'i seçin.
1. Silin ve kaynak grubu adını onaylayın **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

MongoDB verilerini Azure Cosmos DB kullanarak sorgulama hakkında bilgi edinmek için sonraki öğreticiye devam edin. 

> [!div class="nextstepaction"]
> [MongoDB verilerini sorgulama](../cosmos-db/tutorial-query-mongodb.md)
