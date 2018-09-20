---
title: Azure Active Directory'de yönetici rolleri başvurusu | Microsoft Docs
description: Yönetici rolü kullanıcı ekleme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/19/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 04deb1168c8c5c0977d0f20c9307ce10d2d12d35
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466124"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Azure Active Directory’de yönetici rolü atama

Azure Active Directory (Azure AD) kullanarak, farklı işlevler sunmak için ayrı yöneticileri atayabilirsiniz. Yöneticiler, ekleme veya kullanıcıları için değiştirme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi görevleri gerçekleştirmek için Azure AD Portalı'nda belirlenebilir.

## <a name="details-about-the-global-administrator-role"></a>Genel yönetici rolüne hakkında ayrıntılar

Genel yönetici, tüm yönetim özelliklerine erişebilir. Varsayılan olarak, bir Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca genel Yöneticiler diğer yönetici rollerini atayabilir.

## <a name="assign-or-remove-administrator-roles"></a>Atamayı veya kaldırmayı yönetici rolleri

Azure Active Directory'de bir kullanıcıya yönetici rollerini atama hakkında bilgi edinmek için [bir kullanıcı Azure Active Directory'de yönetici rolleri atama](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki Yönetici rollerini kullanılabilir:

* **[Uygulama Yöneticisi](#application-administrator)**: Bu roldeki kullanıcılar oluşturabilir ve kurumsal uygulamaları, uygulama kayıtları ve uygulama proxy'si ayarları tüm özelliklerini yönetebilir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenmez.

* **[Uygulama geliştiricisi](#application-developer)**: Bu roldeki kullanıcılar, uygulama kayıtları oluşturabilir, "Kullanıcılar uygulamaları kaydedebilir" ayarı Hayır olarak ayarlayın Bu rolü üyelerinin kendi adınıza onay de sağlar. zaman "Kullanıcı izni verebilir uygulamalara kendileri adına şirket verilerine erişme" ayarı Hayır olarak ayarlayın Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenir.

* **[Faturalama Yöneticisi](#billing-administrator)**: satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

* **[Bulut uygulaması Yöneticisi](#cloud-application-administrator)**: Bu roldeki kullanıcılar, uygulama proxy'si yönetme olanağı hariç uygulama yöneticisi rolü aynı izinlere sahip. Bu rol oluşturmak ve kurumsal uygulamaları ve uygulama kayıtlarını tüm yönlerini yönetmek için yeteneği verir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenmez.

* **[Bulut cihaz Yöneticisi](#cloud-device-administrator)**: Bu roldeki kullanıcılar etkinleştirmek, devre dışı bırakabilir ve Azure AD'de cihazları silin ve Windows 10 BitLocker anahtarları Azure Portalı'nda (varsa) okuyun. Rol, cihazdaki diğer özelliklerini yönetmek için izinleri tanımaz.

* **[Uyumluluk Yöneticisi](#compliance-administrator)**: Bu role sahip kullanıcılar, Office 365 güvenlik ve Uyumluluk Merkezi ve Exchange yönetici merkezini de yönetim izinlerine sahip. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Koşullu Erişim Yöneticisi](#conditional-access-administrator)**: Bu role sahip olan kullanıcılar Azure Active Directory koşullu erişim ayarlarını yönetme olanağı vardır.
  > [!NOTE]
  > Azure'da Exchange ActiveSync koşullu erişim ilkesi dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **[Cihaz yöneticileri](#device-administrators)**: Bu rol ataması yalnızca ek yerel yönetici olarak kullanılabilir [cihaz ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Bu role sahip kullanıcılar, Azure Active Directory'ye katılan tüm Windows 10 cihazları üzerinde yerel makine yöneticisi olur. Azure Active Directory'de cihaz nesnelerini yönetme olanağına sahip değildir. 

* **[Dizin okuyucular](#directory-readers)**: Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dizin eşitlemesi hesapları](#directory-synchronization-accounts)**: kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil hedeflenen veya herhangi bir kullanım için desteklenir.

* **[Dizin yazıcıları](#directory-writers)**: Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dynamics 365 Hizmet Yöneticisi / CRM Hizmet Yöneticisi](#dynamics-365-service-administrator)**: Bu role sahip kullanıcılar, destek biletlerini yönetme olanağı yanı sıra Microsoft Dynamics 365 hizmet mevcut olduğunda Online içinde genel izinlere sahip ve Hizmet durumunu izleyebilir. Daha fazla bilgiye [kiracınızı yönetmek için Hizmet Yöneticisi rolü kullanmak](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).

* **[Exchange Hizmeti Yöneticisi](#exchange-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Genel yönetici / şirket Yöneticisi](#company-administrator)**: Bu role sahip kullanıcılar, Exchange Online gibi Azure Active Directory kimlikleri kullanmak hizmetlerinin yanı sıra Azure Active Directory, tüm yönetim özelliklerine erişim sahibi SharePoint Online ve Skype Kurumsal çevrimiçi. Azure Active Directory kiracısı için kaydolan kişi genel yönetici olur. Yalnızca genel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel Yöneticiler, herhangi bir kullanıcı ve diğer tüm yöneticilerin parolasını sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" tanımlanır. "Genel yönetici" olarak [Azure portalında](https://portal.azure.com).
  >
  >

* **[Konuk davet eden](#guest-inviter)**: Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetlerini yönetebilir, **üyelerini davet edebildiğiniz** kullanıcı ayarı Hayır olarak ayarlayın B2B işbirliği hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Tüm diğer izinleri içermez.

* **[Bilgi Koruma Yöneticisi](#information-protection-administrator)**: Bu role sahip kullanıcılar Azure Information Protection hizmetinde tüm izinlere sahiptir. Bu rol, Azure Information Protection ilkesi için etiketleri yapılandırma, koruma şablonlarını yönetme ve koruma etkinleştirme sağlar. Bu rol, kimlik koruma Merkezi, Privileged Identity Management, Office 365 hizmet durumunu izleme, veya Office 365 güvenlik ve uyumluluk Merkezi'nde herhangi bir izni tanımaz.

* **[Intune Hizmet Yöneticisi](#intune-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Intune Online içinde genel izinlere sahiptir. Ayrıca, bu rol, ilke ilişkilendirmek yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme olanağı içerir. Daha fazla bilgiye [Intune rol tabanlı yönetim denetimi (RBAC)](https://docs.microsoft.com/intune/role-based-access-control)

* **[Lisans Yöneticisi](#license-administrator)**: Bu roldeki kullanıcılar eklemek, kaldırmak ve güncelleştirme lisans atamalarında kullanıcıları, grupları (Grup tabanlı lisanslama kullanarak) ve kullanıcılar üzerindeki kullanım konumu yönetmek. Rol, satın alma veya Aboneliklerini yönetmek, oluşturma veya grupları yönetme veya oluşturma veya ötesinde kullanım konumu yönetme olanağı tanımaz.

* **[İleti Merkezi okuyucu](#message-center-reader)**: Bu roldeki kullanıcılar, bildirimler ve danışmanlık sistem güncelleştirmeleri izleyebilirsiniz [Office 365 ileti Merkezi](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) kuruluşlarında Exchange, Intune gibi yapılandırılmış hizmetleri ve Microsoft Teams. İleti Merkezi okuyucular Haftalık e-posta özetler gönderilerin, güncelleştirmeleri almak ve Office 365 ileti merkezi gönderileri paylaşabilirsiniz. Azure AD'de bu role atanan kullanıcılar yalnızca salt okunur kullanıcılar ve gruplar gibi Azure AD Hizmetleri erişebilir. 

* **[İş ortağı Tier1 desteği](#partner-tier1-support)**: kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[İş ortağı Tier2 desteği](#partner-tier2-support)**: kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[Parola Yöneticisi / Yardım Masası Yöneticisi](#helpdesk-administrator)**: Bu role sahip olan kullanıcılar parolaları değiştirme, yenileme belirteçleri geçersiz kılmak, hizmet isteklerini yönetebilir ve hizmet durumunu izleyin. Yardım Masası yöneticileri parolaları değiştirebilir ve yenileme belirteçleri yalnızca kullanıcıların ve diğer Yardım Masası yöneticileri için geçersiz. Bir yenileme belirteci geçersiz kılmalarını, kullanıcı yeniden oturum açmak için zorlar.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Yardım Masası Yöneticisi" tanımlanır. "Parola Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).
  >
  >
  
* **[Power BI Hizmet Yöneticisi](#power-bi-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Power BI içinde genel izinlere yanı sıra destek biletlerini yönetebilir ve hizmet durumu izleme olanağı vardır. Daha fazla bilgiye [Power BI yönetici rolünü anlama](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator)**: Bu role sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rol, Privileged Identity Management'ın tüm yönlerini yönetilmesine izin verir.

* **[Rapor okuyucu](#reports-reader)**: Bu role sahip kullanıcılar, kullanım verileri ve raporları Panoda, Office 365 Yönetim merkezine ve benimseme bağlam paketini Power bı'da raporu görüntüleyebilirsiniz. Ayrıca, rol oturum açma için erişim sağlayan raporları ve etkinlik Azure AD'de ve Microsoft Graph tarafından döndürülen veri raporlama API'si. Rapor okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar, ayarları veya Exchange gibi ürün belirli Yönetim Merkezleri erişimi yapılandırmak için bir yönetici izinlere sahip değilsiniz. 

* **[Güvenlik Yöneticisi](#security-administrator)**: Bu role sahip kullanıcılar tüm güvenlik okuyucu rolünün yanı sıra, güvenlikle ilgili şu hizmetler için yapılandırma yönetme olanağı, salt okunur izinleri vardır: Azure Active Directory kimlik koruması, Azure Information Protection ve Office 365 güvenlik ve Uyumluluk Merkezi. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  | İçinde | Yapabilirsiniz |
  | --- | --- |
  | Kimlik Koruma Merkezi |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>Parola sıfırlama dışında tüm IPC işlemleri gerçekleştirmeye ek olarak, yeteneği. |
  | Privileged Identity Management |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>**Olamaz** Azure AD rol üyelikleri veya ayarlarını yönetin. |
  | <p>Office 365 hizmet durumunu izleme</p><p>Office 365 Güvenlik ve Uyumluluk Merkezi |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>(Kötü amaçlı yazılım ve virüs koruması, kötü amaçlı URL yapılandırma, URL izleme, vb.) için Gelişmiş tehdit Koruması özelliği, tüm ayarlarını yapılandırabilirsiniz. |
  
* **[Güvenlik okuyucusu](#security-reader)**: Bu role sahip olan kullanıcılar Azure Active Directory, kimlik koruması, Privileged Identity Management, yanı sıra Azure Active Directory okuma yeteneği tüm bilgiler dahil genel salt okunur erişime sahip oturum açma raporlarını ve Denetim günlükleri. Rol, ayrıca Office 365 güvenlik ve uyumluluk Merkezi'nde salt okunur izni verir. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  | İçinde | Yapabilirsiniz |
  | --- | --- |
  | Kimlik Koruma Merkezi |Tüm güvenlik raporları ve güvenlik özellikleri için ayarlar bilgilerini okuyun<ul><li>İstenmeyen postadan koruma<li>Şifreleme<li>Veri kaybı önleme<li>Kötü amaçlı yazılımdan koruma<li>Gelişmiş tehdit koruması<li>Avından<li>Mailflow kuralları |
  | Privileged Identity Management |<p>Tüm bilgileri salt okunur erişim yapılandırmalarla kullanıma sahip Azure AD PIM'de: güvenlik ilkeleri ve Azure AD rol atamaları için raporları gözden geçirir ve gelecekte Azure AD rol atamanız yanı sıra senaryoları için ilke verilere ve raporlara erişimi'ni okuyun.<p>**Olamaz** herhangi bir değişiklik yapmak veya Azure AD PIM için kaydolun. PIM'ın portalında veya PowerShell aracılığıyla kullanıcı bunları adayı, birisi Bu roldeki ek roller (örneğin, genel yönetici veya ayrıcalıklı Rol Yöneticisi) etkinleştirebilirsiniz. |
  | <p>Office 365 hizmet durumunu izleme</p><p>Office 365 Güvenlik ve Uyumluluk Merkezi</p> |<ul><li>Okuma ve uyarılar yönetme<li>Güvenlik ilkelerini okuma<li>Tehdit zekası, bulut uygulaması bulma ve arama ve Araştır Karantinasında okuyun<li>Tüm raporları okuma |

* **[Hizmet desteği Yöneticisi](#service-support-administrator)**: Bu role sahip kullanıcılar açabilir destek istekleri ile Microsoft Azure ve Office 365 Hizmetleri ve Azure portalı ve Office 365 Yönetim portalında hizmet panosunu ve ileti merkezini için. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[SharePoint Hizmet Yöneticisi](#sharepoint-service-administrator)**: Bu role sahip kullanıcılar sahip Microsoft SharePoint hizmet mevcut olduğunda Online içinde genel izinlere olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[İşletme için Skype / Lync Hizmet Yöneticisi](#lync-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Skype Kurumsal, içinde genel izinlere sahip, hem de Skype özgü kullanıcı öznitelikleri, Azure Active Yönet Dizin. Ayrıca, bu rol, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir ve takımlar ve Skype kurumsal iş Yönetim Merkezi erişim olanağı verir. Hesap, takımlar için lisanslanmalıdır veya ekipler PowerShell cmdlet'leri çalıştırılamaz. Daha fazla bilgi [hakkında Skype kurumsal iş yöneticisi rolüne](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve lisans bilgileri takımlar [iş ve Microsoft Teams eklenti lisansı için Skype](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. "Skype Kurumsal Hizmet Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).
  >
  >

* **[İletişim yönetici takımlar](#teams-communications-administrator)**: Bu roldeki kullanıcılar, ilgili ses & telefon için Microsoft Teams iş yükü özelliklerini yönetebilir. Bu telefon numarası ataması, ses ve toplantı ilkeleri ve çağrı analizi araç takımı tam erişim için yönetim araçlarını içerir.

* **[Takımlar iletişimleri destek mühendisi](#teams-communications-support-engineer)**: Bu roldeki kullanıcılar Microsoft Teams ve Skype Kurumsal sorun giderme araçları kullanarak kullanıcı iş çağrısı içinde Microsoft Teams & Skype iletişim sorun giderme İş Yönetim Merkezi. Bu roldeki kullanıcılar, ilgili tüm katılımcılar için tam çağrı kayıt bilgileri görüntüleyebilirsiniz.

* **[Takımlar iletişimleri destek uzmanı](#teams-communications-support-specialist)**: Bu roldeki kullanıcılar Microsoft Teams ve Skype Kurumsal sorun giderme araçları kullanarak kullanıcı iş çağrısı içinde Microsoft Teams & Skype iletişim sorun giderme İş Yönetim Merkezi. Bu roldeki kullanıcılar yalnızca bunlar Aranan kullanıcıyı çağrıda kullanıcı ayrıntıları da görüntüleyebilirsiniz.

* **[Hizmet Yöneticisi takımlar](#teams-service-administrator)**: Bu roldeki kullanıcılar, iş Yönetim Merkezi ve ilgili PowerShell modülleri için Microsoft Teams iş yükü Microsoft Teams ve Skype üzerinden tüm özelliklerini yönetebilir. Bu, diğer alanları arasında telefon, Mesajlaşma, toplantılar ve takımlar için ilgili tüm Yönetim Araçlar içerir. Bu rol, ayrıca Office 365 grupları yönetme olanağı verir.

* **[Kullanıcı hesabı yöneticisi](#user-account-administrator)**: Bu role sahip kullanıcılar oluşturabilir ve kullanıcıların ve grupların tüm özelliklerini yönetebilir. Ayrıca bu rol, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı içerir. Bazı kısıtlamalar geçerlidir. Örneğin, bu rolü genel yönetici silinmesine izin vermiyor değil. Kullanıcı hesabı yöneticileri, parolaları değiştirmeniz ve diğer kullanıcı hesabı yöneticileri yalnızca kullanıcıların ve Yardım Masası yöneticileri için yenileme belirteçleri geçersiz. Bir yenileme belirteci geçersiz kılmalarını, kullanıcı yeniden oturum açmak için zorlar.

| Yapabilirsiniz | Bunu yapamazsınız |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüle</p><p>Office destek biletlerini yönetebilir</p><p>Diğer kullanıcı hesabı yöneticileri yalnızca kullanıcıların ve Yardım Masası yöneticileri için parolaları değiştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturma, düzenleme ve kullanıcıları ve grupları silme ve kısıtlamalarla kullanıcı lisanslarını yönetme. İsterse bir genel yönetici silemez veya başka Yöneticiler oluşturamaz.</p> |<p>Office ürünleri için faturalama ve satın alma işlemleri gerçekleştirme</p><p>Etki alanlarını yönet</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Etkinleştirmek veya devre dışı çok faktörlü kimlik doğrulaması</p><p>Denetim günlüklerini görüntüleme</p> |

## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar olan kullanım dışı ve ileride Azure AD'den kaldırılacak.

* AdHoc Lisans Yöneticisi
* Cihaz birleştirme
* Cihaz Yöneticileri
* Aygıt kullanıcıları
* E-posta Adresi Doğrulanan Kullanıcı Oluşturucu
* Posta Kutusu Yöneticisi
* Cihazla Çalışma Alanına Katılma

## <a name="detailed-azure-active-directory-permissions"></a>Azure Active Directory izinlerini ayrıntılı
Aşağıdaki tablolarda her rol için belirtilen Azure Active Directory'de özel izinler açıklanmaktadır. Genel yönetici gibi bazı roller, Azure Active Directory Microsoft Hizmetleri outide ek izinlere sahip olabilir.


### <a name="adhoc-license-administrator"></a>AdHoc Lisans Yöneticisi
Tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Domains/default/Read | Azure Active Directory etki alanlarında temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory'de groups.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Groups/default/Read | Azure Active Directory'de grupları temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory'de groups.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Read | Azure Active Directory'de Groups.Members özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Owners/Read | Azure Active Directory'de Groups.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Settings/Read | Azure Active Directory'de Groups.Settings özelliği okuyun. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Azure Active Directory'de oAuth2PermissionGrants temel özelliklerini okuyun. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Azure Active Directory'de oAuth2PermissionGrants güncelleştirin. |
| Microsoft.aad.Directory/Organization/default/Read | Kuruluşunuz Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliği okuyun. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/default/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliği okuyun. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliği okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |

### <a name="application-administrator"></a>Uygulama Yöneticisi
Tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de Applications.Audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de Applications.Authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/default/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Create | Azure Active Directory'de uygulamalar oluşturun. |
| Microsoft.aad.Directory/Applications/credentials/Update | Azure Active Directory'de Applications.credentials özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de Applications.Permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/read | Azure Active Directory'de appRoleAssignments okuyun. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments silin. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/default/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| microsoft.aad.reports/allEntities/read | Azure AD Raporlarını okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="application-developer"></a>Uygulama Geliştirici
Uygulama kayıtları ayarı uygulamaları bağımsız kullanıcılar olarak kaydedebilir oluşturabilirsiniz.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Azure Active Directory'de uygulamalar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Azure Active Directory'de appRoleAssignments oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory'de oAuth2PermissionGrants oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Azure Active Directory'de servicePrincipals oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |

### <a name="billing-administrator"></a>Faturalama Yöneticisi
Ödeme bilgilerini güncelleştirme gibi sık kullanılan faturalandırma görevlerini gerçekleştirebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Organization/default/Update | Kuruluşunuz Azure Active Directory'de temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-application-administrator"></a>Bulut Uygulaması Yöneticisi
Uygulama Ara Sunucusu hariç tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de Applications.Audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de Applications.Authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/default/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Create | Azure Active Directory'de uygulamalar oluşturun. |
| Microsoft.aad.Directory/Applications/credentials/Update | Azure Active Directory'de Applications.credentials özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de Applications.Permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments silin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/default/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/default/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/default/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| microsoft.aad.reports/allEntities/read | Azure AD Raporlarını okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-device-administrator"></a>Bulut cihaz Yöneticisi
Azure AD'de cihazları yönetmek için tam erişim.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory cihazları silin. |
| Microsoft.aad.Directory/Devices/disable | Cihazları Azure Active Directory'de devre dışı bırakın. |
| Microsoft.aad.Directory/Devices/Enable | Cihazların Azure Active Directory'de sağlayın. |
| microsoft.aad.reports/allEntities/read | Azure AD Raporlarını okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="company-administrator"></a>Şirket Yöneticisi
Azure AD'nin ve Azure AD kimliklerini kullanan Microsoft hizmetlerinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Oluşturma ve administrativeUnits, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/applications/allProperties/allTasks | Oluşturma ve uygulamaları, silin ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Oluşturma ve appRoleAssignments, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Oluşturma ve kişiler, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Oluşturma ve sözleşmeleri, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/devices/allProperties/allTasks | Oluşturmak ve cihazları, silin ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Oluşturma ve directoryRoles, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Oluşturma ve directoryRoleTemplates, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/domains/allProperties/allTasks | Oluşturma ve etki alanları, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/groups/allProperties/allTasks | Oluşturma ve silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Oluşturma ve groupSettings, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Oluşturma ve groupSettingTemplates, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Oluşturma ve loginTenantBranding, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Oluşturma ve oAuth2PermissionGrants, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/organization/allProperties/allTasks | Oluşturma ve kuruluş, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/policies/allProperties/allTasks | Oluşturma ve ilkeleri silin ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Oluşturma ve rol, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Oluşturma ve roleDefinitions, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Oluşturma ve scopedRoleMemberships, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/serviceAction/activateService | Azure Active Directory'de Activateservice hizmet eylem gerçekleştirebilirsiniz |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Azure Active Directory'de Disabledirectoryfeature hizmet eylem gerçekleştirebilirsiniz |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Azure Active Directory'de Enabledirectoryfeature hizmet eylem gerçekleştirebilirsiniz |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Azure Active Directory'de Getavailableextentionproperties hizmet eylem gerçekleştirebilirsiniz |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Oluşturma ve servicePrincipals, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Oluşturma ve subscribedSkus, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/users/allProperties/allTasks | Oluşturma ve kullanıcıları, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |
| microsoft.aad.identityProtection/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.aad.identityProtection standart özellikleri. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.aad.reports/allEntities/allTasks | Azure AD Raporlarını okuyun ve yapılandırın. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm özelliklerini yönetin. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi tüm özelliklerini yönetebilir |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 müşteri kasa tüm özelliklerini yönetebilir |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.protectionCenter/allEntities/allTasks | Office 365 koruma merkezinde tüm özelliklerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm özelliklerini yönetin. |

### <a name="compliance-administrator"></a>Uyumluluk Yöneticisi
Azure AD ve Office 365'te uyumluluk yapılandırmasını ve raporları okuyup yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi tüm özelliklerini yönetebilir |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi
Koşullu erişim özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/default/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/default/update | Azure Active Directory'de policies.conditionalAccess özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Azure Active Directory'de policies.conditionalAccess özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |

### <a name="crm-service-administrator"></a>CRM Hizmet Yöneticisi
Dynamics 365 ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="customer-lockbox-access-approver"></a>Müşteri Kasası Erişim Onaylayıcı
Müşterinin kuruluş verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 müşteri kasa tüm özelliklerini yönetebilir |

### <a name="device-administrators"></a>Cihaz Yöneticileri
Bu rolün üyeleri, Azure AD'ye katılmış cihazlarda yerel Yöneticiler grubuna eklenir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groupSettings/default/read | Azure Active Directory'de groupSettings temel özelliklerini okuyun. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Azure Active Directory'de groupSettingTemplates temel özelliklerini okuyun. |

### <a name="device-managers"></a>Cihaz Yöneticileri
Müşterinin kuruluş verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Devices/default/Read | Azure Active Directory'de cihazlarda temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Devices/default/Update | Azure Active Directory'de cihazlarda temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/devices/memberOf/read | Azure Active Directory'de devices.memberOf özelliği okuyun. |
| microsoft.aad.directory/devices/registeredOwners/read | Azure Active Directory'de devices.registeredOwners özelliği okuyun. |
| microsoft.aad.directory/devices/registeredOwners/update | Azure Active Directory'de devices.registeredOwners özelliğini güncelleştirin. |
| microsoft.aad.directory/devices/registeredUsers/read | Azure Active Directory'de devices.registeredUsers özelliği okuyun. |
| microsoft.aad.directory/devices/registeredUsers/update | Azure Active Directory'de devices.registeredUsers özelliğini güncelleştirin. |

### <a name="directory-readers"></a>Dizin Okuyucular
Temel dizin bilgileri okuyabilir. Uygulamalara erişim vermek için

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/default/read | Azure Active Directory'de administrativeUnits temel özelliklerini okuyun. |
| microsoft.aad.directory/administrativeUnits/members/read | Azure Active Directory'de administrativeUnits.members özelliği okuyun. |
Azure Active Directory. |
| Microsoft.aad.Directory/Applications/default/Read | Uygulamaları Azure Active Directory'de temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Applications/Owners/Read | Azure Active Directory'de Applications.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Contacts/default/Read | Azure Active Directory'de kişiler üzerinde temel özelliklerini okuyun. |
| microsoft.aad.directory/contacts/memberOf/read | Azure Active Directory'de contacts.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Contracts/default/Read | Azure Active Directory'de sözleşmelerinde temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Devices/default/Read | Azure Active Directory'de cihazlarda temel özelliklerini okuyun. |
| microsoft.aad.directory/devices/memberOf/read | Azure Active Directory'de devices.memberOf özelliği okuyun. |
| microsoft.aad.directory/devices/registeredOwners/read | Azure Active Directory'de devices.registeredOwners özelliği okuyun. |
| microsoft.aad.directory/devices/registeredUsers/read | Azure Active Directory'de devices.registeredUsers özelliği okuyun. |
| microsoft.aad.directory/directoryRoles/default/read | Azure Active Directory'de directoryRoles temel özelliklerini okuyun. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Azure Active Directory'de directoryRoles.eligibleMembers özelliği okuyun. |
| microsoft.aad.directory/directoryRoles/members/read | Azure Active Directory'de directoryRoles.members özelliği okuyun. |
| Microsoft.aad.Directory/Domains/default/Read | Azure Active Directory etki alanlarında temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory'de groups.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Groups/default/Read | Azure Active Directory'de grupları temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory'de groups.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Read | Azure Active Directory'de Groups.Members özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Owners/Read | Azure Active Directory'de Groups.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Settings/Read | Azure Active Directory'de Groups.Settings özelliği okuyun. |
| microsoft.aad.directory/groupSettings/default/read | Azure Active Directory'de groupSettings temel özelliklerini okuyun. |
| microsoft.aad.directory/groupSettingTemplates/default/read | Azure Active Directory'de groupSettingTemplates temel özelliklerini okuyun. |
| microsoft.aad.directory/oAuth2PermissionGrants/default/read | Azure Active Directory'de oAuth2PermissionGrants temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Organization/default/Read | Kuruluşunuz Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/default/read | Azure Active Directory'de servicePrincipals temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliği okuyun. |
| microsoft.aad.directory/subscribedSkus/default/read | Azure Active Directory'de subscribedSkus temel özelliklerini okuyun. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/default/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliği okuyun. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliği okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |

### <a name="directory-synchronization-accounts"></a>Dizin eşitlemesi hesapları
Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Azure Active Directory'de organization.dirSync özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeleri oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.aad.Directory/Policies/default/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Policies/default/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Owners/Read | Azure Active Directory'de policies.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Azure Active Directory'de policies.policiesAppliedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/default/read | Azure Active Directory'de servicePrincipals temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/default/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |

### <a name="directory-writers"></a>Dizin Yazıcılar
Okuma ve yazma temel dizin bilgileri kullanabilirsiniz. Uygulamalara erişim vermek için

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/default/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/groupSettings/default/update | Azure Active Directory'de groupSettings temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/groupSettings/create | Azure Active Directory'de groupSettings oluşturun. |
| microsoft.aad.directory/groupSettings/delete | Azure Active Directory'de groupSettings silin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/default/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |

### <a name="exchange-service-administrator"></a>Exchange Hizmeti Yöneticisi
Exchange ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="guest"></a>Konuk
Konuk kullanıcılar için varsayılan rol. Sınırlı sayıda dizin bilgileri okuyabilir.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/default/Read | Uygulamaları Azure Active Directory'de temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Applications/Owners/Read | Azure Active Directory'de Applications.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Domains/default/Read | Azure Active Directory etki alanlarında temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory'de groups.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Groups/default/Read | Azure Active Directory'de grupları temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory'de groups.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Read | Azure Active Directory'de Groups.Members özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Owners/Read | Azure Active Directory'de Groups.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Settings/Read | Azure Active Directory'de Groups.Settings özelliği okuyun. |
| microsoft.aad.directory/organization/basicProfile/read | Azure Active Directory'de temel kuruluş profili bilgilerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/default/read | Azure Active Directory'de servicePrincipals temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/members/read | Azure Active Directory'de servicePrincipals.members özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/default/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliği okuyun. |
| microsoft.aad.directory/users/basicProfile/read | Azure Active Directory'de users.basicProfile özelliği okuyun. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/default/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| microsoft.aad.directory/users/eligibleMemberOf/read | Azure Active Directory'de users.eligibleMemberOf özelliği okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliği okuyun. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliği okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.directory/users/pendingMemberOf/read | Azure Active Directory'de users.pendingMemberOf özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |
| microsoft.aad.directory/users/scopedAdministratorOf/read | Azure Active Directory'de users.scopedAdministratorOf özelliği okuyun. |

### <a name="guest-inviter"></a>Konuk Davet Eden
Üyeleri bağımsız Konuk kullanıcıları davet ayarı Konuk davet edebilirsiniz.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/default/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliği okuyun. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory'de konuk kullanıcıları davet edin. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliği okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/default/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |

### <a name="helpdesk-administrator"></a>Yardım Masası Yöneticisi
Yönetici olmayan kullanıcıların ve Yardım Masası Yöneticilerinin parolalarını sıfırlayabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="information-protection-administrator"></a>Bilgi Koruma Yöneticisi
Azure Information Protection ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="intune-service-administrator"></a>Intune Hizmet Yöneticisi
Intune ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| Microsoft.aad.Directory/Devices/default/Update | Azure Active Directory'de cihazlarda temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Devices/Create | Cihazları Azure Active Directory'de oluşturun. |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory cihazları silin. |
| microsoft.aad.directory/devices/registeredOwners/update | Azure Active Directory'de devices.registeredOwners özelliğini güncelleştirin. |
| microsoft.aad.directory/devices/registeredUsers/update | Azure Active Directory'de devices.registeredUsers özelliğini güncelleştirin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/default/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/default/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="license-administrator"></a>Lisans Yöneticisi
Kullanıcılar ve gruplar ürün lisanslarını yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| microsoft.aad.directory/users/usageLocation/update | Azure Active Directory'de users.usageLocation özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="lync-service-administrator"></a>Lync Hizmet Yöneticisi
Skype Kurumsal ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="message-center-reader"></a>İleti Merkezi Okuyucu
Yalnızca Office 365 İleti Merkezi'nde kuruluşuna yönelik iletileri ve güncelleştirmeleri okuyabilir. 

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessmessagecenter/allEntities/allTasks | Tüm kaynakları oluşturup silin ve İleti Merkezi'nde standart özellikleri okuyun ve güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |

### <a name="partner-tier1-support"></a>Partner Tier1 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/default/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="partner-tier2-support"></a>Partner Tier2 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/default/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| microsoft.aad.directory/domains/allTasks | Oluşturma ve etki alanlarını silmek ve okuma ve güncelleştirme, Azure Active Directory'de standart özellikleri. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Organization/default/Update | Kuruluşunuz Azure Active Directory'de temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/default/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="power-bi-service-administrator"></a>Power BI Hizmet Yöneticisi
Power BI ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="privileged-role-administrator"></a>Ayrıcalıklı Rol Yöneticisi
Azure AD'de rol atamalarını yönetebilir

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Azure Active Directory'de directoryRoles güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.aad.privilegedIdentityManagement standart özellikleri. |

### <a name="reports-reader"></a>Rapor Okuyucu
Oturum açma ve denetim raporlarını okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | Azure AD Raporlarını okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Güvenlik bilgilerini ve raporları okuyabilir

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/default/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeleri oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.identityProtection/allEntities/read | Tüm kaynakların microsoft.aad.identityProtection okuyun. |
| microsoft.aad.identityProtection/allEntities/update | Tüm kaynakların microsoft.aad.identityProtection güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| Microsoft.Office365.protectionCenter/allEntities/Update | Tüm kaynakların microsoft.office365.protectionCenter güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="security-reader"></a>Güvenlik Okuyucu
Azure AD ve Office 365'te güvenlik bilgilerini ve raporları okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | Tüm kaynakların microsoft.aad.identityProtection okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="service-support-administrator"></a>Hizmet Desteği Yöneticisi
Hizmet durumu bilgilerini okuyabilir ve destek biletlerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="sharepoint-service-administrator"></a>SharePoint Hizmet Yöneticisi
SharePoint hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="teams-communications-administrator"></a>Takımlar iletişimleri Yöneticisi
Arama ve Microsoft Teams hizmet içinde toplantıları özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="teams-communications-support-engineer"></a>Takımlar iletişimleri destek mühendisi
Gelişmiş araçlar kullanarak takımlar içinde iletişim sorunlarını giderebilirsiniz.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-communications-support-specialist"></a>Takımlar iletişimleri Destek Uzmanı
Temel araçları kullanarak takımlar içinde iletişim sorunlarını giderebilirsiniz.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-service-administrator"></a>Takımlar Hizmet Yöneticisi
Microsoft Teams hizmeti yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında addditonal izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliği okuyun. |
| Microsoft.aad.Directory/Policies/Basic/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="user-account-administrator"></a>Kullanıcı Hesabı Yöneticisi
Kullanıcıların ve grupların tüm özelliklerini yönetebilir

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments silin. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments güncelleştirin. |
| Microsoft.aad.Directory/Contacts/default/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/default/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/default/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/Create | Azure Active Directory'de kullanıcıları oluşturun. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure erişim hizmetinin tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="user"></a>Kullanıcı
Varsayılan rol üyesi kullanıcılar için. Tüm okuyabilir ve sınırlı sayıda dizin bilgileri yazın.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Azure Active Directory'de uygulamalar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/default/Read | Azure Active Directory'de grupları temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/oAuth2PermissionGrants/create | Azure Active Directory'de oAuth2PermissionGrants oluşturun. |
| microsoft.aad.directory/oAuth2PermissionGrants/delete | Azure Active Directory'de oAuth2PermissionGrants silin. |
| microsoft.aad.directory/oAuth2PermissionGrants/update | Azure Active Directory'de oAuth2PermissionGrants güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Azure Active Directory'de servicePrincipals oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/users/activateServicePlan | Azure Active Directory'de Activateserviceplan kullanıcılar. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory'de konuk kullanıcıları davet edin. |
| Microsoft.aad.Directory/Applications/default/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de Applications.Permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Restore | Azure Active Directory'de uygulamalar geri yükleyin. |
| Microsoft.aad.Directory/Devices/disable | Cihazları Azure Active Directory'de devre dışı bırakın. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/default/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| microsoft.aad.directory/groups/dynamicMembershipRule/update | Azure Active Directory'de groups.dynamicMembershipRule özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/default/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/default/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/users/changePassword | Azure Active Directory'de tüm kullanıcıların parolalarının değiştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/users/basicProfile/update | Azure Active Directory'de users.basicProfile özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Mobile/Update | Azure Active Directory'de Users.Mobile özelliğini güncelleştirin. |
| microsoft.aad.directory/users/searchableDeviceKey/update | Azure Active Directory'de users.searchableDeviceKey özelliğini güncelleştirin. |


## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliği Yöneticisi olarak kullanıcı atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
