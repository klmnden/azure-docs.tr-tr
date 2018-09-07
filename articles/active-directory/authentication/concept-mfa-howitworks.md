---
title: Azure multi-Factor Authentication - nasıl çalışır?
description: Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: a79c09df7d5fec5884a6810b82b7c174042a4c93
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44054572"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl çalıştığını: Azure multi-Factor Authentication

İki aşamalı doğrulama, güvenlik, katmanlı bir yaklaşım arasındadır. Birden çok kimlik doğrulama faktörleri ödün saldırganlar için önemli bir sınama gösterir. Bir saldırgan kullanıcı parolasının öğrenmek yönetir olsa bile, bu da ek kimlik doğrulama yöntemi olarak sahip zorunda kalmadan kullanışsızdır. İki veya daha fazla aşağıdaki kimlik doğrulama yöntemlerini isteyerek çalışır:

* Bir şey (genellikle parola) bildirin
* (Kolayca, bir telefon gibi kopyalaması değil güvenilir bir cihaz) sahip olduğunuz şey
* Bir şey (Biyometri) olan

<center>![Kavramsal kimlik doğrulama yöntemleri görüntüsü](./media/concept-mfa-howitworks/methods.png)</center>

Azure multi-Factor Authentication (MFA) erişimi korumaya yardımcı olur ve kolaylık olması için kullanıcıların sürdürürken uygulamalarınıza. İkinci bir form kimlik doğrulaması gerektirerek ek güvenlik sağlar ve kullanımı kolay bir dizi aracılığıyla güçlü kimlik doğrulaması sağladığını [kimlik doğrulama yöntemleri](concept-authentication-methods.md).

## <a name="how-to-get-multi-factor-authentication"></a>Multi-Factor Authentication'ı almak nasıl?

Çok faktörlü kimlik doğrulaması aşağıdaki tekliflere bir parçası olarak sunulur:

* **Azure Active Directory Premium lisansları** -tam özellikli Azure multi-Factor Authentication hizmetinin (bulut) ya da Azure multi-Factor Authentication sunucusu (şirket içi) kullanın.
   * **Azure MFA hizmeti (bulut)** - **bu seçenek yeni dağıtımlar için önerilen yoldur**. Bulutta Azure MFA, herhangi bir şirket içi altyapı gerektirir ve şirket dışı veya salt bulut kullanıcılarınız ile kullanılabilir.
   * **Azure MFA sunucusu** -kuruluşunuz ilişkili altyapı öğelerini yönetmek istiyorsa ve AD FS şirket içi ortamınızda dağıtılmış bu şekilde bir seçenek olabilir.
* **Office 365 için multi-Factor Authentication** -Azure multi-Factor Authentication yeteneklerinin bir alt kümesini, aboneliğinizin bir parçası olarak kullanılabilir. Office 365 için MFA hakkında daha fazla bilgi için bkz [planlama Office 365 dağıtımlar için çok faktörlü kimlik doğrulaması](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Azure Active Directory küresel yöneticileri** -Azure multi-Factor Authentication yeteneklerinin bir alt kümesini genel yönetici hesapları korumak için bir araç olarak kullanılabilir.

> [!NOTE]
> Yeni müşteriler artık etkin 1 Eylül Mayıs 2018 sunan bir tek başına olarak Azure multi-Factor Authentication satın alabilirsiniz. Çok faktörlü kimlik doğrulaması, Azure AD Premium lisansınız kullanılabilir bir özellik olmaya devam edecektir.

### <a name="auth-provider-or-mfa-license"></a>Kimlik doğrulama sağlayıcısı veya MFA lisans

Azure AD Premium veya Azure AD Premium içeren başka bir lisans paketini varsa, Azure mfa'yı zaten sahip. Kuruluşunuz, tüm kullanıcılar için iki aşamalı doğrulama özelliğini genişletmek için ek herhangi bir şey gerekmez. Yalnızca bir kullanıcıya bir lisans ataması gerekir ve ardından MFA'yı etkinleştirebilirsiniz.

Azure mfa'yı içerir veya oluşturabileceğiniz daha sonra tüm kullanıcılarınızı kapsamaya yetecek sayıda lisansınız yoksa lisans yoksa bir [MFA kimlik doğrulama sağlayıcısı](concept-mfa-authprovider.md) bunları isteyen kullanıcılar için MFA'ın tüm özelliklerini genişletmek için.

> [!IMPORTANT]
> Tüm kullanıcılar için yeterince lisansa sahip değilseniz, bir kullanıcı başına çok faktörlü yetki Sağlayıcı'nın kuruluşunuzun rest kapsayacak şekilde oluşturabilirsiniz. Kimlik doğrulaması başına multi-Factor Auth sağlayıcısı oluşturmayın. Bunu yaparsanız, doğrulama istekleri zaten lisanslara sahip kullanıcılardan ödeme son.

## <a name="supportability"></a>Desteklenebilirlik

Çoğu kullanıcı kimlik doğrulaması için yalnızca parola kullanmaya alışkın olduğundan, kuruluşunuz bu işlem ile ilgili tüm kullanıcılara iletişim kuran önemlidir. Farkındalık, kullanıcılar için mfa'yı ilgili önemsiz sorunlar için Yardım Masasını arayın. olasılığını azaltabilirsiniz. Ancak, geçici olarak mfa'yı devre dışı bırakma gerekli olduğu bazı senaryolar vardır. Bu senaryoları nasıl ele alınacağını anlamak için aşağıdaki yönergeleri kullanın:

* Burada kullanıcı, kendi kimlik doğrulama yöntemlerini erişimi olmadığı veya düzgün çalışmayan olduğundan oturum açamaz senaryoları işlemek için destek ekibinize eğitin.
   * Koşullu erişim ilkeleri için Azure MFA hizmetini kullanarak, destek ekibinize, MFA gerektirme ilkesinden hariç tutulan bir gruba kullanıcı ekleyebilirsiniz.
   * Destek personeli, iki aşamalı doğrulama kimlik doğrulaması yapmalarına izin vermek Azure MFA sunucusu kullanıcı için geçici bir kerelik geçiş etkinleştirebilirsiniz. Geçiş geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar.
   * Azure MFA hizmeti için koşullu erişim ilkelerini kullanarak destek ekibinize, MFA gerektirme ilkesinden hariç tutulan bir gruba kullanıcı ekleyebilirsiniz.
* Güvenilen IP'ler veya adlandırılmış konumlar iki aşamalı doğrulama istekleri en aza indirmek için bir yol kullanmayı düşünün. Bu özellik, yönetilen ya da Federasyon Kiracı yöneticileri, kuruluşlarının intraneti gibi bir güvenilen ağa konumdan oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilirsiniz.
* Dağıtma [Azure AD kimlik koruması](../active-directory-identityprotection.md) ve risk etkinliklere göre iki aşamalı doğrulamayı tetikler.

## <a name="next-steps"></a>Sonraki adımlar

- Adım adım bir mfa'yı edinme [dağıtım planı](https://aka.ms/MFADeploymentPlan)

- Hakkındaki ayrıntıları bulabilirsiniz [kullanıcılarınıza lisans](concept-mfa-licensing.md)

- Hakkında ayrıntılı bilgi edinin [dağıtmak için hangi sürümü](concept-mfa-whichversion.md)

- Bul yanıtlar [sık sorulan sorular](multi-factor-authentication-faq.md)
