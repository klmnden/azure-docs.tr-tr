---
title: Azure Active Directory'de yapılandırılabilir belirteci ömürleri | Microsoft Docs
description: Azure AD tarafından yayınlanan belirteçleri için yaşam süresi ayarlamak öğrenin.
services: active-directory
documentationcenter: ''
author: hpsin
manager: mtillman
editor: ''
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: hirsin
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: 086a2fde5905321da7d5689b6f1ee2f5139209ba
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588871"
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Azure Active Directory'de (genel Önizleme) yapılandırılabilir belirteci yaşam süresi
Azure Active Directory (Azure AD) tarafından verilmiş bir belirteç ömrü belirtebilirsiniz. Kuruluşunuzdaki tüm uygulamalar, kuruluşunuzda, çok kiracılı (çok kuruluş) uygulama veya belirli hizmet sorumlusu belirteci yaşam süresi ayarlayabilirsiniz.

> [!IMPORTANT]
> Önizleme sırasında müşterilerden işitme sonra size yeni bir Azure Active Directory koşullu erişim özelliğindeki bu işlevin yerini planlıyorsanız.  Yeni özellik tamamlandıktan sonra bu işlev sonunda bir bildirim süre kullanım dışı kalacaktır.  Yapılandırılabilir belirteç ömrü ilkesi kullanırsanız, kullanılabilir olduğunda yeni koşullu erişim özelliğini geçiş yapmak için hazır olun. 
>
>

Azure AD'de bireysel uygulamaları ya da bir kuruluşta tüm uygulamaları zorlanan kuralları kümesi bir ilke nesnesi temsil eder. Her ilke türü bir benzersiz yapısıyla atanmış olan nesnelere uygulanan özellikler kümesi vardır.

Bir ilke, kuruluşunuz için varsayılan ilke olarak belirleyebilirsiniz. Yüksek önceliğe sahip bir ilke tarafından kılınmayan sürece kuruluştaki herhangi bir uygulama ilkesi uygulanır. Ayrıca, belirli uygulamalar için bir ilke atayabilirsiniz. Öncelik sırasını ilke türüne göre değişir.

> [!NOTE]
> Yapılandırılabilir belirteç ömrü ilkesi SharePoint Online için desteklenmiyor.  Bu ilke PowerShell aracılığıyla oluşturma olanağına sahip olsa da, SharePoint Online bu ilkeyi bildirecektir değil. Başvurmak [SharePoint Online blog](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) oturum boşta kalma zaman aşımı yapılandırma hakkında daha fazla bilgi edinmek için.
>* SharePoint Online erişim belirteci için varsayılan yaşam süresi 1 saattir. 
>* Varsayılan en fazla etkin olmayan zaman SharePoint Online yenileme belirteci 90 gündür.
>

## <a name="token-types"></a>Belirteç türleri

Yenileme belirteçleri, erişim belirteçleri, oturum belirteçleri ve kimlik belirteçlerini belirteç ömrü ilkelerini ayarlayabilirsiniz.

### <a name="access-tokens"></a>Erişim belirteçleri
İstemciler, korunan bir kaynağa erişmek için erişim belirteçleri kullanın. Bir erişim belirteci, kullanıcı, istemci ve kaynak yalnızca belirli bir birleşim için kullanılabilir. Erişim belirteci iptal edilemiyor ve bunların süre sonu kadar geçerlidir. Bir erişim belirteci elde kötü amaçlı bir aktör yaşam uzantı için kullanabilirsiniz. Bir erişim belirteci ömrü ayarlama sistem performansını artırmak ve kullanıcı hesabının devre dışı bırakıldıktan sonra istemci erişimi korur süre miktarını artırmayı arasında bir denge olur. Geliştirilmiş sistem performansı kez bir istemci yeni bir erişim belirteci alması gerekiyor sayısını azaltarak elde edilir.  1 saat - varsayılan değer 1 saat sonra istemci yenileme belirteci (genellikle sessizce) yeni bir yenileme belirteci edinmeli ve belirteç erişmek için kullanmanız gerekir. 

