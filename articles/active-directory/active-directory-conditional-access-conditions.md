---
title: Azure Active Directory koşullu erişim koşulları nelerdir? | Microsoft Docs
description: Koşullar ilke tetiklemek için Azure Active Directory koşullu erişimin nasıl kullanıldığı hakkında bilgi edinin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
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
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232571"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim koşulları nelerdir? 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesi ("bunu") yanıt ("Bu durumda") ilkeniz tetikleme nedenini tanımlayın. 

![Denetim](./media/active-directory-conditional-access-conditions/10.png)


Koşullu erişim bağlamında:

- "**Bu durumda**" olarak adlandırılır **koşullar**. 
- "**Sonra bunu**" olarak adlandırılır **erişim denetimleri**.

Koşullarınızı erişim denetimleri ile birlikte bir koşullu erişim ilkesi temsil eder.

![Denetim](./media/active-directory-conditional-access-conditions/61.png)


Bir koşullu erişim ilkesinde yapılandırılmamış koşullar uygulanmaz. Bazı koşullar [zorunlu](active-directory-conditional-access-best-practices.md#whats-required-to-make-a-policy-work) bir ortam için bir koşullu erişim ilkesini uygulamak için. 

Bu makalede, koşullar ve bir koşullu erişim ilkesini nasıl kullanıldıkları hakkında genel bir bakış sağlar. 

## <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcılar ve gruplar bir koşullu erişim ilkesi zorunlu bir durumdur. Ya da seçim yapabileceğiniz ilkenizde, **tüm kullanıcıların** veya belirli kullanıcılar ve Gruplar'ı seçin.

![Denetim](./media/active-directory-conditional-access-conditions/111.png)

Seçtiğinizde:

- **Tüm kullanıcılar**, ilkenizi dizindeki tüm kullanıcılara uygulanır. Bu, Konuk kullanıcılar içerir.

- **Kullanıcıları ve grupları seçin**, aşağıdaki seçenekleri belirleyin:

    - **Tüm Konuk kullanıcılar** -B2B Konuk kullanıcılar için bir ilke hedef olanak tanır. Bu koşul herhangi bir kullanıcı hesabı ile eşleşen *userType* özniteliğini *Konuk*. Hesap Azure AD'de bir davet akışı oluşturulduktan hemen sonra uygulanan bir ilke gereken durumlarda bu ayarı kullanın.

    - **Dizin rolleri** -kullanıcının rol ataması dayalı bir ilke hedef olanak tanır. Bu durum Dizin rolleri gibi destekleyen *genel yönetici* veya *parola Yöneticisi*.

    - **Kullanıcılar ve gruplar** -hedef belirli kullanıcı kümelerine sağlar. Örneğin, tüm üyeleri ik departmanı, bulut uygulaması seçilen bir HR uygulama olduğunda içeren bir grup seçebilirsiniz.

Bir grup da grup herhangi bir türde dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD'de olabilir

Belirli kullanıcı veya Grup İlkesi'nden dışlayabilirsiniz. Bir ortak kullanım örneği olup hizmet hesapları, ilke çok faktörlü kimlik doğrulaması (MFA) zorunlu tutar. 

Belirli kullanıcı kümeleri için hedefleme yeni bir ilke dağıtımı için yararlıdır. Yeni bir ilke ilkesi davranışını doğrulamak için kullanıcıları ilk bir kümesini hedeflemelidir. 



## <a name="cloud-apps"></a>Bulut uygulamaları 

Web siteleri bir bulut uygulamasıdır veya hizmet. Azure uygulama ara sunucusu tarafından korunan web siteleri içerir. Desteklenen bulut uygulamalarını ayrıntılı bir açıklaması için bkz: [bulut uygulamaları atama](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Bulut uygulamaları bir koşullu erişim ilkesi zorunlu bir durumdur. Ya da seçim yapabileceğiniz ilkenizde, **tüm bulut uygulamaları** veya belirli uygulamaları seçin.

![Denetim](./media/active-directory-conditional-access-conditions/03.png)

Şunları seçebilirsiniz:

- **Tüm bulut uygulamaları** tüm kuruluş uygulanacak temel ilkeleri. Bu seçim için ortak bir kullanım örneği oturum açma riski algılandığında her bulut uygulaması için çok faktörlü kimlik doğrulaması gerektiren bir ilkedir. Uygulanan bir ilke **tüm bulut uygulamaları** erişim için geçerli tüm web sitesi ve hizmetler. Bu ayar görünür bulut uygulamaları için sınırlı değildir **seçin bulut uygulamalarını** listesi.

- Tek tek bulut uygulamalarını ilkelerle hedef belirli hizmetlerine. Kullanıcıların Örneğin, gerektiren bir [uyumlu aygıt](active-directory-conditional-access-policy-connected-applications.md) SharePoint Online'a erişmek için. SharePoint içeriği, örneğin, Microsoft Teams eriştiklerinde Bu ilke ayrıca diğer hizmetlere uygulanır. 

Bir ilkenin belirli uygulamaları de hariç tutabilirsiniz; Ancak, bu uygulamaları hala eriştiklerinde services uygulanan ilkeler tabidir. 



## <a name="sign-in-risk"></a>Oturum açma riski

Bir oturum açma riski bir oturum açma girişimi meşru bir kullanıcı hesabı sahibi tarafından gerçekleştirilmedi olasılığını (yüksek, Orta veya düşük) için bir göstergesidir. Azure AD oturum açma sırasında bir kullanıcının oturum açma risk düzeyi hesaplar. Oturum açma hesaplanan risk düzeyi, bir koşullu erişim ilkesi koşulu olarak kullanabilirsiniz. 

![Koşullar](./media/active-directory-conditional-access-conditions/22.png)

Bu koşulu kullanmak için gerek [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) etkin.
 
Bu koşul için ortak kullanım durumları olan ilkeler:

- Potansiyel olarak yasal olmayan kullanıcıların, bulut uygulamalarınızı erişimini engellemek için bir yüksek oturum açma riski kullanıcılarla engelleyin. 
- Orta bir oturum açma riski olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir. Çok faktörlü kimlik doğrulamasını zorunlu tutarak, oturum açma yasal bir hesap sahibi tarafından gerçekleştirildiğinden ek güvenirlik sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Azure AD kullanıcı aracısı gibi bir aygıt tarafından sağlanan bilgileri kullanarak platform tanımlar. Bu bilgiler doğrulanmamış olduğundan, tüm platformlar bunları, erişimi engelleme, Intune ilkeleriyle uyumluluğunu gerektiren veya cihaz etki alanına katılmış olması gerektiren tarafından uygulanan bir ilke olması önerilir. Tüm cihaz platformları için ilkeyi uygulamak için varsayılandır. 


![Koşullar](./media/active-directory-conditional-access-conditions/24.png)

Desteklenen cihaz platformlarının tam bir listesi için bkz: [cihaz platformu koşul](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Bu durum, bulut uygulamalarınızı erişimi kısıtlayan bir ilke için bir ortak kullanım örneği [yönetilen cihazlara](active-directory-conditional-access-policy-connected-applications.md#managed-devices). Cihaz platform koşulu dahil daha fazla senaryoları için bkz: [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md).



## <a name="device-state"></a>Cihaz durumu

Cihaz durumu koşulu karma Azure AD alanına ve aygıtları bir koşullu erişim ilkesinden hariç tutulacak uyumlu olarak işaretlenmiş sağlar. Bu, bir ilke yalnızca ek oturum güvenliğini sağlamak için yönetilmeyen cihaza uygulanmalıdır durumunda faydalı olur. Örneğin, bir aygıt yönetilmeyen olduğunda yalnızca Microsoft Cloud App Security oturum denetimi uygulayın. 


![Koşullar](./media/active-directory-conditional-access-conditions/112.png)

Yönetilmeyen cihazlar için erişimi engellemek istiyorsanız, uygulamalıdır [cihaz temelli koşullu erişim](active-directory-conditional-access-policy-connected-applications.md).


## <a name="locations"></a>Konumlar

Konumları ile bir bağlantı girişimini gelen başlatıldığı üzerinde temel koşullarını tanımlamak için seçeneğiniz vardır. 
     
![Koşullar](./media/active-directory-conditional-access-conditions/25.png)

Bu koşul için ortak kullanım durumları olan ilkeler:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.  

- Bir hizmeti belirli ülke veya bölgelerden erişen kullanıcıların erişimi engeller. 

Daha fazla bilgi için bkz: [Azure Active Directory koşullu erişim konumu durumu nedir?](active-directory-conditional-access-locations.md)


## <a name="client-apps"></a>İstemci uygulamaları

İstemci uygulamaları koşulu gibi uygulamaları, farklı türde bir ilkenin uygulanacağı sağlar:

- Web siteleri ve Hizmetleri
- Mobil uygulamalar ve Masaüstü uygulamaları. 



Bir uygulama olarak sınıflandırılır:

- Bir web sitesi veya web SSO protokolleri, SAML, kullanıyorsa, hizmet WS-Fed veya Openıd Connect gizli bir istemci için.

- Bir mobil uygulaması ya da yerel bir istemci için mobil uygulama Openıd Connect kullanıyorsa, masaüstü uygulaması.

Koşullu erişim ilkenizi kullanabileceğiniz istemci uygulamaları tam bir listesi için bkz: [istemci uygulamaları koşulu](active-directory-conditional-access-technical-reference.md#client-apps-condition) Azure Active Directory koşullu erişim Teknik Başvurusu.

Bu koşul için ortak kullanım durumları olan ilkeler:

- Gerektiren bir [uyumlu aygıt](active-directory-conditional-access-policy-connected-applications.md) herhangi bir CİHAZDAN tarayıcı erişim sağlarken büyük miktarlarda verinin cihaza karşıdan mobil ve Masaüstü uygulamaları için.

- Web uygulamalarından gelen erişimi engellemek, ancak Mobil ve Masaüstü uygulamaları erişime izin verecek.

Web SSO ve modern kimlik doğrulama protokollerini kullanarak ek olarak, yerel posta uygulamaları çoğu akıllı telefonlar gibi kullanan Exchange ActiveSync posta uygulamaları için bu koşul uygulayabilirsiniz. Şu anda, eski protokolleri kullanarak istemci uygulamaları AD FS kullanarak korunması gerekir.

Bu durum, yalnızca seçebilirsiniz **Office 365 Exchange Online** seçtiğiniz yalnızca bulut uygulaması.

![Bulut uygulamaları](./media/active-directory-conditional-access-conditions/32.png)

Seçme **Exchange ActiveSync** başka koşullar yapılandırılan bir ilke yoksa, istemci uygulamaları olarak koşulu yalnızca desteklenir. Ancak, yalnızca desteklenen platformlar uygulamak için bu koşul kapsamını daraltabilirsiniz.

 
![Desteklenen platformlar](./media/active-directory-conditional-access-conditions/33.png)

Bu koşul yalnızca desteklenen platformlar için uygulama olduğundan tüm cihaz platformları için eşdeğer bir [cihaz platformu koşul](active-directory-conditional-access-conditions.md#device-platforms).

![Desteklenen platformlar](./media/active-directory-conditional-access-conditions/34.png)


 Daha fazla bilgi için bkz.

- [SharePoint Online ve Exchange Online için koşullu erişim Azure Active Directory ayarlama](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md) 


### <a name="legacy-authentication"></a>Eski kimlik doğrulaması  

Koşullu erişim artık modern kimlik doğrulamayı desteklemeyen eski Office istemcileri ve bunun yanı sıra, POP, IMAP, SMTP, vb. gibi posta protokolleri kullanan istemciler için geçerlidir. Bu gibi ilkeleri yapılandırmanıza izin verir **diğer istemcilerden erişimi engelleme**.


![Eski kimlik doğrulaması](./media/active-directory-conditional-access-conditions/160.png)
 



#### <a name="known-issues"></a>Bilinen sorunlar

- İlke yapılandırma **diğer istemciler** SPConnect gibi belirli istemcilerden gelen tüm kuruluş engeller. Beklenmedik bir şekilde kimlik doğrulaması bu eski istemciler nedeni budur. Bu sorunun temel eski Office istemcileri gibi Office uygulamaları geçerli değildir. 

- İlkenin etkili olması için 24 saate kadar sürebilir. 


#### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Bu, Exchange Web Hizmetleri (EWS) engelleyecek mi?**

EWS kullanarak kimlik doğrulama protokolü bağlıdır. EWS uygulama modern kimlik doğrulamasını kullanıyorsanız, "mobil uygulamalar ve Masaüstü istemcileri" istemci uygulaması tarafından ele alınacaktır. EWS uygulama temel kimlik doğrulamasını kullanıyorsanız, "Diğer istemciler" istemci uygulaması tarafından ele alınacaktır.


**Hangi denetimlerin diğer istemciler için kullanabilir miyim**

Herhangi bir denetimi "Diğer istemciler için" yapılandırılabilir. Ancak, son kullanıcı deneyimi için tüm durumlarda erişimi olacaktır. "Diğer istemciler" uyumlu aygıt, etki alanına katılma, vb. denetimleri MFA gibi desteklemez. 
 
**Hangi şartlar diğer istemciler için kullanabilir miyim?**

Tüm koşullar "Diğer istemciler için" yapılandırılabilir.

**Exchange ActiveSync tüm koşullar ve denetimleri destekliyor mu?**

Hayır. Exchange ActiveSync (EAS) destek özeti aşağıda verilmiştir:

- EAS yalnızca kullanıcı ve Grup hedeflemenin destekler. Konuk, rolleri desteklemiyor. Konuk/rol koşul yapılandırdıysanız, biz İlkesi kullanıcıya veya uygulanacaksa belirleyemediğinden tüm kullanıcılar engellenen.

- EAS Exchange ile yalnızca bulut uygulama olarak çalışır. 

- EAS istemci uygulamanın kendi dışında herhangi bir koşul desteklemez.

- EAS (cihaz uyumluluğu dışında tüm bloğuna götürür) herhangi bir denetim ile yapılandırılabilir.

**İlkeler tüm istemci uygulamaları için ileriye dönük varsayılan olarak uygulamak?**

Hayır. Varsayılan ilke davranışında değişiklik yoktur. İlkeler, tarayıcı ve mobil uygulamalar/Masaüstü istemcileri için varsayılan olarak uygulanmaya devam eder.



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory koşullu erişimi olan belirli uygulamalar için MFA gerektiren](active-directory-conditional-access-app-based-mfa.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 

