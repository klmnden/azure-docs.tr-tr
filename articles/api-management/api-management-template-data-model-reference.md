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
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 72936a4d38f809934ddea74e5ae4a6029450a97c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management şablonu veri modeli başvurusu
Bu konuda veri modellerinde Azure API Management'ta Geliştirici Portalı şablonları için kullanılan ortak öğeler için varlık ve türü Beyanları açıklanmaktadır.  
  
 Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [API](#API)  
-   [API özeti](#APISummary)  
-   [Uygulama](#Application)  
-   [Eki](#Attachment)  
-   [Kod örneği](#Sample)  
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
  
##  <a name="API"></a>API  
 `API` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|Dize|Kaynak tanımlayıcısı. API geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` bir API tanımlayıcısıdır. Bu özellik salt okunurdur.|  
|ad|Dize|API adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|Dize|API açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|serviceUrl|Dize|Bu API'yi uygulayan arka uç hizmetine mutlak URL'si.|  
|Yol|Dize|Bu API ve tüm API Management hizmet örneği içinde kendi kaynak yolları tanıtan göreli URL'si. Bu API için genel bir URL oluşturmak için hizmet örneği oluşturma sırasında belirtilen API uç noktası temel URL'sini eklenir.|  
|protokolleri|dizi numarası|Bu API işlemlerinde hangi protokollerine çağrılabilir açıklar. İzin verilen değerler `1 - http` ve `2 - https`, veya her ikisini de.|  
|authenticationSettings|[Yetkilendirme sunucusu kimlik doğrulama ayarları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Bu API içinde bulunan kimlik doğrulama ayarları koleksiyonu.|  
|subscriptionKeyParameterNames|Nesne|Abonelik anahtarı içeren sorgu ve/veya üstbilgi parametreleri için özel adların belirtmek için kullanılan isteğe bağlı özellik. Bu özellik bulunduğunda iki aşağıdaki özellikleri en az birini içermelidir.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>API özeti  
 `API summary` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|Dize|Kaynak tanımlayıcısı. API geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{id}` burada `{id}` bir API tanımlayıcısıdır. Bu özellik salt okunurdur.|  
|ad|Dize|API adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|Dize|API açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
  
##  <a name="Application"></a>Uygulama  
 `application` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|Uygulamanın benzersiz tanımlayıcısı.|  
|Başlık|Dize|Uygulama Başlığı.|  
|Açıklama|Dize|Uygulama açıklaması.|  
|Url|URI|Uygulama için URI.|  
|Sürüm|Dize|Uygulama için sürüm bilgisi.|  
|Gereksinimler|Dize|Uygulama için gereksinimleri açıklaması.|  
|Durum|Sayı|Uygulama geçerli durumu.<br /><br /> -0 - kayıtlı<br /><br /> -Gönderilen 1-<br /><br /> -Yayımlanan 2-<br /><br /> -Reddedilen 3-<br /><br /> -4 - yayımlanmamış|  
|RegistrationDate|Tarih saat|Tarih ve saat uygulama kaydedildi.|  
|Adlı kullanıcı, Categoryıd'si|Sayı|Kategori uygulamanın (Finans, eğlence, vs.)|  
|DeveloperId|Dize|Uygulama gönderilen Geliştirici benzersiz tanımlayıcısı.|  
|Ekleri|Koleksiyonu [ek](#Attachment) varlıklar.|Herhangi bir ek ekran görüntüleri veya simgeler gibi uygulama için.|  
|Simgesi|[Eki](#Attachment)|Simge uygulama için.|  
  
##  <a name="Attachment"></a>Eki  
 `attachment` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|UniqueId|Dize|Eki için benzersiz tanımlayıcı.|  
|Url|Dize|Kaynak URL.|  
|Tür|Dize|Ek türü.|  
|ContentType|Dize|Ek medya türü.|  
  
##  <a name="Sample"></a>Kod örneği  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Başlık|Dize|İşlemin adı.|  
|kod parçacığında|Dize|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|Fırça|Dize|Kod örneği görüntülenirken kullanılacak şablon renklendirme hangi kod sözdizimi. İzin verilen değerler `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, ve `csharp`.|  
|şablon|Dize|Bu kod örneği şablonunun adı.|  
|Gövde|Dize|Kod parçacığını kod örnek kısmı için bir yer tutucu.|  
|Yöntemi|Dize|İşlemi HTTP yöntemi.|  
|Düzeni|Dize|İşlem isteği için kullanılacak protokolü.|  
|Yol|Dize|İşlemi yolu.|  
|sorgu|Dize|Sorgu dizesi örnek tanımlanan parametrelere sahip.|  
|ana bilgisayar|Dize|Bu işlem içeriyor API için API Management hizmeti ağ geçidi URL'si.|  
|Üstbilgileri|Koleksiyonu [üstbilgi](#Header) varlıklar.|Bu işlem için üstbilgiler.|  
|parametreler|Koleksiyonu [parametresi](#Parameter) varlıklar.|Bu işlem için tanımlanan parametreleri.|  
  
##  <a name="Comment"></a>Açıklama  
 `API` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Sayı|Açıklama kimliği.|  
|CommentText|Dize|Açıklama gövdesi. HTML içerebilir.|  
|DeveloperCompany|Dize|Geliştirici şirket adı.|  
|PostedOn|Tarih saat|Tarih ve saat yorum kopyalanmışsa.|  
  
##  <a name="Issue"></a>Sorunu  
 `issue` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|Sorun için benzersiz tanımlayıcı.|  
|ApiID|Dize|Bu sorun bildirildi API kimliği.|  
|Başlık|Dize|Sorun başlığı.|  
|Açıklama|Dize|Sorun açıklaması.|  
|SubscriptionDeveloperName|Dize|Sorunu bildiren Geliştirici adı.|  
|IssueState|Dize|Sorunu geçerli durumu. Olası değerler şunlardır: Önerilen, açık, kapalı.|  
|ReportedOn|Tarih saat|Tarih ve saat sorun bildirildi.|  
|Yorumlar|Koleksiyonu [açıklama](#Comment) varlıklar.|Bu konu hakkında açıklamalar.|  
|Ekleri|Koleksiyonu [ek](api-management-template-data-model-reference.md#Attachment) varlıklar.|Herhangi bir ek verilecek.|  
|Hizmetler|Koleksiyonu [API](#API) varlıklar.|Sorunu Dosyalanan kullanıcı tarafından abone API'leri.|  
  
##  <a name="Filtering"></a>Filtreleme  
 `filtering` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|düzeni|Dize|Geçerli arama terimi; veya `null` arama terimi yok ise.|  
|Yer tutucu|Dize|Belirtilen arama terimi yok olduğunda arama kutusuna görüntülenecek metin.|  
  
##  <a name="Header"></a>Üstbilgi  
 Bu bölümde açıklanmıştır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|Dize|Parametre adı.|  
|açıklama|Dize|Parametre açıklaması.|  
|değer|Dize|Üstbilgi değeri.|  
|TypeName|Dize|Üstbilgi değeri veri türü.|  
|Seçenekler|Dize|Seçenekler.|  
|Gerekli|Boole değeri|Üstbilgi gerekli olup olmadığı.|  
|salt okunur|Boole değeri|Üstbilgi salt okunur olup olmadığı.|  
  
##  <a name="HTTPRequest"></a>HTTP isteği  
 Bu bölümde açıklanmıştır `request` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|açıklama|Dize|İşlem isteği açıklaması.|  
|Üstbilgileri|dizi [üstbilgi](#Header) varlıklar.|İstek üstbilgileri.|  
|parametreler|dizi [parametresi](#Parameter)|İşlem istek parametreleri topluluğu.|  
|temsili|dizi [gösterimi](#Representation)|İşlem istek sunumlarını koleksiyonu.|  
  
##  <a name="HTTPResponse"></a>HTTP yanıtı  
 Bu bölümde açıklanmıştır `response` gösterimi.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|statusCode|Pozitif tamsayı|İşlem yanıt durum kodu.|  
|açıklama|Dize|İşlem yanıt açıklaması.|  
|temsili|dizi [gösterimi](#Representation)|İşlem yanıt Beyanları koleksiyonu.|  
  
##  <a name="Operation"></a>İşlemi  
 `operation` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|id|Dize|Kaynak tanımlayıcısı. İşlemi geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `apis/{aid}/operations/{id}` nerede `{aid}` bir API tanımlayıcısıdır ve `{id}` işlemi tanımlayıcıdır. Bu özellik salt okunurdur.|  
|ad|Dize|İşlemin adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|açıklama|Dize|İşlem açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|Düzeni|Dize|Bu API işlemlerinde hangi protokollerine çağrılabilir açıklar. İzin verilen değerler `http`, `https`, veya her ikisini de `http` ve `https`.|  
|uriTemplate|Dize|Bu işlem için hedef kaynak tanımlayan göreli URL şablonu. Parametreler içerebilir. Örnek:`customers/{cid}/orders/{oid}/?date={date}`|  
|ana bilgisayar|Dize|API barındıran API Yönetimi ağ geçidi URL'si.|  
|HttpMethod|Dize|İşlemi HTTP yöntemi.|  
|İstek|[HTTP isteği](#HTTPRequest)|İstek ayrıntıları içeren bir varlık.|  
|yanıtları|dizi [HTTP yanıtı](#HTTPResponse)|İşlem dizisi [HTTP yanıtı](#HTTPResponse) varlıklar.|  
  
##  <a name="Menu"></a>İşlemi menüsü  
 `operation menu` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|ApiId|Dize|Geçerli API kimliği.|  
|CurrentOperationId|Dize|Geçerli işlem kimliği.|  
|Eylem|Dize|Menü türü.|  
|MenuItems|Koleksiyonu [işlemi menü öğesi](#MenuItem) varlıklar.|Geçerli API işlemleri.|  
  
##  <a name="MenuItem"></a>İşlemi menü öğesi  
 `operation menu item` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|İşlem kimliği.|  
|Başlık|Dize|İşlem açıklaması.|  
|HttpMethod|Dize|İşlemi Http yöntemi.|  
  
##  <a name="Paging"></a>Disk belleği  
 `paging` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfa|Sayı|Geçerli sayfa numarası.|  
|PageSize|Sayı|Tek bir sayfada gösterilecek en fazla sonuç.|  
|TotalItemCount|Sayı|Görüntülenecek öğe sayısı.|  
|ShowAll|Boole değeri|Kullanılıp kullanılmayacağını tüm sonuçları tek bir sayfada göster.|  
|PageCount|Sayı|Sayfa sonuç sayısı.|  
  
##  <a name="Parameter"></a>Parametre  
 Bu bölümde açıklanmıştır `parameter` gösterimi.  
  
|Özellik|Açıklama|Tür|  
|--------------|-----------------|----------|  
|ad|Dize|Parametre adı.|  
|açıklama|Dize|Parametre açıklaması.|  
|değer|Dize|Parametre değeri.|  
|Seçenekler|Dize dizisi|Sorgu parametre değerleri için tanımlanan değerleri.|  
|Gerekli|Boole değeri|Parametresi gerekli olup olmadığını belirtir.|  
|türü|Sayı|Bu parametre bir yol parametresi (1) veya bir sorgu dizesi parametresi (2) olup olmadığı.|  
|TypeName|Dize|Parametre türü.|  
  
##  <a name="Product"></a>Ürün  
 `product` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|Kaynak tanımlayıcısı. Ürününün geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` bir ürün tanımlayıcısı. Bu özellik salt okunurdur.|  
|Başlık|Dize|Ürün adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Açıklama|Dize|Ürün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|Koşullar|Dize|Kullanım koşulları ürün. Ürüne abone olmayı deneyen geliştiricilere sunulan ve abonelik işlemi tamamlanmadan önce bu koşulları kabul etmeniz gerekir.|  
|ProductState|Sayı|Ürün veya yayımlanan belirtir. Yayımlanan ürün geliştiriciler Geliştirici Portalı tarafından bulunabilir. Olmayan yayımlanan ürünleri yalnızca yöneticiler tarafından görülebilir.<br /><br /> Ürün durumu için izin verilen değerler:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Boole değeri|Bir kullanıcı aynı anda birden fazla abonelik bu ürün için sahip olup olmadığını belirtir.|  
|MultipleSubscriptionsCount|Sayı|Geçerli kullanıcı tarafından bu ürün için abonelik sayısı.|  
  
##  <a name="Provider"></a>Sağlayıcı  
 `provider` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Özellikler|dize sözlüğü|Bu kimlik doğrulama sağlayıcısı özellikleri.|  
|authenticationType|Dize|Sağlayıcı türü. (Azure Active Directory, Facebook oturum açma, Google hesabı, Microsoft Account, Twitter).|  
|Açıklamalı alt yazı|Dize|Sağlayıcının görünen adı.|  
  
##  <a name="Representation"></a>Gösterimi  
 Bu bölümde açıklanmıştır bir `representation`.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|ContentType|Dize|Kayıtlı ya da özel bir içerik türü için bu gösterim örneğin belirtir `application/xml`.|  
|Örnek|Dize|Gösterim örneği.|  
  
##  <a name="Subscription"></a>Abonelik  
 `subscription` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|Kaynak tanımlayıcısı. Abonelik geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|ProductID|Dize|Abone olunan ürün ürün kaynak tanımlayıcısı. Biçiminde geçerli bir göreli URL değerdir `products/{pid}` burada `{pid}` bir ürün tanımlayıcısı.|  
|ProductTitle|Dize|Ürün adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|ProductDescription|Dize|Ürün açıklaması. Boş olmamalıdır. HTML biçimlendirme etiketleriyle içerebilir. En fazla 1000 karakter olabilir.|  
|ProductDetailsUrl|Dize|Ürün Ayrıntıları göreli URL.|  
|durum|Dize|Abonelik durumu. Olası durumlar şunlardır:<br /><br /> - `0 - suspended`– Abonelik engellenir ve abone ürünün herhangi bir API çağrılamaz.<br /><br /> - `1 - active`– Aboneliğinizin etkin olduğunu.<br /><br /> - `2 - expired`– Abonelik sona erme tarihini ulaştı ve devre dışı bırakıldı.<br /><br /> - `3 - submitted`– Abonelik isteğinin geliştirici tarafından yapılan ancak henüz onaylanamıyor veya reddedilemiyor.<br /><br /> - `4 - rejected`– Abonelik isteğinin bir yönetici tarafından reddedildi.<br /><br /> - `5 - cancelled`– Abonelik geliştirici veya yönetici tarafından iptal edildi.|  
|Görünen adı|Dize|Abonelik adını görüntüler.|  
|CreatedDate|Tarih saat|Abonelik, ISO 8601 biçiminde oluşturulduğu tarih: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Boole değeri|Abonelik geçerli kullanıcı tarafından iptal.|  
|IsAwaitingApproval|Boole değeri|Olup abonelik onayını bekliyor.|  
|StartDate|Tarih saat|ISO 8601 biçiminde abonelik için başlangıç tarihi: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Tarih saat|Sona erme tarihini ISO 8601 biçiminde abonelik: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Tarih saat|ISO 8601 biçiminde abonelik için bildirim tarihi: `2014-06-24T16:25:00Z`.|  
|primaryKey|Dize|Birincil abonelik anahtarı. En fazla uzunluğu 256 karakterden uzun.|  
|secondaryKey|Dize|İkincil abonelik anahtarı. En fazla uzunluğu 256 karakterden uzun.|  
|CanBeRenewed|Boole değeri|Abonelik geçerli kullanıcı tarafından yenilenmesi.|  
|HasExpired|Boole değeri|Abonelik süresi doldu.|  
|IsRejected|Boole değeri|Olup abonelik istek reddedildi.|  
|CancelUrl|Dize|Aboneliği iptal etmek için göreli URL'si.|  
|RenewUrl|Dize|Aboneliğinizi yenilemek için göreli URL'si.|  
  
##  <a name="SubscriptionSummary"></a>Abonelik özeti  
 `subscription summary` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Kimlik|Dize|Kaynak tanımlayıcısı. Abonelik geçerli API Management hizmet örneği içinde benzersiz olarak tanımlar. Biçiminde geçerli bir göreli URL değerdir `subscriptions/{sid}` burada `{sid}` bir abonelik tanımlayıcısı. Bu özellik salt okunurdur.|  
|Görünen adı|Dize|Aboneliğin görünen adı|  
  
##  <a name="UserAccountInfo"></a>Kullanıcı hesabı bilgileri  
 `user account info` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|FirstName|Dize|İlk adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Soyadı|Dize|Soyadı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|E-posta|Dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|Dize|Kullanıcı hesabı parolası.|  
|NameIdentifier|Dize|Hesap tanımlayıcısı, kullanıcı e-posta ile aynı.|  
|ProviderName|Dize|Kimlik doğrulama sağlayıcısının adı.|  
|IsBasicAccount|Boole değeri|Bu hesabın e-posta ve parola kullanarak kaydolduysanız true; hesap bir sağlayıcı kullanarak kaydolduysanız yanlış.|  
  
##  <a name="UseSignIn"></a>Kullanıcı oturum açma  
 `user sign in` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|E-posta|Dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|Parola|Dize|Kullanıcı hesabı parolası.|  
|ReturnUrl|Dize|Kullanıcı tıklattığınız sayfasının URL'sini oturum açın.|  
|Kullandığınız Beni anımsa|Boole değeri|Mevcut kullanıcının bilgileri kaydedilip kaydedilmeyeceğini belirtir.|  
|RegistrationEnabled|Boole değeri|Kayıt etkinleştirilip etkinleştirilmediği.|  
|DelegationEnabled|Boole değeri|Temsilci oturum açma etkinleştirilip etkinleştirilmediği.|  
|DelegationUrl|Dize|Temsilci oturum açma etkinleştirildiğinde url.|  
|SsoSignUpUrl|Dize|Varsa, kullanıcı için çoklu oturum açma URL'si.|  
|AuxServiceUrl|Dize|Geçerli kullanıcı bir yöneticiyse, Azure Klasik Portalı'nda hizmet örneği için bir bağlantı budur.|  
|Sağlayıcılar|Koleksiyonu [sağlayıcı](#Provider) varlıklar|Bu kullanıcı için kimlik doğrulama sağlayıcıları.|  
|UserRegistrationTerms|Dize|Bir kullanıcı oturum açmadan önce kabul etmeniz gerekir koşulları.|  
|UserRegistrationTermsEnabled|Boole değeri|Koşulları etkinleştirilip etkinleştirilmediği.|  
  
##  <a name="UserSignUp"></a>Kullanıcı Kaydolma  
 `user sign up` Varlık aşağıdaki özellikleri içerir.  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|PasswordConfirm|Boole değeri|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|Parola|Dize|Kullanıcı hesabı parolası.|  
|PasswordVerdictLevel|Sayı|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|UserRegistrationTerms|Dize|Bir kullanıcı oturum açmadan önce kabul etmeniz gerekir koşulları.|  
|UserRegistrationTermsOptions|Sayı|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|ConsentAccepted|Boole değeri|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|E-posta|Dize|E-posta adresi. Boş olmamalı ve hizmet örneği içinde benzersiz olmalıdır. En fazla uzunluğu 254 karakter olabilir.|  
|FirstName|Dize|İlk adı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|Soyadı|Dize|Soyadı. Boş olmamalıdır. En fazla 100 karakter olabilir.|  
|UserData|Dize|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up) denetim.|  
|NameIdentifier|Dize|Tarafından kullanılan değer [kaydolma](api-management-page-controls.md#sign-up)kaydolma denetim.|  
|ProviderName|Dize|Kimlik doğrulama sağlayıcısının adı.|

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).
