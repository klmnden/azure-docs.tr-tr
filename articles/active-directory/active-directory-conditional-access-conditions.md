---
title: "Azure Active Directory koşullu erişim koşullarında | Microsoft Docs"
description: "Atamaları bir ilke tetiklemek için Azure Active Directory koşullu erişimin nasıl kullanıldığı hakkında bilgi edinin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: bb2124613ccc467f3c560e92bdf760420410267c
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim koşulları 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bir koşullu erişim ilkesi ("bunu") yanıt ("Bu durumda") ilkeniz tetikleme nedenini tanımlayın. 

![denetimi](./media/active-directory-conditional-access-conditions/10.png)


Koşullu erişim bağlamında:

- "**Bu durumda**" olarak adlandırılır **koşullar**. 
- "**Sonra bunu**" olarak adlandırılır **erişim denetimleri**.

Koşullarınızı erişim denetimleri ile birlikte bir koşullu erişim ilkesi temsil eder.

![denetimi](./media/active-directory-conditional-access-conditions/61.png)

Bu makalede, koşullar ve bir koşullu erişim ilkesini nasıl kullanıldıkları hakkında genel bir bakış sağlar. 


## <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcılar ve gruplar bir koşullu erişim ilkesi zorunlu bir durumdur. Ya da seçim yapabileceğiniz ilkenizde, **tüm kullanıcıların** veya belirli kullanıcılar ve Gruplar'ı seçin.

![denetimi](./media/active-directory-conditional-access-conditions/02.png)

Seçtiğinizde:

- **Tüm kullanıcılar**, ilkenizi dizininde ile tüm kullanıcılara uygulanır. Bu, Konuk kullanıcılar içerir.

- **Kullanıcıları ve grupları seçin**, ik uygulamaya oturum açtığınızda belirli kullanıcı kümeleri için örneğin, tüm ik departmanı üyeleri hedefleyebilirsiniz. 

- Bir grup, dinamik ya da atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD'de Grup herhangi bir türde olabilir.

Belirli kullanıcı veya Grup İlkesi'nden dışlayabilirsiniz. Bir ortak kullanım örneği olup hizmet hesapları, ilke çok faktörlü kimlik doğrulamasını zorunlu tutar. 

Belirli kullanıcı kümeleri için hedefleme yeni bir ilke dağıtımı için yararlıdır. Yeni bir ilke ilkesi davranışını doğrulamak için kullanıcıları ilk bir kümesini hedeflemelidir. 



## <a name="cloud-apps"></a>Bulut uygulamaları 

