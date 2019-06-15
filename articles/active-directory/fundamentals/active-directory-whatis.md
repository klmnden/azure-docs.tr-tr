---
title: Azure Active Directory nedir? -Azure Active Directory | Microsoft Docs
description: Genel bakış ve kavramsal Azure Active Directory hakkında bilgi terminoloji, hangi lisans yok ve daha fazla bilgi için bağlantılar ile ilişkili özelliklerin bir listesi dahil olmak üzere.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.topic: overview
ms.date: 05/08/2019
ms.author: lizross
ms.custom: it-pro, seodec18, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd4c0bbe4e4ef1cfbd4d9da92d6fcfac509f45ab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110416"
---
# <a name="what-is-azure-active-directory"></a>Azure Active Directory nedir?

Azure Active Directory (Azure AD), Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmeti oturum açın ve kaynaklara erişme, çalışanlarınızın yardımcı olur:

- Microsoft Office 365, Azure portalı ve diğer SaaS uygulamalarına binlerce gibi dış kaynaklar.

- Şirket ağına ve intranet, kendi kuruluşunuzun içinde geliştirilen bir bulut uygulamalarının yanı sıra uygulamaları gibi iç kaynaklara.

Çeşitli kullanabileceğiniz [Kurumsal mimarlara serisi için Microsoft Cloud](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources#identity) temel kimlik hizmetlerini azure'da, Azure AD'nin ötesine geçerek, daha iyi anlamak için Posterler ve Office 365.

## <a name="who-uses-azure-ad"></a>Azure AD kullanan?

Azure AD için tasarlanmıştır:

- **BT yöneticileri.** BT yöneticisi, uygulamalarınızın ve iş gereksinimlerinize göre uygulama kaynaklarınızı erişimi denetlemek için Azure AD kullanabilirsiniz. Örneğin, önemli kurumsal kaynaklara erişirken çok faktörlü kimlik doğrulaması gerektirmek için Azure AD kullanın. Ayrıca, Azure AD kullanıcı sağlama, mevcut Windows Server AD ve Office 365 dahil olmak üzere bulut uygulamalarınız arasında otomatik hale getirmek için kullanabilirsiniz. Son olarak, Azure AD'ye otomatik olarak kullanıcı kimliklerini ve kimlik bilgilerinin korunmasına yardımcı olmak ve erişim yönetimi gereksinimlerinizi karşılamak için güçlü araçlar sağlar. Başlamak için oturum açmak için bir [Ücretsiz 30 günlük Azure Active Directory Premium deneme](https://azure.microsoft.com/trial/get-started-active-directory/).

- **Uygulama geliştiricileri.** Uygulama geliştiricisi olarak, Azure AD, standartlara dayalı bir yaklaşım çoklu oturum açma (SSO) uygulamanıza eklemek için önceden var olan bir kullanıcının kimlik bilgileriyle çalışmaya izin verir. Azure AD yardımcı olabilecek API'leri kullanarak mevcut kurumsal veri kişiselleştirilmiş uygulama deneyimleri oluşturmak da sağlar. Başlamak için oturum açmak için bir [Ücretsiz 30 günlük Azure Active Directory Premium deneme](https://azure.microsoft.com/trial/get-started-active-directory/). Daha fazla bilgi için atabilirsiniz [geliştiriciler için Azure Active Directory](../develop/index.yml).

- **Microsoft 365, Office 365, Azure veya Dynamics CRM Online aboneleri.** Abone olarak, Azure AD zaten kullanıyorsunuz. Microsoft 365, Office 365, Azure ve Dynamics CRM Online her kiracıya otomatik olarak Azure AD kiracısı olur. Tümleşik bulut uygulamalarınıza erişimi yönetmek hemen başlayabilirsiniz.

## <a name="what-are-the-azure-ad-licenses"></a>Azure AD lisansı nedir?

Office 365 veya Microsoft Azure gibi Microsoft Online iş Hizmetleri için oturum açma ve kimlik koruması ile yardımcı olmak için Azure AD gerektirir. Herhangi bir Microsoft Online iş hizmete abone olursanız, Azure AD ile tüm ücretsiz özelliklerine erişimi otomatik olarak alın.

Azure AD uygulamanızı geliştirmek için de Azure Active Directory temel, Premium P1 veya Premium P2 lisansı için yükselterek Ücretli özellikleri ekleyebilirsiniz. Lisansları Ücretli azure AD yerleşik olarak bulunur, var olan boş dizin üzerinde mobil kullanıcılarınız için Self Servis, Gelişmiş izleme, güvenlik raporlama ve güvenli erişim sağlama.

>[!Note]
>Bu lisanslar için fiyatlandırma seçeneklerini görmek [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).
>
>Azure Active Directory Premium P1, Premium P2 ve Azure Active Directory Temel şu an için Çin'de desteklenmemektedir. Azure AD fiyatlandırma hakkında daha fazla bilgi için [Azure Active Directory Forumu](https://azure.microsoft.com/support/community/?product=active-directory).

- **Ücretsiz Azure Active Directory.** Kullanıcı ve Grup Yönetimi, şirket içi dizin eşitleme, temel raporları ve çoklu oturum açma, Azure, Office 365 ve çok sayıda popüler SaaS uygulamaları arasında sağlar.

- **Azure Active Directory Temel.** Temel, ücretsiz özelliklerin yanı sıra, bulut odaklı uygulama erişimi, Grup tabanlı erişim yönetimi, Self Servis parola sıfırlama bulut uygulamaları ve Azure AD uygulama yayımladığınız sayesinde şirket içi Azure AD kullanarak web uygulamaları proxy'si de sağlar.

- **Azure Active Directory Premium P1.** Ücretsiz ve temel özelliklerine ek olarak, P1 ayrıca hem şirket içi ve bulut kaynaklarında karma olanak tanır. Gelişmiş yönetimini de destekler, dinamik gruplar, Self Servis Grup Yönetimi, Microsoft Identity Manager (bir şirket içi kimlik ve erişim Yönetim Paketi) ve bulut geri yazma özellikleri gibi Self Servis parola sıfırlama için ver Şirket içi kullanıcılarınızın.

- **Azure Active Directory Premium P2.** Ücretsiz, temel ve P1 özelliklerin yanı sıra, P2 sunduğu [Azure Active Directory kimlik koruması](../identity-protection/enable.md) uygulamalarınızı ve kritik şirket verileri için risk tabanlı koşullu erişim sağlamak için ve [ayrıcalıklı kimlik Yönetim](../privileged-identity-management/pim-getting-started.md) bulmak, kısıtlamak ve yöneticiler ve bu kimliklerin kaynaklara erişimini izlemenize yardımcı olması için ve gerektiğinde tam zamanında erişim sağlamak için.

- **"Kullandıkça Öde" özelliği lisanslar.** Ayrıca, Azure Active Directory iş müşteriye (B2C) gibi ek özellik lisans alabilirsiniz. B2C kimlik sağlama ve erişim yönetimi çözümleri, müşteriye dönük uygulamalarınız için yardımcı olabilir. Daha fazla bilgi için [Azure Active Directory B2C belgeleri](../../active-directory-b2c/index.yml).

Azure aboneliğinin Azure AD'ye ilişkilendirme hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure Active Directory'ye bir Azure aboneliği ekleme veya ilişkilendirme](active-directory-how-subscriptions-associated-directory.md) ve kullanıcılarınıza lisansları atama hakkında daha fazla bilgi için bkz. [nasıl yapılır: Atayın veya Azure Active Directory lisansları kaldırabilir](license-users-groups.md).

