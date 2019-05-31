---
title: Temel ilke bloğu eski bir kimlik doğrulama (Önizleme) - Azure Active Directory
description: Koşullu erişim ilkesi bloğu eski kimlik doğrulama protokolleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7eebc68ae8a55d636f3bc85e179bd7d6813be8d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235548"
---
# <a name="baseline-policy-block-legacy-authentication-preview"></a>Temel ilke: Blok eski kimlik doğrulama (Önizleme)

Kullanıcılarınıza bulut uygulamalarınız için kolay erişim sunmak için Azure Active Directory (Azure AD) kimlik doğrulama protokolleri eski bir kimlik doğrulama dahil olmak üzere çok çeşitli destekler. Eski bir kimlik doğrulama tarafından yapılan kimlik doğrulama isteği başvuran bir terimdir:

* Modern kimlik doğrulaması (örneğin, Office 2010 istemci) kullanmayan eski Office istemcileri
* IMAP/SMTP/POP3 gibi eski posta protokollerini kullanan tüm istemci

Bugün, tüm riske atmadan oturum açma girişimlerinin çoğunluğu eski kimlik doğrulamasını gelir. Eski bir kimlik doğrulama, çok faktörlü kimlik doğrulaması (MFA) desteklemez. Dizin üzerinde etkin bir MFA ilkesi olsa bile, kötü bir aktör eski bir protokol kullanarak kimlik doğrulaması ve mfa'yı atla.

Eski protokolleri tarafından yapılan kötü amaçlı kimlik doğrulama istekleri hesabınızı korumak için en iyi yolu bu denemeleri tümünü bir araya engellemektir. Eski protokolleri tarafından yapılan tüm oturum açma isteklerinin engellenip engellenmeyeceğini kolaylaştırmak için yapan bir temel ilke oluşturduk.

![Koşullu erişim bloğu eski kimlik doğrulaması](./media/howto-baseline-protect-legacy-auth/baseline-policy-block-legacy-authentication.png)

**Eski bir kimlik doğrulama bloğu** olduğu [temel ilke](concept-baseline-protection.md) eski protokolleri arasından yapılan tüm kimlik doğrulama isteklerini engeller. Modern kimlik doğrulaması, tüm kullanıcılar için başarıyla oturum açmak için kullanılmalıdır. Diğer temel ilkeleri ile birlikte kullanıldığında, eski kurallarından gelen istekleri engellenir ve tüm kullanıcılar için gerekli olduğunda MFA gerekli olacaktır. Bu ilke, Exchange ActiveSync engellemez.

## <a name="identify-legacy-authentication-use"></a>Eski bir kimlik doğrulama kullanımı belirler

Eski bir kimlik doğrulama dizininizde engellemeden önce ilk kullanıcılarınızın eski bir kimlik doğrulama ve genel dizin etkilemesi kullanan uygulamalar olup olmadığını anlamak gerekir. Azure AD oturum açma günlükleri, eski bir kimlik doğrulama kullanıyorsanız anlamak için kullanılabilir.

1. Gidin **Azure portalında** > **Azure Active Directory** > **oturum açma**.
1. Tıklayarak görüntülenmiyorsa istemci uygulaması sütunu eklemek **sütunları** > **istemci uygulaması**.
1. Filtre ölçütü **istemci uygulaması** > **diğer istemcilerin** tıklatıp **Uygula**.

Oturum show denemelerinin yalnızca filtreleme özelliğinde yapılan yeniliklerle eski kimlik doğrulama protokolleri tarafından. Tek tek oturum açma girişimleri üzerinde tıklayarak ek ayrıntılar gösterilir. **İstemci uygulaması** altında **temel bilgilerini** sekmesinde, eski bir kimlik doğrulama hangi protokolün kullanıldığı gösterecektir.

Bu günlükler, hangi kullanıcıların eski kimlik doğrulaması hala bağlı ve hangi uygulamaların kimlik doğrulama isteği yapmak için eski protokolleri kullanan gösterir. Bu günlüklerde görünmez ve eski bir kimlik doğrulama kullanmayan için onaylanan kullanıcılar için bir koşullu erişim ilkesi uygulamak ya da etkinleştirmek **temel ilke: eski bir kimlik doğrulama bloğu** yalnızca bu kullanıcılar için.

## <a name="moving-away-from-legacy-authentication"></a>Eski bir kimlik doğrulama uzağa taşınması

Eski bir kimlik doğrulama dizininizde kullanan ve hangi uygulamaların bağımlı daha iyi bir fikir aldıktan sonra sonraki adıma modern kimlik doğrulaması kullanmak için kullanıcılarınızın yükseltirken lütfen bekleyin. Modern kimlik doğrulaması, daha güvenli kullanıcı kimlik doğrulaması ve yetkilendirme sağlayan kimlik yönetimi için kullanılan bir yöntemdir. MFA ilkesini yerinde dizininiz varsa, kullanıcı için gerektiğinde MFA istenir, modern kimlik doğrulaması sağlar. Eski bir kimlik doğrulama protokolleri için daha güvenli alternatiftir.

