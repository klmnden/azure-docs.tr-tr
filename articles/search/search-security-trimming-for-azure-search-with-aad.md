---
title: Active Directory - Azure Search kullanarak sonuçları kırpmak için güvenlik filtreleri
description: Güvenlik filtreleri ve Azure Active Directory (AAD) Kimlikleridir kullanarak Azure Search içerik üzerinde erişim denetimi.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 11/07/2017
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 410727022b092e2dd8ab8b05e628e25fd60ab833
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282228"
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Active Directory kimlikleri kullanarak Azure Search Sonuçları kırpma için güvenlik filtreleri

Bu makalede Azure Active Directory (AAD) güvenlik kimlikleri filtreleri ile birlikte Azure Search'te kullanıcı grubu üyeliğine göre arama sonuçları kırpma için nasıl kullanılacağını gösterir.

Bu makale aşağıdaki görevleri kapsar:
> [!div class="checklist"]
> - AAD grupları ve kullanıcı oluşturma
> - Kullanıcı oluşturmuş olduğunuz grubuyla ilişkilendirin
> - Yeni gruplar önbelleğe alma
> - İlişkili grupları ile dizin belgeleri
> - Grup tanımlayıcıları Filtresi ile arama isteği sorunu
> 
> [!NOTE]
> Bu makaledeki örnek kod parçacıkları C# dilinde yazılmıştır. Tam kaynak kodunu [GitHub](https://aka.ms/search-dotnet-howto)'da bulabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar

Azure Search dizininizi olmalıdır bir [güvenlik alanı](search-security-trimming-for-azure-search.md) belge okuma erişimi olan Grup kimliklerinin listesini depolamak için. Bu kullanım örneği, güvenli kılınabilir bir öğeye (örneğin, bir kişinin üniversite uygulaması) ve bu öğeye (kullandılar personeli için) kimlerin erişebileceğini belirten bir güvenlik alanı arasında bire bir iletişimin varsayar.

Bu kılavuzda, AAD içinde kullanıcıları, grupları ve ilişkileri oluşturmak için gerekli AAD yönetici izinleri olmalıdır.

Uygulamanız aşağıdaki yordamda açıklandığı gibi AAD ile de kaydedilmelidir.

### <a name="register-your-application-with-aad"></a>Uygulamanızı AAD'ye kaydetme

Bu adım, uygulamanızın oturum açma kullanıcı ve grup hesapları kabul etmek amacıyla AAD ile tümleştirilir. AAD yönetici Kuruluşunuzda kök kullanıcı değilseniz, gerekebilir [yeni bir kiracı oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) için aşağıdaki adımları gerçekleştirin.

