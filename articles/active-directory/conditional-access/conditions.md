---
title: Azure Active Directory koşullu erişim koşulları nelerdir? | Microsoft Docs
description: Koşul bir ilkeyi tetiklemek için Azure Active Directory koşullu erişim nasıl kullanıldığı hakkında bilgi edinin.
services: active-directory
keywords: uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 5f5e2051f9c67fa4e37ce0e1213e14e197222f05
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627551"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim koşulları nelerdir? 

Kullanarak, bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz [Azure Active Directory (Azure AD) koşullu erişim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal). Bir koşullu erişim ilkesinde ilkenizi tetikleme nedeni yanıtı tanımlayın. Örnek yanıt **Bunu yapmak**. Bir örnek nedeni **bu durumda**.

![Neden ve yanıt](./media/conditions/10.png)


Koşullu erişim bağlamında **bu durumda** çağrılır bir **koşul**. **Bunu yapmak** çağrılır bir **erişim denetimi**. Koşullarınızda ve erişim denetimleri bir koşullu erişim ilkesi temsil eder.

![Koşullu erişim ilkesi](./media/conditions/61.png)


Koşullu erişim ilkesinde yapılandırmadıysanız koşullar uygulanmaz. Bazı koşullar [zorunlu](best-practices.md) bir ortama bir koşullu erişim ilkesini uygulamak için. 

Bu makalede, koşulları ve bunların bir koşullu erişim ilkesini nasıl kullanıldığı bir genel bakıştır. 

## <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcılar ve gruplar koşul, bir koşullu erişim ilkesinde zorunludur. İlkenizde, yi yapabilecekleriniz **tüm kullanıcılar** veya belirli kullanıcılar ve Gruplar'ı seçin.

![Kullanıcılar ve gruplar](./media/conditions/111.png)

Seçtiğinizde, **tüm kullanıcılar**, ilkenizi dizinde Konuk kullanıcılar dahil olmak üzere tüm kullanıcılara uygulanır.

Olduğunda, **kullanıcıları ve grupları seçin**, aşağıdaki seçenekleri belirleyebilirsiniz:

* **Tüm Konuk kullanıcılar** B2B Konuk kullanıcılar için bir ilke hedefler. Sahip herhangi bir kullanıcı hesabı bu koşulu ile eşleşene **userType** özniteliğini **Konuk**. Hesap, Azure AD'de bir davet akışı oluşturulduktan hemen sonra uygulanacak ilke ihtiyacı olduğunda bu ayarı kullanabilirsiniz.

* **Dizin rolleri** kullanıcının rol atamaya bağlı bir ilke hedefler. Dizin rolleri gibi bu koşulu destekler **genel yönetici** veya **parola Yöneticisi**.

* **Kullanıcılar ve gruplar** belirli kullanıcıların kümesini hedefler. Örneğin, bulut uygulaması ik uygulama seçildiğinde tüm çözümler. ik departmanı üyeleri içeren bir grup seçebilirsiniz. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere, Azure AD'de Grup herhangi bir türde olabilir.

Belirli kullanıcılar veya gruplar bir ilkeden dışlayabilirsiniz. Bir ortak kullanım örneği hizmet hesapları ise ilkenizde, çok faktörlü kimlik doğrulaması (MFA) zorunlu kılar. 

Kullanıcılar belirli kümelerini hedefleyen yeni bir ilke dağıtımı için kullanışlıdır. Yeni bir ilke yalnızca ilk kümesi ilkesi davranışını doğrulamak için kullanıcı hedeflemelidir. 



## <a name="cloud-apps"></a>Bulut uygulamaları 

