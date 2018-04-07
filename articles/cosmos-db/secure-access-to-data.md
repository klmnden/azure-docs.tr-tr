---
title: Azure Cosmos DB veri erişimi güvenli öğrenin | Microsoft Docs
description: Azure Cosmos veritabanı ana anahtarları, salt okunur anahtarları, kullanıcılar ve izinler de dahil olmak üzere, erişim denetimi kavramları hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: 8641225d-e839-4ba6-a6fd-d6314ae3a51c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: sngun
ms.openlocfilehash: 7a53dda7d6b49187d77ca44bcb55db5f9c305f64
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="securing-access-to-azure-cosmos-db-data"></a>Azure Cosmos DB verilere erişimin güvenliğini sağlama
Bu makalede depolanan verilere erişimin güvenliğini sağlama genel bir bakış sağlar [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB anahtarları iki tür kullanıcıların kimliğini doğrulamak ve kendi veri ve kaynaklarına erişim sağlamak için kullanır. 

|Anahtar türü|Kaynaklar|
|---|---|
|[Ana anahtarları](#master-keys) |Yönetim kaynaklar için kullanılan: veritabanı hesapları, veritabanları, kullanıcılar ve izinler|
|[Kaynak belirteçleri](#resource-tokens)|Uygulama kaynakları için kullanılan: koleksiyonlar, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler|

<a id="master-keys"></a>

## <a name="master-keys"></a>Ana anahtarları 

Ana anahtarları veritabanı hesabı için tüm yönetim kaynaklara erişim sağlar. Ana anahtarları:  
- Hesapları, veritabanları, kullanıcılar ve izinler erişim sağlar. 
- Koleksiyonlar ve belgeler için ayrıntılı erişim sağlamak için kullanılamaz.
- Bir hesap oluşturma sırasında oluşturulur.
- Herhangi bir zamanda yeniden oluşturulacak.

Her hesap iki ana anahtarları oluşur: bir birincil anahtar ve ikincil anahtar. Çift anahtarları amacı, böylelikle yeniden oluşturmak veya hesabı ve veri sürekli erişim sağlayan, anahtarları alma. 

Cosmos DB hesabı için iki ana anahtarları yanı sıra iki salt okunur anahtar vardır. Bu salt okunur anahtarları yalnızca okuma işlemleri hesabındaki izin verir. Salt okunur anahtarları izinleri kaynakları okuma erişimi sağlamaz.

Birincil, ikincil, salt okunur ve okuma-yazma ana anahtarları alınabilir ve Azure portalını kullanarak yeniden oluşturulacak. Yönergeler için bkz: [görüntüleme, kopyalama ve yeniden oluşturma erişim tuşları](manage-account.md#keys).

![NoSQL veritabanı güvenliği gösteren Azure portal - erişim denetimi (IAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Ana anahtarınızı döndürme basit işlemidir. İkincil anahtarınızı almak ikincil anahtarınızı uygulamanızda birincil anahtarınızı değiştirmek için Azure portalına gidin ve ardından Azure portalında birincil anahtar döndür.

![NoSQL veritabanı güvenliği gösteren Azure portalında - ana anahtar döndürme](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Bir ana anahtar kullanmak için örnek kod

Aşağıdaki kod örneği Cosmos DB hesap uç noktası ve ana anahtar bir DocumentClient oluşturmak ve bir veritabanı oluşturmak için nasıl kullanılacağı gösterilmektedir. 

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//NB > Keep these values in a safe and secure location. Together they provide Administrative access to your DocDB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly SecureString authorizationKey = ToSecureString(ConfigurationManager.AppSettings["AuthorizationKey"]);

client = new DocumentClient(new Uri(endpointUrl), authorizationKey);

// Create Database
Database database = await client.CreateDatabaseAsync(
    new Database
    {
        Id = databaseName
    });
```

<a id="resource-tokens"></a>

## <a name="resource-tokens"></a>Kaynak belirteçleri

Kaynak belirteçleri bir veritabanı içinde uygulama kaynaklarına erişim sağlar. Kaynak belirteçleri:
- Belirli koleksiyonlar, bölüm anahtarlarını, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler erişim sağlar.
- Ne zaman oluşturulan bir [kullanıcı](#users) verilir [izinleri](#permissions) belirli bir kaynağa.
- İzni kaynak bağlı POST, GET ve PUT çağrısı tarafından çalıştırıldığı zaman yeniden oluşturulur.
- Kullanıcı, kaynak ve izni için özel olarak oluşturulan bir karma kaynak belirteci kullanın.
- Özelleştirilebilir geçerlilik süresi ile ilişkili zamanı. Varsayılan geçerli timespan bir saattir. Belirteç ömrü, ancak açıkça, en fazla beş saat kadar belirtilebilir.
- Ana anahtar vermiş için güvenli bir alternatif sunar. 
- İstemcilerin okuma, yazma ve bunlar verilmiş izinleriyle Cosmos DB hesaptaki kaynakları silmek için etkinleştirin.

Kaynak belirteci (Cosmos DB kullanıcılar ve izinler oluşturarak) kullanabileceğiniz ana anahtarıyla güvenilemez bir istemciye Cosmos DB hesabınızdaki kaynaklara erişimi sağlamak istediğinizde.  

Cosmos DB kaynak belirteçleri okuma, yazma ve kaynakları izinler göre ve bir ana veya okuma yalnızca anahtar gerek kalmadan Cosmos DB hesabınızda silmek için istemcileri etkinleştirir güvenli bir alternatif sunar.

Adlı kaynak belirteçleri istenen, oluşturulan ve istemcilere teslim tipik tasarım deseni şöyledir:

1. Orta katman hizmet kullanıcı fotoğraflarını paylaşmak için bir mobil uygulamasına hizmet edecek şekilde ayarlanır. 
2. Orta katman hizmet ana anahtarı Cosmos DB hesabının sahiptir.
3. Fotoğraf uygulaması son kullanıcı mobil cihazlarda yüklü. 
4. Oturum açma, Orta katmanda hizmeti ile kullanıcı kimliğini fotoğraf uygulaması oluşturur. Bu mekanizma kimlik kuruluş tamamen kadar uygulamasıdır.
5. Kimlik kurulduktan sonra orta katman hizmet kimliğine göre izinleri ister.
6. Orta katman hizmet kaynak belirteci telefon uygulamaya gönderir.
7. Telefon uygulaması doğrudan kaynak belirteci tarafından ve kaynak belirteci tarafından izin verilen aralığı için tanımlanan izinlerle Cosmos DB kaynaklara erişmek için kaynak belirteci kullanmaya devam edebilirsiniz. 
8. Kaynak belirtecinin süresi dolduğunda, sonraki istekler bir 401 Yetkisiz özel alırsınız.  Bu noktada, telefon uygulaması kimliği yeniden oluşturur ve yeni bir kaynak belirteci ister.

    ![İş akışı Azure Cosmos DB kaynak belirteçleri](./media/secure-access-to-data/resourcekeyworkflow.png)

Kaynak belirteci oluşturma ve yönetim yerel Cosmos DB istemci kitaplıkları tarafından işlenir; Ancak, REST kullanırsanız, istek/kimlik doğrulama üstbilgileri oluşturmalıdır. İçin REST kimlik doğrulama üstbilgileri oluşturma hakkında daha fazla bilgi için bkz: [Cosmos DB kaynaklarda erişim denetimi](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) veya [bizim SDK'ları için kaynak kodu](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Kaynak belirteçleri aracısı veya oluşturmak için kullanılan bir orta katman hizmet örneği için bkz: [ResourceTokenBroker uygulama](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Kullanıcılar
Cosmos DB kullanıcılar Cosmos DB veritabanı ile ilişkilidir.  Her veritabanı sıfır veya daha fazla Cosmos DB kullanıcılar içerebilir.  Aşağıdaki kod örneği Cosmos DB kullanıcı kaynağı oluşturulacağını gösterir.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Her Cosmos DB kullanıcı listesini almak için kullanılan PermissionsLink özelliğine [izinleri](#permissions) kullanıcıyla ilişkili.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>İzinler
Cosmos DB izni kaynak Cosmos DB kullanıcıyla ilişkili değil.  Her bir kullanıcı, sıfır veya daha fazla Cosmos DB izinleri içerebilir.  Belirli uygulama kaynağa erişmeye çalışırken kullanıcının gerektiren bir güvenlik belirteci erişim izni kaynak sağlar.
İzni kaynak tarafından sağlanan iki kullanılabilir erişim düzeyleri şunlardır:

* Tümü: Kullanıcı kaynak üzerinde tam izni.
* Şunu okuyun: Kullanıcı kaynak içeriğini salt okunur ancak yazma, güncelleştirme veya silme işlemleri kaynak üzerinde gerçekleştirilemiyor.

> [!NOTE]
> Cosmos DB çalıştırmak için saklı yordamlar kullanıcı saklı yordamı çalıştırılacağı koleksiyonda tüm izni olmalıdır.
> 
> 

### <a name="code-sample-to-create-permission"></a>İzin oluşturmak için örnek kod

Aşağıdaki kod örneği, izni kaynak oluşturma, okuma izni kaynağının kaynak belirteci ve izinlerle ilişkilendirmek gösterilmiştir [kullanıcı](#users) yukarıda oluşturduğunuz.

```csharp
// Create a permission.
Permission docPermission = new Permission
{
    PermissionMode = PermissionMode.Read,
    ResourceLink = UriFactory.CreateDocumentCollectionUri("db", "collection"),
    Id = "readperm"
};
  
docPermission = await client.CreatePermissionAsync(UriFactory.CreateUserUri("db", "user"), docPermission);
Console.WriteLine(docPermission.Id + " has token of: " + docPermission.Token);
```

Koleksiyonunuz için bir bölüm anahtarı belirtilmişse izin koleksiyonu, belge ve ek kaynaklar için aynı zamanda ResourceLink yanı sıra ResourcePartitionKey içermelidir.

### <a name="code-sample-to-read-permissions-for-user"></a>Kullanıcı izinlerini okumak için örnek kod

Tüm izin kolayca almak için kaynakları her kullanıcı nesnesi için akış izin, kullanılabilir Cosmos DB yaptığı gibi belirli bir kullanıcı ile ilişkilendirilmiş.  Aşağıdaki kod parçacığında, yukarıda oluşturduğunuz kullanıcıyla ilişkilendirilmiş izin almak, bir izin listesi oluşturmak ve kullanıcı adına yeni bir DocumentClient örneği gösterilmektedir.

```csharp
//Read a permission feed.
FeedResponse<Permission> permFeed = await client.ReadPermissionFeedAsync(
  UriFactory.CreateUserUri("db", "myUser"));
 List<Permission> permList = new List<Permission>();

foreach (Permission perm in permFeed)
{
    permList.Add(perm);
}

DocumentClient userClient = new DocumentClient(new Uri(endpointUrl), permList);
```

## <a name="next-steps"></a>Sonraki adımlar
* Cosmos DB veritabanı güvenliği hakkında daha fazla bilgi için bkz: [Cosmos DB: Veritabanı Güvenlik](database-security.md).
* Ana ve salt okunur anahtarlarını yönetme hakkında bilgi edinmek için [Azure Cosmos DB hesabın nasıl yönetileceği](manage-account.md#keys).
* Azure Cosmos DB yetkilendirme belirteçleri oluşturulacağını öğrenmek için bkz: [Azure Cosmos DB kaynaklarda erişim denetimi](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources).
