---
title: Azure API Management şablon kaynaklarını | Microsoft Docs
description: Azure API Management'ta Geliştirici Portalı şablonları için kullanılabilir kaynak türleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 212e7ea7bb2ffea63c7ba210195df0da38aa8f0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23835401"
---
# <a name="azure-api-management-template-resources"></a>Azure API Management şablonu kaynakları
Azure API Management aşağıdaki türdeki kaynakları portal şablonları Geliştirici kullanmak için sağlar.  
  
-   [Dize kaynakları](#strings)  
  
-   [Karakter kaynakları](#glyphs)  
  
##  <a name="strings"></a>Dize kaynakları  
 API Management Geliştirici Portalı kullanmak için dize kaynaklarını kapsamlı bir kümesini sağlar. API Management tarafından desteklenen dillerin tümünün içine yerelleştirilmiş bu kaynakları. Varsayılan kümesi şablonları, bu kaynakları sayfa üstbilgilerinde, etiketler ve Geliştirici Portalı'nda görüntülenen tüm sabit dizeler için kullanır. Bir dize kaynağı, şablonlarında kullanmak için aşağıdaki örnekte gösterildiği gibi dize adından kaynak dize ön ekini belirtin.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Aşağıdaki örnek ürün listesi şablonu ve bir görüntüler **ürünleri** sayfanın üst kısmındaki.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Geliştirici Portalı şablonlarınızı için kullanılabilir dize kaynakları için aşağıdaki tablolara bakın. Tablo adı, bu tablodaki dize kaynakları için önek olarak kullanın.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [ApplicationListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [CommonResources](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Belgeleri](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ProductDetailsStrings](#ProductDetailsStrings)  
  
-   [ProductsStrings](#ProductsStrings)  
  
-   [ProviderInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [SignupStrings](#SignupStrings)  
  
-   [SubscriptionListStrings](#SubscriptionListStrings)  
  
-   [SubscriptionStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [Kullanıcı profili](#UserProfile)  
  
###  <a name="ApisStrings"></a>ApisStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleApis|API'ler|  
  
###  <a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebApplicationsDetailsTitle|Uygulama Önizleme|  
|WebApplicationsRequirementsHeader|Gereksinimler|  
|WebApplicationsScreenshotAlt|ekran görüntüsü|  
|WebApplicationsScreenshotsHeader|Ekran görüntüleri|  
  
###  <a name="ApplicationListStrings"></a>ApplicationListStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Uygulamayı kaldırmak istediğinizden emin misiniz?|  
|WebDevelopersAppNotPublished|Yayımlanmadı|  
|WebDevelopersAppNotSubminted|Gönderilemedi|  
|WebDevelopersAppTableCategoryHeader|Kategori|  
|WebDevelopersAppTableNameHeader|Ad|  
|WebDevelopersAppTableStateHeader|Durum|  
|WebDevelopersEditLink|Düzenle|  
|WebDevelopersRegisterAppLink|Uygulamayı Kaydet|  
|WebDevelopersRemoveLink|Kaldır|  
|WebDevelopersSubmitLink|Gönder|  
|WebDevelopersYourApplicationsHeader|Uygulamalarınızı|  
  
###  <a name="AppStrings"></a>AppStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebApplicationsHeader|Uygulamalar|  
  
###  <a name="CommonResources"></a>CommonResources  
  
|Ad|Metin|  
|----------|----------|  
|NoItemsToDisplay|Sonuç bulunamadı.|  
|GeneralExceptionMessage|Bir şey doğru değil. Geçici bir hata veya bir hata olabilir. Lütfen yeniden deneyin.|  
|GeneralJsonExceptionMessage|Bir şey doğru değil. Geçici bir hata veya bir hata olabilir. Lütfen sayfayı yeniden yükleyin ve yeniden deneyin.|  
|ConfirmationMessageUnsavedChanges|Bazı kaydedilmemiş değişiklikler var. İptal etmek ve değişiklikleri atmak istediğinizden emin misiniz?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|HTTP istek gövdesi çok büyük.|  
  
###  <a name="CommonStrings"></a>CommonStrings  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelCancel|İptal|  
|ButtonLabelSave|Kaydet|  
|GeneralExceptionMessage|Bir şey doğru değil. Geçici bir hata veya bir hata olabilir. Lütfen yeniden deneyin.|  
|NoItemsToDisplay|Görüntülenecek öğe yok.|  
|PagerButtonLabelFirst|İlk|  
|PagerButtonLabelLast|Son|  
|PagerButtonLabelNext|Sonraki|  
|PagerButtonLabelPrevious|Önceki|  
|PagerLabelPageNOfM|{0} {1}|  
|PasswordTooShort|Parola çok kısa|  
|EmailAsPassword|E-posta parolanızı kullanmayın|  
|PasswordSameAsUserName|Parolanız kullanıcı adınızı içeremez|  
|PasswordTwoCharacterClasses|Farklı karakter sınıflarını kullanma|  
|PasswordTooManyRepetitions|Çok fazla tekrarları|  
|PasswordSequenceFound|Parolanızı sıraları içerir|  
|PagerLabelPageSize|Sayfa boyutu|  
|CurtainLabelLoading|Yükleniyor...|  
|TablePlaceholderNothingToDisplay|Seçilen dönem ve kapsam için hiçbir veri olduğu|  
|ButtonLabelClose|Kapat|  
  
###  <a name="Documentation"></a>Belgeleri  
  
|Ad|Metin|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Geçersiz üstbilgi '{0}'|  
|WebDocumentationInvalidRequestErrorMessage|Geçersiz istek URL'si|  
|TextboxLabelAccessToken|Erişim belirteci *|  
|DropdownOptionPrimaryKeyFormat|Birincil-{0}|  
|DropdownOptionSecondaryKeyFormat|İkincil-{0}|  
|WebDocumentationSubscriptionKeyText|Abonelik anahtarınızı|  
|WebDocumentationTemplatesAddHeaders|Gerekli HTTP üst bilgisi Ekle|  
|WebDocumentationTemplatesBasicAuthSample|Temel yetkilendirme örneği|  
|WebDocumentationTemplatesCurlForBasicAuth|Temel yetkilendirme kullanmak için:--kullanıcı {username}: {parola}|  
|WebDocumentationTemplatesCurlValuesForPath|Yolu parametreleri ({...} gösterilir), abonelik anahtarı ve sorgu parametreleri için değerleri için değerleri belirtin|  
|WebDocumentationTemplatesDeveloperKey|Abonelik anahtarınızı belirtin|  
|WebDocumentationTemplatesJavaApache|Bu örnek Apache HTTP istemci HTTP bileşenlerden (http://hc.apache.org/httpcomponents-client-ga/) kullanır|  
|WebDocumentationTemplatesOptionalParams|Gerektiğinde isteğe bağlı parametre değerlerini belirtin|  
|WebDocumentationTemplatesPhpPackage|Bu örnek HTTP_Request2 paketi kullanır. (daha fazla bilgi için: http://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|({...} Gösterilir) yolu parametrelerin değerlerini belirtin ve gerekirse istek gövdesinde|  
|WebDocumentationTemplatesRequestBody|İstek gövdesini belirtin|  
|WebDocumentationTemplatesRequiredParams|Aşağıdaki gerekli parametreleri için değerleri belirtin|  
|WebDocumentationTemplatesValuesForPath|({...} Gösterilir) yolu parametrelerin değerlerini belirtin|  
|OAuth2AuthorizationEndpointDescription|Yetkilendirme uç noktası, kaynak sahibi ile etkileşim kurmanızı ve yetkilendirme verme elde etmek için kullanılır.|  
|OAuth2AuthorizationEndpointName|Yetkilendirme uç noktası|  
|OAuth2TokenEndpointDescription|Belirteç uç noktası, bir erişim belirteci edinmek için yetkilendirme verme sunarak veya belirtecini yenileme istemci tarafından kullanılır.|  
|OAuth2TokenEndpointName|belirteç uç noktası|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> istemci yetkilendirme uç noktası için kaynak sahibinin kullanıcı aracısını yönlendirerek akışını başlatır.  İstemci, istemci tanıtıcısını, istenen kapsamı, yerel durumu ve geri erişim izni (engellendi veya sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir.     < /p\> < p\> yetkilendirme sunucusu (aracılığıyla kullanıcı aracısı) kaynak sahibinin kimliğini doğrular ve kaynak sahibi verir veya istemcinin erişim isteği reddeder oluşturur.     < /p\> < p\> kaynak sahibi olduğu varsayılarak erişim verir, yetkilendirme sunucusu kullanıcı aracısını yeniden yönlendirme URI'si sağlanan kullanarak istemciye yönlendirir (istek veya istemci registrati sırasında önceki açık).  Yeniden yönlendirme URI'si bir yetkilendirme kodu ve istemci tarafından daha önce sağlanan herhangi bir yerel durumu içerir.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> istek geçersiz ise kullanıcı erişim isteği reddeder, istemci yeniden yönlendirme açın eklenen aşağıdaki parametreleri kullanarak bildirilir: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> istemci uygulaması, OAuth işlemini başlatmak için yetkilendirme uç noktasına kullanıcı göndermelisiniz.          Yetkilendirme uç noktada kullanıcının kimliğini doğrular ve ardından verir veya uygulama erişimi engeller.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> kaynak sahibi olduğu varsayılarak erişim verir, yetkilendirme sunucusu kullanıcı aracısını yeniden yönlendirme URI'si sağlanan kullanarak istemciye yönlendirir önceki (istek veya istemci kaydı sırasında).  Yeniden yönlendirme URI'si bir yetkilendirme kodu ve istemci tarafından daha önce sağlanan herhangi bir yerel durumu içerir. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> istemci bir erişim belirteci yetkilendirme sunucusundan ister '' önceki adımda alınan yetkilendirme kodu ekleyerek s belirteç uç noktası.  İsteği yaparken yetkilendirme sunucusu ile istemci kimliğini doğrular.  İstemci doğrulaması için yetkilendirme kodu almak için URI kullanılan yeniden yönlendirme içerir. < /p\> < p\> yetkilendirme sunucusu istemci kimlik doğrulaması, yetkilendirme kodu doğrular ve yeniden yönlendirme URI'si alınan adım (C) istemci yeniden yönlendirmek için kullanılan URI'yi eşleşmesini sağlar.  Geçerliyse, yetkilendirme sunucusu geri bir erişim belirteci ve bir yenileme belirteci ile isteğe bağlı olarak yanıt verir. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> isteği istemci kimlik doğrulaması başarısız olduysa veya geçersiz, yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve yanıt aşağıdaki parametreleri içerir. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> istemci belirteç uç noktası olarak bir karakter HTTP UTF-8 kodlamasını ile "uygulama/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri istek Varlık gövdesi göndererek istekte bulunur. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı yenileme belirteci veren ve 200 (Tamam) durum kodlu HTTP yanıtının varlık gövdesi için aşağıdaki parametreleri ekleyerek yanıt oluşturur. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> istemci yetkilendirme ile kimlik doğrulamasını ve belirteç uç noktasından bir erişim belirteci ister. < /p\> < p\> yetkilendirme sunucusu istemci kimliğini doğrular ve geçerliyse, bir erişim belirteci verir. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> istek istemci kimlik doğrulaması başarısız oldu veya geçersizse yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve yanıt aşağıdaki parametreleri içerir. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> istemci belirteç uç noktası için istek Varlık gövdesi bir karakter HTTP UTF-8 kodlamasını ile "uygulama/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri ekleyerek istekte bulunur. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> erişim belirteci isteği geçerli ve yetkili ise, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı yenileme belirteci sorunları ve bir 200 ile HTTP yanıtının varlık gövdesi için aşağıdaki parametreleri ekleyerek yanıtı oluşturur  (Tamam) durum kodu. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> kaynak sahibi yönlendirerek akış istemci başlatır '' s aracısının yetkilendirme uç noktası.  İstemci, istemci tanıtıcısını, istenen kapsamı, yerel durumu ve geri erişim izni (engellendi veya sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. < /p\> < p\> yetkilendirme sunucusu (aracılığıyla kullanıcı aracısı) kaynak sahibinin kimliğini doğrular ve kaynak sahibi verir veya istemci engellediği olup olmadığını belirler. '' s erişim isteği. < /p\> < p\> kaynak sahibi olduğu varsayılarak erişim verir, yetkilendirme sunucusu kullanıcı aracısını URI sağlanan daha önce yeniden yönlendirmeyi kullanma istemciye yönlendirir.  Yeniden yönlendirme URI'si erişim belirteci URI parçadaki içerir. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> kaynak sahibine erişim isteği reddeder ya da eksik veya geçersiz yeniden yönlendirme URI'si dışında nedenlerle isteği başarısız olursa, yetkilendirme sunucusu istemci parça bileşeni o için aşağıdaki parametreleri ekleyerek bildirir f yeniden yönlendirme URI'si "uygulama/x-www-form-urlencoded" biçimini kullanarak. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> istemci uygulaması, OAuth işlemini başlatmak için yetkilendirme uç noktasına kullanıcı göndermelisiniz.      Yetkilendirme uç noktada kullanıcının kimliğini doğrular ve ardından verir veya uygulama erişimi engeller. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> kaynak sahibine erişim isteği verirse, yetkilendirme sunucusu bir erişim belirteci verir ve istemciye yeniden yönlendirme URI'si "uygulama/x-www kullanarak parça bileşenine aşağıdaki parametreleri ekleyerek sunar -urlencoded form-"biçimi. < /p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Yetkilendirme kodu akışını kimlik bilgilerini (PHP, Java, Python, Ruby, ASP.NET, vb. kullanılarak uygulanan örn., web sunucusu uygulamaları.) gizliliğini koruma özellikli istemciler için optimize edilmiştir.|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Yetkilendirme kodu verme|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|İstemci kimlik bilgileri akışının (uygulamanızı) istemci, denetimi altındaki korunan kaynaklara erişim burada isteyen durumlarda uygundur. Son kullanıcı etkileşimi gerekli olacak şekilde istemci kaynak sahibi olarak kabul edilir.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|İstemci kimlik bilgileri sağlama|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Örtük akış belirli bir yeniden yönlendirme URI'si çalışması için bilinen bir kimlik bilgilerini gizliliğini koruma kapasitesine sahip olmayan istemciler için optimize edilmiştir. Bu istemciler, genellikle bir tarayıcısında JavaScript gibi bir komut dosyası dili kullanılarak uygulanır.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Kapalı verin|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Kaynak sahibi parolası kimlik bilgileri akışının kaynak sahibine bir güven ilişkisi (uygulamanızın), istemci ile cihazın işletim sistemi veya yüksek ayrıcalıklı bir uygulama gibi sahip olduğu durumlarda uygundur. Bu akış sahibin kimlik bilgilerinin (kullanıcı adı ve parola, genellikle etkileşimli form kullanarak) kaynak elde etme olasılığı özellikli istemciler için uygundur.|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Kaynak sahibi parolası kimlik bilgileri verin|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> kaynak sahibi, kullanıcı adı ve parola ile istemci sağlar. < /p\> < p\> istemci bir erişim belirteci yetkilendirme sunucusundan ister '' kaynak sahibinden alınan kimlik bilgileri de dahil olmak üzere tarafından kullanılan s belirteç uç noktası.  İsteği yaparken yetkilendirme sunucusu ile istemci kimliğini doğrular. < /p\> < p\> yetkilendirme sunucusu istemcinin kimliğini doğrular ve kaynak sahibi kimlik bilgilerini doğrular ve geçerliyse, bir erişim belirteci verir. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> istek istemci kimlik doğrulaması başarısız oldu veya geçersizse yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve yanıt aşağıdaki parametreleri içerir. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> istemci belirteç uç noktası için istek Varlık gövdesi bir karakter HTTP UTF-8 kodlamasını ile "uygulama/x-www-form-urlencoded" biçimini kullanarak aşağıdaki parametreleri ekleyerek istekte bulunur. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> erişim belirteci isteği geçerli ve yetkili ise, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı yenileme belirteci sorunları ve bir 20 ile HTTP yanıtının varlık gövdesi için aşağıdaki parametreleri ekleyerek yanıtı oluşturur 0 (Tamam) durum kodu. < /p\>|  
|OAuth2Step_AccessTokenRequest_Name|Erişim belirteci isteği|  
|OAuth2Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|GEREKLİ. İstemci tanımlayıcısı.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|İstemci yetkilendirme sunucusu ile kimlik doğrulaması değil, gerekli.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|GEREKLİ. İstemci tanımlayıcısı.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|GEREKLİ. Yetkilendirme sunucusu tarafından oluşturulan yetkilendirme kodu.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|GEREKLİ. Yetkilendirme sunucusundan alınan yetkilendirme kodu.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayan okunabilir ASCII metni.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayan okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayan okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayan okunabilir ASCII metni.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayan okunabilir ASCII metni.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren okunabilir bir web sayfası tanımlayan bir URI.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren okunabilir bir web sayfası tanımlayan bir URI.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren okunabilir bir web sayfası tanımlayan bir URI.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren okunabilir bir web sayfası tanımlayan bir URI.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren okunabilir bir web sayfası tanımlayan bir URI.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, unauthorized_client, ACCESS_DENIED, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, unauthorized_client, ACCESS_DENIED, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|GEREKLİ. Değeri "authorization_code" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|GEREKLİ. Değeri "client_credentials" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Değer "parola" olarak ayarlanmalıdır.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Kaynak sahibi parolası.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|İSTEĞE BAĞLI. Yeniden yönlendirme uç noktası URI mutlak URI olmalıdır.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|GEREKLİ "redirect_uri" parametresi yetkilendirme isteğine dahil ve bunların değerleri aynı olmalıdır.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|İSTEĞE BAĞLI. Yeniden yönlendirme uç noktası URI mutlak URI olmalıdır.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılan yenileme belirteci.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılan yenileme belirteci.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılan yenileme belirteci.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|GEREKLİ. Değer "code" olarak ayarlanmalıdır.|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|GEREKLİ. Değer olarak ayarlanması gerekir "belirtecine".|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|İSTEĞE BAĞLI. Erişim isteği kapsamı.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|İstemci tarafından istenilen kapsam için aynı ise isteğe BAĞLIDIR; Aksi takdirde, gerekli.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|İSTEĞE BAĞLI. Erişim isteği kapsamı.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|İsteğe bağlı, istemci tarafından istenilen kapsam için aynı ise; Aksi takdirde, gerekli.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|İSTEĞE BAĞLI. Erişim isteği kapsamı.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|İstemci tarafından istenilen kapsam için aynı ise isteğe BAĞLIDIR; Aksi takdirde, gerekli.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|İSTEĞE BAĞLI. Erişim isteği kapsamı.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|İsteğe bağlı, istemci tarafından istenilen kapsam için aynı ise; Aksi takdirde, gerekli.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|"Durum" parametresi istemci yetkilendirme istekte gerekli.  İstemciden alınan tam bir değer.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|ÖNERİLİR. İstek geri çağırma arasındaki durumunu korumak için istemci tarafından kullanılan genel olmayan bir değer.  Yetkilendirme sunucusu, kullanıcı aracısı istemciye yönlendirirken bu değeri içerir.  Parametresi, siteler arası istek sahteciliğini önleme için kullanılmalıdır.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|"Durum" parametresi istemci yetkilendirme istekte gerekli.  İstemciden alınan tam bir değer.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|"Durum" parametresi istemci yetkilendirme istekte gerekli.  İstemciden alınan tam bir değer.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|ÖNERİLİR. İstek geri çağırma arasındaki durumunu korumak için istemci tarafından kullanılan genel olmayan bir değer.  Yetkilendirme sunucusu, kullanıcı aracısı istemciye yönlendirirken bu değeri içerir.  Parametresi, siteler arası istek sahteciliğini önleme için kullanılmalıdır.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|"Durum" parametresi istemci yetkilendirme istekte gerekli.  İstemciden alınan tam bir değer.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|GEREKLİ. Belirteç türü.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|GEREKLİ. Belirteç türü.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|GEREKLİ. Belirteç türü.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|GEREKLİ. Belirteç türü.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Kaynak sahibi kullanıcı adı.|  
|OAuth2UnsupportedTokenType|Belirteç türü '{0}' supporetd değil.|  
|OAuth2InvalidState|Yetkilendirme sunucusundan geçersiz yanıt|  
|OAuth2GrantType_AuthorizationCode|Yetkilendirme kodu|  
|OAuth2GrantType_Implicit|Örtük|  
|OAuth2GrantType_ClientCredentials|İstemci kimlik bilgileri|  
|OAuth2GrantType_ResourceOwnerPassword|Kaynak sahibi parolası|  
|WebDocumentation302Code|302 bulundu|  
|WebDocumentation400Code|400 (Hatalı istek)|  
|OAuth2SendingMethod_AuthHeader|Authorization Üstbilgisi|  
|OAuth2SendingMethod_QueryParam|Sorgu parametresi|  
|OAuth2AuthorizationServerGeneralException|{0} üzerinden erişim yetkisi verme sırasında bir hata oluştu|  
|OAuth2AuthorizationServerCommunicationException|Yetkilendirme sunucusu için bir HTTP bağlantısı kurulamadı veya beklenmedik şekilde kapatıldı.|  
|WebDocumentationOAuth2GeneralErrorMessage|Beklenmeyen bir hata oluştu.|  
|AuthorizationServerCommunicationException|Yetkilendirme sunucusu iletişim özel durumu oluştu. Lütfen yöneticinize başvurun.|  
|TextblockSubscriptionKeyHeaderDescription|Bu API için erişim sağlayan abonelik anahtarı. Bulunan, < bir href ='/ Geliştirici '\>profil < /a\>.|  
|TextblockOAuthHeaderDescription|OAuth 2.0 erişim belirteci elde < t\>{0} < /i\>. Desteklenen sağlama türleri: < i\>{1} < /i\>.|  
|TextblockContentTypeHeaderDescription|API için gönderilen gövdesinin medya türü.|  
|ErrorMessageApiNotAccessible|Aramaya çalıştığınız API şu anda erişilebilir durumda değil. API yayımcı başvurun < bir href = "/ sorunlar"\>burada < /a\>.|  
|ErrorMessageApiTimedout|Aramaya çalıştığınız API yanıt geri dönmek için normalden uzun sürüyor. API yayımcı başvurun < bir href = "/ sorunlar"\>burada < /a\>.|  
|BadRequestParameterExpected|"'{0}' parametresi bekleniyor"|  
|TooltipTextDoubleClickToSelectAll|Tümünü seçmek için çift tıklayın.|  
|TooltipTextHideRevealSecret|Göster/Gizle|  
|ButtonLinkOpenConsole|Deneyin|  
|SectionHeadingRequestBody|İstek gövdesi|  
|SectionHeadingRequestParameters|İstek parametreleri|  
|SectionHeadingRequestUrl|İstek URL'si|  
|SectionHeadingResponse|Yanıt|  
|SectionHeadingRequestHeaders|İstek üstbilgileri|  
|FormLabelSubtextOptional|İsteğe bağlı|  
|SectionHeadingCodeSamples|Kod örnekleri|  
|TextblockOpenidConnectHeaderDescription|Openıd Connect kimliği belirteci elde < t\>{0} < /i\>. Desteklenen sağlama türleri: < i\>{1} < /i\>.|  
  
###  <a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Ad|Metin|  
|----------|----------|  
|LinkLabelBack|Geri|  
|LinkLabelHomePage|Giriş sayfası|  
|LinkLabelSendUsEmail|bize bir e-posta gönderin|  
|PageTitleError|Üzgünüz, istenen sayfa hizmet veren bir sorun oluştu|  
|TextblockPotentialCauseIntermittentIssue|Bu zaten kayboluyor aralıklı veri erişim sorunu olabilir.|  
|TextblockPotentialCauseOldLink|Üzerinde tıkladığınız bağlantı eski ve doğru konuma artık noktası değil.|  
|TextblockPotentialCauseTechnicalProblem|Bizden teknik bir sorun olabilir.|  
|TextblockPotentialSolutionRefresh|Sayfayı yenilemeyi deneyin.|  
|TextblockPotentialSolutionStartOver|Baştan başlamak bizim {0}.|  
|TextblockPotentialSolutionTryAgain|{0} gidin ve yeniden gerçekleştirilen eylem deneyin.|  
|TextReportProblem|geçebiliriz hemen nelerin yanlış gittiğini açıklayan {0} ve biz bakmak.|  
|TitlePotentialCause|Olası neden|  
|TitlePotentialSolution|Muhtemelen yalnızca geçici sorunu deneyin birkaç olduğu|  
  
###  <a name="IssuesStrings"></a>IssuesStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebIssuesIndexTitle|Sorunlar|  
|WebIssuesNoActiveSubscriptions|Hiç etkin aboneliğiniz yok. Bir sorunu bildirmek üzere bir ürün için abone olmanız gerekir.|  
|WebIssuesNotSignin|Açmadınız. Bir sorunu bildirmek veya yorum göndermek için {0} Lütfen.|  
|WebIssuesReportIssueButton|Rapor sorunu|  
|WebIssuesSignIn|oturum aç|  
|WebIssuesStatusReportedBy|Durum: {0} &#124; {1} tarafından bildirilen|  
  
###  <a name="NotFoundStrings"></a>NotFoundStrings  
  
|Ad|Metin|  
|----------|----------|  
|LinkLabelHomePage|Giriş sayfası|  
|LinkLabelSendUsEmail|bize bir e-posta gönderin|  
|PageTitleNotFound|Üzgünüz, aradığınız sayfayı bulamıyoruz|  
|TextblockPotentialCauseMisspelledUrl|İçinde yazdıysanız URL yanlış.|  
|TextblockPotentialCauseOldLink|Üzerinde tıkladığınız bağlantı eski ve doğru konuma artık noktası değil.|  
|TextblockPotentialSolutionRetype|URL yeniden yazmayı deneyin.|  
|TextblockPotentialSolutionStartOver|Baştan başlamak bizim {0}.|  
|TextReportProblem|geçebiliriz hemen nelerin yanlış gittiğini açıklayan {0} ve biz bakmak.|  
|TitlePotentialCause|Olası neden|  
|TitlePotentialSolution|Olası çözüm|  
  
###  <a name="ProductDetailsStrings"></a>ProductDetailsStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebProductsAgreement|{0} ürün abone olarak ediyorum `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Kullanım Koşulları|  
|WebProductsSubscribeButton|Abone olun|  
|WebProductsUsageLimitsHeader|Kullanım sınırları|  
|WebProductsYouAreNotSubscribed|Bu ürüne abone olur.|  
|WebProductsYouRequestedSubscription|Bu ürün için abonelik istedi.|  
|ErrorYouNeedtoAgreeWithLegalTerms|Devam etmeden önce kullanım hükümlerini kabul etmeniz gerekir.|  
|ButtonLabelAddSubscription|Abonelik ekleme|  
|LinkLabelChangeSubscriptionName|değiştirme|  
|ButtonLabelConfirm|Onayla|  
|TextblockMultipleSubscriptionsCount|Bu ürün için {0} abonelikleri vardır:|  
|TextblockSingleSubscriptionsCount|Bu ürün için {0} abonelik vardır:|  
|TextblockSingleApisCount|Bu ürün {0} API içerir:|  
|TextblockMultipleApisCount|Bu ürün {0} API'leri içeriyor:|  
|TextblockHeaderSubscribe|Ürüne abone|  
|TextblockSubscriptionDescription|Yeni bir abonelik şu şekilde oluşturulur:|  
|TextblockSubscriptionLimitReached|Abonelik sınırına ulaşıldı.|  
  
###  <a name="ProductsStrings"></a>ProductsStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleProducts|Ürünler|  
  
###  <a name="ProviderInfoStrings"></a>ProviderInfoStrings  
  
|Ad|Metin|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Oturum açma yöneticileri tarafından şu anda devre dışı bırakılır.|  
|TextboxExternalIdentitiesSigninInvitation|Alternatif olarak, oturum açın|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Oturum açın:|  
  
###  <a name="SigninResources"></a>SigninResources  
  
|Ad|Metin|  
|----------|----------|  
|PrincipalNotFound|Asıl bulunamadı veya imzası geçersiz|  
|ErrorSsoAuthenticationFailed|SSO kimlik doğrulaması başarısız oldu|  
|ErrorSsoAuthenticationFailedDetailed|Sağlanan geçersiz belirteç veya imza doğrulanamıyor.|  
|ErrorSsoTokenInvalid|SSO belirteci geçersiz|  
|ValidationErrorSpecificEmailAlreadyExists|E-posta '{0}' zaten kayıtlı|  
|ValidationErrorSpecificEmailInvalid|E-posta '{0}' geçerli değil|  
|ValidationErrorPasswordInvalid|Parola geçersiz. Lütfen hataları düzeltin ve yeniden deneyin.|  
|PropertyTooShort|{0} çok kısa|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Geçersiz e-posta adresi.|  
|ValidationMessageNewPasswordConfirmationRequired|Yeni parolayı onaylayın|  
|ValidationErrorPasswordConfirmationRequired|Parola boş olduğunu onaylayın|  
|WebAuthenticationEmailChangeNotice|{0} şekilde değişiklik onay e-posta açıktır. Lütfen yeni e-posta adresinizi doğrulamak için içindeki yönergeleri izleyin. Sonraki birkaç dakika içinde e-posta kutunuza ulaşmaz, önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailChangeNoticeHeader|E-posta değişiklik isteği başarıyla işlendi|  
|WebAuthenticationEmailChangeNoticeTitle|E-posta değişiklik istendi|  
|WebAuthenticationEmailHasBeenRevertedNotice|Size e-posta zaten mevcut. İstek geri döndürüldü|  
|ValidationErrorEmailAlreadyExists|E-posta zaten var|  
|ValidationErrorEmailInvalid|Geçersiz e-posta adresi|  
|TextboxLabelEmail|E-posta|  
|ValidationErrorEmailRequired|E-posta gereklidir.|  
|WebAuthenticationErrorNoticeHeader|Hata|  
|WebAuthenticationFieldLengthErrorMessage|{0} {1} en fazla şu uzunlukta olmalıdır|  
|TextboxLabelEmailFirstName|Ad|  
|ValidationErrorFirstNameRequired|Ad gereklidir.|  
|ValidationErrorFirstNameInvalid|Geçersiz ad|  
|NoticeInvalidInvitationToken|Lütfen onay bağlantıları yalnızca 48 saat için geçerli olduğunu unutmayın. Bu süre içinde hala varsa, lütfen bağlantınızı doğru olduğundan emin olun. Bağlantı süresi dolmuşsa, lütfen onaylamak için çalıştığınız eylemi yineleyin.|  
|NoticeHeaderInvalidInvitationToken|Geçersiz Davet belirteci|  
|NoticeTitleInvalidInvitationToken|Doğrulama hatası|  
|WebAuthenticationLastNameInvalidErrorMessage|Geçersiz son adı|  
|TextboxLabelEmailLastName|Soyadı|  
|ValidationErrorLastNameRequired|Son adı gereklidir.|  
|WebAuthenticationLinkExpiredNotice|Onay bağlantısı size gönderilen süresi doldu. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|Parola sıfırlama bağlantısı geçersiz veya süresi dolmuş.|  
|WebAuthenticationLinkExpiredNoticeTitle|Gönderilen bağlantı|  
|WebAuthenticationNewPasswordLabel|Yeni parola|  
|ValidationMessageNewPasswordRequired|Yeni parola zorunludur.|  
|TextboxLabelNotificationsSenderEmail|Bildirimleri gönderen e-posta|  
|TextboxLabelOrganizationName|Kuruluş adı|  
|WebAuthenticationOrganizationRequiredErrorMessage|Kuruluş adı yok|  
|WebAuthenticationPasswordChangedNotice|Parolanızı başarıyla güncelleştirildi|  
|WebAuthenticationPasswordChangedNoticeTitle|Parola güncelleştirildi|  
|WebAuthenticationPasswordCompareErrorMessage|Parolalar eşleşmiyor|  
|WebAuthenticationPasswordConfirmLabel|Parolayı onayla|  
|ValidationErrorPasswordInvalidDetailed|Parola çok zayıf değil.|  
|WebAuthenticationPasswordLabel|Parola|  
|ValidationErrorPasswordRequired|Parola gereklidir.|  
|WebAuthenticationPasswordResetSendNotice|Parola onayı {0} şekilde e-posta açıktır değiştirin. Lütfen parola değişikliği işlemine devam etmek için e-posta içindeki yönergeleri izleyin.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Parola sıfırlama isteği başarıyla işlendi|  
|WebAuthenticationPasswordResetSendNoticeTitle|Parola sıfırlama istendi.|  
|WebAuthenticationRequestNotFoundNotice|İsteği bulunamadı|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Bildirimleri gönderen e-posta boştur|  
|WebAuthenticationSigninPasswordLabel|Lütfen bir parola girerek Değişikliğini Onayla|  
|WebAuthenticationSignupConfirmNotice|Kayıt bir onay e-postadır {0} yolda. < br /\> Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin < br /\> sonraki birkaç dakika içinde e-posta kutunuzda ulaşmaz Lütfen, gereksiz kontrol edin e-posta klasör.|  
|WebAuthenticationSignupConfirmNoticeHeader|Hesabınız başarıyla oluşturuldu|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Kayıt onayı e-posta yeniden gönderildi|  
|WebAuthenticationSignupConfirmNoticeTitle|Hesabı oluşturuldu|  
|WebAuthenticationTokenRequiredErrorMessage|Belirteç boş|  
|WebAuthenticationUserAlreadyRegisteredNotice|Bu e-posta ile bir kullanıcı zaten sistemde kayıtlı görünüyor. Parolanızı unuttuysanız, geri yüklemek veya destek ekibimiz iletişim kurmak Lütfen deneyin.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Kullanıcı zaten kayıtlı|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Zaten kayıtlı|  
|ButtonLabelChangePassword|Parolayı Değiştir|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştirme|  
|ButtonLabelCloseAccount|Hesabı Kapat|  
|WebAuthenticationInvalidCaptchaErrorMessage|Girilen metin, resim metni eşleşmiyor. Lütfen yeniden deneyin.|  
|ValidationErrorCredentialsInvalid|E-posta veya parola geçersiz. Lütfen hataları düzeltin ve yeniden deneyin.|  
|WebAuthenticationRequestIsNotValid|İsteği geçerli değil|  
|WebAuthenticationUserIsNotConfirm|Oturum açmak denemeden önce lütfen Kaydınızı onaylayın.|  
|WebAuthenticationInvalidEmailFormated|E-posta geçersiz: {0}|  
|WebAuthenticationUserNotFound|Kullanıcı bulunamadı|  
|WebAuthenticationTenantNotRegistered|Hesabınız, bu portalına erişmek için yetkili değil bir Azure Active Directory kiracısına ait.|  
|WebAuthenticationAuthenticationFailed|Kimlik doğrulaması başarısız oldu.|  
|WebAuthenticationGooglePlusNotEnabled|Kimlik doğrulaması başarısız oldu. Uygulama yetki sonra lütfen kişi, Google kimlik doğrulamasının emin olmak için yönetici doğru şekilde yapılandırılır.|  
|ValidationErrorAllowedTenantIsRequired|İzin verilen Kiracı gereklidir|  
|ValidationErrorTenantIsNotValid|Azure Active Directory Kiracı '{0}' geçerli değil.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|{0} hesabınızı kullanarak oturum açın|  
|WebAuthenticationUserLimitNotice|Bu hizmet, izin verilen kullanıcıların sayısı üst sınırına. Lütfen `<a href="mailto:{0}"\>contact the administrator</a\>` kendi hizmet yükseltin ve kullanıcı kaydı yeniden etkinleştirin.|  
|WebAuthenticationUserLimitNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserLimitNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserRegistrationDisabledNotice|Kullanıcı kaydı yönetici tarafından devre dışı bırakıldı. Lütfen dış kimlik sağlayıcısı ile oturum açın.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationSignupPendingConfirmationNotice|Hesabınızı oluşturmayı uygulayabilmeniz için önce biz e-posta adresinizi doğrulamanız gerekir. Bir e-posta {0} gönderdik. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. E-posta sonraki birkaç dakika içinde ulaşırsa değil, Lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Onaylanmayan bir hesap için e-posta adresi {0} bulduk. Hesabınızı oluşturma işlemini tamamlamak için size e-posta adresinizi doğrulamanız gerekir. Bir e-posta {0} gönderdik. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. Lütfen e-posta sonraki birkaç dakika içinde ulaşırsa değil, önemsiz e-posta klasörünüzü kontrol edin|  
|WebAuthenticationSignupConfirmationAlmostDone|Neredeyse bitti|  
|WebAuthenticationSignupConfirmationEmailSent|Bir e-posta {0} gönderdik. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. E-posta sonraki birkaç dakika içinde ulaşırsa değil, Lütfen önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailSentNotificationMessage|E-posta {0} başarıyla gönderildi|  
|WebAuthenticationNoAadTenantConfigured|Azure Active Directory Kiracı hizmeti için yapılandırılmış.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Kabul ediyorum `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Lütfen gözden geçirin.`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Kullanım Koşulları|  
|ValidationMessageConsentNotAccepted|Devam etmeden önce kullanım hükümlerini kabul etmeniz gerekir.|  
  
###  <a name="SigninStrings"></a>SigninStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebAuthenticationForgotPassword|Parolanızı mı unuttunuz?|  
|WebAuthenticationIfAdministrator|Bir yöneticiyseniz, oturum açmanız gerekir `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Üyede kullanılamaz henüz? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Bu bilgisayarda Beni anımsa|  
|WebAuthenticationSigininWithPassword|Kullanıcı adı ve parola ile oturum|  
|WebAuthenticationSigninTitle|Oturum aç|  
|WebAuthenticationSignUpNow|Hemen kaydolun|  
  
###  <a name="SignupStrings"></a>SignupStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleSignup|Kaydolma|  
|WebAuthenticationAlreadyAMember|Zaten üye misiniz?|  
|WebAuthenticationCreateNewAccount|Yeni bir API Management hesabı oluşturma|  
|WebAuthenticationSigninNow|Şimdi oturum açın|  
|ButtonLabelSignup|Kaydolma|  
  
###  <a name="SubscriptionListStrings"></a>SubscriptionListStrings  
  
|Ad|Metin|  
|----------|----------|  
|SubscriptionCancelConfirmation|Bu aboneliği iptal etmek istediğinizden emin misiniz?|  
|SubscriptionRenewConfirmation|Bu aboneliği yenilemek istediğinizden emin misiniz?|  
|WebDevelopersManageSubscriptions|Abonelikleri yönetme|  
|WebDevelopersPrimaryKey|Birincil anahtar|  
|WebDevelopersRegenerateLink|Yeniden Oluştur|  
|WebDevelopersSecondaryKey|İkincil anahtar|  
|ButtonLabelShowKey|Göster|  
|ButtonLabelRenewSubscription|Yenile|  
|WebDevelopersSubscriptionReqested|{0} istendi|  
|WebDevelopersSubscriptionRequestedState|İstenen|  
|WebDevelopersSubscriptionTableNameHeader|Ad|  
|WebDevelopersSubscriptionTableStateHeader|Durum|  
|WebDevelopersUsageStatisticsLink|Analytics Raporları|  
|WebDevelopersYourSubscriptions|Aboneliklerinizi|  
|SubscriptionPropertyLabelRequestedDate|Üzerinde istenen|  
|SubscriptionPropertyLabelStartedDate|Üzerinde başlatıldı|  
|PageTitleRenameSubscription|Aboneliği yeniden adlandırılamadı|  
|SubscriptionPropertyLabelName|Abonelik adı|  
  
###  <a name="SubscriptionStrings"></a>SubscriptionStrings  
  
|Ad|Metin|  
|----------|----------|  
|SectionHeadingCloseAccount|Hesabınızı kapatmak için mi arıyorsunuz?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Gizle|  
|ButtonLabelRegenerateKey|Yeniden Oluştur|  
|InformationMessageKeyWasRegenerated|Bu anahtarı yeniden oluşturmak istediğinizden emin misiniz?|  
|ButtonLabelShowKey|Göster|  
  
###  <a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profil güncelleştirme|  
|PageTitleUpdateProfile|Hesap bilgilerini güncelleştir|  
  
###  <a name="UserProfile"></a>Kullanıcı profili  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştirme|  
|ButtonLabelChangePassword|Parolayı Değiştir|  
|ButtonLabelCloseAccount|Hesabı Kapat|  
|TextboxLabelEmail|E-posta|  
|TextboxLabelEmailFirstName|Ad|  
|TextboxLabelEmailLastName|Soyadı|  
|TextboxLabelNotificationsSenderEmail|Bildirimleri gönderen e-posta|  
|TextboxLabelOrganizationName|Kuruluş adı|  
|SubscriptionStateActive|Etkin|  
|SubscriptionStateCancelled|İptal edildi|  
|SubscriptionStateExpired|Süresi dolmuş|  
|SubscriptionStateRejected|Reddetti|  
|SubscriptionStateRequested|İstenen|  
|SubscriptionStateSuspended|askıya alındı|  
|DefaultSubscriptionNameTemplate|{0} (varsayılan)|  
|SubscriptionNameTemplate|Geliştirici erişim #{0}|  
|TextboxLabelSubscriptionName|Abonelik adı|  
|ValidationMessageSubscriptionNameRequired|Abonelik adı boş olamaz.|  
|ApiManagementUserLimitReached|Bu hizmet, izin verilen kullanıcıların sayısı üst sınırına. Lütfen daha yüksek bir fiyatlandırma katmanına yükseltin.|  
  
##  <a name="glyphs"></a>Karakter kaynakları  
 API Management Geliştirici Portalı şablonları gelen karakterlerin kullanabileceğiniz [Glyphicons önyükleme gelen](http://getbootstrap.com/components/#glyphicons). Bu karakterlerin 250'den fazla karakterlerin yazı tipi biçiminden içinde içerir [Glyphicon](http://glyphicons.com/) Halflings ayarlayın. Bu kümesinden karakter kullanmak için aşağıdaki sözdizimini kullanın.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Karakterlerin tam listesi için bkz: [Glyphicons önyükleme gelen](http://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Sonraki adımlar
Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [şablonları kullanarak API Management Geliştirici Portalı nasıl özelleştireceğinizi](api-management-developer-portal-templates.md).
