---
title: Azure Active Directory koşullu erişim ile kimlik doğrulama oturumunun yönetimini yapılandırma
description: Kullanıcı oturum sıklığı ve tarayıcı oturum kalıcılığı dahil olmak üzere Azure AD kimlik doğrulaması oturum yapılandırmasını özelleştirin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8897de5ee86d20e52b948f21afaef4acf196539
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988569"
---
# <a name="configure-authentication-session-management-with-conditional-access"></a>Kimlik doğrulaması oturum yönetimi ile koşullu erişimi yapılandırma

Karmaşık dağıtımlarda, kuruluşların kimlik doğrulama oturumları kısıtlamak gerekli olabilir. Bazı senaryolarda şunlar olabilir:

* Yönetilmeyen veya paylaşılan bir CİHAZDAN kaynak erişimi
* Dış bir ağ üzerinden hassas bilgilere erişim
* Yüksek etkili kullanıcılar
* İş açısından kritik uygulamalar

Koşullu erişim denetimleri belirli kullanım durumları, kuruluşunuzdaki tüm kullanıcılar etkilenmeden hedefleyen ilkeler oluşturmanızı sağlar.

İlke yapılandırma hakkında ayrıntılara girmeden önce varsayılan yapılandırmayı inceleyelim.

## <a name="user-sign-in-frequency"></a>Kullanıcı oturum açma sıklığı

Oturum açma sıklığı, bir kullanıcı bir kaynağa erişmeye çalışırken yeniden oturum açmanız istenir önceki zaman aralığını tanımlar.

Kullanıcı oturumu açma sıklığı için Azure Active Directory (Azure AD) varsayılan yapılandırma, 90 gün sıralı bir penceredir. Kullanıcılar genellikle kimlik bilgilerinin sorulmasına yapılacak mantıklı bir şey gibi görünüyor, ancak backfire: yanlışlıkla düşünmeye can olmadan kimlik bilgilerini girmek için eğitim görmüş olan kullanıcılar bunları bir kötü amaçlı kimlik bilgisi istemine sağlayın.

90 gün boyunca herhangi bir ihlali BT ilkeleri oturumu iptal eder gerçekte yeniden oturum açmasına soran değildir bu yüzden ilgili ses. Bazı örnekler dahil (ancak bunlarla sınırlı olmayan) bir parola değişikliği, uyumsuz cihaz ya da hesabı devre dışı bırak. Aynı zamanda açıkça yapabilecekleriniz [PowerShell kullanarak kullanıcıların oturumunu iptal](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0). Azure AD varsayılan yapılandırması "güvenlik duruşunu oturumlarını değiştirilmediyse, kimlik bilgilerini sağlamak üzere kullanıcıları sorma için" gelir.

Oturum açma sıklığı ayarını OATH2 veya OIDC protokollerini standartlarına göre uyguladıysanız uygulamalarla çalışır. Çoğu Microsoft Windows, Mac ve mobil için yerel uygulamalar ayarı ile uyumlu.

## <a name="persistence-of-browsing-sessions"></a>Oturumlarının atma kalıcılığı

Kalıcı tarayıcı oturumu, kullanıcıların kendi tarayıcı penceresi kapatılıp yeniden sonra oturumunun açık kalmasına olanak tanır.

Azure AD varsayılan tarayıcı oturum kalıcılığı için kullanıcıların kişisel cihazlarda oturum "bir kalın oturum açıldı?" göstererek kalıcı hale getirmek isteyip istemediğinizi seçin sağlar. başarılı kimlik doğrulamasından sonra ister. Tarayıcı Kalıcılık makaledeki yönergeler kullanılarak AD FS'de yapılandırılıp yapılandırılmadığını [AD FS çoklu oturum açma ayarları](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-single-sign-on-settings#enable-psso-for-office-365-users-to-access-sharepoint-online
), bu ilkeye uymak ve Azure AD oturum devam ederiz. Kiracınızdaki kullanıcılara "kalın oturum açıldı?" görüp de yapılandırabilirsiniz komut istemi bölmesinde makaledeki yönergeleri kullanarak Azure portalında marka şirketteki uygun ayarı değiştirerek [Azure AD oturum açma sayfanızı özelleştirmek](../fundamentals/customize-branding.md).

## <a name="configuring-authentication-session-controls"></a>Kimlik doğrulama oturumu denetimlerini yapılandırma

Koşullu erişim, bir Azure AD Premium özelliğidir ve bir premium lisansı gerektirir. Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure Active Directory'de koşullu erişim nedir?](overview.md#license-requirements)

