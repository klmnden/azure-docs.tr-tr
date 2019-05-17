---
title: Azure Active Directory'de yapılandırılabilir belirteç ömrünü | Microsoft Docs
description: Azure AD tarafından verilen belirteçlere yaşam süreleri ayarlama konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2019
ms.author: ryanwi
ms.custom: aaddev, annaba
ms.reviewer: hirsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc81f0a5c75d9aeee39f0633521d692c8d30c474
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823463"
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-preview"></a>Azure Active Directory'de (Önizleme) yapılandırılabilir belirteç ömürleri

Azure Active Directory (Azure AD) tarafından verilmiş bir belirteç ömrünü belirtebilirsiniz. Kuruluşunuzda, kuruluşunuzdaki tüm uygulamalar, çok kiracılı (çok kuruluşlu) bir uygulama veya belirli bir hizmet sorumlusu için belirteç ömrünü ayarlayabilirsiniz.

> [!IMPORTANT]
> Önizleme sırasında müşterilerden işitme sonra biz yapılandırılabilir belirteç ömrünü özelliğiyle değiştirilmiştir [kimlik doğrulama oturumu yönetim özellikleri](https://go.microsoft.com/fwlink/?linkid=2083106) Azure AD koşullu erişim. Bu özellik, 1 Kasım 2019 üzerinde kullanımdan kaldırılacaktır. Yapılandırılabilir belirteç ömrü ilkesi kullanıyorsanız, yeni koşullu erişim özelliğini geçin. 

Azure AD'de, ayrı ayrı uygulamaların veya kuruluştaki tüm uygulamalar uygulanan bir kurallar kümesi İlkesi nesnesini temsil eder. Her ilke türü, atanmış olan nesnelere uygulanan özellik kümesi ile benzersiz bir yapıya sahiptir.

Kuruluşunuz için varsayılan ilke olarak, bir ilke belirleyebilirsiniz. Daha yüksek bir önceliğe sahip bir ilke tarafından kılınmayan sürece kuruluşta herhangi bir uygulama ilkesi uygulanır. Bu gibi durumlarda, bir ilke ayrıca belirli uygulamaları atayabilirsiniz. Öncelik sırasını ilke türüne göre değişir.

> [!NOTE]
> Yapılandırılabilir belirteç ömrü ilkesi SharePoint Online için desteklenmiyor.  Bu ilke PowerShell aracılığıyla oluşturma olanağına sahip olsa da, SharePoint Online bu ilke bildirecektir değil. Başvurmak [SharePoint Online blog](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) oturum boşta kalma zaman aşımı yapılandırma hakkında daha fazla bilgi edinmek için.
>* SharePoint Online erişim belirteci için varsayılan yaşam süresi 1 saattir. 
>* Varsayılan en fazla etkin olmayan zaman SharePoint Online'da yenileme belirtecinin 90 gündür.

## <a name="token-types"></a>Belirteç türleri

Yenileme belirteçleri, erişim belirteçleri, oturumu belirteçleri ve kimlik belirteçleri için belirteç ömrü ilkeleri ayarlayabilirsiniz.

### <a name="access-tokens"></a>Erişim belirteçleri

İstemciler, korunan bir kaynağa erişmek için erişim belirteçleri kullanır. Bir erişim belirteci yalnızca belirli bir birleşimi kullanıcı, istemci ve kaynak için kullanılabilir. Erişim belirteçleri iptal edilemiyor ve bunların sona erme tarihine kadar geçerlidir. Bir erişim belirteci elde kötü amaçlı bir aktör, yaşam süresi bir uzantı için kullanabilirsiniz. Bir erişim belirteci ömrü ayarlama, sistem performansı artırmak ve kullanıcı hesabının devre dışı bırakıldıktan sonra istemci erişimi korur süre miktarını artırmak arasında bir dengedir. İyileştirilmiş sistem performansı, yeni erişim belirteci almak için bir istemci gereken sayısını azaltarak elde edilir.  1 saat - varsayılan değer 1 saat sonra istemci yenileme belirteci (genellikle sessizce) yeni bir yenileme belirteci alma ve belirtecine erişmek için kullanmanız gerekir. 

### <a name="refresh-tokens"></a>Yenileme belirteçlerini

Bir istemci bir korumalı kaynağa erişmek için bir erişim belirteci alır, istemci ayrıca bir yenileme belirteci alır. Yenileme belirteci, geçerli erişim belirtecinin süresi dolduğunda yeni erişim/yenileme belirteci çiftleri elde etmek için kullanılır. Kullanıcı ve istemci bir birleşimi için bağlı bir yenileme belirteci. Bir yenileme belirteci olabilir [herhangi bir zamanda İptal](access-tokens.md#token-revocation), ve belirteç her kullanılışında belirtecin geçerlilik denetlenir.  Yenileme yeni erişim belirteçleri almak için kullanıldığında, belirteçleri iptal değil - en iyi uygulama, ancak yeni bir tane alırken eski belirteci güvenli bir şekilde silmek için değildir. 

Bu yenileme belirteçleri ne kadar süre kullanılabilir etkiliyor gibi gizli ve genel istemciler arasında bir ayrım yapmak önemlidir. Farklı istemci türleri hakkında daha fazla bilgi için bkz: [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Gizli istemci yenileme belirteçleri ile belirteç ömürleri
Gizli istemciler, istemci parolası (gizli) güvenli bir şekilde depolayabilirsiniz uygulamalardır. Bunlar, isteklerin güvenli istemci uygulamadan alınan ve kötü amaçlı bir aktör yerine geldiğini kanıtlayabilirsiniz. Örneğin, bir web uygulaması web sunucusunda bir istemci gizli anahtarını depolayabileceğiniz için gizli bir istemcidir. Bunu gösterilmez. Bu akışlar daha güvenlidir, yenileme belirteçleri bu akışlar için verilen varsayılan ömrü çünkü `until-revoked`İlkesi kullanılarak değiştirilemez ve üzerinde gönüllü parola sıfırlama iptal değil.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Belirteç ömürleri ortak istemci yenileme belirteçleri ile

Genel istemciler, istemci parolası (gizli) güvenli bir şekilde depolayamıyor. Örneğin, genel bir istemci olarak kabul edilir şekilde iOS/Android uygulaması gizli dizi kaynak sahibinden karartmak olamaz. Belirli bir süre eski genel istemcilerden yenileme belirteçleri yeni bir erişim/yenileme belirteci çifti elde etmesini önlemek için kaynakları ilkeleri ayarlayabilirsiniz. (Bunu yapmak için yenileme belirteci etkin olmayan zaman sınırı özelliğini kullanın (`MaxInactiveTime`).) Bir süre sonrasında artık yenileme belirteçleri kabul ayarlamak için ilkelerini de kullanabilirsiniz. (Bunu yapmak için yenileme belirteci Maksimum yaş özelliğini kullanın.) Ne zaman ve ne sıklıkta sessiz bir şekilde, bir ortak istemci uygulaması kullanarak yeniden kimlik doğrulaması yerine kimlik bilgileri yeniden girmek için kullanıcının gereklidir denetlemek için bir yenileme belirteci ömrü ayarlayabilirsiniz.

### <a name="id-tokens"></a>Kimlik belirteçleri
Web siteleri ve yerel istemci kimliği belirteçleri geçirilir. Kimliği belirteçleri bir kullanıcıyla ilgili profil bilgilerini içerir. Bir kimlik belirteci kullanıcı ve istemci belirli bir birleşim bağlıdır. Kimlik belirteçlerini, sona erme tarihine kadar geçerli kabul edilir. Genellikle, bir kullanıcı bir web uygulaması ile eşleşen oturum ömrünü kimlik belirteci ömrü uygulamadaki kullanıcı için verilen. Ne sıklıkla web uygulaması uygulama oturum süresinin sona ve Azure AD ile (Sessiz veya etkileşimli) kimliğinin yeniden doğrulanması kullanıcıya gerektirir ne sıklıkta denetlemek için bir kimlik belirteci ömrünü ayarlayabilirsiniz.

### <a name="single-sign-on-session-tokens"></a>Çoklu oturum açma oturumu belirteçleri
Azure AD ile bir kullanıcı kimliği doğruladığında bir tek oturum açma oturumu (SSO), kullanıcının tarayıcı ve Azure AD ile oluşturulur. SSO belirteci biçiminde bir tanımlama bilgisinin bu oturumu temsil eder. SSO Oturum belirteci bir belirli kaynak/istemci uygulamaya bağlı değil. SSO oturum belirteçleri iptal edilebilir ve kullanıldıkları her zaman geçerliliği denetlenir.

Azure AD, iki tür SSO oturum belirteçleri kullanır: kalıcı ve kalıcı olmayan. Kalıcı oturum belirteçleri, tarayıcı tarafından kalıcı tanımlama bilgileri depolanır. Kalıcı olmayan oturumu belirteç, oturum tanımlama bilgileri depolanır. (Tarayıcı kapatıldığında oturum tanımlama bilgileri yok.) Genellikle, bir kalıcı Oturum belirteci depolanır. Ancak, kullanıcı seçtiğinde **Oturumumu açık bırak** onay kutusu kalıcı Oturum belirteci kimlik doğrulaması sırasında depolanır.

Kalıcı olmayan oturumu belirteçleri, 24 saatlik bir ömre sahiptir. Kalıcı belirteçleri, 180 günlük bir ömre sahiptir. SSO Oturum belirteci bir geçerlilik süresi kullanılan herhangi bir zamanda, geçerlilik süresi, başka bir 24 saat veya belirteç türüne bağlı olarak 180 gün genişletilir. SSO Oturum belirteci bir geçerlilik süresi kullanılmıyorsa, olarak kabul edilir süresi doldu ve artık kabul edilir.

Sonra oturum belirteci artık kabul ötesindeki ilk oturum belirteci verilmiş süresini ayarlamak için bir ilke kullanabilirsiniz. (Bunu yapmak için oturum belirteci Maksimum yaş özelliğini kullanın.) Kullanıcı ne zaman ve ne sıklıkta sessiz bir şekilde, bir web uygulaması kullanılırken kimlik doğrulaması gerçekleştirilen yerine kimlik bilgilerini girmek zorunda denetlemek için bir oturum belirteç ömrünü ayarlayabilirsiniz.

### <a name="token-lifetime-policy-properties"></a>Belirteç ömrü ilkesi özellikleri
Bir belirteç ömrü ilkesi belirteç ömrü kuralları içeren ilke nesne türüdür. Belirtilen belirteç ömrünü denetlemek için ilke özelliklerini kullanın. İlke yok ayarlarsanız, sistem varsayılan yaşam süresi değeri zorlar.

### <a name="configurable-token-lifetime-properties"></a>Yapılandırılabilir belirteç ömrü özellikleri
| Özellik | İlke özelliği dizesi | Etkiler | Varsayılan | Minimum | Maksimum |
| --- | --- | --- | --- | --- | --- |
| Erişim belirteci ömrü |AccessTokenLifetime |Erişim belirteçleri, kimlik belirteçlerini SAML2 belirteçleri |1 saat |10 dakika |1 gün |
| Yenileme belirteci en fazla etkin olmayan zamanı |MaxInactiveTime |Yenileme belirteçlerini |90 gün |10 dakika |90 gün |
| Tek öğeli yenileme belirteci Maksimum yaş |MaxAgeSingleFactor |Yenileme belirteçlerini (tüm kullanıcılar için) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Çok faktörlü yenileme belirteci Maksimum yaş |MaxAgeMultiFactor |Yenileme belirteçlerini (tüm kullanıcılar için) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Tek öğeli Oturum belirteci Maksimum yaş |MaxAgeSessionSingleFactor<sup>2</sup> |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Çok faktörlü Oturum belirteci Maksimum yaş |MaxAgeSessionMultiFactor<sup>3</sup> |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |

* <sup>1</sup>365 gün olduğu bu öznitelikler için ayarlanabilen en fazla açık uzunluğu.

### <a name="exceptions"></a>Özel Durumlar
| Özellik | Etkiler | Varsayılan |
| --- | --- | --- |
| Yenileme belirteci Maksimum yaş (yetersiz iptal bilgilerini federe kullanıcılar için verilen<sup>1</sup>) |Yenileme belirteçlerini (yetersiz iptal bilgilerini federe kullanıcılar için verilen<sup>1</sup>) |12 saat |
| Belirteç en fazla etkin olmayan (gizli istemciler için verilen) yenileme zamanı |Yenileme belirteçlerini (gizli istemciler için verilen) |90 gün |
| Yenileme belirteci Maksimum yaş (gizli istemciler için verilen) |Yenileme belirteçlerini (gizli istemciler için verilen) |Kadar iptal |

* <sup>1</sup>yetersiz iptal bilgilerini sahip federe kullanıcılar eşitlenmiş "LastPasswordChangeTimestamp" özniteliğine sahip olmayan tüm kullanıcıları içerir. AAD ne zaman eski bir kimlik bilgisi (örneğin değiştirilmiş bir parola) gerektirir ve geri daha sık kullanıcı ve ilişkili belirteçleri hala olmasını sağlamak için iade etmeniz belirteçleri iptal doğrulanamıyor olduğundan bu kullanıcılar bu kısa Maksimum yaş verilen iyi içinde  ayakta duran. Bu deneyimi geliştirmek için Kiracı yöneticileri (Bu Powershell kullanarak kullanıcı nesnesindeki veya Aadconnect'in aracılığıyla ayarlanabilir) "LastPasswordChangeTimestamp" öznitelik eşitleme yapan emin olmalısınız.

### <a name="policy-evaluation-and-prioritization"></a>İlke değerlendirmesi ve önceliği
Oluşturun ve ardından belirli bir uygulama, kuruluşunuzun ve hizmet sorumluları bir belirteç ömrü ilkesi atayabilirsiniz. Belirli bir uygulama için birden çok ilke uygulanabilir. Etkinleşir belirteç ömrü ilkesi bu kurallara uygun olmalıdır:

* Hizmet sorumlusuna açıkça bir ilke atanırsa, zorlanır.
* Hizmet sorumlusuna ilke yok açıkça atanmışsa, hizmet sorumlusunun üst kuruluş açıkça atanan bir ilke uygulanmaz.
* İlke yok açıkça hizmet sorumlusu veya kuruluş atanırsa, uygulamaya atanmış ilkelerin hiçbiri uygulanmaz.
* Hizmet sorumlusu, kuruluş veya uygulama nesnesi ilkesiz atandıysa, varsayılan değerleri zorlanır. (Bölümündeki tabloya bakın [yapılandırılabilir belirteç ömrünü](#configurable-token-lifetime-properties).)

Uygulama nesneleri ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).

Bir belirtecin geçerlilik belirteci kullanıldığında değerlendirilir. En yüksek öncelikli erişiliyor aplikaci ilke etkili olur.

Burada kullanılan tüm timespans C# göre biçimlendirilir [TimeSpan](/dotnet/api/system.timespan) nesnesi - D.HH:MM:SS.  80 gün ve 30 dakika olacaktır `80.00:30:00`.  D bırakılan sıfır ise, en iyi şekilde 90 dakika olur `00:90:00`.  

> [!NOTE]
> Örnek bir senaryo aşağıda verilmiştir.
>
> Bir kullanıcı, iki web uygulaması erişmek istiyor: Web uygulaması A ve Web uygulama b
> 
> Faktörleri:
> * Her iki web uygulaması aynı üst kuruluşta ' dir.
> * Bir oturum belirteci Maksimum yaş sekiz saat ile belirteç ömrü ilkesi 1, üst kuruluşunuzun varsayılan ayarlanır.
> * Bir Web uygulaması normal kullanım web uygulaması ve tüm ilkeler için bağlı değil.
> * Web uygulama B son derece hassas işlemler için kullanılır. Hizmet sorumlusu belirteci ömrü ilkesi bir oturum belirteci Maksimum yaş 30 dakika olan 2'ye bağlıdır.
>
> 12:00 PM, kullanıcının yeni bir tarayıcı oturumu başlatır ve Web uygulaması a erişmeye çalışır Kullanıcı, Azure AD'ye yeniden yönlendirilir ve oturum açması istenir. Bu, tarayıcıda oturum belirteci içeren bir tanımlama bilgisi oluşturur. Kullanıcı, bir Web uygulaması kullanıcının uygulamaya erişim sağlayan bir kimlik belirteci dön yönlendirilir.
>
> 12:15 PM kullanıcı Web uygulama b erişmeye çalışır. Tarayıcı oturumu tanımlama bilgisi algılar Azure AD'ye yönlendirir. Web uygulaması B'nin hizmet sorumlusu için belirteç ömrü ilkesi 2 bağlandı, ancak ayrıca ana kuruluşla varsayılan belirteç ömrü ilkesi 1 parçasıdır. Hizmet sorumluları için bağlantılı ilkeler kuruluşun varsayılan ilkeler daha yüksek bir önceliğe sahip olduğundan, belirteç ömrü ilkesi 2 etkili olur. Geçerli sayılması için oturum belirteci son 30 dakika içinde ilk yayımlandığında. Kullanıcı geri bunları erişim veren bir kimlik belirteci ile Web uygulaması B yönlendirilir.
>
> 1: 00'da, kullanıcı A. Web uygulamasına erişmeye çalışır. Kullanıcı, Azure AD'ye yeniden yönlendirilir. Tüm ilkeler için Web uygulaması A bağlı olmayan, ancak varsayılan belirteç ömrü ilkesi 1 olan bir kuruluşta olduğundan, bu ilke etkili olur. Son sekiz saat içinde ilk yayımlandığında oturum tanımlama bilgisi algılandı. Kullanıcı, yeni bir kimlik belirteci ile Web uygulaması A dön sessizce yönlendirilir. Kullanıcı kimlik doğrulaması için gerekli değildir.
>
> Hemen sonra kullanıcı, Web uygulama b erişmeye çalışır Kullanıcı, Azure AD'ye yeniden yönlendirilir. Olarak daha önce belirteç ömrü ilkesi 2 etkinleşir. Belirteç 30 dakikadan daha önce verilmiş olduğundan, kullanıcı oturum açma kimlik bilgilerini yeniden girmeniz istenir. Yeni Oturum belirteci ve kimlik belirteci verilir. Kullanıcı daha sonra Web uygulaması b erişebilirsiniz
>
>

## <a name="configurable-policy-property-details"></a>Yapılandırılabilir İlkesi özellik ayrıntıları
### <a name="access-token-lifetime"></a>Erişim belirteci ömrü
**Dize:** AccessTokenLifetime

**Etkiler:** Erişim belirteçleri, kimlik belirteçleri

**Özet:** Bu ilke, ne kadar süreyle erişim ve kimlik belirteçlerini bu kaynak için geçerli olarak kabul edilir denetler. Erişim belirteci ömrü özelliği azaltma, bir erişim belirteci veya kötü amaçlı bir aktör, uzun bir süre için kullanılan kimlik belirteci riskini azaltır. (Bu belirteçleri iptal edilemiyor.) Belirteçlerin daha sık değiştirilmesi gerektiği için performansı olumsuz şekilde etkilenir dengedir.

### <a name="refresh-token-max-inactive-time"></a>Yenileme belirteci en fazla etkin olmayan zamanı
**Dize:** MaxInactiveTime

**Etkiler:** Yenileme belirteçlerini

**Özet:** Bu ilke, istemci artık bu kaynağa erişmeye çalışırken yeni bir erişim/yenileme belirteci çifti almak için kullanmadan önce ne kadar eski bir yenileme belirteci olabilir denetler. Yeni bir yenileme belirteci yenileme belirteci kullanıldığında genellikle döndürüldüğünden istemcinin belirtilen süre boyunca geçerli yenileme belirtecini kullanarak herhangi bir kaynağa erişmeye çalışırsa, bu ilke erişimi engeller.

Yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlayabilir kendi istemci üzerinde etkin olan kullanıcılar bu ilkeyi zorlar.

Yenileme belirteci etkin olmayan zaman sınırı özelliği, tek öğeli belirteci Maksimum yaş ve çok faktörlü yenileme belirteci Maksimum yaş özellikleri daha düşük bir değere ayarlanmalıdır.

### <a name="single-factor-refresh-token-max-age"></a>Tek öğeli yenileme belirteci Maksimum yaş
**Dize:** MaxAgeSingleFactor

**Etkiler:** Yenileme belirteçlerini

**Özet:** Bu ilke denetimleri ne kadar bir kullanıcı, son başarılı bir şekilde tek bir etkenle kullanarak kimlik doğrulaması sonra yeni bir erişim/yenileme belirteci çifti almak için bir yenileme belirteci kullanabilir. Bir kullanıcının kimliğini doğrular ve yeni bir yenileme belirteci aldıktan sonra kullanıcının yenileme belirteci akışı belirtilen süre için kullanabilirsiniz. (Bu geçerli bir yenileme belirteci iptal ve bunu etkin olmayan süresinden daha uzun bir süre kullanılmayan bırakılır değil sürece geçerlidir.) Bu noktada, kullanıcı yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlayabilir zorlanır.

En yüksek yaş azaltarak kullanıcılara daha sık doğrulamak için zorlar. Tek öğeli kimlik doğrulama çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, çok faktörlü yenileme belirteci Maksimum yaş özelliği'den küçük veya ona eşit bir değer ayarlamanızı öneririz.

### <a name="multi-factor-refresh-token-max-age"></a>Çok faktörlü yenileme belirteci Maksimum yaş
**Dize:** MaxAgeMultiFactor

**Etkiler:** Yenileme belirteçlerini

**Özet:** Bu ilke denetimleri ne kadar bir kullanıcı, son başarılı bir şekilde çoklu faktörlerle kullanarak kimlik doğrulaması sonra yeni bir erişim/yenileme belirteci çifti almak için bir yenileme belirteci kullanabilir. Bir kullanıcının kimliğini doğrular ve yeni bir yenileme belirteci aldıktan sonra kullanıcının yenileme belirteci akışı belirtilen süre için kullanabilirsiniz. (Bu geçerli bir yenileme belirteci iptal ve etkin olmayan süresinden daha uzun bir süre kullanılmayan değil sürece geçerlidir.) Bu noktada, kullanıcıların yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlayabilir zorlanır.

En yüksek yaş azaltarak kullanıcılara daha sık doğrulamak için zorlar. Tek öğeli kimlik doğrulama çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, tek öğeli yenileme belirteci Maksimum yaş özelliğine ilişkin değerden büyük veya ona eşit bir değer ayarlamanızı öneririz.

### <a name="single-factor-session-token-max-age"></a>Tek öğeli Oturum belirteci Maksimum yaş
**Dize:** MaxAgeSessionSingleFactor

**Etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bu ilke denetimleri ne kadar bir kullanıcı, son başarılı bir şekilde tek bir etkenle kullanarak kimlik doğrulaması sonra yeni kimliği ve oturum belirteci almak için oturum belirteci kullanabilir. Bir kullanıcının kimliğini doğrular ve yeni oturum belirteci aldıktan sonra kullanıcı oturum belirteci akışı belirtilen süre için kullanabilirsiniz. (Geçerli oturum belirteci iptal ve süresi geçmemiş sürece bu durum geçerlidir.) Belirtilen süre sonunda, yeni oturum belirteci almak için yeniden kimlik doğrulamaya zorlayabilir kullanıcı zorlanır.

En yüksek yaş azaltarak kullanıcılara daha sık doğrulamak için zorlar. Tek öğeli kimlik doğrulama çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, eşit veya çok faktörlü Oturum belirteci Maksimum yaş özelliğinden daha az olan bir değer ayarlamanızı öneririz.

### <a name="multi-factor-session-token-max-age"></a>Çok faktörlü Oturum belirteci Maksimum yaş
**Dize:** MaxAgeSessionMultiFactor

**Etkiler:** Oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** Bir kullanıcı son saatten sonra yeni kimliği ve oturum belirteci almak için oturum belirteci kullanabilir ne kadar bu ilke denetimleri başarıyla çoklu faktörlerle kullanarak kimlik doğrulaması yapılmış. Bir kullanıcının kimliğini doğrular ve yeni oturum belirteci aldıktan sonra kullanıcı oturum belirteci akışı belirtilen süre için kullanabilirsiniz. (Geçerli oturum belirteci iptal ve süresi geçmemiş sürece bu durum geçerlidir.) Belirtilen süre sonunda, yeni oturum belirteci almak için yeniden kimlik doğrulamaya zorlayabilir kullanıcı zorlanır.

En yüksek yaş azaltarak kullanıcılara daha sık doğrulamak için zorlar. Tek öğeli kimlik doğrulama çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, tek öğeli Oturum belirteci Maksimum yaş özelliğine ilişkin değerden büyük veya ona eşit bir değer ayarlamanızı öneririz.

## <a name="example-token-lifetime-policies"></a>Örnek belirteç ömrü ilkeleri
Birçok senaryo oluşturabilir ve uygulamaları, hizmet sorumluları ve genel kuruluşunuz için belirteç ömrünü yönetmek, Azure AD'de kullanılabilir. Bu bölümde, yeni kurallar için büyük oranda yansıtmaktadır yardımcı olabilecek bazı genel ilke senaryoları size rehberlik eder:

* Belirteç Ömrü
* Etkin olmayan zaman belirteci sınırı
* Belirteç Maksimum yaş

Örneklerde, şunların nasıl yapılır:

* Bir kuruluşun varsayılan ilkesini yönetme
* Web oturumu açma için bir ilke oluşturun
* Bir web API'si çağıran bir yerel uygulama için bir ilke oluşturun
* Gelişmiş bir ilke yönetme

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki örneklerde, oluşturma, güncelleştirme, bağlantı ve uygulamaları, hizmet sorumluları ve kuruluşunuzun genel ilkeleri silin. Azure AD'ye yeni başladıysanız, hakkında bilgi edinin öneririz [bir Azure AD kiracısı edinme](quickstart-create-new-tenant.md) bu örnekleri ile devam etmeden önce.  

Başlamak için aşağıdaki adımları uygulayın:

1. En son sürümünü indirin [Azure AD PowerShell modülü genel Önizleme sürümü](https://www.powershellgallery.com/packages/AzureADPreview).
2. Çalıştırma `Connect` komutu Azure AD yönetici hesabınızda oturum açın. Her zaman bu komutu çalıştırın, yeni bir oturum başlatın.

    ```powershell
    Connect-AzureAD -Confirm
    ```

3. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. Aşağıdaki senaryolarda çoğu işlemlerinden sonra bu komutu çalıştırın. Komutu çalıştırmadan da yardımcı olur, Al ** ** ilkelerinizin.

    ```powershell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Örnek: Bir kuruluşun varsayılan ilkesini yönetme
Bu örnekte, kullanıcılarınızın oturum sağlar bir ilke daha az sıklıkta kuruluşunuz genelinde oluşturun. Bunu yapmak için tek öğeli Yenile kuruluşunuz genelinde uygulanan belirteçleri için bir belirteç ömrü ilkesi oluşturun. İlke, kuruluşunuzdaki her bir uygulama ve bir ilke kümesi zaten sahip olmayan her hizmet sorumlusu için uygulanır.

1. Bir belirteç ömrü ilkesi oluşturun.

    1. Kümesi tek öğeli yenileme belirteci "kadar iptal edilen." Erişimi iptal kadar belirtecin süresi sona ermiyor. Aşağıdaki ilke tanımı oluşturun:

        ```powershell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2. İlkeyi oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3. Yeni ilkeniz bakın ve ilkenin almak için **objectID**, aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

2. İlkesini güncelleştirin.

    Bu örnekte, ayarladığınız ilk ilke hizmetiniz için gerekli olarak katı değil karar verebilirsiniz. Tek öğeli yenileme süresi iki gün içinde dolacak belirtecinizi ayarlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName $policy.DisplayName -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Örnek: Web oturumu açma için bir ilke oluşturun

Bu örnekte, kullanıcıların daha sık web uygulamanızda kimlik doğrulaması gerektiren bir ilke oluşturun. Bu ilke, web uygulamanızın hizmet sorumlusuna erişim/kimlik belirteçlerinin ömrü ve en yüksek yaş çok faktörlü Oturum belirteci ayarlar.

1. Bir belirteç ömrü ilkesi oluşturun.

    Bu ilke, oturum açmak için web, erişim/kimlik belirteci ömrü ve en fazla tek öğeli Oturum belirteci yaş iki saate ayarlar.

    1. İlkeyi oluşturmak için şu komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2. Yeni ilkeniz görmek için ve ilkeyi almak üzere **objectID**, aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

2. Hizmet sorumlunuzu ilkeyi atayın. Almanız gereken **objectID** hizmet sorumlunuzun.

    1. Kullanım [Get-azureadserviceprincipal cmdlet'ini](/powershell/module/azuread/get-azureadserviceprincipal) kuruluşunuzun tüm hizmet sorumlularını veya tek bir hizmet sorumlusu görmek için cmdlet.
        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"
        ```

    2. Hizmet sorumlusuna sahip olduğunuzda, aşağıdaki komutu çalıştırın:
        ```powershell
        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Örnek: Bir web API'si çağıran bir yerel uygulama için bir ilke oluşturun
Bu örnekte, kullanıcıların daha az sıklıkta kimlik doğrulaması gerektiren bir ilke oluşturun. İlke de kullanıcı, kullanıcı yeniden kimliğini doğrulaması gerekir önce etkin olmayan olabilir süreyi uzatır. Web API'sine ilke uygulanır. Yerel uygulama, web API'si bir kaynak istediğinde, bu ilke uygulanır.

1. Bir belirteç ömrü ilkesi oluşturun.

    1. Web API'si için katı bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2. Yeni ilkeniz görmek için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

2. Web API'nize ilkeyi atayın. Almanız gereken **objectID** uygulamanızın. Kullanma [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) uygulamanızın bulmak için cmdlet **objectID**, veya [Azure portalında](https://portal.azure.com/).

    Alma **objectID** uygulamanızın ve atama İlkesi:

    ```powershell
    # Get the application
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Fourth Coffee Web API'"

    # Assign the policy to your web API.
    Add-AzureADApplicationPolicy -Id $app.ObjectId -RefObjectId $policy.Id
    ```

### <a name="example-manage-an-advanced-policy"></a>Örnek: Gelişmiş bir ilke yönetme
Bu örnekte, öncelik sistem nasıl çalıştığını öğrenmek için bazı ilkeler oluşturursunuz. Ayrıca, çok sayıda nesneye uygulanan birden çok ilke yönetmeyi öğrenin.

1. Bir belirteç ömrü ilkesi oluşturun.

    1. 30 gün için tek öğeli yenileme belirteci ömrü ayarlar bir kuruluş varsayılan ilkeyi oluşturmak için aşağıdaki komutu çalıştırın:

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2. Yeni ilkeniz görmek için aşağıdaki komutu çalıştırın:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

2. Bir hizmet sorumlusuna ilke atayın.

    Artık, kuruluşun tamamı için geçerli bir ilke var. Bu 30 günlük ilkesi için belirli bir hizmet sorumlusu korumak, ancak "kadar iptal edilen." sayısı üst sınırı için kuruluşunuzun varsayılan ilkeyi değiştirmek isteyebilirsiniz

    1. Kuruluşunuzun tüm hizmet sorumlularını görmek için kullandığınız [Get-azureadserviceprincipal cmdlet'ini](/powershell/module/azuread/get-azureadserviceprincipal) cmdlet'i.

    2. Hizmet sorumlusuna sahip olduğunuzda, aşağıdaki komutu çalıştırın:

        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

3. Ayarlama `IsOrganizationDefault` bayrağı false:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Yeni bir kuruluş varsayılan ilkesi oluşturun:

    ```powershell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Artık hizmet sorumlunuzu bağlı özgün ilkenin var ve yeni ilke, kuruluşunuzun varsayılan ilke olarak ayarlanır. Hizmet sorumluları için uygulanan ilkeler, kuruluş varsayılan ilkeleri üzerinde önceliğe sahip unutmamak önemlidir.

## <a name="cmdlet-reference"></a>Cmdlet başvurusu

### <a name="manage-policies"></a>İlkeleri yönetme

Aşağıdaki cmdlet, ilkelerini yönetmek için kullanabilirsiniz.

#### <a name="new-azureadpolicy"></a>New-AzureADPolicy

Yeni bir ilke oluşturur.

```powershell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Tüm ilke kuralları içeren dizeleştirilmiş JSON dizisi. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |TRUE ise, ilke, kuruluşunuzun varsayılan ilke olarak ayarlar. False ise, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |İlke türü. Belirteç ömürleri için her zaman "TokenLifetimePolicy." kullanın | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Tüm Azure AD ilkeleri veya belirtilen ilkesini alır.

```powershell
Get-AzureADPolicy
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> [İsteğe bağlı] |**Nesne Kimliği (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Tüm uygulamaları ve bir ilke için bağlı hizmet sorumluları alır.

```powershell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Var olan bir ilkeyi güncelleştirir.

```powershell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code> [İsteğe bağlı] |Tüm ilke kuralları içeren dizeleştirilmiş JSON dizisi. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code> [İsteğe bağlı] |TRUE ise, ilke, kuruluşunuzun varsayılan ilke olarak ayarlar. False ise, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> [İsteğe bağlı] |İlke türü. Belirteç ömürleri için her zaman "TokenLifetimePolicy." kullanın |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Remove-AzureADPolicy
Belirtilen ilke siler.

```powershell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** istediğiniz ilke. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Uygulama ilkeleri
Uygulama ilkeleri için aşağıdaki cmdlet'leri kullanabilirsiniz.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Add-AzureADApplicationPolicy
Belirtilen ilke uygulama bağlar.

```powershell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectID** ilkesinin. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Bir uygulamaya atanan ilkesini alır.

```powershell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Remove-AzureADApplicationPolicy
Bir ilke, bir uygulamadan kaldırır.

```powershell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectID** ilkesinin. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Hizmet sorumlusu ilkeleri
Aşağıdaki cmdlet'leri için hizmet sorumlusu ilkeleri kullanabilirsiniz.

#### <a name="add-azureadserviceprincipalpolicy"></a>AzureADServicePrincipalPolicy ekleyin
Belirtilen ilke için bir hizmet sorumlusu bağlar.

```powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectID** ilkesinin. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Belirtilen hizmet sorumlusuna bağlı herhangi bir ilke alır.

```powershell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Remove-AzureADServicePrincipalPolicy
İlke, belirtilen hizmet sorumlusu kaldırır.

```powershell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Nesne Kimliği (ID)** uygulama. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectID** ilkesinin. | `-PolicyId <ObjectId of Policy>` |
