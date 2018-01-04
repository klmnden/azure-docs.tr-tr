---
title: "Azure AD uygulamaları ile tümleştirme kullanmaya başlama | Microsoft Docs"
description: "Bu makalede, Azure Active Directory (AD) şirket içi uygulamaları ve bulut uygulamaları ile tümleştirmek için bir başlangıç kılavuzuna getirilmiştir."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: asteen
ms.openlocfilehash: ffbc8af54ce542630f9bdc77a8823d3c2a22afd6
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory alma uygulamaları ile tümleştirme Kılavuzu
## <a name="overview"></a>Genel Bakış
Bu konuda, Azure Active Directory (AD ile) uygulamaları tümleştirmek için bir yol haritası sağlamak üzere tasarlanmıştır. Aşağıdaki bölümler her içeren daha ayrıntılı bir konu kısa bir özeti bu başlangıç kılavuzuna hangi kısımlarının sizinle ilgili tanımlayabilir.  Her konu üzerinde daha ayrıntılı bilgi edinmek için bağlantıları izleyin.

## <a name="before-you-begin-take-inventory"></a>Başlamadan önce envanteri
Uygulamaları Azure AD ile tümleştirmek için hemen önce nerede olduğunuza ve gitmek istediğiniz yere bilmeniz önemlidir.  Aşağıdaki sorular, Azure AD uygulama tümleştirmesi projenizi hakkında düşünmek yardımcı olmak üzere tasarlanmıştır.

### <a name="application-inventory"></a>Uygulama envanteri
* Tüm uygulamalarınızın nerede? Bunları Kime ait?
* Ne tür bir kimlik doğrulama uygulamalarınızı gerektiriyor mu?
* Kimin hangi uygulamalara erişimi gerekiyor?
* Yeni bir uygulama dağıtmak istiyor musunuz?
  * Şirket içi derleme ve bir Azure işlem örneğinde dağıtma?
  * Azure uygulama galerisinde kullanılabilir olan bir kullanacaksınız?

### <a name="user-and-group-inventory"></a>Kullanıcı ve Grup envanteri
* Kullanıcı hesaplarınızı bulunduğu?
  * Şirket içi Active Directory
  * Azure AD
  * Sahip olduğunuz bir ayrı uygulama veritabanı içinde
  * Tasdik edilmemiş uygulamalar
  * Yukarıdakilerin tümü
* Hangi izinleri ve rol atamalarını bireysel kullanıcılar şu anda var mı? Erişimleri gözden geçirmek gerekiyor mu yoksa, kullanıcı erişimi ve rol atamalarını uygun olduğundan emin misiniz?
* Grupları, şirket içi Active Directory'de zaten oluşturulmuş?
  * Gruplarınızı nasıl düzenlenir?
  * Grup üyeleri kim?
  * Hangi izinleri/rol atamalarını gruplar şu anda var mı?
* Tümleştirme önce kullanıcı/Grup veritabanlarını temizleme gerekecek mi?  (Bu oldukça önemli bir soru içindir. Çöp girişi, atık.)

### <a name="access-management-inventory"></a>Erişim Yönetimi envanteri
* Şu anda uygulamalara kullanıcı erişimi yönetme? Değiştirmeniz gerekiyor mu?  Erişim gibi ile yönetmenin başka yolları göz önünde bulundurduğunuzdan [RBAC](role-based-access-control-configure.md) Örneğin?
* Kimin ne erişmesi gerekiyor?

Bu soruların yanıtlarını Önden belki yok ancak Tamam olmasıdır.  Bu kılavuz, bu sorulara yanıt ve bazı bilinçli kararlar almanıza yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure aboneliği ve bir Azure Active Directory dizin.  Bir Azure aboneliğiniz yoksa, ücretsiz 30 gün boyunca Azure deneyebilirsiniz. [Deneyin!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Azure AD ile uygulama tümleştirmesi
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Cloud App Discovery ile bulut uygulamaları tasdik bulma
Yukarıda belirtildiği gibi şimdiye kadar kuruluşunuz tarafından yönetilen henüz uygulamalar olabilir.  Envanteri işleminin bir parçası olarak, tasdik edilmemiş bulut uygulamalarını bulmak mümkündür. Bkz: [Cloud App Discovery tasdik edilmemiş bulut uygulamalarıyla bulma](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Kimlik doğrulama türleri
Uygulamalarınızın her birinde farklı kimlik doğrulama gereksinimleri olabilir. Azure AD ile imzalama sertifikalarının SAML 2.0, WS-Federasyon, veya Openıd Connect protokollerinin yanı parola çoklu oturum açma kullanan uygulamalar ile kullanılabilir. Uygulama hakkında daha fazla bilgi için bkz: Azure AD ile kullanmak için kimlik doğrulama türleri [sertifikaların yönetilmesi için Federasyon çoklu oturum açma Azure Active Directory'de](active-directory-sso-certs.md) ve [parola temel çoklu oturum açma](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure AD uygulama proxy'si ile SSO etkinleştirme
Microsoft Azure AD uygulama proxy'si ile özel ağınızda güvenli bir şekilde, her yerden ve herhangi bir cihazda bulunan uygulamalara erişim sağlayabilir. Ortamınızda bir uygulama proxy Bağlayıcısı yükledikten sonra Azure AD ile kolayca yapılandırılabilir.

### <a name="integrating-applications-with-azure-ad"></a>Uygulamaları Azure AD ile tümleştirme
Aşağıdaki makaleler uygulamaları Azure AD ile tümleştirmek ve bazı kılavuzluk farklı şekilde ele alır.

* [Hangi Active Directory belirleme](active-directory-administer.md)
* [Azure uygulama galerisinde uygulamaları kullanma](active-directory-appssoaccess-whatis.md)
* [SaaS uygulamaları öğreticiler listesi tümleştirme](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Uygulamalara erişimi yönetme
Aşağıdaki makaleler Azure AD bağlayıcıları kullanarak Azure AD ve Azure AD ile tümleşik olan sonra uygulamalara erişimi yönetebilirsiniz yolları açıklanmaktadır.

* [Azure AD kullanarak uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md)
* [Azure AD Bağlayıcılarla otomatikleştirme](active-directory-saas-app-provisioning.md)
* [Uygulamaya kullanıcı atama](active-directory-applications-guiding-developers-assigning-users.md)
* [Uygulamaya grup atama](active-directory-applications-guiding-developers-assigning-groups.md)
* [Hesapları paylaşma](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Özel uygulamaları tümleştirme
Yeni uygulama ve istediğiniz Azure AD, güç yararlanarak geliştiricilere yardımcı olmak için bkz: yazıyorsanız [Guiding geliştiriciler](active-directory-applications-guiding-developers-for-lob-applications.md).

Özel uygulamanızı Azure uygulama Galerisi eklemek istiyorsanız, bkz: ["kendi uygulamanızı Azure AD Self Servis SAML yapılandırmayla getir"](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

