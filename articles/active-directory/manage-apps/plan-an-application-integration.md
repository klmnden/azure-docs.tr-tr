---
title: Azure AD ile uygulamaları tümleştirme çalışmaya başlama | Microsoft Docs
description: Bu makale, Azure Active Directory (AD) şirket içi uygulamaları ve bulut uygulamaları ile tümleştirmek için bir başlangıç kılavuzunda yöneliktir.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8db7ac63aaf9ae5b1b16bb233e87ace06867309
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65824309"
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory alma uygulamaları ile tümleştirme için Başlangıç Kılavuzu

Bu konuda, uygulamaları Azure Active Directory (AD ile) tümleştirme işlemi özetlenmektedir. Aşağıdaki bölümlerde içeren daha ayrıntılı bir konu ile ilgili kısa bir özeti bu başlangıç kılavuzunda hangi parçalarının sizinle ilgili belirleyebilmeniz.

Ayrıntılı dağıtım planları indirmek için bkz [sonraki adımlar](#next-steps).

## <a name="take-inventory"></a>Envanteri
Azure AD ile uygulamaları tümleştirme önce nerede ve gitmek istediğiniz bilmek önemlidir.  Azure AD uygulama tümleştirme projeniz hakkında düşünmek yardımcı olması için aşağıdaki soruları yöneliktir.

### <a name="application-inventory"></a>Uygulama envanteri
* Tüm uygulamalarınızı nerede? Bunları Kime ait?
* Ne tür bir kimlik doğrulama uygulamalarınız gerektiriyor mu?
* Kimlerin, hangi uygulamaların erişmesi gerekiyor?
* Yeni bir uygulama dağıtmak istiyor musunuz?
  * Şirket içi derleme ve bir Azure işlem örneğinde dağıtma?
  * Azure uygulama galerisinde mevcut olan bir kullanacak mısınız?

### <a name="user-and-group-inventory"></a>Kullanıcı ve Grup envanteri
* Kullanıcı hesaplarınızı nerede bulunur?
  * Şirket içi Active Directory
  * Azure AD
  * Sahip olduğunuz bir ayrı uygulama veritabanı içinde
  * Tasdiksiz uygulamalar
  * Yukarıdakilerin tümü
* Hangi izinleri ve rol atamalarını bireysel kullanıcılar şu anda var mı? Kullanıcıların erişim gözden geçirmek zorunda veya kullanıcı erişimi ve rol atamalarını uygun olduğundan emin misiniz?
* Grupları, şirket içi Active Directory'nizde zaten kurulur?
  * Gruplarınızı nasıl düzenlenir?
  * Grup üyelerini kim?
  * Hangi izinleri/rol atamalarını gruplar şu anda var mı?
* Kullanıcı/Grup veritabanlarını tümleştirme önce temiz gerekecek mi?  (Bu oldukça önemli bir soru değildir. Çöp içinde çöp.)

### <a name="access-management-inventory"></a>Erişim Yönetimi envanteri
* Nasıl şu anda uygulamalara kullanıcı erişimini yönetebilirsiniz? Değişmesi gerekiyor mu?  İle olduğu gibi erişimi yönetmenin başka yolları olarak kabul [RBAC](../../role-based-access-control/role-assignments-portal.md) Örneğin?
* Kimin, ne erişmesi gerekiyor?

Belki de tüm Bu soruların yanıtlarını önceden sahip değilseniz ancak Tamam olmasıdır.  Bu kılavuz, bazı bu soruları yanıtlamak ve bazı bilgiye dayalı kararlar yardımcı olabilir.

### <a name="find-unsanctioned-cloud-applications-with-cloud-discovery"></a>Cloud Discovery ile tasdiksiz bulut uygulamalarını bulun

Yukarıda belirtildiği gibi şimdiye kadar kuruluşunuz tarafından yönetilen henüz uygulamalar olabilir.  Envanteri işleminin bir parçası olarak, tasdiksiz bulut uygulamalarını bulmak mümkündür. Bkz: [Cloud Discovery'yi ayarlama](/cloud-app-security/set-up-cloud-discovery).

## <a name="integrating-applications-with-azure-ad"></a>Azure AD ile uygulamaları tümleştirme
Aşağıdaki makaleler uygulamaları Azure AD ile tümleştirmek ve bazı rehberlik farklı şekilde ele alınmıştır.

* [Hangi Active Directory belirleme](../fundamentals/active-directory-administer.md)
* [Azure uygulama galerisinde bulunan uygulamaları kullanma](what-is-single-sign-on.md)
* [SaaS uygulama öğreticileri listesi tümleştirme](../active-directory-saas-tutorial-list.md)

### <a name="authentication-types"></a>Kimlik doğrulama türleri
Uygulamalarınızın her birinde farklı kimlik doğrulama gereksinimleri olabilir. Azure AD ile imzalama sertifikalarının SAML 2.0, WS-Federation, veya Openıd Connect protokolleri hem de parola çoklu oturum açma kullanan uygulamalar ile kullanılabilir. Uygulama hakkında daha fazla bilgi için Azure AD ile kimlik doğrulaması türlerini görmek [sertifikaların yönetilmesi için Federasyon çoklu oturum açma, Azure Active Directory'de](manage-certificates-for-federated-single-sign-on.md) ve [parola tabanlı çoklu oturum açma üzerinde](what-is-single-sign-on.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure AD uygulama ara sunucusu ile SSO etkinleştirme
Microsoft Azure AD uygulama proxy'sinde özel ağınızda güvenli bir şekilde, her yerden ve herhangi bir cihazda bulunan uygulamaları erişim sağlayabilir. Ortamınızda bir uygulama ara sunucusu Bağlayıcısı'nı yükledikten sonra Azure AD ile kolayca yapılandırılabilir.

### <a name="integrating-custom-applications"></a>Özel uygulamaları tümleştirme
Yeni uygulama ve istediğiniz Azure AD power yararlanarak geliştiricilere yardımcı olmak için bkz. yazıyorsanız [Guiding geliştiriciler](../active-directory-applications-guiding-developers-for-lob-applications.md).

Azure uygulama galerisinde özel uygulamanıza eklemek istiyorsanız, bkz. ["kendi uygulamanızı Azure AD Self Servis SAML yapılandırmasıyla getir"](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
Aşağıdaki makaleler Azure AD bağlayıcıları kullanarak Azure AD ile Azure AD ile tümleşik olan sonra uygulamalara erişimi yönetebilir yolları açıklanmaktadır.

* [Azure AD kullanarak uygulamalara erişimi yönetme](what-is-access-management.md)
* [Azure AD bağlayıcıları ile otomatikleştirme](user-provisioning.md)
* [Uygulamaya kullanıcı atama](../active-directory-applications-guiding-developers-assigning-users.md)
* [Uygulamaya grup atama](../active-directory-applications-guiding-developers-assigning-groups.md)
* [Hesapları paylaşma](../active-directory-sharing-accounts.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi için Azure Active Directory Dağıtım planlarından indirebilirsiniz [GitHub](https://aka.ms/deploymentplans). Galeri uygulamalar için çoklu oturum açma, koşullu erişim ve kullanıcı aracılığıyla sağlama için dağıtım planları indirebilirsiniz [Azure portalında](https://portal.azure.com). 

Azure portalından bir dağıtım planı indirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **kurumsal uygulamalar** | **uygulama çekme** | **dağıtım planı**.

Lütfen dağıtım planlarında yararlanarak görüş [dağıtım planı anket](https://aka.ms/DeploymentPlanFeedback).
