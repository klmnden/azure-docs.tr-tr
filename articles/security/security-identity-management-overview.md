---
title: Azure güvenlik yardımcı olan kimlik yönetimi özellikleri | Microsoft Docs
description: " Bu makalede, kimlik yönetimi ile yardımcı olan Azure güvenlik özellikleri çekirdek genel bir bakış sağlar. Microsoft kimlik ve erişim yönetimi çözümlerini Yardımı BT doğrulama, çok faktörlü kimlik doğrulama ve koşullu erişim gibi ek düzeylerini etkinleştirme kurumsal veri merkezi ve bulut uygulamalarına ve kaynaklarına erişimi koruma ilkeleri. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: terrylan
Customer intent: As an IT Pro or decision maker I am trying to learn about identity management capabilities in Azure
ms.openlocfilehash: 758ed2e44718da709acec1379cfc79936c8b7cdf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083630"
---
# <a name="azure-identity-management-security-overview"></a>Azure Kimlik Yönetimi güvenliğine genel bakış

 Kimlik Yönetimi, kimlik doğrulama ve yetkilendirme işlemi [güvenlik sorumluları](https://docs.microsoft.com/windows/security/identity-protection/access-control/security-principals). Ayrıca, bu ilkeleri (Kimlikler) denetleme bilgilerini içerir. Güvenlik ilkeleri (Kimlikler) Hizmetleri, uygulamalar, kullanıcılar, gruplar, vb. içerebilir. Microsoft kimlik ve erişim yönetimi çözümlerini Yardımı BT kurumsal veri merkezi ve bulut uygulamalarına ve kaynaklarına erişimi koruyun. Bu tür bir koruma doğrulama, çok faktörlü kimlik doğrulama ve koşullu erişim ilkeleri gibi ek düzeyleri sağlar. Şüpheli etkinlik üzerinden yardımcı olur, olası güvenlik sorunlarını azaltma uyarı raporlama ve denetleme Gelişmiş Güvenlik İzleme. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) , şirket içi web uygulamalarına erişim ve çoklu oturum açma (SSO), bulut yazılım binlerce hizmet (SaaS) uygulamaları olarak sağlar.
 
Azure Active Directory (Azure AD) güvenlik avantajlarından yararlanarak, şunları yapabilirsiniz:

* Oluşturma ve kullanıcılara, gruplara veya cihazlara eşitlenmiş durumda kalmasını karma kuruluşunuz genelinde her kullanıcı için tek bir kimlik yönetin. 
* Binlerce önceden tümleştirilmiş SaaS uygulamasını da dahil olmak üzere uygulamalarınızın SSO erişimi sağlar.
* Ve bulut uygulamalarında kural tabanlı çok faktörlü kimlik doğrulaması için hem şirket içi zorlayarak uygulama erişim güvenliği etkinleştirmek.
* Azure AD uygulama proxy'si aracılığıyla şirket içi web uygulamalarına güvenli uzaktan erişim sağlayın.

Bu makalenin amacı, temel genel bakış, kimlik yönetimi ile yardımcı olan Azure güvenlik özellikleri sağlamaktır. Daha fazla bilgi için her özelliğin ayrıntılarını veren makalelerin bağlantıları da sunuyoruz.  

Bu makalede aşağıdaki çekirdek Azure kimlik yönetimi özellikleri odaklanır:

* Çoklu oturum açma
* Ters proxy
* Multi-Factor Authentication
* Rol tabanlı erişim denetimi (RBAC)
* Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar
* Tüketici kimliği ve erişim yönetimi
* Cihaz kaydı
* Ayrıcalıklı kimlik yönetimi
* Kimlik koruması
* Karma Kimlik Yönetimi/Azure AD connect
* Azure AD erişim gözden geçirmeleri

## <a name="single-sign-on"></a>Çoklu oturum açma

SSO, tüm uygulama ve yalnızca tek bir kullanıcı hesabı kullanarak bir kez oturum açarak iş yapmanız gereken kaynaklara erişmeye çalıştığında anlamına gelir. Oturum açtıktan sonra tüm gereken kimlik doğrulaması için gerekli olmadan uygulamaları erişebilirsiniz (örneğin, bir parola yazmak) ikinci kez.

Office 365 kutusu ve Salesforce gibi SaaS uygulamaları için kullanıcı üretkenliğini bağlı pek çok kuruluş kullanır. Tarihsel olarak, BT personeliniz ayrı ayrı oluşturup her SaaS uygulamasında kullanıcı hesaplarını güncelleştirmek gereken ve kullanıcıların her bir SaaS uygulaması için bir parola hatırlaması gerekiyordu.

Yalnızca kendi etki alanına katılmış aygıtlar ve şirket kaynaklarına oturum açmak için birincil kuruluş hesabını kullanmalarını sağlama Azure AD şirket içi Active Directory, bulut ortamlarına genişleten, ancak aynı zamanda tüm web ve SaaS uygulamaları için ihtiyaç duydukları Kullanıcıların işlerini için.

Yalnızca kullanıcılar birden fazla kullanıcı adları ve parolaları yönetmek izniniz yok, sağlama veya uygulama erişimi otomatik olarak kendi kuruluş gruplarına ve çalışan durumlarına göre sağlamasını. Azure AD güvenlik ve erişim yönetimi denetimleri ile merkezi olarak kullanıcıların erişimini SaaS uygulamaları arasında yönetebileceğiniz tanıtır.

Daha fazla bilgi edinin:

* [Çoklu oturum açma genel bakış](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../active-directory/manage-apps/what-is-single-sign-on.md)
* [Azure Active Directory çoklu oturum açma SaaS uygulamaları ile tümleştirme](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Ters proxy

Azure AD uygulama proxy'si gibi şirket içi uygulama yayımlamanıza olanak tanır [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) siteleri [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), ve [IIS](https://www.iis.net/)-tabanlı özel ağınızdaki uygulamalar ve ağınızın dışından kullanıcılar güvenli erişim sağlar. Uygulama Ara sunucusu uzaktan erişim sağlar ve SSO için birçok türde şirket içi web uygulamaları Azure AD'ye destekleyen SaaS uygulamaları binlerce sahip. Çalışanların uygulamasında oturum açabilir, uygulamalarınızdaki ev kendi cihazlarında ve bu bulut tabanlı bir proxy üzerinden kimlik doğrulaması.

Daha fazla bilgi edinin:

* [Azure AD uygulama ara sunucusunu etkinleştirme](../active-directory/manage-apps/application-proxy-enable.md)
* [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](../active-directory/active-directory-application-proxy-publish.md)
* [Uygulama proxy'si ile çoklu oturum açma](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Koşullu erişim ile çalışma](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure multi-Factor Authentication, birden fazla doğrulama yönteminin kullanılmasını gerektirir ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekler, kimlik doğrulama yöntemidir. Çok faktörlü kimlik doğrulaması erişimi korumaya yardımcı olur ve uygulamalarınıza karşılarken basit bir oturum açma işlemi. Doğrulama seçenekleri aracılığıyla güçlü kimlik doğrulama sunar: telefon görüşmeleri, sms'ler veya mobil uygulama bildirimleri veya doğrulama kodları ve üçüncü taraf OAuth belirteçlerini.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../active-directory/authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır?](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="rbac"></a>RBAC

RBAC, Azure kaynak Azure kaynaklarının ayrıntılı erişim yönetimi sağlayan Manager'da oluşturulan bir yetkilendirme sistemidir. RBAC, tabletleri ayrıntılı olarak, kullanıcıların erişim düzeyini denetlemenize olanak tanır. Örneğin, yalnızca sanal ağlarını yönetmeleri için bir kullanıcı ve bir kaynak grubundaki tüm kaynakları yönetmek için başka bir kullanıcı sınırlayabilirsiniz. Azure, kullanabileceğiniz birkaç yerleşik rol içerir. Dört temel yerleşik rol aşağıda listelenmiştir. İlk üçü tüm kaynak türleri için geçerlidir.

Daha fazla bilgi edinin:

* [Rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
* [Azure kaynakları için yerleşik roller](../role-based-access-control/built-in-roles.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar

Güvenlik İzleme, uyarılar ve tutarsız erişim düzenlerini belirleyen makine öğrenmesi tabanlı raporlarla işletmenizi korumanıza yardımcı olabilir. Azure AD erişim kullanabilirsiniz ve kullanım raporları için bütünlük ve güvenliğini işlemlerini kuruluş dizininizle görünürlük elde edin. Bu bilgiler ile dizin Yöneticisi böylece bunlar bu riskleri azaltmak yeterince planlayabilirsiniz olası güvenlik risklerini nereden kaynaklanıyor olabilir daha iyi belirleyebilirsiniz.

Azure portalında, raporlar, aşağıdaki kategorilere ayrılır:

* **Anomali raporları**: Anormal olarak bulduk oturum açma olaylarını içerir. Hedefimiz, bu tür bir etkinlik haberdar olun ve şüpheli bir olay olup olmadığını belirlemek etkinleştirmeniz oluşturmaktır.
* **Tümleşik uygulama raporları**: Kuruluşunuzda bulut uygulamalarının nasıl kullanıldığını içine sağlar. Azure AD ile binlerce bulut uygulamasına tümleştirme sunar.
* **Hata raporlarını**: Dış uygulama hesaplarına sağladığınızda oluşabilecek hatalar gösterir.
* **Kullanıcıya özel raporları**: Belirli bir kullanıcı için cihaz oturum açma etkinliği verileri görüntüle.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 gün ve Grup etkinlik değişiklikleri ve parola sıfırlama ve kayıt etkinliği içinde denetlenen tüm olayların bir kaydını içerir.

Daha fazla bilgi edinin:

* [Erişim ve kullanım raporlarınızı görüntüleme](../active-directory/active-directory-view-access-usage-reports.md)
* [Başlama Azure Active Directory Raporlama ile](../active-directory/active-directory-reporting-getting-started.md)
* [Azure Active Directory raporlama Kılavuzu](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Tüketici kimliği ve erişim yönetimi

Azure AD B2C yüz milyonlarca kimliğe kadar ölçeklendirebileceğiniz müşteri uygulamaları için bir yüksek oranda kullanılabilir ve global bir kimlik Yönetim Hizmeti ' dir. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz için özelleştirilebilir bir deneyimle uygulamalarınızda ister mevcut sosyal hesaplarını kullanarak ister yeni kimlik bilgileri oluşturarak oturum açabilir.

Geçmişte, müşteriler ve kullandıkları uygulamalar için oturum açın isteyen uygulama geliştiricileri kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Azure AD B2C, kuruluşunuzun uygulamalarına güvenli, standartlara dayalı bir platform ve Genişletilebilir ilkeler büyük bir dizi Yardım tüketici kimlik yönetimini tümleştirmeleri için daha iyi bir yol sunar.

Azure AD B2C kullandığınızda tüketicileriniz uygulamalarınız için ister mevcut sosyal hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak ister yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak kaydolabilirsiniz.

Daha fazla bilgi edinin:

* [Azure Active Directory B2C nedir?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C önizlemesi: Kaydolun ve tüketicilerinizin uygulamanıza oturum açma](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C önizlemesi: Uygulama türleri](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Cihaz kaydı

Azure AD cihaz kaydı için temel olan aygıt tabanlı [koşullu erişim](../active-directory/active-directory-conditional-access-device-registration-overview.md) senaryoları. Bir cihaz kaydedildiğinde Azure AD cihaz kaydı, bir kullanıcı oturum açtığında cihazın kimliğini doğrulamak için kullanılan kimliğe sahip cihazı sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri sonra Bulut ve şirket içi barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak üzere kullanılabilir.

Intune gibi bir mobil cihaz yönetimi çözümü ile birleştirildiğinde Azure AD'de cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Ardından, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturabilirsiniz.

Daha fazla bilgi edinin:

* [Azure AD cihaz kaydı ile çalışmaya başlama](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Windows etki alanına katılmış cihazlar için Azure AD ile otomatik cihaz kaydı](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Windows otomatik kaydı oluşturma Azure AD ile etki alanına katılmış cihazları ayarlama](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Ayrıcalıklı kimlik yönetimi

Azure AD Privileged Identity Management ile yönetebilir, denetleyebilir ve ayrıcalıklı kimliklerinizi izleyebilir ve Azure ad'deki kaynakların yanı sıra Office 365 ve Microsoft Intune gibi diğer Microsoft online services erişim.

Kullanıcılar bazen kaynakları Azure veya Office 365 veya diğer SaaS uygulamalarını ayrıcalıklı işlemleri gerçekleştirmek gerekebilir. Bu gereksinim sıklıkla kuruluşlar Azure AD'de kullanıcılara kalıcı ayrıcalıklı erişim vermek olduğu anlamına gelir. Erişim bulutta barındırılan kaynakları için artan bir güvenlik riski çünkü kuruluşların, kendi yönetici ayrıcalıklarına sahip kullanıcıların ne yaptıklarını yeterince izleyemez. Ayrıca, ayrıcalıklı erişime sahip bir hesap tehlikede olursa bu bir güvenlik ihlaline kuruluş genel bulut güvenliği etkileyebilir. Azure AD Privileged Identity Management Bu riskin azaltılmasına yardımcı olur.

Azure AD Privileged Identity Management ile şunları yapabilirsiniz:

* Hangi kullanıcıların Azure AD yöneticileri olduğuna bakın.
* İsteğe bağlı just-ın-time (JIT) yönetimsel erişim için Office 365 ve Intune gibi Microsoft hizmetlerini etkinleştirin.
* Yönetici atamalarını yönetici erişim geçmişine ve değişiklikleri hakkında rapor alın.
* Ayrıcalıklı bir role erişimi hakkında uyarı alın.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md)
* [Azure AD dizin rollerini PIM atayın](../active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Kimlik koruması

Azure AD kimlik koruması, risk olayları ve kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarına ilişkin birleştirilmiş görünüm sağlayan bir güvenlik hizmetidir. Kimlik koruması, Azure AD anormal etkinlik raporları aracılığıyla kullanılabilen mevcut Azure AD anomali algılama özelliklerinin avantajlarından yararlanır. Kimlik koruması, anormallikleri gerçek zamanlı olarak tespit edebilirsiniz yeni risk olayı türleri de sunar.

Daha fazla bilgi edinin:

* [Azure AD Kimlik Koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD kimlik gösterin: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-managementazure-ad-connect"></a>Karma Kimlik Yönetimi/Azure AD connect

Microsoft’un kimlik çözümleri şirket içi ve bulut tabanlı çözümleri birleştirerek konumdan bağımsız olarak tüm kaynaklara kimlik doğrulaması ve yetkilendirme sağlamak üzere tek bir kullanıcı kimliği oluşturur. Buna hibrit kimlik denir. Azure AD Connect, Microsoft'un karma kimlik hedeflerinizi karşılamak ve gerçekleştirmek için tasarladığı araçtır. Bu sayede kullanıcılarınıza Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik sağlayabilirsiniz. Aşağıdaki özellikleri sağlar:

* Eşitleme
* AD FS ile Federasyon tümleştirmesi
* Kimlik doğrulaması aracılığıyla geçirin
* Sistem Durumunu İzleme

Daha fazla bilgi edinin:

* [Karma kimlik teknik incelemesi](https://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Azure AD ekip blogu](https://blogs.technet.microsoft.com/ad/)

## <a name="azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri

Azure Active Directory (Azure AD) erişim gözden geçirmeleri, kuruluşların grup üyeliklerini, kurumsal uygulamalara erişimi ve ayrıcalıklı rol atamalarını verimli şekilde yönetmesine olanak sağlar.

Daha fazla bilgi edinin:

* [Azure AD erişim gözden geçirmeleri](../active-directory/governance/access-reviews-overview.md)
* [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](../active-directory/governance/access-reviews-overview.md)
