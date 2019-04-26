---
title: Azure Active Directory koşullu erişim koşulları nelerdir? | Microsoft Docs
description: Koşul bir ilkeyi tetiklemek için Azure Active Directory koşullu erişim nasıl kullanıldığı hakkında bilgi edinin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2018
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f95fd85b5a0fd9e905b93b9b90f18f963dbf1690
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60355783"
---
# <a name="what-are-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim koşulları nelerdir? 

Kullanılarak kullanıcıların bulut uygulamalarınıza erişme denetleyebilirsiniz [Azure Active Directory (Azure AD) koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Bir koşullu erişim ilkesinde yanıtı tanımlayın ("ardından ("Bu durumda") ilkeniz tetikleme nedeni için bunun"). 

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

* **Tüm Konuk kullanıcılar** B2B Konuk kullanıcılar için bir ilke hedefler. Sahip herhangi bir kullanıcı hesabı bu koşulu ile eşleşene **userType** özniteliğini **Konuk**. Hesap, Azure AD'de bir davet akışı oluşturulduktan hemen sonra uygulanacak ilke ihtiyacı olduğunda bu ayarı kullanın.

* **Dizin rolleri** kullanıcının rol atamaya bağlı bir ilke hedefler. Dizin rolleri gibi bu koşulu destekler **genel yönetici** veya **parola Yöneticisi**.

* **Kullanıcılar ve gruplar** belirli kullanıcıların kümesini hedefler. Örneğin, bulut uygulaması ik uygulama seçildiğinde tüm çözümler. ik departmanı üyeleri içeren bir grup seçebilirsiniz. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere, Azure AD'de Grup herhangi bir türde olabilir.

Belirli kullanıcılar veya gruplar bir ilkeden dışlayabilirsiniz. Bir ortak kullanım örneği hizmet hesapları ise ilkenizde, çok faktörlü kimlik doğrulaması (MFA) zorunlu kılar. 

Kullanıcılar belirli kümelerini hedefleyen yeni bir ilke dağıtımı için kullanışlıdır. Yeni bir ilke yalnızca ilk kümesi ilkesi davranışını doğrulamak için kullanıcı hedeflemelidir. 



## <a name="cloud-apps"></a>Bulut uygulamaları 

Bulut uygulaması, bir Web sitesi veya hizmet örneğidir. Azure AD uygulama ara sunucusu tarafından korunan Web siteleri, bulut uygulamaları da taşır. Desteklenen bulut uygulamalarının ayrıntılı bir açıklaması için bkz. [bulut uygulamaları atamaları](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#cloud-apps-assignments). 

**Bulut uygulamaları** koşulu bir koşullu erişim ilkesinde zorunludur. İlkenizde, yi yapabilecekleriniz **tüm bulut uygulamaları** veya belirli uygulamalar seçin.

![Bulut uygulamaları içerir](./media/conditions/03.png)

Seçin:

- **Tüm bulut uygulamaları** kuruluş genelinde uygulamak için temel ilkeleri. Oturum açma riski algılandığında herhangi bir bulut uygulamasında için çok faktörlü kimlik doğrulaması gerektiren ilkeleri için bu seçimi kullanın. Uygulanan bir ilke **tüm bulut uygulamaları** erişim için geçerli tüm Web siteleri ve Hizmetleri. Bu ayar görünen bulut uygulamalarına sınırlı değildir **uygulamaları Seç** listesi. 

- **Uygulamaları seçin** ilkeniz tarafından hedef belirli hizmetler için. Örneğin, kullanıcıların gerektirebilir bir [uyumlu cihaz](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam#app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online) SharePoint Online'a erişmek için. SharePoint içeriği eriştiklerinde Bu ilke, diğer hizmetlere de uygulanır. Microsoft Teams buna bir örnektir. 

Bir ilkenin belirli uygulamaları hariç tutabilirsiniz. Ancak, bu uygulamalar yine de erişim hizmetleri için uygulanan ilkelerle tabidir. 



## <a name="sign-in-risk"></a>Oturum açma riski

Oturum açma riski bir oturum açma kullanıcı hesabının meşru sahibi tarafından yapılan değildi olasılığını (yüksek, Orta veya düşük) bir göstergesidir. Azure AD oturum açma sırasında bir kullanıcı oturum açma risk düzeyini hesaplar. Koşullu erişim ilkesi koşulu olarak hesaplanan oturum açma risk düzeyini kullanabilirsiniz.

![Oturum açma risk düzeyleri](./media/conditions/22.png)

Bu koşulu kullanmak için ihtiyacınız [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable) etkin.
 
Bu koşul için yaygın kullanım örnekleri aşağıdaki korumalar ilkelerdir: 

- Bir yüksek oturum açma riski kullanıcılarla engelleyin. Bu koruma potansiyel olarak yasal olmayan kullanıcılar, bulut uygulamalarınıza erişmesini engeller. 
- Orta ölçekli bir oturum açma riski olan kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir. Çok faktörlü kimlik doğrulamasını zorunlu tutarak, oturum açmanın meşru bir hesap sahibi tarafından yapılan ek güvence sağlayabilir.

Daha fazla bilgi için [oturumu risk algılandığında erişimi engelle](app-sign-in-risk.md).  

## <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Azure AD kullanıcı aracısı gibi cihaz tarafından sağlanan bilgileri kullanarak platform tanımlar. Bu bilgiler, doğrulanmamış. Tüm platformlar bir ilke uygulanmış olmasını öneririz. İlke ya da erişimi engellemek, Intune ilkelerle uyumluluğu gerektiren veya cihaz etki alanına katılmış olması gerekir. Tüm cihaz platformları için bir ilkeyi uygulayabilmeniz için varsayılandır. 


![Cihaz platformlarını yapılandırın](./media/conditions/24.png)

Desteklenen cihaz platformları listesi için bkz. [cihaz platformu koşul](technical-reference.md#device-platform-condition).


Bu durum, bulut uygulamalarınıza erişimi kısıtlayan bir ilke için bir ortak kullanım örneği [yönetilen cihazlar](require-managed-devices.md). Cihaz platformu koşulu dahil olmak üzere daha fazla senaryo için bkz. [Azure Active Directory uygulama tabanlı koşullu erişim](app-based-conditional-access.md).



## <a name="device-state"></a>Cihaz durumu

Cihaz durumu koşulunu ve koşullu erişim ilkesi uyumsuz olarak işaretlenmiş aygıtlar Azure AD'ye katılmış karma dışlar. 


![Cihaz durumlarını yapılandırın](./media/conditions/112.png)

Bu durum, ek bir oturum güvenliği sağlamak için yalnızca bir yönetilmeyen cihaza bir ilkenin uygulanacağı yararlıdır. Örneğin, bir cihazın yönetilmeyen olduğunda yalnızca Microsoft Cloud App Security oturum denetimi uygular. 

## <a name="locations"></a>Konumlar

Konumları kullanarak bağlantı burada denendi göre koşullar tanımlayabilirsiniz. 

![Konumları yapılandırın](./media/conditions/25.png)

Bu koşul için yaygın kullanım örnekleri aşağıdaki koruma ilkeleriyle şunlardır:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektirir.  

- Bir hizmetin belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelleyin. 

Daha fazla bilgi için [konum koşulu Azure Active Directory koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).


## <a name="client-apps"></a>İstemci uygulamaları

Varsayılan olarak, şu uygulamalar için koşullu erişim ilkesi uygular:

- **[Tarayıcı uygulamaları](technical-reference.md#supported-browsers)**  -tarayıcı uygulamalarıdır SAML kullanarak Web siteleri WS-Federation ve Openıd Connect web SSO protokoller. Bu bir OAuth gizli istemci kayıtlı herhangi bir Web sitesi veya web hizmeti için de geçerlidir. Örneğin, Office 365 SharePoint Web sitesi. 

- **[Mobil ve Masaüstü uygulamaları modern kimlik doğrulaması kullanan](technical-reference.md#supported-mobile-applications-and-desktop-clients)**  -bu uygulamaları, telefon uygulamaları ve Office Masaüstü uygulamalarını içerir. 


Ayrıca, bir modern kimlik doğrulaması, örneğin kullanmayan belirli istemci uygulamalarında ilkeyi hedef alabilirsiniz:

- **[Exchange ActiveSync istemcileri](conditions.md#exchange-activesync-clients)**  - Exchange ActiveSync kullanarak bir ilke blokları, etkilenen kullanıcılar engellenen neden hakkında bilgi içeren bir tek karantina e-posta alın. Gerekirse, e-posta, cihazlarını ıntune'a kaydetme yönergelerini içerir.

- **[Diğer istemciler](block-legacy-authentication.md)**  -posta protokollerini IMAP MAPI, POP, SMTP ve gibi modern kimlik doğrulama kullanmayan eski Office uygulamaları ile temel kimlik doğrulaması kullanan istemciler bu uygulamaları içerir. Daha fazla bilgi için [Office 2013 ve Office 2016 istemci uygulamaları için nasıl modern kimlik doğrulaması çalıştığı](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016).

![İstemci uygulamaları](./media/conditions/41.png)

Bu koşul için yaygın kullanım örnekleri aşağıdaki gereksinimlere sahip ilkelerdir:

- **[Yönetilen bir cihazı gerektiren](require-managed-devices.md)**  verileri indirmek için bir cihaz mobil ve Masaüstü uygulamaları için. Aynı zamanda, tüm cihazlardan tarayıcı erişimi izin verin. Bu senaryo için yönetilmeyen bir cihazı kaydetme ve eşitleme belgeleri engeller. Cihaz kaybolur veya çalınırsa, bu yöntem ile veri kaybı için olasılık azaltabilir.

- **[Yönetilen bir cihazı gerektiren](require-managed-devices.md)**  Exchange Online'a erişmek için ActiveSync kullanarak uygulamalar için.

- **[Eski bir kimlik doğrulama bloğu](block-legacy-authentication.md)**  Azure AD'ye (diğer istemciler)

- Web uygulamalarından erişimi engellemek, ancak Mobil ve Masaüstü uygulamalardan erişime izin ver



### <a name="exchange-activesync-clients"></a>Exchange ActiveSync istemcileri

Yalnızca seçebilirsiniz **Exchange ActiveSync istemcileri** varsa:


- Microsoft Office 365 Exchange Online, seçtiğiniz tek bulut uygulamasıdır.

    ![Bulut uygulamaları](./media/conditions/32.png)

- Bir ilkede yapılandırılabilecek diğer koşullar yoktur. Ancak, bu koşul yalnızca uygulanacak kapsamını daraltmak daraltabilirsiniz [desteklenen platformlar](technical-reference.md#device-platform-condition).
 
    ![İlkeyi yalnızca desteklenen platformlara uygula](./media/conditions/33.png)


Ne zaman erişim engellendi bir [yönetilen cihaz](require-managed-devices.md) olan gerekli, etkilenen kullanıcılar bunları Intune kullanacak şekilde yol gösteren tek bir posta alın. 

Etkilenen kullanıcılar, onaylanmış bir uygulama gerekiyorsa, yükleme ve Outlook mobil istemciyi kullanma yönergeleri alın.

MFA gerekliyse, temel kimlik doğrulaması kullanan istemciler, mfa'yı desteklemeyen çünkü diğer durumlarda, örneğin, etkilenen kullanıcılar, engellenir.

Kullanıcılar ve gruplar için bu ayarı yalnızca hedefleyebilirsiniz. Konuklar veya rollerini desteklemez. Bir konuk veya rol durumu yapılandırdıysanız, koşullu erişim ilkesi kullanıcıya veya uygulanacaksa belirleyemediğinden tüm kullanıcılar engellenir.


 Daha fazla bilgi için bkz.

- [SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişim ayarlama](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
 
- [Azure Active Directory uygulama tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). 



## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma konusunda bilgi edinmek için bkz: [hızlı başlangıç: Azure Active Directory koşullu erişimiyle belirli uygulamalar için mfa'yı gerekli](app-based-mfa.md).

- Ortamınız için koşullu erişim ilkeleri yapılandırmak için bkz: [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 

