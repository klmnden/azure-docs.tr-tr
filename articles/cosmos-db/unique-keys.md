---
title: Azure Cosmos veritabanı benzersiz anahtarlar | Microsoft Docs
description: Azure Cosmos DB veritabanınızda benzersiz anahtarları kullanmayı öğrenin.
services: cosmos-db
keywords: benzersiz anahtar kısıtlaması, benzersiz anahtar kısıtlaması ihlali
author: rafats
manager: kfile
editor: monicar
documentationcenter: ''
ms.assetid: b15d5041-22dd-491e-a8d5-a3d18fa6517d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: rafats
ms.openlocfilehash: a00e61cc30f9e500df6c2c4336f0ed78d9988f68
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Azure Cosmos veritabanı benzersiz anahtar

Benzersiz anahtarlar geliştiricilerin kendi veri bütünlüğü katmanı eklemek için olanak sağlar. Bir kapsayıcı oluşturulduğunda, benzersiz bir anahtar ilke oluşturarak, bir veya daha fazla değer benzersizliğini olun [bölüm anahtarı](partition-data.md). Bir kapsayıcı benzersiz bir anahtar ilkesiyle oluşturulduktan sonra tüm yeni veya güncelleştirilmiş öğeleri yinelenen değerleri benzersiz anahtar kısıtlaması tarafından belirtilen değerlerle oluşturulmasını engeller.   

> [!NOTE]
> Benzersiz anahtarlar en son sürümleri tarafından desteklenir [.NET](sql-api-sdk-dotnet.md) ve [.NET Core](sql-api-sdk-dotnet-core.md) SQL SDK'ları ve [MongoDB API](mongodb-feature-support.md#unique-indexes). Tablo API ve grafik API'si benzersiz anahtarları şu anda desteklemiyor. 
> 
>

## <a name="use-case"></a>Kullanım örneği

