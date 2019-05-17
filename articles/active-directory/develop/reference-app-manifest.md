---
title: Azure Active Directory Uygulama bildirimini anlama | Microsoft Docs
description: Azure AD kiracısında bir uygulamanın kimlik yapılandırmasını temsil eder ve OAuth yetkilendirme, onayı deneyimi ve diğer kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildirimini ayrıntılı kapsamını.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d02642b0c069124ddcfbef1ea655438c906739a
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545664"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory Uygulama bildirimi

Uygulama bildirimi, Microsoft kimlik platformu uygulama nesnenin tüm öznitelikleri bir tanım içeriyor. Ayrıca uygulama nesnesi güncelleştirmeye yönelik bir mekanizma olarak kullanılır. Uygulama varlığı ve şeması hakkında daha fazla bilgi için bkz. [Graph API uygulaması entity belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity).

Azure portal veya program aracılığıyla kullanarak bir uygulamanın özniteliklerinden yapılandırabileceğiniz [REST API](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) veya [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications). Bununla birlikte, burada bir uygulamanın özniteliği yapılandırmak için uygulama bildiriminizi düzenleyin gerekecektir bazı senaryolar vardır. Bu senaryolar şunlardır:

* Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak uygulama kaydettiyseniz, kullanıcı arabiriminde desteklenen Microsoft hesapları değiştiremezsiniz. Bunun yerine, desteklenen bir hesap türünü değiştirmek için uygulama bildirim düzenleyicisini kullanmanız gerekir.
* İzinler ve uygulamanızın desteklediği rolleri tanımlamak gerekiyorsa, uygulama bildirimini değiştirmeniz gerekir.

## <a name="configure-the-app-manifest"></a>Uygulama bildirimi yapılandırma

Uygulama bildirimini yapılandırmak için:

