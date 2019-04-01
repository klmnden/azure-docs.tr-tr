---
title: Azure API Management şablon kaynakları | Microsoft Docs
description: Azure API Management'ta Geliştirici portal şablonları için kullanılabilir kaynak türleri hakkında bilgi edinin.
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
ms.openlocfilehash: 673dcbeb630899eebc328cd4fae16f7fe8f47a55
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757614"
---
# <a name="azure-api-management-template-resources"></a>Azure API Management şablon kaynakları
Azure API Management aşağıdaki türdeki kaynakları portal şablonları kullanılmak üzere Geliştirici sağlar.  
  
-   [Dize kaynakları](#strings)  
  
-   [Glif kaynakları](#glyphs)  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="strings"></a> Dize kaynakları  
 API Management Geliştirici Portalı kullanmak için kapsamlı dize kaynakları sağlar. Bu kaynaklar, tüm API Management tarafından desteklenen dilleri yerelleştirilmiştir. Varsayılan şablonlar kümesi bu kaynakları sayfa üstbilgilerinde, etiketler ve Geliştirici Portalı'nda görüntülenen tüm sabit dizeler için kullanır. Bir dize kaynağı şablonlarınızın kullanmak için aşağıdaki örnekte gösterildiği gibi dize adından önce gelen kaynak dizesi ön ekini belirtin.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Aşağıdaki örnek Ürün liste şablonu ve bir görüntüler **ürünleri** sayfanın üstünde.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
Yerelleştirme şunlardan desteklenir:

| Yerel Ayar    | Dil               |
|-----------|------------------------|
| "tr"      | "İngilizce"              |
| "cs"      | "Čeština"              |
| "de"      | "Deutsch"              |
| "es"      | "Español"              |
| "fr"      | "Français"             |
| "hu"      | "Magyar"               |
| ""      | "Italiano"             |
| "ja-JP"   | "日本語"                |
| "ko"      | "한국어"                |
| "nl"      | "Nederlands"           |
| "pl"      | "Polski"               |
| "pt-br"   | "Português (Brezilya)"   |
| "pt-pt"   | "Português (Portekiz)" |
| "ru"      | "Русский"              |
| "sv"      | "Svenska"              |
| "tr"      | "Türkçe"               |
| "zh-hans" | "中文(简体)"           |
| "zh-hant" | "中文(繁體)"           |

 Dize kaynaklarını kullanılmak üzere Geliştirici portal şablonları için aşağıdaki tablolara bakın. Tablo adını bu tablodaki dize kaynakları için önek olarak kullanın.  
  
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
  
###  <a name="ApisStrings"></a> ApisStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleApis|API'ler|  
  
###  <a name="AppDetailsStrings"></a> AppDetailsStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebApplicationsDetailsTitle|Uygulama önizlemesi|  
|WebApplicationsRequirementsHeader|Gereksinimler|  
|WebApplicationsScreenshotAlt|Ekran Görüntüsü|  
|WebApplicationsScreenshotsHeader|Ekran görüntüleri|  
  
###  <a name="ApplicationListStrings"></a> ApplicationListStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Uygulamayı kaldırmak istediğinizden emin misiniz?|  
|WebDevelopersAppNotPublished|Yayımlanmadı|  
|WebDevelopersAppNotSubmitted|Gönderilmedi|  
|WebDevelopersAppTableCategoryHeader|Kategori|  
|WebDevelopersAppTableNameHeader|Ad|  
|WebDevelopersAppTableStateHeader|Durum|  
|WebDevelopersEditLink|Düzenle|  
|WebDevelopersRegisterAppLink|Uygulamayı kaydet|  
|WebDevelopersRemoveLink|Kaldır|  
|WebDevelopersSubmitLink|Gönder|  
|WebDevelopersYourApplicationsHeader|Uygulamalarınız|  
  
###  <a name="AppStrings"></a> AppStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebApplicationsHeader|Uygulamalar|  
  
###  <a name="CommonResources"></a> CommonResources  
  
|Ad|Metin|  
|----------|----------|  
|NoItemsToDisplay|Sonuç bulunamadı.|  
|GeneralExceptionMessage|Doğru değildir. Geçici bir sorun veya hata olabilir. Lütfen yeniden deneyin.|  
|GeneralJsonExceptionMessage|Doğru değildir. Geçici bir sorun veya hata olabilir. Lütfen sayfayı yeniden yükleyin ve yeniden deneyin.|  
|ConfirmationMessageUnsavedChanges|Bazı kaydedilmemiş değişiklikler var. İptal etmek ve değişiklikleri atmak istediğinizden emin misiniz?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|Http İstek Gövdesi çok büyük.|  
  
###  <a name="CommonStrings"></a> CommonStrings  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelCancel|İptal|  
|ButtonLabelSave|Kaydet|  
|GeneralExceptionMessage|Doğru değildir. Geçici bir sorun veya hata olabilir. Lütfen yeniden deneyin.|  
|NoItemsToDisplay|Görüntülenecek öğe yok.|  
|PagerButtonLabelFirst|İlk|  
|PagerButtonLabelLast|Son|  
|PagerButtonLabelNext|Sonraki|  
|PagerButtonLabelPrevious|Önceki|  
|PagerLabelPageNOfM|Sayfa {0} , {1}|  
|PasswordTooShort|Parola çok kısa|  
|EmailAsPassword|Parola olarak e-postanızı kullanmayın|  
|PasswordSameAsUserName|Parolanız kullanıcı adınızı içeremez|  
|PasswordTwoCharacterClasses|Farklı karakter sınıfları kullanın|  
|PasswordTooManyRepetitions|Çok fazla tekrar var|  
|PasswordSequenceFound|Parolanız sıralı karakterler içeriyor|  
|PagerLabelPageSize|Sayfa boyutu|  
|CurtainLabelLoading|Yükleniyor...|  
|TablePlaceholderNothingToDisplay|Seçili dönem ve kapsam için veri yok|  
|ButtonLabelClose|Kapat|  
  
###  <a name="Documentation"></a> Belgeleri  
  
|Ad|Metin|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Geçersiz üst bilgi '{0}'|  
|WebDocumentationInvalidRequestErrorMessage|Geçersiz İstek URL'si|  
|TextboxLabelAccessToken|Access token *|  
|DropdownOptionPrimaryKeyFormat|Birincil-{0}|  
|DropdownOptionSecondaryKeyFormat|İkincil-{0}|  
|WebDocumentationSubscriptionKeyText|Abonelik anahtarınız|  
|WebDocumentationTemplatesAddHeaders|Gerekli HTTP üst bilgilerini ekleyin|  
|WebDocumentationTemplatesBasicAuthSample|Basic Authorization Örneği|  
|WebDocumentationTemplatesCurlForBasicAuth|Basic Authorization için şunu kullanın: --user {username}:{password}|  
|WebDocumentationTemplatesCurlValuesForPath|Yol parametreleri ({...} olarak gösterilir) için değerleri, sorgu parametreleri için abonelik anahtarınızı ve değerleri belirtin|  
|WebDocumentationTemplatesDeveloperKey|Abonelik anahtarınızı belirtin|  
|WebDocumentationTemplatesJavaApache|Bu örnek HTTP Components'tan (Apache HTTP istemcisini kullanır http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|İsteğe bağlı parametreler için değerleri gereken şekilde belirtin|  
|WebDocumentationTemplatesPhpPackage|Bu örnek HTTP_Request2 paketini kullanır. (daha fazla bilgi için: https://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Gerekirse yol parametreleri ({...} olarak gösterilir) ve istek gövdesi için değerler belirtin|  
|WebDocumentationTemplatesRequestBody|İstek gövdesi belirtin|  
|WebDocumentationTemplatesRequiredParams|Aşağıdaki gerekli parametreler için değerleri belirtin|  
|WebDocumentationTemplatesValuesForPath|Yol parametreleri ({...} olarak gösterilir) için değerler belirtin|  
|OAuth2AuthorizationEndpointDescription|Yetkilendirme uç noktası, kaynak sahibiyle etkileşimde bulunmak ve bir yetkilendirme izni almak için kullanılır.|  
|OAuth2AuthorizationEndpointName|Yetkilendirme uç noktası|  
|OAuth2TokenEndpointDescription|Belirteç uç noktası, istemci tarafından yetkilendirme izni veya yenileme belirtecini sunarak bir erişim belirteci almak için kullanılır.|  
|OAuth2TokenEndpointName|Belirteç uç noktası|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> yetkilendirme uç noktasına kaynak sahibinin kullanıcı aracısını yönlendirerek istemci akışı başlatır.  İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve geri erişim verilen (veya reddedilen sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir.     < /p\> < p\> yetkilendirme sunucusu (kullanıcı aracısı) aracılığıyla kaynak sahibi kimliğini doğrular ve kaynak sahibi erişim izni verilir veya istemci erişim isteğini reddederse olup olmadığını belirler.     < /p\> < p\> varsayılarak kaynak sahibi erişim izni verirse yetkilendirme sunucusu kullanıcı aracısını kullanarak sağlanan URI yeniden yönlendirmesi istemciye yönlendirir (istekte veya istemci registrati sırasında önceki üzerinde).  Yeniden yönlendirme URI'si bir yetkilendirme kodunu ve istemci tarafından daha önce sağlanan yerel durumları içerir.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> istek geçersizse kullanıcı erişim isteğini reddederse, istemci yeniden yönlendirmeye eklenen aşağıdaki parametreler kullanılarak bildirilir: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> istemci uygulamasının yetkilendirme uç noktasına OAuth işlemini başlatmak için kullanıcıyı göndermesi gerekir.          Yetkilendirme uç noktasında kullanıcı kimliğini doğrular ve ardından verir veya uygulamaya erişimi engeller.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> varsayılarak kaynak sahibi erişim izni verirse yetkilendirme sunucusu kullanıcı aracısını kullanarak sağlanan URI yeniden yönlendirmesi istemciye yönlendirir (istekte veya istemci kaydında) daha eski.  Yeniden yönlendirme URI'si bir yetkilendirme kodunu ve istemci tarafından daha önce sağlanan yerel durumları içerir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> istemci yetkilendirme sunucusundan bir erişim belirteci ister '' önceki adımda alınan yetkilendirme kodunu ekleyerek s belirteç uç noktası.  İsteği yaparken, istemci yetkilendirme sunucusunda kimliğini doğrular.  İstemci doğrulama yetkilendirme kodunu almak için URI kullanılan yeniden yönlendirme ekler. < /p\> < p\> yetkilendirme sunucusu istemcinin kimliğini doğrular, yetkilendirme kodunu doğrular ve alınan URI yeniden yönlendirmesi (C) adımında istemciyi yeniden yönlendirmek için kullanılan URI eşleşmesini sağlar.  Geçerliyse yetkilendirme sunucusu geri bir erişim belirteci ve yenileme belirteci ile isteğe bağlı olarak yanıt verir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> istek istemci kimlik doğrulaması başarısız oldu veya geçersizse yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve aşağıdaki parametreleri yanıta ekler. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> istemci, bir karakter HTTP UTF-8 kodlaması ile "application/x-www-form-urlencoded işlemek" biçimini kullanarak aşağıdaki parametreleri istek Varlık gövdesi göndererek belirteç uç noktasına bir istek gönderir. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı bir yenileme belirteci verir ve 200 (Tamam) durum kodu ile HTTP yanıtının entity-body aşağıdaki parametreleri ekleyerek yanıtı oluşturur. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> istemci yetkilendirme sunucusunda kimlik doğrulaması yapar ve belirteç uç noktasından bir erişim belirteci ister. < /p\> < p\> yetkilendirme sunucusu istemcinin kimliğini doğrular ve geçerliyse bir erişim belirteci verir. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> istek istemci kimlik doğrulaması başarısız oldu veya geçersizse yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve aşağıdaki parametreleri yanıta ekler. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> istemci, bir karakter HTTP UTF-8 kodlaması ile "application/x-www-form-urlencoded işlemek" biçimini kullanarak aşağıdaki parametreleri istek Varlık gövdesi ekleyerek belirteç uç noktasına bir istek gönderir. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> erişim belirteci isteği geçerliyse ve ise, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı bir yenileme belirteci verir ve 200 ile HTTP yanıtının entity-body aşağıdaki parametreleri ekleyerek yanıtı oluşturur.  (Tamam) durum kodu. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> kaynak sahibi yönlendirerek istemci akışı başlatan '' s yetkilendirme uç noktası için kullanıcı aracısı.  İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve geri erişim verilen (veya reddedilen sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. < /p\> < p\> yetkilendirme sunucusu (kullanıcı aracısı) aracılığıyla kaynak sahibi kimliğini doğrular ve kaynak sahibi erişim izni verilir veya istemciyi engellediği olup olmadığını belirler. '' s erişim isteği. < /p\> < p\> varsayılarak kaynak sahibi erişim izni verirse yetkilendirme sunucusu kullanıcı aracısını kullanarak daha önce sağlanan URI yeniden yönlendirmesi istemciye yönlendirir.  Yeniden yönlendirme URI'si URI parçası erişim belirtecini içerir. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> kaynak sahibi erişim isteğini reddederse veya istek eksik ya da geçersiz yeniden yönlendirme URI'si dışında bir nedenle başarısız olursa yetkilendirme sunucusu istemci parça bileşeni o aşağıdaki parametreleri ekleyerek bilgilendirir f "application/x-www-form-urlencoded işlemek" biçimini kullanarak URI yeniden yönlendirmesi. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> istemci uygulamasının yetkilendirme uç noktasına OAuth işlemini başlatmak için kullanıcıyı göndermesi gerekir.      Yetkilendirme uç noktasında kullanıcı kimliğini doğrular ve ardından verir veya uygulamaya erişimi engeller. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> kaynak sahibi erişim isteğini verirse yetkilendirme sunucusu bir erişim belirteci verir ve "application/x-www kullanarak URI yeniden yönlendirmesi parça bileşenine aşağıdaki parametreleri ekleyerek istemciye teslim -urlencoded form-"biçimi. </p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Yetkilendirme kodu akışı kimlik bilgilerinin gizliliğini koruma özelliğine sahip istemciler (örneğin PHP, Java, Python, Ruby, ASP.NET vb. kullanılarak uygulanan web sunucusu uygulamaları) için en iyi duruma getirilmiştir.|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Yetkilendirme Kodu verme|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|İstemci kimlik bilgileri akışı, istemcinin (uygulamanızın) denetimi altındaki korumalı kaynaklara erişimi burada istediği durumlarda uygundur. Son kullanıcı etkileşimi gerekli değildir, dolayısıyla istemci bir kaynak sahibi olarak kabul edilir.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|İstemci Kimlik Bilgileri verme|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Örtük akış, belirli bir yeniden yönlendirme URI'si çalıştırdığı bilinen kimlik bilgilerinin gizliliğini koruma özelliğine sahip olmayan istemciler için optimize edilmiştir. Bu istemciler genellikle bir tarayıcıda JavaScript gibi bir betik oluşturma dili kullanılarak uygulanır.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Örtük izin verme|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Kaynak sahibi parola kimlik bilgileri akışı, kaynak sahibi bir güven ilişkisi istemcinin (uygulamanızın), cihazın işletim sistemi veya üst düzeyde ayrıcalıklı bir uygulama gibi sahip olduğu durumlarda uygundur. Bu akış, kaynak sahibinin kimlik bilgilerini (kullanıcı adı ve parola, genellikle etkileşimli bir form kullanılarak) alma özelliğine sahip istemciler için uygundur.|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Kaynak Sahibi Parola Kimlik Bilgileri verme|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> kendi kullanıcı adı ve parola ile kaynak sahibi istemciye sağlar. < /p\> < p\> istemci yetkilendirme sunucusundan bir erişim belirteci ister '' kaynak sahibinden alınan kimlik bilgilerini ekleyerek s belirteç uç noktası.  İsteği yaparken, istemci yetkilendirme sunucusunda kimliğini doğrular. < /p\> < p\> yetkilendirme sunucusu istemcinin kimliğini doğrular ve kaynak sahibinin kimlik bilgilerini doğrular ve geçerliyse bir erişim belirteci verir. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> istek istemci kimlik doğrulaması başarısız oldu veya geçersizse yetkilendirme sunucusu (Aksi belirtilmediği sürece) bir HTTP 400 (Hatalı istek) durum koduyla yanıt verir ve aşağıdaki parametreleri yanıta ekler. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> istemci, bir karakter HTTP UTF-8 kodlaması ile "application/x-www-form-urlencoded işlemek" biçimini kullanarak aşağıdaki parametreleri istek Varlık gövdesi ekleyerek belirteç uç noktasına bir istek gönderir. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> erişim belirteci isteği geçerliyse ve ise, yetkilendirme sunucusu bir erişim belirteci ve isteğe bağlı bir yenileme belirteci verir ve 20 ile HTTP yanıtının entity-body aşağıdaki parametreleri ekleyerek yanıtı oluşturur. 0 (Tamam) durum kodu. </p\>|  
|OAuth2Step_AccessTokenRequest_Name|Erişim belirteci isteği|  
|OAuth2Step_AuthorizationRequest_Name|Yetkilendirme isteği|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|GEREKLİ. Yetkilendirme sunucusu tarafından verilen erişim belirteci.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|GEREKLİ. İstemci tanımlayıcısı.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|İstemci, yetkilendirme sunucusu ile kimlik doğrulaması yapmıyorsa ZORUNLU.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|GEREKLİ. İstemci tanımlayıcısı.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|GEREKLİ. Yetkilendirme sunucusu tarafından oluşturulan yetkilendirme kodu.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|GEREKLİ. Yetkilendirme sunucusundan alınan yetkilendirme kodu.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayarak kullanıcı tarafından okunabilen ASCII metni.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayarak kullanıcı tarafından okunabilen ASCII metni.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayarak kullanıcı tarafından okunabilen ASCII metni.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayarak kullanıcı tarafından okunabilen ASCII metni.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Ek bilgi sağlayarak kullanıcı tarafından okunabilen ASCII metni.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren kullanıcı tarafından okunabilen bir web sayfasını tanımlayan URI.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren kullanıcı tarafından okunabilen bir web sayfasını tanımlayan URI.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren kullanıcı tarafından okunabilen bir web sayfasını tanımlayan URI.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren kullanıcı tarafından okunabilen bir web sayfasını tanımlayan URI.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|İSTEĞE BAĞLI. Hata hakkında bilgi içeren kullanıcı tarafından okunabilen bir web sayfasını tanımlayan URI.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|GEREKLİ. Aşağıdakiler arasından tek bir ASCII hata kodu: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|ÖNERİLİR. Erişim belirtecinin saniye cinsinden yaşam süresi.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|GEREKLİ. Değer "authorization_code" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|GEREKLİ. Değer "client_credentials" olarak ayarlanmalıdır.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Değer, "parola" olarak ayarlanmalıdır.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Kaynak sahibi parolası.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|İSTEĞE BAĞLI. Yeniden yönlendirme uç nokta URI'si mutlak bir URI olmalıdır.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|Yetkilendirme isteğine "redirect_uri" parametresi eklendiyse ZORUNLUDUR, değerler aynı OLMALIDIR.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|İSTEĞE BAĞLI. Yeniden yönlendirme uç nokta URI'si mutlak bir URI olmalıdır.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılabilecek yenileme belirteci.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılabilecek yenileme belirteci.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|İSTEĞE BAĞLI. Yeni erişim belirteçleri almak için kullanılabilecek yenileme belirteci.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|GEREKLİ. Değer "code" olarak ayarlanmalıdır.|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|GEREKLİ. Değer "Token".|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|İSTEĞE BAĞLI. Erişim isteğinin kapsamı.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|İstemci tarafından istenen kapsam ile aynıysa İSTEĞE BAĞLI; aksi takdirde ZORUNLU.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|İSTEĞE BAĞLI. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|İstemci tarafından istenen kapsam ile aynıysa İSTEĞE BAĞLI; aksi takdirde ZORUNLU.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|İSTEĞE BAĞLI. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|İstemci tarafından istenen kapsam ile aynıysa İSTEĞE BAĞLI; aksi takdirde ZORUNLU.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|İSTEĞE BAĞLI. Erişim isteğinin kapsamı.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|İstemci tarafından istenen kapsam ile aynıysa İSTEĞE BAĞLI; aksi takdirde ZORUNLU.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|"State" parametresi istemci yetkilendirme isteğinde bulunuyorsa bulunuyorsa ZORUNLUDUR.  İstemciden alınan tam değerdir.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|ÖNERİLİR. İstemci tarafından istek ve geri çağırma arasında durumu korumak için kullanılan genel olmayan bir değer.  Yetkilendirme sunucusu, kullanıcı aracısını istemciye yeniden yönlendirirken bu değeri içerir.  Siteler arası istek sahteciliğini önlemek için parametrenin kullanılması gerekir.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|"State" parametresi istemci yetkilendirme isteğinde bulunuyorsa bulunuyorsa ZORUNLUDUR.  İstemciden alınan tam değerdir.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|"State" parametresi istemci yetkilendirme isteğinde bulunuyorsa bulunuyorsa ZORUNLUDUR.  İstemciden alınan tam değerdir.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|ÖNERİLİR. İstemci tarafından istek ve geri çağırma arasında durumu korumak için kullanılan genel olmayan bir değer.  Yetkilendirme sunucusu, kullanıcı aracısını istemciye yeniden yönlendirirken bu değeri içerir.  Siteler arası istek sahteciliğini önlemek için parametrenin kullanılması gerekir.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|"State" parametresi istemci yetkilendirme isteğinde bulunuyorsa bulunuyorsa ZORUNLUDUR.  İstemciden alınan tam değerdir.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|GEREKLİ. Verilen belirtecin türü.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|GEREKLİ. Verilen belirtecin türü.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|GEREKLİ. Verilen belirtecin türü.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|GEREKLİ. Verilen belirtecin türü.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|GEREKLİ. Kaynak sahibi kullanıcı adı.|  
|OAuth2UnsupportedTokenType|Belirteç türü '{0}' desteklenmiyor.|  
|OAuth2InvalidState|Yetkilendirme sunucusundan geçersiz yanıt|  
|OAuth2GrantType_AuthorizationCode|Yetkilendirme kodu|  
|OAuth2GrantType_Implicit|Örtük|  
|OAuth2GrantType_ClientCredentials|İstemci kimlik bilgileri|  
|OAuth2GrantType_ResourceOwnerPassword|Kaynak sahibinin parolası|  
|WebDocumentation302Code|302 Bulundu|  
|WebDocumentation400Code|400 (Hatalı istek)|  
|OAuth2SendingMethod_AuthHeader|Yetkilendirme üst bilgisi|  
|OAuth2SendingMethod_QueryParam|Sorgu parametresi|  
|OAuth2AuthorizationServerGeneralException|Erişim üzerinden yetkilendirilirken bir hata oluştu {0}|  
|OAuth2AuthorizationServerCommunicationException|Yetkilendirme sunucusuna HTTP bağlantısı kurulamadı veya bağlantı beklenmedik bir şekilde kapatıldı.|  
|WebDocumentationOAuth2GeneralErrorMessage|Beklenmeyen bir hata oluştu.|  
|AuthorizationServerCommunicationException|Yetkilendirme sunucusu iletişim özel durumu oluştu. Lütfen yöneticinize başvurun.|  
|TextblockSubscriptionKeyHeaderDescription|Bu API'ye erişim sağlayan abonelik anahtarı. Bulunan, < a href = ='/ developer'\>profil < /a\>.|  
|TextblockOAuthHeaderDescription|Öğesinden alınan OAuth 2.0 erişim belirteci < ı\>{0}< /i\>. Desteklenen verme türleri: < ı\>{1}< /i\>.|  
|TextblockContentTypeHeaderDescription|API'ye gönderilen gövdenin medya türü.|  
|ErrorMessageApiNotAccessible|Çağırmaya çalıştığınız API şu anda erişilebilir değil. Lütfen API yayımcısına başvurun. < a href = = "/ issues"\>burada < /a\>.|  
|ErrorMessageApiTimedout|Çağırmaya çalıştığınız API'nin yanıt almak için normalden uzun sürüyor. Lütfen API yayımcısına başvurun. < a href = = "/ issues"\>burada < /a\>.|  
|BadRequestParameterExpected|"'{0}' parametresi bekleniyor"|  
|TooltipTextDoubleClickToSelectAll|Tümünü seçmek için çift tıklayın.|  
|TooltipTextHideRevealSecret|Göster/Gizle|  
|ButtonLinkOpenConsole|Deneyin|  
|SectionHeadingRequestBody|İstek gövdesi|  
|SectionHeadingRequestParameters|İstek parametreleri|  
|SectionHeadingRequestUrl|İstek URL'si|  
|SectionHeadingResponse|Yanıt|  
|SectionHeadingRequestHeaders|İstek üst bilgileri|  
|FormLabelSubtextOptional|isteğe bağlı|  
|SectionHeadingCodeSamples|Kod örnekleri|  
|TextblockOpenidConnectHeaderDescription|Öğesinden alınan Openıd Connect kimlik belirteci < ı\>{0}< /i\>. Desteklenen verme türleri: < ı\>{1}< /i\>.|  
  
###  <a name="ErrorPageStrings"></a> ErrorPageStrings  
  
|Ad|Metin|  
|----------|----------|  
|LinkLabelBack|geri|  
|LinkLabelHomePage|giriş sayfasını ziyaret edin|  
|LinkLabelSendUsEmail|Bize e-posta gönderin|  
|PageTitleError|İstenen sayfa sunulurken bir hata oluştu|  
|TextblockPotentialCauseIntermittentIssue|Bu zaten çözülmüş olan bir aralıklı veri erişimi sorunu olabilir.|  
|TextblockPotentialCauseOldLink|Tıkladığınız bağlantı eski olabilir ve artık doğru konuma işaret etmiyor olabilir.|  
|TextblockPotentialCauseTechnicalProblem|Bizim tarafımızda teknik bir sorun olabilir.|  
|TextblockPotentialSolutionRefresh|Sayfayı yenilemeyi deneyin.|  
|TextblockPotentialSolutionStartOver|Üzerinden başlangıç sayfamızı {0}.|  
|TextblockPotentialSolutionTryAgain|Git {0} ve yeniden gerçekleştirdiğiniz eylemi deneyin.|  
|TextReportProblem|{0} açıklayan bir sorun oluştu ve size mümkün olduğunca kısa sürede biz başlayacağız.|  
|TitlePotentialCause|Olası neden|  
|TitlePotentialSolution|Yalnızca geçici bir sorun olabilir, deneyebileceğiniz birkaç şey bulunur|  
  
###  <a name="IssuesStrings"></a> IssuesStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebIssuesIndexTitle|Sorunlar|  
|WebIssuesNoActiveSubscriptions|Hiç etkin aboneliğiniz yok. Bir sorunu bildirmek için bir ürüne abone olmanız.|  
|WebIssuesNotSignin|Oturum açmadıysanız değil. Lütfen {0} sorun bildirmek veya yorum göndermek için.|  
|WebIssuesReportIssueButton|Sorun Raporla|  
|WebIssuesSignIn|oturum aç|  
|WebIssuesStatusReportedBy|Durum: {0} &#124; tarafından bildirilen {1}|  
  
###  <a name="NotFoundStrings"></a> NotFoundStrings  
  
|Ad|Metin|  
|----------|----------|  
|LinkLabelHomePage|giriş sayfasını ziyaret edin|  
|LinkLabelSendUsEmail|bize e-posta gönderin|  
|PageTitleNotFound|Üzgünüz, aradığınız sayfayı bulamıyoruz|  
|TextblockPotentialCauseMisspelledUrl|İçinde yazdıysanız URL yanlış.|  
|TextblockPotentialCauseOldLink|Tıkladığınız bağlantı eski olabilir ve artık doğru konuma işaret etmiyor olabilir.|  
|TextblockPotentialSolutionRetype|URL'yi yeniden yazmayı deneyin.|  
|TextblockPotentialSolutionStartOver|Üzerinden başlangıç sayfamızı {0}.|  
|TextReportProblem|{0} açıklayan bir sorun oluştu ve size mümkün olduğunca kısa sürede biz başlayacağız.|  
|TitlePotentialCause|Olası neden|  
|TitlePotentialSolution|Olası çözüm|  
  
###  <a name="ProductDetailsStrings"></a> ProductDetailsStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebProductsAgreement|Abone tarafından {0} ürün, kabul ediyorum için `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Kullanım Koşulları|  
|WebProductsSubscribeButton|Abone olun|  
|WebProductsUsageLimitsHeader|Kullanım sınırları|  
|WebProductsYouAreNotSubscribed|Bu ürüne abonesiniz.|  
|WebProductsYouRequestedSubscription|Bu ürün için abonelik talebinde bulundunuz.|  
|ErrorYouNeedToAgreeWithLegalTerms|Devam etmeden önce Kullanım Koşulları'nı kabul etmeniz gerekir.|  
|ButtonLabelAddSubscription|Abonelik ekleme|  
|LinkLabelChangeSubscriptionName|değiştir|  
|ButtonLabelConfirm|Onayla|  
|TextblockMultipleSubscriptionsCount|Sahip olduğunuz {0} bu ürün için abonelikleri:|  
|TextblockSingleSubscriptionsCount|Sahip olduğunuz {0} bu ürün için abonelik:|  
|TextblockSingleApisCount|Bu ürünü içeren {0} API:|  
|TextblockMultipleApisCount|Bu ürünü içeren {0} API'leri:|  
|TextblockHeaderSubscribe|Ürüne abone olun|  
|TextblockSubscriptionDescription|Şu şekilde yeni bir abonelik oluşturulacak:|  
|TextblockSubscriptionLimitReached|Abonelik sınırına ulaşıldı.|  
  
###  <a name="ProductsStrings"></a> ProductsStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleProducts|Ürünler|  
  
###  <a name="ProviderInfoStrings"></a> ProviderInfoStrings  
  
|Ad|Metin|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Oturum açma, yöneticiler tarafından devre dışı bırakılmış durumda.|  
|TextboxExternalIdentitiesSigninInvitation|Oturum açmak için alternatif olarak şunu kullanabilirsiniz:|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Şununla oturum açın:|  
  
###  <a name="SigninResources"></a> SigninResources  
  
|Ad|Metin|  
|----------|----------|  
|PrincipalNotFound|Sorumlu bulunamadı veya imza geçersiz|  
|ErrorSsoAuthenticationFailed|SSO kimlik doğrulaması başarısız oldu|  
|ErrorSsoAuthenticationFailedDetailed|Geçersiz belirteç sağlandı veya imza doğrulanamıyor.|  
|ErrorSsoTokenInvalid|SSO belirteci geçersiz|  
|ValidationErrorSpecificEmailAlreadyExists|E-posta '{0}' zaten kayıtlı|  
|ValidationErrorSpecificEmailInvalid|E-posta '{0}' geçersiz|  
|ValidationErrorPasswordInvalid|Parola geçersiz. Lütfen hataları düzeltin ve yeniden deneyin.|  
|PropertyTooShort|{0} çok kısa|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Geçersiz e-posta adresi.|  
|ValidationMessageNewPasswordConfirmationRequired|Yeni parolayı onayla|  
|ValidationErrorPasswordConfirmationRequired|Parola onayı boş|  
|WebAuthenticationEmailChangeNotice|Değişiklik Onayı e-posta etkin şekilde {0}. Lütfen yeni e-posta adresinizi onaylamak için içindeki yönergeleri izleyin. E-posta birkaç dakika içinde gelen kutunuza gelmezse Lütfen istenmeyen e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailChangeNoticeHeader|E-posta değişiklik isteğiniz başarıyla işlendi|  
|WebAuthenticationEmailChangeNoticeTitle|E-posta değişikliği istendi|  
|WebAuthenticationEmailHasBeenRevertedNotice|E-postanız zaten mevcut. İstek geri döndürüldü|  
|ValidationErrorEmailAlreadyExists|E-posta zaten var|  
|ValidationErrorEmailInvalid|Geçersiz e-posta adresi|  
|TextboxLabelEmail|Email|  
|ValidationErrorEmailRequired|E-posta gereklidir.|  
|WebAuthenticationErrorNoticeHeader|Hata|  
|WebAuthenticationFieldLengthErrorMessage|{0} maksimum uzunlukta olmalıdır {1}|  
|TextboxLabelEmailFirstName|Ad|  
|ValidationErrorFirstNameRequired|Ad gereklidir.|  
|ValidationErrorFirstNameInvalid|Geçersiz ad|  
|NoticeInvalidInvitationToken|Lütfen onay bağlantılarının yalnızca 48 saat geçerli olduğuna dikkat edin. Bu zaman aralığındaysanız hala varsa, Lütfen bağlantınızın doğru olduğundan emin olun. Bağlantınızın süresi dolduysa Lütfen onaylamak için çalıştığınız eylemi tekrarlayın.|  
|NoticeHeaderInvalidInvitationToken|Geçersiz davet belirteci|  
|NoticeTitleInvalidInvitationToken|Onaylama hatası|  
|WebAuthenticationLastNameInvalidErrorMessage|Geçersiz soyadı|  
|TextboxLabelEmailLastName|Soyadı|  
|ValidationErrorLastNameRequired|Soyadı gereklidir.|  
|WebAuthenticationLinkExpiredNotice|Size gönderilen onay bağlantısının süresi doldu. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|Parola sıfırlama bağlantınız geçersiz veya bağlantınızın süresi dolmuş.|  
|WebAuthenticationLinkExpiredNoticeTitle|Bağlantı gönderildi|  
|WebAuthenticationNewPasswordLabel|Yeni parola|  
|ValidationMessageNewPasswordRequired|Yeni parola gereklidir.|  
|TextboxLabelNotificationsSenderEmail|Bildirim gönderen e-postası|  
|TextboxLabelOrganizationName|Kuruluş adı|  
|WebAuthenticationOrganizationRequiredErrorMessage|Kuruluş adı boş|  
|WebAuthenticationPasswordChangedNotice|Parolanız başarıyla güncelleştirildi|  
|WebAuthenticationPasswordChangedNoticeTitle|Parola güncelleştirildi|  
|WebAuthenticationPasswordCompareErrorMessage|Parolalar eşleşmiyor|  
|WebAuthenticationPasswordConfirmLabel|Parolayı onayla|  
|ValidationErrorPasswordInvalidDetailed|Parola çok zayıf.|  
|WebAuthenticationPasswordLabel|Parola|  
|ValidationErrorPasswordRequired|Parola gereklidir.|  
|WebAuthenticationPasswordResetSendNotice|Değişiklik parola onayı e-posta etkin şekilde {0}. Lütfen parola değişikliği işleminize devam etmek için e-posta içindeki yönergeleri izleyin.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Parola sıfırlama isteğiniz başarıyla işlendi|  
|WebAuthenticationPasswordResetSendNoticeTitle|Parola sıfırlama istendi|  
|WebAuthenticationRequestNotFoundNotice|İstek bulunamadı|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Bildirim gönderen e-postası boş|  
|WebAuthenticationSigninPasswordLabel|Lütfen bir parola girerek değişikliği onaylayın|  
|WebAuthenticationSignupConfirmNotice|Kayıt onayı e-posta olan çekmek {0}. < br /\> Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin < br /\> e-posta birkaç dakika içinde kutunuza gelmezse Lütfen Önemsiz e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationSignupConfirmNoticeHeader|Hesabınız başarıyla oluşturuldu|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Kayıt onayı e-postası tekrar gönderildi|  
|WebAuthenticationSignupConfirmNoticeTitle|Hesap oluşturuldu|  
|WebAuthenticationTokenRequiredErrorMessage|Belirteç boş|  
|WebAuthenticationUserAlreadyRegisteredNotice|Bu e-posta ile bir kullanıcı sistemde zaten kayıtlı görünüyor. Parolanızı unuttuysanız Lütfen geri yükleyin veya destek ekibimize başvurun deneyin.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Kullanıcı zaten kayıtlı|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Zaten kayıtlı|  
|ButtonLabelChangePassword|Parolayı değiştir|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştir|  
|ButtonLabelCloseAccount|Hesabı kapat|  
|WebAuthenticationInvalidCaptchaErrorMessage|Girilen metin, resim metni eşleşmiyor. Lütfen yeniden deneyin.|  
|ValidationErrorCredentialsInvalid|E-posta veya parola geçersiz. Lütfen hataları düzeltin ve yeniden deneyin.|  
|WebAuthenticationRequestIsNotValid|İstek geçerli değil|  
|WebAuthenticationUserIsNotConfirm|Lütfen oturum açmayı denemeden önce kaydınızı onaylayın.|  
|WebAuthenticationInvalidEmailFormated|E-posta geçersiz: {0}|  
|WebAuthenticationUserNotFound|Kullanıcı bulunamadı|  
|WebAuthenticationTenantNotRegistered|Hesabınız bu portala erişim için yetkilendirilmemiş bir Azure Active Directory kiracısına ait.|  
|WebAuthenticationAuthenticationFailed|Kimlik doğrulaması başarısız oldu.|  
|WebAuthenticationGooglePlusNotEnabled|Kimlik doğrulaması başarısız oldu. Uygulama yetkili olmadığını sonra lütfen Google kimlik doğrulamasının emin olmak için yöneticiye başvurun doğru şekilde yapılandırılır.|  
|ValidationErrorAllowedTenantIsRequired|İzin Verilen Kiracı gereklidir|  
|ValidationErrorTenantIsNotValid|Azure Active Directory kiracısı '{0}' geçerli değil.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Kullanarak oturum açın, {0} hesabı|  
|WebAuthenticationUserLimitNotice|Bu hizmet izin verilen kullanıcıların sayısı sınırına ulaştı. Lütfen `<a href="mailto:{0}"\>contact the administrator</a\>` hizmeti yükseltmesi ve kullanıcı kaydını yeniden etkinleştirmesi için.|  
|WebAuthenticationUserLimitNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserLimitNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserRegistrationDisabledNotice|Kullanıcı kaydı yönetici tarafından devre dışı bırakıldı. Lütfen dış kimlik sağlayıcısı ile oturum açın.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Kullanıcı kaydı devre dışı|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Kullanıcı kaydı devre dışı|  
|WebAuthenticationSignupPendingConfirmationNotice|Biz hesabınızı oluşturma işlemini tamamlayabilmek için önce size e-posta adresinizi doğrulamamız gerekiyor. Bir e-posta gönderdik {0}. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. E-posta birkaç dakika içinde gelmezse Lütfen istenmeyen e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|E-posta adresi için onaylanmamış bir hesap bulduk {0}. Hesabınızı oluşturmayı tamamlamak için size e-posta adresinizi doğrulamamız gerekiyor. Bir e-posta gönderdik {0}. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. E-posta birkaç dakika içinde gelmezse Lütfen istenmeyen e-posta klasörünüzü kontrol edin|  
|WebAuthenticationSignupConfirmationAlmostDone|Neredeyse Bitti|  
|WebAuthenticationSignupConfirmationEmailSent|Bir e-posta gönderdik {0}. Lütfen hesabınızı etkinleştirmek için e-posta içindeki yönergeleri izleyin. E-posta birkaç dakika içinde gelmezse Lütfen istenmeyen e-posta klasörünüzü kontrol edin.|  
|WebAuthenticationEmailSentNotificationMessage|E-posta başarıyla gönderildi {0}|  
|WebAuthenticationNoAadTenantConfigured|Hizmet için Azure Active Directory kiracısı yapılandırılmadı.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Kabul ediyorum `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Lütfen gözden geçirin `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Kullanım Koşulları|  
|ValidationMessageConsentNotAccepted|Devam etmeden önce Kullanım Koşulları'nı kabul etmeniz gerekir.|  
  
###  <a name="SigninStrings"></a> SigninStrings  
  
|Ad|Metin|  
|----------|----------|  
|WebAuthenticationForgotPassword|Parolanızı mı unuttunuz?|  
|WebAuthenticationIfAdministrator|Bir yöneticiyseniz, oturum açmanız gerekir `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Henüz üye değil mi? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Bu bilgisayarda beni anımsa|  
|WebAuthenticationSigininWithPassword|Kullanıcı adınız ve parolanızla oturum açın|  
|WebAuthenticationSigninTitle|Oturum aç|  
|WebAuthenticationSignUpNow|Hemen kaydolun|  
  
###  <a name="SignupStrings"></a> SignupStrings  
  
|Ad|Metin|  
|----------|----------|  
|PageTitleSignup|Kaydolma|  
|WebAuthenticationAlreadyAMember|Zaten üye misiniz?|  
|WebAuthenticationCreateNewAccount|Yeni API Yönetim hesabı oluşturma|  
|WebAuthenticationSigninNow|Hemen oturum açın|  
|ButtonLabelSignup|Kaydolma|  
  
###  <a name="SubscriptionListStrings"></a> SubscriptionListStrings  
  
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
|WebDevelopersSubscriptionRequested|İstendiği tarihi {0}|  
|WebDevelopersSubscriptionRequestedState|İstenen|  
|WebDevelopersSubscriptionTableNameHeader|Ad|  
|WebDevelopersSubscriptionTableStateHeader|Durum|  
|WebDevelopersUsageStatisticsLink|Analytics raporları|  
|WebDevelopersYourSubscriptions|Abonelikleriniz|  
|SubscriptionPropertyLabelRequestedDate|İstendiği tarihi|  
|SubscriptionPropertyLabelStartedDate|Başlama tarihi|  
|PageTitleRenameSubscription|Aboneliği yeniden adlandır|  
|SubscriptionPropertyLabelName|Abonelik adı|  
  
###  <a name="SubscriptionStrings"></a> SubscriptionStrings  
  
|Ad|Metin|  
|----------|----------|  
|SectionHeadingCloseAccount|Hesabınızı kapatmak mı istiyorsunuz?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Gizle|  
|ButtonLabelRegenerateKey|Yeniden Oluştur|  
|InformationMessageKeyWasRegenerated|Bu anahtarı yeniden oluşturmak istediğinizden emin misiniz?|  
|ButtonLabelShowKey|Göster|  
  
###  <a name="UpdateProfileStrings"></a> UpdateProfileStrings  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profili güncelleştir|  
|PageTitleUpdateProfile|Hesap bilgilerini güncelleştir|  
  
###  <a name="UserProfile"></a> Kullanıcı profili  
  
|Ad|Metin|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Hesap bilgilerini değiştir|  
|ButtonLabelChangePassword|Parolayı değiştir|  
|ButtonLabelCloseAccount|Hesabı kapat|  
|TextboxLabelEmail|Email|  
|TextboxLabelEmailFirstName|Ad|  
|TextboxLabelEmailLastName|Soyadı|  
|TextboxLabelNotificationsSenderEmail|Bildirim gönderen e-postası|  
|TextboxLabelOrganizationName|Kuruluş adı|  
|SubscriptionStateActive|Etkin|  
|SubscriptionStateCancelled|İptal edildi|  
|SubscriptionStateExpired|Süresi dolmuş|  
|SubscriptionStateRejected|Reddedildi|  
|SubscriptionStateRequested|İstenen|  
|SubscriptionStateSuspended|Askıya Alındı|  
|DefaultSubscriptionNameTemplate|{0}  (varsayılan)|  
|SubscriptionNameTemplate|Geliştirici erişimi #{0}|  
|TextboxLabelSubscriptionName|Abonelik adı|  
|ValidationMessageSubscriptionNameRequired|Abonelik adı boş olamaz.|  
|ApiManagementUserLimitReached|Bu hizmet izin verilen kullanıcıların sayısı sınırına ulaştı. Lütfen daha yüksek bir fiyatlandırma katmanına yükseltin.|  
  
##  <a name="glyphs"></a> Glif kaynakları  
 API Management Geliştirici portal şablonları gelen karakter kullanabilirsiniz [önyükleme gelen Glyphicons](https://getbootstrap.com/components/#glyphicons). Bu karakterleri kümesini yazı tipi biçiminden 250'den fazla karakter içeren [Glyphicon](https://glyphicons.com/) Halflings ayarlayın. Bu kümesindeki bir karakteri kullanmak için aşağıdaki sözdizimini kullanın.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Glif tam listesi için bkz. [önyükleme gelen Glyphicons](https://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Sonraki adımlar
Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [şablonlarını kullanarak API Management Geliştirici portalını özelleştirmek nasıl](api-management-developer-portal-templates.md).
