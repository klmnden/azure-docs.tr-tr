---
title: Azure AD'de izinler | Microsoft Docs
description: Azure Active Directory'deki kapsamları, izinleri ve bunların kullanımını öğrenin
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 6c0dc122-2cd8-4d70-be5a-3943459d308e
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/20/2017
ms.author: celested
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: 749253d6a082bcdc2b80c5984f20c4b8c4039ad0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156900"
---
# <a name="permissions-in-azure-ad"></a>Azure AD'de izinler
Azure Active Directory (Azure AD), hem OAuth hem de OpenID Connect (OIDC) akışlarında izinleri yaygın olarak kullanır. Uygulamanız Azure AD'den bir erişim belirteci aldığında, bu belirteç uygulamanızın belirli bir kaynakla ilgili olarak sahip olduğu izinleri (kapsamlar olarak da bilinir) açıklayan talepler içerir. Bu durum kaynağın yetkilendirmesini kolaylaştırır çünkü yalnızca belirtecinizin çağırdığınız API için uygun izni içerip içermediğini denetlemesi yeterli olur. 

## <a name="types-of-permissions"></a>İzin türleri
Azure AD iki tür izin tanımlar: 
* **Temsilci izinleri** - Oturum açmış kullanıcının olduğu uygulamalar tarafından kullanılır. Bu uygulamalar için, kullanıcı veya yönetici uygulamanın istediği onayları verir ve uygulamaya API çağrıları yaparken oturum açmış kullanıcı adına işlem yapması için temsilci izni verilir. API'yi bağlı olarak, kullanıcı doğrudan API'ye onay veremeyebilir ve bunun yerine [bir yöneticinin "yönetici onayı" sağlaması gerekebilir.](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent)
* **Uygulama izinleri** - Oturum açmış kullanıcı olmadan çalıştırılan uygulamalar tarafından kullanılır; örneğin, arka plan hizmetleri veya daemon programları olarak çalıştırılan uygulamalar böyledir. Uygulama izinleri yalnızca [yönetici tarafından verilebilir](/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant) çünkü normalde bunlar çok güçlü izinlerdir ve kullanıcı sınırlarının ötesindeki verilere veya aslında yöneticilerin kullanımıyla kısıtlanmış olabilecek verilere erişim izin verebilir. 

Etkili izinler, uygulamanızın API istekleri yaparken sahip olacağı izinlerdir. 

* Temsilci izinleri için, uygulamanızın etkili izinleri uygulamaya verilmiş olan (onay yoluyla) temsilci izinleriyle o anda oturum açmış olan kullanıcının ayrıcalıklarının en düşük ayrıcalıklı kesişimi olacaktır. Uygulamanızın ayrıcalıkları hiçbir zaman oturum açmış kullanıcının ayrıcalıklarından fazla olamaz. Kuruluşların içinde, oturum açmış kullanıcının ayrıcalıkları ilkeyle ya da bir veya birden çok yönetici rolü üyeliğiyle belirlenebilir. Yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure AD'de yönetici rolleri atama](/azure/active-directory/active-directory-assign-admin-roles-azure-portal.md).
    Örneğin, Microsoft Graph'te uygulamanıza `User.ReadWrite.All` temsilci izni verildiğini varsayalım. Adından da anlaşıldığı gibi bu izin uygulamanıza kuruluştaki her kullanıcının profilini okuma ve güncelleştirme izni verir. Oturum açmış kullanıcının bir genel yönetici olması durumunda, uygulamanız kuruluştaki her kullanıcının profilini güncelleştirebilir. Öte yandan oturum açmış kullanıcı bir yönetici rolünde değilse, uygulamanız yalnızca oturum açmış kullanıcının profilini güncelleştirebilir. Kuruluştaki diğer kullanıcıların profillerini güncelleştiremez çünkü adına çalışma iznine sahip olduğu kullanıcı söz konusu ayrıcalıklara sahip değildir.
* Uygulama izinleri için, uygulamanızın etkili izinleri izin tarafından belirtilen tüm ayrıcalık düzeylerini kapsar. Örneğin, `User.ReadWrite.All` uygulama iznine sahip bir uygulama kuruluştaki her kullanıcının profilini güncelleştirebilir. 

## <a name="permission-attributes"></a>İzin öznitelikleri
Azure AD'deki izinlerin bir dizi özelliği vardır ve bu özellikler kullanıcıların, yöneticilerin veya uygulama geliştiricilerin iznin ne erişimi verdiği konusunda bilinçli kararlar almasına yardımcı olur. 

> [!NOTE]
> Azure portalını veya PowerShell'i kullanarak Azure AD Uygulaması veya Hizmet Sorumlusunun kullanıma sunduğu izinleri görüntüleyebilirsiniz. Microsoft Graph tarafından kullanıma sunulan izinleri görüntülemek için bu betiği deneyin.
> ```powershell
> Connect-AzureAD
> 
> # Get OAuth2 Permissions/delegated permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").OAuth2Permissions
> 
> # Get App roles/application permissions
> (Get-AzureADServicePrincipal -filter "DisplayName eq 'Microsoft Graph'").AppRoles
> ```