Web siteleri bir bulut uygulamasıdır veya hizmet. Azure uygulama ara sunucusu tarafından korunan web siteleri içerir. Desteklenen bulut uygulamalarını ayrıntılı bir açıklaması için bkz: [bulut uygulamaları atama](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Bulut uygulamaları bir koşullu erişim ilkesi zorunlu bir durumdur. Ya da seçim yapabileceğiniz ilkenizde, **tüm bulut uygulamaları** veya belirli uygulamaları seçin.

![denetimi](./media/active-directory-conditional-access-conditions/03.png)

Şunları seçebilirsiniz:

- **Tüm bulut uygulamaları** tüm kuruluş uygulanacak temel ilkeleri. Bu seçim için ortak bir kullanım örneği oturum açma riski algılandığında her bulut uygulaması için çok faktörlü kimlik doğrulaması gerektiren bir ilkedir.

- Tek tek bulut uygulamalarını ilkelerle hedef belirli hizmetlerine. Kullanıcıların Örneğin, gerektiren bir [uyumlu aygıt](active-directory-conditional-access-policy-connected-applications.md) SharePoint Online'a erişmek için. SharePoint içeriği, örneğin, Microsoft Teams eriştiklerinde Bu ilke ayrıca diğer hizmetlere uygulanır. 

Bir ilkenin belirli uygulamaları de hariç tutabilirsiniz; Ancak, bu uygulamaları hala eriştiklerinde services uygulanan ilkeler tabidir. 



## <a name="sign-in-risk"></a>Oturum açma riski

Bir oturum açma riski bir oturum açma girişimi meşru bir kullanıcı hesabı sahibi tarafından gerçekleştirilmedi olasılığını (yüksek, Orta veya düşük) için bir göstergesidir. Azure AD oturum açma sırasında bir kullanıcının oturum açma risk düzeyi hesaplar. Oturum açma hesaplanan risk düzeyi, bir koşullu erişim ilkesi koşulu olarak kullanabilirsiniz. 

![Koşullar](./media/active-directory-conditional-access-conditions/22.png)

Bu koşulu kullanmak için gerek [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) etkin.
 
Bu koşul için ortak kullanım durumları olan ilkeler:

- Potansiyel olarak yasal olmayan kullanıcıların, bulut uygulamalarınızı erişimini engellemek için bir yüksek oturum açma riski kullanıcılarla engelleyin. 
- Orta bir oturum açma riski sahip olan kullanıcıların, çok faktörlü kimlik doğrulaması gerektirir. Çok faktörlü kimlik doğrulamasını zorunlu tutarak, oturum açma yasal bir hesap sahibi tarafından gerçekleştirildiğinden ek güvenirlik sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Azure AD kullanıcı aracısı gibi bir aygıt tarafından sağlanan bilgileri kullanarak platform tanımlar. Bu bilgiler doğrulanmamış olduğundan, tüm platformlar bunları, erişimi engelleme, Intune ilkeleriyle uyumluluğunu gerektiren veya cihaz etki alanına katılmış olması gerektiren tarafından uygulanan bir ilke olması önerilir. Tüm cihaz platformları için ilkeyi uygulamak için varsayılandır. 


![Koşullar](./media/active-directory-conditional-access-conditions/02.png)

Desteklenen cihaz platformlarının tam bir listesi için bkz: [cihaz platformu koşul](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Bu durum, bulut uygulamalarınızı erişimi kısıtlayan bir ilke için bir ortak kullanım örneği [güvenilen cihazları](active-directory-conditional-access-policy-connected-applications.md#trusted-devices). Cihaz platform koşulu dahil daha fazla senaryoları için bkz: [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Konumlar

Konumları ile bir bağlantı girişimini gelen başlatıldığı üzerinde temel koşullarını tanımlamak için seçeneğiniz vardır. 
     
![Koşullar](./media/active-directory-conditional-access-conditions/03.png)

Bu koşul için ortak kullanım durumları olan ilkeler:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.  

- Bir hizmeti belirli ülke veya bölgelerden erişen kullanıcıların erişimi engeller. 

Daha fazla bilgi için bkz: [Azure Active Directory koşullu erişim konumu koşullarında](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>İstemci uygulamaları

İstemci uygulamaları koşulu gibi uygulamaları, farklı türde bir ilkenin uygulanacağı sağlar:

- Web siteleri ve Hizmetleri
- Mobil uygulamalar ve Masaüstü uygulamaları. 

![Koşullar](./media/active-directory-conditional-access-conditions/04.png)

Bir uygulama olarak sınıflandırılır:

- Bir web sitesi veya web SSO protokolleri, SAML, kullanıyorsa, hizmet WS-Fed veya Openıd Connect gizli bir istemci için.

- Bir mobil uygulaması ya da yerel bir istemci için mobil uygulama Openıd Connect kullanıyorsa, masaüstü uygulaması.

Koşullu erişim ilkenizi kullanabileceğiniz istemci uygulamaları tam bir listesi için bkz: [Azure Active Directory koşullu erişim teknik başvuru](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Bu koşul için ortak kullanım durumları olan ilkeler:

- Gerektiren bir [uyumlu aygıt](active-directory-conditional-access-policy-connected-applications.md) herhangi bir CİHAZDAN tarayıcı erişim sağlarken büyük miktarlarda verinin cihaza karşıdan mobil ve Masaüstü uygulamaları için.

- Web uygulamalarından gelen erişimi engellemek, ancak Mobil ve Masaüstü uygulamaları erişime izin verecek.

Web SSO ve modern kimlik doğrulama protokollerini kullanarak ek olarak, yerel posta uygulamaları çoğu akıllı telefonlar gibi kullanan Exchange ActiveSync posta uygulamaları için bu koşul uygulayabilirsiniz. Şu anda, eski protokolleri kullanarak istemci uygulamaları AD FS kullanarak korunması gerekir.

 Daha fazla bilgi için bkz.

- [SharePoint Online ve Exchange Online için koşullu erişim Azure Active Directory ayarlama](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory Uygulama temelli koşullu erişim](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 

