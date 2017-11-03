---
title: "Azure Kimlik Yönetimi temelleri | Microsoft Docs"
description: "Bulut tabanlı kimlikleri üzerinde denetimi korumak için en iyi yolu ve içine nasıl ve ne zaman kullanıcıların Kurumsal uygulamalara ve verilere erişim görünürlük sunulmuştur."
keywords: 
author: jeffgilb
manager: femila
ms.reviewr: jsnow
ms.author: jeffgilb
ms.date: 07/05/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 52f05ee8a5c07fc008da40aef12d1ad8e8136429
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="fundamentals-of-azure-identity-management"></a>Azure Kimlik Yönetimi temelleri
Bulutta ve aygıtlarda, kurumsal ağ dışından daha da fazla şirket dijital kaynakları dinamik olarak bir harika bulut tabanlı kimlik ve erişim yönetimi çözümü zorunlu durumundadır. Bulut tabanlı kimlikleri üzerinde denetimi korumak için en iyi yolu ve içine nasıl ve ne zaman kullanıcıların Kurumsal uygulamalara ve verilere erişim görünürlük sunulmuştur.

Microsoft güvenliğini sağlamak için bulut tabanlı kimlikleri bir on üzerinden ve şimdi ile [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/active-directory-editions), bu aynı koruma sistemleri için kullanılabilir. Azure AD ile kurumsal yöneticiler kolaylıkla kullanıcı ve yönetici sorumluluk daha iyi güvenlik ve idare zamankinden emin olabilirsiniz.

