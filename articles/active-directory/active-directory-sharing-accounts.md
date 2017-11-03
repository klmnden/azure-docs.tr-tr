---
title: "Azure AD kullanarak hesapları paylaşma | Microsoft Docs"
description: "Nasıl Azure Active Directory hesapları şirket içi uygulamalar ve tüketici bulut Hizmetleri için güvenli bir şekilde paylaşmak kuruluşların açıklar."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e2d77104-d978-46a3-bfea-03ffdf3b61e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: b5a6bad6eca3f0262d5cc2ac01fb3a4eb3676e5e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sharing-accounts-with-azure-ad"></a>Azure AD ile hesapları paylaşma
## <a name="overview"></a>Genel Bakış
Bazen kuruluşlar için birden çok kişinin tek kullanıcı adı ve parola kullanmanız gerekir. Bu genellikle iki durumlarda gerçekleşir:

* Her kullanıcı için bir benzersiz kullanıcı adı ve parolası gerektiren uygulamalar erişirken olup şirket içi uygulamalar veya tüketici bulut hizmetlerini (ör, Kurumsal sosyal medya hesapları).
* Çok kullanıcılı ortamlar oluştururken. Kurulum, yönetim ve kurtarma etkinliklerini (örneğin yerel "Genel yönetici" hesabı Office 365 için) veya kök hesabı Salesforce çekirdek için kullanılan ve ayrıcalıkları yükselten tek, yerel bir hesap olabilir.

Geleneksel olarak, bu hesapların kimlik bilgilerini (kullanıcı adı/parola) doğru kişilere dağıtma veya birden çok güvenilen aracı bunları erişebildiği paylaşılan bir konumda depolamak tarafından paylaşılan.

Geleneksel paylaşım modelinin birkaç sakıncaları vardır:

* Yeni uygulamalarına erişimini etkinleştirme erişmesi gereken kimlik bilgilerini herkese dağıtmak gerektirir.
* Paylaşılan her uygulamanın kendi benzersiz birden çok kimlik bilgileri kümesini hatırlamak kullanıcıların paylaşılan kimlik bilgileri kümesi gerektirebilir. Kullanıcılar birçok kimlik bilgilerini Hatırla gerektiğinde, riskli uygulamalar dönecek riskini artırır. (örneğin parolaları yazma).
* Uygulamaya kimlerin erişebileceğini bildiremez.
* Kimlerin olduğunu bildiremez *erişilen* bir uygulama.
* Bir uygulamaya erişim kaldırmanız gerektiğinde, kimlik bilgilerini güncelleştirin ve bunları bu uygulamaya erişim gerektiren herkese yeniden dağıtmak zorunda.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory hesabı paylaşımı
Azure AD, bu dezavantajları ortadan kaldıran paylaşılan hesaplarını kullanmak için yeni bir yaklaşım sağlar.

Azure AD yönetici kullanıcı erişim paneli kullanılarak ve çoklu oturum açma en iyi türünü seçmek erişebilir hangi uygulamaların bu uygulama için uygun yapılandırır. Bu türlerinden birini *çoklu oturum parola tabanlı açma*, Azure AD bu uygulama için oturum açma işlemi sırasında "Aracısı" bir tür olarak davranmasına izin verir.

Kullanıcılar kendi Kurumsal hesap ile bir kez oturum açın. Düzenli olarak, masaüstü veya e-posta erişmek için kullandıkları aynı hesap budur. Bul ve atanmış olan uygulamalara erişin. Paylaşılan hesaplarıyla, bu uygulamaların listesini herhangi bir sayıda Paylaşılan kimlik bilgileri içerebilir. Son kullanıcı, unutmayın veya bunlar kullanabilecek çeşitli hesaplar yazmak zorunda değildir.

Paylaşılan hesapları yalnızca gözetim artırmak ve kullanılabilirliği artıran, bunlar Ayrıca, güvenliği artırmak. Kullanıcıların kimlik bilgilerini kullanmak için gerekli izinlere sahip paylaşılan parola görmüyorum, ancak bunun yerine parola kimlik doğrulaması bağımsızlıklar akışının bir parçası kullanmak için izin almak. Ayrıca, bazı parola SSO uygulamaları, Azure AD sağlamak için seçeneğiniz düzenli aralıklarla rollover (güncelleştirme) büyük ve karmaşık parolalar, hesap güvenliği artırma kullanarak parola. Yönetici kolayca vermek veya bir uygulamaya erişimi iptal ve ayrıca hesabına kimlerin erişebileceğini ve geçmişte kimin biliyor.

Azure AD Premium ya da parola tek tüm türleri arasındaki temel lisanslı kullanıcılar, uygulamaları imzalamak herhangi Enterprise Mobility Suite (EMS için), paylaşılan hesaplarını destekler. Önceden tümleştirilmiş uygulamaların uygulama galerisinde binlerce hesaplarını paylaşabilir ve kendi parola kimlik doğrulaması uygulamayla ekleyebilirsiniz [özel SSO uygulamaları](active-directory-sso-integrate-saas-apps.md).

Hesap paylaşımını etkinleştir azure AD özellikleri içerir:

* [Parola çoklu oturum açma](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Oturum açma parolası tek Aracısı
* [Grup ataması](active-directory-accessmanagement-self-service-group-management.md)
* Özel parola uygulamalar
* [Uygulama kullanım Pano/raporları](active-directory-passwords-get-insights.md)
* Son kullanıcı erişimi portalları
* [Uygulama proxy'si](active-directory-application-proxy-get-started.md)
* [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Bir hesap paylaşımı
Azure AD için gereken bir hesabı paylaşmak üzere kullanmak için:

* Bir uygulama eklemek [uygulama Galerisi](https://azure.microsoft.com/marketplace/active-directory/) veya [özel uygulama](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
* Uygulaması parola çoklu oturum açma (SSO) için yapılandırma
* Kullanım [grup tabanlı atama](active-directory-accessmanagement-group-saasapps.md) ve paylaşılan bir kimlik bilgisi girin seçeneğini belirleyin
* İsteğe bağlı: Facebook, Twitter ve LinkedIn, gibi bazı uygulamalarda, seçeneğini etkinleştirebilirsiniz [Azure AD parola toplama-üzerinden otomatik](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Da paylaşılan hesabınızı çok faktörlü kimlik doğrulama (MFA) ile daha güvenli hale getirebilirsiniz (hakkında daha fazla bilgi [uygulamalarını Azure AD ile güvenli hale getirme](../multi-factor-authentication/multi-factor-authentication-get-started.md)) ve uygulama kullanarak erişimi olanların yönetme olanağı devredebilirsiniz [Azure AD Self Servis](active-directory-accessmanagement-self-service-group-management.md) Grup Yönetimi.

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Koşullu erişim ile uygulamaları koruma](active-directory-conditional-access.md)
* [Self Servis Grup Yönetimi/SSAA](active-directory-accessmanagement-self-service-group-management.md)

