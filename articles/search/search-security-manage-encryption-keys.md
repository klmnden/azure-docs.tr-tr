---
title: Bekleyen şifreleme müşteri tarafından yönetilen anahtarları Azure Key Vault'ta (Önizleme) - Azure Search kullanma
description: Dizinleri ve oluşturduğunuz ve Azure anahtar Kasası'nda yönetme anahtarlar aracılığıyla Azure Search'te eş anlamlı eşlemeleri üzerinden ek sunucu tarafı şifreleme.
author: NatiNimni
manager: jlembicz
ms.author: natinimn
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: ''
ms.openlocfilehash: 567f32cba76aaf2d1657b2476c4d11596d44dec5
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753880"
---
# <a name="azure-search-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanarak azure Search şifreleme

> [!Note]
> Müşteri tarafından yönetilen anahtarlarla şifreleme olduğunu Önizleme ve amaçlayan üretim kullanımı için değildir. [2019-05-06-Önizleme REST API sürümü](search-api-preview.md) bu özelliği sağlar. .NET SDK'sı sürüm 8.0 Önizleme de kullanabilirsiniz.
>
> Bu özellik, ücretsiz hizmetler için kullanılabilir değil. Veya 2019-01-01 işleminden sonra oluşturulan bir Faturalanabilir arama hizmeti kullanmanız gerekir. Şu anda portalı desteği yoktur.