| Özellik adı | Açıklama | Örnek | 
| --- | --- | --- |
| Kimlik | Bu izni benzersiz olarak tanımlayan GUID değeri. | 570282fd-fa5c-430d-a7fd-fc8dc98a9dca | 
| IsEnabled | Bu kapsamın kullanılabilir olup olmadığını gösterir. | true | 
| Tür | Bu iznin kullanıcı onayı veya yönetici onayı gerektirip gerektirmediğini gösterir. | Kullanıcı | 
| AdminConsentDescription | Bu, yönetici onayı deneyimleri sırasında yöneticilere gösterilen açıklamadır | Uygulamanın kullanıcı posta kutularındaki e-postayı okumasına izin verir. | 
| AdminConsentDisplayName | Bu, yönetici onayı deneyimi sırasında yöneticilere gösterilen kolay addır. | Kullanıcı postasını okuma | 
| UserConsentDescription | Bu, kullanıcı onayı deneyimi sırasında kullanıcılara gösterilen açıklamadır. |  Uygulamanın, posta kutunuzdaki e-postalarınızı okumasına izin verir. | 
| UserConsentDisplayName | Bu, kullanıcı onayı deneyimi sırasında kullanıcılara gösterilen kolay addır. | Postalarınızı okuma | 
| Değer | Bu, OAuth 2.0 yetkilendirme akışları sırasında izni tanımlamak için kullanılan dizedir. Tam izin adı oluşturmak için Uygulama Kimliği URI dizesiyle birleştirilebilir. | `Mail.Read` | 

## <a name="types-of-consent"></a>Onay türleri
Azure AD'deki uygulamalar gerekli kaynaklara veya API'lere erişim kazanmak için onaya bağımlıdır. Uygulamanızın başarılı olabilmesi için tanıyor olması gereken çeşitli onay türleri vardır. İzinleri tanımlıyorsanız, kullanıcıların uygulamanıza veya API'nize nasıl erişim kazanacağını da anlamalısınız.

* **Statik kullanıcı onayı** - Uygulamanızın etkileşimli çalışmak istediği kaynağı belirttiğinizde [OAuth 2.0 yetkilendirme akışı](/azure/active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) sırasında otomatik olarak gerçekleşir. Statik kullanıcı onayı senaryosunda, uygulamanızın gereken tüm izinleri Azure portalındaki uygulama yapılandırmasında zaten belirtmiş olması gerekir. Kullanıcı (veya uygun olduğunda yönetici) bu uygulamaya onay vermezse, Azure AD kullanıcının şu anda onay vermesini ister. 

    Statik bir API kümesine erişim isteyen bir Azure AD uygulamasını kaydetme hakkında daha fazla bilgi edinin.
* **Dinamik kullanıcı onayı** - v2 Azure AD uygulama modelinin bir özelliğidir. Bu senaryoda, uygulamanız [v2 uygulamaları için OAuth 2.0 yetkilendirme akışı](/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent) içinde ihtiyacı olan bir dizi kapsam ister. Kullanıcı henüz onaylamadıysa, bu aşamada onaylaması istenir. [Dinamik onay hakkında daha fazla bilgi edinin](/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent).

    > [!NOTE]
    > Dinamik onay kullanışlı olabilir ama yönetici onayı gerektiren izinlerde çok zorluk çıkarabilir çünkü yönetici onayı deneyimi, onay zamanında söz konusu izinleri bilmiyor olacaktır. Ayrıcalıklı yönetici kapsamlarına ihtiyacınız varsa, uygulamanızın bunları Azure Portal'da kaydetmesi gerekir.
  
* **Yönetici onayı** - Uygulamanızın bazı yüksek ayrıcalıklı izinlere erişmeye ihtiyacı olması durumunda gereklidir. Bu, uygulamalara veya kullanıcılara kuruluşunuzun yüksek ayrıcalıklı verilerine erişme yetkisi verilmeden önce yöneticilerin bazı ek denetimler yapabilmesini sağlar. [Yönetici onayı verme hakkında daha fazla bilgi edinin](/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="best-practices"></a>En iyi yöntemler

### <a name="resource-best-practices"></a>Kaynaklarla ilgili en iyi yöntemler
API'leri kullanıma sunan kaynaklar, kesinlikle korudukları verilere veya eylemlere özgü izinler tanımlamalıdır. Bu yöntem, istemcilerin ihtiyaçları olmayan verilere erişmemesini ve kullanıcıların hangi verilere onay verdikleri konusunda bilgi sahibi olmasını sağlamaya yardımcı olur.

Kaynaklar `Read` ve `ReadWrite` izinlerini ayrı ayrı ve açıkça tanımlamalıdır. 

Kaynaklar, kullanıcı sınırlarının ötesinde veri erişimine olanak tanıyan tüm izinleri `Admin` izinleri olarak işaretlemelidir. 

Kaynaklar `Subject.Permission[.Modifier]` adlandırma desenine uymalıdır; burada `Subject` kullanılabilir veri türüne karşılık gelir, `Permission` kullanıcının veriler üzerinde gerçekleştirebileceği eyleme karşılık gelir ve `Modifier` başka bir iznin özelleştirilmiş durumlarını açıklamak için isteğe bağlı olarak kullanılır. Örnek: 
* Mail.Read - Kullanıcıların postayı okumasına izin verir. 
* Mail.ReadWrite - Kullanıcıların postayı yazmasına veya okumasına izin verir.
* Mail.ReadWrite.All - Yöneticinin veya kullanıcının kuruluştaki tüm postaya erişmesine izin verir.

### <a name="client-best-practices"></a>İstemciyle ilgili en iyi yöntemler
Yalnızca uygulamanıza gereken kapsamlar için izin isteyin. Aşırı fazla izinleri olan uygulamaların güvenliği aşıldığında, kullanıcı verilerini açığa çıkarma riski doğar.

İstemciler aynı uygulamadan uygulama izinleri ve temsilci izinleri istememelidir. Bunun sonucunda ayrıcalık yükseltmesi ortaya çıkabilir ve kullanıcının kendi izinlerinin yetmediği verilere erişim kazanması söz konusu olabilir. 




