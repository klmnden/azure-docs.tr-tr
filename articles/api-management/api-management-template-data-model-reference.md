---
title: Azure API Management şablonu veri modeli başvurusu | Microsoft Docs
description: Varlık ve türü temsiller için Azure API Management'ta Geliştirici portal şablonları için veri modellerinde kullanılan ortak öğeler hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 8c21ed737cab98c9136e1c1991997ff3931a4c9d
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447206"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management şablonu veri modeli başvurusu
Bu konu içinde veri modellerini Azure API Management'ta Geliştirici portal şablonları için kullanılan ortak öğeler için varlık ve türü gösterimleri açıklar.  
  
 Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

Geliştirici Portalı tüketim katmanında kullanılamıyor.

## <a name="reference"></a>Başvuru

-   [API](#API)  
-   [API özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Eki](#Attachment)  
-   [Kod örneği](#Sample)  
-   [Açıklama](#Comment)  
-   [Filtreleme](#Filtering)  
-   [Üst bilgi](#Header)  
-   [HTTP isteği](#HTTPRequest)  
-   [HTTP yanıtı](#HTTPResponse)  
-   [Sorunu](#Issue)  
-   [İşlem](#Operation)  
-   [İşlem menüsü](#Menu)  
-   [İşlem menü öğesi](#MenuItem)  
-   [Disk belleği](#Paging)  
-   [Parametre](#Parameter)  
-   [Ürün](#Product)  
-   [Sağlayıcı](#Provider)  
-   [Temsili](#Representation)  
-   [Abonelik](#Subscription)  
-   [Abonelik özeti](#SubscriptionSummary)  
-   [Kullanıcı hesabı bilgileri](#UserAccountInfo)  
-   [Kullanıcı oturum açma](#UseSignIn)  
-   [Kullanıcı kaydı](#UserSignUp)  
  
##  <a name="API"></a> API  
 `API` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde API'yi benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` API tanımlayıcıdır. Bu özellik salt okunurdur.|  
|ad|dize|API adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|açıklama|dize|API tanımı. Boş olmamalıdır. HTML etiketleri biçimlendirme içerebilir. En fazla 1000 karakter olabilir.|  
|ServiceUrl|dize|Bu API'yi uygulayan arka uç hizmetine mutlak URL'si.|  
|yol|dize|Bu API ve API Management hizmet örneği içinde kendi kaynak yolları tanıtan göreli URL'si. Bu API için genel bir URL oluşturmak için hizmet örneği oluşturma sırasında belirtilen API uç noktası temel URL'sine eklenir.|  
|Protokolleri|sayı dizisi|Bu API işlemlerinde üzerinde hangi protokollerin çağrılabilir açıklar. İzin verilen değerler `1 - http` ve `2 - https`, veya her ikisini de.|  
|authenticationSettings|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API içinde bulunan kimlik doğrulama ayarları koleksiyonu.|  
|subscriptionKeyParameterNames|object|Abonelik anahtarını içeren üst bilgi ve/veya sorgu parametreleri için özel bir ad belirtmek için kullanılan isteğe bağlı özellik. Bu özellik, mevcut olduğunda, aşağıdaki iki özelliği en az birini içermelidir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> API özeti  
 `API summary` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği içinde API'yi benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` API tanımlayıcıdır. Bu özellik salt okunurdur.|  
|ad|dize|API adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|açıklama|dize|API tanımı. Boş olmamalıdır. HTML etiketleri biçimlendirme içerebilir. En fazla 1000 karakter olabilir.|  
  
##  <a name="Application"></a> Uygulama  
 `application` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|Uygulamanın benzersiz tanımlayıcısı.|  
|Unvan|dize|Uygulama Başlığı.|  
|Açıklama|dize|Uygulama açıklaması.|  
|Url|URI|Uygulama için URI.|  
|Sürüm|dize|Uygulama için sürüm bilgileri.|  
|Gereksinimler|dize|Uygulama için gereksinimleri açıklaması.|  
|Durum|number|Uygulamanın geçerli durumu.<br /><br /> -0 - kayıtlı<br /><br /> -1 - gönderildi<br /><br /> -Yayımlanan 2-<br /><br /> -Reddedilen 3-<br /><br /> -4 - yayımdan kaldırıldı|  
|RegistrationDate|DateTime|Tarih ve saat uygulama kaydedildi.|  
|CategoryID|number|Kategori uygulamanın (Finans, eğlence, vs.)|  
|DeveloperId|dize|Uygulama gönderilen geliştiricisinin benzersiz tanımlayıcısı.|  
|Ekler|Koleksiyonu [Eki](#Attachment) varlıklar.|Uygulama ekran görüntüleri veya simgeler gibi tüm ekleri.|  
|Simge|[Eki](#Attachment)|Simge uygulama için.|  
  
##  <a name="Attachment"></a> Eki  
 `attachment` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|UniqueId|dize|Ek için benzersiz tanımlayıcısı.|  
|Url|dize|Kaynak URL'si.|  
|Tür|dize|Ek türü.|  
|ContentType|dize|Ek medya türü.|  
  
##  <a name="Sample"></a> Kod örneği  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|başlık|dize|İşlemin adı.|  
|Kod parçacığı|dize|Bu özellik, kullanım dışıdır ve kullanılmamalıdır.|  
|Fırça|dize|Hangi kod renklendirme kodu örneği görüntülenirken kullanılacak şablon söz dizimi. İzin verilen değerler `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, ve `csharp`.|  
|şablon|dize|Bu kod örneği şablonunun adı.|  
|body|dize|Kod parçacığını kod örnek bölümü için yer tutucu.|  
|method|dize|İşlemin HTTP yöntemi.|  
|Düzeni|dize|İşlem isteği için kullanılacak protokolü.|  
|yol|dize|İşlem yolu.|  
|sorgu|dize|Sorgu dizesi örneği tanımlanan parametrelere sahip.|  
|konak|dize|Bu işlem içeren bir API için API Management hizmet ağ geçidi URL'si.|  
|Üst bilgileri|Koleksiyonu [üstbilgi](#Header) varlıklar.|Bu işlem için üstbilgiler.|  
|parametreler|Koleksiyonu [parametre](#Parameter) varlıklar.|Bu işlem için tanımlanan parametreler.|  
  
##  <a name="Comment"></a> Açıklama  
 `API` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|number|Açıklama kimliği.|  
|CommentText|dize|Yorumun gövdesi. HTML içerebilir.|  
|DeveloperCompany|dize|Geliştirici şirket adı.|  
|PostedOn|DateTime|Tarih ve saat yorum gönderildi.|  
  
##  <a name="Issue"></a> Sorunu  
 `issue` Varlık aşağıdaki özelliklere sahiptir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|Sorunun benzersiz tanımlayıcısı.|  
|ApiID|dize|Kendisi için bu sorunu bildiren API kimliği.|  
|Unvan|dize|Sorun başlığı.|  
|Açıklama|dize|Sorunun açıklaması.|  
|SubscriptionDeveloperName|dize|Sorunu bildiren Geliştirici adı.|  
|IssueState|dize|Sorunun geçerli durumu. Olası değerler şunlardır: Önerilen, açık, kapalı.|  
|ReportedOn|DateTime|Tarih ve saat sorun bildirildi.|  
|Yorumlar|Koleksiyonu [yorum](#Comment) varlıklar.|Bu konu hakkında açıklamalar.|  
|Ekler|Koleksiyonu [Eki](api-management-template-data-model-reference.md#Attachment) varlıklar.|Sorun tüm ekler.|  
|Hizmetler|Koleksiyonu [API](#API) varlıklar.|Dosyalanmış sorun kullanıcı tarafından abone API'leri.|  
  
##  <a name="Filtering"></a> Filtreleme  
 `filtering` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Desen|dize|Geçerli arama terimi; veya `null` arama terimi yok ise.|  
|Yer tutucu|dize|Arama kutusuna arama terimi yok olduğunda görüntülenecek metin.|  
  
##  <a name="Header"></a> Üst bilgi  
 Bu bölümde açıklanmaktadır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|dize|Parametre adı.|  
|açıklama|dize|Parametre açıklaması.|  
|değer|dize|Üstbilgi değeri.|  
|typeName|dize|Üst bilgi değeri veri türü.|  
|seçenekler|dize|Seçenekler.|  
|gerekli|boole|Üst bilgi gerekli olup olmadığı.|  
|Salt okunur|boole|Üst bilgisi salt okunur olup olmadığı.|  
  
##  <a name="HTTPRequest"></a> HTTP isteği  
 Bu bölümde açıklanmaktadır `request` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|açıklama|dize|İşlem istek açıklaması.|  
|Üst bilgileri|dizi [üstbilgi](#Header) varlıklar.|İstek üst bilgileri.|  
|parametreler|dizi [parametresi](#Parameter)|İşlem istek parametreleri koleksiyonu.|  
|İfadeleri|dizi [gösterimi](#Representation)|İşlem istek sunumlarını koleksiyonu.|  
  
##  <a name="HTTPResponse"></a> HTTP yanıtı  
 Bu bölümde açıklanmaktadır `response` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|statusCode|pozitif bir tamsayı|İşlem yanıt durum kodu.|  
|açıklama|dize|İşlem yanıt açıklaması.|  
|İfadeleri|dizi [gösterimi](#Representation)|İşlem yanıt gösterimleri koleksiyonu.|  
  
##  <a name="Operation"></a> İşlemi  
 `operation` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|dize|Kaynak tanımlayıcısı. İşlemi geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{aid}/operations/{id}` burada `{aid}` API tanımlayıcısıdır ve `{id}` işlemi tanımlayıcıdır. Bu özellik salt okunurdur.|  
|ad|dize|İşlemin adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|açıklama|dize|İşlem açıklaması. Boş olmamalıdır. HTML etiketleri biçimlendirme içerebilir. En fazla 1000 karakter olabilir.|  
|Düzeni|dize|Bu API işlemlerinde üzerinde hangi protokollerin çağrılabilir açıklar. İzin verilen değerler `http`, `https`, veya her ikisini de `http` ve `https`.|  
|UriTemplate|dize|Göreli URL şablonu bu işlem için hedef kaynak tanımlama. Parametreler içerebilir. Örnek: `customers/{cid}/orders/{oid}/?date={date}`|  
|konak|dize|API'sini barındıran API Management ağ geçidi URL'si.|  
|HttpMethod|dize|İşlem HTTP yöntemi.|  
|istek|[HTTP isteği](#HTTPRequest)|İstek ayrıntılarını içeren bir varlık.|  
|Yanıtları|dizi [HTTP yanıtı](#HTTPResponse)|İşlemi bir dizi [HTTP yanıtı](#HTTPResponse) varlıklar.|  
  
##  <a name="Menu"></a> İşlem menüsü  
 `operation menu` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|apiId|dize|Geçerli API kimliği.|  
|CurrentOperationId|dize|Geçerli işlemin kimliği.|  
|Eylem|dize|Menü türü.|  
|MenuItems|Koleksiyonu [işlemi menü öğesi](#MenuItem) varlıklar.|Geçerli API için işlemler.|  
  
##  <a name="MenuItem"></a> İşlem menü öğesi  
 `operation menu item` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|İşlem kimliği.|  
|Unvan|dize|İşlem açıklaması.|  
|HttpMethod|dize|İşlemin Http yöntemi.|  
  
##  <a name="Paging"></a> Disk belleği  
 `paging` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfa|number|Geçerli sayfa numarası.|  
|PageSize|number|Tek bir sayfada görüntülenecek en yüksek sonuç sayısı.|  
|TotalItemCount|number|Görüntülenecek öğe sayısı.|  
|ShowAll|boole|Tek bir sayfada tüm sonuçları göster tutulmayacağı.|  
|PageCount|number|Sayfa sonuçlarının sayısı.|  
  
##  <a name="Parameter"></a> Parametre  
 Bu bölümde açıklanmaktadır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|dize|Parametre adı.|  
|açıklama|dize|Parametre açıklaması.|  
|değer|dize|Parametre değeri.|  
|seçenekler|dize dizisi|Sorgu parametresi değerleri için tanımlanan değerleri.|  
|gerekli|boole|Parametre gerekli olup olmadığını belirtir.|  
|tür|number|Bu parametre bir yol parametresi (1) veya bir sorgu dizesi parametresini (2) olup olmadığı.|  
|typeName|dize|Parametre türü.|  
  
##  <a name="Product"></a> Ürün  
 `product` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|Kaynak tanımlayıcısı. Ürününün geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` ürün tanımlayıcısı. Bu özellik salt okunurdur.|  
|Unvan|dize|Ürün adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|Açıklama|dize|Ürün açıklaması. Boş olmamalıdır. HTML etiketleri biçimlendirme içerebilir. En fazla 1000 karakter olabilir.|  
|Koşullar|dize|Ürün koşulları kullanın. Geliştiriciler ürüne abone olunmaya çalışılırken sunulan ve abonelik işlemi uygulayabilmeniz için önce bu koşulları kabul etmeniz gerekir.|  
|ProductState|number|Ürün veya yayımlanan belirtir. Yayımlanan ürünleri tarafından geliştiriciler Geliştirici portalında bulunabilir. Ürünleri olmayan yayımlanan yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilen değerler şunlardır:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|boole|Bir kullanıcı, bu ürün için birden çok abonelik aynı anda sahip olup olmadığını belirtir.|  
|MultipleSubscriptionsCount|number|Bu ürün için abonelik maksimum sayısı, bir kullanıcı, aynı anda sahip izin verilmez.|  
  
##  <a name="Provider"></a> Sağlayıcı  
 `provider` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Özellikler|dize sözlüğü|Bu kimlik doğrulama sağlayıcısı için özellikleri.|  
|AuthenticationType|dize|Sağlayıcı türü. (Azure Active Directory, Facebook oturum açma, Microsoft Account, Twitter, Google hesabınızı).|  
|Altyazı|dize|Sağlayıcının görünen adı.|  
  
##  <a name="Representation"></a> Temsili  
 Bu bölümde açıklanmaktadır bir `representation`.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|contentType|dize|Örneğin, kayıtlı veya özel bir içerik türü için bu gösterim belirtir `application/xml`.|  
|Örnek|dize|Gösterim örneği.|  
  
##  <a name="Subscription"></a> Abonelik  
 `subscription` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği bir abonelikte benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|Ürün kimliği|dize|Abone olunan ürünün ürün kaynak tanımlayıcısı. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` ürün tanımlayıcısı.|  
|ProductTitle|dize|Ürün adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|ProductDescription|dize|Ürün açıklaması. Boş olmamalıdır. HTML etiketleri biçimlendirme içerebilir. En fazla 1000 karakter olabilir.|  
|ProductDetailsUrl|dize|Ürün Ayrıntıları göreli URL'si.|  
|durum|dize|Abonelik durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended` – Abonelik engellenir ve abone ürünün herhangi bir API çağrılamaz.<br /><br /> - `1 - active` – Aboneliğinizin etkin olduğunu.<br /><br /> - `2 - expired` – Abonelik ulaştı, sona erme tarihine ve devre dışı bırakıldı.<br /><br /> - `3 - submitted` – abonelik isteğini geliştirici tarafından yapılır, ancak henüz onaylanamıyor veya reddedilemiyor.<br /><br /> - `4 - rejected` – bir yönetici tarafından abonelik isteği reddedildi.<br /><br /> - `5 - cancelled` – Abonelik geliştirici veya yönetici tarafından iptal edildi.|  
|DisplayName|dize|Aboneliğin görünen adı.|  
|CreatedDate|Tarih/saat|Abonelik, ISO 8601 biçiminde oluşturulduğu tarih: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|boole|Abonelik geçerli kullanıcı tarafından iptal edilebilir.|  
|IsAwaitingApproval|boole|Olup abonelik onayı bekliyor.|  
|startDate|Tarih/saat|ISO 8601 biçimli bir abonelik için başlangıç tarihi: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Tarih/saat|Sona erme tarihini ISO 8601 biçimli abonelik: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Tarih/saat|ISO 8601 biçimli bir abonelik için bildirim tarihi: `2014-06-24T16:25:00Z`.|  
|primaryKey|dize|Birincil bir abonelik anahtarı. En fazla uzunluk 256 karakterdir.|  
|ikincil anahtarı oluşturma|dize|İkincil bir abonelik anahtarı. En fazla uzunluk 256 karakterdir.|  
|CanBeRenewed|boole|Abonelik, geçerli kullanıcı tarafından mı yenilenebilir.|  
|HasExpired|boole|Abonelik süresi doldu.|  
|IsRejected|boole|Abonelik isteği olup olmadığını reddedildi.|  
|CancelUrl|dize|Aboneliği iptal etmek için göreli URL'si.|  
|RenewUrl|dize|Aboneliğinizi yenilemek için göreli URL'si.|  
  
##  <a name="SubscriptionSummary"></a> Abonelik özeti  
 `subscription summary` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|dize|Kaynak tanımlayıcısı. Geçerli API Management hizmet örneği bir abonelikte benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|DisplayName|dize|Aboneliğin görünen adı|  
  
##  <a name="UserAccountInfo"></a> Kullanıcı hesabı bilgileri  
 `user account info` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|FirstName|dize|İlk adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|LastName|dize|Soyadı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|Email|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|dize|Kullanıcı hesabı parolası.|  
|NameIdentifier|dize|Tanımlayıcı, aynı kullanıcı e-posta hesabı.|  
|ProviderName|dize|Kimlik doğrulama sağlayıcısının adı.|  
|IsBasicAccount|boole|Bu hesabın e-posta ve parola kullanarak kaydolduysanız true; hesap bir sağlayıcı kullanarak kaydolduysanız false.|  
  
##  <a name="UseSignIn"></a> Kullanıcı oturum açma  
 `user sign in` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Email|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|dize|Kullanıcı hesabı parolası.|  
|ReturnUrl|dize|Kullanıcı tıklattığınız sayfasının URL'sini oturum açın.|  
|RememberMe|boole|Geçerli kullanıcının bilgileri kaydedilip kaydedilmeyeceğini belirtir.|  
|RegistrationEnabled|boole|Kayıt etkinleştirilip etkinleştirilmediği.|  
|DelegationEnabled|boole|Temsilci oturum açma etkinleştirilip etkinleştirilmediği.|  
|DelegationUrl|dize|Temsilci oturum açma etkinse URL'si.|  
|SsoSignUpUrl|dize|Varsa kullanıcı için çoklu oturum açma URL'si.|  
|AuxServiceUrl|dize|Geçerli kullanıcı bir yöneticiyse, Azure portalında hizmet örneği için bir bağlantı budur.|  
|Sağlayıcılar|Koleksiyonu [sağlayıcısı](#Provider) varlıklar|Bu kullanıcı için kimlik doğrulama sağlayıcıları.|  
|UserRegistrationTerms|dize|Bir kullanıcının oturum açmadan önce kabul etmesi gereken koşulları.|  
|UserRegistrationTermsEnabled|boole|Koşulları etkinleştirilip etkinleştirilmediği.|  
  
##  <a name="UserSignUp"></a> Kullanıcı Kaydolma  
 `user sign up` Varlık aşağıdaki özelliklere sahiptir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|PasswordConfirm|boole|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi.|  
|Parola|dize|Kullanıcı hesabı parolası.|  
|PasswordVerdictLevel|number|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi.|  
|UserRegistrationTerms|dize|Bir kullanıcının oturum açmadan önce kabul etmesi gereken koşulları.|  
|UserRegistrationTermsOptions|number|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi.|  
|ConsentAccepted|boole|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi.|  
|Email|dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|FirstName|dize|İlk adı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|LastName|dize|Soyadı. Boş olmamalıdır. En fazla 100 karakterdir.|  
|userData|dize|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up) denetimi.|  
|NameIdentifier|dize|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kayıt denetimi.|  
|ProviderName|dize|Kimlik doğrulama sağlayıcısının adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).