Örnek olarak, nasıl bir kullanıcı veritabanı ile ilişkili en göz atalım bir [sosyal uygulama](use-cases.md#web-and-mobile-applications) e-posta adreslerini benzersiz bir anahtar ilke kalmaktan faydalanabilir. Yaparak kullanıcının e-posta adresi benzersiz bir anahtar, her kayıt benzersiz e-posta adresi olduğundan ve hiçbir yeni kayıtlar yinelenen e-posta adresleriyle oluşturulabilir emin olun. 

Kullanıcıların oluşturmak istiyorsanız birden fazla kayıt aynı aynı ad, Soyadı adresini, e-posta ve e-posta adresi, benzersiz anahtar ilkesi diğer yolları ekleyebilirsiniz. Bu nedenle yalnızca bir e-posta adresine göre benzersiz bir anahtar oluşturmak yerine ilk adını, soyadını ve e-posta birleşimi benzersiz bir anahtar oluşturabilirsiniz. Bu durumda, her üç yolu benzersiz bir birleşimi izin verilir, böylece veritabanı aşağıdaki yolu değerleri olan öğeler içerebilir. Bu kayıtların her birinde, benzersiz anahtar ilkesi geçip geçmeyeceğini.  

**İzin verilen değerler firstName, lastName ve e-posta benzersiz anahtar**

|Ad|Soyadı|E-posta adresi|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Çalışan Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Yukarıdaki tabloda listelenen birleşimlerinden herhangi biri ile başka bir kayıt ekleme girişiminde bulunuldu, benzersiz anahtar kısıtlaması karşılanmadığı belirten bir hata alırsınız. "Belirtilen kimliğe veya ada sahip kaynak zaten var." Azure Cosmos DB döndürür hata. veya "Kaynak belirtilen kimliği, ad veya benzersiz bir dizin zaten var." 

## <a name="using-unique-keys"></a>Benzersiz anahtar kullanma

Benzersiz anahtar kapsayıcı oluşturulduğunda ve benzersiz bir anahtar bölüm anahtarı kapsamlıdır tanımlanması gerekir. Önceki örnekte olduğu temel alınarak posta kodu bölerseniz oluşturmak için her bölüm yinelenen tablodaki kayıtların olabilir.

Varolan kapsayıcıları benzersiz anahtarları kullanmak için güncelleştirilemiyor.

Bir kapsayıcı benzersiz bir anahtar ilkesiyle oluşturulduktan sonra ilkeyi kapsayıcı yeniden sürece değiştirilemez. Üzerinde benzersiz anahtarlar uygulamak istediğiniz var olan verileri varsa, yeni kapsayıcı oluşturun ve yeni kapsayıcı verileri taşımak için uygun veri geçiş aracı kullanın. SQL kapsayıcılar için kullanma [veri geçiş aracı](import-data.md). MongoDB kapsayıcılar için kullanma [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md).

Her benzersiz anahtarında en fazla 16 yol değerleri (örneğin /firstName, /lastName, /address/zipCode, vb.) dahil edilebilir. 

Her benzersiz bir anahtar ilke en fazla 10 benzersiz anahtar kısıtlamalarını olabilir veya birleşimleri ve tüm benzersiz dizin özelliklerinin birleşik yolları 60 karakteri aşamaz. Böylece kullanan önceki örnek adı, Soyadı, e-posta adresi yalnızca bir kısıtlamadır ve üç kullanılabilir 16 olası yolları kullanır. 

Birim oluşturmak için güncelleştirme, ücretler ve öğe silme biraz daha yüksek kapsayıcıya ilişkin benzersiz bir anahtar ilke olduğunda isteyin. 

Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yolları için değerler eksikse, bölümü benzersizlik kısıtlamasını alır özel bir null değer olarak kabul edilir.

## <a name="sql-api-sample"></a>SQL API örnek

Aşağıdaki kod örneği iki benzersiz anahtar kısıtlamalarını ile yeni bir SQL kapsayıcı oluşturulacağını gösterir. İlk sınırlamadır firstName, lastName, önceki örnekte açıklanan kısıtlaması e-posta. Kullanıcıların adresi/zipCode ikinci sınırlamadır. Yolları Bu benzersiz anahtar ilkede kullanan örnek bir JSON dosyası kod örnek aşağıda verilmiştir. 

```csharp
// Create a collection with two separate UniqueKeys, one compound key for /firstName, /lastName,
// and /email, and another for /address/zipCode.
private static async Task CreateCollectionIfNotExistsAsync(string dataBase, string collection)
{
    try
    {
        await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(dataBase, collection));
    }
    catch (DocumentClientException e)
    {
        if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            DocumentCollection myCollection = new DocumentCollection();
            myCollection.Id = collection;
            myCollection.PartitionKey.Paths.Add("/pk");
            myCollection.UniqueKeyPolicy = new UniqueKeyPolicy
            {
                UniqueKeys =
                new Collection<UniqueKey>
                {
                    new UniqueKey { Paths = new Collection<string> { "/firstName" , "/lastName" , "/email" }}
                    new UniqueKey { Paths = new Collection<string> { "/address/zipCode" } },

                }
            };
            await client.CreateDocumentCollectionAsync(
                UriFactory.CreateDatabaseUri(dataBase),
                myCollection,
                new RequestOptions { OfferThroughput = 2500 });
        }
        else
        {
            throw;
        }
    }
```

Örnek JSON belgesi.

```json
{
    "id": "1",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": [
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipCode": 98012
        }
    ],
}
```
## <a name="mongodb-api-sample"></a>MongoDB API örnek

Aşağıdaki komut örnek firstName, lastName ve e-posta alanları MongoDB API için kullanıcıların koleksiyonunun benzersiz bir dizin oluşturulacağını gösterir. Bu, tüm üç alanları birleşimi için benzersizlik koleksiyondaki tüm belgeler arasında sağlar. MongoDB API koleksiyonlar için koleksiyon oluşturulduktan sonra ancak koleksiyon doldurma önce benzersiz dizin oluşturulur.

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir veritabanında öğeleri için benzersiz anahtarlar oluşturmak nasıl öğrendiniz. Bir kapsayıcı ilk kez oluşturuyorsanız, gözden [Azure Cosmos veritabanı veri bölümlendirme](partition-data.md) benzersiz anahtarlar ve bölüm anahtarlarını birbirine bağlı olarak. 


