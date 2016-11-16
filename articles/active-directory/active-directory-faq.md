---
title: Azure Active Directory ile ilgili SSS | Microsoft Belgeleri
description: "Azure ve Azure Active Directory erişimine, parola yönetimine ve uygulama erişimine ilişkin sorulara yanıtlar sağlayan Azure Active Directory ile ilgili SSS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: markusvi
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0f7070d9d691e2471978a2986025ebfdafbeaa7c


---
# <a name="azure-active-directory-faq"></a>Azure Active Directory ile ilgili SSS
Azure Active Directory kimlik, erişim yönetimi ve güvenliği tüm yönleriyle kapsayan bir Identity as a Service (IDaaS) çözümüdür.

Daha ayrıntılı bilgi için bkz. [Azure Active Directory Nedir?](active-directory-whatis.md).

## <a name="accessing-azure-and-azure-active-directory"></a>Azure ve Azure Active Directory Erişimi
**S: Klasik Azure portalında (https://manage.windowsazure.com) Azure AD'ye erişmeye çalıştığımda neden "Abonelik bulunamadı" yanıtını alıyorum?**

**Y:** Klasik Azure portalına erişebilmek için tüm kullanıcıların bir Azure aboneliğine yönelik izinlere sahip olması gerekir. Ücretli Office 365'e veya Azure AD'ye sahipseniz bir kerelik etkinleştirme adımı için [http://aka.ms/accessAAD](http://aka.ms/accessAAD) adresine gidin; aksi halde tam [Azure deneme sürümünü](https://azure.microsoft.com/pricing/free-trial/) veya ücretli aboneliği etkinleştirmeniz gerekir. 

Daha ayrıntılı bilgi için bkz.

* [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)
* [Azure'da Office 365 aboneliğinize yönelik dizini yönetme](active-directory-manage-o365-subscription.md)

- - -
**S: Azure AD, Office 365 ve Azure arasında ne gibi bir ilişki söz konusudur?**

**Y:** Azure Active Directory, tüm Microsoft çevrimiçi hizmetlerine yönelik ortak kimlik ve erişim işlevleri sunar. Office 365, Microsoft Azure, Intune veya diğer uygulamalardan hangisini kullanırsanız kullanın bu hizmetlerin tümü için oturum açma ve erişim yönetimini etkinleştirmek üzere zaten Azure AD'yi kullanırsınız. 

Aslında, Microsoft Çevrimiçi hizmetleri için etkinleştirdiğiniz tüm kullanıcılar, bir veya daha fazla Azure AD örneğinde kullanıcı hesapları olarak tanımlanır. Bu hesapları, bulut uygulama erişimi gibi ücretsiz Azure AD işlevlerinden faydalanmak üzere etkinleştirebilirsiniz.

Ayrıca, Azure AD ücretli hizmetleri (ör. Azure AD temel, Premium, EMS vb.), kurumsal ölçekteki kapsamlı yönetim ve güvenlik çözümleri ile Office 365 ve Microsoft Azure gibi diğer Çevrimiçi hizmetler için tamamlayıcı niteliktedir.

- - -
## <a name="getting-started-with-hybrid-azure-ad"></a>Karma Azure AD ile çalışmaya başlama
**S: Şirket içi dizinimi Azure AD'ye nasıl bağlayabilirim?**

**Y:** Şirket içi dizininizi Azure AD'ye bağlamak için **Azure AD Connect**'i kullanabilirsiniz. 

Daha ayrıntılı bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

- - -
**S: Şirket içi dizinim ve bulut uygulamalarım arasında SSO'yu nasıl ayarlarım?**

**Y:** SSO'yu yalnızca şirket içi dizininiz ve Azure AD arasında ayarlamanız yeterlidir. Bulut uygulamalarınıza Azure AD üzerinden eriştiğiniz sürece, hizmet otomatik olarak kullanıcılarınızı şirket içi kimlik bilgileriyle doğru şekilde kimlik doğrulaması gerçekleştirmeye yönlendirir.

Şirket içi konumdan SSO uygulama işlemi, ADFS gibi federasyon çözümleri veya parola karma eşitlemesini yapılandırma işlemi ile kolayca gerçekleştirilebilir. Azure AD Connect yapılandırma sihirbazını kullanarak her iki seçeneği de kolayca dağıtabilirsiniz.

Daha ayrıntılı bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

- - -
**S: Azure Active Directory, kuruluşumdaki kullanıcılar için bir self servis portal sağlar mı?**

**Y:** Evet, Azure Active Directory size kullanıcı self servis ve uygulama erişimi için [Azure AD Erişim Paneli](http://myapps.microsoft.com)'ni sağlar. Office 365 müşterisiyseniz aynı işlevlerin birçoğunu Office 365 portalında da bulabilirsiniz. 

Daha fazla bilgi için bkz. [Erişim Paneli'ne Giriş](active-directory-saas-access-panel-introduction.md). 

- - -
**S: Azure AD, şirket içi altyapımı yönetmeme yardımcı olur mu?**

**Y:** Evet. Azure AD Premium sürümü size **Connect Health** olanağını sağlar. Azure AD Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur.  

Daha ayrıntılı bilgi için bkz. [Bulutta şirket içi kimlik altyapınızı ve eşitleme hizmetlerinizi izleme](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Parola yönetimi
**S: Azure AD parola geri yazma özelliğini parola eşitleme olmadan kullanabilir miyim? (Başka bir deyişle, Azure AD SSPR'yi parola geri yazma özelliğiyle kullanmak istiyorum ancak parolalarımın bulutta depolanmasını istemiyorum.)**

**Y:** Geri yazmayı etkinleştirmek için AD parolalarınızı Azure AD ile eşitlemeniz gerekmez. Birleştirilmiş bir ortamda Azure AD SSO, kullanıcının kimliğini doğrulamak için şirket içi dizini kullanır. Bu senaryoda, şirket içi parolanın Azure AD'de izlenmesi gerekmez.

- - -
**S: Bir parolanın AD şirket içi konumuna geri yazılması ne kadar zaman alır?**

**Y:** Parola geri yazma işlemi gerçek zamanlı olarak gerçekleşir. 

Daha ayrıntılı bilgi için bkz. [Parola Yönetimine Başlarken](active-directory-passwords-getting-started.md) 

- - -
**S: Parola geri yazma özelliğini bir yönetici tarafından yönetilen parolalarla kullanabilir miyim?**

**Y:** Evet, parola geri yazma özelliğini etkinleştirdiyseniz bir yönetici tarafından gerçekleştirilen parola işlemleri, şirket içi ortamınıza geri yazılır.  

Parola ile ilgili daha fazla soru yanıtı için bkz. [Parola Yönetimi ile ilgili Sık Sorulan Sorular](active-directory-passwords-faq.md).

- - -
## <a name="application-access"></a>Uygulama erişimi
**S: Azure AD ile önceden tümleştirilmiş olan uygulamaların ve özelliklerinin listesini nereden bulabilirim?**

**Y:** Azure AD, Microsoft'a, uygulama hizmeti sağlayıcılarına veya iş ortaklarına ait 2600'ü aşkın önceden tümleştirilmiş uygulama içerir. Önceden tümleştirilmiş tüm uygulamalar SSO'yu destekler. SSO, uygulamalarınıza erişmek için kurumsal kimlik bilgilerinizi kullanmanıza olanak tanır. Uygulamalardan bazıları aynı zamanda otomatik hazırlama ve sağlamayı kaldırma özelliklerini destekler

Önceden tümleştirilmiş uygulamaların tam listesi için bkz. [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**S: İhtiyacım olan uygulama Azure AD marketinde yoksa ne olur?**

**Y:** Azure AD Premium ile istediğiniz uygulamayı ekleyip yapılandırabilirsiniz. Uygulamanızın özelliklerine ve tercihlerinize bağlı olarak, SSO'yu ve otomatik hazırlamayı yapılandırabilirsiniz.  

Daha ayrıntılı bilgi için bkz.

* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](active-directory-saas-custom-apps.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](active-directory-scim-provisioning.md) 

- - -
**S: Kullanıcılar Azure Active Directory'yi kullanarak uygulamalarda nasıl oturum açar?**

**Y:** Azure Active Directory, kullanıcılara uygulamalarını görüntülemeleri ve bunlara erişmeleri için birçok yol sağlar. Bunlardan bazıları aşağıda verilmiştir:

* Azure AD erişim paneli
* Office 365 uygulama başlatıcı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Daha fazla bilgi için bkz. [Azure AD tümleşik uygulamalarını kullanıcılara dağıtma](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**S: Azure Active Directory, uygulamalar için kimlik doğrulaması ve çoklu oturum açmaya ne gibi farklı yollarla olanak tanır?**

**Y:** Azure Active Directory; SAML 2.0, OpenID Connect, OAuth 2.0 ve WS-Federasyon gibi birçok standartlaştırılmış kimlik doğrulaması ve yetkilendirme protokolünü destekler. Azure AD aynı zamanda, yalnızca form tabanlı kimlik doğrulamasını destekleyen uygulamalar için parola kasası oluşturma ve otomatik oturum açma işlevlerini de destekler.  

Daha fazla bilgi için bkz.

* [Azure AD için Kimlik Doğrulama Senaryoları](active-directory-authentication-scenarios.md)
* [Active Directory Kimlik Doğrulama Protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Çoklu oturum açma özelliği, Azure Active Directory ile nasıl kullanılır?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**S: Şirket içi olarak çalıştırdığım uygulamaları ekleyebilir miyim?**

**Y:** Azure AD Uygulama Ara Sunucusu, size seçtiğiniz şirket içi web uygulamaları için kolay ve güvenli erişim sağlar. Bu uygulamalara, Azure Active Directory'de SaaS uygulamalarınıza eriştiğiniz gibi erişebilirsiniz. VPN kullanmanız veya ağ altyapınızı değiştirmeniz gerekmez.  

Daha ayrıntılı bilgi için bkz. [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md).

- - -
**S: Belirli bir uygulamaya erişen kullanıcılar için MFA'yı nasıl gerekli kılarım?**

**Y:** Azure AD koşullu erişimi ile her bir uygulama için benzersiz bir erişim ilkesi atayabilirsiniz. İlkenizde, MFA'yı her zaman veya kullanıcılar yerel ağa bağlı olmadığında gerekli kılabilirsiniz.  

Daha ayrıntılı bilgi için bkz. [Office 365'e ve Azure Active Directory'ye bağlı diğer uygulamalara erişimi güvence altına alma](active-directory-conditional-access.md).

- - -
**S: SaaS Uygulamaları için Otomatik Kullanıcı Hazırlama nedir?**

**Y:** Azure Active Directory, birçok popüler bulut (SaaS) uygulamasında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmenize olanak tanır. 

Daha fazla bilgi için bkz. [Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](active-directory-saas-app-provisioning.md)

- - -



<!--HONumber=Nov16_HO2-->


