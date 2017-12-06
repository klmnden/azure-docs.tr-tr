---
title: "Azure Active Directory ile Azure Search'te kırpma güvenlik | Microsoft Docs"
description: "Azure arama filtresi ve Azure Active Directory kullanılarak güvenlik kırpma uygulayın."
services: search
author: revitalbarletz
manager: jlembicz
ms.service: search
ms.topic: article
ms.date: 11/07/2017
ms.author: revitalb
ms.openlocfilehash: 8d277ff43aa0d5d14471426632b5aa369df0e316
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="security-trimming-in-azure-search-with-azure-active-directory"></a>Azure Search'te Azure Active Directory ile güvenlik kırpma

Bu makalede, Azure Active Directory (AAD) Azure arama ile birlikte kullanıcı grup üyeliğine dayalı belge erişimi kısıtlamak için nasıl kullanılacağı gösterilmektedir.

Bu makalede aşağıdaki görevleri içerir:
> [!div class="checklist"]
- AAD gruplar ve kullanıcılar oluşturma
- Kullanıcı, oluşturduğunuz grubuyla ilişkilendirin
- Yeni gruplar önbelleğe alma
- Dizin belgelerle ilişkili grupları
- Grup tanımlayıcıları Filtresi ile arama isteği gönderin

>[!NOTE]
> Bu makaledeki örnek kod parçacıkları C# dilinde yazılmıştır. Tam kaynak kodunu [GitHub](http://aka.ms/search-dotnet-howto)'da bulabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Azure Search dizininizi olmalıdır bir [güvenlik alan](search-security-trimming-for-azure-search.md) belgeyi okuma erişimi olan grup kimlikleri listesini depolamak için. Bu kullanım örneği bire bir güvenliği sağlanabilir öğesi (örneğin, bireyin üniversitenin uygulaması) ve bu öğeye (giriş personeli) kimlerin erişebileceğini belirten bir güvenlik alanı arasındaki varsayar.

Kullanıcılar, gruplar ve ilişkilendirmeleri AAD'de oluşturmak için bu kılavuzda gerekli AAD yönetici izinleri olmalıdır.

Uygulamanız da AAD'ye, aşağıdaki yordamda açıklandığı gibi kayıtlı olması gerekir.

### <a name="register-your-application-with-aad"></a>AAD ile uygulamanızı kaydetme

Bu adımı uygulamanız oturum açma işlemleri, kullanıcı ve grup hesaplarını kabul etmek amacıyla AAD ile tümleşir. Bir AAD yönetici, kuruluşunuzda emin değilseniz, gerekebilir [yeni bir kiracı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) aşağıdaki adımları gerçekleştirmek için.