Varsayılan olarak, Azure Search rest ile kullanıcı içeriği şifreler [hizmet tarafından yönetilen anahtarlar](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest#data-encryption-models). Varsayılan şifreleme, oluşturduğunuz ve Azure anahtar Kasası'nda yönetme tuşlarını kullanarak bir ek şifreleme katmanı ile destekleyebilirsiniz. Bu makalede adım adım gösterilmektedir.

Sunucu tarafı şifreleme ile tümleştirme aracılığıyla desteklenir [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-overview). Kendi şifreleme anahtarları oluşturmak ve bunları bir anahtar kasasında depolama veya Azure anahtar Kasası'nın API'leri, şifreleme anahtarları oluşturmak için kullanabilirsiniz. Azure Key Vault ile anahtar kullanımını da denetleyebilirsiniz. 

Müşteri tarafından yönetilen anahtarlarla şifreleme, dizin veya eş anlamlı harita düzeyinde nesneleri oluştururken ve arama hizmeti düzeyi üzerinde yapılandırılır. Zaten içeriği şifrelenemiyor. 

Farklı anahtar kasaları farklı anahtarlarından kullanabilirsiniz. Başka bir deyişle, her potansiyel olarak farklı bir müşteri tarafından yönetilen anahtar, müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmemiş indexes\synonym eşlemeleri ile birlikte kullanılarak şifrelenmiş birden çok şifrelenmiş indexes\synonym eşlemesi tek bir arama hizmeti barındırabilir. 

## <a name="prerequisites"></a>Önkoşullar

Bu örnekte aşağıdaki hizmetler kullanılır. 

+ [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

+ [Bir Azure anahtar kasası kaynak oluşturma](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault) veya aboneliğiniz kapsamındaki mevcut bir kasayı bulun.

+ [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) yapılandırma görevleri için kullanılır.

+ [Postman](search-fiddler.md), [Azure PowerShell](search-create-index-rest-api.md) ve [Azure Search SDK'sı](https://aka.ms/search-sdk-preview) Önizleme REST API'sini çağırmak için kullanılabilir. Portalı veya .NET SDK'sı şu anda, müşteri tarafından yönetilen şifreleme desteği yoktur.

## <a name="1---enable-key-recovery"></a>1 - anahtar kurtarma etkinleştirin

Bu adım isteğe bağlıdır ancak uygulanması önemle önerilir. Azure anahtar kasası kaynak oluşturduktan sonra etkinleştirme **geçici silme** ve **temizleme koruma** aşağıdaki PowerShell veya Azure CLI komutları yürüterek seçili anahtar Kasası'nda:   

```powershell
$resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

>[!Note]
> Müşteri tarafından yönetilen anahtarlar özelliğiyle şifreleme çok yapısı nedeniyle, Azure Search, Azure Key vault anahtarı silinirse, verilerinizi almak mümkün olmayacaktır. Key Vault anahtar yanlışlıkla silmeleri kaynaklanan veri kayıplarını önlemek için seçili anahtar kasasında geçici silme ve temizleme korumasını etkinleştirme önerilir. Daha fazla bilgi için [Azure Key Vault geçici silmeyi](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).   

## <a name="2---create-a-new-key"></a>2 - yeni bir anahtar oluşturun

Azure Search içerik şifrelemek için mevcut bir anahtarı kullanıyorsanız, bu adımı atlayın.

1. [Azure portalında oturum açın](https://portal.azure.com) ve anahtar kasası panosuna gidin.

1. Seçin **anahtarları** sol gezinti bölmesinden ayarlama ve tıklayın **+ Oluştur/içeri aktarma**.

1. İçinde **anahtar oluşturma** bölmesinden listesini **seçenekleri**, bir anahtar oluşturmak için kullanmak istediğiniz yöntemi seçin. Şunları yapabilirsiniz **Oluştur** yeni bir anahtar **karşıya** mevcut bir anahtar veya kullanın **geri yedekleme** anahtarının bir yedekleme seçin.

1. Girin bir **adı** anahtarınız için ve isteğe bağlı olarak diğer anahtar Özellikler'i seçin.

1. Tıklayarak **Oluştur** dağıtımı başlatmak için düğme.

Anahtar tanımlayıcısı Not: Bu oluşur **anahtar değeri URI**, **anahtar adı**ve **anahtar sürümü**. Bunlar şifrelenmiş bir dizini Azure Search'te tanımlamak için gerekli olacaktır.
 
![Yeni bir anahtar kasası anahtar oluşturma](./media/search-manage-encryption-keys/create-new-key-vault-key.png "yeni bir anahtar kasası anahtar oluşturun")

## <a name="3---create-a-service-identity"></a>3 - hizmet kimliği oluşturma

Arama hizmetinize bir kimlik atama, arama hizmetiniz için Key Vault erişim izni sağlar. Arama hizmetiniz, Azure Key vault ile kimlik doğrulaması yapmak için kendi kimliğini kullanır.

Azure arama, kimlik atamak için iki yolla destekler: yönetilen bir kimlik ya da dışarıdan yönetilen bir Azure Active Directory uygulaması. 

Mümkünse, yönetilen bir kimlik kullanın. Bu en basit yolu, arama hizmetiniz için bir kimlik atama ve çoğu senaryoda çalışması gerekir. Birden çok anahtar dizinleri ve eş anlamlı sözcük eşlemelerini kullanma veya çözümünüzü kimlik tabanlı kimlik doğrulaması dışlar dağıtılan bir mimaride ise Gelişmiş kullanırsanız [harici olarak yönetilen bir Azure Active Directory yaklaşım](#aad-app)bu makalenin sonunda açıklanmaktadır.

 Genel olarak, arama hizmetiniz Azure anahtar Kasası'na kimlik bilgilerini kod içinde depolamadan kimlik doğrulaması bir yönetilen kimlik sağlar. Bu tür bir yönetilen kimlik yaşam döngüsü, yalnızca tek bir yönetilen kimlik olabilir, arama hizmetinizin yaşam döngüsü için bağlıdır. [Yönetilen kimlikleri hakkında daha fazla bilgi](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

1. Yönetilen bir kimlik oluşturmak üzere [toAzure Portalı'nda oturum](https://portal.azure.com) ve, arama hizmet panosunu açın. 

1. Tıklayın **kimlik** sol gezinti bölmesinde, durumunu değiştir **üzerinde**, tıklatıp **Kaydet**.

![Yönetilen bir kimlik etkinleştirme](./media/search-enable-msi/enable-identity-portal.png "manged kimliğini etkinleştirme")

## <a name="4---grant-key-access-permissions"></a>4 - anahtar erişim izinleri verme

Arama hizmetiniz, Key Vault anahtarınızı kullanmayı etkinleştirmek için arama vermek gerekecektir belirli erişim izinleri hizmet.

Erişim izinlerini belirli bir zamanda iptal edilebilir. İptal sonra bu anahtar kasası kullanan tüm arama hizmet dizini veya eş anlamlı eşlemesi kullanılamaz hale gelir. Anahtar kasası erişim izinlerini daha sonraki bir zamanda geri index\synonym harita erişim geri yükler. Daha fazla bilgi için [güvenli bir anahtar kasasına erişim](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

1. [Azure portalında oturum açın](https://portal.azure.com) ve, anahtar kasası genel bakış sayfasını açın. 

1. Seçin **erişim ilkeleri** sol gezinti bölmesinden ayarlama ve tıklayın **+ yeni Ekle**.

   ![Yeni anahtar kasası erişim ilkesi ekleme](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "yeni anahtar kasası erişim ilkesi ekleme")

1. Tıklayın **Select sorumlusu** ve Azure Search hizmetinizi seçin. Bunun için ada göre veya yönetilen kimlik etkinleştirdikten sonra görüntülenen nesne Kimliğine göre arama yapabilirsiniz.

   ![Select anahtar kasası erişim ilkesi sorumlusu](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Select anahtar kasası erişim ilkesi sorumlusu")

1. Tıklayarak **anahtar izinleri** seçip *alma*, *Unwrap Key* ve *Wrap Key*. Kullanabileceğiniz *Azure Data Lake Storage veya Azure depolama* şablonu hızlı bir şekilde gerekli izinleri seçin.

   Azure arama ile aşağıdakileri verilmelidir [erişim izinlerini](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-operations):

   * *Alma* -ortak anahtarınızı anahtar Kasası'nda bölümlerini almak, arama hizmetinizin sağlar
   * *Anahtarı sarmalama* -iç şifreleme anahtarını korumak için anahtarınızı kullanmak, arama hizmetinizin sağlar
   * *Anahtarı kaydırma* -arama hizmetiniz, iç şifreleme anahtarını açılacak anahtarınızı kullanmayı sağlar.

   ![Anahtar kasası erişim ilkesi anahtar izinleri seçin](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "anahtar kasası erişim ilkesi anahtar izinleri seçin")

1. Tıklayın **Tamam** ve **Kaydet** erişim ilkesi değişikliklerinin.

> [!Important]
> Azure Search'te şifrelenmiş içerik, belirli bir Azure Key Vault anahtarı ile belirli bir kullanacak şekilde yapılandırıldığını **sürüm**. Yeni key\version kullanmak için anahtar veya sürüm değiştirirseniz, dizin veya eş anlamlı eşlemi güncelleştirilmelidir **önce** önceki key\version siliniyor. Bunu yapmak başarısız olan dizin işlenir veya eş anlamlı kullanılamaz harita, sizin hakkınızda anahtar erişimi kayıp olduğunda içeriğin şifresini çözmek mümkün olmayacaktır.   

## <a name="5---encrypt-content"></a>5 - içeriğini şifreleyin

Müşteri tarafından yönetilen anahtarla şifrelenmiş bir dizin veya eş anlamlı eşlemi oluşturma henüz Azure portalını kullanarak mümkün değildir. Bir tür dizini veya eş anlamlı eşlemi oluşturmak için Azure Search REST API'sini kullanın.

Dizin hem de eş anlamlı eşleme desteği yeni bir üst düzey **encryptionKey** anahtarın belirtilmesi için kullanılan özellik. 

Kullanarak **anahtar kasası Uri'si**, **anahtar adı** ve **anahtar sürümü** oluşturabiliriz Key vault anahtarı bir **encryptionKey** tanımı:

```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
> [!Note] 
> Bu key vault ayrıntılarını hiçbiri, gizli olarak kabul edilir ve Azure portalında ilgili Azure Key Vault anahtar sayfasına göz atarak kolayca alınamadı.

Yönetilen bir kimlik kullanmak yerine, Key Vault kimlik doğrulaması için bir AAD uygulaması kullanıyorsanız, AAD uygulaması Ekle **erişim kimlik bilgilerini** şifreleme anahtarınızın için: 
```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660",
    "accessCredentials": {
      "applicationId": "00000000-0000-0000-0000-000000000000",
      "applicationSecret": "myApplicationSecret"
    }
  }
}
```

## <a name="example-index-encryption"></a>Örnek: Dizin şifreleme
REST API aracılığıyla yeni bir dizin oluşturma ayrıntılarını konumunda bulunamadı [dizin oluşturma (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-index), burada tek fark dizin tanımını bir parçası olarak şifreleme anahtar ayrıntıları burada belirtilmesidir: 

```json
{
 "name": "hotels",  
 "fields": [
  {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
  {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
  {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
  {"name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
  {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
  {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
  {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
  {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true},
 ],
 "encryptionKey": {
   "keyVaultUri": "https://demokeyvault.vault.azure.net",
   "keyVaultKeyName": "myEncryptionKey",
   "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
 }
}
```
Şimdi dizin oluşturma isteği gönderin ve ardından dizin normal olarak kullanmaya başlayın.

## <a name="example-synonym-map-encryption"></a>Örnek: Eş anlamlı eşlemi şifreleme

REST API aracılığıyla yeni bir eş anlamlı eşlemi oluşturma ayrıntıları şu yolda bulunabilir: [eş anlamlı eşlemi oluşturma (Azure Search Hizmeti REST API'si)](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map), burada tek fark eş anlamlı eşlemi tanımının bir parçası şifreleme anahtar ayrıntıları burada belirtilmesidir: 

```json
{   
  "name" : "synonymmap1",  
  "format" : "solr",  
  "synonyms" : "United States, United States of America, USA\n
  Washington, Wash. => WA",
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
Şimdi eş anlamlı eşlemi oluşturma isteği gönderin ve daha sonra normal olarak kullanmaya başlayın.

>[!Important] 
> Sırada **encryptionKey** için var olan Azure Search dizinlerini eklenemez veya eş anlamlı eşler, üç key vault ayrıntılarını (örneğin, anahtar sürümü güncelleştirme) hiçbiri için farklı değerler sağlayarak güncelleştirilebilir. Yeni bir Key Vault anahtarı veya yeni bir anahtar sürümü değiştirirken, anahtarı kullanan tüm Azure Search dizini veya eş anlamlı harita ilk yeni key\version kullanacak şekilde güncelleştirilmesi gerekir **önce** önceki key\version siliniyor. Bunu yapmak başarısız olan dizin işlenir veya içerik kez anahtar erişimi şifresini çözmek mümkün olmayacaktır olarak kullanılamaz, eş anlamlı eşlemi kaybolur.   
> Anahtar kasası erişim izinlerini daha sonraki bir zamanda geri içerik erişimi geri yükler.

## <a name="aad-app"></a> Gelişmiş: Harici olarak yönetilen bir Azure Active Directory uygulama kullanma

Ne zaman bir yönetilen kimlik, bir Azure Active Directory uygulaması ile bir güvenlik sorumlusu Azure Search hizmetiniz için oluşturabileceğiniz mümkün değildir. Özellikle, bir yönetilen kimlik Bu koşullar altında uygun değil:

* (Örneğin, arama hizmeti Azure anahtar Kasası ' dan farklı bir Active Directory kiracısında değilse) arama doğrudan Key vault hizmetinin erişim izni verilemiyor.

* Her yere her anahtar kasasını kullanmanız gerekir farklı bir anahtar Kasası'ndaki farklı bir anahtar kullanarak birden çok şifrelenmiş indexes\synonym eşlemesi barındırmak için gerekli tek arama hizmetidir **farklı bir kimlik** kimlik doğrulaması için. Farklı anahtar kasalarını yönetmesi için farklı bir kimlik kullanarak bir gereksinim değildir, yukarıdaki yönetilen kimlik seçeneğini kullanmayı düşünün.  

Bu tür topolojileri uyum sağlamak için Azure Key Vault ve arama hizmeti arasında kimlik doğrulaması için Azure Active Directory (AAD) uygulamaları kullanılmasını destekler arayın.    
Portalda bir AAD uygulaması oluşturmak için:

1. [Azure Active Directory uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

1. [Uygulama kimliği ve kimlik doğrulama anahtarını alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) gibi bu şifrelenmiş bir dizin oluşturma için gerekli olacaktır. Değerler sağlamanız gerekiyor **uygulama kimliği** ve **kimlik doğrulama anahtarı**.

>[!Important]
> Yerine yönetilen bir kimlik doğrulamasının bir AAD uygulaması kullanmaya karar verirken, Azure Search, AAD uygulaması sizin adınıza Yönetme yetkisine sahip değil ve AAD uygulamanızı gibi dönemsel rotasyonunu yönetmek için en fazla olduğu gerçeği göz önünde bulundurun. Uygulama kimlik doğrulama anahtarı.
> AAD uygulaması veya kendi kimlik doğrulama anahtarı değiştirilirken, uygulamanın kullandığı tüm Azure Search dizini veya eş anlamlı harita ilk yeni uygulamayı ID\key güncelleştirilmelidir **önce** önceki uygulama silindiğinde veya kendi yetkilendirme anahtarı ve, anahtar kasası erişimi iptal etme önce.
> Bunu yapmak başarısız olan dizin işlenir veya içerik kez anahtar erişimi şifresini çözmek mümkün olmayacaktır olarak kullanılamaz, eş anlamlı eşlemi kaybolur.   

## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik mimarisi ile alışkın değilseniz, gözden [Azure güvenlik belgeleri](https://docs.microsoft.com/azure/security/)ve bu makalede özellikle:

> [!div class="nextstepaction"]
> [Bekleyen veri şifreleme](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