1. Oturum [Azure portalında](https://portal.azure.com).
1. Seçin **Azure Active Directory** hizmet ve ardından **uygulama kayıtları**.
1. Yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Portal bildirimi düzenlemenize olanak sağlayan web tabanlı bir bildirim düzenleyicisinde açılır. İsteğe bağlı olarak seçebileceğiniz **indirin** yerel bildirimi düzenleyin ve ardından **karşıya** uygulamanızı uygulamak için.

## <a name="manifest-reference"></a>Bildirimi başvurusu

> [!NOTE]
> Göremiyorsanız **örnek değer** sonra sütun **açıklama**tarayıcı pencerenizin ekranı kaplaması ve kaydırma/geçirme görene kadar **örnek değer** sütun.

| Anahtar  | Değer türü | Açıklama  | Örnek değer |
|---------|---------|---------|---------|
| `accessTokenAcceptedVersion` | Boş değer atanabilir Int32 | Kaynak tarafından beklenen erişim belirteci sürümünü belirtir. Bu sürüm değiştirir ve biçimi JWT uç nokta veya istemci erişim belirteci istemek için kullanılan bağımsız olarak üretilen.<br/><br/>Kullanılan uç nokta veya v1.0, v2.0 istemci tarafından seçilir ve yalnızca id_tokens sürümünü etkiler. Açıkça yapılandırmanız gereken kaynakları `accesstokenAcceptedVersion` desteklenen erişim belirteci biçimini belirtmek için.<br/><br/>Olası değerler için `accesstokenAcceptedVersion` 1, 2 ya da null. Değer null ise, bu v1.0 uç noktaya karşılık gelen varsayılan olarak 1. | `2` |
| `addIns` | Koleksiyon | Kullanan bir hizmet, bir uygulama belirli bağlamlarda çağırmak için kullanabileceğiniz özel davranışını tanımlar. Örneğin, dosya akışları oluşturabileceği uygulamaları eklentileri özelliği "FileHandler" işlevselliği için ayarlayabilir. Bu, Office 365 gibi kullanıcı üzerinde çalıştığı bir belge bağlamında uygulama arama hizmetleri sağlar. | <code>{<br>&nbsp;&nbsp;&nbsp;"id":"968A844F-7A47-430C-9163-07AE7C31D407"<br>&nbsp;&nbsp;&nbsp;"type": "FileHandler",<br>&nbsp;&nbsp;&nbsp;"properties": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{"key": "version", "value": "2" }<br>&nbsp;&nbsp;&nbsp;]<br>}</code>|
| `allowPublicClient` | Boolean | Geri dönüş uygulaması türünü belirtir. Azure AD, varsayılan olarak replyUrlsWithType uygulama türünden çıkarır. Burada Azure AD'ye belirleyemiyor istemci uygulama türüne belirli senaryolar vardır (örneğin [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) burada HTTP isteği bir URL yeniden yönlendirme ' olmuyor akış). Bu gibi durumlarda, Azure AD uygulama türü, bu özelliğin değerine göre görürler. Bu değer, geri dönüş uygulama türünü true olarak ayarlanırsa, bir mobil cihazda çalışan yüklü bir uygulama gibi ortak istemci olarak ayarlanır. Web uygulaması gibi gizli bir istemci geri dönüş uygulaması türüdür yani varsayılan değer false'tur. | `false` |
| `availableToOtherTenants` | Boolean | uygulamanın diğer kiracılarla paylaşılıyorsa true; Aksi takdirde false. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `signInAudience` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | |
| `appId` | String | Azure AD tarafından atanmış bir uygulama uygulama için benzersiz tanımlayıcısını belirtir. | `"601790de-b632-4f57-9523-ee7cb6ceba95"` |
| `appRoles` | Koleksiyon | Bir uygulamanın bildirebileceği rolleri koleksiyonunu belirtir. Bu roller, kullanıcıları, grupları veya hizmet sorumlularına atanabilir. Daha fazla örnekler ve bilgi için bkz. [uygulamanızda uygulama rolleri eklemek ve bunları belirteci alma](howto-add-app-roles-in-azure-ad-apps.md) | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&nbsp;&nbsp;&nbsp;],<br>&nbsp;&nbsp;&nbsp;"description":"Read-only access to device information",<br>&nbsp;&nbsp;&nbsp;"displayName":"Read Only",<br>&nbsp;&nbsp;&nbsp;"id":guid,<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"value":"ReadOnly"<br>&nbsp;&nbsp;}<br>]</code>  |
| `displayName` | String | Uygulama görünen adı. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `name` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | `"MyRegisteredApp"` |
| `errorUrl` | String | Desteklenmeyen. | |
| `groupMembershipClaims` | String | Yapılandırır `groups` bir kullanıcı veya uygulamanın beklediği OAuth 2.0 erişim belirteci veren talep. Bu öznitelik ayarlamak için aşağıdaki geçerli dize değerlerinden birini kullanın:<br/><br/>- `"None"`<br/>- `"SecurityGroup"` (güvenlik grupları ve Azure AD rolleri için)<br/>- `"All"` (Bu tüm güvenlik grupları, dağıtım grupları ve oturum açmış kullanıcının üyesi olduğu bir Azure AD Dizin rolleri alırsınız. | `"SecurityGroup"` |
| `homepage` | String | Uygulamanın giriş sayfası URL'si. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `signInUrl` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | `"https://MyRegisteredApp"` |
| `objectId` | String | Uygulama dizinindeki benzersiz tanımlayıcısı. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `id` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `optionalClaims` | String | İsteğe bağlı talepleri özel bu uygulama için güvenlik belirteci hizmeti tarafından belirteç döndürdü.<br>Şu anda, hem kişisel hesapları hem de Azure AD (kayıtlı uygulama kayıt portalı üzerinden) destekleyen uygulamalar, isteğe bağlı bir talep kullanamazsınız. Ancak, yalnızca Azure AD v2.0 uç noktası kullanmak için kayıtlı uygulamalar bildiriminde talep isteğe bağlı bir talep elde edebilirsiniz. Daha fazla bilgi için bkz. [isteğe bağlı bir talep](active-directory-optional-claims.md). | `null` |
| `id` | String | Uygulama dizinindeki benzersiz tanımlayıcısı. Bu kimliği, herhangi bir protokol işlem uygulamayı tanımlamak için kullanılan tanımlayıcı değil. Dizin sorguları nesneye başvurmak için kullanılır. | `"f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"` |
| `identifierUris` | String Array | Çok kiracılı bir uygulama ise, bir Web uygulaması doğrulanmış özel etki alanı veya kendi Azure AD kiracısı içinde benzersiz olarak tanımlayan kullanıcı tanımlı URI(s). | <code>[<br>&nbsp;&nbsp;"https://MyRegisteredApp"<br>]</code> |
| `informationalUrls` | String | Uygulamanın koşullarını hizmet ve gizlilik bildirimi bağlantıları belirtir. Koşulları hizmet ve gizlilik bildirimi kullanıcı onayı deneyimi aracılığıyla kullanıcılara çıkarılır. Daha fazla bilgi için bkz. [nasıl yapılır: Kayıtlı Azure AD uygulamaları için hizmet ve gizlilik bildirimini koşulları ekleme](howto-add-terms-of-service-privacy-statement.md). | <code>{<br>&nbsp;&nbsp;&nbsp;"marketing":"https://MyRegisteredApp/marketing",<br>&nbsp;&nbsp;&nbsp;"privacy":"https://MyRegisteredApp/privacystatement",<br>&nbsp;&nbsp;&nbsp;"support":"https://MyRegisteredApp/support",<br>&nbsp;&nbsp;&nbsp;"termsOfService":"https://MyRegisteredApp/termsofservice"<br>}</code> |
| `keyCredentials` | Koleksiyon | Uygulama tarafından atanan kimlik bilgileri, paylaşılan gizlilikler dize tabanlı ve X.509 sertifikaları için başvurular içerir. Bu kimlik bilgileri erişim belirteci isteğinde bulunurken kullanılır (uygulama istemci olarak hareket ne zaman yerine, kaynak olarak). | <code>[<br>&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-09-13T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2017-09-12T00:00:00Z",<br>&nbsp;&nbsp;&nbsp;"type":"AsymmetricX509Cert",<br>&nbsp;&nbsp;&nbsp;"usage":"Verify",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;}<br>]</code> |
| `knownClientApplications` | String Array | İki bölümü içeren bir çözümünüz varsa paket onayı için kullanılan: bir istemci uygulaması hem de özel web API uygulaması. İçinde bu değer istemci uygulamasının uygulama kimliği girin, kullanıcının yalnızca bir kez istemci uygulamasına kabul sahip olur. Azure AD, istemciye yönelik örtük olarak web API'sine yönelik anlamına gelir ve hem istemci hem de web API'si için hizmet sorumluları, aynı anda otomatik olarak sağlamış olduğunu bilirsiniz. Hem istemci hem de web API'si uygulaması aynı kiracıda kayıtlı olması gerekir. | `["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"]` |
| `logoUrl` | String | Portalda karşıya yüklenen logoyu CDN URL'sine işaret eden değerini okur. | `"https://MyRegisteredAppLogo"` |
| `logoutUrl` | String | Uygulama oturumunu URL. | `"https://MyRegisteredAppLogout"` |
| `name` | String | Uygulama görünen adı. | `"MyRegisteredApp"` |
| `oauth2AllowImplicitFlow` | Boolean | Bu web uygulaması OAuth2.0 örtük akış erişim belirteçleri isteme olanaklarının olup olmadığını belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. Daha fazla bilgi için girin `OAuth 2.0 implicit grant flow` içindekiler tablosunda ve örtük akış hakkındaki konulara bakın. | `false` |
| `oauth2AllowIdTokenImplicitFlow` | Boolean | Bu web uygulaması OAuth2.0 örtük akış kimliği belirteç isteyip isteyemeyeceğini belirtir. Varsayılan değer false'dur. Bu bayrak, Javascript tek sayfa uygulamaları gibi tarayıcı tabanlı uygulamalar için kullanılır. | `false` |
| `oauth2Permissions` | Koleksiyon | İstemci uygulamalarında web API (kaynak) uygulamanın sunduğu OAuth 2.0 izin kapsamları koleksiyonu belirtir. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"adminConsentDescription":"Allow the app to access resources on behalf of the signed-in user.",<br>&nbsp;&nbsp;&nbsp;"adminConsentDisplayName":"Access resource1",<br>&nbsp;&nbsp;&nbsp;"id":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"isEnabled":true,<br>&nbsp;&nbsp;&nbsp;"type":"User",<br>&nbsp;&nbsp;&nbsp;"userConsentDescription":"Allow the app to access resource1 on your behalf.",<br>&nbsp;&nbsp;&nbsp;"userConsentDisplayName":"Access resources",<br>&nbsp;&nbsp;&nbsp;"value":"user_impersonation"<br>&nbsp;&nbsp;}<br>]</code>|
| `oauth2RequiredPostResponse` | Boolean | OAuth 2.0 belirteç isteklerini bir parçası olarak, Azure AD GET istekleri aksine POST istekleri izin olup olmadığını belirtir. Yalnızca GET isteklerini verileceğini belirten varsayılan false değeridir. | `false` |
| `parentalControlSettings` | String | `countriesBlockedForMinors` uygulama için yetişkin engellenir ülkeleri belirtir.<br>`legalAgeGroupRule` Uygulamayı kullanıcılara uygulanan yasal yaş grubu kuralı belirtir. Ayarlanabilir `Allow`, `RequireConsentForPrivacyServices`, `RequireConsentForMinors`, `RequireConsentForKids`, veya `BlockMinors`.  | <code>{<br>&nbsp;&nbsp;&nbsp;"countriesBlockedForMinors":[],<br>&nbsp;&nbsp;&nbsp;"legalAgeGroupRule":"Allow"<br>}</code> |
| `passwordCredentials` | Koleksiyon | Açıklamasına bakın `keyCredentials` özelliği. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;"customKeyIdentifier":null,<br>&nbsp;&nbsp;&nbsp;"endDate":"2018-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"keyId":"\<guid>",<br>&nbsp;&nbsp;&nbsp;"startDate":"2016-10-19T17:59:59.6521653Z",<br>&nbsp;&nbsp;&nbsp;"value":null<br>&nbsp;&nbsp;&nbsp;}<br>]</code> |
| `preAuthorizedApplications` | Koleksiyon | Uygulamalar ve tanımlanabilen istenen izinleri listeler. Bir yöneticinin uygulamaya izin vermiş gerektirir. preAuthorizedApplications istenen izinleri kabul etmesi gerekmez. Kullanıcı onayı preAuthorizedApplications içinde listelenen izinlere gerek yoktur. Ancak preAuthorizedApplications listelenmeyen ek istenen izinleri, kullanıcı onayı gerektirir. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",<br>&nbsp;&nbsp;&nbsp;&nbsp;"permissionIds": [<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"8748f7db-21fe-4c83-8ab5-53033933c8f1"<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `publicClient` | Boolean | Bu uygulamanın genel bir istemci (örneğin, bir mobil cihazda çalışan yüklü bir uygulama) olup olmadığını belirtir. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `allowPublicClient` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | |
| `publisherDomain` | String | Uygulama için yayımcı etki alanı. Salt okunur. | https://www.contoso.com |
| `replyUrls` | Dize dizisi | Bu çok değerli özellik belirteçleri döndürülürken hedefleri olarak Azure AD'ye kabul kayıtlı redirect_uri değerlerin listesini tutar. <br><br> _Not: Bu, yalnızca uygulama kayıtları (eski) deneyimini kullanılabilir. Değiştirilen `replyUrlsWithType` içinde [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) karşılaşırsınız._ | |
| `replyUrlsWithType` | Koleksiyon | Bu çok değerli özellik belirteçleri döndürülürken hedefleri olarak Azure AD'ye kabul kayıtlı redirect_uri değerlerin listesini tutar. Her URI değeri, bir ilişkili uygulama türü değeri içermelidir. Desteklenen türde değerler şunlardır: `Web`, `InstalledClient`. | <code>"replyUrlsWithType":&nbsp;[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"url":&nbsp;"https://localhost:4400/services/office365/redirectTarget.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":&nbsp;"InstalledClient"&nbsp;&nbsp;&nbsp;<br>&nbsp;&nbsp;}<br>]</code> |
| `requiredResourceAccess` | Koleksiyon | Dinamik onayı ile `requiredResourceAccess` yönetici onayı deneyimi ve statik onay kullanan kullanıcılar için kullanıcı onayı deneyimi sürücüler. Ancak, bu genel durum kullanıcı onayı deneyimi sürücü değil.<br>`resourceAppId` Uygulama erişim gerektiren kaynağın benzersiz tanımlayıcısıdır. Bu değer, hedef kaynak uygulama bildirilen AppID eşit olmalıdır.<br>`resourceAccess` Belirtilen kaynak uygulama gerektiren uygulama rolleri ve izin kapsamları OAuth2.0 listeleyen bir dizidir. İçeren `id` ve `type` değerleri belirtilen kaynakları. | <code>[<br>&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAppId":"00000002-0000-0000-c000-000000000000",<br>&nbsp;&nbsp;&nbsp;&nbsp;"resourceAccess":[<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id":"311a71cc-e848-46a1-bdf8-97ff7156d8e6",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"type":"Scope"<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>&nbsp;&nbsp;&nbsp;&nbsp;]<br>&nbsp;&nbsp;}<br>]</code> |
| `samlMetadataUrl` | String | Uygulama SAML meta verilerinin URL'si. | `https://MyRegisteredAppSAMLMetadata` |
| `signInUrl` | String | Uygulama giriş sayfası URL'sini belirtir. | `https://MyRegisteredApp` |
| `signInAudience` | String | Hangi Microsoft hesapları için geçerli uygulamanın desteklenen belirtir. Desteklenen değerler şunlardır:<ul><li>**AzureADMyOrg** -kullanıcılar Microsoft ile iş veya Okul hesabı kuruluşumun Azure AD kiracısında (yani, tek kiracılı)</li><li>**AzureADMultipleOrgs** -kullanıcılar Microsoft ile iş veya Okul hesabı herhangi bir kuruluşun Azure AD kiracısında (yani, çok kiracılı)</li> <li>**AzureADandPersonalMicrosoftAccount** -kişisel bir Microsoft hesabı ya da herhangi bir kuruluşun Azure AD kiracısında bir iş veya Okul hesabı olan kullanıcılar</li></ul> | `AzureADandPersonalMicrosoftAccount` |
| `tags` | String Array | Kategorilere ayırmak ve uygulamayı tanımlamak için kullanılan özel dizeleri. | <code>[<br>&nbsp;&nbsp;"ProductionApp"<br>]</code> |

