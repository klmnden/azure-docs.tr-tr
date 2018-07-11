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
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 42792170593dbd94d0eae9b408c70f326891508a
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "36232571"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim koşulları nelerdir? 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesinde ("Bu durumda") ilkeniz tetikleme nedeni için ("bunu") yanıtı tanımlayın. 

![Denetim](./media/active-directory-conditional-access-conditions/10.png)


Koşullu erişim bağlamında:

- "**Bu durumda**" olarak adlandırılır **koşullar**. 
- "**Bunu yapmak**" olarak adlandırılır **erişim denetimleri**.

Koşullarınızda erişim denetimleri ile koşullu erişim ilkesi temsil eder.

![Denetim](./media/active-directory-conditional-access-conditions/61.png)


Koşullu erişim ilkesinde yapılandırılmış olmaması koşullar uygulanmaz. Bazı koşullar [zorunlu](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) bir ortama bir koşullu erişim ilkesini uygulamak için. 

Bu makalede, koşullar ve bir koşullu erişim ilkesini nasıl kullanıldığı hakkında genel bir bakış sağlar. 

## <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcılar ve gruplar koşul, bir koşullu erişim ilkesinde zorunludur. İlkenizde, yi yapabilecekleriniz **tüm kullanıcılar** veya belirli kullanıcılar ve Gruplar'ı seçin.

![Denetim](./media/active-directory-conditional-access-conditions/111.png)

Seçtiğinizde:

- **Tüm kullanıcılar**, ilkenizi dizinindeki tüm kullanıcılara uygulanır. Bu, Konuk kullanıcıları içerir.

- **Kullanıcıları ve grupları seçme**, aşağıdaki seçenekleri belirleyebilirsiniz:

    - **Tüm Konuk kullanıcılar** -B2B konuk kullanıcıların bir ilkesinin hedefleyeceği sağlar. Bu koşul herhangi bir kullanıcı hesabı ile eşleşen *userType* özniteliğini *Konuk*. Bu ayar, bir ilke hesap, Azure AD'de bir davet akışı oluşturulduktan hemen sonra uygulanması gereken yere durumlarda kullanabilirsiniz.

    - **Dizin rolleri** -kullanıcının rol atamaya bağlı bir ilkesinin hedefleyeceği sağlar. Bu durum gibi dizin rolünü destekler *genel yönetici* veya *parola Yöneticisi*.

    - **Kullanıcılar ve gruplar** -hedef belirli kullanıcı kümelerine sağlar. Örneğin, bulut uygulaması seçilen bir ik uygulaması varsa, tüm çözümler. ik departmanı üyeleri içeren bir grup seçebilirsiniz.

Bir grup, herhangi bir grup türünü Azure AD'de dinamik veya atanan güvenlik ve dağıtım grupları dahil olabilir

Belirli kullanıcılar veya gruplar bir ilkeden dışlayabilirsiniz. Bir ortak kullanım örneği olup hizmet hesapları, ilke multi factor authentication (MFA) zorunlu kılar. 

Kullanıcılar belirli kümelerini hedefleyen yeni bir ilke dağıtımı için kullanışlıdır. Yeni bir ilke yalnızca ilk kümesi ilkesi davranışını doğrulamak için kullanıcı hedeflemelidir. 



## <a name="cloud-apps"></a>Bulut uygulamaları 