> [!WARNING]
> Kullanıyorsanız [yapılandırılabilir belirteç ömrü](../develop/active-directory-configurable-token-lifetimes.md) özelliği şu anda genel Önizleme sürümünde, aynı kullanıcı veya uygulama birleşimi için iki farklı ilke oluşturmayı desteklemiyoruz olduğunu lütfen unutmayın: Bu özellik ve ile başka bir sahip yapılandırılabilir belirteç ömrü özelliği. Microsoft, yapılandırılabilir bir belirteç ömrü özellik 15 Ekim devre dışı bırakma ve koşullu erişimi kimlik doğrulaması oturum yönetim özelliğini değiştirmek planlamaktadır.  

### <a name="policy-1-sign-in-frequency-control"></a>İlke 1: Oturum açma sıklığı denetimi

1. Yeni ilke oluştur
1. Hedef bulut uygulamaları dahil olmak üzere, müşterinin ortam için gerekli tüm koşulları seçin.

   > [!NOTE]
   > En iyi kullanıcı deneyimi için Exchange Online ve SharePoint Online gibi anahtar Microsoft Office uygulamaları için eşit kimlik doğrulama istemi sıklığı ayarlamak için önerilir.

1. Git **erişim denetimleri** > **oturumu** tıklatıp **oturum sıklığı**
1. İlk metin kutusuna günleri ve saatleri gerekli değeri girin
1. Değerini seçin **saat** veya **gün** açılır listesinden
1. İlkenizi Kaydet

![Sıklığı oturum için yapılandırılmış koşullu erişim ilkesi](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-sign-in-frequency.png)

Azure AD'de kayıtlı Windows cihazları oturum açma cihaza bir komut istemi olarak kabul edilir. Örneğin, Office uygulamaları için 24 saat için sıklıkla oturum yapılandırdıysanız, cihaz oturum açma sıklığı ilke cihaza açarak yerine getirecek ve yeniden Office uygulamaları açılırken istenmez Windows kullanıcıları Azure AD'de kayıtlı.

Aynı tarayıcı oturumunda çalışan farklı web apps için farklı oturum açma sıklığı yapılandırdıysanız, aynı tarayıcı oturumunda çalışan tüm uygulamalar Çoklu Oturum belirteci paylaştığından her iki uygulama için en katı ilke uygulanır.

### <a name="policy-2-persistent-browser-session"></a>İlke 2: Kalıcı tarayıcı oturumu

1. Yeni ilke oluştur
1. Tüm gerekli koşulları seçin.

   > [!NOTE]
   > Lütfen bu denetimi "Tüm bulut uygulamaları" seçmek için bir koşul olarak gerektiğini unutmayın. Tarayıcı oturum kalıcılığını kimlik doğrulaması Oturum belirteci tarafından denetlenir. Tek Oturum belirteci bir tarayıcı oturumu'deki tüm sekmeler paylaşabilir ve bu nedenle tüm Kalıcılık durumu paylaşmanız gerekir.

1. Git **erişim denetimleri** > **oturumu** tıklatıp **kalıcı tarayıcı oturumu**
1. Açılan listeden bir değer seçin
1. İlkeyi Kaydet

![Kalıcı tarayıcı için yapılandırılmış koşullu erişim ilkesi](media/howto-conditional-access-session-lifetime/conditional-access-policy-session-persistent-browser.png)

> [!NOTE]
> Azure AD koşullu erişim kalıcı tarayıcı oturumu yapılandırmasında "kalın açan?" üzerine yazar Her iki ilkeyi yapılandırdıysanız, aynı kullanıcı için Azure portalındaki bölmesinde marka şirketteki ayarlanıyor.

## <a name="validation"></a>Doğrulama

Hedef uygulama ve diğer koşulları ilkenizi nasıl yapılandırdığınıza göre kullanıcı oturum açma benzetimi için benzetim aracını kullanın. Kimlik doğrulaması oturum yönetim denetimleri aracının sonucu gösterilir.

![Koşullu erişim aracı ne olur](media/howto-conditional-access-session-lifetime/conditional-access-what-if-tool-result.png)

## <a name="policy-deployment"></a>İlke dağıtımı

İlkenizi beklendiği gibi çalıştığından emin olmak için üretim ortamına sunulmadan önce test etmek için önerilen en iyi yöntem olacaktır. İdeal olarak, bir de test kiracılığınız yeni ilkeniz beklendiği gibi çalıştığını doğrulamak için kullanın. Daha fazla bilgi için bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

* Koşullu erişim ilkesini yapılandırma bilmek istiyorsanız, bkz [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).
* Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md).
