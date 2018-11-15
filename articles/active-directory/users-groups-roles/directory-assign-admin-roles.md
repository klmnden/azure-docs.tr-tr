---
title: Azure Active Directory'de Yönetici rolü izinleri | Microsoft Docs
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
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 8c5da669d490bf295c4066854ac1173bcc79ad5e
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686135"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory'de Yönetici rolü izinleri

Azure Active Directory (Azure AD) kullanarak, farklı işlevler sunmak için ayrı yöneticileri atayabilirsiniz. Yöneticiler, ekleme veya kullanıcıları için değiştirme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi görevleri gerçekleştirmek için Azure AD Portalı'nda belirlenebilir.

Genel yönetici, tüm yönetim özelliklerine erişebilir. Varsayılan olarak, bir Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca genel Yöneticiler ve ayrıcalıklı rol yöneticileri, yönetici rollerini devredebilirsiniz.

## <a name="assign-or-remove-administrator-roles"></a>Atamayı veya kaldırmayı yönetici rolleri

Azure Active Directory'de bir kullanıcıya yönetici rollerini atama hakkında bilgi edinmek için [Azure Active Directory'de yönetici rolleri görüntüleyin ve Ata](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki Yönetici rollerini kullanılabilir:

* **[Uygulama Yöneticisi](#application-administrator)**: Bu roldeki kullanıcılar oluşturabilir ve kurumsal uygulamaları, uygulama kayıtları ve uygulama proxy'si ayarları tüm özelliklerini yönetebilir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Uygulama Yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Uygulama geliştiricisi](#application-developer)**: Bu roldeki kullanıcılar, uygulama kayıtları oluşturabilir, "Kullanıcılar uygulamaları kaydedebilir" ayarı Hayır olarak ayarlayın Bu rolü üyelerinin kendi adınıza onay de sağlar. zaman "Kullanıcı izni verebilir uygulamalara kendileri adına şirket verilerine erişme" ayarı Hayır olarak ayarlayın Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenir.

* **[Faturalama Yöneticisi](#billing-administrator)**: satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

* **[Bulut uygulaması Yöneticisi](#cloud-application-administrator)**: Bu roldeki kullanıcılar, uygulama proxy'si yönetme olanağı hariç uygulama yöneticisi rolü aynı izinlere sahip. Bu rol oluşturmak ve kurumsal uygulamaları ve uygulama kayıtlarını tüm yönlerini yönetmek için yeteneği verir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Yeni uygulama kaydı veya kurumsal uygulamalar oluştururken, bu rolün üyeleri, sahip olarak eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Bulut uygulaması yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Bulut cihaz Yöneticisi](#cloud-device-administrator)**: Bu roldeki kullanıcılar etkinleştirmek, devre dışı bırakabilir ve Azure AD'de cihazları silin ve Windows 10 BitLocker anahtarları Azure Portalı'nda (varsa) okuyun. Rol, cihazdaki diğer özelliklerini yönetmek için izinleri tanımaz.

* **[Uyumluluk Yöneticisi](#compliance-administrator)**: Bu role sahip kullanıcılar, Office 365 güvenlik ve Uyumluluk Merkezi ve Exchange yönetici merkezini de yönetim izinlerine sahip. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **[Koşullu Erişim Yöneticisi](#conditional-access-administrator)**: Bu role sahip olan kullanıcılar Azure Active Directory koşullu erişim ayarlarını yönetme olanağı vardır.
  > [!NOTE]
  > Azure'da Exchange ActiveSync koşullu erişim ilkesi dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **[Cihaz yöneticileri](#device-administrators)**: Bu rol ataması yalnızca ek yerel yönetici olarak kullanılabilir [cihaz ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Bu role sahip kullanıcılar, Azure Active Directory'ye katılan tüm Windows 10 cihazları üzerinde yerel makine yöneticisi olur. Azure Active Directory'de cihaz nesnelerini yönetme olanağına sahip değildir. 

* **[Dizin okuyucular](#directory-readers)**: Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dizin eşitlemesi hesapları](#directory-synchronization-accounts)**: kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil hedeflenen veya herhangi bir kullanım için desteklenir.

* **[Dizin yazıcıları](#directory-writers)**: Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dynamics 365 Yönetici / CRM yönetici](#dynamics-365-administrator)**: Bu role sahip kullanıcılar, Microsoft Dynamics 365 hizmet mevcut olduğunda Online içinde genel izinlere olmanın yanı sıra destek biletlerini yönetebilir ve hizmet izlemek için sahip Sistem durumu. Daha fazla bilgiye [kiracınızı yönetmek için Hizmet Yöneticisi rolü kullanmak](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Dynamics 365 Hizmet Yöneticisi" tanımlanır. Azure portalında "Dynamics 365 Yönetici" olduğu.

* **[Exchange Yöneticisi](#exchange-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. tüm Office 365 grupları oluşturma ve yönetme olanağı, yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Exchange Hizmet Yöneticisi" olarak tanımlanır. Azure portalında "Exchange Yönetici" olduğu.

* **[Genel yönetici / şirket Yöneticisi](#company-administrator)**: Bu role sahip kullanıcılar, Exchange Online gibi Azure Active Directory kimlikleri kullanmak hizmetlerinin yanı sıra Azure Active Directory, tüm yönetim özelliklerine erişim sahibi SharePoint Online ve Skype Kurumsal çevrimiçi. Azure Active Directory kiracısı için kaydolan kişi genel yönetici olur. Yalnızca genel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel Yöneticiler, herhangi bir kullanıcı ve diğer tüm yöneticilerin parolasını sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" tanımlanır. "Genel yönetici" olarak [Azure portalında](https://portal.azure.com).
  >
  >

* **[Konuk davet eden](#guest-inviter)**: Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetlerini yönetebilir, **üyelerini davet edebildiğiniz** kullanıcı ayarı Hayır olarak ayarlayın B2B işbirliği hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Tüm diğer izinleri içermez.

* **[Bilgi Koruma Yöneticisi](#information-protection-administrator)**: Bu role sahip kullanıcılar Azure Information Protection hizmetinde tüm izinlere sahiptir. Bu rol, Azure Information Protection ilkesi için etiketleri yapılandırma, koruma şablonlarını yönetme ve koruma etkinleştirme sağlar. Bu rol, kimlik koruma Merkezi, Privileged Identity Management, Office 365 hizmet durumunu izleme, veya Office 365 güvenlik ve uyumluluk Merkezi'nde herhangi bir izni tanımaz.

* **[Intune yönetici](#intune-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Intune Online içinde genel izinlere sahiptir. Ayrıca, bu rol, ilke ilişkilendirmek yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme olanağı içerir. Daha fazla bilgiye [Intune rol tabanlı yönetim denetimi (RBAC)](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Intune Hizmet Yöneticisi" olarak tanımlanır. Azure portalında "Intune Yönetici" olduğu.

* **[Lisans Yöneticisi](#license-administrator)**: Bu roldeki kullanıcılar eklemek, kaldırmak ve güncelleştirme lisans atamalarında kullanıcıları, grupları (Grup tabanlı lisanslama kullanarak) ve kullanıcılar üzerindeki kullanım konumu yönetmek. Rol, satın alma veya Aboneliklerini yönetmek, oluşturma veya grupları yönetme veya oluşturma veya ötesinde kullanım konumu yönetme olanağı tanımaz.

* **[İleti Merkezi okuyucu](#message-center-reader)**: Bu roldeki kullanıcılar, bildirimler ve danışmanlık sistem güncelleştirmeleri izleyebilirsiniz [Office 365 ileti Merkezi](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) kuruluşlarında Intune, Exchange gibi yapılandırılmış hizmetleri ve Microsoft Teams. İleti Merkezi okuyucular Haftalık e-posta özetler gönderilerin, güncelleştirmeleri almak ve Office 365 ileti merkezi gönderileri paylaşabilirsiniz. Azure AD'de bu role atanan kullanıcılar yalnızca salt okunur kullanıcılar ve gruplar gibi Azure AD Hizmetleri erişebilir. 

* **[İş ortağı Tier1 desteği](#partner-tier1-support)**: kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[İş ortağı Tier2 desteği](#partner-tier2-support)**: kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[Parola Yöneticisi / Yardım Masası Yöneticisi](#helpdesk-administrator)**: Bu role sahip olan kullanıcılar parolaları değiştirme, yenileme belirteçleri geçersiz kılmak, hizmet isteklerini yönetebilir ve hizmet durumunu izleyin. Bir yenileme belirteci geçersiz kılmalarını, kullanıcı yeniden oturum açmak için zorlar. Yardım Masası yöneticileri, parolaları sıfırlama ve yenileme belirteçleri olmayanların olan diğer kullanıcıların veya yalnızca aşağıdaki rollerinin üyeleri geçersiz kıl:
  * Dizin Okuyucular
  * Konuk Davet Eden
  * Yardım Masası Yöneticisi
  * İleti Merkezi Okuyucu
  * Rapor Okuyucu
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve Yardım Masası yöneticileri izni başka bir yerde değil. Yardım Masası Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Yardım Masası Yöneticisi" tanımlanır. "Parola Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).
  >
  
* **[Power BI Yöneticisi](#power-bi-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Power BI içinde genel izinlere yanı sıra destek biletlerini yönetebilir ve hizmet durumu izleme olanağı vardır. Daha fazla bilgiye [Power BI yönetici rolünü anlama](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Power BI Hizmet Yöneticisi olarak" olarak tanımlanır. Azure portalında "Power BI Yönetici" olduğu.

* **[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator)**: Bu role sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rol, Privileged Identity Management'ın tüm yönlerini yönetilmesine izin verir.

  <b>Önemli</b>: Bu rolü genel Yönetici rolüne dahil olmak üzere tüm Azure AD rollerine üyelik yönetme olanağı sağlar. Bu rol, oluşturma veya kullanıcıları güncelleştirerek gibi Azure AD'de herhangi bir ayrıcalıklı yeteneklerini içermez. Ancak, bu role atanan kullanıcılar kendileri veya diğerleri ek ayrıcalık ek rolleri atayarak verebilirsiniz.

* **[Rapor okuyucu](#reports-reader)**: Bu role sahip kullanıcılar, kullanım verileri ve raporları Panoda, Office 365 Yönetim merkezine ve benimseme bağlam paketini Power BI'da raporu görüntüleyebilirsiniz. Ayrıca, rol oturum açma için erişim sağlayan raporları ve etkinlik Azure AD'de ve Microsoft Graph tarafından döndürülen veri raporlama API'si. Rapor okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar, ayarları veya Exchange gibi ürüne özel yönetim merkezlerine erişimi yapılandırmak için bir yönetici izinleri yok. 

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

* **[SharePoint Yöneticisi](#sharepoint-administrator)**: Bu role sahip olan kullanıcılar hizmet oluşturmak ve tüm Office 365 grupları yönetme, destek biletlerini yönetme olanağı yanı sıra mevcut olduğunda Microsoft SharePoint Online içinde genel izinlere sahiptir ve Hizmet durumunu izleyebilir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "SharePoint Hizmet Yöneticisi olarak." tanımlanır Azure portalında "SharePoint Yönetici" olduğu.

* **[İşletme için Skype / Lync Yöneticisi](#skype-for-business-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Skype Kurumsal, içinde genel izinlere sahip, hem de Azure Active Directory'de Skype özgü kullanıcı özniteliklerini yönetin. Ayrıca, bu rol, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir ve takımlar ve Skype kurumsal iş Yönetim Merkezi erişim olanağı verir. Hesap, takımlar için lisanslanmalıdır veya ekipler PowerShell cmdlet'leri çalıştırılamaz. Daha fazla bilgi [hakkında Skype kurumsal iş yöneticisi rolüne](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve lisans bilgileri takımlar [iş ve Microsoft Teams eklenti lisansı için Skype](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. "Skype Kurumsal Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).

* **[İletişim yönetici takımlar](#teams-communications-administrator)**: Bu roldeki kullanıcılar, ilgili ses & telefon için Microsoft Teams iş yükü özelliklerini yönetebilir. Bu telefon numarası ataması, ses ve toplantı ilkeleri ve çağrı analizi araç takımı tam erişim için yönetim araçlarını içerir.

* **[Takımlar iletişimleri destek mühendisi](#teams-communications-support-engineer)**: Bu roldeki kullanıcılar Microsoft Teams ve Skype Kurumsal sorun giderme araçları kullanarak kullanıcı iş çağrısı içinde Microsoft Teams & Skype iletişim sorun giderme İş Yönetim Merkezi. Bu roldeki kullanıcılar, ilgili tüm katılımcılar için tam çağrı kayıt bilgileri görüntüleyebilirsiniz.

* **[Takımlar iletişimleri destek uzmanı](#teams-communications-support-specialist)**: Bu roldeki kullanıcılar Microsoft Teams ve Skype Kurumsal sorun giderme araçları kullanarak kullanıcı iş çağrısı içinde Microsoft Teams & Skype iletişim sorun giderme İş Yönetim Merkezi. Bu roldeki kullanıcılar yalnızca bunlar Aranan kullanıcıyı çağrıda kullanıcı ayrıntıları da görüntüleyebilirsiniz.

* **[Takımlar, yönetici](#teams-administrator)**: Bu roldeki kullanıcılar, iş Yönetim Merkezi ve ilgili PowerShell modülleri için Microsoft Teams iş yükü Microsoft Teams ve Skype üzerinden tüm özelliklerini yönetebilir. Bu, diğer alanları arasında telefon, Mesajlaşma, toplantılar ve takımlar için ilgili tüm Yönetim Araçlar içerir. Bu rol, ayrıca oluşturun ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı verir.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "takımlar Hizmet Yöneticisi" olarak tanımlanır. Azure portalında "Takımlar Yönetici" olduğu.

* **[Kullanıcı hesabı yöneticisi](#user-account-administrator)**: Bu role sahip kullanıcılar kullanıcıları oluşturun ve bazı kısıtlamalar (aşağıya bakın) sahip kullanıcılar tüm özelliklerini yönetebilir. Ayrıca, bu role sahip kullanıcılar oluşturun ve tüm gruplarını yönetin. Bu rolü, oluşturma ve kullanıcı görünümleri yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı da içerir.

  | | |
  | --- | --- |
  |Genel izinler|<p>Kullanıcılar ve gruplar oluşturma</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Office destek biletlerini yönetebilir|
  |<p>Tüm yöneticilerin tüm kullanıcılar dahil</p>|<p>Lisanslarını yönetme</p><p>Kullanıcı asıl adı dışındaki tüm kullanıcı özelliklerini yönetme</p>
  |Yönetici olmayan ya da aşağıdakilerden birini sınırlı yönetici rolleri yalnızca kullanıcılar üzerinde:<ul><li>Dizin Okuyucular<li>Konuk Davet Eden<li>Yardım Masası Yöneticisi<li>İleti Merkezi Okuyucu<li>Rapor Okuyucu<li>Kullanıcı Hesabı Yöneticisi|<p>Silme ve geri yükleme</p><p>Devre dışı bırakma ve etkinleştirme</p><p>Geçersiz belirteç yenileme</p><p>Kullanıcı asıl adı dahil olmak üzere tüm kullanıcı özelliklerini yönetme</p><p>Parola sıfırlama</p><p>Güncelleştirme (FIDO) cihaz anahtarları</p>
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolalarını değiştirebilir. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve kullanıcı yöneticileri izni başka bir yerde değil. Kullanıcı Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

Aşağıdaki tablolarda her rol için belirtilen Azure Active Directory'de özel izinler açıklanmaktadır. Bazı roller, Azure Active Directory dışında Microsoft hizmetlerindeki ek izinlere sahip olabilir.

### <a name="application-administrator"></a>Uygulama Yöneticisi
Tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de applications.audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de applications.authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Basic/Update | Azure Active Directory'de uygulamalar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Applications/Create | Azure Active Directory'de uygulamalar oluşturun. |
| Microsoft.aad.Directory/Applications/credentials/Update | Azure Active Directory'de applications.credentials özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de applications.owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de applications.permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de applications.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/read | Azure Active Directory'de appRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments özelliğini silin. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals özelliğini silin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="application-developer"></a>Uygulama Geliştirici
Uygulama kayıtları 'kullanıcılar uygulamaları kaydedebilir' bağımsız oluşturabilirsiniz ayarı.

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
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Organization/Basic/Update | Azure Active Directory'de kuruluş üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="desktop-analytics-administrator"></a>Masaüstü Analytics Yöneticisi
Erişebilir ve Masaüstü Yönetimi Araçları ve Hizmetleri Intune dahil olmak üzere yönetebilirsiniz.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.desktopAnalytics/allEntities/allTasks | Masaüstü Analytics tüm özelliklerini yönetebilir. |
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
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de applications.audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de applications.authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Basic/Update | Azure Active Directory'de uygulamalar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Applications/Create | Azure Active Directory'de uygulamalar oluşturun. |
| Microsoft.aad.Directory/Applications/credentials/Update | Azure Active Directory'de applications.credentials özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de applications.owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de applications.permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de applications.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments özelliğini silin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals özelliğini silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
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
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory'de cihazları silin. |
| Microsoft.aad.Directory/Devices/disable | Azure Active Directory'de cihazları devre dışı bırakın. |
| Microsoft.aad.Directory/Devices/Enable | Cihazların Azure Active Directory'de sağlayın. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="company-administrator"></a>Şirket Yöneticisi
Azure AD'nin ve Azure AD kimliklerini kullanan Microsoft hizmetlerinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | administrativeUnits oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/applications/allProperties/allTasks | Uygulamalar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | appRoleAssignments oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Kişileri oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Kişiler oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/devices/allProperties/allTasks | Cihazlar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | directoryRoles oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | directoryRoleTemplates oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/domains/allProperties/allTasks | Etki alanları oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/groups/allProperties/allTasks | Gruplar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | groupSettings oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | groupSettingTemplates oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | loginTenantBranding oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | oAuth2PermissionGrants oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/organization/allProperties/allTasks | Kuruluş oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/policies/allProperties/allTasks | İlkeler oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | roleAssignments oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | roleDefinitions oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | scopedRoleMemberships oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/serviceAction/activateService | Azure Active Directory'de Activateservice hizmet eylemini gerçekleştirebilir |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Azure Active Directory'de Disabledirectoryfeature hizmet eylemini gerçekleştirebilir |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Azure Active Directory'de Enabledirectoryfeature hizmet eylemini gerçekleştirebilir |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Azure Active Directory'de Getavailableextentionproperties hizmet eylemini gerçekleştirebilir |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | servicePrincipals oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | subscribedSkus oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/users/allProperties/allTasks | Kullanıcılar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |
| microsoft.aad.identityProtection/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.aad.identityProtection üzerindeki standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm özelliklerini yönetin. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisinin tüm özelliklerini yönetin |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 Müşteri Kasasının tüm özelliklerini yönetin. |
| Microsoft.Office365.messageCenter/Messages/Read | Microsoft.office365.messageCenter iletilerini okuyun. |
| Microsoft.Office365.messageCenter/securityMessages/Read | SecurityMessages microsoft.office365.messageCenter içinde okuyun. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.protectionCenter/allEntities/allTasks | Office 365 Koruma Merkezi'nin tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm özelliklerini yönetin. |

### <a name="compliance-administrator"></a>Uyumluluk Yöneticisi
Azure AD ve Office 365'te uyumluluk yapılandırmasını ve raporları okuyup yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisinin tüm özelliklerini yönetin |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi
Koşullu erişim özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Azure Active Directory'de policies.conditionalAccess ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Azure Active Directory'de policies.conditionalAccess ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |

### <a name="crm-service-administrator"></a>CRM Hizmet Yöneticisi
Dynamics 365 ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
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
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 Müşteri Kasasının tüm özelliklerini yönetin. |

### <a name="device-administrators"></a>Cihaz Yöneticileri
Bu rolün üyeleri, Azure AD'ye katılmış cihazlarda yerel Yöneticiler grubuna eklenir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory'de groupSettings üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory'de groupSettingTemplates üzerindeki temel özellikleri okuyun. |

### <a name="directory-readers"></a>Dizin Okuyucular
Temel dizin bilgileri okuyabilir. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Azure Active Directory'de administrativeUnits üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/administrativeUnits/members/read | Azure Active Directory'de administrativeUnits.members özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Audience/Read | Azure Active Directory'de applications.audience özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Authentication/Read | Azure Active Directory'de applications.authentication özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Basic/Read | Azure Active Directory'de uygulamalar üzerindeki temel özellikleri okuyun. |
| Microsoft.aad.Directory/Applications/credentials/Read | Azure Active Directory'de applications.credentials özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Owners/Read | Azure Active Directory'de applications.owners özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Permissions/Read | Azure Active Directory'de applications.permissions özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Policies/Read | Azure Active Directory'de applications.policies özelliğini okuyun. |
| Microsoft.aad.Directory/Contacts/Basic/Read | Azure Active Directory'de kişiler üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/contacts/memberOf/read | Azure Active Directory'de contacts.memberOf özelliğini okuyun. |
| Microsoft.aad.Directory/Contracts/Basic/Read | Azure Active Directory'de kişiler üzerindeki temel özellikleri okuyun. |
| Microsoft.aad.Directory/Devices/Basic/Read | Azure Active Directory'de cihazlar üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/devices/memberOf/read | Azure Active Directory'de devices.memberOf özelliğini okuyun. |
| microsoft.aad.directory/devices/registeredOwners/read | Azure Active Directory'de devices.registeredOwners özelliğini okuyun. |
| microsoft.aad.directory/devices/registeredUsers/read | Azure Active Directory'de devices.registeredUsers özelliğini okuyun. |
| microsoft.aad.directory/directoryRoles/basic/read | Azure Active Directory'de directoryRoles üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Azure Active Directory'de directoryRoles.eligibleMembers özelliğini okuyun. |
| microsoft.aad.directory/directoryRoles/members/read | Azure Active Directory'de directoryRoles.members özelliğini okuyun. |
| Microsoft.aad.Directory/Domains/Basic/Read | Azure Active Directory'de etki alanları üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory'de groups.appRoleAssignments özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Basic/Read | Azure Active Directory'de gruplar üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory'de groups.memberOf özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Members/Read | Azure Active Directory'de groups.members özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Owners/Read | Azure Active Directory'de groups.owners özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Settings/Read | Azure Active Directory'de groups.owners özelliğini okuyun. |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory'de groupSettings üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory'de groupSettingTemplates üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Azure Active Directory'de oAuth2PermissionGrants üzerindeki temel özellikleri okuyun. |
| Microsoft.aad.Directory/Organization/Basic/Read | Azure Active Directory'de kuruluş üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/roleAssignments/basic/read | Azure Active Directory'de rol temel özelliklerini okuyun. |
| microsoft.aad.directory/roleDefinitions/basic/read | Azure Active Directory'de roleDefinitions temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliğini okuyun. |
| microsoft.aad.directory/subscribedSkus/basic/read | Azure Active Directory'de subscribedSkus üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| Microsoft.aad.Directory/Users/Basic/Read | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliğini okuyun. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliğini okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliğini okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

### <a name="directory-synchronization-accounts"></a>Dizin eşitlemesi hesapları
Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Azure Active Directory'de organization.dirSync özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeler oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory'de ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Basic/Read | Azure Active Directory'de ilkeler üzerindeki temel özellikleri okuyun. |
| Microsoft.aad.Directory/Policies/Basic/Update | Azure Active Directory'de ilkeler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Policies/Owners/Read | Azure Active Directory'de policies.owners özelliğini okuyun. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Azure Active Directory'de policies.policiesAppliedTo özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |

### <a name="directory-writers"></a>Dizin Yazıcılar
Okuma ve yazma temel dizin bilgileri kullanabilirsiniz. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de gruplar oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de gruplar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de groups.members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/groupSettings/basic/update | Azure Active Directory'de groupSettings üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/groupSettings/create | Azure Active Directory'de groupSettings oluşturun. |
| microsoft.aad.directory/groupSettings/delete | Azure Active Directory'de groupSettings özelliğini silin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |

### <a name="exchange-service-administrator"></a>Exchange Hizmeti Yöneticisi
Exchange ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="guest-inviter"></a>Konuk Davet Eden
'Üyeler konuk davet edebilir' ayarından bağımsız olarak konuk kullanıcı davet edebilir.

  > [!NOTE]
  > Bu rol, rol ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| Microsoft.aad.Directory/Users/Basic/Read | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.aad.directory/users/invitedBy/read | Azure Active Directory'de users.invitedBy özelliğini okuyun. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory'de konuk kullanıcıları davet edin. |
| microsoft.aad.directory/users/invitedUsers/read | Azure Active Directory'de users.invitedUsers özelliğini okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliğini okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

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
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="information-protection-administrator"></a>Bilgi Koruma Yöneticisi
Azure Information Protection ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="intune-service-administrator"></a>Intune Hizmet Yöneticisi
Intune ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişileri oluşturun. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişileri silin. |
| Microsoft.aad.Directory/Devices/Basic/Update | Azure Active Directory'de cihazlar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Devices/Create | Azure Active Directory'de cihazlar oluşturun. |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory'de cihazları silin. |
| microsoft.aad.directory/devices/registeredOwners/update | Azure Active Directory'de devices.registeredOwners özelliğini güncelleştirin. |
| microsoft.aad.directory/devices/registeredUsers/update | Azure Active Directory'de devices.registeredUsers özelliğini güncelleştirin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de gruplar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de gruplar oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory'de grupları silin. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de groups.members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de grupları geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
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
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/users/usageLocation/update | Azure Active Directory'de users.usageLocation özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="lync-service-administrator"></a>Lync Hizmet Yöneticisi
Skype Kurumsal ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="message-center-reader"></a>İleti Merkezi Okuyucu
Yalnızca Office 365 İleti Merkezi'nde kuruluşuna yönelik iletileri ve güncelleştirmeleri okuyabilir. 

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| Microsoft.Office365.messageCenter/Messages/Read | Microsoft.office365.messageCenter iletilerini okuyun. |

### <a name="partner-tier1-support"></a>Partner Tier1 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişileri oluşturun. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişileri silin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de gruplar oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de groups.members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="partner-tier2-support"></a>Partner Tier2 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişileri oluşturun. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişileri silin. |
| microsoft.aad.directory/domains/allTasks | Etki alanları oluşturup silin ve Azure Active Directory'de standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de gruplar oluşturun. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory'de grupları silin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de groups.members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de grupları geri yükleyin. |
| Microsoft.aad.Directory/Organization/Basic/Update | Azure Active Directory'de kuruluş üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="power-bi-service-administrator"></a>Power BI Hizmet Yöneticisi
Power BI ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="privileged-role-administrator"></a>Ayrıcalıklı Rol Yöneticisi
Azure AD'de rol atamalarını ve ayrıcalıklı Kimlik Yönetimi'nin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Azure Active Directory'de directoryRoles özelliğini güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.aad.privilegedIdentityManagement üzerindeki standart özellikleri okuyun ve güncelleştirin. |

### <a name="reports-reader"></a>Rapor Okuyucu
Oturum açma ve denetim raporlarını okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Güvenlik bilgilerini ve raporları okuma ve Azure AD'de yapılandırmasını yönetmek ve Office 365.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de applications.policies özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Basic/Update | Azure Active Directory'de ilkeler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeler oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory'de ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection içindeki tüm kaynakları okuyun. |
| microsoft.aad.identityProtection/allEntities/update | microsoft.aad.identityProtection içindeki tüm kaynakları güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| Microsoft.Office365.protectionCenter/allEntities/Update | microsoft.office365.protectionCenter içindeki tüm kaynakları güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="security-reader"></a>Güvenlik Okuyucu
Azure AD ve Office 365'te güvenlik bilgilerini ve raporları okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection içindeki tüm kaynakları okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.aad.reports/applicationAuditLogs/read | Azure AD raporlarında applicationAuditLogs okuyun. |
| microsoft.aad.reports/applicationSignInReports/read | Azure AD raporlarında applicationSignInReports okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.protectionCenter/allEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="service-support-administrator"></a>Hizmet Desteği Yöneticisi
Hizmet durumu bilgilerini okuyabilir ve destek biletlerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="sharepoint-service-administrator"></a>SharePoint Hizmet Yöneticisi
SharePoint hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="teams-communications-administrator"></a>Teams İletişim Yöneticisi
Microsoft Teams hizmeti içinde arama ve toplantı özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Azure Active Directory'de ilkeler üzerindeki temel özellikleri okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="teams-communications-support-engineer"></a>Teams İletişim Destek Mühendisi
Teams içinde gelişmiş araçları kullanarak iletişim sorunlarını giderebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Azure Active Directory'de ilkeler üzerindeki temel özellikleri okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-communications-support-specialist"></a>Teams İletişim Destek Uzmanı
Teams içinde temel araçları kullanarak iletişim sorunlarını giderebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Policies/Basic/Read | Azure Active Directory'de ilkeler üzerindeki temel özellikleri okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-service-administrator"></a>Teams Hizmet Yöneticisi
Microsoft Teams hizmetini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliğini okuyun. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Basic/Read | Azure Active Directory'de ilkeler üzerindeki temel özellikleri okuyun. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="user-account-administrator"></a>Kullanıcı Hesabı Yöneticisi
Sınırlı yöneticilerin parolalarını sıfırlama dahil olmak üzere kullanıcılar ve grupların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments özelliğini silin. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişileri oluşturun. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişileri silin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de gruplar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de gruplar oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory'de grupları silin. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliğini okuyun. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de groups.members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de grupları geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de groups.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Users/Create | Azure Active Directory'de kullanıcılar oluşturun. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.accessService/allEntities/allTasks | Azure Access hizmetinin tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar, kullanım dışı bırakıldı ve gelecekte Azure AD'deki kaldırıldı.

* AdHoc Lisans Yöneticisi
* Cihaz birleştirme
* Cihaz Yöneticileri
* Aygıt kullanıcıları
* E-posta Adresi Doğrulanan Kullanıcı Oluşturucu
* Posta Kutusu Yöneticisi
* Cihazla Çalışma Alanına Katılma

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliği Yöneticisi olarak kullanıcı atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
