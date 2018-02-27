---
title: "Azure API Management şablonu veri modeli başvurusu | Microsoft Docs"
description: "Azure API Management'ta Geliştirici Portalı şablonları için veri modellerinde kullanılan ortak öğeler için varlık ve türü ifadeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 0f27b6b529c2591e37d48e3386190077fc8efc32
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management şablonu veri modeli başvurusu
Bu konuda veri modellerinde Azure API Management'ta Geliştirici Portalı şablonları için kullanılan ortak öğeler için varlık ve türü Beyanları açıklanmaktadır.  
  
 Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [API](#API)  
-   [API özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Eki](#Attachment)  
-   [kod örneği](#Sample)  
-   [Açıklama](#Comment)  
-   [Filtreleme](#Filtering)  
-   [Üstbilgi](#Header)  
-   [HTTP isteği](#HTTPRequest)  
-   [HTTP yanıtı](#HTTPResponse)  
-   [Sorunu](#Issue)  
-   [İşlem](#Operation)  
-   [İşlemi menüsü](#Menu)  
-   [İşlemi menü öğesi](#MenuItem)  
-   [Disk belleği](#Paging)  
-   [Parametre](#Parameter)  
-   [Ürün](#Product)  
-   [Sağlayıcı](#Provider)  
-   [Gösterimi](#Representation)  
-   [Abonelik](#Subscription)  
-   [Abonelik özeti](#SubscriptionSummary)  
-   [Kullanıcı hesabı bilgileri](#UserAccountInfo)  
-   [Kullanıcı oturum açma](#UseSignIn)  
-   [Kullanıcı Kaydolma](#UserSignUp)  
  
##  <a name="API"></a> API  
 `API` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|string|Kaynak tanımlayıcısı. API geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` bir API tanımlayıcısıdır. Bu özellik salt okunurdur.|  
|ad|string|API adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|string|API açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|serviceUrl|string|Bu API'yi uygulayan arka uç hizmetine mutlak URL'si.|  
|yol|string|Bu API ve tüm API Management hizmet örneği içinde kendi kaynak yolları tanıtan göreli URL'si. Bu API için genel bir URL oluşturmak için hizmet örneği oluşturma sırasında belirtilen API uç noktası temel URL'sini eklenir.|  
|protokolleri|dizi numarası|Bu API işlemlerinde hangi protokollerine çağrılabilir açıklar. İzin verilen değerler `1 - http` ve `2 - https`, veya her ikisini de.|  
|authenticationSettings|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API içinde bulunan kimlik doğrulama ayarları koleksiyonu.|  
|subscriptionKeyParameterNames|nesne|Abonelik anahtarı içeren sorgu ve/veya üstbilgi parametreleri için özel adların belirtmek için kullanılan isteğe bağlı özellik. Bu özellik varsa, iki aşağıdaki özellikleri en az birini içermelidir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> API özeti  
 `API summary` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|string|Kaynak tanımlayıcısı. API geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` bir API tanımlayıcısıdır. Bu özellik salt okunurdur.|  
|ad|string|API adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|string|API açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
  
##  <a name="Application"></a> Uygulama  
 `application` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|Uygulamanın benzersiz tanımlayıcısı.|  
|Başlık|string|Uygulama Başlığı.|  
|Açıklama|string|Uygulama açıklaması.|  
|Url|URI|Uygulama için URI.|  
|Sürüm|string|Uygulama için sürüm bilgisi.|  
|Gereksinimler|string|Uygulama için gereksinimleri açıklaması.|  
|Durum|numarası|Uygulama geçerli durumu.<br /><br /> -0 - kayıtlı<br /><br /> -Gönderilen 1-<br /><br /> -Yayımlanan 2-<br /><br /> -Reddedilen 3-<br /><br /> -4 - yayımlanmamış|  
|RegistrationDate|DateTime|Tarih ve saat uygulama kaydedildi.|  
|Adlı kullanıcı, Categoryıd'si|numarası|Kategori uygulamanın (Finans, eğlence, vs.)|  
|DeveloperId|string|Uygulama gönderilen Geliştirici benzersiz tanımlayıcısı.|  
|Ekler|Koleksiyonu [ek](#Attachment) varlıklar.|Herhangi bir ek ekran görüntüleri veya simgeler gibi uygulama için.|  
|Simge|[Eki](#Attachment)|Simge uygulama için.|  
  
##  <a name="Attachment"></a> Eki  
 `attachment` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|UniqueId|string|Eki için benzersiz tanımlayıcı.|  
|Url|string|Kaynak URL.|  
|Tür|string|Ek türü.|  
|ContentType|string|Ek medya türü.|  
  
##  <a name="Sample"></a> kod örneği  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|başlık|string|İşlemin adı.|  
|snippet|string|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|Fırça|string|Kod örneği görüntülenirken kullanılacak şablon renklendirme hangi kod sözdizimi. İzin verilen değerler `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, ve `csharp`.|  
|şablon|string|Bu kod örneği şablonunun adı.|  
|body|string|Kod parçacığını kod örnek kısmı için bir yer tutucu.|  
|yöntem|string|İşlemi HTTP yöntemi.|  
|Düzeni|string|İşlem isteği için kullanılacak protokolü.|  
|yol|string|İşlemi yolu.|  
|sorgu|string|Sorgu dizesi örnek tanımlanan parametrelere sahip.|  
|konak|string|Bu işlem içeriyor API için API Management hizmeti ağ geçidi URL'si.|  
|headers|Koleksiyonu [üstbilgi](#Header) varlıklar.|Bu işlem için üstbilgiler.|  
|parametreler|Koleksiyonu [parametresi](#Parameter) varlıklar.|Bu işlem için tanımlanan parametreleri.|  
  
##  <a name="Comment"></a> Açıklama  
 `API` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|numarası|Açıklama kimliği.|  
|CommentText|string|Açıklama gövdesi. HTML içerebilir.|  
|DeveloperCompany|string|Geliştirici şirket adı.|  
|PostedOn|DateTime|Tarih ve saat yorum kopyalanmışsa.|  
  
##  <a name="Issue"></a> Sorunu  
 `issue` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|Sorun için benzersiz tanımlayıcı.|  
|ApiID|string|Bu sorun bildirildi API kimliği.|  
|Başlık|string|Sorun başlığı.|  
|Açıklama|string|Sorun açıklaması.|  
|SubscriptionDeveloperName|string|Sorunu bildiren Geliştirici adı.|  
|IssueState|string|Sorunu geçerli durumu. Olası değerler şunlardır: Önerilen, açık, kapalı.|  
|ReportedOn|DateTime|Tarih ve saat sorun bildirildi.|  
|Yorumlar|Koleksiyonu [açıklama](#Comment) varlıklar.|Bu konu hakkında açıklamalar.|  
|Ekler|Koleksiyonu [ek](api-management-template-data-model-reference.md#Attachment) varlıklar.|Herhangi bir ek verilecek.|  
|Hizmetler|Koleksiyonu [API](#API) varlıklar.|Sorunu Dosyalanan kullanıcı tarafından abone API'leri.|  
  
##  <a name="Filtering"></a> Filtreleme  
 `filtering` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Desen|string|Geçerli arama terimi; veya `null` arama terimi yok ise.|  
|Yer tutucu|string|Belirtilen arama terimi yok olduğunda arama kutusuna görüntülenecek metin.|  
  
##  <a name="Header"></a> Üstbilgi  
 Bu bölümde açıklanmıştır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|string|Parametre adı.|  
|açıklama|string|Parametre açıklaması.|  
|değer|string|Üstbilgi değeri.|  
|typeName|string|Üstbilgi değeri veri türü.|  
|seçenekler|string|Seçenekler.|  
|Gerekli|boole|Üstbilgi gerekli olup olmadığı.|  
|readOnly|boole|Üstbilgi salt okunur olup olmadığı.|  
  
##  <a name="HTTPRequest"></a> HTTP isteği  
 Bu bölümde açıklanmıştır `request` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|açıklama|string|İşlem isteği açıklaması.|  
|headers|dizi [üstbilgi](#Header) varlıklar.|İstek üstbilgileri.|  
|parametreler|dizi [parametresi](#Parameter)|İşlem istek parametreleri topluluğu.|  
|temsili|dizi [gösterimi](#Representation)|İşlem istek sunumlarını koleksiyonu.|  
  
##  <a name="HTTPResponse"></a> HTTP yanıtı  
 Bu bölümde açıklanmıştır `response` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|statusCode|Pozitif tamsayı|İşlem yanıt durum kodu.|  
|açıklama|string|İşlem yanıt açıklaması.|  
|temsili|dizi [gösterimi](#Representation)|İşlem yanıt Beyanları koleksiyonu.|  
  
##  <a name="Operation"></a> işlemi  
 `operation` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|string|Kaynak tanımlayıcısı. İşlemi geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{aid}/operations/{id}` nerede `{aid}` bir API tanımlayıcısıdır ve `{id}` işlemi tanımlayıcıdır. Bu özellik salt okunurdur.|  
|ad|string|İşlemin adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|string|İşlem açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|Düzeni|string|Bu API işlemlerinde hangi protokollerine çağrılabilir açıklar. İzin verilen değerler `http`, `https`, veya her ikisini de `http` ve `https`.|  
|uriTemplate|string|Bu işlem için hedef kaynak tanımlayan göreli URL şablonu. Parametreler içerebilir. Örnek: `customers/{cid}/orders/{oid}/?date={date}`|  
|konak|string|API barındıran API Yönetimi ağ geçidi URL'si.|  
|HttpMethod|string|İşlemi HTTP yöntemi.|  
|İstek|[HTTP isteği](#HTTPRequest)|İstek ayrıntıları içeren bir varlık.|  
|yanıtları|dizi [HTTP yanıtı](#HTTPResponse)|İşlem dizisi [HTTP yanıtı](#HTTPResponse) varlıklar.|  
  
##  <a name="Menu"></a> İşlemi menüsü  
 `operation menu` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|ApiId|string|Geçerli API kimliği.|  
|CurrentOperationId|string|Geçerli işlem kimliği.|  
|Eylem|string|Menü türü.|  
|MenuItems|Koleksiyonu [işlemi menü öğesi](#MenuItem) varlıklar.|Geçerli API işlemleri.|  
  
##  <a name="MenuItem"></a> İşlemi menü öğesi  
 `operation menu item` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|İşlem kimliği.|  
|Başlık|string|İşlem açıklaması.|  
|HttpMethod|string|İşlemi Http yöntemi.|  
  
##  <a name="Paging"></a> Disk belleği  
 `paging` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfa|numarası|Geçerli sayfa numarası.|  
|PageSize|numarası|Tek bir sayfada gösterilecek en fazla sonuç.|  
|TotalItemCount|numarası|Görüntülenecek öğe sayısı.|  
|ShowAll|boole|Kullanılıp kullanılmayacağını tüm sonuçları tek bir sayfada göster.|  
|PageCount|numarası|Sayfa sonuç sayısı.|  
  
##  <a name="Parameter">Parametre</a>  
 Bu bölümde açıklanmıştır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|string|Parametre adı.|  
|açıklama|string|Parametre açıklaması.|  
|değer|string|Parametre değeri.|  
|seçenekler|Dize dizisi|Sorgu parametre değerleri için tanımlanan değerleri.|  
|Gerekli|boole|Parametresi gerekli olup olmadığını belirtir.|  
|türü|numarası|Bu parametre bir yol parametresi (1) veya bir sorgu dizesi parametresi (2) olup olmadığı.|  
|typeName|string|Parametre türü.|  
  
##  <a name="Product">Ürün</a>  
 `product` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|Kaynak tanımlayıcısı. Ürününün geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` bir ürün tanımlayıcısı. Bu özellik salt okunurdur.|  
|Başlık|string|Ürün adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Açıklama|string|Ürün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|Koşullar|string|Kullanım koşulları ürün. Ürüne abone olmayı deneyen geliştiricilere sunulan ve abonelik işlemi tamamlanmadan önce bu koşulları kabul etmeniz gerekir.|  
|ProductState|numarası|Ürün veya yayımlanan belirtir. Yayımlanan ürün geliştiriciler Geliştirici Portalı tarafından bulunabilir. Olmayan yayımlanan ürünleri yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilen değerler:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|boole|Bir kullanıcı aynı anda birden fazla abonelik bu ürün için sahip olup olmadığını belirtir.|  
|MultipleSubscriptionsCount|numarası|Bu ürün için abonelik maksimum sayısı bir kullanıcı aynı anda sahip olabilir.|  
  
##  <a name="Provider"></a> Sağlayıcı  
 `provider` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Özellikler|dize sözlüğü|Bu kimlik doğrulama sağlayıcısı özellikleri.|  
|authenticationType|string|Sağlayıcı türü. (Azure Active Directory, Facebook oturum açma, Google hesabı, Microsoft Account, Twitter).|  
|Açıklamalı alt yazı|string|Sağlayıcının görünen adı.|  
  
##  <a name="Representation">Gösterimi</a>  
 Bu bölümde açıklanmıştır bir `representation`.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|contentType|string|Kayıtlı ya da özel bir içerik türü için bu gösterim belirtir, örneğin, `application/xml`.|  
|Örnek|string|Gösterim örneği.|  
  
##  <a name="Subscription"></a> Abonelik  
 `subscription` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|Kaynak tanımlayıcısı. Abonelik geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|Ürün kimliği|string|Abone olunan ürün ürün kaynak tanımlayıcısı. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` bir ürün tanımlayıcısı.|  
|ProductTitle|string|Ürün adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|ProductDescription|string|Ürün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|ProductDetailsUrl|string|Ürün Ayrıntıları göreli URL.|  
|durum|string|Abonelik durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended` – Abonelik engellenir ve abone ürünün herhangi bir API çağrılamaz.<br /><br /> - `1 - active` – Aboneliğinizin etkin olduğunu.<br /><br /> - `2 - expired` – Abonelik sona erme tarihini ulaştı ve devre dışı bırakıldı.<br /><br /> - `3 - submitted` – Abonelik isteğinin geliştirici tarafından yapılan ancak henüz onaylanamıyor veya reddedilemiyor.<br /><br /> - `4 - rejected` – Abonelik isteğinin bir yönetici tarafından reddedildi.<br /><br /> - `5 - cancelled` – Abonelik geliştirici veya yönetici tarafından iptal edildi.|  
|Görünen adı|string|Abonelik adını görüntüler.|  
|CreatedDate|Tarih saat|Abonelik, ISO 8601 biçiminde oluşturulduğu tarih: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|boole|Abonelik geçerli kullanıcı tarafından iptal.|  
|IsAwaitingApproval|boole|Olup abonelik onayını bekliyor.|  
|StartDate|Tarih saat|ISO 8601 biçiminde abonelik için başlangıç tarihi: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Tarih saat|Sona erme tarihini ISO 8601 biçiminde abonelik: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Tarih saat|ISO 8601 biçiminde abonelik için bildirim tarihi: `2014-06-24T16:25:00Z`.|  
|primaryKey|string|Birincil abonelik anahtarı. En fazla uzunluğu 256 karakterden uzun.|  
|secondaryKey|string|İkincil abonelik anahtarı. En fazla uzunluğu 256 karakterden uzun.|  
|CanBeRenewed|boole|Abonelik geçerli kullanıcı tarafından yenilenmesi.|  
|HasExpired|boole|Abonelik süresi doldu.|  
|IsRejected|boole|Olup abonelik istek reddedildi.|  
|CancelUrl|string|Aboneliği iptal etmek için göreli URL'si.|  
|RenewUrl|string|Aboneliğinizi yenilemek için göreli URL'si.|  
  
##  <a name="SubscriptionSummary"></a> Abonelik özeti  
 `subscription summary` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|string|Kaynak tanımlayıcısı. Abonelik geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|Görünen adı|string|Aboneliğin görünen adı|  
  
##  <a name="UserAccountInfo"></a> Kullanıcı hesabı bilgileri  
 `user account info` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|FirstName|string|İlk adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Soyadı|string|Soyadı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|E-posta|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|string|Kullanıcı hesabı parolası.|  
|NameIdentifier|string|Hesap tanımlayıcısı, kullanıcı e-posta ile aynı.|  
|ProviderName|string|Kimlik doğrulama sağlayıcısının adı.|  
|IsBasicAccount|boole|Bu hesabın e-posta ve parola kullanarak kaydolduysanız true; hesap bir sağlayıcı kullanarak kaydolduysanız yanlış.|  
  
##  <a name="UseSignIn"></a> Kullanıcı oturum açma  
 `user sign in` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|E-posta|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|string|Kullanıcı hesabı parolası.|  
|ReturnUrl|string|Kullanıcı tıklattığınız sayfasının URL'sini oturum açın.|  
|Kullandığınız Beni anımsa|boole|Mevcut kullanıcının bilgileri kaydedilip kaydedilmeyeceğini belirtir.|  
|RegistrationEnabled|boole|Kayıt etkinleştirilip etkinleştirilmediği.|  
|DelegationEnabled|boole|Temsilci oturum açma etkinleştirilip etkinleştirilmediği.|  
|DelegationUrl|string|Temsilci oturum açma etkinleştirildiğinde url.|  
|SsoSignUpUrl|string|Varsa, kullanıcı için çoklu oturum açma URL'si.|  
|AuxServiceUrl|string|Geçerli kullanıcı bir yöneticiyse, Azure portalında hizmet örneği için bir bağlantı budur.|  
|Sağlayıcılar|Koleksiyonu [sağlayıcı](#Provider) varlıklar|Bu kullanıcı için kimlik doğrulama sağlayıcıları.|  
|UserRegistrationTerms|string|Bir kullanıcı oturum açmadan önce kabul etmeniz gerekir koşulları.|  
|UserRegistrationTermsEnabled|boole|Koşulları etkinleştirilip etkinleştirilmediği.|  
  
##  <a name="UserSignUp"></a> Kullanıcı Kaydolma  
 `user sign up` Varlık aşağıdaki özellikleri içerir:  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|PasswordConfirm|boole|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|Parola|string|Kullanıcı hesabı parolası.|  
|PasswordVerdictLevel|numarası|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|UserRegistrationTerms|string|Bir kullanıcı oturum açmadan önce kabul etmeniz gerekir koşulları.|  
|UserRegistrationTermsOptions|numarası|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|ConsentAccepted|boole|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|E-posta|string|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|FirstName|string|İlk adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Soyadı|string|Soyadı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|UserData|string|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up) denetim.|  
|NameIdentifier|string|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|ProviderName|string|Kimlik doğrulama sağlayıcısının adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).
