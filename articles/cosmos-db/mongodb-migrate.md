---
title: Veya mongorestore ile Azure Cosmos DB için MongoDB verilerinizi geçirme
description: Veya ve mongorestore Cosmos DB'ye veri almak için nasıl kullanılacağını öğreneceksiniz.
keywords: mongoimport, mongorestore
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB to Cosmos DB.
ms.openlocfilehash: 4cd30c7981cd6807113729292db403a80cbddef0
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793759"
---
# <a name="migrate-your-mongodb-data-to-azure-cosmos-db"></a>MongoDB verilerinizi Azure Cosmos DB'ye geçirme

 Bu öğretici, Azure Cosmos DB için MongoDB depolanan verileri geçirmek yönergeler MongoDB için Cosmos DB'nin API'sini kullanmak üzere yapılandırılmış sağlar. Mongodb'deki verileri içeri aktarın ve Azure Cosmos DB SQL API ile kullanmayı planlıyorsanız, kullanmanız gereken [veri geçiş aracı](import-data.md) verileri içeri aktarma.

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

Veya veya mongorestore geçirmeden önce tüm koleksiyonlardan önceden oluştur [Azure portalında](https://portal.azure.com) veya MongoDB sürücüleri ve araçlar. 

Gelen [Azure portalında](https://portal.azure.com), geçiş için koleksiyonları aktarım hızınızı artırın. Daha yüksek bir işleme hacmiyle oranı sınırlı tükenmesini önlersiniz ve daha kısa sürede geçirin. Maliyetlerden tasarruf etmek için geçişten hemen sonra aktarım hızını düşürebilirsiniz.

Koleksiyon düzeyinde sağlama aktarım hızına ek olarak, aktarım hızı için sağlanan aktarım hızına paylaşmak için koleksiyonları kümesi için veritabanı düzeyinde sağlayabilirsiniz. Veritabanı ve koleksiyon önceden oluşturma ve paylaşılan aktarım hızı veritabanında bir parça anahtarı için her bir koleksiyon tanımlamanız gerekir.

Tercih edilen araç, sürücü ya da SDK kullanılarak parçalı koleksiyonlar oluşturabilirsiniz. Bu örnekte, parçalı bir koleksiyon oluşturmak için Mongo Shell kullanacağız:

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

MongoDB Kabuğu'ndan, MongoDB için Cosmos DB'nin API'sini kullanmak üzere yapılandırılmış Cosmos hesabınıza bağlanın. Yönergeleri bulabilirsiniz [bir Cosmos DB MongoDB uygulamasına bağlanma](connect-mongodb-account.md).

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
    
#### <a name="determine-the-latency-from-your-machine-to-cosmos-db"></a>Cosmos DB'ye makinenizden gecikme süresini belirleme
    
Komutu ile MongoDB Kabuğu'ndan ayrıntılı günlüğe yazmayı etkinleştir `setVerboseShell(true)`.
    
Temel sorgu komutu veritabanında çalıştırın `db.coll.find().limit(1)`.

Aşağıdaki çıktı benzer bir yanıt alırsınız:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Geçiş işlemini çalıştırmadan önce yinelenen belge olmadığından emin olmak için eklenen belge kaldırın. Belgeler komutuyla kaldırabilirsiniz `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>BatchSize ve numInsertionWorkers özelliklerinin yaklaşık değerleri hesaplama

İçin **batchSize** özelliği, toplam sağlanan aktarım hızını (RU/sn) "cosmos DB, makinenizde gecikme süresini belirler." bölümünde tamamlandı olarak tek bir belge yazma için tüketilen RU miktarı tarafından Böl Hesaplanan değer 24'e eşit veya daha az ise, bu sayıyı özellik değeri olarak kullanın. Özellik değeri, hesaplanan değer 24'ten büyükse, 24'e ayarlayın.
    
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

* **Örnek veri alma**: Geçişe başlamadan önce bazı örnek veriler olduğundan emin olun. 

* **Üretilen iş hacmini artırmak**: Veri geçişinizi süresi, bir tek bir koleksiyonu veya veritabanı için sağlama aktarım hızı miktarına bağlıdır. Büyük veri geçişleri için aktarım hızını artırdığınızdan emin olun. Geçişi tamamlandıktan sonra maliyet tasarrufu için Aktarım azaltabilirsiniz. 

* **SSL'yi**:  Cosmos DB, katı güvenlik gereksinimleri ve standartları vardır. Cosmos hesabınızla etkileşim kurduğunuzda SSL'yi emin olun. Bu makalede yer alan yordamları veya ve mongorestore komutlar için SSL'yi etkinleştirecek şekilde nasıl içerir.

* **Cosmos DB kaynaklarını oluşturan**: Geçişe başlamadan önce tüm koleksiyonlarınız Azure portalından önceden oluşturun. Veritabanı düzeyinde sağlanan aktarım hızına sahip bir Cosmos hesabına geçirirseniz, koleksiyon oluştururken bir bölüm anahtarı sağlamak emin olun.

* **Bağlantı dizenizi alma**: İçinde [Azure portalında](https://portal.azure.com)seçin **Azure Cosmos DB** soldaki girişi. Altında **abonelikleri**, hesabınızın adını seçin. Altında **bağlantı dizesi**seçin **bağlantı dizesi**. Portalın sağ tarafındaki bilgileri gösterir, hesabınıza bağlanmanız gerekir:

    ![Bağlantı Dizesi bilgileri](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Veya kullanın

Verileri Cosmos hesabınızda içeri aktarmak için aşağıdaki şablonu kullanın.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Değiştirin \<your_hostname >, \<your_username >, ve \<your_password > hesabınız için belirli değerleri içeren parametre. Aşağıdaki örnekte **sampleDB** değeri olarak \<your_database >, ve **sampleColl** değeri olarak \<your_collection >:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Mongorestore kullanın

MongoDB için Cosmos DB API'si ile yapılandırılan Cosmos hesabınıza verileri geri yüklemek için alma işlemi yürütmek için aşağıdaki şablonu kullanın.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Değiştirin \<your_hostname >, \<your_username >, ve \<your_password > hesabınız için belirli değerleri içeren parametre. Aşağıdaki örnekte **./dumps/dump-2016-12-07** değeri olarak \<path_to_backup >:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu, Cosmos hesabı ve tüm ilgili kaynakları silin. Kaynak grubunu silmek için aşağıdaki adımları kullanın:

1. Cosmos hesabı oluşturduğunuz kaynak grubuna gidin.
1. **Kaynak grubunu sil**'i seçin.
1. Silin ve kaynak grubu adını onaylayın **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'nin MongoDB kullanarak veri sorgulama hakkında bilgi edinmek için sonraki öğreticiye devam edin. 

> [!div class="nextstepaction"]
> [MongoDB verilerini sorgulama](../cosmos-db/tutorial-query-mongodb.md)