Bu bölüm, modern kimlik doğrulaması için ortamınızı güncelleştirme hakkında adım adım bir genel bakış sağlar. Kuruluşunuzdaki ilke engelleme eski bir kimlik doğrulama etkinleştirmeden önce aşağıdaki adımları inceleyin.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>1. adım: Dizininizdeki modern kimlik doğrulamasını etkinleştirme

Modern kimlik doğrulaması etkinleştirmenin ilk adımı, modern kimlik doğrulaması dizininize desteklediğinden emin yapıyor. Modern kimlik doğrulaması, 1 Ağustos 2017 veya sonrasında oluşturulan dizinler için varsayılan olarak etkindir. Dizininizi bu tarihten önce oluşturulduysa aşağıdaki adımları uygulayarak dizininiz için modern kimlik doğrulaması el ile etkinleştirmeniz gerekir:

1. Dizin zaten modern kimlik doğrulaması çalıştırarak destekleyip desteklemediğini görmek için onay `Get-CsOAuthConfiguration` gelen [Skype Kurumsal çevrimiçi PowerShell Modülü](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Komutunuz boş döndürürse `OAuthServers` özelliği ve Modern kimlik doğrulaması devre dışı bırakıldı. Modern kimlik doğrulaması kullanmak üzere ayarını güncelleştirme `Set-CsOAuthConfiguration`. Varsa, `OAuthServers` özelliği, bir giriş içerir, şimdi hazırsınız.

Geçmeden önce bu adımı tamamlamak emin olun. Hangi protokolün tüm Office istemcileri tarafından kullanılacak dikte çünkü dizin yapılandırmalarınızı ilk değiştirilir önemlidir. Modern kimlik doğrulamasını destekleyen Office istemcileri kullanıyorsanız bile, eski protokolleri kullanarak dizininize modern kimlik doğrulaması devre dışı bırakılırsa varsayılacaktır.

### <a name="step-2-office-applications"></a>2. adım: Office uygulamaları

Modern kimlik doğrulaması dizininizde etkinleştirdikten sonra Office istemcileri için modern kimlik doğrulamasını etkinleştirerek uygulamaları güncelleştirme başlatabilirsiniz. Modern kimlik doğrulaması, Office 2016 veya sonraki istemciler varsayılan olarak destekler. Hiçbir ek adımlar gereklidir.

Office 2013 Windows istemcileri kullanıyorsanız veya daha eski Office 2016 veya sonraki bir sürüme yükseltmenizi öneriyoruz. Dizininizdeki modern kimlik doğrulamasının etkinleştirilmesi, önceki adımda tamamladıktan sonra bile, eski Office uygulamalarını eski kimlik doğrulama protokolleri kullanmaya devam eder. Office 2013 istemcilerindeki kullanıyorsanız ve hemen Office 2016 veya sonraki bir sürüme yükseltmek belirleyemiyoruz, aşağıdaki makaleye bağlantısındaki [etkinleştirme Modern kimlik doğrulaması için Office 2013 Windows cihazlarda](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication). Eski kimlik doğrulaması kullanırken hesabınızın korunmasına yardımcı olmak için güçlü parolalar dizininize arasında kullanmanızı öneririz. Kullanıma [Azure AD parola koruması](../authentication/concept-password-ban-bad.md) dizininize zayıf parolalarda yasaklamak için.

Office 2010, modern kimlik doğrulamasını desteklemez. Tüm kullanıcılar Office 2010 ile daha yeni bir Office sürümüne yükseltmeniz gerekir. Varsayılan olarak eski kimlik doğrulamasını engeller gibi Office 2016 veya sonraki sürümleri yükseltme yapmanızı öneririz.

MacOS kullanıyorsanız, Office Mac 2016 veya sonraki yükseltme öneririz. Yerel posta istemcisi kullanıyorsanız, MacOS sürümü 10.14 veya tüm cihazlar daha sonra gerekecektir.

### <a name="step-3-exchange-and-sharepoint"></a>3. adım: Exchange ve SharePoint

Modern kimlik doğrulaması kullanmak Windows tabanlı Outlook istemcileri için Exchange Online modern kimlik doğrulaması de etkin olması gerekir. Modern kimlik doğrulaması için devre dışı bırakılırsa Outlook Windows tabanlı istemciler modern kimlik doğrulaması (Outlook 2013 veya üzeri), temel kimlik doğrulaması için Exchange Online posta kutusu bağlanmak için kullanacağı destekleyen Exchange Online.

SharePoint Online modern kimlik doğrulaması varsayılan olarak etkinleştirilir. 1 Ağustos 2017'den sonra oluşturulan dizinleri için modern kimlik doğrulaması Exchange Online'da varsayılan olarak etkindir. Ancak, önceden modern kimlik doğrulaması devre dışı veya bu tarihten önce oluşturulan bir dizin kullanıyorsanız, aşağıdaki makaleye adımları [Exchange Online'da modern kimlik doğrulamayı etkinleştirme](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>4. Adım: Skype Kurumsal

Skype Kurumsal'a yaptığı eski bir kimlik doğrulama istekleri önlemek için modern kimlik doğrulaması için Skype Kurumsal çevrimiçi'ı etkinleştirmek gereklidir. 1 Ağustos 2017'den sonra oluşturulan dizinleri için Skype Kurumsal için modern kimlik doğrulaması varsayılan olarak etkindir.

Skype Kurumsal, modern kimlik doğrulamasını etkinleştirmek için Microsoft Teams, modern kimlik doğrulaması varsayılan olarak destekler, geçiş öneririz. Ancak, şu an için tr mümkün değilse, modern kimlik doğrulaması kullanarak Skype kurumsal iş başlatır. böylece modern kimlik doğrulaması için Skype Kurumsal çevrimiçi etkinleştirmeniz gerekir. Bu makalede açıklanan adımları [Skype için Modern kimlik doğrulaması ile desteklenen iş topolojiler](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), Skype Kurumsal için Modern kimlik doğrulaması etkinleştirme adımları için.

Modern kimlik doğrulaması için Skype Kurumsal çevrimiçi etkinleştirmeye ek olarak, modern öneririz kimlik doğrulamasının etkin olması Exchange Online için Skype Kurumsal için modern kimlik doğrulama etkinleştirilirken. Bu işlem, modern kimlik doğrulaması Exchange Online ve Skype Kurumsal çevrimiçi durumunu eşitlemek yardımcı olur ve çoklu oturum açma istemlerini Skype kurumsal iş istemciler için engeller.

### <a name="step-5-using-mobile-devices"></a>5. Adım: Mobil cihazları kullanma

Mobil Cihazınızda uygulamaları engelleme de eski kimlik doğrulaması gerekir. Outlook mobil için kullanmanızı öneririz. Outlook Mobile, modern kimlik doğrulaması varsayılan olarak destekler ve diğer MFA temel koruma ilkeleri eşleşecektir.

Yerel iOS posta istemcisi kullanabilmeniz için iOS 11.0 veya daha eski bir kimlik doğrulama engellemek için posta istemci güncelleştirildiğinden emin olmak için çalıştırıyor olması gerekir.

### <a name="step-6-on-premises-clients"></a>6. Adım: Şirket içi istemcileri

Şirket içi Exchange Server ve Skype Kurumsal şirket içi için kullanan bir karma müşteri varsa, her iki hizmet modern kimlik doğrulamasını etkinleştirmek için güncelleştirilmesi gerekir. Karma bir ortamda, modern kimlik doğrulaması kullanırken, kullanıcılar şirket içi kimlik doğrulaması yine de. Kendi kaynaklarını (dosyalar veya e-postalar) değişiklikleri erişimi yetkilendirme dönüştürüldüğünü.

Şirket içinde modern kimlik doğrulamasını etkinleştirme başlamadan önce theIf karşıladığından emin olmanız gereksinimlerini karşılamıyorsa, şirket içinde modern kimlik doğrulamasını etkinleştirmek artık hazırsınız.

Modern kimlik doğrulamasının etkinleştirilmesi için adımlar aşağıdaki makaleler de bulunur:

* [İçi Exchange Server üzerinde karma Modern kimlik doğrulaması kullanacak şekilde yapılandırma](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Modern kimlik doğrulaması (ADAL) işletme için Skype ile kullanma](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="enable-the-baseline-policy"></a>Temel ilke etkinleştir

İlke **temel ilke: Blok eski kimlik doğrulama (Önizleme)** önceden yapılandırılmış olarak gelir ve Azure portalında koşullu erişim dikey penceresine gittiğinizde en üstünde gösterilir.

Bu ilkeyi etkinleştirmek ve kuruluşunuzu korumak için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde seçin **temel ilke: Blok eski kimlik doğrulama (Önizleme)** .
1. Ayarlama **ilkesini etkinleştir** için **ilkeyi hemen kullan**.
1. Herhangi bir kullanıcı özel tıklayarak Ekle **kullanıcılar** > **dışlanan kullanıcılar seçin** ve hariç tutulması gerektiğini kullanıcıları seçme. Tıklayın **seçin** ardından **Bitti**.
1. Tıklayın **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)
