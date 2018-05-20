---
title: Azure AD kullanarak hesapları paylaşma | Microsoft Docs
description: Nasıl Azure Active Directory hesapları şirket içi uygulamalar ve tüketici bulut Hizmetleri için güvenli bir şekilde paylaşmak kuruluşların açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/13/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: b97ec4ffacead7630c267284f79f954ef03eff61
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="sharing-accounts-with-azure-ad"></a>Azure AD ile hesapları paylaşma
## <a name="overview"></a>Genel Bakış
Bazen kuruluşlar için genellikle iki durumda olur birden çok kişinin tek kullanıcı adı ve parola kullanmanız gerekebilir:

* Her kullanıcı için bir benzersiz oturum açma ve parola gerektiren uygulamalar erişirken olup şirket içi uygulamalar veya tüketici bulut hizmetlerini (örneğin, şirket sosyal medya hesapları).
* Çok kullanıcılı ortamlar oluştururken. Kurulum, yönetim ve kurtarma etkinliklerini çekirdek için kullanılan ve ayrıcalıkları yükselten tek, yerel bir hesap olabilir. Örneğin, Office 365 veya Salesforce kök hesabı için yerel "Genel yönetici" hesap.

Geleneksel olarak, bu hesapların kimlik (kullanıcı adı ve parola) doğru kişilere dağıtma veya birden çok güvenilen aracı bunları erişebildiği paylaşılan bir konumda depolamak tarafından paylaşılır.

Geleneksel paylaşım modelinin birkaç sakıncaları vardır:

* Yeni uygulamalarına erişimini etkinleştirme erişmesi gereken kimlik bilgilerini herkese dağıtmak gerektirir.
* Paylaşılan her uygulamanın kendi benzersiz birden çok kimlik bilgileri kümesini hatırlamak kullanıcıların paylaşılan kimlik bilgileri kümesi gerektirebilir. Kullanıcılar birçok kimlik bilgilerini Hatırla gerektiğinde, riskli uygulamalar çözümlemelere riskini artırır. (örneğin, parolaları yazma).
* Uygulamaya kimlerin erişebileceğini bildiremez.
* Kimlerin olduğunu bildiremez *erişilen* bir uygulama.
* Uygulama erişimi kaldırmak istediğinizde, kimlik bilgilerini güncelleştirin ve bunları bu uygulamaya erişim gerektiren herkese dağıtmanız gerekir.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory hesabı paylaşımı
Azure AD, bu dezavantajları ortadan kaldıran paylaşılan hesaplarını kullanmak için yeni bir yaklaşım sağlar.

Azure AD yönetici kullanıcı erişim paneli kullanılarak ve çoklu oturum açma en iyi türünü seçmek erişebilir hangi uygulamaların bu uygulama için uygun yapılandırır. Bu türlerinden birini *çoklu oturum parola tabanlı açma*, Azure AD bu uygulama için oturum açma işlemi sırasında "Aracısı" bir tür olarak davranmasına izin verir.

Kullanıcılar kendi Kurumsal hesap ile bir kez oturum açın. Bu hesap bir düzenli olarak, masaüstü veya e-posta erişmek için kullandıkları aynıdır. Bul ve atanmış olan uygulamalara erişin. Paylaşılan hesaplarıyla, bu uygulamaların listesini herhangi bir sayıda Paylaşılan kimlik bilgileri içerebilir. Son kullanıcı, unutmayın veya bunlar kullanıyor olabilecek çeşitli hesaplar yazmak zorunda değildir.

Paylaşılan hesapları yalnızca gözetim artırmak ve kullanılabilirliği artıran, bunlar Ayrıca, güvenliği artırmak. Kullanıcıların kimlik bilgilerini kullanmak için gerekli izinlere sahip paylaşılan parola görmüyorum, ancak bunun yerine parola kimlik doğrulaması bağımsızlıklar akışının bir parçası kullanmak için izin almak. Ayrıca, bazı parola SSO uygulamaları Azure AD ile düzenli aralıklarla rollover (güncelleştirme) parolaları kullanma seçeneği sunar. Sistem, hangi hesap güvenliği artırır büyük ve karmaşık parolalar kullanır. Yönetici, kolayca vermek veya erişimi iptal hesabına erişim izni olanların ve kimin, geçmişte bir uygulamaya bilir.

Azure AD destekleyen paylaşılan hesapları herhangi Enterprise Mobility Suite (EMS), Premium veya Basic lisanslı kullanıcılar, oturum açma parolası tek uygulamalarının tüm türlerine. Önceden tümleştirilmiş uygulamaların uygulama galerisinde binlerce hesaplarını paylaşabilir ve kendi parola kimlik doğrulaması uygulamayla ekleyebilirsiniz [özel SSO uygulamaları](active-directory-enterprise-apps-manage-sso.md).

Hesap paylaşımını etkinleştir azure AD özellikleri içerir:

* [Parola çoklu oturum açma](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Oturum açma parolası tek Aracısı
* [Grup ataması](active-directory-accessmanagement-self-service-group-management.md)
* Özel parola uygulamalar
* [Uygulama kullanım Pano/raporları](active-directory-passwords-get-insights.md)
* Son kullanıcı erişimi portalları
* [Uygulama proxy'si](manage-apps/application-proxy.md)
* [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Bir hesap paylaşımı
Bir hesabı paylaşmak için Azure AD kullanmak için aktarmanız gerekir:

* Bir uygulama eklemek [uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/) veya [özel uygulama](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Uygulaması parola çoklu oturum açma (SSO) için yapılandırma
* Kullanım [grup tabanlı atama](active-directory-accessmanagement-group-saasapps.md) ve paylaşılan bir kimlik bilgisi girin seçeneğini belirleyin
* İsteğe bağlı: Facebook, Twitter ve LinkedIn, gibi bazı uygulamalarda, seçeneğini etkinleştirebilirsiniz [Azure AD parola toplama-üzerinden otomatik](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Da paylaşılan hesabınızı çok faktörlü kimlik doğrulama (MFA) ile daha güvenli hale getirebilirsiniz (hakkında daha fazla bilgi [uygulamalarını Azure AD ile güvenli hale getirme](authentication/concept-mfa-whichversion.md)) ve uygulama kullanarak erişimi olanların yönetme olanağı devredebilirsiniz [Azure AD Self Servis](active-directory-accessmanagement-self-service-group-management.md) Grup Yönetimi.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Koşullu erişim ile uygulamaları koruma](active-directory-conditional-access-azure-portal.md)
* [Self Servis Grup Yönetimi/SSAA](active-directory-accessmanagement-self-service-group-management.md)

