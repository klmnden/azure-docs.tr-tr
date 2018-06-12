---
title: Azure güvenlik özellikleri Kimlik Yönetimi ile ilgili Yardım | Microsoft Docs
description: " Bu makalede Identity management ile Yardım Azure güvenlik özellikleri çekirdek genel bir bakış sağlar. Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT doğrulama, çok faktörlü kimlik doğrulama ve koşullu erişim gibi ek düzeylerini etkinleştirme uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma ilkeleri. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 8763f1dca110a43586619c09f5d25c340c177b09
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300666"
---
# <a name="azure-identity-management-security-overview"></a>Azure Kimlik Yönetimi güvenliğine genel bakış
Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma. Böyle bir koruma doğrulama, çok faktörlü kimlik doğrulama ve koşullu erişim ilkeleri gibi ek düzeyleri sağlar. Raporlama, denetlemek ve olası güvenlik sorunlarını yardımcı azaltmak uyarı Gelişmiş Güvenlik ile izleme şüpheli etkinlik. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) bulut yazılım binlerce hizmet (SaaS) uygulamaları olarak, çoklu oturum açma (SSO) sağlar ve şirket içi web uygulamaları için erişirsiniz.

Azure Active Directory (Azure AD) sunduğu güvenlik avantajlarından yararlanarak, şunları yapabilirsiniz:

* Oluşturun ve her kullanıcı için tek bir kimliği kullanıcıları, grupları ve aygıtları eşitlenmiş şekilde kalmasının karma kuruluşunuz yönetin.
* Önceden tümleştirilmiş SaaS uygulamaları binlerce de dahil olmak üzere, uygulamalarınız için SSO erişimi sağlar.
* Uygulama erişimi güvenliğini kural tabanlı çok faktörlü kimlik doğrulamasını hem şirket içi zorlayarak etkinleştirmek ve bulut uygulamalarında.
* Şirket içi web uygulamaları Azure AD uygulama proxy'si aracılığıyla güvenli uzaktan erişim sağlayın.

Bu makalede amacı, Identity management ile Yardım Azure güvenlik özellikleri çekirdek genel bir bakış sağlamaktır. Daha fazla bilgi için her bir özelliğin ayrıntılarını veren makalelerinin bağlantıları da sunuyoruz.  

Makaleyi aşağıdaki çekirdek Azure kimlik yönetimi özellikleri üzerine odaklanır:

* Çoklu oturum açma
* Ters proxy
* Multi-Factor Authentication
* Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar
* Tüketici kimliği ve erişim yönetimi
* Cihaz kaydı
* Ayrıcalıklı Kimlik Yönetimi
* Kimlik koruması
* Karma Kimlik Yönetimi

## <a name="single-sign-on"></a>Çoklu oturum açma
SSO, tüm uygulamaları ve iş, yalnızca tek bir kullanıcı hesabı kullanarak bir kez oturum açarak yapmak için gereken kaynaklar erişebildiklerinden anlamına gelir. Oturum açıldıktan sonra tüm gereken kimlik doğrulaması için gerekli olmadan uygulamaları erişebilirsiniz (örneğin, bir parola yazın) ikinci kez.

Office 365, kutusunu ve Salesforce gibi SaaS uygulamaları için kullanıcı üretkenliğini bağlı birçok kuruluş kullanır. Geçmişte, tek tek oluşturun ve her SaaS uygulamasının kullanıcı hesaplarını güncelleştirmek BT personeli gerekli ve kullanıcıların her SaaS uygulaması için bir parola hatırlaması gerekiyordu.

Yalnızca etki alanına katılmış aygıtlar ve şirket kaynaklarına oturum açmak için birincil kuruluş hesaplarıyla kullanmalarını sağlama Azure AD şirket içi Active Directory bulut ortamlarına genişletir, ancak aynı zamanda tüm web ve SaaS uygulamaları için ihtiyaç duydukları işlerini için.

Yalnızca kullanıcıların kullanıcı adları ve parolalar birden çok kümelerini yönetme gerekmez, sağlamak veya uygulama erişimini otomatik olarak, kuruluş, gruplar ve çalışan durumlarına göre sağlanmasını. Azure AD güvenlik ve erişim yönetimi denetimleri ile merkezi olarak kullanıcıların erişimini SaaS uygulamaları arasında yönetebilirsiniz tanıtır.

