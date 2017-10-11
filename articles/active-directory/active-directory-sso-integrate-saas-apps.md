---
title: "Azure Active Directory çoklu oturum açma SaaS uygulamaları ile tümleştirme | Microsoft Docs"
description: "Çoklu oturum açma kimlik doğrulaması ve merkezi erişim yönetimi Azure Active Directory'de SaaS uygulamaları için sağlama kullanıcı etkinleştirin. SaaS uygulamaları için Azure Active Directory Tümleştirme genel bakış."
services: active-directory
keywords: "Azure AD SaaS uygulamaları ile tümleştirme"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 53b9d341-d1fc-4bbb-ac7c-3f4c68fcf00a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: aaronsm
ms.openlocfilehash: fc0d297598c334ca8f6f8a2bd3ae948c87956342
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Azure Active Directory çoklu oturum açma SaaS uygulamaları ile tümleştirme
> [!div class="op_single_selector"]
> * [Azure portal](active-directory-enterprise-apps-manage-sso.md)
> * [Klasik Azure Portalı](active-directory-sso-integrate-saas-apps.md)
>
>

[!INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Kuruluşunuzun getiren bir uygulama için çoklu oturum açmayı ayarlama başlamak için varolan bir dizin Azure Active Directory (Azure AD) kullanacaksınız. Microsoft Azure, Office 365 veya Windows Intune elde bir Azure AD dizini kullanabilirsiniz. İki veya daha fazlası varsa, bkz: [Azure AD dizininizi yönetme](active-directory-administer.md) hangisinin kullanılacağını belirlemek için.

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor. Azure AD Yönetim Merkezi'nden yönetici rolleri atamak için bkz: nasıl [Azure Active Directory'de yönetici rolleri atama](active-directory-enterprise-apps-manage-sso.md).

## <a name="authentication"></a>Kimlik Doğrulaması
WS-Federation, SAML 2.0 veya protokolleri, Openıd Connect imzalama sertifikalarının güven ilişkileri kurmak için Azure Active Directory kullanan destekleyen uygulamalar için. Bu konu hakkında daha fazla bilgi için bkz: [için sertifikaları yönetme federe çoklu oturum açma](active-directory-sso-certs.md).

Yalnızca HTML form tabanlı oturum açma destekleyen uygulamalar için 'parola kasası oluşturma' Azure Active Directory kullanan güven ilişkileri kurmak için. Bu SaaS uygulamasının kullanıcı hesabı bilgilerini kullanarak Azure AD tarafından bir SaaS uygulaması için otomatik olarak oturum açmanız ve kuruluşunuzdaki kullanıcıların sağlar. Azure AD toplar ve kullanıcı hesabı bilgilerini ve ilişkili parolayı güvenli bir şekilde depolar. Daha fazla bilgi için bkz: [parola tabanlı çoklu oturum açma](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Yetkilendirme
Sağlanan hesap kullanıcının çoklu oturum açma doğrulandıktan sonra bir uygulama kullanma yetkisi olanak sağlar. Kullanıcı sağlamayı el ile veya ekleyin ve Azure Active Directory'de yapılan değişikliklere göre SaaS uygulama kullanıcı bilgilerini kaldırmak, bazı durumlarda yapılabilir. Otomatik sağlama için var olan Azure AD bağlayıcılarını kullanma hakkında daha fazla bilgi için bkz: [otomatik kullanıcı sağlamayı ve SaaS uygulamaları için sağlamayı kaldırma özelliklerini](active-directory-saas-app-provisioning.md).

Aksi takdirde, el ile kullanıcı bilgilerini bir uygulamaya ekleyebilir, veya markette kullanılabilir diğer sağlama çözümleri kullanır.

## <a name="access"></a>Access
Azure AD kuruluşunuzdaki son kullanıcılar uygulamaları dağıtmak için özelleştirilebilir çeşitli yollar sağlar. Herhangi bir belirli dağıtım veya erişim çözümü kilitli değil. Kullanabileceğiniz [gereksinimlerinize en uygun çözümü](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Ek hususlar uygulamalar zaten kullanımda
Kuruluşunuz zaten kullanan bir uygulama için tek kaydolma ayarı, yeni bir uygulama için yeni hesaplar oluşturma işlemi farklı bir işlemdir. İlk adımlar da dahil olmak üzere birkaç vardır: Azure AD kimlik uygulamaya kullanıcı kimliklerini eşleme ve uygulama ile tümleştirildikten sonra oturum açmayı kullanıcıların nasıl yaşar anlama.

> [!NOTE]
> Var olan bir uygulama için SSO'yu ayarlamak için hem Azure AD genel yönetici haklarına sahip olmanız gerekir ve SaaS uygulaması.
>
>

### <a name="mapping-user-accounts"></a>Kullanıcı hesapları eşleme
Bir kullanıcının kimliğini genellikle bir e-posta adresi veya kullanıcı asıl adı (UPN) benzersiz bir tanımlayıcı vardır. Bağlantı (map) ihtiyacınız olacak her bir kullanıcının uygulama kimliğine kendi ilgili Azure AD kimlik. Çeşitli şekillerde nasıl bağlı olarak bunu vardır, uygulama kimlik doğrulama gereksinimi.

Azure AD kimliklerle uygulama kimlikleri eşleme hakkında daha fazla bilgi için bkz: [SAML belirtecinde verilen talepler özelleştiriliyor](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) ve [sağlama öznitelik eşlemelerini özelleştirme](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Kullanıcının oturum açma deneyimi anlama
Zaten bir uygulama için SSO'i tümleştirdiğinizde kullanıcı deneyimini etkilenecek hayata geçirmek önemlidir. Tüm uygulamalar için kullanıcıların oturum açmak için Azure AD kimlik bilgilerini kullanarak başlatın. Ayrıca, kullanıcılar uygulamaya erişmek için farklı bir portal kullanmanız gerekir olabilir.

SSO için bazı uygulamalar, uygulamanın oturum arabiriminde yapılabilir, ancak diğer uygulamalar için Yönetim Portalı aracılığıyla gibi gitmek kullanıcı gerekir ([My uygulamaları](http://myapps.microsoft.com) veya [Office365](http://portal.office.com/myapps)) oturum açmak için. SSO ve kullanıcı deneyimlerini farklı türleri hakkında daha fazla bilgi [uygulama erişimi ve çoklu oturum açma Azure Active Directory ile nedir](active-directory-appssoaccess-whatis.md).

Başka bir değerli kaynaktır *kullanıcı izni gizleme* içinde [Guiding geliştiriciler](active-directory-applications-guiding-developers-for-lob-applications.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar
Çok sayıda Azure AD uygulama galerisinde Bul SaaS uygulamaları için sağlar [öğreticileri SaaS uygulamalarını tümleştirme ile nasıl](active-directory-saas-tutorial-list.md).

Uygulama uygulama galerisinde değilse, yapabilecekleriniz [özel bir uygulama olarak Azure AD uygulama galerisinde eklemek](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Tüm itibaren bu sorunların Azure.com Kitaplığı'nda daha fazla ayrıntı yok [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md).

Ayrıca, kaçırmayın [Azure Active Directory'de uygulama yönetimi için makale dizini](active-directory-apps-index.md).
