---
title: Hesaplar ve kimlik bilgileri - Azure Active Directory paylaşma | Microsoft Docs
description: Azure Active Directory kuruluş hesapları şirket içi uygulamalar ve tüketici bulut Hizmetleri için güvenli bir şekilde paylaşmak nasıl sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba9deb00b885dad1d69eb38d4977aafd3d80ab91
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60468030"
---
# <a name="sharing-accounts-with-azure-ad"></a>Azure AD ile hesapları paylaşma
## <a name="overview"></a>Genel Bakış
Bazen kuruluşlar genellikle iki durumda gerçekleşir birden çok kişi için tek bir kullanıcı adı ve parolası kullanmanız gerekebilir:

* Her kullanıcı için benzersiz bir oturum açma ve parola gerektiren uygulamalar erişirken olup şirket içi uygulamaları veya tüketici bulut Hizmetleri (örneğin, Kurumsal sosyal medya hesapları).
* Çok kullanıcılı ortamlar oluşturulurken. Yükseltilmiş ayrıcalıklara ve Kurulum, yönetim ve kurtarma etkinliklerini çekirdek için kullanılan tek, yerel bir hesap olabilir. Örneğin, Office 365 veya Salesforce kök hesabı için yerel "Genel yönetici" hesabı.

Geleneksel olarak, bu hesapların kimlik bilgilerini (kullanıcı adı ve parola) doğru kişilere dağıtma veya onları, birden çok güvenilen aracı bunları erişebildiği paylaşılan bir konumda depolamasını tarafından paylaşılır.

Geleneksel paylaşım modeli birkaç engelleri vardır:

* Yeni uygulama erişimini etkinleştirme erişmesi gereken kimlik bilgilerini herkese dağıtabilirsiniz gerektirir.
* Paylaşılan her uygulama kendi benzersiz bir kümeye Paylaşılan kimlik bilgileri, kullanıcıların birden fazla kimlik bilgilerini Hatırla gerek gerektirebilir. Birçok kimlik bilgilerini hatırlamanız kullanıcılar varsa, bunlar için riskli uygulamalar başvurmadan riskini artırır. (örneğin, parola yazmak).
* Bir uygulamaya kimlerin erişebildiğini bildiremez.
* Kimlerin bildiremez *erişilen* bir uygulama.
* Bir uygulamaya erişimini kaldırmak istediğinizde, kimlik bilgilerini güncelleştirin ve herkesin bu uygulamaya erişmesi için bunları yeniden dağıtmanız gerekir.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory hesabı paylaşma
Azure AD, bu engelleri ortadan kaldıran için paylaşılan hesaplarını kullanarak yeni bir yaklaşım sağlar.

Azure AD Yöneticisi, bir kullanıcının erişim panelini kullanarak ve çoklu oturum açma en iyi türünü seçmek erişebildiği hangi uygulamaların bu uygulama için uygun yapılandırır. Bu türlerinden birini *parola tabanlı çoklu-oturum açma*, Azure AD bu uygulama için oturum açma işlemi sırasında bir "aracıda" türü olarak davranmasına izin verir.

Kullanıcılar kuruluş hesabını ile bir kez oturum. Bu hesabın düzenli olarak, masaüstü veya e-posta erişmek için kullandıkları aynı olduğundan. Bulma ve atanmış olan uygulamalara. Paylaşılan hesaplar ile paylaşılan kimlik bilgilerini herhangi bir sayıda bu uygulamaların listesini içerebilir. Son kullanıcı aklınızda tutun veya, kullandıkları çeşitli hesaplarına yazmak gerekmez.

Paylaşılan hesaplar yalnızca gözetim artırmak ve kullanılabilirliği artıran, bunlar da güvenliğinizi artırın. Kimlik bilgilerini kullanmak için gerekli izinlere sahip kullanıcılar, paylaşılan parola görmüyorsanız, ancak yerine parolayı bir düzenlenmiş kimlik doğrulama akışının bir parçası kullanmak için izin almak. Ayrıca, bazı parola SSO uygulamaları düzenli aralıklarla (güncelleştirme) geçişi parolaları Azure AD'ye kullanma seçeneği sunar. Sistem, büyük ve karmaşık parolalar, hesap güvenliği artırır kullanır. Administrator, kolayca vermek veya erişimi iptal bir uygulamaya kimin hesabın erişimi olduğunu ve kimlerin erişilen, geçmişte bilir.

Tüm Enterprise Mobility Suite (EMS), Premium veya Basic destekler paylaşılan hesapları Azure AD lisansına sahip kullanıcılar, tüm oturum açma parolası tek uygulama türleri arasında. Kendi parola kimlik doğrulaması uygulamayla ekleyebilirsiniz ve hesaplar için binlerce önceden tümleştirilmiş uygulamalar uygulama galerisinde paylaşabilir [özel SSO uygulamaları](../manage-apps/configure-single-sign-on-portal.md).

Hesabı paylaşmasını sağlayan azure AD özellikleri şunlardır:

* [Parola çoklu oturum açma](../manage-apps/what-is-single-sign-on.md#password-based-sso)
* Oturum açma parolası tek aracı
* [Grup ataması](groups-self-service-management.md)
* Özel parola uygulamaları
* [Uygulama kullanım Pano/raporları](../active-directory-passwords-get-insights.md)
* Son kullanıcı erişimi portalları
* [Uygulama Ara sunucusu](../manage-apps/application-proxy.md)
* [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Bir hesap paylaşma
Bir hesabı paylaşmak için Azure AD kullanmak için yapmanız:

* Uygulama ekleme [uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/) veya [özel uygulama](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/)
* Uygulama parolası çoklu oturum açma (SSO) için yapılandırma
* Kullanım [grup tabanlı atama](groups-saasapps.md) ve paylaşılan bir kimlik bilgisi girin seçeneğini belirleyin
* İsteğe bağlı: Facebook, Twitter ve LinkedIn gibi bazı uygulamalarda, seçeneğini etkinleştirebilirsiniz [Azure AD'ye otomatik parola devretme toplama](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/)

Da paylaşılan hesabınız multi-Factor Authentication (MFA) ile daha güvenli hale getirebilirsiniz (daha fazla bilgi edinin [Azure AD ile uygulamaları güvenli hale getirme](../authentication/concept-mfa-whichversion.md)) ve kullanarakuygulamayakimlerinerişebildiğiniyönetmeolanağıdevredebilirsiniz.[ Azure AD Self Servis](groups-self-service-management.md) Grup Yönetimi.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Koşullu erişim ile uygulamaları koruma](../active-directory-conditional-access-azure-portal.md)
* [Self Servis Grup Yönetimi/SSAA](groups-self-service-management.md)
