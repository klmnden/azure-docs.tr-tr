---
title: Yönetici rolü açıklamaları ve izinleri - Azure Active Directory | Microsoft Docs
description: Yönetici rolü kullanıcı ekleme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme.
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82afadef58310f46046c8c3168ed93a34769b316
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609532"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory'de Yönetici rolü izinleri

Azure Active Directory (Azure AD) kullanarak, daha az ayrıcalıklı rolleri kimlik görevleri yönetmek için sınırlı yöneticileri atayabilirsiniz. Yöneticiler, ekleme veya kullanıcıları için değiştirme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi amaçlarla atanabilir. Varsayılan kullanıcı izinleri yalnızca kullanıcı ayarları, Azure AD'de değiştirilebilir.

Genel yönetici, tüm yönetim özelliklerine erişebilir. Varsayılan olarak, bir Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca genel Yöneticiler ve ayrıcalıklı rol yöneticileri, yönetici rollerini devredebilirsiniz. Risk iş riskinizi azaltmak için şirketinizde yalnızca birkaç kişinin bu role atamak öneririz.


## <a name="assign-or-remove-administrator-roles"></a>Atamayı veya kaldırmayı yönetici rolleri

Azure Active Directory'de bir kullanıcıya yönetici rollerini atama hakkında bilgi edinmek için [Azure Active Directory'de yönetici rolleri görüntüleyin ve Ata](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki Yönetici rollerini kullanılabilir:

* **[Uygulama Yöneticisi](#application-administrator)**: Bu roldeki kullanıcılar oluşturabilir ve kurumsal uygulamaları, uygulama kayıtları ve uygulama proxy'si ayarları tüm özelliklerini yönetebilir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Uygulama Yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Uygulama geliştiricisi](#application-developer)**: Bu roldeki kullanıcılar, uygulama kayıtları oluşturabilir, "Kullanıcılar uygulamaları kaydedebilir" ayarı Hayır olarak ayarlayın Bu rol, ayrıca birinin kendi adınıza onaylamasına izin verir, "Kullanıcı izni verebilir uygulamalara kendileri adına şirket verilerine erişme" ayarı Hayır olarak ayarlayın Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenir.

* **[Kimlik doğrulaması yönetici](#authentication-administrator)**: Bu role sahip kullanıcılar, ayarlayın ya da parolası olmayan kimlik bilgilerini sıfırlayın. Kimlik doğrulaması yöneticileri, kullanıcıların mevcut olmayan bir parola kimlik bilgisi karşı (örneğin, MFA veya FIDO) yeniden kaydedin ve iptal etmesine gerektirebilirsiniz **cihazda MFA unutmayın**, hangi ister MFA için bir sonraki oturum açma olan kullanıcıların Yönetici olmayanlar veya yalnızca aşağıdaki rolleri atanmış:
  * Kimlik Doğrulaması Yöneticisi
  * Dizin Okuyucuları
  * Konuk Davet Eden
  * İleti Merkezi Okuyucusu
  * Rapor Okuyucusu

  Kimlik doğrulaması yönetici rolünün şu anda genel Önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için kimlik bilgilerini değiştirebilirsiniz. Bir kullanıcının kimlik bilgilerini değiştirme, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:

  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve kimlik yöneticileri için verilen başka bir yerde değil. Kimlik doğrulaması yönetici uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden ayrıcalıklı bir uygulama kimliği, uygulama için kimlik bilgilerini güncelleştirerek varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

* **[Faturalama Yöneticisi](#billing-administrator)**: Satın alma gerçekleştirir, abonelikleri yönetir, destek biletlerini yönetir ve hizmet sistem durumunu izler.

* **[Bulut uygulaması Yöneticisi](#cloud-application-administrator)**: Bu roldeki kullanıcılar, uygulama proxy'si yönetme olanağı hariç uygulama yöneticisi rolü aynı izinlere sahip. Bu rol oluşturmak ve kurumsal uygulamaları ve uygulama kayıtlarını tüm yönlerini yönetmek için yeteneği verir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Bulut uygulaması yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Bulut cihaz Yöneticisi](#cloud-device-administrator)**: Bu roldeki kullanıcılar etkinleştirebilir, devre dışı bırakma ve Azure AD'de cihazları silin ve Windows 10 BitLocker anahtarları Azure Portalı'nda (varsa) okuyun. Rol, cihazdaki diğer özelliklerini yönetmek için izinleri tanımaz.

* **[Uyumluluk Yöneticisi](#compliance-administrator)**: Bu role sahip kullanıcılar, Microsoft 365 Uyumluluk Merkezi, Microsoft 365 Yönetim Merkezi, Azure ve Office 365 güvenlik ve uyumluluk Merkezi'nde özelliklerini ilgili yönetmek için izinlere sahip. Kullanıcılar ayrıca iş Yönetim Merkezi için Exchange yönetici merkezini ve takımlar ve Skype Kurumsal içinde tüm özelliklerini yönetebilir ve Azure ve Microsoft 365 için destek bileti oluşturun. Daha fazla bilgi edinilebilir [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  İçinde | Yapabilirsiniz
  ----- | ----------
  [Microsoft 365 Uyumluluk Merkezi](https://protection.office.com) | Koruma ve Microsoft 365 hizmetlerindeki kuruluşunuzun verilerini yönetme<br>Uyumluluk Uyarıları yönetme
  [Uyumluluk Yöneticisi](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | İzleme, atamak ve kuruluşunuzun Mevzuata uygunluk etkinliklerinin doğrulayın
  [Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Veri Yönetimi yönetme<br>Yasal ve veri araştırma gerçekleştirme<br>Veri sahibi isteği yönetme
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Tüm Intune denetim verilerini görüntüleme
  [Cloud App Security'yi](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinlere sahiptir ve Uyarıları yönetebilir<br>Oluşturma ve dosya ilkeleri değiştirebilir ve dosya idare eylemleri izin ver<br> Veri Yönetimi altındaki tüm yerleşik raporları görüntüleyebilirsiniz

<!--* **[Compliance Data Administrator](#compliance-data-administrator)**: Users with this role have permissions to protect and track data in the Microsoft 365 compliance center, Microsoft 365 admin center, and Azure. Users can also manage all features within the Exchange admin center, Compliance Manager, and Teams & Skype for Business admin center and create support tickets for Azure and Microsoft 365.

  In | Can do
  ----- | ----------
  [Microsoft 365 compliance center](https://protection.office.com) | Monitor compliance-related policies across Microsoft 365 services<br>Manage compliance alerts
  [Compliance Manager](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Track, assign, and verify your organization's regulatory compliance activities
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Manage data governance<br>Perform legal and data investigation<br>Manage Data Subject Request
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | View all Intune audit data
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Has read-only permissions and can manage alerts<br>Can create and modify file policies and allow file governance actions<br> Can view all the built-in reports under Data Management
-->
* **[Koşullu Erişim Yöneticisi](#conditional-access-administrator)**: Bu role sahip olan kullanıcılar, Azure Active Directory koşullu erişim ayarlarını yönetebilir.
  > [!NOTE]
  > Azure'da Exchange ActiveSync koşullu erişim ilkesi dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **[Müşteri kasası erişim onaylayıcı](#customer-lockbox-access-approver)**: Yöneten [müşteri kasa istekleri](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) kuruluşunuzdaki. Bunlar müşteri kasa istekleri için e-posta bildirimleri almak ve onaylayabilir ve Microsoft 365 Yönetim merkezinden istekleri reddetme. Bunlar ayrıca müşteri kasa özelliğini açıp kapatabilirsiniz. Yalnızca genel Yöneticiler, bu role atanan kişi parolalarını sıfırlayabilir.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Cihaz yöneticileri](#device-administrators)**: Bu rol ataması yalnızca ek yerel yönetici olarak kullanılabilir [cihaz ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Bu role sahip kullanıcılar, Azure Active Directory'ye katılan tüm Windows 10 cihazları üzerinde yerel makine yöneticisi olur. Azure Active Directory'de cihaz nesnelerini yönetme olanağına sahip değildir. 

* **[Dizin okuyucular](#directory-readers)**: Desteklemeyen yalnızca eski uygulamalar atanmış bir rol budur [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Bu, kullanıcılara atamayın.

* **[Dizin eşitlemesi hesapları](#directory-synchronization-accounts)**: Kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil hedeflenen veya herhangi bir kullanım için desteklenir.

* **[Dizin yazıcıları](#directory-writers)**: Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dynamics 365 Yönetici / CRM yönetici](#crm-service-administrator)**: Bu role sahip kullanıcılar Microsoft Dynamics 365 hizmet mevcut olduğunda Online içinde genel izinlere sahip olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [kiracınızı yönetmek için Hizmet Yöneticisi rolü kullanmak](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Hizmet Yöneticisi olarak Dynamics 365" tanımlanır. "Dynamics 365 Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Exchange Yöneticisi](#exchange-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. Ayrıca, oluşturmak ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumunu izlemek için özelliğine sahiptir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Exchange Hizmet Yöneticisi" olarak tanımlanır. "Exchange Yöneticisi" olarak [Azure portalında](https://portal.azure.com). "Exchange Online Yönetici" olarak [Exchange yönetici merkezini](https://go.microsoft.com/fwlink/p/?LinkID=529144). 


* **[Genel yönetici / şirket Yöneticisi](#company-administrator)**: Bu role sahip kullanıcılar gibi Microsoft 365 Güvenlik Merkezi, Azure Active Directory kimlikleri kullanmak hizmetlerinin yanı sıra Azure Active Directory, tüm yönetim özelliklerine erişim sahibi Microsoft 365 Uyumluluk Merkezi, Exchange Online, SharePoint Online ve Skype Kurumsal çevrimiçi. Azure Active Directory kiracısı için kaydolan kişi genel yönetici olur. Yalnızca genel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel Yöneticiler, herhangi bir kullanıcı ve diğer tüm yöneticilerin parolasını sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" tanımlanır. "Genel yönetici" olarak [Azure portalında](https://portal.azure.com).
  >
  >

* **[Konuk davet eden](#guest-inviter)**: Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetlerini yönetebilir, **üyelerini davet edebildiğiniz** kullanıcı ayarı Hayır olarak ayarlayın B2B işbirliği hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Tüm diğer izinleri içermez.

* **[Bilgi Koruma Yöneticisi](#information-protection-administrator)**: Bu role sahip kullanıcılar Azure Information Protection hizmetinde tüm izinlere sahip. Bu rol, Azure Information Protection ilkesi için etiketleri yapılandırma, koruma şablonlarını yönetme ve koruma etkinleştirme sağlar. Bu rol, kimlik koruma Merkezi, Privileged Identity Management, Office 365 hizmet durumunu izleme, veya Office 365 güvenlik ve uyumluluk Merkezi'nde herhangi bir izni tanımaz.

* **[Intune yönetici](#intune-service-administrator)**: Bu role sahip kullanıcılar, hizmet olduğunda Microsoft Intune Online içinde genel izinlere sahip. Ayrıca, bu rol, ilke ilişkilendirmek yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme olanağı içerir. Daha fazla bilgiye [Intune rol tabanlı yönetim denetimi (RBAC)](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Intune Hizmet Yöneticisi" olarak tanımlanır. "Intune Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Lisans Yöneticisi](#license-administrator)**: Bu roldeki kullanıcılar ekleyebilir, kaldırma ve güncelleştirme lisans atamalarında kullanıcıları, grupları (Grup tabanlı lisanslama kullanarak) ve kullanım konumu kullanıcıları yönetme. Rol, satın alma veya Aboneliklerini yönetmek, oluşturma veya grupları yönetme veya oluşturma veya ötesinde kullanım konumu yönetme olanağı tanımaz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[İleti Merkezi okuyucu](#message-center-reader)**: Bu roldeki kullanıcılar, bildirimler ve danışmanlık sistem güncelleştirmeleri izleyebilirsiniz [Office 365 ileti Merkezi](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) kuruluşlarında Exchange, Intune ve Microsoft Teams gibi yapılandırılmış hizmetleri. İleti Merkezi okuyucular Haftalık e-posta özetler gönderilerin, güncelleştirmeleri almak ve Office 365 ileti merkezi gönderileri paylaşabilirsiniz. Azure AD'de bu role atanan kullanıcılar yalnızca salt okunur kullanıcılar ve gruplar gibi Azure AD Hizmetleri erişebilir. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[İş ortağı Tier1 desteği](#partner-tier1-support)**: Kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[İş ortağı Tier2 desteği](#partner-tier2-support)**: Kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[(Parola) Yardım Masası Yöneticisi](#helpdesk-administrator)**: Bu role sahip olan kullanıcılar parolaları değiştirme, yenileme belirteçleri geçersiz, hizmet isteklerini yönetebilir ve hizmet durumunu izleyebilir. Bir yenileme belirteci geçersiz kılmalarını, kullanıcı yeniden oturum açmak için zorlar. Yardım Masası yöneticileri, parolaları sıfırlama ve yenileme belirteçleri olmayanların ya da yalnızca aşağıdaki roller atanan diğer kullanıcıların geçersiz kıl:
  * Dizin Okuyucuları
  * Konuk Davet Eden
  * Yardım Masası Yöneticisi
  * İleti Merkezi Okuyucusu
  * Rapor Okuyucusu
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolaları değiştirebilirsiniz. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve Yardım Masası yöneticileri izni başka bir yerde değil. Yardım Masası Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

  > [!NOTE]
  > Bu rolü daha önce "parola Yöneticisi" çağrıldı [Azure portalında](https://portal.azure.com/). Azure AD PowerShell, Azure AD Graph API'si ve Microsoft Graph API adını eşleştirmek için "Yardım Masası Yöneticisi" için biz adını değiştirmiş olursunuz. Kısa bir süre için adı "Yardım Masası (parola) yönetici için" Azure portalında "Yardım Masası Yöneticisi" değişiklikten önce değiştireceğiz.
  >
  
* **[Power BI Yöneticisi](#power-bi-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Power BI içinde genel izinlere sahip olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [Power BI yönetici rolünü anlama](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Power BI Hizmet Yöneticisi olarak" olarak tanımlanır. "Power BI Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Ayrıcalıklı kimlik doğrulaması yönetici](#privileged-authentication-administrator)**: Bu role sahip kullanıcılar, ayarlayın ya da genel yöneticileri dahil tüm kullanıcılar için parola olmayan kimlik bilgilerini sıfırlayın. Kullanıcıların mevcut olmayan bir parola kimlik bilgisi karşı (örn: MFA'yı FIDO) yeniden kaydedin ve 'MFA cihazda unutmayın', iptal etme ayrıcalıklı kimlik doğrulaması yöneticileri zorlayabilirsiniz MFA için tüm kullanıcılar sonraki oturum açma istemi. Ayrıcalıklı kimlik doğrulaması yöneticiler şunları yapabilir:
  * Kullanıcıları, mevcut olmayan bir parola kimlik bilgisi karşı (örn: MFA'yı FIDO) yeniden kaydetmek için
  * 'MFA cihazda unutmayın', iptal etmek için mfa'yı bir sonraki oturum açma istemi

* **[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator)**: Bu role sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rol, Privileged Identity Management'ın tüm yönlerini yönetilmesine izin verir.

  <b>Önemli</b>: Bu rolü genel Yönetici rolüne dahil olmak üzere tüm Azure AD rol atamalarını yönetme olanağı sağlar. Bu rol, oluşturma veya kullanıcıları güncelleştirerek gibi Azure AD'de herhangi bir ayrıcalıklı yeteneklerini içermez. Ancak, bu role atanan kullanıcılar kendileri veya diğerleri ek ayrıcalık ek rolleri atayarak verebilirsiniz.

* **[Rapor okuyucu](#reports-reader)**: Power BI'da raporları Pano Microsoft 365 Yönetim merkezinde ve benimseme bağlam paketini ve bu role sahip kullanıcılar, kullanım raporlama verileri görüntüleyebilir. Ayrıca, rol oturum açma için erişim sağlayan raporları ve etkinlik Azure AD'de ve Microsoft Graph tarafından döndürülen veri raporlama API'si. Rapor okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar, ayarları veya Exchange gibi ürüne özel yönetim merkezlerine erişimi yapılandırmak için bir yönetici izinleri yok. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[Güvenlik Yöneticisi](#security-administrator)**: Bu role sahip kullanıcılar, Microsoft 365 Güvenlik Merkezi, Azure Active Directory kimlik koruması, Azure Information Protection ve Office 365 güvenlik ve uyumluluk Merkezi'nde özelliklerini güvenlikle ilgili yönetmek için izinlere sahip. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  İçinde | Yapabilirsiniz
  --- | ---
  [Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetlerindeki güvenlikle ilgili ilkelerini izleme<br>Güvenlik tehditlerini ve Uyarıları yönetme<br>Raporları görüntüleme
  Kimlik Koruma Merkezi | Güvenlik okuyucu rolünün tüm izinler<br>Ayrıca, parola sıfırlama dışında kimlik koruma merkezi tüm işlemleri gerçekleştirme imkanı
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Güvenlik okuyucu rolünün tüm izinler<br>**Olamaz** Azure AD rol atamaları veya ayarlarını yönetme
  [Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini yönetme<br>Görüntüleme, araştırmak ve güvenlik tehditleri<br>Raporları görüntüleme
  Azure Gelişmiş Tehdit Koruması | İzleme ve güvenlik şüpheli etkinliğin
  Windows Defender ATP ve EDR | Rol atama<br>Makine gruplarını yönetme<br>Uç nokta tehdit algılama ve otomatik düzeltme yapılandırma<br>Görüntüleme, araştırmak ve uyarılarını yanıtlama
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Görünümleri kullanıcı, cihaz, kayıt, yapılandırma ve uygulama bilgileri<br>Intune'da değişiklik yapamaz
  [Cloud App Security'yi](https://docs.microsoft.com/cloud-app-security/manage-admins) | Yöneticileri eklemek, ilkeleri ve ayarları, günlük yükleme ve idare eylemler gerçekleştirme Ekle
  [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Güvenlik ilkelerini görüntüleyin, güvenlik durumlarını görüntülemek, güvenlik ilkeleri, uyarıları görüntüleme ve öneriler düzenleme, uyarıları ve öneriler Kapat
  [Office 365 hizmet durumu](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleyin

<!--* **[Security operator](#security-operator)**: Users with this role can manage alerts and have global read-only access on security-related feature, including all information in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, as well as the ability to read Azure Active Directory sign-in reports and audit logs, and in Office 365 Security & Compliance Center.

  In | Can do
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | All permissions of the Security Reader role<br>View, investigate, and respond to security threats alerts
  Identity Protection Center | All permissions of the Security Reader role<br>Additionally, the ability to perform all Identity Protection Center operations except for resetting passwords
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | All permissions of the Security Reader role
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  Windows Defender ATP and EDR | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | All permissions of the Security Reader role
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | All permissions of the Security Reader role
  [Office 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health) | View the health of Office 365 services
-->
* **[Güvenlik okuyucusu](#security-reader)**: Bu role sahip kullanıcılar, özelliği, güvenlikle ilgili tüm bilgiler Microsoft 365 Güvenlik Merkezi, Azure Active Directory, kimlik koruması, Privileged Identity Management, yanı sıra Azure Active okuma yeteneği dahil olmak üzere genel salt okunur erişim vardır. Directory oturum açma raporlarını ve denetim günlüklerini ve Office 365 güvenlik ve uyumluluk Merkezi'nde. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  İçinde | Yapabilirsiniz
  --- | ---
  [Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetlerindeki güvenlikle ilgili ilkelerini görüntüle<br>Görünüm güvenlik tehditlerini ve uyarılar<br>Raporları görüntüleme
  Kimlik Koruma Merkezi | Tüm güvenlik raporları ve güvenlik özellikleri için ayarlar bilgilerini okuyun<br><ul><li>İstenmeyen postadan koruma<li>Şifreleme<li>Veri kaybı önleme<li>Kötü amaçlı yazılımdan koruma<li>Gelişmiş tehdit koruması<li>Avından<li>Mailflow kuralları
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Tüm bilgileri salt okunur erişimi, Azure AD PIM'de ortaya: İlkeleri ve Azure AD rol atamaları için raporları, güvenlik inceler ve gelecekte Azure AD rol atamanız yanı sıra senaryoları için erişim ilkesi veriler ve raporlar için okuyun.<br>**Olamaz** herhangi bir değişiklik yapmak veya Azure AD PIM için kaydolun. PIM portalda veya PowerShell aracılığıyla kullanıcı bunlar için uygun, birisi Bu roldeki ek roller (örneğin, genel yönetici veya ayrıcalıklı Rol Yöneticisi) etkinleştirebilirsiniz.
  [Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini görüntüleme<br>Güvenlik tehditlerini araştırmak ve görüntüleme<br>Raporları görüntüleme
  Windows Defender ATP ve EDR | Görüntüleyebilir ve Uyarıları araştırma
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Görünümleri kullanıcı, cihaz, kayıt, yapılandırma ve uygulama bilgileri. Intune'da değişiklik yapamaz.
  [Cloud App Security'yi](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinlere sahiptir ve Uyarıları yönetebilir
  [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Öneriler ve uyarılar, güvenlik ilkeleri, güvenlik durumlarını görüntüleyebilir ancak değişiklik yapamaz görünüm görüntüleyebilirsiniz.
  [Office 365 hizmet durumu](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleyin

* **[Hizmet desteği Yöneticisi](#service-support-administrator)**: Bu role sahip kullanıcılar açabilir destek istekleri ile Microsoft Azure ve Office 365 Hizmetleri ve hizmet panosunu ve ileti merkezini de [Azure portalında](https://portal.azure.com) ve [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com). Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "olarak hizmet desteği Yöneticisi." tanımlanır "Hizmet Yöneticisi" olarak [Azure portalında](https://portal.azure.com), [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com)ve Intune portalı.

* **[SharePoint Yöneticisi](#sharepoint-service-administrator)**: Bu role sahip kullanıcılar Microsoft SharePoint hizmet mevcut olduğunda Online içinde genel izinlere sahip olmanın yanı sıra oluşturun ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "SharePoint Hizmet Yöneticisi olarak." tanımlanır "SharePoint Yöneticisi" olarak [Azure portalında](https://portal.azure.com).

* **[İşletme için Skype / Lync Yöneticisi](#lync-service-administrator)**: Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Skype Kurumsal, içinde genel izinlere sahip, hem de Azure Active Directory'de Skype özgü kullanıcı özniteliklerini yönetin. Ayrıca, bu rol, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir ve takımlar ve Skype kurumsal iş Yönetim Merkezi erişim olanağı verir. Hesap, takımlar için lisanslanmalıdır veya ekipler PowerShell cmdlet'leri çalıştırılamaz. Daha fazla bilgi [hakkında Skype kurumsal iş yöneticisi rolüne](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve lisans bilgileri takımlar [iş ve Microsoft Teams eklenti lisansı için Skype](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. "Skype Kurumsal Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).

* **[Takımlar, yönetici](#teams-service-administrator)**: Bu roldeki kullanıcılar Microsoft Teams ve Skype üzerinden Microsoft Teams iş yükü iş Yönetim Merkezi ve ilgili PowerShell modülleri için tüm özelliklerini yönetebilir. Bu, diğer alanları arasında telefon, Mesajlaşma, toplantılar ve takımlar için ilgili tüm Yönetim Araçlar içerir. Bu rol, ayrıca oluşturun ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı verir.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "takımlar Hizmet Yöneticisi" olarak tanımlanır. "Takımlar Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Takımlar iletişimleri Yöneticisi](#teams-communications-administrator)**: Bu roldeki kullanıcılar, ilgili ses & telefon için Microsoft Teams iş yükü özelliklerini yönetebilir. Bu telefon numarası ataması, ses ve toplantı ilkeleri ve çağrı analizi araç takımı tam erişim için yönetim araçlarını içerir.

* **[Takımlar iletişimleri destek mühendisi](#teams-communications-support-engineer)**: Bu roldeki kullanıcılar, Microsoft Teams & Skype Kurumsal'a kullanarak sorun giderme araçları Microsoft Teams ve Skype kurumsal iş Yönetim Merkezi için kullanıcı çağrısı içinde iletişim sorunları giderebilirsiniz. Bu roldeki kullanıcılar, ilgili tüm katılımcılar için tam çağrı kayıt bilgileri görüntüleyebilirsiniz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[Takımlar iletişimleri destek uzmanı](#teams-communications-support-specialist)**: Bu roldeki kullanıcılar, Microsoft Teams & Skype Kurumsal'a kullanarak sorun giderme araçları Microsoft Teams ve Skype kurumsal iş Yönetim Merkezi için kullanıcı çağrısı içinde iletişim sorunları giderebilirsiniz. Bu roldeki kullanıcılar yalnızca bunlar Aranan kullanıcıyı çağrıda kullanıcı ayrıntıları da görüntüleyebilirsiniz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **Kullanıcı Yöneticisi**: Bu rolü olan kullanıcılar kullanıcıları, oluşturmanız ve bazı kısıtlamalar (aşağıya bakın) sahip kullanıcılar tüm özelliklerini yönetebilir ve parola süre sonu ilkeleri güncelleştirebilirsiniz. Ayrıca, bu role sahip kullanıcılar oluşturun ve tüm gruplarını yönetin. Bu rolü, oluşturma ve kullanıcı görünümleri yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı da içerir.

  | | |
  | --- | --- |
  |Genel izinler|<p>Kullanıcılar ve gruplar oluşturma</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Office destek biletlerini yönetebilir<p>Parola süre sonu ilkeleri güncelleştirin|
  |<p>Tüm yöneticilerin tüm kullanıcılar dahil</p>|<p>Lisanslarını yönetme</p><p>Kullanıcı asıl adı dışındaki tüm kullanıcı özelliklerini yönetme</p>
  |Yönetici olmayan ya da aşağıdakilerden birini sınırlı yönetici rolleri yalnızca kullanıcılar üzerinde:<ul><li>Dizin Okuyucuları<li>Konuk Davet Eden<li>Yardım Masası Yöneticisi<li>İleti Merkezi Okuyucusu<li>Rapor Okuyucusu<li>Kullanıcı Yöneticisi|<p>Silme ve geri yükleme</p><p>Devre dışı bırakma ve etkinleştirme</p><p>Geçersiz belirteç yenileme</p><p>Kullanıcı asıl adı dahil olmak üzere tüm kullanıcı özelliklerini yönetme</p><p>Parola sıfırlama</p><p>Güncelleştirme (FIDO) cihaz anahtarları</p>
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolaları değiştirebilirsiniz. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve kullanıcı yöneticileri izni başka bir yerde değil. Kullanıcı Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

## <a name="role-permissions"></a>Rol izinleri
Aşağıdaki tablolarda her rol için belirtilen Azure Active Directory'de özel izinler açıklanmaktadır. Bazı roller, Azure Active Directory dışında Microsoft hizmetlerindeki ek izinlere sahip olabilir.

### <a name="application-administrator"></a>Uygulama Yöneticisi
Tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

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
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals özelliğini silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="application-developer"></a>Uygulama Geliştiricisi
Uygulama kayıtları 'kullanıcılar uygulamaları kaydedebilir' bağımsız oluşturabilirsiniz ayarı.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Azure Active Directory'de uygulamalar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Azure Active Directory'de appRoleAssignments oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory'de oAuth2PermissionGrants oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Azure Active Directory'de servicePrincipals oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |

### <a name="authentication-administrator"></a>Kimlik Doğrulaması Yöneticisi
Görüntüleyebilir, ayarlayabilir ve herhangi bir yönetici olmayan kullanıcı kimlik doğrulama yöntemi bilgileri sıfırlama izni.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/users/strongAuthentication/update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="billing-administrator"></a>Faturalama Yöneticisi
Ödeme bilgilerini güncelleştirme gibi sık kullanılan faturalandırma görevlerini gerçekleştirebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Organization/Basic/Update | Azure Active Directory'de kuruluş üzerindeki temel özellikleri güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics Yöneticisi
Erişebilir ve Masaüstü Yönetimi Araçları ve Hizmetleri Intune dahil olmak üzere yönetebilirsiniz.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics'in tüm yönlerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-application-administrator"></a>Bulut Uygulaması Yöneticisi
Uygulama Ara Sunucusu hariç tüm uygulama kayıtlarını ve kurumsal uygulamaları oluşturabilir ve bunların tüm özelliklerini yönetebilir.

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
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration ilkelerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals özelliğini silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="cloud-device-administrator"></a>Bulut Cihazı Yöneticisi
Azure AD'de cihazları yönetmek için tam erişim.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory'de cihazları silin. |
| Microsoft.aad.Directory/Devices/disable | Azure Active Directory'de cihazları devre dışı bırakın. |
| Microsoft.aad.Directory/Devices/Enable | Azure Active Directory'de cihazları etkinleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="company-administrator"></a>Şirket Yöneticisi
Azure AD'nin ve Azure AD kimliklerini kullanan Microsoft hizmetlerinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.aad.cloudAppSecurity içindeki standart özellikleri okuyup güncelleştirin. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | administrativeUnits oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/applications/allProperties/allTasks | Uygulamalar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | appRoleAssignments oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
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
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | subscribedSkus oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directory/users/allProperties/allTasks | Kullanıcılar oluşturup silin ve Azure Active Directory'de tüm özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |
| microsoft.aad.identityProtection/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.aad.identityProtection üzerindeki standart özellikleri okuyun ve güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.azure.advancedThreatProtection/allEntities/read | microsoft.azure.advancedThreatProtection içindeki tüm kaynakları okuyun. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalamasının tüm özelliklerini yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm özelliklerini yönetin. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisinin tüm özelliklerini yönetin |
| Microsoft.Office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics'in tüm yönlerini yönetin. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 Müşteri Kasasının tüm özelliklerini yönetin. |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter içindeki tüm iletileri okuyun. |
| Microsoft.Office365.messageCenter/securityMessages/Read | microsoft.office365.messageCenter içindeki securityMessages iletilerini okuyun. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Office 365 Koruma Merkezi'nin tüm özelliklerini yönetin. |
| Microsoft.Office365.securityComplianceCenter/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.securityComplianceCenter içindeki standart özellikleri okuyup güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm özelliklerini yönetin. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | microsoft.windows.defenderAdvancedThreatProtection içindeki tüm kaynakları okuyun. |

### <a name="compliance-administrator"></a>Uyumluluk Yöneticisi
Azure AD ve Office 365'te uyumluluk yapılandırmasını ve raporları okuyup yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 Uyumluluk Yöneticisinin tüm özelliklerini yönetin |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi
Koşullu erişim özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Azure Active Directory'de policies.conditionalAccess ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/create | Azure Active Directory'de ilkeler oluşturun. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Azure Active Directory'de ilkeleri silin. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Azure Active Directory'de policies.conditionalAccess ilkelerini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory'de policies.conditionalAccess ilkelerini okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | Azure Active Directory'de policies.conditionalAccess ilkelerini güncelleştirin. |

### <a name="crm-service-administrator"></a>CRM Hizmet Yöneticisi
Dynamics 365 ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365'in tüm özelliklerini yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="customer-lockbox-access-approver"></a>Müşteri Kasası Erişimi Onaylayıcısı
Müşterinin kuruluş verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 Müşteri Kasasının tüm özelliklerini yönetin. |

### <a name="device-administrators"></a>Cihaz Yöneticileri
Bu role atanan kullanıcılar, Azure AD'ye katılmış cihazlarda yerel Yöneticiler grubuna eklenir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory'de groupSettings üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory'de groupSettingTemplates üzerindeki temel özellikleri okuyun. |

### <a name="directory-readers"></a>Dizin Okuyucuları
Temel dizin bilgileri okuyabilir. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Azure Active Directory'de administrativeUnits üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/administrativeUnits/members/read | Azure Active Directory'de administrativeUnits.members özelliğini okuyun. |
| Microsoft.aad.Directory/Applications/Basic/Read | Azure Active Directory'de uygulamalar üzerindeki temel özellikleri okuyun. |
| Microsoft.aad.Directory/Applications/Owners/Read | Azure Active Directory'de applications.owners özelliğini okuyun. |
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
| microsoft.aad.directory/roleAssignments/basic/read | Azure Active Directory'de roleAssignments üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/roleDefinitions/basic/read | Azure Active Directory'de roleDefinitions üzerindeki temel özellikleri okuyun. |
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
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliğini okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

### <a name="directory-synchronization-accounts"></a>Dizin eşitlemesi hesapları
Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

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
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory'de policies.tenantDefault özelliğini güncelleştirin.3 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals üzerindeki temel özellikleri güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliğini okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'te tüm eylemleri gerçekleştirin. |

### <a name="directory-writers"></a>Dizin Yazıcıları
Okuma ve yazma temel dizin bilgileri kullanabilirsiniz. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

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
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de groups.unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 Gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 Grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 Gruplarını silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 Gruplarına üyeliği güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="guest-inviter"></a>Konuk Davet Eden
'Üyeler konuk davet edebilir' ayarından bağımsız olarak konuk kullanıcı davet edebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliğini okuyun. |
| Microsoft.aad.Directory/Users/Basic/Read | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliğini okuyun. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory'de konuk kullanıcıları davet edin. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de users.manager özelliğini okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliğini okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliğini okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliğini okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliğini okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliğini okuyun. |

### <a name="helpdesk-administrator"></a>Yardım Masası Yöneticisi
Yönetici olmayan kullanıcıların ve Yardım Masası Yöneticilerinin parolalarını sıfırlayabilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="information-protection-administrator"></a>Information Protection Yöneticisi
Azure Information Protection ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection'ın tüm özelliklerini yönetin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="intune-service-administrator"></a>Intune Hizmet Yöneticisi
Intune ürününün tüm özelliklerini yönetebilir.

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
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
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
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| microsoft.intune/allEntities/allTasks | Intune'un tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |

### <a name="license-administrator"></a>Lisans Yöneticisi
Kullanıcılar ve gruplar ürün lisanslarını yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| microsoft.aad.directory/users/usageLocation/update | Azure Active Directory'de users.usageLocation özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="lync-service-administrator"></a>Lync Hizmet Yöneticisi
Skype Kurumsal ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Skype Kurumsal Çevrimiçi Sürüm'ün tüm özelliklerini yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="message-center-reader"></a>İleti Merkezi Okuyucusu
Yalnızca Office 365 İleti Merkezi'nde kuruluşuna yönelik iletileri ve güncelleştirmeleri okuyabilir. 

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter içindeki tüm iletileri okuyun. |

### <a name="partner-tier1-support"></a>Partner Tier1 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

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
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="partner-tier2-support"></a>Partner Tier2 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

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
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'de kullanıcıların lisanslarını yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory'de kullanıcılar üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de users.manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory'de silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="power-bi-service-administrator"></a>Power BI Hizmet Yöneticisi
Power BI ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI'ın tüm özelliklerini yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="privileged-authentication-administrator"></a>Ayrıcalıklı Kimlik Doğrulaması Yöneticisi
Tüm kullanıcılara (yönetici olan veya olmayan) ait kimlik doğrulaması yöntemi bilgilerini görüntüleme, ayarlama ve sıfırlama iznine sahiptir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory'de tüm kullanıcı yenileme belirteçlerini geçersiz kılın. |
| microsoft.aad.directory/users/strongAuthentication/update | MFA kimlik bilgileri gibi güçlü kimlik doğrulama özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="privileged-role-administrator"></a>Ayrıcalıklı Rol Yöneticisi
Azure AD'de rol atamalarını ve ayrıcalıklı Kimlik Yönetimi'nin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Azure Active Directory'de directoryRoles özelliğini güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.aad.privilegedIdentityManagement üzerindeki standart özellikleri okuyun ve güncelleştirin. |

### <a name="reports-reader"></a>Rapor Okuyucusu
Oturum açma ve denetim raporlarını okuyabilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Güvenlik bilgilerini ve raporları okuma ve Azure AD'de yapılandırmasını yönetmek ve Office 365.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de applications.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| Microsoft.aad.Directory/Policies/Basic/Update | Azure Active Directory'de ilkeler üzerindeki temel özellikleri güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeler oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory'de ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory'de policies.tenantDefault özelliğini güncelleştirin.3 |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection içindeki tüm kaynakları okuyun. |
| microsoft.aad.identityProtection/allEntities/update | microsoft.aad.identityProtection içindeki tüm kaynakları güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| microsoft.office365.protectionCenter/allEntities/update | microsoft.office365.protectionCenter içindeki tüm kaynakları güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="security-reader"></a>Güvenlik Okuyucusu
Azure AD ve Office 365'te güvenlik bilgilerini ve raporları okuyabilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de auditLogs üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliğini okuyun. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports üzerinde tüm özellikleri (ayrıcalıklı özellikler dahil) okuyabilir.  |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection içindeki tüm kaynakları okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement içindeki tüm kaynakları okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 Koruma Merkezi'nin tüm özellikleriyle ilgili bilgi edinin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="service-support-administrator"></a>Hizmet Desteği Yöneticisi
Hizmet durumu bilgilerini okuyabilir ve destek biletlerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="sharepoint-service-administrator"></a>SharePoint Hizmet Yöneticisi
SharePoint hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de groups.unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 Gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 Grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 Gruplarını silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 Gruplarına üyeliği güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Tüm kaynakları oluşturup silin ve microsoft.office365.sharepoint üzerinde standart özellikleri okuyun ve güncelleştirin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

### <a name="teams-communications-administrator"></a>Teams İletişim Yöneticisi
Microsoft Teams hizmeti içinde arama ve toplantı özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="teams-communications-support-engineer"></a>Teams İletişim Destek Mühendisi
Teams içinde gelişmiş araçları kullanarak iletişim sorunlarını giderebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-communications-support-specialist"></a>Teams İletişim Destek Uzmanı
Teams içinde temel araçları kullanarak iletişim sorunlarını giderebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |

### <a name="teams-service-administrator"></a>Teams Hizmet Yöneticisi
Microsoft Teams hizmetini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliğini okuyun. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de groups.unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 Gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 Grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 Gruplarını silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 Gruplarına üyeliği güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 Gruplarının sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |
| Microsoft.Office365.usageReports/allEntities/Read | Office 365 kullanım raporlarını okuyun. |

### <a name="user-administrator"></a>Kullanıcı Yöneticisi
Sınırlı yöneticilerin parolalarını sıfırlama dahil olmak üzere kullanıcılar ve grupların tüm özelliklerini yönetebilir.

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
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Hizmet Durumu'nu okuyun ve yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure desteği biletleri oluşturun ve yönetin. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | microsoft.office365.webPortal içindeki tüm kaynaklarda temel özellikleri okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Office 365 Hizmet Durumu'nu okuyun ve yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Office 365 destek biletleri oluşturun ve yönetin. |

## <a name="role-template-ids"></a>Rolü şablonu kimlikleri

Rol şablon kimlikleri belirtilmeli çoğunlukla Graph API'si veya PowerShell kullanıcılar tarafından kullanılır.

Graf displayName | Azure portalında görünen adı | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Uygulama Yöneticisi | Uygulama yöneticisi | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Uygulama Geliştiricisi | Uygulama geliştiricisi | CF1C38E5-3621-4004-A7CB-879624DCED7C
Kimlik Doğrulaması Yöneticisi | Kimlik doğrulaması yöneticisi | c4e39bd9-1100-46d3-8c65-fb160da0071f
Faturalama Yöneticisi | Faturalama yöneticisi | b0f54661-2d74-4c50-afa3-1ec803f12efe
Desktop Analytics Yöneticisi | Desktop Analytics Yöneticisi | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Bulut Uygulaması Yöneticisi | Bulut uygulaması yöneticisi | 158c047a-c907-4556-b7ef-446551a6b5f7
Bulut Cihazı Yöneticisi | Bulut cihazı yöneticisi | 7698a772-787b-4ac8-901f-60d6b08affd2
Şirket Yöneticisi | Genel yönetici | 62e90394-69f5-4237-9190-012177145e10
Uyumluluk Yöneticisi | Uyumluluk yöneticisi | 17315797-102d-40b4-93e0-432062caca18
Koşullu Erişim Yöneticisi | Koşullu Erişim yöneticisi | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM Hizmet Yöneticisi | Dynamics 365 yöneticisi | 44367163-eba1-44c3-98af-f5787879f96a
Müşteri Kasası Erişimi Onaylayıcısı | Müşteri kasası erişim onaylayıcı | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Cihaz Yöneticileri | Cihaz yöneticileri | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Cihaz birleştirme | Cihaz birleştirme | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Cihaz Yöneticileri | Cihaz yöneticileri | 2b499bcd-da44-4968-8aec-78e1674fa64d
Aygıt kullanıcıları | Aygıt kullanıcıları | d405c6df-0af8-4e3b-95e4-4d06e542189e
Dizin Okuyucuları | Dizin okuyucuları | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Dizin eşitlemesi hesapları | Dizin eşitleme hesapları | d29b2b05-8046-44ba-8758-1e26182fcf32
Dizin Yazıcıları | Dizin yazıcıları | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Hizmeti Yöneticisi | Exchange yöneticisi | 29232cdf-9323-42fd-ade2-1d097af3e4de
Konuk Davet Eden | Konuk davet eden | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Yardım Masası Yöneticisi | Parola yöneticisi | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Information Protection Yöneticisi | Information Protection yöneticisi | 7495fdc4-34c4-4d15-a289-98788ce399fd
Intune Hizmet Yöneticisi | Intune yöneticisi | 3a2c62db-5318-420d-8d74-23affee5d9d5
Lisans Yöneticisi | Lisans yöneticisi | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync Hizmet Yöneticisi | Skype Kurumsal yöneticisi | 75941009-915a-4869-abe7-691bff18279e
İleti Merkezi Okuyucusu | İleti merkezi okuyucusu | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Partner Tier1 Desteği | Partner tier1 desteği | 4ba39ca4-527c-499a-b93d-d9b492c50246
Partner Tier2 Desteği | Partner tier2 desteği | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Power BI Hizmet Yöneticisi | Power BI yöneticisi | a9ea8996-122f-4c74-9520-8edcd192826c
Ayrıcalıklı Kimlik Doğrulaması Yöneticisi | Ayrıcalıklı kimlik doğrulaması yöneticisi | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Ayrıcalıklı Rol Yöneticisi | Ayrıcalıklı rol yöneticisi | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapor Okuyucusu | Rapor okuyucusu | 4a5d8f65-41da-4de4-8968-e035b65339cf
Güvenlik Yöneticisi | Güvenlik yöneticisi | 194ae4cb-b126-40b2-bd5b-6091b380977d
Güvenlik Okuyucusu | Güvenlik okuyucusu | 5d6b6bb7-de71-4623-b4af-96380a352509
Hizmet Desteği Yöneticisi | Hizmet yöneticisi | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint Hizmet Yöneticisi | SharePoint Yöneticisi | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Teams İletişim Yöneticisi | Teams İletişim Yöneticisi | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Teams İletişim Destek Mühendisi | Teams İletişim Destek Mühendisi | f70938a0-fc10-4177-9e90-2178f8765737
Teams İletişim Destek Uzmanı | Teams İletişim Destek Uzmanı | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Teams Hizmet Yöneticisi | Teams Hizmet Yöneticisi | 69091246-20e8-4a56-aa4d-066075b2a7a8
Kullanıcı | Kullanıcı | a0b1b346-4d3e-4e8b-98f8-753987be4970
Kullanıcı Hesabı Yöneticisi | Kullanıcı yöneticisi | fe930be7-5e62-47db-91af-98c3a49a38b1
Cihazla Çalışma Alanına Katılma | Cihazla çalışma alanına katılma | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar, kullanım dışı bırakıldı ve gelecekte Azure AD'deki kaldırıldı.

* Geçici Lisans Yöneticisi
* Cihaz birleştirme
* Cihaz Yöneticileri
* Aygıt kullanıcıları
* E-postayla Doğrulanan Kullanıcı Oluşturucu
* Posta Kutusu Yöneticisi
* Cihazla Çalışma Alanına Katılma

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliği Yöneticisi olarak kullanıcı atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
