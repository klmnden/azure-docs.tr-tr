---
title: Azure Cosmos DB'de veri erişiminin güvenliğini sağlama hakkında bilgi edinin
description: Erişim denetimi kavramlarına ana anahtarları, salt okunur anahtarlar, kullanıcılar ve izinler de dahil olmak üzere Azure Cosmos DB'de hakkında bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2018
ms.author: rimman
ms.openlocfilehash: 133181fcc76d759a57725df1ff965966f3797399
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60446518"
---
# <a name="secure-access-to-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de veri erişiminin güvenliğini sağlama

Bu makalede depolanan verilere erişimin güvenliğini sağlama genel bir bakış sağlar [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Azure Cosmos DB, kullanıcıların kimliklerini doğrulamak ve kendi veri ve kaynaklara erişim sağlamak için iki tür anahtarları kullanır. 

|Anahtar türü|Kaynaklar|
|---|---|
|[Ana anahtarları](#master-keys) |Yönetim kaynakları için kullanılan: veritabanı hesapları, veritabanları, kullanıcılar ve izinler|
|[Kaynak belirteçleri](#resource-tokens)|Uygulama kaynakları için kullanılan: kapsayıcılar, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler|

<a id="master-keys"></a>

## <a name="master-keys"></a>Ana anahtarları 

Ana anahtarları, veritabanı hesabı için tüm yönetim kaynaklara erişim sağlar. Ana anahtarları:  
- Erişim hesapları, veritabanları, kullanıcılar ve izinler sağlar. 
- Kapsayıcılar ve belgeler için ayrıntılı erişim sağlamak için kullanılamaz.
- Bir hesap oluşturma sırasında oluşturulur.
- Herhangi bir zamanda yeniden oluşturulabilir.

Her hesap iki ana anahtarları oluşur: bir birincil anahtar ve ikincil anahtar. Çift anahtarları amacı, böylelikle yeniden oluşturulmuş veya hesabınızı ve verilerinizi sürekli erişim anahtarları, geri alma. 

Cosmos DB hesabı için iki ana anahtarları ek olarak iki salt okunur anahtar vardır. Bu salt okunur anahtarlar yalnızca hesabın okuma işlemlerinin izin verir. Salt okunur anahtarları izinleri kaynakları okumak üzere erişim sağlamaz.

Birincil, ikincil, salt okunur ve okuma-yazma ana anahtarları alınabilir ve Azure portalını kullanarak yeniden oluşturuldu. Yönergeler için [görüntüleme, kopyalama ve yeniden oluşturma erişim anahtarlarını](manage-with-cli.md#regenerate-account-key).

![NoSQL veritabanı güvenliği gösteren Azure portalı - erişim denetimi (IAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Ana anahtarınızı döndürme işlemini basittir. İkincil anahtarınızı alma ve birincil anahtarınızı uygulamanızdaki ikincil anahtarınızı değiştirin Azure portalına gidin ve ardından Azure portalında birincil anahtarı döndür.

![NoSQL veritabanı güvenliği gösteren Azure portalı - ana anahtarı dönüşü](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Bir ana anahtar kullanmak için kod örneği

Aşağıdaki kod örneği bir Cosmos DB hesabınızın uç noktası ve ana anahtarı bir DocumentClient örneği oluşturma ve bir veritabanı oluşturmak için nasıl kullanılacağını gösterir. 

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

Kaynak belirteçleri, bir veritabanı içinde uygulama kaynaklarına erişim sağlar. Kaynak belirteçleri:
- Belirli bir kapsayıcı, bölüm anahtarları, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler erişim sağlar.
- Ne zaman oluşturulan bir [kullanıcı](#users) verilir [izinleri](#permissions) belirli bir kaynağa.
- Bir izni kaynak üzerinde POST, GET veya PUT çağrısı tarafından izlemede durumlarda yeniden oluşturulur.
- Kullanıcı, kaynak ve izni için özel olarak oluşturulmuş bir karma kaynak belirtecini kullanın.
- Özelleştirilebilir geçerlilik süresiyle bağlı zamanı. Varsayılan geçerli zaman bir saattir. Belirteç ömrü, ancak açıkça, en fazla beş saate kadar belirtilebilir.
- Ana anahtar verme için güvenli bir yöntem sağlar. 
- Okuma, yazma ve Cosmos DB hesabı geneline verdiğiniz izinleri göre kaynakları silmek için istemcileri etkinleştirir.

Kaynak belirteci (Cosmos DB kullanıcıları ve izinleri oluşturarak) kullanabileceğiniz ana anahtarı ile güvenilir olmayan bir istemci, Cosmos DB hesabınızdaki kaynaklara erişimi sağlamak istediğinizde.  

Cosmos DB kaynak belirteçleri, okuma, yazma ve Cosmos DB hesabınızın göre verilen izinleri ve ana veya yalnızca anahtar okuma için gerek kalmadan kaynakları silmek için istemcileri etkinleştirir güvenli bir yöntem sağlar.

Yapabildiği kaynak belirteçleri istediği, oluşturulan ve istemciye teslim tipik tasarım Düzen aşağıdaki gibidir:

1. Orta katmanlı bir hizmet, kullanıcı fotoğraflarını paylaşmak için bir mobil uygulamanıza hizmet edecek şekilde ayarlanır. 
2. Orta katman hizmet Cosmos DB hesabı ana anahtarı sahiptir.
3. Fotoğraf uygulaması, son kullanıcı mobil cihazlara yüklenir. 
4. Oturum açma, Orta katmanda hizmeti ile kullanıcı kimliğini fotoğraf uygulaması oluşturur. Bu mekanizma kimlik kuruluş tamamen kadar uygulamasıdır.
5. Kimlik kurulduktan sonra orta katmanda hizmet kimliğine izin ister.
6. Orta katman hizmet kaynak belirtecini telefon uygulamaya geri gönderir.
7. Telefon uygulaması doğrudan kaynak belirtecini ve kaynak belirtecini tarafından izin verilen zaman aralığı için tanımlanan izinlere sahip Cosmos DB kaynaklarına erişmek için kaynak belirtecini kullanmaya devam edebilirsiniz. 
8. Kaynak belirtecin süresi dolduğunda, sonraki istekler 401 Yetkisiz bir özel durum alırsınız.  Bu noktada, telefon uygulaması kimliği yeniden oluşturur ve yeni bir kaynak belirteci ister.

    ![Azure Cosmos DB kaynak belirteçleri iş akışı](./media/secure-access-to-data/resourcekeyworkflow.png)

Kaynak belirteci oluşturma ve yönetim yerel Cosmos DB istemci kitaplıkları tarafından işlenir; Ancak, REST kullanırsanız, istek/kimlik doğrulama üstbilgileri oluşturmak gerekir. İçin REST kimlik doğrulama üst bilgileri oluşturma hakkında daha fazla bilgi için bkz. [Cosmos DB kaynaklarına erişim denetimini](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) veya [Larımız için kaynak kodu](https://github.com/Azure/azure-documentdb-node/blob/master/source/lib/auth.js).

Kaynak belirteçleri aracı veya oluşturmak için kullanılan bir orta katman hizmet örneği için bkz: [ResourceTokenBroker uygulama](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

<a id="users"></a>

## <a name="users"></a>Kullanıcılar
Cosmos DB kullanıcıların Cosmos DB veritabanı ile ilişkilidir.  Her veritabanı, sıfır veya daha fazla Cosmos DB kullanıcıları içerebilir.  Aşağıdaki kod örneği bir Cosmos DB kullanıcı kaynak oluşturma işlemini gösterir.

```csharp
//Create a user.
User docUser = new User
{
    Id = "mobileuser"
};

docUser = await client.CreateUserAsync(UriFactory.CreateDatabaseUri("db"), docUser);
```

> [!NOTE]
> Her Cosmos DB kullanıcı listesini almak için kullanılan bir PermissionsLink özelliğine sahip [izinleri](#permissions) kullanıcıyla ilişkili.
> 
> 

<a id="permissions"></a>

## <a name="permissions"></a>İzinler
Bir Cosmos DB izni kaynak bir Cosmos DB kullanıcıyla ilişkili olur.  Her bir kullanıcı, sıfır veya daha fazla Cosmos DB izinleri içerebilir.  Bir izni kaynak kullanıcının belirli bir uygulama bir kaynağa erişmeye çalışırken gereken bir güvenlik belirteci erişim sağlar.
Bir izni kaynak tarafından sağlanan iki kullanılabilir erişim düzeyi vardır:

* Tüm: Kullanıcı, kaynağın tam izni vardır.
* Okuma: Kullanıcı yalnızca kaynak içeriğini okuyabilir, ancak yazma, güncelleştirme veya silme işlemleri kaynak üzerinde gerçekleştirilemiyor.

> [!NOTE]
> Cosmos DB çalıştırmak için saklı gereken yordamları kullanıcı, saklı yordam çalıştırılacağı kapsayıcıdaki tüm iznine sahip olmalıdır.
> 
> 

### <a name="code-sample-to-create-permission"></a>İzin oluşturmak için kod örneği

Aşağıdaki kod örneği nasıl izni kaynak oluşturma, okuma izni kaynak kaynak belirtecini ve izinleri ile ilişkilendirmek gösterir [kullanıcı](#users) yukarıda oluşturduğunuz.

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

Koleksiyonunuz için bir bölüm anahtarı belirtilmişse koleksiyonu, belge ve ek kaynaklar için izni ResourcePartitionKey ResourceLink yanı sıra aynı zamanda içermelidir.

### <a name="code-sample-to-read-permissions-for-user"></a>Kullanıcı izinlerini okumayı kod örneği

Kaynakları kolayca tüm izin almak için her bir kullanıcının nesne için akış izin, Cosmos DB yapar kullanılabilir gibi belirli bir kullanıcı ile ilişkilendirilmiş.  Aşağıdaki kod parçacığı, yukarıda oluşturulan kullanıcıyla ilişkilendirilmiş izin almak, bir izin listesi oluşturmak ve kullanıcı adına yeni bir DocumentClient örneğini oluşturma işlemi gösterilmektedir.

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

## <a name="add-users-and-assign-roles"></a>Kullanıcı ekleme ve Rolleri Ata

Azure Cosmos DB hesabı okuyucusu erişimi kullanıcı hesabınıza eklemek için Azure portalında aşağıdaki adımları uygulayın, bir abonelik sahibi var.

1. Azure portalını açın ve Azure Cosmos DB hesabınızı seçin.
2. Tıklayın **erişim denetimi (IAM)** sekmesine ve ardından **+ rol ataması Ekle**.
3. İçinde **rol ataması Ekle** bölmesinde, **rol** kutusunda **Cosmos DB hesabı okuyucusu rolü**.
4. İçinde **kutusuna erişim atama**seçin **Azure AD kullanıcı, Grup veya uygulama**.
5. Dizininizde, erişim vermek istediğiniz kullanıcı, Grup veya uygulama seçin.  Görünen ad, e-posta adresi veya nesne tanımlayıcıları dizin arama yapabilirsiniz.
    Seçilen kullanıcıya, gruba veya uygulamaya seçili Üyeler listesinde görünür.
6. **Kaydet**’e tıklayın.

Varlık, artık Azure Cosmos DB kaynaklarını okuyabilir.

## <a name="delete-or-export-user-data"></a>Kullanıcı verilerini dışarı aktarma veya silme
Azure Cosmos DB, arayın, seçin, değiştirmek ve veritabanı veya koleksiyon içinde bulunan herhangi bir kişisel verilerini silme sağlar. Azure Cosmos DB API'leri bulup ancak kişisel verilerini silme sağlar, API'leri ve kişisel verileri silmek için gerekli mantığı tanımlamak için sizin sorumluluğunuzdadır. Her çok modelli bir API (SQL, MongoDB, Gremlin, Cassandra, tablo) farklı dil aramak ve kişisel verilerini silme yöntemlerini içeren SDK'lar sağlar. Ayrıca etkinleştirebilirsiniz [süresi (TTL) canlı](time-to-live.md) belirli bir süre sonra otomatik olarak silmek herhangi bir ek ücret ödemeden veri özelliği.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Cosmos DB veritabanı güvenliği hakkında daha fazla bilgi için bkz: [Cosmos DB: Veritabanı Güvenlik](database-security.md).
* Azure Cosmos DB yetkilendirme belirteçleri oluşturmak nasıl öğrenmek için bkz. [erişim denetimini Azure Cosmos DB kaynaklarını](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources).