## <a name="common-issues"></a>Sık karşılaşılan sorunlar

### <a name="manifest-limits"></a>Sınırları bildirimi

Bir uygulama bildirimi, koleksiyon olarak adlandırılan birden çok özniteliğe sahip; Örneğin, approles keycredentials, knownClientApplications, identifierUris, rediretUris, requiredResourceAccess ve oauth2Permissions. Herhangi bir uygulama için tam bir uygulama bildirimi içinde birlikte tüm koleksiyonları giriş toplam sayısı 1200 tavan. 100 yeniden yönlendirme URI'leri uygulama bildiriminde belirtilen zaten varsa, yalnızca sol girişlerle bildirimi oluşturan birleştirilmiş diğer tüm koleksiyonlarındaki kullanılacak 1100 kalan demektir.

> [!NOTE]
> Uygulama bildiriminde 1200'den fazla girişleri eklemeye çalıştığınız durumunda, bir hata görebilirsiniz **"uygulama xxxxxx güncelleştirilemedi. Hata ayrıntıları: Bildirimin boyut sınırını aştı. "Lütfen değerlerin sayısını azaltın ve isteğinizi yeniden deneyin."**

### <a name="unsupported-attributes"></a>Desteklenmeyen öznitelikleri

Uygulama bildirimini Azure AD'de temel uygulama modeli şemasını temsil eder. Arka plandaki şema geliştikçe bildirim düzenleyicisini yeni şemayı zaman zaman yansıtacak şekilde güncelleştirilir. Sonuç olarak, uygulama bildiriminde gösteren yeni öznitelikler fark edebilirsiniz. Nadir durumlarda, varolan öznitelikleri bir söz dizimi veya anlam değişiklik fark edebilirsiniz veya önceden var olan bir öznitelik artık desteklenmiyor bulabilirsiniz. Örneğin, yeni öznitelikler göreceğiniz [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) olduğu bilinir uygulama kayıtları (eski) deneyimi içinde farklı bir ada sahip.