### <a name="refresh-tokens"></a>Yenileme belirteçlerini
Bir istemci korunan bir kaynağa erişmek için bir erişim belirteci yaptığında, istemci ayrıca bir yenileme belirteci alır. Yenileme belirteci geçerli erişim belirtecinin süresi dolduğunda yeni erişim/yenileme belirteci çiftleri elde etmek için kullanılır. Bir yenileme belirteci birleşimi kullanıcı ve istemci bağlıdır. Bir yenileme belirteci olabilir [herhangi bir zamanda İptal](develop/active-directory-token-and-claims.md#token-revocation), ve belirteç her kullanılışında belirtecin geçerlilik denetlenir.  

Bu yenileme belirteçleri ne kadar süreyle kullanılabilir etkiler gibi gizli ve ortak istemcilerinin arasında ayrım yapmak önemlidir. Farklı istemci türleri hakkında daha fazla bilgi için bkz: [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Gizli istemci yenileme belirteçleri ile belirteci yaşam süresi
Gizli istemcileri güvenli bir şekilde bir istemci parolası (gizli) depolayabilirsiniz uygulamalardır. İsteklerin güvenli istemci uygulamasından ve kötü amaçlı aktör değil, geldiğini kanıtlarlar. Örneğin, bir web uygulaması web sunucusunda bir istemci parolası depolayabileceğiniz gizli bir istemcidir. Bu açık değil. Bu akışlar daha güvenlidir, yenileme belirteçleri bu akışlara verilen varsayılan ömrü çünkü `until-revoked`İlkesi kullanılarak değiştirilemez ve gönüllü parola sıfırlama üzerinde iptal değil.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Ortak istemci yenileme belirteçleri ile belirteci yaşam süresi

Ortak istemcileri güvenli bir şekilde bir istemci parolası (gizli) depolanamıyor. Örneğin, ortak bir istemci olarak kabul edilir şekilde iOS/Android uygulama gizli anahtarı kaynak sahibinden belirsizleştirirseniz olamaz. İlkeleri yenileme belirteçleri belirtilen süresinden daha eski ortak istemcilerden gelen yeni bir erişim/yenileme belirteci çifti ele geçirmesini önlemek için kaynak ayarlayabilirsiniz. (Bunu yapmak için yenileme belirteci etkin olmayan zaman sınırı özelliğini kullanın (`MaxInactiveTime`).) Sonrasında artık yenileme belirteçleri kabul süresini ayarlamak için ilkelerini de kullanabilirsiniz. (Bunu yapmak için yenileme belirteci Maksimum yaş özelliğini kullanın.) Zaman ve ne sıklıkta kullanıcının sessiz bir şekilde, bir ortak istemci uygulamasını kullanırken, yeniden kimlik doğrulaması yerine kimlik bilgilerinizi yeniden girmeniz gerekir denetlemek için bir yenileme belirteci ömrü ayarlayabilirsiniz.

### <a name="id-tokens"></a>Kimliği belirteçleri
Kimlik belirteçlerini Web siteleri ve yerel istemcilerine geçirilir. Kimlik belirteçlerini bir kullanıcı profili bilgilerini içerir. Bir kimliği belirteci kullanıcı ve istemci belirli bir birleşim bağlıdır. Kimlik belirteçlerini kendi süre sonu kadar geçerli kabul edilir. Genellikle, bir kullanıcının bir web uygulaması eşleşip kimliği belirteç ömrü için uygulamada oturum yaşam kullanıcı için verilen. Ne sıklıkta web uygulaması uygulama oturum sona erer ve ne sıklıkta Azure AD ile (Sessiz veya etkileşimli) kimliğinin yeniden doğrulanması kullanıcının gerektiren denetlemek için bir kimliği belirteç ömrü ayarlayabilirsiniz.

### <a name="single-sign-on-session-tokens"></a>Tek oturum açma oturumu belirteçleri
Bir kullanıcı Azure AD ile doğruladığında, tek bir oturum açma oturumu (SSO) kullanıcının tarayıcı ve Azure AD ile kurulur. Bir tanımlama bilgisi biçiminde SSO belirtecin bu oturumu temsil eder. SSO Oturum belirteci bir belirli bir kaynak/istemci uygulaması ilişkilendirilmediğinden unutmayın. SSO oturum belirteçleri iptal edilebilir ve kullanıldıkları her zaman geçerliliği denetlenir.

Azure AD iki tür SSO oturum belirteçleri kullanır: kalıcı ve kalıcı olmayan. Kalıcı oturum belirteç kalıcı tanımlama bilgileri tarayıcı tarafından depolanır. Kalıcı olmayan oturum belirteç oturum tanımlama bilgileri depolanır. (Tarayıcı kapatıldığında oturum tanımlama bilgileri yok.) Genellikle, kalıcı olmayan oturum belirteci depolanır. Ancak, kullanıcı seçtiğinde **Oturumumu açık bırak** onay kutusunu kalıcı Oturum belirteci kimlik doğrulaması sırasında depolanır.

Kalıcı olmayan oturum belirteçleri 24 saatlik bir ömrü vardır. Kalıcı belirteçleri 180 gün ömrü vardır. SSO Oturum belirteci, geçerlilik süresi içinde kullanılan dilediğiniz zaman, başka bir 24 saat veya belirteç türüne bağlı olarak 180 gün geçerlilik süresini genişletilir. SSO Oturum belirteci, geçerlilik süresi içinde kullanılmazsa, olarak kabul süresi doldu ve artık kabul edilir.

İlk oturum belirteci hangi Oturum belirteci artık kabul edilen ötesinde verilmiş sonra süresini ayarlamak için bir ilke kullanabilirsiniz. (Bunu yapmak için oturum belirteci Maksimum yaş özelliğini kullanın.) Zaman ve ne sıklıkta bir kullanıcı sessiz bir şekilde, bir web uygulaması kullanırken kimlik doğrulaması gerçekleştirilen yerine kimlik bilgilerinizi yeniden girmeniz gereken denetlemek için bir oturum belirteci ömrü ayarlayabilirsiniz.

### <a name="token-lifetime-policy-properties"></a>Belirteç ömrü ilkesi özellikleri
Belirteç ömrü ilkesi, belirteç ömrü kuralları içeren ilke nesne türüdür. Belirtilen belirteç yaşam süreleri denetlemek için bir ilkenin özelliklerini kullanın. Hiçbir ilke ayarlanırsa, sistem varsayılan yaşam süresi değeri zorlar.

### <a name="configurable-token-lifetime-properties"></a>Yapılandırılabilir belirteç ömrü özellikleri
| Özellik | İlke özellik dizesi | Etkiler | Varsayılan | Minimum | Maksimum |
| --- | --- | --- | --- | --- | --- |
| Erişim belirteci ömrü |AccessTokenLifetime |Erişim belirteçleri, kimlik belirteçlerini, SAML2 belirteçleri |1 saat |10 dakika |1 gün |
| Etkin olmayan zaman belirteci sınırı Yenile |MaxInactiveTime |Yenileme belirteçlerini |90 gün |10 dakika |90 gün |
| Tek Faktörlü yenileme belirteci Maksimum yaş |MaxAgeSingleFactor |Yenileme belirteçlerini (tüm kullanıcılar için) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Çok faktörlü yenileme belirteci Maksimum yaş |MaxAgeMultiFactor |Yenileme belirteçlerini (tüm kullanıcılar için) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Tek Faktörlü Oturum belirteci Maksimum yaş |MaxAgeSessionSingleFactor<sup>2</sup> |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |
| Çok faktörlü Oturum belirteci Maksimum yaş |MaxAgeSessionMultiFactor<sup>3</sup> |Oturum belirteçleri (kalıcı ve kalıcı olmayan) |Kadar iptal |10 dakika |Kadar iptal<sup>1</sup> |

* <sup>1</sup>365 gündür bu öznitelikler için ayarlanabilir en fazla açık uzunluğu.
* <sup>2</sup>varsa **MaxAgeSessionSingleFactor** ayarlanmazsa bu değeri alır **MaxAgeSingleFactor** değeri. Hiçbir parametre ayarlanırsa, varsayılan değer (kadar iptal edilen) özelliği alır.
* <sup>3</sup>varsa **MaxAgeSessionMultiFactor** ayarlanmazsa bu değeri alır **MaxAgeMultiFactor** değeri. Hiçbir parametre ayarlanırsa, varsayılan değer (kadar iptal edilen) özelliği alır.

### <a name="exceptions"></a>Özel durumlar
| Özellik | Etkiler | Varsayılan |
| --- | --- | --- |
| Belirteç Maksimum yaş Yenile (yetersiz iptal bilgilerini federe kullanıcılar için verilen<sup>1</sup>) |Yenileme belirteçlerini (yetersiz iptal bilgilerini federe kullanıcılar için verilen<sup>1</sup>) |12 saat |
| Belirteç etkin olmayan (gizli istemcileri için verilen) zaman sınırı Yenile |Yenileme belirteçlerini (gizli istemcileri için verilen) |90 gün |
| Belirteç Maksimum yaş (gizli istemcileri için verilen) Yenile |Yenileme belirteçlerini (gizli istemcileri için verilen) |Kadar iptal |

* <sup>1</sup>yetersiz iptal bilgilerini sahip federe kullanıcılar eşitlenen "LastPasswordChangeTimestamp" özniteliğine sahip olmayan tüm kullanıcıları içerir. AAD eski bir kimlik bilgisi (örneğin, değiştirilmiş bir parola) bağlıdır ve daha sık ilişkilendirilmiş belirteçleri ve kullanıcı yine de iyi yeri olduğundan emin olmak için geri denetlemelidir belirteçleri iptal etmek ne zaman doğrulayamadı olduğu için bu kullanıcılara bu kısa Maksimum yaş verilir. Bu deneyimini geliştirmek için Kiracı yöneticileri (Bu Powershell kullanarak kullanıcı nesnesindeki veya Modu'nu aracılığıyla ayarlanabilir) "LastPasswordChangeTimestamp" özniteliği eşitleniyor emin olmanız gerekir.

### <a name="policy-evaluation-and-prioritization"></a>İlke değerlendirmesi ve öncelik belirleme
Oluşturma ve bir belirteç ömrü ilkesi belirli bir uygulama, kuruluşunuz ve hizmet asıl adı atayın. Birden çok ilke belirli bir uygulama için geçerli. Etkinleşir belirteç ömrü ilkesi bu kurallar aşağıdaki gibidir:

* Bir ilke açıkça hizmet sorumlusu atanmışsa zorlanır.
* İlke yok açıkça hizmet sorumlusu atanırsa, açıkça hizmet sorumlusu üst kuruluşa atanan bir ilke uygulanır.
* İlke yok açıkça hizmet sorumlusu veya kuruluş atanırsa, uygulamaya atanan ilkesi uygulanır.
* Hizmet sorumlusu, kuruluş veya uygulama nesnesi hiçbir ilke atanmışsa, varsayılan değerleri zorlanır. (Bölümündeki tabloya bakın [yapılandırılabilir belirteç ömrü özellikleri](#configurable-token-lifetime-properties).)

Uygulama nesneleri ve hizmet asıl nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](active-directory-application-objects.md).

Bir belirtecin geçerlilik belirteç kullanıldığında değerlendirilir. Erişiliyor uygulama üzerinde en yüksek öncelikli ilke etkili olur.

Burada kullanılan tüm timespans C# göre biçimlendirileceğini [TimeSpan](https://msdn.microsoft.com/library/system.timespan) nesnesi - D.HH:MM:SS.  80 gün ve 30 dakika olacak şekilde `80.00:30:00`.  D bırakılan sıfır ise, başında böylece 90 dakika olur `00:90:00`.  

> [!NOTE]
> Burada, örnek bir senaryo verilmiştir.
>
> Bir kullanıcı iki web uygulamasından erişmek istiyor: bir Web uygulaması ve Web uygulama b
> 
> Faktörleri:
> * Aynı üst kuruluştaki her iki web uygulamalardır.
> * Belirteç ömrü ilkesi 1 bir oturum belirteci Maksimum yaş sekiz saat ile üst kuruluşunuzun varsayılan olarak ayarlanır.
> * Web uygulaması A normal kullanım web uygulaması ve tüm ilkeleri bağlı değil.
> * Web uygulaması B son derece hassas işlemler için kullanılır. Kendi hizmet sorumlusu belirteç ömrü ilkesi bir oturum belirteci Maksimum yaş 30 dakika olan 2'ye bağlıdır.
>
> 12:00 PM, kullanıcı yeni bir tarayıcı oturumu başlatır ve Web uygulaması A. erişmeyi dener Kullanıcı için Azure AD yönlendirilir ve oturum açması istenir. Oturum belirteci tarayıcıda sahip bir tanımlama bilgisi oluşturur. Kullanıcı, kullanıcının uygulamaya erişim sağlayan bir kimliği belirteci ile bir Web uygulaması için yeniden yönlendirilir.
>
> 12: 15'te, kullanıcı Web uygulama B. erişmeyi dener Tarayıcı oturumu tanımlama bilgisi algılar Azure AD yönlendirir. Web uygulaması B'nin hizmet sorumlusu belirteç ömrü ilkesi 2'ye bağlıdır, ancak üst kuruluşla varsayılan belirteç ömrü ilkesi 1 parçası olduğunu da. Hizmet sorumluları bağlantılı ilkeler kuruluşun varsayılan ilkelerden daha yüksek bir öncelik olduğundan belirteç ömrü ilkesi 2 etkinleşir. Geçerli kabul edilir şekilde oturum belirteci son 30 dakika içinde ilk yayımlandığında. Kullanıcı erişim veren bir kimliği belirteci ile Web uygulaması B dön yönlendirilir.
>
> 1: 00'da, kullanıcı Web uygulaması A. erişmeyi dener Kullanıcı, Azure AD ile yönlendirilir. Web Uygulama A tüm ilkeleri bağlı değil, ancak bu ilke, varsayılan belirteç ömrü ilkesi 1 olan bir kuruluşta olduğundan, etkili olur. Son sekiz saat içinde ilk yayımlandığında oturum tanımlama bilgisi algılandı. Kullanıcı, yeni bir kimliği belirteci Web uygulamasıyla A dön sessizce yönlendirilir. Kullanıcı kimlik doğrulaması için gerekli değildir.
>
> Kullanıcı daha sonra Web uygulaması B. erişmeyi dener hemen Kullanıcı, Azure AD ile yönlendirilir. Olarak daha önce belirteç ömrü ilkesi 2 etkinleşir. Belirteç 30 dakikadan daha önce verilmiş olduğundan, kullanıcı oturum açma kimlik bilgilerini yeniden girmeniz istenir. Yeni Oturum belirteci ve kimliği belirteci verilir. Kullanıcı daha sonra Web uygulama B. erişebilirsiniz
>
>

## <a name="configurable-policy-property-details"></a>Yapılandırılabilir İlkesi özellik ayrıntıları
### <a name="access-token-lifetime"></a>Erişim belirteci ömrü
**Dize:** AccessTokenLifetime

**Etkiler:** erişim belirteçleri, kimlik belirteçlerini

**Özet:** Bu ilke ne kadar süreyle erişim ve kimliği belirteçleri bu kaynak için geçerli olarak kabul denetler. Erişim belirteci ömrü özelliği azaltma bir erişim belirteci veya kötü amaçlı bir oyuncu tarafından uzun bir süre için kullanılan kimliği belirteci riskini azaltır. (Bu belirteçleri iptal edilemiyor.) Dengelemeyi performansı olumsuz şekilde etkilenir daha sık değiştirilecek belirteçlere sahip olmasıdır.

### <a name="refresh-token-max-inactive-time"></a>Etkin olmayan zaman belirteci sınırı Yenile
**Dize:** MaxInactiveTime

**Etkiler:** yenileme belirteçleri

**Özet:** Bu ilke bir istemci artık bu kaynağa erişmeye çalışırken zaman yeni bir erişim/yenileme belirteci çifti almak üzere kullanabilmeniz için önce ne kadar eski bir yenileme belirteci olabilir denetler. Bir yenileme belirteci kullanıldığında, yeni bir yenileme belirteci genellikle döndürüldüğünden, istemcinin belirtilen süre boyunca geçerli yenileme belirtecini kullanarak herhangi bir kaynağa erişmeye çalışırsa, bu ilkeyi erişimi engeller.

Bu ilke, yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya, istemcide etkin edilmemiş kullanıcılar zorlar.

Yenileme belirteci etkin olmayan zaman sınırı özelliğini tek Etmenli belirteci Maksimum yaş ve çok faktörlü yenileme belirteci Maksimum yaş Özellikler'den daha düşük bir değere ayarlamalısınız.

### <a name="single-factor-refresh-token-max-age"></a>Tek Faktörlü yenileme belirteci Maksimum yaş
**Dize:** MaxAgeSingleFactor

**Etkiler:** yenileme belirteçleri

**Özet:** bir kullanıcı bir yenileme belirteci son yalnızca tek bir faktörü kullanarak kimlik doğrulamasının sonra yeni bir erişim/yenileme belirteci çifti almak için kullanabileceğiniz ne kadar bu ilke denetler. Bir kullanıcının kimliğini doğrular ve yeni bir yenileme belirteci aldıktan sonra kullanıcı belirtilen süre yenileme belirteci akışı kullanabilirsiniz. (Bu geçerli yenileme belirteci iptal ve, etkin olmayan saatten daha uzun bir süre kullanılmayan bırakılır değil sürece geçerlidir.) Bu noktada, kullanıcı yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlanır.

Maksimum yaş azaltarak daha sık kimlik doğrulaması yapmalarına zorlar. Tek faktörlü kimlik doğrulaması çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, eşit veya çok faktörlü yenileme belirteci Maksimum yaş özelliğinden daha düşük bir değere ayarlamanız önerilir.

### <a name="multi-factor-refresh-token-max-age"></a>Çok faktörlü yenileme belirteci Maksimum yaş
**Dize:** MaxAgeMultiFactor

**Etkiler:** yenileme belirteçleri

**Özet:** bir kullanıcı bir yenileme belirteci son birden çok faktörünü kullanarak kimlik doğrulamasının sonra yeni bir erişim/yenileme belirteci çifti almak için kullanabileceğiniz ne kadar bu ilke denetler. Bir kullanıcının kimliğini doğrular ve yeni bir yenileme belirteci aldıktan sonra kullanıcı belirtilen süre yenileme belirteci akışı kullanabilirsiniz. (Bu geçerli yenileme belirteci iptal ve etkin olmayan saatten daha uzun bir süre kullanılmayan değil sürece geçerlidir.) Bu noktada, kullanıcıların yeni bir yenileme belirteci almak için yeniden kimlik doğrulamaya zorlanır.

Maksimum yaş azaltarak daha sık kimlik doğrulaması yapmalarına zorlar. Tek faktörlü kimlik doğrulaması çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, eşit veya bundan tek Faktörlü yenileme belirteci Maksimum yaş özelliği büyük bir değere ayarlamanız önerilir.

### <a name="single-factor-session-token-max-age"></a>Tek Faktörlü Oturum belirteci Maksimum yaş
**Dize:** MaxAgeSessionSingleFactor

**Etkiler:** oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** bir kullanıcı oturum belirteci son yalnızca tek bir faktörü kullanarak kimlik doğrulamasının sonra yeni kimliği ve oturum belirteci almak için kullanabileceğiniz ne kadar bu ilke denetler. Bir kullanıcının kimliğini doğrular ve yeni oturum belirteci aldıktan sonra kullanıcı oturum belirteci akışı belirtilen süre için kullanabilirsiniz. (Geçerli oturum belirteci iptal ve süresi geçmemiş sürece bu doğrudur.) Belirtilen süre sonra kullanıcı yeni oturum belirteci almak için yeniden kimlik doğrulamaya zorlanır.

Maksimum yaş azaltarak daha sık kimlik doğrulaması yapmalarına zorlar. Tek faktörlü kimlik doğrulaması çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, eşit veya çok faktörlü Oturum belirteci Maksimum yaş özelliği'den büyük bir değere ayarlamanız önerilir.

### <a name="multi-factor-session-token-max-age"></a>Çok faktörlü Oturum belirteci Maksimum yaş
**Dize:** MaxAgeSessionMultiFactor

**Etkiler:** oturum belirteçleri (kalıcı ve kalıcı olmayan)

**Özet:** bir kullanıcının oturum belirteci yeni kimliği ve oturum doğrulandığını başarıyla birden çok faktörünü kullanarak son saatten sonra belirtecini almak için kullanabileceği Bu ilke denetimleri ne kadar. Bir kullanıcının kimliğini doğrular ve yeni oturum belirteci aldıktan sonra kullanıcı oturum belirteci akışı belirtilen süre için kullanabilirsiniz. (Geçerli oturum belirteci iptal ve süresi geçmemiş sürece bu doğrudur.) Belirtilen süre sonra kullanıcı yeni oturum belirteci almak için yeniden kimlik doğrulamaya zorlanır.

Maksimum yaş azaltarak daha sık kimlik doğrulaması yapmalarına zorlar. Tek faktörlü kimlik doğrulaması çok faktörlü kimlik doğrulamasından daha az güvenli olduğu kabul edildiği için bu özellik, eşit veya bundan tek Faktörlü Oturum belirteci Maksimum yaş özelliği büyük bir değere ayarlamanız önerilir.

## <a name="example-token-lifetime-policies"></a>Örnek belirteç ömrü ilkeleri
Birçok senaryoları oluşturabilir ve uygulamaları, hizmet asıl adı ve genel kuruluşunuz için belirteç ömürleri yönetmek zaman Azure AD'de mümkündür. Bu bölümde, yeni kurallar için zorunlu tuttukları yardımcı olacak birkaç ilke senaryoları size rehberlik eder:

* Belirteç ömrü
* Etkin olmayan zaman belirteci sınırı
* Belirteç Maksimum yaş

Örneklerde, öğrenebilir nasıl yapılır:

* Bir kuruluşun varsayılan ilkesini yönetme
* Web oturum açma için bir ilke oluşturun
* Web API'si çağıran yerel bir uygulama için bir ilke oluşturun
* Gelişmiş ilkesini yönetme

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki örneklerde, oluşturmak, güncelleştirmek, bağlantı ve uygulamaları, hizmet asıl adı ve genel kuruluşunuz için ilkelerini silin. Azure AD ile yeni başladıysanız, hakkında bilgi edinin öneririz [Azure AD kiracısı alma](active-directory-howto-tenant.md) bu örnekleri ile devam etmeden önce.  

Başlamak için aşağıdaki adımları uygulayın:

1. En son karşıdan [Azure AD PowerShell modülü genel Önizleme sürümü](https://www.powershellgallery.com/packages/AzureADPreview).
2. Çalıştırma `Connect` komut Azure AD Yönetici hesabınızla oturum açın. Her zaman bu komutu çalıştırmak yeni bir oturum başlatın.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. Kuruluşunuzda oluşturulan tüm ilkeleri görmek için aşağıdaki komutu çalıştırın. Aşağıdaki senaryolarda çoğu işlemlerinden sonra bu komutu çalıştırın. Komutunu çalıştırarak da yardımcı olur size ** **, ilkelerinizin.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Örnek: bir kuruluşun varsayılan ilkesini yönetme
Bu örnekte, tüm kuruluş genelinde daha az sıklıkta oturum açın, kullanıcılarınızın sağlayan bir ilkesi oluşturun. Bunu yapmak için tek Faktörlü Yenile kuruluşunuz genelinde uygulanan belirteçleri, bir belirteç ömrü ilkesi oluşturun. Kuruluşunuzdaki her uygulama ve bir ilke kümesi zaten yok her hizmet sorumlusu ilkesi uygulanır.

1. Bir belirteç ömrü ilkesi oluşturun.

    1.  Küme için tek Faktörlü yenileme belirtecini "kadar iptal edilen." Erişimi iptal kadar belirtecin süresi sona ermiyor. Aşağıdaki ilke tanımı oluşturun:

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  Bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  Yeni ilke görmek için ve ilkenin almak için **objectID**, aşağıdaki komutu çalıştırın:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. İlke güncelleştirin.

    Bu örnekte, ayarladığınız ilk ilke hizmetiniz için gerekli olarak katı değil karar verebilirsiniz. Tek Faktörlü yenileme süresi iki gün içinde dolacak belirteci ayarlamak için aşağıdaki komutu çalıştırın:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Örnek: web oturum açma için bir ilke oluşturun

Bu örnekte, kullanıcılar web uygulamanızı daha sık kimlik doğrulaması gerektiren bir ilke oluşturun. Bu ilke, web uygulamanızın hizmet sorumlusu erişim/kimlik belirteçlerini ömrü ve çok faktörlü Oturum belirteci Maksimum yaş ayarlar.

1. Bir belirteç ömrü ilkesi oluşturun.

    Bu ilke için web oturum açma, erişim/kimlik belirteç ömrü ve max tek Faktörlü Oturum belirteci yaş iki saate ayarlar.

    1.  Bir ilke oluşturmak için bu komutu çalıştırın:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Yeni ilke görmek ve ilkeyi almak üzere **objectID**, aşağıdaki komutu çalıştırın:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  İlke, hizmet sorumlusu atayın. Almanız gereken **objectID** hizmet asıl. 

    1.  Kuruluşunuzun tüm hizmet asıl adı görmek için ya da sorgulayabilirsiniz [Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/serviceprincipal#properties) veya [Azure AD grafik](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Bu test ayrıca [Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net/)ve [Microsoft Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) Azure AD hesabınızı kullanarak.

    2.  Olduğunda **objectID** aşağıdaki komutu çalıştırın, hizmet asıl:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Örnek: web API'si çağıran yerel bir uygulama için bir ilke oluşturun
Bu örnekte, daha az sıklıkta kimlik doğrulaması gerektiren bir ilke oluşturun. İlke, aynı zamanda kullanıcının sağlamalarını gerekir önce bir kullanıcının etkin olmayan kalabileceği süreyi uzatır. Web API ilkesi uygulanır. Yerel uygulama web API'si bir kaynak olarak istediğinde, bu ilke uygulanır.

1. Bir belirteç ömrü ilkesi oluşturun.

    1.  Web API'si için kesin bir ilke oluşturmak için aşağıdaki komutu çalıştırın:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Yeni ilke görmek ve ilkeyi almak üzere **objectID**, aşağıdaki komutu çalıştırın:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. İlke web API'nize atayın. Almanız gereken **objectID** uygulamanızın. Uygulamanızın bulmak için en iyi yolu **objectID** kullanmaktır [Azure portal](https://portal.azure.com/).

   Olduğunda **objectID** , uygulamanızın, aşağıdaki komutu çalıştırın:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of the Application> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Örnek: bir Gelişmiş ilkesini yönetme
Bu örnekte, öncelik sistem nasıl çalıştığını öğrenmek için birkaç ilkeleri oluşturun. Ayrıca, bazı nesnelere uygulanan birden çok ilkelerini yönetmek nasıl öğrenebilirsiniz.

1. Bir belirteç ömrü ilkesi oluşturun.

    1.  30 gün için tek Faktörlü Yenile belirteç ömrü ayarlar bir kuruluş varsayılan ilkeyi oluşturmak için aşağıdaki komutu çalıştırın:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  Yeni ilke görmek için ve ilkenin almak için **objectID**, aşağıdaki komutu çalıştırın:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. İlke için bir hizmet sorumlusu atayın.

    Artık, tüm kuruluş genelinde geçerli bir ilkesi var. Bu 30 günlük ilkesi için bir özel hizmet sorumlusu korumak, ancak "kadar iptal edilen." sayısı üst sınırı için kuruluşunuzun varsayılan ilkesini değiştirmek isteyebilirsiniz

    1.  Kuruluşunuzun tüm hizmet asıl adı görmek için ya da sorgulayabilirsiniz [Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/serviceprincipal#properties) veya [Azure AD grafik](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Bu test ayrıca [Azure AD Graph Explorer'a](https://graphexplorer.cloudapp.net/)ve [Microsoft Graph Explorer'a](https://developer.microsoft.com/graph/graph-explorer) Azure AD hesabınızı kullanarak.

    2.  Olduğunda **objectID** aşağıdaki komutu çalıştırın, hizmet asıl:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
            ```
        
3. Ayarlama `IsOrganizationDefault` bayrağı false olarak ayarlayın:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Yeni bir kuruluş varsayılan ilke oluşturun:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Şimdi, hizmet sorumlusuna bağlı özgün ilkesine sahip ve yeni ilke, kuruluş varsayılan ilke olarak ayarlanır. Hizmet sorumluları uygulanan ilkeler kuruluş varsayılan ilke önceliğe sahip unutmamak önemlidir.

## <a name="cmdlet-reference"></a>Cmdlet başvurusu

### <a name="manage-policies"></a>İlkeleri yönetme

Aşağıdaki cmdlet ilkelerini yönetmek için kullanabilirsiniz.

#### <a name="new-azureadpolicy"></a>New-AzureADPolicy

Yeni bir ilke oluşturur.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Tüm ilkesinin kuralları içeren stringified JSON dizisi. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |TRUE ise, ilke kuruluşunuzun varsayılan ilke olarak ayarlar. False ise, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |İlke türü. Belirteç yaşam süreleri her zaman "TokenLifetimePolicy." kullan | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Tüm Azure AD ilkeleri veya belirtilen ilkesini alır.

```PowerShell
Get-AzureADPolicy
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> [İsteğe bağlı] |**ObjectID (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Tüm uygulamaları ve bir ilke bağlantılı hizmet asıl adı alır.

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Mevcut bir ilkenin güncelleştirir.

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** istediğiniz ilke. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |İlke adı dizesi. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code> [İsteğe bağlı] |Tüm ilkesinin kuralları içeren stringified JSON dizisi. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code> [İsteğe bağlı] |TRUE ise, ilke kuruluşunuzun varsayılan ilke olarak ayarlar. False ise, hiçbir şey yapmaz. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> [İsteğe bağlı] |İlke türü. Belirteç yaşam süreleri her zaman "TokenLifetimePolicy." kullan |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [İsteğe bağlı] |İlke için alternatif bir kimlik ayarlar. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Remove-AzureADPolicy
Belirtilen ilke siler.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** istediğiniz ilke. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Uygulama ilkeleri
Uygulama ilkeleri için aşağıdaki cmdlet'leri kullanabilirsiniz.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Ekleme AzureADApplicationPolicy
Belirtilen ilke uygulama bağlar.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectID** ilkesi. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Bir uygulamaya atanan ilkesini alır.

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Remove-AzureADApplicationPolicy
Bir ilke bir uygulamadan kaldırır.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectID** ilkesi. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Hizmet sorumlusu ilkeleri
Aşağıdaki cmdlet'leri için hizmet asıl ilkeleri kullanabilirsiniz.

#### <a name="add-azureadserviceprincipalpolicy"></a>Ekleme AzureADServicePrincipalPolicy
Belirtilen ilke için bir hizmet sorumlusu bağlar.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectID** ilkesi. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Belirtilen hizmet sorumlusuna bağlı herhangi bir ilke alır.

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Remove-AzureADServicePrincipalPolicy
İlke asıl belirtilen hizmetinden kaldırır.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parametreler | Açıklama | Örnek |
| --- | --- | --- |
| <code>&#8209;Id</code> |**ObjectID (ID)** uygulamanın. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectID** ilkesi. | `-PolicyId <ObjectId of Policy>` |