Bulut uygulaması, bir Web sitesi veya hizmet örneğidir. Azure AD uygulama ara sunucusu tarafından korunan Web siteleri, bulut uygulamaları da taşır. Desteklenen bulut uygulamalarının ayrıntılı bir açıklaması için bkz. [bulut uygulamaları atamaları](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference#cloud-apps-assignments). 

**Bulut uygulamaları** koşulu bir koşullu erişim ilkesinde zorunludur. İlkenizde, yi yapabilecekleriniz **tüm bulut uygulamaları** veya belirli uygulamalar seçin.

![Bulut uygulamaları içerir](./media/conditions/03.png)

- Seçin **tüm bulut uygulamaları** kuruluş genelinde uygulamak için temel ilkeleri. Oturum açma riski algılandığında herhangi bir bulut uygulamasında için çok faktörlü kimlik doğrulaması gerektiren ilkeleri için bu seçimi kullanın. Uygulanan bir ilke **tüm bulut uygulamaları** erişim için geçerli tüm Web siteleri ve Hizmetleri. Bu ayar görünen bulut uygulamalarına sınırlı değildir **uygulamaları Seç** listesi. 

- İlke tarafından hedef belirli hizmetler için tek bulut uygulamaları seçin. Örneğin, kullanıcıların gerektirebilir bir [uyumlu cihaz](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) SharePoint Online'a erişmek için. SharePoint içeriği eriştiklerinde Bu ilke, diğer hizmetlere de uygulanır. Microsoft Teams buna bir örnektir. 

Bir ilkenin belirli uygulamaları hariç tutabilirsiniz. Ancak, bu uygulamalar yine de erişim hizmetleri için uygulanan ilkelerle tabidir. 



## <a name="sign-in-risk"></a>Oturum açma riski

Oturum açma riski yüksek, Orta veya düşük bir oturum açma girişimi bir kullanıcı hesabının meşru sahibi tarafından yapılan değildi olasılığını bir göstergesidir. Azure AD oturum açma sırasında bir kullanıcı oturum açma risk düzeyini hesaplar. Hesaplanan oturum açma risk düzeyini bir koşullu Erişim İlkesi'nde bir koşul olabilir. 

![Oturum açma risk düzeyleri](./media/conditions/22.png)

Bu koşulu kullanmak için ihtiyacınız [Azure Active Directory kimlik koruması](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection-enable) etkin.
 
Bu koşul için yaygın kullanım örnekleri aşağıdaki korumalar ilkelerdir: 

- Bir yüksek oturum açma riski kullanıcılarla engelleyin. Bu koruma potansiyel olarak yasal olmayan kullanıcılar, bulut uygulamalarınıza erişmesini engeller. 
- Orta ölçekli bir oturum açma riski olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir. Çok faktörlü kimlik doğrulamasını zorunlu tutarak, oturum açmanın meşru bir hesap sahibi tarafından yapılan ek güvence sağlayabilir.

Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-security-risky-sign-ins).  

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Azure AD kullanıcı aracısı gibi cihaz tarafından sağlanan bilgileri kullanarak platform tanımlar. Bu bilgiler, doğrulanmamış. Tüm platformlar bir ilke uygulanmış olmasını öneririz. İlke ya da erişimi engellemek, Intune ilkelerle uyumluluğu gerektiren veya cihaz etki alanına katılmış olması gerekir. Tüm cihaz platformları için bir ilkeyi uygulayabilmeniz için varsayılandır. 


![Cihaz platformlarını yapılandırın](./media/conditions/24.png)

