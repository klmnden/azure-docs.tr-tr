---
title: Azure Cosmos DB'de benzersiz anahtarlar | Microsoft Docs
description: Azure Cosmos DB veritabanınıza benzersiz anahtarlar kullanmayı öğrenin.
services: cosmos-db
keywords: benzersiz anahtar kısıtlaması, benzersiz anahtar kısıtlaması ihlali
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: rafats
ms.openlocfilehash: 796971ff541b62a22a70df4022ab78817e7158e9
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003326"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Azure Cosmos DB'de benzersiz anahtarlar

Benzersiz anahtarlar geliştiriciler kendi veritabanı için bir veri bütünlüğü katmanı ekleme olanağı sunar. Bir kapsayıcı oluştururken bir benzersiz anahtar ilkesi oluşturarak, bir veya daha fazla değer benzersizliği sağlamak [bölüm anahtarı](partition-data.md). Bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra tüm yeni veya güncelleştirilmiş öğeleri yinelenen değerler benzersiz anahtar kısıtlaması tarafından belirtilen değerlerle oluşturulmasını engeller.   

> [!NOTE]
> Benzersiz anahtarlar en son sürümleri tarafından desteklenen [.NET](sql-api-sdk-dotnet.md) ve [.NET Core](sql-api-sdk-dotnet-core.md) SQL SDK'ları ve [MongoDB API'si](mongodb-feature-support.md#unique-indexes). Tablo API ve Graph API'si şu anda benzersiz anahtarları desteklemez. 
> 
>

## <a name="use-case"></a>Kullanım örneği

Örneğin, bakalım nasıl bir kullanıcı veritabanı ile ilişkili en bir [sosyal uygulama](use-cases.md#web-and-mobile-applications) e-posta adresleri benzersiz bir anahtar ilke kalmamasını yararlı olabilir. Kullanıcının e-posta adresi benzersiz bir anahtar oluşturarak, her bir kaydın benzersiz e-posta adresi olan ve yinelenen bir e-posta adresleriyle yeni kayıt oluşturulabilir emin olun. 

Kullanıcıların oluşturabilmek istiyorsanız aynı olan birden fazla kayıt aynı ad, Soyadı adresini, e-posta ve e-posta adresi, diğer yolları için benzersiz anahtar ilkesi ekleyebilirsiniz. Bu nedenle bir e-posta adresini temel alan benzersiz bir anahtar oluşturmak yerine, bir ad, Soyadı ve e-posta birleşimi benzersiz bir anahtar oluşturabilirsiniz. Bu durumda, şu üç yoldan her benzersiz birleşimi izin verilir, böylece veritabanı aşağıdaki yol değerleri olan öğeler içerebilir. Bu kayıtların her birinde benzersiz anahtar ilkesi geçirmeniz gerekir.  

**İzin verilen değerler için ad, Soyadı ve e-posta benzersiz anahtar**

|Ad|Soyadı|E-posta adresi|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Çalışan Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Yukarıdaki tabloda listelenen birleşimleri biriyle başka bir kayıt eklemek denenirse, benzersiz anahtar kısıtlaması karşılanmadığı belirten bir hata alırsınız. Azure Cosmos DB döndürülen hata "Belirtilen kimliğe veya ada sahip kaynak zaten var." olan veya "Kaynak belirtilen kimlik, ad veya benzersiz bir dizin zaten var." 

## <a name="using-unique-keys"></a>Benzersiz anahtarlar kullanarak

Kapsayıcı oluşturulduğunda ve bölüm anahtarı için benzersiz anahtar kapsamlıdır benzersiz anahtarlar tanımlanmalıdır. Önceki örnekte, temel alınarak posta kodu bölümlemeniz halinde oluşturmak için her bölümün çoğaltılması tablodaki kayıtların olabilir.

Benzersiz anahtarları kullanmak için mevcut kapsayıcı güncelleştirilemiyor.

Bir kapsayıcı benzersiz bir anahtar ilke oluşturulduktan sonra ilkeyi kapsayıcısı oluşturmayın sürece değiştirilemez. Benzersiz anahtarlar üzerinde uygulamak istediğiniz mevcut veriler varsa, yeni bir kapsayıcı oluşturun ve yeni kapsayıcıya verileri taşımak için uygun veri geçiş aracını kullanın. SQL kapsayıcıları için [veri geçiş aracı](import-data.md). MongoDB kapsayıcıları için [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md).

Her benzersiz anahtarında en fazla 16 yol değerlerinin (örneğin /firstName, /lastName, /address/zipCode, vb.) eklenebilir. 

Her bir benzersiz anahtar ilkesi, en fazla 10 benzersiz anahtar kısıtlamaları olabilir veya birleşimleri ve tüm benzersiz dizin özelliklerini birleştirilmiş yollarını 60 karakterden fazla olmamalıdır. Bu nedenle kullanan önceki örnek adı, Soyadı, e-posta adresi, yalnızca bir sınırlamadır ve üç kullanılabilir 16 olası yolları kullanır. 

Birim oluşturmak için güncelleştirme, ücretleri, bir öğenin silinmesi biraz daha yüksek kapsayıcıdaki bir benzersiz anahtar ilkesi olduğunda isteyin. 

Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yollara değerlerini eksikse, bölümü benzersizlik kısıtlamasını alır özel bir null değer olarak kabul edilir.

## <a name="sql-api-sample"></a>SQL API örneği

Aşağıdaki kod örneği, iki benzersiz anahtar kısıtlamaları ile yeni bir SQL kapsayıcı oluşturulacağını gösterir. İlk sınırlamadır firstName, lastName, e-posta, önceki örnekte açıklanan kısıtlaması. Kullanıcılar Adres/PostaKodu ikinci sınırlamadır. Bu benzersiz anahtar ilkesi ile yolları kullanan bir örnek JSON dosyasını kod örnek aşağıda verilmiştir. 

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
                    new UniqueKey { Paths = new Collection<string> { "/address/zipcode" } },
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
    "pk": "1234",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": 
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipcode": 98012
        }
    
}
```
> [!NOTE]
> Lütfen Not benzersiz anahtar adı büyük/küçük harfe duyarlıdır. Yukarıdaki örnekte gösterildiği gibi /address/zipcode için benzersiz bir ad ayarlanır. Verilerinizi ZipCode varsa, posta kodu için ZipCode eşit değil olarak ardından, "null" içinde benzersiz anahtar ekler. Ve bu büyük/küçük harfe duyarlılık nedeniyle tüm ZipCode kayıtlarıyla yinelenen "null" benzersiz anahtar kısıtlamasını ihlal olarak eklenecek mümkün olmayacaktır.

## <a name="mongodb-api-sample"></a>MongoDB API örneği

Aşağıdaki örnek komut, firstName, lastName ve e-posta alanları MongoDB API'si için kullanıcı koleksiyonunun benzersiz bir dizin oluşturma işlemi gösterilmektedir. Bu koleksiyondaki tüm belgeleri üç tüm alanları bileşimi benzersiz sağlar. MongoDB API'si koleksiyonlar için koleksiyon oluşturulduktan sonra ancak önce koleksiyonu doldurma benzersiz dizin oluşturulur.

> [!NOTE]
> MongoDB API hesabı için benzersiz anahtar biçimi SQL API hesabı, farklı burada alanı adından önce ters eğik çizgi (/) karakteri belirtmeniz gerekmez. 

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Benzersiz anahtarlar Azure portalını kullanarak yapılandırma

Yukarıdaki bölümlerde nasıl SQL API veya MongoDB API'sini kullanarak bir koleksiyon oluştururken benzersiz anahtar kısıtlamaları tanımlayabilirsiniz gösteren kod örnekleri bulabilirsiniz. Ancak, Azure portalında web kullanıcı Arabirimi aracılığıyla bir koleksiyon oluştururken benzersiz anahtar tanımlamak da mümkündür. 

- Gidin **Veri Gezgini** Cosmos DB hesabınızın
- Tıklayın **yeni koleksiyon**
- Bölüm benzersiz anahtarları ** tıklayarak istenen benzersiz anahtar kısıtlamaları ekleyebilirsiniz **benzersiz anahtar Ekle**

![Veri Gezgini'nde benzersiz anahtarlar tanımlama](./media/unique-keys/unique-keys-azure-portal.png)

- Eklediğiniz, lastName yolu benzersiz bir anahtar kısıtlaması oluşturmak istiyorsanız, `/lastName`.
- Bunu lastName firstName birleşimi için benzersiz bir anahtar kısıtlaması oluşturmak isterseniz, Ekle `/lastName,/firstName`

Click bittiğinde **Tamam** koleksiyonu oluşturmak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir veritabanında öğelerinin benzersiz anahtarlara oluşturulacağını öğrendiniz. Bir kapsayıcı ilk kez oluşturuyorsanız, gözden [verileri Azure Cosmos DB'de bölümleme](partition-data.md) gibi benzersiz anahtarlar ve bölüm anahtarları birbirine dayanır. 