## <a name="terminology"></a>Terminoloji

Azure AD daha iyi anlamak için ve kendi belgeleri, aşağıdaki koşulları incelemeniz önerilir.

|Kavram veya sözleşme|Açıklama|
|---------------|-----------|
|Azure aboneliği| Azure bulut Hizmetleri için kullandığınız ödeme yöntemi için kullanılır. Birçok abonelik olabilir ve bir kredi kartına bağlı.|
|Azure kiracısı| Kuruluşunuz Microsoft Azure, Microsoft Intune veya Office 365 gibi Microsoft bulut hizmeti aboneliği kaydolduğunda otomatik olarak oluşturulan Azure ad ayrılmış ve güvenilir bir örneği. Bir Azure kiracısı, tek bir kuruluşu temsil eder.|
|Tek kiracılı| Ayrılmış bir ortamda diğer hizmetlere erişmek azure kiracılarında, tek bir kiracı olarak kabul edilir.|
|Çok Kiracılı| Diğer hizmetler paylaşılan bir ortamda birden çok kuruluşta, erişimi azure kiracılarında, çok kiracılı olarak kabul edilir.|
|Azure AD dizini|Her bir Azure kiracısına sahip adanmış ve güvenilir bir Azure AD dizini. Azure AD dizini, kiracının kullanıcıları, grupları ve uygulamaları içerir ve kimliği gerçekleştirin ve yönetim işlevleri için Kiracı kaynaklarına erişmek için kullanılır.|
|Azure AD hesabı | Azure AD oluşturulan bir kimlik veya Office 365 gibi başka bir Microsoft bulut hizmeti. Azure AD'de depolanan ve kuruluşunuzun bulut hizmeti abonelikleri için erişilebilir Kimlikleridir. Bu hesap bazen çalışma adlandırılır ya da Okul hesabı.|
|Özel etki alanı|Her yeni Azure AD dizini bir ilk etki alanı adı ile gelir domainname.onmicrosoft.com. Buna ek olarak, ilk adı listesi için kuruluşunuzun kaynaklarına erişmek için iş ve kullanıcılarınızı yapmak için kullandığınız adlarında adlarını kullanın, kuruluşunuzun etki alanı da ekleyebilirsiniz. Özel etki alanı adları ekleme yardımcı olur, kullanıcılarınızın tanıdığı gibi kullanıcı adları oluşturmak için alain@contoso.com.|
|Hesap Yöneticisi|Bu Klasik Abonelik Yöneticisi rolü kavramsal olarak bir abonelik fatura sahibidir. Bu rolün erişimi olan [Azure hesap Merkezi](https://account.azure.com/Subscriptions) ve bir hesaptaki tüm abonelikleri yönetmenize imkan sağlar. Daha fazla bilgi için [Klasik Abonelik Yöneticisi rolleri, Azure rol tabanlı erişim denetimi (RBAC) rollerini ve Azure AD yönetici rollerini](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Hizmet Yöneticisi|Bu Klasik Abonelik Yöneticisi rolü erişim dahil olmak üzere tüm Azure kaynaklarını yönetmenizi sağlar. Bu rol, abonelik kapsamında sahip rolüne atanan kullanıcı eşdeğer erişebilir. Daha fazla bilgi için [Klasik Abonelik Yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rollerini](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Sahip|Bu rolü dahil olmak üzere tüm Azure kaynaklarını yönetmenize yardımcı olur. Bu rol, Azure kaynaklarına ayrıntılı erişim yönetimi sağlar, rol tabanlı erişim denetimi (RBAC) adı verilen yeni bir yetkilendirme sistemine yerleşik olarak bulunur. Daha fazla bilgi için [Klasik Abonelik Yöneticisi rolleri, Azure RBAC rolleri ve Azure AD yönetici rollerini](../../role-based-access-control/rbac-and-directory-admin-roles.md).|
|Azure AD Genel yöneticisi|Bu yönetici rolü otomatik olarak, Azure AD kiracısını oluşturan kişiye atandı. Genel Yöneticiler tüm Azure AD için yönetim işlevlerini ve federasyona tüm hizmetleri Exchange Online, SharePoint Online ve Skype gibi Azure AD'ye çevrimiçi yapabilirsiniz. Birden çok genel Yöneticiler olabilir, ancak yalnızca genel Yöneticiler, kullanıcılara yönetici rolleri (başka genel Yöneticiler atama dahil olmak üzere) atayabilirsiniz.<br><br>**Not**<br>Bu yönetici rolü genel yönetici Azure Portalı'nda çağrılır, ancak adlandırılır **şirket Yöneticisi** Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell.<br><br>Çeşitli yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure Active Directory'de Yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).|
|Microsoft hesabı (olarak da bilinen MSA)|Tüketici yönelimli Microsoft ürünlerine erişim sağlar ve bulut Hizmetleri, Outlook, OneDrive, Xbox LIVE veya Office 365 gibi kişisel hesaplar. Microsoft hesabınızı oluşturulur ve Microsoft tarafından çalıştırılan Microsoft tüketici kimlik hesap sistemi depolanır.|

## <a name="which-features-work-in-azure-ad"></a>Hangi özellikleri Azure AD'de çalışıyor?

Azure AD lisansınızın seçtikten sonra kuruluşunuz için bazılarını veya tümünü aşağıdaki özelliklere erişim elde edersiniz:

|Kategori|Açıklama|
|-------|-----------|
|Uygulama yönetimi|Uygulama Ara sunucusu kullanarak Bulut ve şirket içi uygulamalarınızı yönetin çoklu oturum açma, uygulamalarım portalında (erişim paneli olarak da bilinir) ve yazılım hizmet (SaaS) uygulamaları olarak. Daha fazla bilgi için [güvenli uzaktan erişim sağlamak şirket içi uygulamalara](../manage-apps/application-proxy.md) ve [uygulama yönetimi belgeleri](../manage-apps/index.yml).|
|Kimlik Doğrulaması|Azure Active Directory Self Servis parola sıfırlama, çok faktörlü kimlik doğrulaması, özel yasaklı parola listesi ve akıllı kilitleme yönetin. Daha fazla bilgi için [Azure AD kimlik doğrulaması belgeleri](../authentication/index.yml).|
|İşletmeden işletmeye (B2B)|Konuk kullanıcıları ve dış iş ortakları, şirket verilerinizin kontrolünü korurken yönetin. Daha fazla bilgi için [Azure Active Directory B2B belgelerini](../b2b/index.yml).|
|Müşteriye (B2C)|Özelleştirme ve denetim nasıl, kullanıcıların oturum açın ve uygulamalarınızı kullanırken profillerini yönetme. Daha fazla bilgi için [Azure Active Directory B2C belgeleri](../../active-directory-b2c/index.yml).|
|Koşullu Erişim|Bulut uygulamalarınıza erişimi yönetin. Daha fazla bilgi için [Azure AD koşullu erişim belgelerine](../conditional-access/index.yml).|
|Geliştiriciler için Azure Active Directory|Tüm Microsoft kimliklerini oturum, Microsoft Graph, diğer Microsoft APIs veya özel API'leri çağırmak için belirteçleri almak uygulamalar oluşturun. Daha fazla bilgi için [Microsoft kimlik Platformu (geliştiriciler için Azure Active Directory)](../develop/index.yml).|
|Cihaz Yönetimi|Bulutta veya şirket içi cihazlarınızı şirket verilerinize nasıl erişim yönetin. Daha fazla bilgi için [Azure AD cihaz Yönetimi belgeleri](../devices/index.yml).|
|Etki alanı hizmetleri|Azure sanal makinelerini, etki alanı denetleyicileri kullanmak zorunda kalmadan bir etki alanına katılın. Daha fazla bilgi için [Azure AD Domain Services belgeleri](../../active-directory-domain-services/index.yml).|
|Kurumsal kullanıcılar|Lisans ataması, uygulamalara erişimi yönetme ve temsilciler gruplar ve yönetici rolleri kullanarak ayarlayın. Daha fazla bilgi için [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).|
|Karma kimlik|Kimlik doğrulaması ve yetkilendirme (Bulut veya şirket içi) konumdan bağımsız olarak tüm kaynaklar için bir tek kullanıcı kimlik sağlamak için Azure Active Directory Connect ve Connect Health kullanın. Daha fazla bilgi için [karma kimlik belgeleri](../hybrid/index.yml).|
|Kimlik idaresi|Kuruluşunuzun kimlik çalışan, iş ortağı, satıcı, hizmet ve uygulama erişim denetimleri aracılığıyla yönetin. Erişim gözden geçirmeleri de gerçekleştirebilirsiniz. Daha fazla bilgi için [Azure AD kimlik İdaresi belgelerinde](../governance/identity-governance-overview.md) ve [Azure AD erişim gözden geçirmeleri](../governance/access-reviews-overview.md).|
|Kimlik koruması|Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını kuşkulu eylemleri için yanıt vermek için ilkeler yapılandırın ve ardından bunları gidermek için uygun eylemi gerçekleştirin algılayın. Daha fazla bilgi için [Azure AD kimlik koruması](../identity-protection/index.yml).|
|Azure kaynakları için yönetilen kimlikler|Azure hizmetlerinizi otomatik olarak yönetilen bir kimlik ile anahtar kasası dahil olmak üzere, herhangi bir Azure AD tarafından desteklenen kimlik doğrulama hizmeti kimlik doğrulaması Azure AD'de sağlar. Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir?](../managed-identities-azure-resources/overview.md).|
|Privileged Identity management (PIM)|Yönetebilir, denetleyebilir ve erişimi kuruluşunuz içinde izleyin. Bu özellik, Azure ad'deki ve Azure ve diğer Microsoft Çevrimiçi Hizmetleri olan Office 365 veya Intune gibi erişim içerir. Daha fazla bilgi için [Azure AD Privileged Identity Management](../privileged-identity-management/index.yml).|
|Raporlar ve izleme|Ortamınızdaki güvenlik ve kullanım desenleri hakkında Öngörüler elde edin. Daha fazla bilgi için [Azure Active Directory raporlar ve izleme](../reports-monitoring/index.yml).|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory Premium'a kaydolma](active-directory-get-started-premium.md)

- [Azure aboneliğinin Azure Active Directory'ye ilişkilendirin](active-directory-how-subscriptions-associated-directory.md)

- [Azure Active Directory'ye erişmek ve yeni Kiracı oluşturma](active-directory-access-create-new-tenant.md)

- [Azure Active Directory Premium P2 özellik dağıtım denetim listesi](active-directory-deployment-checklist-p2.md)
