---
title: Azure Active Directory'de yönetici rolleri atama | Microsoft Docs
description: Yönetici rolü kullanıcı ekleme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme. Yönetici rolü atanan bir kullanıcı, kuruluşunuzun abone olduğu tüm bulut hizmetlerinde aynı izinlere sahiptir.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/27/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 9b56f540af2b8d35258a4db79502c9edf83cdb45
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128475"
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

* **[Parola Yöneticisi / Yardım Masası Yöneticisi](#helpdesk-administrator)**: Bu role sahip olan kullanıcılar parolaları değiştirme, hizmet isteklerini yönetebilir ve hizmet durumunu izleyin. Yardım Masası yöneticileri yalnızca kullanıcıların ve diğer Yardım Masası yöneticileri için parolaları değiştirebilirsiniz. 

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Yardım Masası Yöneticisi" tanımlanır. "Parola Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).
  >
  >
  
* **[Power BI Hizmet Yöneticisi](#power-bi-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Power BI içinde genel izinlere yanı sıra destek biletlerini yönetebilir ve hizmet durumu izleme olanağı vardır. Daha fazla bilgiye [Power BI yönetici rolünü anlama](https://docs.microsoft.com/power-bi/service-admin-role).

* **[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator)**: Bu role sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rol, Privileged Identity Management'ın tüm yönlerini yönetilmesine izin verir.

* **[Rapor okuyucu](#reports-reader)**: Bu role sahip kullanıcılar, kullanım verileri ve raporları Panoda, Office 365 Yönetim merkezine ve benimseme bağlam paketini Power bı'da raporu görüntüleyebilirsiniz. Ayrıca, rol oturum açma için erişim sağlayan raporları ve etkinlik Azure AD'de ve Microsoft Graph tarafından döndürülen veri raporlama API'si. Rapor okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar, ayarları veya Exchange gibi ürün belirli Yönetim Merkezleri erişimi yapılandırmak için bir yönetici izinlere sahip değilsiniz. 

* **[Güvenlik Yöneticisi](#security-administrator)**: Bu role sahip kullanıcılar tüm güvenlik okuyucu rolünün yanı sıra, güvenlikle ilgili şu hizmetler için yapılandırma yönetme olanağı, salt okunur izinleri vardır: Azure Active Directory kimlik koruması, Azure Information Protection, Privileged Identity Management ve Office 365 güvenlik ve Uyumluluk Merkezi. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
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

* **[İşletme için Skype / Lync Hizmet Yöneticisi](#lync-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Skype Kurumsal, içinde genel izinlere sahip, hem de Skype özgü kullanıcı öznitelikleri, Azure Active Yönet Dizin. Ayrıca, bu rol, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı verir. Daha fazla bilgiye [hakkında Skype kurumsal iş yöneticisi rolüne](https://support.office.com/en-us/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5).

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. "Skype Kurumsal Hizmet Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).
  >
  >

* **[Kullanıcı hesabı yöneticisi](#user-account-administrator)**: Bu role sahip kullanıcılar oluşturabilir ve kullanıcıların ve grupların tüm özelliklerini yönetebilir. Ayrıca bu rol, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı içerir. Bazı kısıtlamalar geçerlidir. Örneğin, bu rolü genel yönetici silinmesine izin vermiyor değil. Kullanıcı hesabı yöneticileri, kullanıcılar, Yardım Masası yöneticileri ve diğer kullanıcı hesabı yöneticileri yalnızca değiştirebilirsiniz.

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


### <a name="to-add-a-colleague-as-a-global-administrator"></a>Bir iş arkadaşınız genel yönetici olarak eklemek için

1. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) bir genel yönetici veya Kiracı dizinini için ayrıcalıklı rol yöneticisi olan bir hesapla.

   ![Azure AD yönetim merkezini açma](./media/directory-assign-admin-roles/active-directory-admin-center.png)

2. Seçin **kullanıcılar**.

3. Kullanıcı dikey penceresini açın ve genel yönetici olarak belirlemek istediğiniz kullanıcıyı bulun.

4. Kullanıcı dikey penceresinde seçin **dizin rolü**.
 
5. Dizin rolü dikey penceresinde seçin **genel yönetici** rolü ve kaydedin.

## <a name="detailed-azure-active-directory-permissions"></a>Azure Active Directory izinlerini ayrıntılı
Aşağıdaki tablolarda her rol için belirtilen Azure Active Directory'de özel izinler açıklanmaktadır. Genel yönetici gibi bazı roller, Azure Active Directory Microsoft Hizmetleri outide ek izinlere sahip olabilir.


### <a name="application-administrator"></a>Uygulama Yöneticisi
Tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Azure Active Directory'de Uygulamalar oluşturun. |
| microsoft.aad.directory/Application/Delete | Azure Active Directory'de Uygulamaları silin. |
| microsoft.aad.directory/Application/Update | Azure Active Directory'de Uygulamalar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Azure Active Directory'de Applications.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/Application/Update/Owners | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/AppRoleAssignment/Create | Azure Active Directory'de AppRoleAssignments oluşturun. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Azure Active Directory'de AppRoleAssignments'ı silin. |
| microsoft.aad.directory/AppRoleAssignment/Update | Azure Active Directory'de AppRoleAssignments üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Create | Azure Active Directory'de İlkeler oluşturun. |
| microsoft.aad.directory/Policy/Delete | Azure Active Directory'de İlkeleri silin. |
| microsoft.aad.directory/Policy/Update | Azure Active Directory'de İlkeler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Update/Owners | Azure Active Directory'de Policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Azure Active Directory (Azure AD Graph ve Microsoft Graph) dışında tüm kaynaklara tüm kullanıcılar adına izin verebilir. |
| microsoft.aad.directory/ServicePrincipal/Create | Azure Active Directory'de ServicePrincipals oluşturun. |
| microsoft.aad.directory/ServicePrincipal/Delete | Azure Active Directory'de ServicePrincipals'ı silin. |
| microsoft.aad.directory/ServicePrincipal/Update | Azure Active Directory'de ServicePrincipals üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Azure Active Directory'de ServicePrincipals.AppRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Azure Active Directory'de ServicePrincipals.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Azure Active Directory'de ServicePrincipals.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.reports/AllEntities/Read | Azure AD Raporlarını okuyun. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="application-developer"></a>Uygulama Geliştirici
Uygulama kayıtları bağımsız olarak oluşturabilirsiniz **kullanıcılar uygulamaları kaydedebilir** ayarı.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Application/CreateAsOwner | Azure Active Directory'de Uygulamalar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/AppRoleAssignment/CreateAsOwner | Azure Active Directory'de AppRoleAssignments oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/OAuth2PermissionGrant/CreateAsOwner | Azure Active Directory'de OAuth2PermissionGrants oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/ServicePrincipal/CreateAsOwner | Azure Active Directory'de ServicePrincipals oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |

### <a name="billing-administrator"></a>Faturalama Yöneticisi
Ödeme bilgilerini güncelleştirme gibi sık kullanılan faturalandırma görevlerini gerçekleştirebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıda rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/Organization/Update | Azure Active Directory'de Kuruluşlar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.billing/AllEntities/AllActions | Office 365 faturalamasının tüm özelliklerini yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-application-administrator"></a>Bulut Uygulaması Yöneticisi
Uygulama Ara Sunucusu hariç tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Application/Create | Azure Active Directory'de Uygulamalar oluşturun. |
| microsoft.aad.directory/Application/Delete | Azure Active Directory'de Uygulamaları silin. |
| microsoft.aad.directory/Application/Update | Azure Active Directory'de Uygulamalar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Azure Active Directory'de Applications.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/Application/Update/Owners | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/AppRoleAssignment/Create | Azure Active Directory'de AppRoleAssignments oluşturun. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Azure Active Directory'de AppRoleAssignments'ı silin. |
| microsoft.aad.directory/AppRoleAssignment/Update | Azure Active Directory'de AppRoleAssignments üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Create | Azure Active Directory'de İlkeler oluşturun. |
| microsoft.aad.directory/Policy/Delete | Azure Active Directory'de İlkeleri silin. |
| microsoft.aad.directory/Policy/Update | Azure Active Directory'de İlkeler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Update/Owners | Azure Active Directory'de Policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllNoDirectory | Azure Active Directory (Azure AD Graph ve Microsoft Graph) dışında tüm kaynaklara tüm kullanıcılar adına izin verebilir. |
| microsoft.aad.directory/ServicePrincipal/Create | Azure Active Directory'de ServicePrincipals oluşturun. |
| microsoft.aad.directory/ServicePrincipal/Delete | Azure Active Directory'de ServicePrincipals'ı silin. |
| microsoft.aad.directory/ServicePrincipal/Update | Azure Active Directory'de ServicePrincipals üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Azure Active Directory'de ServicePrincipals.AppRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Azure Active Directory'de ServicePrincipals.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Azure Active Directory'de ServicePrincipals.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.reports/AllEntities/Read | Azure AD Raporlarını okuyun. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="company-administrator"></a>Şirket Yöneticisi
Azure AD'nin ve Azure AD kimliklerini kullanan Microsoft hizmetlerinin tüm özelliklerini yönetebilir. Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" tanımlanır. "Genel yönetici" olarak [Azure portalında](https://portal.azure.com).

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/AllActions/AllProperties | AdministrativeUnits oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Application/AllActions/AllProperties | Uygulamaları oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/AppRoleAssignment/AllActions/AllProperties | AppRoleAssignments oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/CollaborationSpace/AllActions/AllProperties | CollaborationSpaces oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Contact/AllActions/AllProperties | Kişiler oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Device/AllActions/AllProperties | Cihazlar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/DirectoryRole/AllActions/AllProperties | DirectoryRoles oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/DirectoryRoleTemplate/AllActions/AllProperties | DirectoryRoleTemplates oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/DirectorySetting/AllActions/AllProperties | DirectorySettings oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/DirectorySettingTemplate/AllActions/AllProperties | DirectorySettingTemplates oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Domain/AllActions/AllProperties | Etki Alanları oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Group/AllActions/AllProperties | Gruplar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/LoginTenantBranding/AllActions/AllProperties | LoginTenantBrandings oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/OAuth2PermissionGrant/AllActions/AllProperties | OAuth2PermissionGrants oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Policy/AllActions/AllProperties | İlkeler oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/ServiceAction/ConsentOnBehalfOfAllWithDirectory | Azure Active Directory (Azure AD Graph ve Microsoft Graph) dahil tüm kaynaklara tüm kullanıcılar adına izin verebilir. |
| microsoft.aad.directory/ServicePrincipal/AllActions/AllProperties | ServicePrincipals oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Organization/AllActions/AllProperties | Kuruluşları oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/User/AllActions/AllProperties | Kullanıcılar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.aadconnect/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve microsoft.aad.aadconnect üzerinde standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.billing/AllEntities/AllActions | Office 365 faturalamasının tüm özelliklerini yönetin. |
| microsoft.aad.compliance/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Uyumluluk Merkezi'nde standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directorysync/AllEntities/AllActions | Azure AD Connect'te tüm eylemleri gerçekleştirin. |
| microsoft.aad.lockbox/AllEntities/AllActions | Kasa hizmetinin tüm özelliklerini yönetin. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Privileged Role Management hizmetinin tüm özelliklerini yönetin. |
| microsoft.aad.reports/AllEntities/AllActions | Azure AD Raporlarını okuyun ve yapılandırın. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.crm/AllEntities/AllActions | Dynamics 365'in tüm özelliklerini yönetin. |
| microsoft.exchange/AllEntities/AllActions | Exchange Online'ın tüm özelliklerini yönetin. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.intune/AllEntities/AllActions | Intune'un tüm özelliklerini yönetin. |
| microsoft.powerbi/AllEntities/AllActions | Power BI'ın tüm özelliklerini yönetin. |
| microsoft.protectioncenter/AllEntities/AllActions | Office 365 Koruma Merkezi'ni yönetin. |
| microsoft.sharepoint/AllEntities/AllActions | SharePoint Online'ı yönetin. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Skype Kurumsal Çevrimiçi Sürüm'ü yönetin. |

### <a name="compliance-administrator"></a>Uyumluluk Yöneticisi
Azure AD ve Office 365'te uyumluluk yapılandırmasını ve raporları okuyup yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.compliance/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Uyumluluk Merkezi'nde standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.exchange/Compliance/AllActions | Exchange Online'da uyumluluğu yönetin. |
| microsoft.sharepoint/Compliance/AllActions | SharePoint Online'da uyumluluğu yönetin. |
| microsoft.skypeforbusiness/Compliance/AllActions | Skype Kurumsal Çevrimiçi Sürüm'de uyumluluğu yönetin. |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi
Koşullu erişim özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/ConditionalAccessPolicy/Create | Azure Active Directory'de ConditionalAccessPolicy oluşturun. |
| microsoft.aad.directory/ConditionalAccessPolicy/Delete | Azure Active Directory'de ConditionalAccessPolicys silin. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read | Azure Active Directory'de ConditionalAccessPolicys üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/Owners | Azure Active Directory'de ConditionalAccessPolicys.Owners özelliğini okuyun. |
| microsoft.aad.directory/ConditionalAccessPolicy/Read/PolicyAppliedTo | Azure Active Directory'de ConditionalAccessPolicys.PolicyAppliedTo özelliğini okuyun. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update | Azure Active Directory'de ConditionalAccessPolicys üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/ConditionalAccessPolicy/Update/Owners | Azure Active Directory'de ConditionalAccessPolicys.Owners özelliğini güncelleştirin. |

### <a name="device-administrators"></a>Cihaz Yöneticileri

Bu role sahip kullanıcılar, Azure Active Directory'ye katılan tüm Windows 10 cihazları üzerinde yerel makine yöneticisi olur. Azure Active Directory'de cihaz nesnelerini yönetme olanağına sahip değildir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

### <a name="directory-readers"></a>Dizin Okuyucular
Temel dizin bilgileri okuyabilir. Uygulamalara erişim vermek için.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/AdministrativeUnit/Read | Azure Active Directory'de AdministrativeUnits üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/AdministrativeUnit/Read/Members | Azure Active Directory'de AdministrativeUnits.Members özelliğini okuyun. |
| microsoft.aad.directory/Application/Read | Azure Active Directory'de Uygulamalar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Application/Read/Owners | Azure Active Directory'de Applications.Owners özelliğini okuyun. |
| microsoft.aad.directory/CollaborationSpace/Read | Azure Active Directory'de CollaborationSpaces üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/CollaborationSpace/Read/Owners | Azure Active Directory'de CollaborationSpaces.Owners özelliğini okuyun. |
| microsoft.aad.directory/Contact/Read | Azure Active Directory'de Kişiler üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Contact/Read/MemberOf | Azure Active Directory'de Contacts.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/Device/Read | Azure Active Directory'de Cihazlar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Device/Read/MemberOf | Azure Active Directory'de Devices.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/Device/Read/RegisteredOwners | Azure Active Directory'de Devices.RegisteredOwners özelliğini okuyun. |
| microsoft.aad.directory/Device/Read/RegisteredUsers | Azure Active Directory'de Devices.RegisteredUsers özelliğini okuyun. |
| microsoft.aad.directory/DirectoryRole/Read | Azure Active Directory'de DirectoryRoles üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/DirectoryRole/Read/EligibleMembers | Azure Active Directory'de DirectoryRoles.EligibleMembers özelliğini okuyun. |
| microsoft.aad.directory/DirectoryRole/Read/Members | Azure Active Directory'de DirectoryRoles.Members özelliğini okuyun. |
| microsoft.aad.directory/DirectorySetting/Read | Azure Active Directory'de DirectorySettings üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/DirectorySettingTemplate/Read | Azure Active Directory'de DirectorySettingTemplates üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Domain/Read | Azure Active Directory'de Etki Alanları üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Read/AppRoleAssignments | Azure Active Directory'de Groups.AppRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/Group/Read/MemberOf | Azure Active Directory'de Groups.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/Group/Read/Members | Azure Active Directory'de Groups.Members özelliğini okuyun. |
| microsoft.aad.directory/Group/Read/Owners | Azure Active Directory'de Groups.Owners özelliğini okuyun. |
| microsoft.aad.directory/Group/Read/Settings | Azure Active Directory'de Groups.Settings özelliğini okuyun. |
| microsoft.aad.directory/OAuth2PermissionGrant/Read | Azure Active Directory'de OAuth2PermissionGrants üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read | Azure Active Directory'de ServicePrincipals üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Azure Active Directory'de ServicePrincipals.AppRoleAssignedTo özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Azure Active Directory'de ServicePrincipals.AppRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Azure Active Directory'de ServicePrincipals.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Azure Active Directory'de ServicePrincipals.OAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Azure Active Directory'de ServicePrincipals.Owners özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Azure Active Directory'de ServicePrincipals.OwnedObjects özelliğini okuyun. |
| microsoft.aad.directory/Organization/Read | Azure Active Directory'de Kuruluşlar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/Read | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/User/Read/DirectReports | Azure Active Directory'de Users.DirectReports özelliğini okuyun. |
| microsoft.aad.directory/User/Read/InvitedBy | Azure Active Directory'de Users.InvitedBy özelliğini okuyun. |
| microsoft.aad.directory/User/Read/InvitedUsers | Azure Active Directory'de Users.InvitedUsers özelliğini okuyun. |
| microsoft.aad.directory/User/Read/Manager | Azure Active Directory'de Users.Manager özelliğini okuyun. |
| microsoft.aad.directory/User/Read/MemberOf | Azure Active Directory'de Users.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Azure Active Directory'de Users.OAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OwnedDevices | Azure Active Directory'de Users.OwnedDevices özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OwnedObjects | Azure Active Directory'de Users.OwnedObjects özelliğini okuyun. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Azure Active Directory'de Users.RegisteredDevices özelliğini okuyun. |

### <a name="directory-synchronization-accounts"></a>Dizin eşitlemesi hesapları
Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Policy/Create | Azure Active Directory'de İlkeler oluşturun. |
| microsoft.aad.directory/Policy/Delete | Azure Active Directory'de İlkeleri silin. |
| microsoft.aad.directory/Policy/Read | Azure Active Directory'de İlkeler üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Policy/Read/Owners | Azure Active Directory'de Policies.Owners özelliğini okuyun. |
| microsoft.aad.directory/Policy/Read/PolicyAppliedTo | Azure Active Directory'de Policies.PolicyAppliedTo özelliğini okuyun. |
| microsoft.aad.directory/Policy/Update | Azure Active Directory'de İlkeler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Update/Owners | Azure Active Directory'de Policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Create | Azure Active Directory'de ServicePrincipals oluşturun. |
| microsoft.aad.directory/ServicePrincipal/Read | Azure Active Directory'de ServicePrincipals üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignedTo | Azure Active Directory'de ServicePrincipals.AppRoleAssignedTo özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/AppRoleAssignments | Azure Active Directory'de ServicePrincipals.AppRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/MemberOf | Azure Active Directory'de ServicePrincipals.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/OAuth2PermissionGrants | Azure Active Directory'de ServicePrincipals.OAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/Owners | Azure Active Directory'de ServicePrincipals.Owners özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Read/OwnedObjects | Azure Active Directory'de ServicePrincipals.OwnedObjects özelliğini okuyun. |
| microsoft.aad.directory/ServicePrincipal/Update | Azure Active Directory'de ServicePrincipals üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignedTo | Azure Active Directory'de ServicePrincipals.AppRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/AppRoleAssignments | Azure Active Directory'de ServicePrincipals.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/Owners | Azure Active Directory'de ServicePrincipals.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Update/DirSync | Azure Active Directory'de Organizations.DirSync özelliğini güncelleştirin. |
| microsoft.aad.directorysync/AllEntities/AllActions | Azure AD Connect'te tüm eylemleri gerçekleştirin. |

### <a name="directory-writer"></a>Directory yazıcısı
Okuma ve yazma temel dizin bilgileri kullanabilirsiniz. Uygulamalara erişim vermek için

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/DirectorySetting/Create | Azure Active Directory'de DirectorySettings oluşturun. |
| microsoft.aad.directory/DirectorySetting/Delete | Azure Active Directory'de DirectorySettings'i silin. |
| microsoft.aad.directory/DirectorySetting/Update | Azure Active Directory'de DirectorySettings üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Create | Azure Active Directory'de Gruplar oluşturun. |
| microsoft.aad.directory/Group/CreateAsOwner | Azure Active Directory'de Gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Update | Azure Active Directory'de Gruplar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Azure Active Directory'de Groups.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Members | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Owners | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Settings | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/User/Update | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Manager | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |

### <a name="dynamics-365-service-administrator"></a>Dynamics 365 Hizmet Yöneticisi
Dynamics 365 ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.crm/AllEntities/AllActions | Dynamics 365'in tüm özelliklerini yönetin. |

### <a name="exchange-service-administrator"></a>Exchange Hizmeti Yöneticisi
Exchange ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.exchange/AllEntities/AllActions | Exchange Online'ın tüm özelliklerini yönetin. |

### <a name="guest-inviter"></a>Konuk Davet Eden
Bağımsız olarak Konuk kullanıcıları davet etmeden **üyeler Konuk davet** ayarı.

  > [!NOTE]
  > Bu rol Konuk rolünden ek izinleri devralır.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/User/InviteGuest | Azure Active Directory'de konuk kullanıcıları davet edin. |
| microsoft.aad.directory/User/Read | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/User/Read/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/User/Read/DirectReports | Azure Active Directory'de Users.DirectReports özelliğini okuyun. |
| microsoft.aad.directory/User/Read/InvitedBy | Azure Active Directory'de Users.InvitedBy özelliğini okuyun. |
| microsoft.aad.directory/User/Read/InvitedUsers | Azure Active Directory'de Users.InvitedUsers özelliğini okuyun. |
| microsoft.aad.directory/User/Read/Manager | Azure Active Directory'de Users.Manager özelliğini okuyun. |
| microsoft.aad.directory/User/Read/MemberOf | Azure Active Directory'de Users.MemberOf özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OAuth2PermissionGrants | Azure Active Directory'de Users.OAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OwnedDevices | Azure Active Directory'de Users.OwnedDevices özelliğini okuyun. |
| microsoft.aad.directory/User/Read/OwnedObjects | Azure Active Directory'de Users.OwnedObjects özelliğini okuyun. |
| microsoft.aad.directory/User/Read/RegisteredDevices | Azure Active Directory'de Users.RegisteredDevices özelliğini okuyun. |

### <a name="helpdesk-administrator"></a>Yardım Masası Yöneticisi
Yönetici olmayan kullanıcıların ve Yardım Masası Yöneticilerinin parolalarını sıfırlayabilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/User/Update/PasswordHelpdeskScope | Sınırlı yöneticilerinin ve Azure Active Directory'de diğer Yardım Masası yöneticilerinin parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="information-protection-administrator"></a>Bilgi Koruma Yöneticisi
Azure Information Protection ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.informationprotection/AllEntities/AllActions | Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="intune-service-administrator"></a>Intune Hizmet Yöneticisi
Intune ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Contact/Create | Azure Active Directory'de Kişiler oluşturun. |
| microsoft.aad.directory/Contact/Delete | Azure Active Directory'de Kişileri silin. |
| microsoft.aad.directory/Contact/Update | Azure Active Directory'de Kişiler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Device/Create | Azure Active Directory'de Cihazlar oluşturun. |
| microsoft.aad.directory/Device/Delete | Azure Active Directory'de Cihazları silin. |
| microsoft.aad.directory/Device/Update | Azure Active Directory'de Cihazlar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Device/Update/RegisteredOwners | Azure Active Directory'de Devices.RegisteredOwners özelliğini güncelleştirin. |
| microsoft.aad.directory/Device/Update/RegisteredUsers | Azure Active Directory'de Devices.RegisteredUsers özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Create | Azure Active Directory'de Gruplar oluşturun. |
| microsoft.aad.directory/Group/CreateAsOwner | Azure Active Directory'de Gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/Group/Delete | Azure Active Directory'de Grupları silin. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Azure Active Directory'de Groups.HiddenMembers özelliğini okuyun. |
| microsoft.aad.directory/Group/Restore | Azure Active Directory'de Grupları geri yükleyin. |
| microsoft.aad.directory/Group/Update | Azure Active Directory'de Gruplar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Azure Active Directory'de Groups.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Members | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Owners | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Settings | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/Update | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Manager | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.intune/AllEntities/AllActions | Intune'un tüm özelliklerini yönetin. |


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
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.skypeforbusiness/AllEntities/AllActions | Skype Kurumsal Çevrimiçi Sürüm'ü yönetin. |

### <a name="message-center-reader"></a>İleti Merkezi Okuyucu
Yalnızca Office 365 İleti Merkezi'nde kuruluşuna yönelik iletileri ve güncelleştirmeleri okuyabilir. 

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.accessmessagecenter/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve İleti Merkezi'nde standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |

### <a name="partner-tier1-support"></a>Partner Tier1 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Contact/Create | Azure Active Directory'de Kişiler oluşturun. |
| microsoft.aad.directory/Contact/Delete | Azure Active Directory'de Kişileri silin. |
| microsoft.aad.directory/Contact/Update | Azure Active Directory'de Kişiler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Create | Azure Active Directory'de Gruplar oluşturun. |
| microsoft.aad.directory/Group/CreateAsOwner | Azure Active Directory'de Gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Update/Members | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Owners | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/User/Delete | Azure Active Directory'de Kullanıcıları silin. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/User/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/User/Update | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Manager | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/PasswordUserScope | Yönetici olmayanlar için Azure Active Directory'de parolaları güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="partner-tier2-support"></a>Partner Tier2 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Contact/Create | Azure Active Directory'de Kişiler oluşturun. |
| microsoft.aad.directory/Contact/Delete | Azure Active Directory'de Kişileri silin. |
| microsoft.aad.directory/Contact/Update | Azure Active Directory'de Kişiler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Domain/AllActions | Etki Alanları oluşturup silin ve Azure Active Directory'de standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/Group/Create | Azure Active Directory'de Gruplar oluşturun. |
| microsoft.aad.directory/Group/Delete | Azure Active Directory'de Grupları silin. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Restore | Azure Active Directory'de Grupları geri yükleyin. |
| microsoft.aad.directory/Group/Update/Members | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/Organization/Update | Azure Active Directory'de Kuruluşlar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Organization/Update/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini güncelleştirin. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/User/Delete | Azure Active Directory'de Kullanıcıları silin. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/User/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/User/Update | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Manager | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Password | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="power-bi-service-administrator"></a>Power BI Hizmet Yöneticisi
Power BI ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.powerbi/AllEntities/AllActions | Power BI'ın tüm özelliklerini yönetin. |

### <a name="privileged-role-administrator"></a>Ayrıcalıklı Rol Yöneticisi
Azure AD'de rol atamalarını yönetebilir

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/DirectoryRole/Update | Azure Active Directory'de DirectoryRoles üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.privilegedrolemanagement/AllEntities/AllActions | Privileged Role Management hizmetinin tüm özelliklerini yönetin. |

### <a name="reports-reader"></a>Rapor Okuyucu
Oturum açma ve denetim raporlarını okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.reports/AllEntities/Read | Azure AD Raporlarını okuyun. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.usagereports/AllEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Güvenlik bilgilerini ve raporları okuyabilir

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Application/Update/DefaultPolicy | Azure Active Directory'de Applications.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/Policy/Create | Azure Active Directory'de İlkeler oluşturun. |
| microsoft.aad.directory/Policy/Delete | Azure Active Directory'de İlkeleri silin. |
| microsoft.aad.directory/Policy/Update | Azure Active Directory'de İlkeler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Policy/Update/Owners | Azure Active Directory'de Policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/ServicePrincipal/Update/DefaultPolicy | Azure Active Directory'de ServicePrincipals.DefaultPolicy özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.privilegedrolemanagement/AllEntities/Read | Privileged Identity Management hakkındaki ayrıntıları okuyun. |
| microsoft.protectioncenter/AllEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| microsoft.protectioncenter/AllEntities/Update | Office 365 Koruma Merkezi'ni yönetin. |

### <a name="security-reader"></a>Güvenlik Okuyucu
Azure AD ve Office 365'te güvenlik bilgilerini ve raporları okuyabilir.

  > [!NOTE]
  > Bu rol dizin okuyucular rolünden ek izinleri devralır.
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.privilegedidentitymanagement/AllEntities/Read | Privileged Identity Management hakkındaki ayrıntıları okuyun. |
| microsoft.protectioncenter/AllEntities/Read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |

### <a name="service-support-administrator"></a>Hizmet Desteği Yöneticisi
Hizmet durumu bilgilerini okuyabilir ve destek biletlerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="sharepoint-service-administrator"></a>SharePoint Hizmet Yöneticisi
SharePoint hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Rol tanımı yukarıda daha fazla bilgi için bkz.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.sharepoint/AllEntities/AllActions | SharePoint Online'ı yönetin. |

### <a name="user-account-administrator"></a>Kullanıcı Hesabı Yöneticisi
Kullanıcıların ve grupların tüm özelliklerini yönetebilir

  > [!NOTE]
  > Bu rol ek izinlerinden devralan [kullanıcı rolü](https://docs.microsoft.com/azure/active-directory/users-default-permissions).
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/AppRoleAssignment/Create | Azure Active Directory'de AppRoleAssignments oluşturun. |
| microsoft.aad.directory/AppRoleAssignment/Delete | Azure Active Directory'de AppRoleAssignments'ı silin. |
| microsoft.aad.directory/AppRoleAssignment/Update | Azure Active Directory'de AppRoleAssignments üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Contact/Create | Azure Active Directory'de Kişiler oluşturun. |
| microsoft.aad.directory/Contact/Delete | Azure Active Directory'de Kişileri silin. |
| microsoft.aad.directory/Contact/Update | Azure Active Directory'de Kişiler üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Create | Azure Active Directory'de Gruplar oluşturun. |
| microsoft.aad.directory/Group/CreateAsOwner | Azure Active Directory'de Gruplar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/Group/Delete | Azure Active Directory'de Grupları silin. |
| microsoft.aad.directory/Group/Read | Azure Active Directory'de Gruplar üzerindeki standart özellikleri okuyun. |
| microsoft.aad.directory/Group/Read/HiddenMembers | Azure Active Directory'de Groups.HiddenMembers özelliğini okuyun. |
| microsoft.aad.directory/Group/Restore | Azure Active Directory'de Grupları geri yükleyin. |
| microsoft.aad.directory/Group/Update | Azure Active Directory'de Gruplar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/Group/Update/AppRoleAssignments | Azure Active Directory'de Groups.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Members | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Owners | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/Group/Update/Settings | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/Organization/Read/TrustedCAsForPasswordlessAuth | Azure Active Directory'de Organizations.TrustedCAsForPasswordlessAuth özelliğini okuyun. |
| microsoft.aad.directory/User/AssignLicense | Azure Active Directory'de Kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/User/Create | Azure Active Directory'de Kullanıcılar oluşturun. |
| microsoft.aad.directory/User/Delete | Azure Active Directory'de Kullanıcıları silin. |
| microsoft.aad.directory/User/InvalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/User/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/User/Update | Azure Active Directory'de Kullanıcılar üzerindeki standart özellikleri güncelleştirin. |
| microsoft.aad.directory/User/Update/AppRoleAssignments | Azure Active Directory'de Users.AppRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/Manager | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.directory/User/Update/PasswordUserAcctAdminScope | Sınırlı yöneticilerin, Yardım Masası yöneticileri ve diğer Azure Active Directory kullanıcı hesabı yöneticilerin parolalarını güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.aad.accessservice/AllEntities/AllActions | Tüm kaynakları oluşturup silin ve Azure Access Control'da standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.supporttickets/AllEntities/AllActions | Office 365 destek biletleri oluşturun ve yönetin. |
| microsoft.aad.servicehealth/AllEntities/AllActions | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliğine yönelik olarak yöneticileri değiştirme hakkında daha fazla bilgi için bkz. [Azure yönetici rollerini ekleme veya değiştirme](../../billing/billing-add-change-azure-subscription-administrator.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