| Uygulama kayıtları (eski)| Uygulama kayıtları           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Bu öznitelikler için açıklamalar için bkz [bildirim başvuru](#manifest-reference) bölümü.

Daha önce yüklenen bir bildirim yüklemeye çalıştığınızda, aşağıdaki hatalardan birini görebilirsiniz. Bildirim düzenleyicisini artık bir yüklemeye çalıştığınız eşleşmeyen şema daha yeni bir sürümünü desteklediğinden olasıdır.

- "**Xxxxxx uygulama güncelleştirilemedi. Hata ayrıntısı: Geçersiz nesne tanımlayıcısı 'undefined'. [].** "
- "**Xxxxxx uygulama güncelleştirilemedi. Hata ayrıntısı: Belirtilen bir veya daha fazla özellik değeri geçersiz. [].** "
- "**Xxxxxx uygulama güncelleştirilemedi. Hata ayrıntısı: Bu güncelleştirme için API sürümü availableToOtherTenants kümesindeki izin verilmiyor. [].** "
- "**Xxxxxx uygulama güncelleştirilemedi. Hata ayrıntısı: Güncelleştirmeleri 'replyUrls' özelliği için bu uygulama için izin verilmiyor. Bunun yerine 'replyUrlsWithType' özelliğini kullanın. [].** "
- "**Xxxxxx uygulama güncelleştirilemedi. Hata ayrıntısı: Bir tür adı olmayan bir değer bulundu ve beklenen tür kullanılabilir. Modeli belirtilen, akıştaki her bir değeri yükte, çağıran tarafından açıkça belirtilen ya da örtük olarak üst değerden çıkarılan bir tür olması gerekir. []**"

Şu hatalardan biriyle gördüğünüzde, şunları öneririz:

1. Karşıya yüklemeyi daha önce yüklenen bir bildirim yerine tek tek bildirim düzenleyicisinde özniteliklerini düzenleyin. Kullanım [bildirim başvuru](#manifest-reference) sözdizimi ve semantiği eski ve yeni öznitelikler, ilgilendiğiniz öznitelikleri başarıyla düzenleyebilmek anlamak için tablo. 
1. İş akışınızı bildirimleri daha sonra kullanmak için kaynak deponuza kaydetmeye gerektiriyorsa, kaydedilmiş bildirimleri içinde gördüğünüz deponuzda bir yeniden Temellendirme öneririz **uygulama kayıtları** karşılaşırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [uygulaması ve Azure AD'de hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).
* Bkz: [Microsoft kimlik platformu Geliştirici sözlüğü](developer-glossary.md) Microsoft kimlik platformu Geliştirici kavramları bazılarının tanımları için.

Aşağıdaki Açıklamalar bölümü daraltmak ve içeriklerimizde şekil yardımcı olur. geri bildirim sağlamak için kullanın.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
