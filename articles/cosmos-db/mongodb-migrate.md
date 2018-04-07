---
title: Azure Cosmos DB API'si ile mongoimport ve mongorestore MongoDB için kullanma | Microsoft Docs
description: Mongoimport ve mongorestore bir API MongoDB hesabı için veri almak için nasıl kullanılacağını öğrenin
keywords: mongoimport, mongorestore
services: cosmos-db
author: AndrewHoh
manager: kfile
documentationcenter: ''
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 5c87483e384a09591aca496292638d7b68476beb
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: Alma MongoDB veri 

MongoDB’deki verileri, MongoDB’ye yönelik bir API ile kullanılacak bir Azure Cosmos DB hesabına geçirmek için şunları yapmalısınız:

* Ya da indirme *mongoimport.exe* veya *mongorestore.exe* gelen [MongoDB Yükleme Merkezi'nden](https://www.mongodb.com/download-center).
* [MongoDB için API bağlantı dizenizi](connect-mongodb-account.md) alın.

Veri adresinden aldığınız ve Azure Cosmos DB ile kullanmayı planlıyorsanız, kullanmanız gereken [veri geçiş aracı](import-data.md) veri almak için.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bağlantı dizesi alınıyor
> * Mongoimport kullanarak MongoDB veri alma
> * Mongorestore kullanarak MongoDB veri alma

## <a name="prerequisites"></a>Önkoşullar

* Verimliliğini artırmak: veri geçişinizi süresini ayarladığınız Koleksiyonlarınız için işleme miktarına bağlıdır. Büyük veri geçişler verimliliğini artırmak emin olun. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak verimliliği azaltır. Performansı artırma hakkında daha fazla bilgi için [Azure portal](https://portal.azure.com), bkz: [performans düzeyleri ve Azure Cosmos veritabanı fiyatlandırma katmanlarına](performance-levels.md).

* SSL'yi etkinleştirin: Azure Cosmos DB sıkı güvenlik gereksinimlerine ve standartları vardır. Hesabınız ile etkileşim kurarken SSL'yi emin olun. Makalenin kalanında yordamlarda mongoimport ve mongorestore için SSL etkinleştirmek nasıl içerir.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>(Konak, bağlantı noktası, kullanıcı adı ve parola) bağlantı dizesi bilgilerinizi Bul

1. İçinde [Azure portal](https://portal.azure.com), sol bölmede **Azure Cosmos DB** girişi.
2. İçinde **abonelikleri** bölmesinde, hesabınızın adını seçin.
3. İçinde **bağlantı dizesi** dikey penceresinde tıklatın **bağlantı dizesi**.  
Sağ bölmede başarıyla hesabınıza bağlanmak için gereken tüm bilgileri içerir.

    ![Bağlantı dizesi dikey penceresi](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a>Mongoimport kullanarak API MongoDB için Veri Al

Azure Cosmos DB hesabınıza veri almak için aşağıdaki şablonu kullanın. Doldurmak *konak*, *kullanıcıadı*, ve *parola* hesabınıza özgü değerlerle.  

Şablonu:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Örnek:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a>Mongorestore kullanarak API MongoDB için Veri Al

Apı'nize MongoDB hesabı için verileri geri yüklemek için alma işlemi yürütmek için aşağıdaki şablonu kullanın. Doldurmak *konak*, *kullanıcıadı*, ve *parola* hesabınıza özgü değerlerle.

Şablonu:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Örnek:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Başarılı bir geçiş Kılavuzu

1. Önceden oluşturma ve koleksiyonlarınızı ölçeği:
        
    * Varsayılan olarak, Azure Cosmos DB 1.000 istek birimleri (RUs) ile yeni bir MongoDB koleksiyon sağlar. Mongoimport, mongorestore veya mongomirror kullanarak Geçişe başlamadan önce tüm koleksiyonlardan önceden oluştur [Azure portal](https://portal.azure.com) veya MongoDB sürücüleri ve araçları. Koleksiyonunuz 10 GB'den büyükse oluşturduğunuzdan emin olun bir [parçalı/bölümlenmiş koleksiyonu](partition-data.md) uygun parça anahtara sahip.

    * Gelen [Azure portal](https://portal.azure.com), tek bölümlü bir koleksiyon için 1.000 RUs ve geçiş için yalnızca parçalı bir koleksiyon için 2.500 RUs koleksiyonlarınızı verimliliğini artırmak. Daha yüksek işleme ile azaltma önlemek ve daha kısa sürede geçirilir. Azure Cosmos DB'de saatlik faturalandırma ile maliyet tasarrufu sağlamak hemen geçişten sonra işleme azaltabilir.

2. Tek belge yazmak için yaklaşık RU ücret Hesapla:

    a. Azure Cosmos DB MongoDB veritabanınızı MongoDB Kabuğu'ndan bağlayın. ' Ndaki yönergeleri bulabilirsiniz [Azure Cosmos DB MongoDB uygulamaya bağlama](connect-mongodb-account.md).
    
    b. Örnek belgelerinizi MongoDB Kabuğu'ndan birini kullanarak bir örnek INSERT komutu çalıştırın:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Çalıştırma ```db.runCommand({getLastRequestStatistics: 1})``` ve bunun gibi bir yanıtı alırsınız:
     
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
        
    d. İstek ücret not edin.
    
3. Makinenizden gecikme Azure Cosmos DB bulut hizmetine belirleyin:
    
    a. Bu komutu kullanarak MongoDB Kabuğu'ndan ayrıntılı günlük kaydını etkinleştir: ```setVerboseShell(true)```
    
    b. Basit bir sorgu veritabanına karşı çalışırlar: ```db.coll.find().limit(1)```. Bunun gibi bir yanıtı alırsınız:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Yinelenen belge yok olduğundan emin olmak için geçiş işleminden önce eklenen belge kaldırın. Bu komutu kullanarak belgeler kaldırabilirsiniz: ```db.coll.remove({})```

5. Yaklaşık hesaplamak *batchSize* ve *numInsertionWorkers* değerler:

    * İçin *batchSize*, toplam RUs adım 3'te, tek bir belge yazma gelen tüketilen RUs tarafından sağlanan bölün.
    
    * Varsa hesaplanan *batchSize* < = 24, bu sayı olarak kullanmak, *batchSize* değeri.
    
    * Varsa hesaplanan *batchSize* > 24, ayarlamak *batchSize* 24 değeri.
    
    * İçin *numInsertionWorkers*, bu denklemi kullanın: *numInsertionWorkers = (sağlanan işleme * gecikme süresini saniye cinsinden) / (yığın boyutu * RUs için tek bir yazma tüketilen)*.
        
    |Özellik|Değer|
    |--------|-----|
    |batchSize| 24 |
    |Sağlanan RUs | 10000 |
    |Gecikme süresi | 0.100 s |
    |RU 1 belge yazmak için sizden ücret | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (0,1 x 10000 RUs s) / (24 x 10 RUs) 4.1666 =*

6. Son geçiş komutu çalıştırın:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğretici devam ve Azure Cosmos DB kullanarak MongoDB veri sorgulama öğrenin. 

> [!div class="nextstepaction"]
>[MongoDB verileri sorgulamak nasıl?](../cosmos-db/tutorial-query-mongodb.md)