Desteklenen cihaz platformları listesi için bkz. [cihaz platformu koşul](technical-reference.md#device-platform-condition).


Bu durum, bulut uygulamalarınıza erişimi kısıtlayan bir ilke için bir ortak kullanım örneği [yönetilen cihazlar](require-managed-devices.md). Cihaz platformu koşulu dahil olmak üzere daha fazla senaryo için bkz. [Azure Active Directory uygulama tabanlı koşullu erişim](app-based-conditional-access.md).



## <a name="device-state"></a>Cihaz durumu

Cihaz durumu koşulunu ve koşullu erişim ilkesi uyumsuz olarak işaretlenmiş aygıtlar Azure AD'ye katılmış karma dışlar. Bu durum, ek bir oturum güvenliği sağlamak için yalnızca bir yönetilmeyen cihaza bir ilkenin uygulanacağı yararlıdır. Örneğin, bir cihazın yönetilmeyen olduğunda yalnızca Microsoft Cloud App Security oturum denetimi uygular. 


![Cihaz durumlarını yapılandırın](./media/conditions/112.png)

Yönetilmeyen cihazların erişimini engellemek istiyorsanız, uygulama [cihaz tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).


## <a name="locations"></a>Konumlar

Konumları kullanarak bağlantı burada denendi göre koşullar tanımlayabilirsiniz. 

![Konumları yapılandırın](./media/conditions/25.png)

Bu koşul için yaygın kullanım örnekleri aşağıdaki koruma ilkeleriyle şunlardır:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.  

- Bir hizmetin belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelleyin. 

Daha fazla bilgi için [konum koşulu Azure Active Directory koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>İstemci uygulamaları

İstemci uygulamaları koşulu kullanarak, farklı uygulama türleri için bir ilke uygulayabilirsiniz. Web siteleri, hizmetleri, mobil uygulamalar ve Masaüstü uygulamaları verilebilir. 



Bir uygulamayı şu şekilde sınıflandırılır:

- Bir Web sitesi veya web SSO protokolleri, SAML, kullanıyorsa, hizmet WS-Federasyon veya Openıd Connect gizli bir istemci için.

- Bir mobil uygulama veya yerel bir istemci için Openıd Connect mobil uygulamayı kullanıyorsa, masaüstü uygulaması.

Koşullu erişim ilkenizi kullanabileceğiniz istemci uygulamalar listesi için bkz. [istemci uygulamalar koşulunu](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#client-apps-condition) Azure Active Directory koşullu erişim teknik başvuru.

Bu koşul için yaygın kullanım örnekleri aşağıdaki koruma ilkeleriyle şunlardır: 

- Gerekli bir [uyumlu cihaz](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) büyük miktarlarda verinin cihaza indirilmesine mobil ve Masaüstü uygulamaları için. Aynı zamanda, tüm cihazlardan tarayıcı erişimi izin verin.

- Web uygulamalarından erişimi engellemek, ancak Mobil ve Masaüstü uygulamalardan erişime izin ver

Bu durum web SSO ve modern kimlik doğrulama protokolleri uygulayabilirsiniz. Bunu kullanan Microsoft Exchange ActiveSync posta uygulamaları için de uygulayabilirsiniz. Çoğu akıllı telefonlar yerel posta uygulamaları verilebilir. 

Microsoft Office 365 Exchange Online, seçtiğiniz tek bulut uygulaması ise, yalnızca istemci uygulamaları koşul seçebilirsiniz.

![Bulut uygulamaları](./media/conditions/32.png)

Seçme **Exchange ActiveSync** yalnızca bir ilkede yapılandırılabilecek diğer koşullar yoksa, bir istemci olarak uygulamalar koşulunu desteklenir. Ancak, bu koşul yalnızca desteklenen platformlara Uygula kapsamını daraltabilirsiniz.

 
![İlkeyi yalnızca desteklenen platformlara uygula](./media/conditions/33.png)

Bu koşul yalnızca desteklenen platformlara uygulayarak tüm cihaz platformlarını eşit bir [cihaz platformu koşul](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online).

![Cihaz platformlarını yapılandırın](./media/conditions/34.png)


 Daha fazla bilgi için şu makalelere bakın:

- [SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişim ayarlama](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
 
- [Azure Active Directory uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam). 


### <a name="legacy-authentication"></a>Eski bir kimlik doğrulama  

Koşullu erişim artık modern kimlik doğrulamayı desteklemeyen eski Microsoft Office istemciler için geçerlidir. SMTP POP ve IMAP gibi e-posta protokolleri kullanan istemciler için de geçerlidir. Eski bir kimlik doğrulama kullanarak gibi ilkeleri yapılandırabilirsiniz **diğer istemcilerden erişimi engelle**.


![İstemci uygulamalarını yapılandırın](./media/conditions/160.png)  


#### <a name="known-issues"></a>Bilinen sorunlar

- Bir ilke için yapılandırma **diğer istemciler** SPConnect gibi belirli istemcilerden gelen tüm kuruluş engeller. Bunun nedeni, eski istemciler, beklenmedik bir şekilde kimlik doğrulaması bu blok kullanmasıdır. Sorun, eski Office istemcileri gibi önemli Office uygulamaları için geçerli değildir. 

- Bu ilkenin yürürlüğe 24 saate kadar sürebilir. 


#### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S:** Microsoft Exchange Web Hizmetleri bu kimlik doğrulamasını engeller?

Bu, Exchange Web hizmetleri kullanan kimlik doğrulama protokolü bağlıdır. Exchange Web Hizmetleri uygulaması modern kimlik doğrulaması kullanıyorsa, tarafından kapsamındadır **mobil uygulamalar ve masaüstü istemciler** istemci uygulaması. Temel kimlik doğrulaması tarafından kapsanan **diğer istemciler** istemci uygulaması.


**S:** hangi denetimleri için kullanabilirim **diğer istemciler**?

Herhangi bir denetim için yapılandırılabilir **diğer istemciler**. Bununla birlikte, son kullanıcı deneyimi olacak tüm durumlarda erişimi engellenir. **Diğer istemciler** MFA ve uyumlu bir cihaz etki alanına katılım gibi denetimlerini desteklemez. 
 
**S:** hangi koşullar için kullanabileceğim **diğer istemciler**?

Herhangi bir koşul için yapılandırılabilir **diğer istemciler**.

**S:** mu Exchange ActiveSync desteği tüm koşullarını ve denetimlerini?

Hayır. Aşağıdaki listede, Exchange ActiveSync desteği özetlenmektedir: 

- Exchange ActiveSync yalnızca kullanıcı ve Grup hedefleme destekler. Konuklar veya rollerini desteklemez. Bir konuk ya da rolü koşul yapılandırılmışsa, tüm kullanıcıların engellenir. Exchange ActiveSync, ilkenin kullanıcıya veya uygulanacaksa belirleyemediğinden tüm kullanıcılar engeller.

- Exchange ActiveSync yalnızca Microsoft Exchange Online ile bulut uygulaması çalışır. 

- Exchange ActiveSync istemci uygulaması dışında herhangi bir koşul desteklemiyor. 

- Exchange ActiveSync ile herhangi bir denetimi yapılandırılabilir. Cihaz uyumluluğu dışındaki tüm denetimler için bir blok sağlama.

**S:** ilkelerinin ileriye dönük varsayılan olarak tüm istemci uygulamaları için geçerli mi?

Hayır. Varsayılan ilke davranışında değişiklik yoktur. İlkeler, tarayıcı ve mobil uygulamalar ve masaüstü istemciler varsayılan olarak uygulanmaya devam eder.



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma konusunda bilgi edinmek için bkz: [hızlı başlangıç: Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

- Ortamınız için koşullu erişim ilkeleri yapılandırmak için bkz: [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 