Bir web siteleri bulut uygulaması, ya da hizmet. Bu, Azure uygulama ara sunucusu tarafından korunan web siteleri içerir. Desteklenen bulut uygulamalarının ayrıntılı bir açıklaması için bkz [bulut uygulamaları atama](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Bulut uygulamaları koşul, bir koşullu erişim ilkesinde zorunludur. İlkenizde, yi yapabilecekleriniz **tüm bulut uygulamaları** veya belirli uygulamalar seçin.

![Denetim](./media/active-directory-conditional-access-conditions/03.png)

Şunları seçebilirsiniz:

- **Tüm bulut uygulamaları** temel ilkeleri, kuruluş genelinde uygulanacak. Bu seçim için yaygın bir kullanım örneği oturum açma riski algılandığında herhangi bir bulut uygulamasında için çok faktörlü kimlik doğrulaması gerektiren bir ilkedir. Uygulanan bir ilke **tüm bulut uygulamaları** erişim için geçerli tüm web sitesi ve Hizmetleri. Bu ayar görünen bulut uygulamalarına sınırlı değildir **seçin bulut uygulamaları** listesi.

- İlkesi tarafından hedef belirli hizmetler için tek bulut uygulamaları. Örneğin, kullanıcıların gerektirebilir bir [uyumlu cihaz](active-directory-conditional-access-policy-connected-applications.md) SharePoint Online'a erişmek için. SharePoint içeriği, örneğin, Microsoft Teams eriştiklerinde Bu ilke, diğer hizmetlere de uygulanır. 

Belirli uygulamalar İlkesi'nden de hariç tutabilirsiniz; Ancak, bu uygulamalar yine de erişim hizmetleri için uygulanan ilkelerle tabidir. 



## <a name="sign-in-risk"></a>Oturum açma riski

Oturum açma riski (yüksek, Orta veya düşük) bir oturum açma girişimi bir kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmedi olasılığını göstergesidir. Azure AD oturum açma sırasında bir kullanıcının oturum açma risk düzeyini hesaplar. Koşullu erişim ilkesi koşulu olarak hesaplanan oturum açma risk düzeyini kullanabilirsiniz. 

![Koşullar](./media/active-directory-conditional-access-conditions/22.png)

Bu koşulu kullanmak için ihtiyacınız [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) etkin.
 
Bu koşul için ortak kullanım durumları olan ilkeler:

- Potansiyel olarak yasal olmayan kullanıcılar, bulut uygulamalarınıza erişimini engellemek için bir yüksek oturum açma riski kullanıcılarla engelleyin. 
- Orta ölçekli bir oturum açma riski olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir. Çok faktörlü kimlik doğrulaması zorunlu tutarak, oturum açmanın meşru bir hesap sahibi tarafından gerçekleştirildiğini ek güvence sağlayabilir.

Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Azure AD kullanıcı aracısı gibi cihaz tarafından sağlanan bilgileri kullanarak platform tanımlar. Bu bilgiler doğrulanmamış olduğundan, tüm platformlar bunları, erişimi engelleme, Intune ilkeleriyle uyumluluğunu gerektiren veya cihaz etki alanına katılmış olmasını gerektiren tarafından uygulanan bir ilke olması önerilir. Tüm cihaz platformları için ilkeyi uygulamak için varsayılandır. 


![Koşullar](./media/active-directory-conditional-access-conditions/24.png)

Desteklenen cihaz platformlarını tam bir listesi için bkz. [cihaz platformu koşul](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Bu durum, bulut uygulamalarınıza erişimi kısıtlayan bir ilke için bir ortak kullanım örneği [yönetilen cihazlar](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Cihaz platformu koşulu dahil olmak üzere daha fazla senaryo için bkz. [Azure Active Directory uygulama tabanlı koşullu erişim](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Cihaz durumu

Cihaz durumu koşulunu hibrit Azure AD'ye katıldı ve cihaz bir koşullu erişim ilkesinden hariç tutulacak uyumlu olarak işaretli sağlar. Bir ilke yalnızca ek oturum güvenliği sağlamak için yönetilmeyen cihaza uygulanmasını gerektiren bu kullanışlıdır. Örneğin, bir cihazın yönetilmeyen olduğunda yalnızca Microsoft Cloud App Security oturum denetimi uygular. 


![Koşullar](./media/active-directory-conditional-access-conditions/112.png)

Yönetilmeyen cihazların erişimini engellemek istiyorsanız, uygulamalıdır [cihaz tabanlı koşullu erişim](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Konumlar

Konumlar ile bir bağlantı denemesi gelen başlatıldığı üzerinde temel koşulları tanımlamak için seçeneğiniz vardır. 
     
![Koşullar](./media/active-directory-conditional-access-conditions/25.png)

Bu koşul için ortak kullanım durumları olan ilkeler:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.  

- Bir hizmetin belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelleyin. 

Daha fazla bilgi için [konum koşulu Azure Active Directory koşullu erişim nedir?](active-directory-conditional-access-locations.md)


## <a name="client-apps"></a>İstemci uygulamaları

İstemci uygulamaları durumu gibi farklı uygulama türleri için bir ilkenin uygulanacağı sağlar:

- Web siteleri ve Hizmetleri
- Mobil uygulamalar ve Masaüstü uygulamaları. 



Bir uygulama olarak sınıflandırılır:

- Web sitesi veya web SSO protokolleri, SAML, kullanıyorsa, hizmet WS-Federasyon veya Openıd Connect gizli bir istemci için.

- Bir mobil uygulaması ya da yerel bir istemci için Openıd Connect mobil uygulamayı kullanıyorsa, masaüstü uygulaması.

Koşullu erişim ilkenizi kullanabileceğiniz istemci uygulamaları tam bir listesi için bkz. [istemci uygulamalar koşulunu](active-directory-conditional-access-technical-reference.md#client-apps-condition) Azure Active Directory koşullu erişim teknik başvuru.

Bu koşul için ortak kullanım durumları olan ilkeler:

- Gerekli bir [uyumlu cihaz](active-directory-conditional-access-policy-connected-applications.md) tarayıcı erişimini herhangi bir CİHAZDAN verirken cihaza, büyük miktarlarda veri indirme mobil ve Masaüstü uygulamaları için.

- Web uygulamalarından erişimi engellemek, ancak Mobil ve Masaüstü uygulamalar erişime izin verecek.

Web SSO ve modern kimlik doğrulama protokollerini kullanarak ek olarak, yerel posta uygulamaları çoğu akıllı telefonlar gibi kullanan Exchange ActiveSync posta uygulamaları için bu durum da uygulayabilirsiniz. Şu anda, eski protokolleri kullanarak istemci uygulamaları AD FS kullanarak korunması gerekir.

Bu durum, yalnızca seçebilirsiniz **Office 365 Exchange Online** seçtiğiniz tek bulut uygulamasıdır.

![Bulut uygulamaları](./media/active-directory-conditional-access-conditions/32.png)

Seçme **Exchange ActiveSync** diğer koşullar yapılandırılmış ilke yoksa, istemci uygulamaları olarak koşulu yalnızca desteklenir. Ancak, bu koşul yalnızca desteklenen platformlara Uygula kapsamını daraltabilirsiniz.

 
![Desteklenen platformlar](./media/active-directory-conditional-access-conditions/33.png)

Bu koşul yalnızca desteklenen platformlara uygulama tüm cihaz platformları için eşdeğer olan bir [cihaz platformu koşul](active-directory-conditional-access-conditions.md#device-platforms).

![Desteklenen platformlar](./media/active-directory-conditional-access-conditions/34.png)


 Daha fazla bilgi için bkz.

- [SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişim ayarlama](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory uygulama tabanlı koşullu erişim](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Eski bir kimlik doğrulama  

Koşullu erişim artık modern kimlik doğrulaması desteği olmayan eski Office istemcileri ve bunun yanı sıra, POP, IMAP, SMTP vb. gibi posta protokollerini kullanan istemciler için geçerlidir. Bu sayede gibi ilkeleri yapılandırmak **diğer istemcilerden erişimi engelle**.


![Eski bir kimlik doğrulama](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Bilinen sorunlar

- Bir ilke için yapılandırma **diğer istemciler** SPConnect gibi belirli istemcilerden gelen tüm kuruluş engeller. Bu, beklenmedik bir şekilde kimlik doğrulaması bu eski istemciler kaynaklanır. Bu sorun, eski Office istemcileri gibi Office uygulamaları büyük geçerli değildir. 

- Bu ilkenin etkili olması için 24 saate kadar sürebilir. 


#### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Bu, Exchange Web Hizmetleri (EWS) bloke edilir?**

Bu, EWS kullanan kimlik doğrulama protokolü bağlıdır. EWS uygulaması modern kimlik doğrulaması kullanıyorsa, "mobil uygulamalar ve masaüstü istemciler" istemci uygulama tarafından ele alınmaktadır. EWS uygulama temel kimlik doğrulaması kullanıyorsa, "Diğer istemciler" istemci uygulama tarafından ele alınmaktadır.


**Diğer istemciler için hangi denetimlerin kullanabilirim**

"Diğer istemciler için" herhangi bir denetimi yapılandırılabilir. Bununla birlikte, son kullanıcı deneyimi erişimi engellemek için her durumda olacaktır. "Diğer istemciler" uyumlu cihaz, etki alanına katılım, vb. gibi MFA'yı denetimleri desteklemez. 
 
**Hangi durumlarda diğer istemciler için kullanabilir miyim?**

"Diğer istemciler için" koşulları yapılandırılabilir.

**Exchange ActiveSync tüm koşullarını ve denetimlerini destekliyor mu?**

Hayır. Exchange ActiveSync (EAS) destek özeti aşağıda verilmiştir:

- EAS yalnızca kullanıcı ve Grup hedefleyen destekler. Bunu konuk, rolleri desteklemiyor. Biz ilke kullanıcıya veya uygulanacaksa belirleyemediğinden Konuk/rol koşul yapılandırılmışsa, tüm kullanıcılar engellenmiş.

- EAS, Exchange ile yalnızca bulut uygulaması çalışır. 

- EAS istemci uygulaması kendisi dışında herhangi bir koşul desteklemez.

- EAS (cihaz uyumluluğu hariç tüm bloğu götürür) herhangi bir denetime yapılandırılabilir.

**İlkeler tüm istemci uygulamaları için varsayılan değer olarak ileriye dönük uygulanır mı?**

Hayır. Varsayılan ilke davranışında değişiklik yoktur. İlkeler, tarayıcı ve mobil uygulamalarını/masaüstü istemcisi için varsayılan olarak uygulanmaya devam eder.



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](active-directory-conditional-access-app-based-mfa.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](active-directory-conditional-access-best-practices.md). 

