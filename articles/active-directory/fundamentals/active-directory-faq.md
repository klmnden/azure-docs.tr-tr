---
title: Sık sorulan sorular (SSS) - Azure Active Directory | Microsoft Docs
description: Ortak hakkında sorular ve cevaplar Azure ve Azure Active Directory, parola yönetimi ve uygulama erişimi.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: lizross
ms.custom: it-pro, seodec18
ms.openlocfilehash: a3571ff24c36b7f2129f60348414d4d178ef30d1
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084103"
---
# <a name="frequently-asked-questions-about-azure-active-directory"></a>Azure Active Directory hakkında sık sorulan sorular
Azure Active Directory (Azure AD), kimlik, erişim yönetimi ve güvenliği tüm yönleriyle kapsayan bir hizmet olarak kimlik (IDaaS) çözümüdür.

Daha fazla bilgi için bkz. [Azure Active Directory nedir?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Azure ve Azure Active Directory erişimi
**S: Azure portalında Azure AD'ye erişmeye çalıştığımda neden "Abonelik bulunamadı" yanıtını alıyorum?**

**Y:** Azure portalına erişmek için her kullanıcının bir Azure aboneliğiyle birlikte izinleri olmalıdır. Ücretli bir Office 365 veya Azure AD aboneliğiniz varsa, tek seferlik etkinleştirme adımı için [https://aka.ms/accessAAD](https://aka.ms/accessAAD) sayfasına gidin. Aksi takdirde, ücretsiz bir [Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) veya ücretli aboneliği etkinleştirmeniz gerekir.

Daha fazla bilgi için bkz.

* [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)

- - -
**S: Azure AD, Office 365 ve Azure arasında ne gibi bir ilişki söz konusudur?**

**Y:** Azure AD, tüm web hizmetlerine yönelik ortak kimlik ve erişim işlevleri sunar. Office 365, Microsoft Azure, Intune veya diğer uygulamalardan hangisini kullanırsanız kullanın bu hizmetlerin tümü için oturum açma ve erişim yönetimini etkinleştirmek üzere zaten Azure AD'yi kullanırsınız.

Web hizmetlerini kullanacak şekilde ayarlanmış tüm kullanıcılar, bir veya daha fazla Azure AD örneğinde kullanıcı hesabı olarak tanımlanır. Bulut uygulama erişimi gibi ücretsiz Azure AD özellikleri için bu hesapları ayarlayabilirsiniz.

Enterprise Mobility + Security gibi ücretli Azure AD hizmetleri, kurumsal ölçekte kapsamlı yönetim ve güvenlik çözümleriyle Office 365 ve Microsoft Azure gibi diğer web hizmetlerini tamamlar.

- - -

**S: sahibi ve genel yönetici arasındaki farklar nelerdir?**

**Y:** varsayılan olarak, bir Azure aboneliği için kaydolan kişi Azure kaynakları için sahip rolü atanır. Sahibi, bir Microsoft hesabı ya da Azure aboneliğinin ilişkili olduğu dizinden bir iş veya Okul hesabı kullanabilirsiniz.  Bu rol Azure portaldaki hizmetleri yönetme yetkisine sahiptir.

Başkalarının oturum açın ve aynı aboneliği kullanarak hizmetlere erişmesi gerekiyorsa, bunları uygun atayabilirsiniz [yerleşik rol](../../role-based-access-control/built-in-roles.md). Ek bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md).

Varsayılan olarak, bir Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Genel yönetici, tüm Azure AD directory özelliklerine erişebilir. Azure AD dizin ve kimlikle ilgili özelliklerin yönetilmesine yönetici rolleri farklı bir dizi vardır. Bu Yöneticiler, Azure portalında çeşitli özelliklere erişim gerekir. Yönetici rolü, oluşturma veya kullanıcıları Düzenle, diğerlerine yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme gibi yapabileceklerini belirler.  Azure AD dizin yöneticileri ve rolleri hakkında ek bilgi için bkz: [bir kullanıcı Azure Active Directory'de yönetici rolleri atama](active-directory-users-assign-role-azure-portal.md) ve [AzureActiveDirectory'deyöneticirolleriatama](../users-groups-roles/directory-assign-admin-roles.md).

Ayrıca, Enterprise Mobility + Security gibi ücretli Azure AD hizmetleri, kurumsal ölçekte kapsamlı yönetim ve güvenlik çözümleriyle Office 365 ve Microsoft Azure gibi diğer web hizmetlerini tamamlar.

- - -
**S: Azure AD kullanıcı lisanslarımın ne zaman sona ereceğini gösteren bir rapor var mı?**

**C:** Hayır.  Bu özellik şu an kullanılamıyor.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Karma Azure AD ile çalışmaya başlama


**S: Ortak çalışan olarak eklendiğimde kiracıdan nasıl ayrılabilirim?**

**Y:** Başka bir kuruluşun kiracısına ortak çalışan olarak eklendiğinizde, kiracılar arasında geçiş yapmak için sağ üst köşedeki "kiracı değiştiricisi" seçeneğini kullanabilirsiniz.  Şu anda, davet eden kuruluştan ayrılma seçeneği mevcut değildir ve Microsoft bu işlevselliği sunmak için çalışmaktadır.  Bu özellik kullanılabilir oluncaya kadar, davet eden kuruluştan sizi kiracısından kaldırmasını isteyebilirsiniz.
- - -
**S: Şirket içi dizinimi Azure AD'ye nasıl bağlayabilirim?**

**Y:** Şirket içi dizininizi Azure AD'ye bağlamak için Azure AD Connect'i kullanabilirsiniz.

Daha fazla bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

- - -
**S: Şirket içi dizinim ve bulut uygulamalarım arasında SSO'yu nasıl ayarlarım?**

**Y:** Çoklu oturum açmayı (SSO) yalnızca şirket içi dizininiz ve Azure AD arasında ayarlamanız yeterlidir. Bulut uygulamalarınıza Azure AD üzerinden eriştiğiniz sürece, hizmet otomatik olarak kullanıcılarınızı şirket içi kimlik bilgileriyle doğru şekilde kimlik doğrulaması gerçekleştirmeye yönlendirir.

Şirket içi konumdan SSO uygulama işlemi, Active Directory Federation Services (ADFS) gibi federasyon çözümleri veya parola karma eşitlemesini yapılandırma işlemi ile kolayca gerçekleştirilebilir. Azure AD Connect yapılandırma sihirbazını kullanarak her iki seçeneği de kolayca dağıtabilirsiniz.

Daha fazla bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

- - -
**S: Azure AD, kuruluşumdaki kullanıcılar için bir self servis portal sağlar mı?**

**Y:** Evet, AD size kullanıcı self servis ve uygulama erişimi için [Azure AD Erişim Paneli](https://myapps.microsoft.com)'ni sağlar. Office 365 müşterisiyseniz aynı işlevlerin birçoğunu Office 365 portalında da bulabilirsiniz.

Daha fazla bilgi için bkz. [Erişim Paneli'ne Giriş](../user-help/active-directory-saas-access-panel-introduction.md).

- - -
**S: Azure AD, şirket içi altyapımı yönetmeme yardımcı olur mu?**

**Y:** Evet. Azure AD Premium sürümü size Azure AD Connect Health olanağını sağlar. Azure AD Connect Health, şirket içi kimlik altyapınızı ve eşitleme hizmetlerini izlemenize ve daha iyi kavramanıza yardımcı olur.  

Daha fazla bilgi için bkz. [Bulutta şirket içi kimlik altyapınızı ve eşitleme hizmetlerinizi izleme](../hybrid/whatis-hybrid-identity-health.md).  

- - -
## <a name="password-management"></a>Parola yönetimi
**S: Azure AD parola geri yazma özelliğini parola eşitleme olmadan kullanabilir miyim? (Bu senaryoda Azure AD self servis parola sıfırlama (SSPR) özelliğinin parola geri yazma ile birlikte kullanılması ve parolaların buluta depolanmaması mümkün mü?)**

**Y:** Geri yazmayı etkinleştirmek için Active Directory parolalarınızı Azure AD ile eşitlemeniz gerekmez. Birleştirilmiş bir ortamda Azure AD çoklu oturum açma (SSO), kullanıcının kimliğini doğrulamak için şirket içi dizini kullanır. Bu senaryoda, şirket içi parolanın Azure AD'de izlenmesi gerekmez.

- - -
**S: Bir parolanın Active Directory şirket içi konumuna geri yazılması ne kadar zaman alır?**

**Y:** Parola geri yazma işlemi gerçek zamanlı olarak gerçekleşir.

Daha fazla bilgi için bkz. [Parola yönetimine başlarken](../authentication/quickstart-sspr.md).

- - -
**S: Parola geri yazma özelliğini bir yönetici tarafından yönetilen parolalarla kullanabilir miyim?**

**Y:** Evet, parola geri yazma özelliğini etkinleştirdiyseniz bir yönetici tarafından gerçekleştirilen parola işlemleri, şirket içi ortamınıza geri yazılır.  

Parola ile ilgili daha fazla soru yanıtı için bkz. [Parola yönetimi ile ilgili sık sorulan sorular](../authentication/active-directory-passwords-faq.md).
- - -
**S:  Parolamı değiştirirken mevcut Office 365/Azure AD parolamı hatırlayamazsam ne olur?**

**Y:** Bu tür bir durum için birkaç seçenek vardır.  Varsa, self servis parola sıfırlama (SSPR) özelliğini kullanın.  SSPR’nin çalışıp çalışmaması nasıl yapılandırıldığına bağlıdır.  Daha fazla bilgi için bkz. [Parola sıfırlama portalının çalışması](../authentication/howto-sspr-deployment.md).

Office 365 kullanıcıları için yöneticiniz [Kullanıcı parolalarını sıfırlama](https://support.office.com/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US) bölümünde özetlenen adımları kullanarak parolayı sıfırlayabilir.

Azure AD hesapları için yöneticiler aşağıdakilerden birini kullanarak parolaları sıfırlayabilir:

- [Azure portalda hesapları sıfırlama](active-directory-users-reset-password-azure-portal.md)
- [PowerShell’i kullanma](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Güvenlik
**S: Hesaplar belirli sayıda girişim başarısız olduktan sonra kilitleniyor mu, yoksa kullanılan daha karmaşık bir strateji mi var?**

Hesapları kilitlemek için daha karmaşık bir strateji kullanırız.  Bu strateji, isteğin IP adresini ve girilen parolaları temel alır. Kilitleme süresi ayrıca girişimin bir saldırı olma olasılığına göre artar.  

**S:  Bazı (yaygın) parolalar ‘bu parola birçok kez kullanılmış’ iletisiyle reddediliyor; bu durum parolaların geçerli active directory’de kullanıldığı anlamına mı gelir?**

Bu durum, parolaların “Parola” ve “123456” değerlerinin çeşitlemeleri gibi genel olarak yaygın olduğunu ifade eder.

**S: Güvenilmez kaynaklardan (botnet, tor uç noktası) gelen oturum açma istekleri bir B2C kiracısında engellenir mi veya bir Temel ya da Premium sürüm kiracı gerekir mi?**

İstekleri filtreleyen ve botnetlere karşı koruma sağlayıp tüm B2C kiracılarına uygulanan bir ağ geçidine sahibiz.

## <a name="application-access"></a>Uygulama erişimi

**S: Azure AD ile önceden tümleştirilmiş olan uygulamaların ve özelliklerinin listesini nereden bulabilirim?**

**Y:** Azure AD, Microsoft'a, uygulama hizmeti sağlayıcılarına ve iş ortaklarına ait 2.600'ü aşkın önceden tümleştirilmiş uygulama içerir. Önceden tümleştirilmiş tüm uygulamalar çoklu oturum açmayı (SSO) destekler. SSO, uygulamalarınıza erişmek için kurumsal kimlik bilgilerinizi kullanmanıza olanak tanır. Uygulamalardan bazıları aynı zamanda otomatik hazırlama ve sağlamayı kaldırma özelliklerini destekler.

Önceden tümleştirilmiş uygulamaların tam listesi için bkz. [Active Directory Marketi](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**S: İhtiyacım olan uygulama Azure AD marketinde yoksa ne olur?**

**Y:** Azure AD Premium ile istediğiniz uygulamayı ekleyip yapılandırabilirsiniz. Uygulamanızın özelliklerine ve tercihlerinize bağlı olarak, SSO'yu ve otomatik hazırlamayı yapılandırabilirsiniz.  

Daha fazla bilgi için bkz.

* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](../manage-apps/use-scim-to-provision-users-and-groups.md)

- - -
**S: Kullanıcılar Azure AD'yi kullanarak uygulamalarda nasıl oturum açar?**

**Y:** Azure AD, kullanıcılara uygulamalarını görüntülemeleri ve bunlara erişmeleri için birçok yol sağlar. Bunlardan bazıları aşağıda verilmiştir:

* Azure AD erişim paneli
* Office 365 uygulama başlatıcı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Daha fazla bilgi için [uygulamalar için son kullanıcı deneyimlerini](../manage-apps/end-user-experiences.md).

- - -
**S: Azure AD, uygulamalar için kimlik doğrulaması ve çoklu oturum açmaya ne gibi farklı yollarla olanak tanır?**

**Y:** Azure AD; SAML 2.0, OpenID Connect, OAuth 2.0 ve WS-Federasyon gibi birçok standartlaştırılmış kimlik doğrulaması ve yetkilendirme protokolünü destekler. Azure AD aynı zamanda, yalnızca form tabanlı kimlik doğrulamasını destekleyen uygulamalar için parola kasası oluşturma ve otomatik oturum açma işlevlerini de destekler.  

Daha fazla bilgi için bkz.

* [Azure AD için Kimlik Doğrulama Senaryoları](../develop/authentication-scenarios.md)
* [Active Directory kimlik doğrulama protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Azure AD uygulamaları için çoklu oturum açma](../manage-apps/what-is-single-sign-on.md)

- - -
**S: Şirket içi olarak çalıştırdığım uygulamaları ekleyebilir miyim?**

**Y:** Azure AD Uygulama Ara Sunucusu, size seçtiğiniz şirket içi web uygulamaları için kolay ve güvenli erişim sağlar. Bu uygulamalara, Azure AD'de hizmet olarak yazılım (SaaS) uygulamalarınıza eriştiğiniz gibi erişebilirsiniz. VPN kullanmanız veya ağ altyapınızı değiştirmeniz gerekmez.  

Daha fazla bilgi için bkz. [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](../manage-apps/application-proxy.md).

- - -
**S: Belirli bir uygulamaya erişen kullanıcılar için çok faktörlü kimlik doğrulamasını nasıl isteyebilirim?**

**Y:** Azure AD koşullu erişimi ile her bir uygulama için benzersiz bir erişim ilkesi atayabilirsiniz. İlkenizde, çok faktörlü kimlik doğrulamasını her zaman veya kullanıcılar yerel ağa bağlı olmadığında gerekli kılabilirsiniz.  

Daha fazla bilgi için bkz. [Office 365'e ve Azure Active Directory'ye bağlı diğer uygulamalara erişimi güvence altına alma](../active-directory-conditional-access-azure-portal.md).

- - -
**S: SaaS uygulamaları için otomatik kullanıcı hazırlama nedir?**

**Y:** Birçok popüler bulut SaaS uygulamasında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını otomatik hale getirmek için Azure AD kullanın.

Daha fazla bilgi için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md).

- - -
**S:  Azure AD ile güvenli bir LDAP bağlantısı oluşturabilir miyim?**

**Y:** Hayır. Azure AD, LDAP protokolünü desteklemez. Ancak, Azure AD Domain Services ile güvenli LDAP yapılandırabilirsiniz.