Azure AD Premium olan tüm uygulamalar, kimlik koruması için bir güvenli kimlik sağlayan bir bulut tabanlı kimlik ve erişim yönetimi çözümüyle gelişmiş koruma özellikleri (tarafından geliştirilmiş [Microsoft Intelligence güvenlik grafik](https://www.microsoft.com/en-us/security/intelligence)) ve ayrıcalıklı kimlik yönetimi. Değil yalnızca başka bir izleme veya Raporlama Aracı, Azure AD Premium gerçek zamanlı kullanıcı kimlikleri koruyabilir ve kuruluşunuzun verilerini korumak için risk tabanlı, Uyarlamalı erişim ilkeleri oluşturmanızı sağlar.

Azure AD kimlik yönetimi ve koruması hızlı bir genel bakış için kısa bu videoyu izleyin:
<iframe width="560" height="315" src="https://www.youtube.com/embed/9LGIJ2-FKIM" frameborder="0" allowfullscreen></iframe>

Microsoft, her yerde geçen bir kimlik değil yalnızca sağlar, ancak da otomatik hale getirmek için Araçlar güvenli hale getirmek ve yönetmek BT kuruluşunuz içinde. Hatta geliştirilirken bulut sonra hala yönetmek ve kullanıcı parolaları sıfırlama, kullanıcı Grup Yönetimi ve uygulama isteklerini Yardım Masası çağrıları gibi BT görevleri denetlemek için isteğe bağlı yoktur. Daha fazla şey karmaşıklaştırarak, Çalışanlar şimdi kişisel cihazlarını çalışma ve kullanıma hazır SaaS uygulamaları kullanarak getiren. Bu uygulamalarını Bakımı denetime kurumsal veri merkezleri ve genel bulut platformda önemli zor hale getirir.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Azure AD ile şirket içi Active Directory connect ve Office 365
Şirket içi Active Directory'de büyük yatırımlar yapmış kuruluşlar kendi şirket içi dizinlerinde Azure AD ile ile tümleştirerek bu Yatırımlar buluta genişletebilir [karma Kimlik Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Bunun yapılması, kullanıcılarınızın daha üretken konum bağımsız olarak kaynaklara erişmek için ortak bir kimlik sağlayarak hale getirir. Kullanıcıların ve kuruluşların çoklu oturum açma hem şirket içi kaynaklara erişmek ve Office 365 gibi hizmetler bulut üzerinde (SSO) kullanabilirsiniz.

[Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) bitti tümleştirme almanız gereken tek araç. Azure AD Connect kimlik eşitleme gereksinimlerinizi desteklemek için özellikleri sağlar ve DirSync ve Azure AD eşitleme gibi kimlik tümleştirme araçlarının eski sürümlerinin yerine geçer. Azure AD Connect ile şirket içi ve Azure AD arasındaki eşitleme ve kimlik yönetimi etkin aracılığıyla:

- Eşitleme - Bu bileşen; kullanıcı, grup ve diğer nesnelerin oluşturulmasından sorumludur. Aynı zamanda şirket içi kullanıcılarınıza ve gruplarınıza ilişkin kimlik bilgilerinin bulut ile eşleşmesini sağlamaktan da sorumludur. Parola geri yazma, bir kullanıcı parolalarını Azure AD içinde güncelleştirdiğinde şirket içi dizinlere eşitlenmiş tutmak için de etkinleştirilebilir.
- AD FS - federasyon, şirket içi kullanarak karma bir ortamı yapılandırmak için kullanılan Azure AD Connect tarafından sağlanan isteğe bağlı bir özellik değil AD FS altyapısı. Federasyon, çoklu oturum açma, akıllı kart veya üçüncü taraf MFA ve AD oturum açma ilkesini zorlama gibi karmaşık dağıtımlar gerçekleştiren kuruluşlar tarafından kullanılabilir.
- Sistem durumu izleme - [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health) izleme olanağı sağlayarak ve bu etkinliği görüntülemek için Azure portalında merkezi bir konum sağlar.

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Üretkenliği artırmak ve Self Servis ve çoklu oturum açma deneyimlerini ile Yardım Masası maliyetlerini azaltma

Tek bir kullanıcı adı ve parolayı ve tutarlı bir deneyim her aygıttan olduğunda daha üretken çalışanlardır. Bunlar aynı zamanda bunların ne zaman gerçekleştirebilir gibi Self Servis görevleri zamandan [Unutulan parolayı sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords) veya Yardım Masası Yardım beklemeden bir uygulamaya erişim isteme.

Azure AD [şirket içi Active Directory genişletir](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) bulutunu hem kendi etki alanına katılmış aygıtlar, şirket kaynaklarına ve tüm web ve SaaS uygulamaları için ihtiyaç duydukları birincil kuruluş hesaplarıyla kullanılmak üzere kullanıcıları etkinleştirme işlerini halletmek için kullanın. Birden çok kullanıcı adları ve parolalar kümesini hatırlamak zorunda değil ek olarak, kullanıcıların uygulama erişimi de otomatik olarak sağlanan (XML'deki sağlanan veya) kuruluş grup üyeliklerini ve durumlarını çalışan olarak göre. Ve galeri uygulamalar veya için geliştirilen ve üzerinden yayımlanan kendi şirket içi uygulamalar bu erişimi denetleyebilir [Azure AD uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="manage-and-control-access-to-corporate-resources"></a>Yönetmek ve şirket kaynaklarına erişimi denetleme
Microsoft kimlik ve erişim yönetimi çözümlerini Yardım BT doğrulama ek düzeyleri gibi etkinleştirme uygulamaları ve kaynaklara erişim kurumsal veri merkezi genelinde ve buluta koruma [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)ve [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Raporlama, Denetim ve yardımcı olası güvenlik sorunlarını azaltmak uyarı Gelişmiş Güvenlik ile izleme şüpheli etkinlik.

Azure AD Premium koşullu erişim ilkeleri, kuruluş yöneticisi, ilke tabanlı uygulama için erişim kurallarının tüm Azure AD bağlı (SaaS uygulamaları, Bulut veya şirket içi web uygulamaları çalıştıran özel uygulamalar) oluşturma olanağı verir. Azure AD, bu ilkeler gerçek zamanlı değerlendirir ve bir kullanıcı bir uygulamaya erişmeye çalıştığında bunları zorlar. Azure kimlik koruma ilkeleri şüpheli etkinlik belirlenirse, otomatik olarak eyleme olanak tanır. Bu eylemler yüksek risk kullanıcılara erişimi engelleme, çok faktörlü kimlik doğrulamasını zorunlu ve kimlik bilgilerini tehlikeye gibi görünüyor, kullanıcı parolalarını sıfırlama içerebilir.


## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Ayrıcalıklı Kimlik Yönetimi

[Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started), Bul olanak tanır Azure Active Directory Premium P2 teklifi ile dahil kısıtlayın ve izleyin yönetici hesapları ve bunların Azure Active Directory'yi ve diğer Microsoft kaynaklara erişim Çevrimiçi Hizmetler. Ayrıca, tam süre ihtiyacınız isteğe bağlı yönetim erişimi yönetmenize yardımcı olur.

Böylece yöneticiler çok faktörlü kimlik doğrulamalı, geçici ayrıcalıkların kendi için önceden yapılandırılmış bir süre için normal bir kullanıcı hesaplarına döndürmeden önce isteyebilir isteğe bağlı yönetici hakları privileged Identity Management zorlayabilir durumu.

## <a name="benefits-of-azure-identity"></a>Azure kimlik yararları

Azure Kimlik Yönetimi ile şunları yapabilirsiniz:

-   Oluşturma ve kullanıcılara, gruplara ve cihazlara ile eşitlenmiş şekilde kalmasının tüm kuruluşunuz genelinde her kullanıcı için tek bir kimliği yönetme [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

-   Önceden tümleştirilmiş SaaS uygulamaları binlerce de dahil olmak üzere uygulamalarınıza tek oturum açma erişim sağlamak veya kullanarak şirket içi SaaS uygulamalara güvenli uzaktan erişim sağlama [Azure AD uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

-   Uygulama erişimi güvenliğini kural tabanlı zorlayarak etkinleştirmek [çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) şirket içi ve bulut uygulamaları.

-   Kullanıcı verimliliğini artırmak [Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords), Grup ve erişim isteklerini kullanarak uygulama ve [MyApps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-user-help).

-   Yararlanmak [yüksek kullanılabilirlik ve güvenilirlik](https://docs.microsoft.com/azure/architecture/resiliency/high-availability-azure-applications) bir dünya çapında, kurumsal düzeyde, bulut tabanlı kimlik ve erişim yönetimi çözümü.

## <a name="next-steps"></a>Sonraki adımlar
[Azure kimlik çözümleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions)