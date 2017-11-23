---
title: "Azure güvenlik özellikleri Kimlik Yönetimi ile ilgili Yardım | Microsoft Docs"
description: " Bu makalede Identity management ile Yardım Azure güvenlik özellikleri çekirdek genel bir bakış sağlar. Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT doğrulama çok faktörlü kimlik doğrulama ve koşullu erişim gibi ek düzeylerini etkinleştirme uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma ilkeleri. "
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
ms.openlocfilehash: 85ea328bdea1aad28765712e3639f6719deab7e2
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2017
---
# <a name="azure-identity-management-security-overview"></a>Azure Kimlik Yönetimi güvenliğine genel bakış
Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT doğrulama çok faktörlü kimlik doğrulama ve koşullu erişim gibi ek düzeylerini etkinleştirme uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma ilkeleri. Raporlama, Denetim ve yardımcı olası güvenlik sorunlarını azaltmak uyarı Gelişmiş Güvenlik ile izleme şüpheli etkinlik. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) çoklu oturum açma bulut binlerce (SaaS) uygulamaları sağlar ve web uygulamaları için şirket içi erişebilirsiniz.

Güvenlik, Azure Active Directory (AD) yeteneği yararları:

* Oluşturma ve kullanıcılara, gruplara ve cihazlara eşitlenmiş şekilde kalmasının, karma kuruluşunuzda her kullanıcı için tek bir kimliği yönetme
* Önceden tümleştirilmiş SaaS uygulamaları binlerce de dahil olmak üzere uygulamalarınıza tek oturum açma erişim sağlamak
* Uygulama erişimi güvenliğini kural tabanlı çok faktörlü kimlik doğrulamasını hem şirket içi zorlayarak etkinleştirmek ve bulut uygulamalarında
* Şirket içi web uygulamaları Azure AD uygulama proxy'si aracılığıyla güvenli uzaktan erişim sağlama

Bu makalede amacı, Identity management ile Yardım Azure güvenlik özellikleri çekirdek genel bir bakış sağlamaktır. Daha fazla bilgi için her bir özelliğin ayrıntılarını veren makalelerinin bağlantıları da sunuyoruz.  

Makaleyi aşağıdaki çekirdek Azure kimlik yönetimi özellikleri üzerine odaklanır:

* Çoklu oturum açma
* Ters proxy
* Multi-factor authentication
* Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar
* Tüketici kimliği ve erişim yönetimi
* Cihaz kaydı
* Ayrıcalıklı Kimlik Yönetimi
* Kimlik koruması
* Karma Kimlik Yönetimi

## <a name="single-sign-on"></a>Çoklu oturum açma
Tüm uygulamaları ve iş, yalnızca bir kez tek bir kullanıcı hesabı kullanarak oturum açma tarafından yapmak için gereken kaynaklar erişebildiklerinden tek oturum açma (SSO) anlamına gelir. Oturum açıldıktan sonra tüm gereken kimlik doğrulaması için gerekli olmadan uygulamaları erişebilirsiniz (örneğin, bir parola yazın) ikinci kez.

Birçok kuruluş yazılım için son kullanıcı üretkenliğini Office 365, kutusunu ve Salesforce gibi bir hizmet (SaaS) uygulamaları olarak kullanır. Geçmişte, tek tek oluşturun ve her SaaS uygulamasının kullanıcı hesaplarını güncelleştirmek BT personeli gerekli ve kullanıcıların her SaaS uygulaması için bir parola hatırlaması gerekiyordu.

Yalnızca kendi etki alanına katılmış cihazlarda oturum açmak ve şirket kaynaklarını birincil kuruluş hesaplarıyla kullanmalarını sağlama Azure AD şirket içi Active Directory bulut ortamlarına genişletir, ancak aynı zamanda tüm web ve SaaS uygulamaları için gerekli işlerini.

Yalnızca kullanıcıların kullanıcı adları ve parolalar birden çok kümelerini yönetme gerekmez, uygulama erişimini otomatik olarak sağlanan veya XML'deki sağlanan dayalı olarak kuruluş grupları ve durumlarını çalışan olarak olabilir. Azure AD güvenlik ve SaaS uygulamaları arasında kullanıcıların erişimini merkezi olarak yönetmenizi sağlayan erişim İdaresi denetimleri tanıtır.

Daha fazla bilgi edinin:

* [Çoklu oturum açma genel bakış](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [Uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](../active-directory/active-directory-appssoaccess-whatis.md)
* [Azure Active Directory çoklu oturum açma SaaS uygulamaları ile tümleştirme](../active-directory/active-directory-enterprise-apps-manage-sso.md)

## <a name="reverse-proxy"></a>Ters proxy
Azure AD uygulama proxy'si sağlar, şirket içi uygulamalar gibi yayımlama [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) siteler, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), ve [IIS](http://www.iis.net/)-tabanlı uygulamalar özel ağınızdan ve ağınızın dışından kullanıcılarla güvenli erişim sağlar. Uygulama proxy'si, Azure AD destekleyen SaaS uygulamaları binlerce şirket içi web uygulamaları birçok türleri için uzaktan erişim ve çoklu oturum açma (SSO) sağlar. Çalışanlar oturum açtığınızda, uygulamalardan ev kendi cihazlarda ve bu bulut tabanlı proxy üzerinden kimlik doğrulaması.

Daha fazla bilgi edinin:

* [Azure AD uygulama ara sunucusunu etkinleştirme](../active-directory/active-directory-application-proxy-enable.md)
* [Azure AD uygulama proxy'si ile uygulama yayımlama](../active-directory/active-directory-application-proxy-publish.md)
* [Çoklu oturum açma uygulama proxy'si ile uygulama](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
* [Koşullu erişim ile çalışma](../active-directory/application-proxy-enable-remote-access-sharepoint.md)

## <a name="multi-factor-authentication"></a>Multi-factor authentication
Azure çok faktörlü kimlik doğrulaması (MFA), birden fazla doğrulama yöntemi kullanılmasını gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. MFA yardımcı olan veri ve basit bir oturum açma işlemi için kullanıcı talebine toplantı sırasında uygulamalara erişimi korumaya. Güçlü kimlik doğrulama seçeneklerini çeşitli aracılığıyla sunar — telefon araması, SMS mesajı veya mobil uygulama bildirimi veya doğrulama kodu ve üçüncü taraf OAuth belirteçleri.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Güvenlik İzleme, uyarılar ve makine öğrenme tabanlı raporlar
Güvenlik İzleme ve uyarılar ve tutarsız erişim desenlerini tanımlamak makine öğrenme tabanlı raporlar, işinizin korunmasına yardımcı olabilir. Bütünlük ve kuruluşunuzun dizininin güvenlik görünürlük elde etmek için Azure Active Directory'nin erişim ve kullanım raporlarını kullanabilirsiniz. Bu bilgileri kullanarak bir dizin yönetici böylece bunlar bu riskleri azaltmak yeterli planlayabilirsiniz olası güvenlik riskleri burada bulunan daha iyi belirleyebilirsiniz.

Klasik Azure portalında raporları aşağıdaki yollarla ayrılır:

* Anomali raporları – oturum açma anormal olarak bulduk olaylarını içerir. Amacımız olduğu gibi etkinlik farkında olun ve bir olay şüpheli olup olmadığı hakkında bir belirleme hale getiremezsiniz olanak sağlar.
* Tümleşik uygulama raporları – bulut uygulamalarını, kuruluşunuzda nasıl kullanıldığını içine Öngörüler sağlar. Azure Active Directory bulut uygulamalarını binlerce ile tümleştirme sağlar.
* Hata raporlarını – dış uygulamalara hesap sağlamada oluşabilecek hatalar gösterir.
* Kullanıcıya özgü raporları – belirli bir kullanıcı için etkinlik verilerdeki aygıt/oturum görüntüler.
* Etkinlik günlükleri – son 24 saat, son 7 gün veya son 30 gün ve Grup etkinlik değişikliklerini ve parola sıfırlama ve kayıt etkinlik tüm Denetlenen olayları kaydını içerir.

Daha fazla bilgi edinin:

* [Erişim ve kullanım raporlarınızı görüntüleme](../active-directory/active-directory-view-access-usage-reports.md)
* [Azure Active Directory Raporlama ile çalışmaya başlama](../active-directory/active-directory-reporting-getting-started.md)
* [Azure Active Directory raporlama Kılavuzu](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Tüketici kimliği ve erişim yönetimi
Azure Active Directory B2C için yüz milyonlarca kimlikleri ölçeklendirilebilen bir yüksek oranda kullanılabilir, genel kimlik yönetimi tüketiciye yönelik uygulamalar için hizmetidir. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz, ister mevcut sosyal hesaplarını kullanarak ister yeni kimlik bilgileri oluşturarak özelleştirilebilir bir deneyimle uygulamalarınızda oturum açabilir.

Geçmişte tüketicilerin uygulamalarına kaydolmasını ve oturum açmasını isteyen uygulama geliştiricileri kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Kuruluşunuz Azure Active Directory B2C tüketici kimlik yönetimini uygulamalarıyla güvenli, standartlara dayalı bir platform ve çok sayıda ilkeler yardımıyla bütünleştirmek için daha iyi bir yol sunar.

Azure Active Directory B2C kullandığınızda tüketicileriniz uygulamalarınız için var olan sosyal hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak veya yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak kaydolabilir.

Daha fazla bilgi edinin:

* [Azure Active Directory B2C nedir?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C önizlemesi: oturum ayarlama ve tüketicilerinizin uygulamanıza oturum](../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C önizlemesi: Uygulama türleri](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Cihaz kaydı
Azure AD cihaz kaydı için temel olan aygıt tabanlı [koşullu erişim](../active-directory/active-directory-conditional-access-device-registration-overview.md) senaryoları. Bir cihaz kaydedildiğinde Azure Active Directory cihaz kaydı kullanıcı oturum açtığında cihazın kimliğini doğrulamak için kullanılan bir kimliğe sahip cihazı sağlar. Kimliği doğrulanmış cihaz ve cihaz öznitelikleri böylece bulutta ve şirket içinde barındırılan uygulamalar için koşullu erişim ilkelerini zorlamak üzere kullanılabilir.

Intune gibi bir mobil cihaz Yönetimi (MDM) çözümü ile birleştirildiğinde Azure Active Directory'deki cihaz öznitelikleri cihaz hakkındaki ek bilgilerle güncelleştirilir. Bu durum, güvenlik ve uyumluluğa yönelik standartlarınızı karşılamak için cihazlardan erişimi zorlayan koşullu erişim kuralları oluşturmanıza olanak sağlar.

Daha fazla bilgi edinin:

* [Azure Active Directory cihaz kaydını kullanmaya başlama](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Azure Active Directory için Windows etki alanına katılmış aygıtlar ile otomatik cihaz kaydı](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Windows otomatik kayıt Azure Active Directory ile etki alanına katılmış cihazları ayarlama](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi
Azure Active Directory (AD) Privileged Identity Management, ayrıcalıklı kimliklerinizi ve Azure AD'deki kaynakların yanı sıra, Office 365 veya Microsoft Intune diğer Microsoft çevrimiçi hizmetlerinin içerdiği kaynaklara yönelik erişimi yönetmenize, denetlemenize ve izlemenize olanak tanır.

Bazen kullanıcıların Azure veya Office 365 kaynakları veya diğer SaaS uygulamaları ayrıcalıklı işlemleri gerçekleştirmek gerekir. Bu, genellikle bunları Azure AD'de kalıcı ayrıcalıklı erişim vermek kuruluş sahip anlamına gelir. Kuruluşlar, yeterince yönetici ayrıcalıklarını bu kullanıcıların ne yaptıklarını izleyemez bulutta barındırılan kaynaklar için büyüyen bir güvenlik riski olmasıdır. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı ihlal edilmesi durumunda, bir ihlali, genel bulutun güvenlik etkileyebilir. Azure AD Privileged Identity Management bu riski gidermeye yardımcı olur.

Azure AD Privileged Identity Management sağlar:

* Hangi kullanıcıların Azure AD admins olduğuna bakın
* İsteğe bağlı, "Microsoft Online Services yönetim erişimi, Office 365 ve Intune gibi tam zamanında" etkinleştirme
* Yönetici erişim geçmişine ve değişiklikler hakkında raporlar yönetici atamaları alma
* Ayrıcalıklı bir rol için erişim hakkında uyarı alın

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Azure AD Privileged Identity Management rollerinde](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: nasıl'ın bir kullanıcı rolü ekleme veya kaldırma](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Kimlik koruması
Azure AD kimlik koruması risk olaylarına ve olası güvenlik açıklarını kuruluşunuzdaki kimlikleri etkileyen birleştirilmiş bir görünüm sağlayan bir güvenlik hizmetidir. Kimlik koruma, var olan Azure Active Directory'nin (Azure AD anormal etkinlik raporları kullanılabilir) anomali algılama özelliklerinden yararlanır ve anormallikleri gerçek zamanlı olarak algılayabilir yeni risk olayı türleri sunar.

Daha fazla bilgi edinin:

* [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD ve kimlik göster: kimlik koruması Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Karma Kimlik Yönetimi
Şirket içi ve bulut kimlik doğrulaması ve yetkilendirme konum bağımsız olarak tüm kaynaklar için tek bir kullanıcı kimliği oluşturma, kimlik Microsoft'un yaklaşımı yayar.

Daha fazla bilgi edinin:

* [Karma kimlik teknik incelemesi](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Active Directory ekip blogu](https://blogs.technet.microsoft.com/ad/)