Daha fazla bilgi edinin:

* [Çoklu oturum açma genel bakış](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../active-directory/manage-apps/what-is-single-sign-on.md)
* [Azure Active Directory çoklu oturum açma SaaS uygulamaları ile tümleştirme](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Ters proxy
Azure AD uygulama proxy'si sağlar, şirket içi uygulamalar gibi yayımlama [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) siteler, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), ve [IIS](http://www.iis.net/)-tabanlı uygulamalar özel ağınızdan ve ağınızın dışından kullanıcılarla güvenli erişim sağlar. SSO için çeşitli şirket içi web uygulamaları Azure AD destekleyen SaaS uygulamaları binlerce ve uygulama proxy'si uzaktan erişim sağlar. Çalışanlar uygulamasında oturum açabilir, uygulamalardan ev kendi cihazlarda ve bu bulut tabanlı proxy üzerinden kimlik doğrulaması.

Daha fazla bilgi edinin:

* [Azure AD uygulama ara sunucusunu etkinleştirme](../active-directory/manage-apps/application-proxy-enable.md)
* [Azure AD uygulama proxy'si ile uygulama yayımlama](../active-directory/active-directory-application-proxy-publish.md)
* [Uygulama proxy'si ile çoklu oturum açma](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Koşullu erişim ile çalışma](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-Factor Authentication, birden fazla doğrulama yöntemi kullanılmasını gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. Çok faktörlü kimlik doğrulaması yardımcı erişimi korumaya veri ve uygulamalara basit bir oturum açma işlemi için kullanıcı talebine toplantı oluştu. Güçlü kimlik doğrulama seçeneklerini çeşitli aracılığıyla sunar: telefon görüşmeleri, metin iletileri veya mobil uygulamasının bildirimleri veya doğrulama kodları ve üçüncü taraf OAuth belirteçleri.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../active-directory/authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar
Güvenlik İzleme, uyarılar ve tutarsız erişim desenlerini tanımlamak makine öğrenme tabanlı raporlar, işinizin korunmasına yardımcı olabilir. Azure AD erişim kullanabilirsiniz ve kullanım raporları için bütünlük ve kuruluşunuzun dizin güvenliğini görünürlük elde etmek. Bu bilgi ile böylece bunlar bu riskleri azaltmak yeterli planlayabilirsiniz olası güvenlik riskleri burada kaynaklanıyor olabilir bir dizin Yöneticisi daha iyi belirleyebilirsiniz.

Azure portalında raporları aşağıdaki kategorilere ayrılır:

* **Kural dışı durum raporları**: anormal olarak bulduk oturum açma olayları içerir. Amacımız, bu tür etkinlik farkında olun ve bir olay şüpheli olup olmadığını belirlemek etkinleştirmeniz olmaktır.
* **Uygulama raporları tümleşik**: Bulut uygulamalarını, kuruluşunuzda nasıl kullanıldığını içine Öngörüler sağlayın. Azure AD bulut uygulamalarını binlerce ile tümleştirme sağlar.
* **Hata raporlarını**: dış uygulamaları hesaplarına sağladığınızda oluşabilecek hatalar gösterir.
* **Kullanıcıya özgü raporları**: cihaz oturum açma etkinliği verileri belirli bir kullanıcı için görüntüler.
* **Etkinlik günlükleri**: Son 24 saat, son 7 gün veya son 30 gün ve Grup etkinlik değişikliklerini ve parola sıfırlama ve kayıt etkinlik tüm Denetlenen olayları kaydını içerir.

Daha fazla bilgi edinin:

* [Erişim ve kullanım raporlarınızı görüntüleme](../active-directory/active-directory-view-access-usage-reports.md)
* [Başlama Azure Active Directory Raporlama ile](../active-directory/active-directory-reporting-getting-started.md)
* [Azure Active Directory raporlama Kılavuzu](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Tüketici kimliği ve erişim yönetimi
Azure AD B2C yüz milyonlarca kimlikleri için ölçeklendirilebilen bir yüksek oranda kullanılabilir, genel kimlik yönetimi tüketiciye yönelik uygulamalar için hizmetidir. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınıza, var olan sosyal hesaplarını kullanarak veya yeni kimlik bilgileri oluşturma oturum açabilir.

Geçmişte, müşteriler oturumu ve kullandıkları uygulamalar için oturum açın isteyen uygulama geliştiricileri kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Kuruluşunuz Azure AD B2C tüketici kimlik yönetimini uygulamalarıyla güvenli, standartlara dayalı bir platform ve çok sayıda ilkeler yardımıyla bütünleştirmek için daha iyi bir yol sunar.

Azure AD B2C kullandığınızda tüketicileriniz uygulamalarınız için var olan sosyal hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak veya yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak kaydolabilir.

Daha fazla bilgi edinin:

* [Azure Active Directory B2C nedir?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C önizlemesi: oturum ayarlama ve tüketicilerinizin uygulamanıza oturum](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C önizlemesi: Uygulama türleri](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Cihaz kaydı
Azure AD cihaz kaydı için temel olan aygıt tabanlı [koşullu erişim](../active-directory/active-directory-conditional-access-device-registration-overview.md) senaryoları. Bir cihaz kaydedildiğinde Azure AD cihaz kaydı, bir kullanıcı oturum açtığında cihazın kimliğini doğrulamak için kullanılan kimliğe sahip cihazı sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri sonra Bulut ve şirket içi barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak için kullanılabilir.

Intune gibi bir mobil cihaz yönetimi çözümü ile birleştirildiğinde, Azure AD cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Ardından, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturabilirsiniz.

Daha fazla bilgi edinin:

* [Azure AD cihaz kaydını kullanmaya başlama](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Windows etki alanına katılmış cihazlar için Azure AD ile otomatik cihaz kaydı](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Windows otomatik kayıt etki alanına katılmış cihazları Azure AD ile ayarlama](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi
Azure AD Privileged Identity Management'ı yönetin, denetleyin ve ayrıcalıklı kimliklerinizi izlemek ve Azure ad'deki kaynakların yanı sıra Office 365 ve Microsoft Intune gibi diğer Microsoft online services erişimi.

Kullanıcılar bazen Azure veya Office 365 kaynakları veya diğer SaaS uygulamaları ayrıcalıklı işlemleri gerçekleştirmek gerekir. Bu gereksinim genellikle kuruluş Azure AD'de kullanıcıları kalıcı ayrıcalıklı erişim vermeniz anlamına gelir. Kuruluşlar, kullanıcıların yönetici ayrıcalıklarına sahip kullanıcıların ne yaptıklarını yeterince izleyemez çünkü bu erişimin bulutta barındırılan, kaynaklar için büyüyen bir güvenlik riski oluşturur. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı ihlal edilmesi durumunda, bir ihlali kuruluşun genel bulutun güvenlik etkileyebilir. Azure AD Privileged Identity Management Bu riskin azaltılmasına yardımcı olur.

Azure AD Privileged Identity Management ile şunları yapabilirsiniz:

* Hangi kullanıcıların Azure AD Yöneticiler olduğuna bakın.
* İsteğe bağlı, tam zamanında (JIT) yönetim erişim Office 365 ve Intune gibi Microsoft hizmetlerinde etkinleştirin.
* Yönetici erişim geçmişine ve değişiklikler hakkında raporlar yönetici atamaları alın.
* Ayrıcalıklı bir rol için erişim hakkında uyarı alın.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Azure AD Privileged Identity Management rollerinde](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: nasıl'ın bir kullanıcı rolü ekleme veya kaldırma](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Kimlik koruması
Azure AD kimlik koruması birleştirilmiş görünüme risk olaylarına ve kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını sağlayan bir güvenlik hizmetidir. Kimlik koruması Azure AD anormal etkinlik raporları kullanılabilen mevcut Azure AD anomali algılama özelliklerinden yararlanır. Kimlik koruması, aynı zamanda anormallikleri gerçek zamanlı olarak algılayabilir yeni risk olayı türleri sunar.

Daha fazla bilgi edinin:

* [Azure AD Kimlik Koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD ve kimlik göster: kimlik koruması Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Karma Kimlik Yönetimi
Şirket içi ve bulut kimlik doğrulaması ve yetkilendirme konum bağımsız olarak tüm kaynaklar için tek bir kullanıcı kimliği oluşturma, kimlik Microsoft yaklaşımı yayar.

Daha fazla bilgi edinin:

* [Karma kimlik teknik incelemesi](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Azure AD ekip blogu](https://blogs.technet.microsoft.com/ad/)