1. Git [ **uygulama kayıt portalı**](https://apps.dev.microsoft.com) >  **yakınsanmış uygulama** > **bir uygulama ekleyin**.
2. Uygulamanız için bir ad girin ve ardından **oluşturma**. 
3. Yeni kaydettiğiniz uygulamanızı uygulamalarım sayfasında seçin.
4. Uygulama kayıt sayfasında > **platformları** > **eklemek Platform**, seçin **Web API**.
5. Hala uygulama kaydı sayfasında, Git > **Microsoft Graph izinleri** > **Ekle**.
6. Aşağıdaki temsilci izinleri izinleri seçin, ekler ve ardından **Tamam**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph AAD bir REST API'si aracılığıyla programlı erişim sağlayan bir API sağlar. Bu kılavuz için kod örnekleri izinlerini grupları, kullanıcıları ve ilişkileri oluşturmak için Microsoft grafik API'sini çağırmak için kullanır. API'leri daha hızlı filtreleme için önbellek grup kimlikleri için de kullanılır.

## <a name="create-users-and-groups"></a>Kullanıcılar ve gruplar oluşturma

Yerleşik uygulama arama ekliyorsanız, varolan kullanıcı ve grup kimlikleri AAD'de olabilir. Bu durumda, sonraki üç adımı atlayabilirsiniz. 

Ancak, mevcut kullanıcıları yoksa, güvenlik sorumluları oluşturmak için Microsoft Graph API'ları kullanabilirsiniz. Aşağıdaki kod parçacıkları, Azure Search dizininizi güvenlik alan için veri değerleri hale tanımlayıcıları oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Bizim kuramsal üniversitenin erişim uygulamada, bu erişim personeli için güvenlik tanımlayıcıları olacaktır.

Kullanıcı ve grup üyeliği özellikle büyük kuruluşlarda çok esnektir olabilir. Kullanıcı ve grup kimlikleri oluşturan kod sıklıkta kuruluş üyelik değişiklikleri alması çalıştırmanız gerekir. Benzer şekilde, Azure Search dizininizi izin verilen kullanıcıları ve kaynakları geçerli durumunu yansıtacak şekilde benzer bir güncelleştirme zamanlaması gerekir.

### <a name="step-1-create-aad-grouphttpsdevelopermicrosoftcomgraphdocsapi-referencev10apigrouppostgroups"></a>1. adım: Oluşturma [AAD Grup](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/group_post_groups) 
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
   
### <a name="step-2-create-aad-userhttpsdevelopermicrosoftcomgraphdocsapi-referencev10apiuserpostusers"></a>2. adım: Oluşturma [AAD kullanıcısı](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_post_users) 
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

### <a name="step-3-associate-user-and-group"></a>3. adım: kullanıcı ve Grup ilişkilendirme
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>4. adım: grupları tanımlayıcıları önbelleğe alma
İsteğe bağlı olarak, ağ gecikmesini azaltmak için bir arama isteğine verildiğinde grupları önbellekten AAD'ye bir gidiş dönüş kaydetme döndürülen böylece kullanıcı grubu ilişkileri önbelleğe alabilir. (AAD toplu birden çok kullanıcıya sahip tek bir Http isteği göndermek ve önbellek oluşturmak için API) [https://developer.microsoft.com/graph/docs/concepts/json_batching] kullanabilirsiniz.

Microsoft Graph, yüksek hacimli isteklerini işlemek için tasarlanmıştır. Microsoft Graph zorlamayı bir istek sayısı meydana gelirse, HTTP durum kodu 429 isteği başarısız olur. Daha fazla bilgi için bkz: [Microsoft Graph azaltma](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>İzin verilen kullanıcıların gruplarla dizini belgesinde

Azure Search'te sorgu işlemleri Azure Search dizini yürütülür. Bu adımda, bir dizin oluşturma işlemi güvenlik filtre olarak kullanılan tanımlayıcıları dahil olmak üzere bir dizin aranabilir veri aktarır. 

Azure arama değil kullanıcı kimliklerini doğrulamak veya hangi içeriği görüntüleme izni kullanıcı olan kurmak için mantığı sağlar. Kullanım örneği güvenlik kırpma için gizli bir belgeyi olduğu gibi bir arama dizine alınan bu belgeye erişimi olan grup tanımlayıcısı arasındaki ilişkiyi sağlayan varsayar. 

Azure Search dizini PUT istek gövdesinde kuramsal örnekte Başvuranın üniversitenin özellik ya da bu içeriği görüntüleme izni grup tanımlayıcısı ile birlikte dökümü içerir. 

Bu kılavuz için kod örneğinde kullanılan genel örnekte dizin eylem şu şekilde görünebilir:

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

## <a name="issue-a-search-request"></a>Bir arama istek

Güvenlik kırpma amacıyla, statik değerler içerir veya arama sonuçlarında belgeler için kullanılan dizin, güvenlik alanında değerlerdir. Erişim için grup tanımlayıcısı "A11B22C33D44 E55F66G77 H88I99JKK" ise, örneğin, bu tanıtıcıyı Dosyalanan güvenlik sahip Azure Search dizini içinde herhangi bir belgeniz dahil (dışlanan istek sahibine gönderildi arama sonuçlarında veya).

Arama sonuçlarında istek veren kullanıcı gruplarını temel alan döndürdüğü belgeler filtre uygulamak için aşağıdaki adımları gözden geçirin.

### <a name="step-1-retrieve-users-group-identifiers"></a>1. adım: kullanıcının grup kimlikleri alma

Kullanıcı grupları zaten önbelleğe alınmamışsa veya önbellek süresi doldu, sorun [grupları](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/directoryobject_getmembergroups) isteği
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

### <a name="step-2-compose-the-search-request"></a>2. adım: arama isteği oluştur

Kullanıcının grup üyeliğine sahip olduğu varsayılarak, uygun filtre değerleriyle arama isteği verebilir.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>3. adım: sonuçlarını işleme

Yanıt, kullanıcının görüntülemek için izne sahip olan oluşan belgeler, filtrelenmiş bir listesini içerir. Arama sonuçları sayfasını nasıl oluşturmak bağlı olarak, filtrelenmiş sonuç kümesi yansıtmak için görsel ipuçları eklemek isteyebilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu kılavuzda, istek üzerine sağlanan filtre eşleşmiyor belgeleri sonuçlarını kırpma belgeleri Azure arama sonuçlarında, filtre uygulamak için AAD oturum açma işlemleri kullanarak teknikleri öğrendiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search güvenlik kırpma](search-security-trimming-for-azure-search.md)
+ [Azure Search'te filtreleri](search-filters.md)