1. Git [ **uygulama kayıt portalı**](https://apps.dev.microsoft.com) >  **yakınsanmış uygulama** > **uygulama ekleme**.
2. Uygulamanız için bir ad girin ve ardından tıklayın **Oluştur**. 
3. Yeni kaydettiğiniz uygulamanız uygulamalarım sayfasında seçin.
4. Uygulama kayıt sayfasında > **platformları** > **Platform Ekle**, seçin **Web API**.
5. Yine de uygulama kayıt sayfasında gidin > **Microsoft Graph izinleri** > **Ekle**.
6. İzinleri seçin, aşağıdaki temsilci izinleri ekleyin ve ardından **Tamam**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph REST API aracılığıyla AAD programlı erişim sağlayan bir API sağlar. Bu izlenecek yol için kod örneği, grupları, kullanıcıları ve ilişkileri oluşturmak için Microsoft Graph API'sini çağırmak için izinleri kullanır. API'leri daha hızlı filtreleme için önbellek grup kimlikleri için de kullanılır.

## <a name="create-users-and-groups"></a>Kullanıcılar ve gruplar oluşturma

Yerleşik uygulama arama ekliyorsanız, varolan kullanıcı ve Grup tanımlayıcıları AAD'de olabilir. Bu durumda, sonraki üç adımı atlayabilirsiniz. 

Ancak, varolan kullanıcı yoksa, güvenlik sorumluları oluşturmak için Microsoft Graph API'lerini kullanabilirsiniz. Aşağıdaki kod parçacıkları için Azure Search dizininizi güvenlik alanında veri değerleri duruma tanımlayıcıları, oluşturma adımları gösterilmektedir. Bizim kuramsal üniversite kullandılar uygulamasında bu kullandılar personel güvenlik tanımlayıcıları olacaktır.

Kullanıcı ve grup üyeliği, özellikle büyük kuruluşlarda çok akıcı olabilir. Kullanıcı ve grup kimlikleri oluşturan kod sıklıkta kuruluş üyelik değişiklikleri alması için çalıştırmanız gerekir. Benzer şekilde, Azure Search dizininizi izin verilen kullanıcılar ve kaynaklar geçerli durumu yansıtacak şekilde benzer bir güncelleştirme zamanlaması gerektirir.

### <a name="step-1-create-aad-grouphttpsdocsmicrosoftcomgraphapigroup-post-groupsviewgraph-rest-10"></a>1. Adım: Oluşturma [AAD grubu](https://docs.microsoft.com/graph/api/group-post-groups?view=graph-rest-1.0) 
```csharp
// Instantiate graph client 
GraphServiceClient graph = new GraphServiceClient(new DelegateAuthenticationProvider(...));
Group group = new Group()
{
    DisplayName = "My First Prog Group",
    SecurityEnabled = true,
    MailEnabled = false,
    MailNickname = "group1"
}; 
Group newGroup = await graph.Groups.Request().AddAsync(group);
```
   
### <a name="step-2-create-aad-userhttpsdocsmicrosoftcomgraphapiuser-post-usersviewgraph-rest-10"></a>2. Adım: Oluşturma [AAD kullanıcısı](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0)
```csharp
User user = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = "User1@FirstUser.com",
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
User newUser = await graph.Users.Request().AddAsync(user);
```

### <a name="step-3-associate-user-and-group"></a>3. Adım: Kullanıcı ve Grup ilişkilendirme
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>4. Adım: Grupları tanımlayıcılar önbelleğe alma
İsteğe bağlı olarak, ağ gecikme süresini azaltmak için bir arama talebi, grupları önbellekten bir gidiş dönüş AAD'ye kaydetme getirilir, böylece kullanıcı grubu ilişkilendirmeleri önbelleğe alabilir. Kullanabileceğiniz [AAD Batch API'sini](https://developer.microsoft.com/graph/docs/concepts/json_batching) birden çok kullanıcıya sahip tek bir Http isteği gönderip önbelleği oluşturacak.

Microsoft Graph, büyük hacimde istekleri işlemek için tasarlanmıştır. Büyük bir istek sayısı meydana gelirse, Microsoft Graph İstek HTTP durum kodu 429 ile başarısız olur. Daha fazla bilgi için [Microsoft Graph azaltma](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>İzin verilen kendi gruplarıyla dizin belge

Azure Search'te sorgu işlemleri, Azure Search dizini üzerinde yürütülür. Bu adımda, bir dizin oluşturma işlemi güvenlik filtre olarak kullanılan tanımlayıcıları dahil olmak üzere, bir dizin aranabilir verileri aktarır. 

Azure Search'ü değil kullanıcı kimlikleri doğrulamak veya hangi içeriği görüntüleme izni kullanıcının kurmak için mantığı sağlar. Güvenlik kırpma için kullanım örneği, önemli belge ve bozulmadan search dizinine içeri aktarılan bu belgeye erişimi olan grup tanımlayıcısı arasındaki ilişkiyi sağladığını varsayar. 

Azure Search dizini PUT İsteği gövdesi kuramsal örnekte Başvuranın üniversite özellik veya transkript bu içeriği görüntüleme izni olan grup tanımlayıcısı ile birlikte içerir. 

Bu kılavuz için kod örneğinde kullanılan genel örnek dizin eylemi şu şekilde görünebilir:

```csharp
var actions = new IndexAction<SecuredFiles>[]
              {
                  IndexAction.Upload(
                  new SecuredFiles()
                  {
                      FileId = "1",
                      Name = "secured_file_a",
                      GroupIds = new[] { groups[0] }
                  }),
              ...
             };

var batch = IndexBatch.New(actions);

_indexClient.Documents.Index(batch);  
```

## <a name="issue-a-search-request"></a>Bir arama talebi verme

Güvenlik kırpma amacıyla statik değerleri dahil olmak üzere veya arama sonuçlarındaki belgelerin hariç kullanılan dizin, güvenlik alanında değerlerdir. Grup tanımlayıcısı Kullandılar "A11B22C33D44 E55F66G77 H88I99JKK" ise, örneğin, tüm belgelerin Dosyalanan güvenlik tanımlayıcı sahip bir Azure Search dizini dahil (dışlanan geri istek sahibine gönderilen arama sonuçlarında veya).

Talep verme kullanıcı gruplarını temel alan arama sonuçlarında döndürülen belgelerin filtre uygulamak için aşağıdaki adımları gözden geçirin.

### <a name="step-1-retrieve-users-group-identifiers"></a>1. Adım: Kullanıcının grup kimlikleri alma

Kullanıcının grupları zaten önbelleğe alınmazsa veya önbellek süresi doldu, sorun [grupları](https://docs.microsoft.com/graph/api/directoryobject-getmembergroups?view=graph-rest-1.0) isteği
```csharp
private static void RefreshCacheIfRequired(string user)
{
    if (!_groupsCache.ContainsKey(user))
    {
        var groups = GetGroupIdsForUser(user).Result;
        _groupsCache[user] = groups;
    }
}

private static async Task<List<string>> GetGroupIdsForUser(string userPrincipalName)
{
    List<string> groups = new List<string>();
    var allUserGroupsRequest = graph.Users[userPrincipalName].GetMemberGroups(true).Request();

    while (allUserGroupsRequest != null) 
    {
        IDirectoryObjectGetMemberGroupsRequestBuilder allUserGroups = await allUserGroupsRequest.PostAsync();
        groups = allUserGroups.ToList();
        allUserGroupsRequest = allUserGroups.NextPageRequest;
    }
    return groups;
}
``` 

### <a name="step-2-compose-the-search-request"></a>2. Adım: Arama isteği oluştur

Kullanıcının grup üyeliği olduğunu varsayarsak, uygun filtre değerleriyle arama isteği gönderebilir.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>3. Adım: Sonuçlarını işleme

Yanıt, belgeler, kullanıcı görüntüleme iznine sahip olanlar oluşan filtrelenmiş bir listesini içerir. Arama sonuçları sayfasını nasıl oluşturmak bağlı olarak, filtrelenmiş sonuç kümesinde yansıtacak şekilde görsel ipuçları eklemek isteyebilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu kılavuzda, Azure arama sonuçlarındaki belgelerin filtrelemek için AAD oturum açma kullanarak teknikleri istek üzerine sağlanan filtre eşleşmeyen belgeleri sonuçlarını kırpma öğrendiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure arama filtreleri kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Search'te filtreler](search-filters.md)
+ [Azure Search işlemlerinde veri güvenliği ve erişim denetimi](search-security-overview.md)
