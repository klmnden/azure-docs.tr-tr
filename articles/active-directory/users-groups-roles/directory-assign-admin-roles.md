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
ms.date: 05/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5044567396d832d3c3b2b46e3c3e90e053834595
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417887"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory'de Yönetici rolü izinleri

Azure Active Directory (Azure AD) kullanarak, daha az ayrıcalıklı rolleri kimlik görevleri yönetmek için sınırlı yöneticileri atayabilirsiniz. Yöneticiler, ekleme veya kullanıcıları için değiştirme, yönetici rolleri atama, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve etki alanı adlarını yönetme gibi amaçlarla atanabilir. Varsayılan kullanıcı izinleri yalnızca kullanıcı ayarları, Azure AD'de değiştirilebilir.

Genel yönetici, tüm yönetim özelliklerine erişebilir. Varsayılan olarak, bir Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca genel Yöneticiler ve ayrıcalıklı rol yöneticileri, yönetici rollerini devredebilirsiniz. Risk iş riskinizi azaltmak için şirketinizde yalnızca birkaç kişinin bu role atamak öneririz.


## <a name="assign-or-remove-administrator-roles"></a>Atamayı veya kaldırmayı yönetici rolleri

Azure Active Directory'de bir kullanıcıya yönetici rollerini atama hakkında bilgi edinmek için [Azure Active Directory'de yönetici rolleri görüntüleyin ve Ata](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Kullanılabilir roller

Aşağıdaki Yönetici rollerini kullanılabilir:

* **[Uygulama Yöneticisi](#application-administrator)** : Bu roldeki kullanıcılar oluşturabilir ve kurumsal uygulamaları, uygulama kayıtları ve uygulama proxy'si ayarları tüm özelliklerini yönetebilir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Uygulama Yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Uygulama geliştiricisi](#application-developer)** : Bu roldeki kullanıcılar, uygulama kayıtları oluşturabilir, "Kullanıcılar uygulamaları kaydedebilir" ayarı Hayır olarak ayarlayın Bu rol, ayrıca birinin kendi adınıza onaylamasına izin verir, "Kullanıcı izni verebilir uygulamalara kendileri adına şirket verilerine erişme" ayarı Hayır olarak ayarlayın Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenir.

* **[Kimlik doğrulaması yönetici](#authentication-administrator)** : Bu role sahip kullanıcılar, ayarlayın ya da parolası olmayan kimlik bilgilerini sıfırlayın. Kimlik doğrulaması yöneticileri, kullanıcıların mevcut olmayan bir parola kimlik bilgisi karşı (örneğin, MFA veya FIDO) yeniden kaydedin ve iptal etmesine gerektirebilirsiniz **cihazda MFA unutmayın**, hangi ister MFA için bir sonraki oturum açma olan kullanıcıların Yönetici olmayanlar veya yalnızca aşağıdaki rolleri atanmış:
  * Kimlik doğrulama Yöneticisi
  * Dizin okuyucular
  * Konuk davet eden
  * İleti Merkezi okuyucusu
  * Rapor okuyucu

  Kimlik doğrulaması yönetici rolünün şu anda genel Önizleme aşamasındadır. Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için kimlik bilgilerini değiştirebilirsiniz. Bir kullanıcının kimlik bilgilerini değiştirme, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:

  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve kimlik yöneticileri için verilen başka bir yerde değil. Kimlik doğrulaması yönetici uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden ayrıcalıklı bir uygulama kimliği, uygulama için kimlik bilgilerini güncelleştirerek varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

* **[Azure Information Protection Yöneticisi](#azure-information-protection-administrator)** : Bu role sahip kullanıcılar Azure Information Protection hizmetinde tüm izinlere sahip. Bu rol, Azure Information Protection ilkesi için etiketleri yapılandırma, koruma şablonlarını yönetme ve koruma etkinleştirme sağlar. Bu rol, kimlik koruma Merkezi, Privileged Identity Management, Office 365 hizmet durumunu izleme, veya Office 365 güvenlik ve uyumluluk Merkezi'nde herhangi bir izni tanımaz.

* **[B2C kullanıcı Akışı Yöneticisi](#b2c-user-flow-administrator)** : Bu role sahip kullanıcılar oluşturabilir ve B2C kullanıcı akışları ("yerleşik" ilkeleri olarak da bilinir) Azure portalında yönetebilirsiniz. Oluşturma veya düzenleme kullanıcı akışları, bu kullanıcılar kullanıcı deneyimini html/CSS/javascript içeriğini değiştirme, kullanıcı akışı başına MFA Gereksinimler değiştikçe, belirtecinde talep değiştirebilir ve kiracıdaki tüm ilkeler için oturumu ayarlarını. Öte yandan, bu rolü kullanıcı verilerini gözden geçirmek için bu özelliği içermez, veya Kiracı şemasında bulunan öznitelikler değişiklik. Kimlik deneyimi çerçevesi için değişiklikleri (özel olarak da bilinir) ilkeleri, ayrıca bu rol kapsamı dışındadır.

* **[B2C kullanıcı akışı özniteliği Yöneticisi](#b2c-user-flow-attribute-administrator)** : Bu role sahip kullanıcılar, ekleyebilir veya özel öznitelikler kiracıdaki tüm kullanıcı akışları kullanımına silebilirsiniz. Bu nedenle, bu role sahip kullanıcılar değiştirebilir veya yeni öğeler için son kullanıcı şeması ekleyin ve tüm kullanıcı akışları davranışını etkiler ve dolaylı olarak hangi verilerin son kullanıcılarının sorular ve sonuçta uygulamaları talep olarak gönderilen neden. Bu rol, kullanıcı akışları düzenleyemezsiniz.

* **[B2C IEF anahtar kümesi yönetici](#b2c-ief-keyset-administrator)** :    Kullanıcı oluşturma ve ilke anahtarları yönetmek ve belirteç şifreleme için gizli dizileri belirteci imzalar ve şifreleme/şifre çözme talep. Varolan anahtar kapsayıcıları için yeni anahtarları ekleyerek bu sınırlı yönetici yapabilirsiniz rollover gizli dizileri mevcut uygulamaları etkilemeden gerektiğinde. Bu kullanıcı, gizli bilgileri ve sona erme tarihlerinin, oluşturulduktan sonra bile tam içeriğini görebilir.
    
  <b>Önemli:</b> bu hassas bir roldür. Anahtar kümesi Yönetici rolü dikkatli bir şekilde denetlenen ve üretim öncesi ve üretim sırasında dikkatli atanan gerekir.

* **[B2C IEF İlke Yöneticisi](#b2c-ief-policy-administrator)** : Bu roldeki kullanıcılar oluşturma, okuma, güncelleştirme ve tüm özel Azure AD B2C ilkeleri silin ve bu nedenle ilgili Azure AD B2C kiracısında kimlik deneyimi çerçevesi üzerinde tam denetime sahip seçeneğine sahipsiniz. İlkeleri düzenleyerek, bu kullanıcı dış kimlik sağlayıcısı ile doğrudan Federasyon kurmak, directory şemasını değiştirme, kullanıcıya yönelik tüm içerik (HTML, CSS, JavaScript) değiştirmek için bir kimlik doğrulamasını tamamlamak için yeni kullanıcı oluşturma, Gönder Gereksinimler değiştikçe kullanıcı verilerini dış sistemlerle geçişleri tam ve parolalar ve telefon numaraları gibi hassas alanları dahil olmak üzere tüm kullanıcı bilgilerini düzenleyin. Buna karşılık, bu rolü şifreleme anahtarlarını değiştirmek veya kiracıdaki Federasyon için kullanılan gizli dizileri düzenleyin.

  <b>Önemli:</b> B2 IEF İlke Yöneticisi üretimde kiracılar için çok kısıtlı bir kapsamla atanan son derece hassas bir roldür. Etkinlikler bu kullanıcı tarafından yakından, üretimde kiracılar için özellikle denetlenmesi.

* **[Faturalama Yöneticisi](#billing-administrator)** : Satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

* **[Bulut uygulaması Yöneticisi](#cloud-application-administrator)** : Bu roldeki kullanıcılar, uygulama proxy'si yönetme olanağı hariç uygulama yöneticisi rolü aynı izinlere sahip. Bu rol oluşturmak ve kurumsal uygulamaları ve uygulama kayıtlarını tüm yönlerini yönetmek için yeteneği verir. Bu rol, ayrıca temsilci izinleri ve Microsoft Graph ve Azure AD Graph hariç olmak üzere uygulama izinleri onayını olanağı verir. Bu role atanan kullanıcılar, yeni uygulama kaydı ve kurumsal uygulamaları oluştururken sahipleri eklenmez.

  <b>Önemli</b>: Bu rol, uygulama kimlik bilgilerini yönetme olanağı verir. Bu role atanan kullanıcılar, bir uygulamaya kimlik Ekle ve uygulamanın kimliğine bürünülecek bu kimlik bilgilerini kullanın. Ardından uygulamanın kimlik erişimi Azure Active Directory, kullanıcı ya da diğer nesneleri oluştur veya güncelleştir olanağı gibi verilmişse, bu role atanmış bir kullanıcı uygulama kimliğine bürünülürken bu eylemleri gerçekleştirebilir. Uygulamanın kimliğine bürünülecek bu özelliği, kullanıcının rol atamalarının Azure AD'de neler yapabileceğinizi ayrıcalık olabilir. Bulut uygulaması yöneticisi rolü için kullanıcı atama bunları bir uygulamanın kimliğini kimliğine bürünme özelliğini verdiğini anlamak önemlidir.

* **[Bulut cihaz Yöneticisi](#cloud-device-administrator)** : Bu roldeki kullanıcılar etkinleştirebilir, devre dışı bırakma ve Azure AD'de cihazları silin ve Windows 10 BitLocker anahtarları Azure Portalı'nda (varsa) okuyun. Rol, cihazdaki diğer özelliklerini yönetmek için izinleri tanımaz.

* **[Uyumluluk Yöneticisi](#compliance-administrator)** : Bu role sahip kullanıcılar, Microsoft 365 Uyumluluk Merkezi, Microsoft 365 Yönetim Merkezi, Azure ve Office 365 güvenlik ve uyumluluk Merkezi'nde özelliklerini ilgili yönetmek için izinlere sahip. Kullanıcılar ayrıca iş Yönetim Merkezi için Exchange yönetici merkezini ve takımlar ve Skype Kurumsal içinde tüm özelliklerini yönetebilir ve Azure ve Microsoft 365 için destek bileti oluşturun. Daha fazla bilgi edinilebilir [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

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
* **[Koşullu Erişim Yöneticisi](#conditional-access-administrator)** : Bu role sahip olan kullanıcılar Azure Active Directory koşullu erişim ayarlarını yönetmek için sahipsiniz.
  > [!NOTE]
  > Azure'da Exchange ActiveSync koşullu erişim ilkesi dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **[Müşteri kasası erişim onaylayıcı](#customer-lockbox-access-approver)** : Yöneten [müşteri kasa istekleri](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) kuruluşunuzdaki. Bunlar müşteri kasa istekleri için e-posta bildirimleri almak ve onaylayabilir ve Microsoft 365 Yönetim merkezinden istekleri reddetme. Bunlar ayrıca müşteri kasa özelliğini açıp kapatabilirsiniz. Yalnızca genel Yöneticiler, bu role atanan kişi parolalarını sıfırlayabilir.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Cihaz yöneticileri](#device-administrators)** : Bu rol ataması yalnızca ek yerel yönetici olarak kullanılabilir [cihaz ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Bu role sahip olan kullanıcılar Azure Active Directory'e katılan tüm Windows 10 cihazlarda Yerel Makine Yöneticisi olur. Azure Active Directory'de cihaz nesnelerini yönetme olanağına sahip değildir. 

* **[Dizin okuyucular](#directory-readers)** : Desteklemeyen yalnızca eski uygulamalar atanmış bir rol budur [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Bu, kullanıcılara atamayın.

* **[Dizin eşitlemesi hesapları](#directory-synchronization-accounts)** : Kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil hedeflenen veya herhangi bir kullanım için desteklenir.

* **[Dizin yazıcıları](#directory-writers)** : Bu desteklemeyen uygulamalar için atanacak olan, eski bir roldür [onay Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Herhangi bir kullanıcıya atanmamalıdır.

* **[Dynamics 365 Yönetici / CRM yönetici](#crm-service-administrator)** : Bu role sahip kullanıcılar Microsoft Dynamics 365 hizmet mevcut olduğunda Online içinde genel izinlere sahip olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [kiracınızı yönetmek için Hizmet Yöneticisi rolü kullanmak](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Hizmet Yöneticisi olarak Dynamics 365" tanımlanır. "Dynamics 365 Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Exchange Yöneticisi](#exchange-service-administrator)** : Bu role sahip kullanıcılar, hizmet olduğunda Microsoft Exchange Online içinde genel izinlere sahip. Ayrıca, oluşturmak ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumunu izlemek için özelliğine sahiptir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Exchange Hizmet Yöneticisi" olarak tanımlanır. "Exchange Yöneticisi" olarak [Azure portalında](https://portal.azure.com). "Exchange Online Yönetici" olarak [Exchange yönetici merkezini](https://go.microsoft.com/fwlink/p/?LinkID=529144). 

* **[Dış kimlik sağlayıcısı yönetici](#external-identity-provider-administrator)** : Bu yönetici, Azure Active Directory kiracıları ve Dış kimlik sağlayıcısı arasında federasyon yönetir. Bu rol ile kullanıcılar yeni kimlik sağlayıcıları eklemek ve tüm kullanılabilir ayarları (örneğin kimlik doğrulaması yolu, anahtar kapsayıcıları atanan hizmet kimliği) yapılandırın. Bu kullanıcı, Kiracı kimlik doğrulama dış kimlik sağlayıcılardan gelen güven etkinleştirebilirsiniz. Son kullanıcı deneyimi elde edilen etkisi Kiracı türüne bağlıdır:
  * Çalışanlar ve iş ortakları için Azure Active Directory kiracıları: Federasyon (örneğin ile Gmail) eklenmesi, henüz alınma tarihinden itibaren tüm Konuk davet hemen etkiler. Bkz: [B2B Konuk kullanıcılar için bir kimlik sağlayıcısı olarak ekleyerek Google](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).
  * Azure Active Directory B2C kiracıları: Kimlik sağlayıcısı (diğer adıyla yerleşik ilke) kullanıcı akışı bir seçenek olarak eklenene kadar (örneğin Facebook veya başka bir Azure Active Directory ile) bir Federasyon eklenmesini son kullanıcı akışları hemen etkilemez. Bkz: [bir Microsoft hesabı kimlik sağlayıcısı olarak yapılandırma](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) örneği. Kullanıcı akışları değiştirmek için "B2C kullanıcı akışı Yönetici" sınırlı rolü gereklidir.

* **[Genel yönetici / şirket Yöneticisi](#company-administrator)** : Bu role sahip kullanıcılar gibi Microsoft 365 Güvenlik Merkezi, Azure Active Directory kimlikleri kullanmak hizmetlerinin yanı sıra Azure Active Directory, tüm yönetim özelliklerine erişim sahibi Microsoft 365 Uyumluluk Merkezi, Exchange Online, SharePoint Online ve Skype Kurumsal çevrimiçi. Azure Active Directory kiracısı için kaydolan kişi genel yönetici olur. Yalnızca genel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel Yöneticiler, herhangi bir kullanıcı ve diğer tüm yöneticilerin parolasını sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Şirket Yöneticisi" tanımlanır. "Genel yönetici" olarak [Azure portalında](https://portal.azure.com).
  >
  >

* **[Konuk davet eden](#guest-inviter)** : Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetlerini yönetebilir, **üyelerini davet edebildiğiniz** kullanıcı ayarı Hayır olarak ayarlayın B2B işbirliği hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Tüm diğer izinleri içermez.

* **[Intune yönetici](#intune-service-administrator)** : Bu role sahip kullanıcılar, hizmet olduğunda Microsoft Intune Online içinde genel izinlere sahip. Ayrıca, bu rol, ilke ilişkilendirmek yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme olanağı içerir. Daha fazla bilgiye [Intune rol tabanlı yönetim denetimi (RBAC)](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Intune Hizmet Yöneticisi" olarak tanımlanır. "Intune Yönetici" olarak [Azure portalında](https://portal.azure.com).
  
 * **[Kaizala yönetici](#kaizala-administrator)** : Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Kaizala, ayarlarında olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izlemek için yönetmek için genel izinlere sahip.
Ayrıca, kullanıcının benimseme & Kaizala kullanımını kuruluş üyeleri Kaizala eylemleri kullanarak üretilen iş rapor ile ilgili raporları erişebilirsiniz. 

* **[Lisans Yöneticisi](#license-administrator)** : Bu roldeki kullanıcılar ekleyebilir, kaldırma ve güncelleştirme lisans atamalarında kullanıcıları, grupları (Grup tabanlı lisanslama kullanarak) ve kullanım konumu kullanıcıları yönetme. Rol, satın alma veya Aboneliklerini yönetmek, oluşturma veya grupları yönetme veya oluşturma veya ötesinde kullanım konumu yönetme olanağı tanımaz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[İleti Merkezi gizlilik okuyucu](#message-center-privacy-reader)** : Bu roldeki kullanıcılar, veriler gizlilik iletileri de dahil ileti Merkezi'nde tüm bildirimleri izleyebilirsiniz. İleti Merkezi gizlilik okuyucular veri gizliliği ilgili olanlar dahil olmak üzere e-posta bildirimleri alın ve ileti merkezi tercihleri kullanarak kaldırabilirsiniz. Yalnızca genel Yöneticiler ve ileti merkezi gizlilik okuyucu veri gizlilik iletileri okuyabilir. Ayrıca, bu rol grupları, etki alanları ve abonelikleri görüntüleme olanağı içerir. Bu rol, görüntülemek, oluşturmak veya hizmet isteklerini yönetme izni yok.

* **[İleti Merkezi okuyucu](#message-center-reader)** : Bu roldeki kullanıcılar, bildirimler ve danışmanlık sistem güncelleştirmeleri izleyebilirsiniz [Office 365 ileti Merkezi](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) kuruluşlarında Exchange, Intune ve Microsoft Teams gibi yapılandırılmış hizmetleri. İleti Merkezi okuyucular Haftalık e-posta özetler gönderilerin, güncelleştirmeleri almak ve Office 365 ileti merkezi gönderileri paylaşabilirsiniz. Azure AD'de bu role atanan kullanıcılar yalnızca salt okunur kullanıcılar ve gruplar gibi Azure AD Hizmetleri erişebilir. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[İş ortağı Tier1 desteği](#partner-tier1-support)** : Kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[İş ortağı Tier2 desteği](#partner-tier2-support)** : Kullanmayın. Bu rolü kullanım dışıdır ve gelecekte Azure AD'deki kaldırıldı. Bu rol, az sayıda Microsoft satışıyla iş ortakları tarafından kullanılmak üzere tasarlanmış ve genel kullanıma yönelik değildir.

* **[(Parola) Yardım Masası Yöneticisi](#helpdesk-administrator)** : Bu role sahip olan kullanıcılar parolaları değiştirme, yenileme belirteçleri geçersiz, hizmet isteklerini yönetebilir ve hizmet durumunu izleyebilir. Bir yenileme belirteci geçersiz kılmalarını, kullanıcı yeniden oturum açmak için zorlar. Yardım Masası yöneticileri, parolaları sıfırlama ve yenileme belirteçleri olmayanların ya da yalnızca aşağıdaki roller atanan diğer kullanıcıların geçersiz kıl:
  * Dizin okuyucular
  * Konuk davet eden
  * Yardım Masası Yöneticisi
  * İleti Merkezi okuyucusu
  * Rapor okuyucu
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolaları değiştirebilirsiniz. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve Yardım Masası yöneticileri izni başka bir yerde değil. Yardım Masası Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.


  > [!NOTE]
  > Kullanıcılar alt kümeleri üzerinde yönetim izinleri için temsilci seçme ve bir kullanıcı alt sağlamaktan ile olası [Yönetim birimleri (Önizleme)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).


  > [!NOTE]
  > Bu rolü daha önce "parola Yöneticisi" çağrıldı [Azure portalında](https://portal.azure.com/). Azure AD PowerShell, Azure AD Graph API'si ve Microsoft Graph API adını eşleştirmek için "Yardım Masası Yöneticisi" için biz adını değiştirmiş olursunuz. Kısa bir süre için adı "Yardım Masası (parola) yönetici için" Azure portalında "Yardım Masası Yöneticisi" değişiklikten önce değiştireceğiz.


* **[Power BI Yöneticisi](#power-bi-service-administrator)** : Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Power BI içinde genel izinlere sahip olmanın yanı sıra destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [Power BI yönetici rolünü anlama](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Power BI Hizmet Yöneticisi olarak" olarak tanımlanır. "Power BI Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Ayrıcalıklı kimlik doğrulaması yönetici](#privileged-authentication-administrator)** : Bu role sahip kullanıcılar, ayarlayın ya da genel yöneticileri dahil tüm kullanıcılar için parola olmayan kimlik bilgilerini sıfırlayın. Kullanıcıların mevcut olmayan bir parola kimlik bilgisi karşı (örn: MFA'yı FIDO) yeniden kaydedin ve 'MFA cihazda unutmayın', iptal etme ayrıcalıklı kimlik doğrulaması yöneticileri zorlayabilirsiniz MFA için tüm kullanıcılar sonraki oturum açma istemi. Ayrıcalıklı kimlik doğrulaması yöneticiler şunları yapabilir:
  * Kullanıcıları, mevcut olmayan bir parola kimlik bilgisi karşı (örn: MFA'yı FIDO) yeniden kaydetmek için
  * 'MFA cihazda unutmayın', iptal etmek için mfa'yı bir sonraki oturum açma istemi

* **[Ayrıcalıklı Rol Yöneticisi](#privileged-role-administrator)** : Bu role sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rol, Privileged Identity Management ve Yönetim birimleri tüm yönlerini yönetilmesine izin verir.

  <b>Önemli</b>: Bu rolü genel Yönetici rolüne dahil olmak üzere tüm Azure AD rol atamalarını yönetme olanağı sağlar. Bu rol, oluşturma veya kullanıcıları güncelleştirerek gibi Azure AD'de herhangi bir ayrıcalıklı yeteneklerini içermez. Ancak, bu role atanan kullanıcılar kendileri veya diğerleri ek ayrıcalık ek rolleri atayarak verebilirsiniz.

* **[Rapor okuyucu](#reports-reader)** : Power BI'da raporları Pano Microsoft 365 Yönetim merkezinde ve benimseme bağlam paketini ve bu role sahip kullanıcılar, kullanım raporlama verileri görüntüleyebilir. Ayrıca, rol oturum açma için erişim sağlayan raporları ve etkinlik Azure AD'de ve Microsoft Graph tarafından döndürülen veri raporlama API'si. Rapor okuyucu rolüne atanan bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar, ayarları veya Exchange gibi ürüne özel yönetim merkezlerine erişimi yapılandırmak için bir yönetici izinleri yok. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[Arama yönetici](#search-administrator)** : Bu roldeki kullanıcılar, Microsoft 365 Yönetim Merkezi'nde tüm Microsoft Search Yönetimi özelliklerine tam erişime sahiptir. Yöneticileri ara arama yöneticileri ve arama Düzenleyicisi roller kullanıcılara, temsilci ve oluşturun ve içeriği gibi yer işaretleri, Q & olarak ve konumları yönetin. Ayrıca, bu kullanıcılar kullanarak ileti Merkezi'ni görüntülemek, hizmet durumunu izleyebilir ve hizmet istekleri oluşturma.

* **[Arama Düzenleyicisi](#search-editor)** : Bu roldeki kullanıcılar, oluşturma, yönetme ve Microsoft Search yer işaretleri, Q dahil olmak üzere Microsoft 365 Yönetim merkezinde & olarak ve konumları, içerik silebilir.

* **[Güvenlik Yöneticisi](#security-administrator)** : Bu role sahip kullanıcılar, Microsoft 365 Güvenlik Merkezi, Azure Active Directory kimlik koruması, Azure Information Protection ve Office 365 güvenlik ve uyumluluk Merkezi'nde özelliklerini güvenlikle ilgili yönetmek için izinlere sahip. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  İçinde | Yapabilirsiniz
  --- | ---
  [Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetlerindeki güvenlikle ilgili ilkelerini izleme<br>Güvenlik tehditlerini ve Uyarıları yönetme<br>Raporları görüntüleme
  Kimlik koruma Merkezi | Güvenlik okuyucu rolünün tüm izinler<br>Ayrıca, parola sıfırlama dışında kimlik koruma merkezi tüm işlemleri gerçekleştirme imkanı
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
* **[Güvenlik okuyucusu](#security-reader)** : Bu role sahip kullanıcılar, özelliği, güvenlikle ilgili tüm bilgiler Microsoft 365 Güvenlik Merkezi, Azure Active Directory, kimlik koruması, Privileged Identity Management, yanı sıra Azure Active okuma yeteneği dahil olmak üzere genel salt okunur erişim vardır. Directory oturum açma raporlarını ve denetim günlüklerini ve Office 365 güvenlik ve uyumluluk Merkezi'nde. Office 365 izinler hakkında daha fazla bilgi edinilebilir [Office 365 güvenlik ve uyumluluk Merkezi'nde izinler](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  İçinde | Yapabilirsiniz
  --- | ---
  [Microsoft 365 Güvenlik Merkezi](https://protection.office.com) | Microsoft 365 hizmetlerindeki güvenlikle ilgili ilkelerini görüntüle<br>Görünüm güvenlik tehditlerini ve uyarılar<br>Raporları görüntüleme
  Kimlik koruma Merkezi | Tüm güvenlik raporları ve güvenlik özellikleri için ayarlar bilgilerini okuyun<br><ul><li>İstenmeyen postadan koruma<li>Şifreleme<li>Veri kaybı önleme<li>Kötü amaçlı yazılımdan koruma<li>Gelişmiş tehdit koruması<li>Avından<li>Mailflow kuralları
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Tüm bilgileri salt okunur erişimi, Azure AD PIM'de ortaya: İlkeleri ve Azure AD rol atamaları için raporları, güvenlik inceler ve gelecekte Azure AD rol atamanız yanı sıra senaryoları için erişim ilkesi veriler ve raporlar için okuyun.<br>**Olamaz** herhangi bir değişiklik yapmak veya Azure AD PIM için kaydolun. PIM portalda veya PowerShell aracılığıyla kullanıcı bunlar için uygun, birisi Bu roldeki ek roller (örneğin, genel yönetici veya ayrıcalıklı Rol Yöneticisi) etkinleştirebilirsiniz.
  [Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Güvenlik ilkelerini görüntüleme<br>Güvenlik tehditlerini araştırmak ve görüntüleme<br>Raporları görüntüleme
  Windows Defender ATP ve EDR | Görüntüleyebilir ve Uyarıları araştırma
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Görünümleri kullanıcı, cihaz, kayıt, yapılandırma ve uygulama bilgileri. Intune'da değişiklik yapamaz.
  [Cloud App Security'yi](https://docs.microsoft.com/cloud-app-security/manage-admins) | Salt okunur izinlere sahiptir ve Uyarıları yönetebilir
  [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Öneriler ve uyarılar, güvenlik ilkeleri, güvenlik durumlarını görüntüleyebilir ancak değişiklik yapamaz görünüm görüntüleyebilirsiniz.
  [Office 365 hizmet durumu](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 hizmetlerinin durumunu görüntüleyin

* **[Hizmet desteği Yöneticisi](#service-support-administrator)** : Bu role sahip kullanıcılar açabilir destek istekleri ile Microsoft Azure ve Office 365 Hizmetleri ve hizmet panosunu ve ileti merkezini de [Azure portalında](https://portal.azure.com) ve [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com). Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "olarak hizmet desteği Yöneticisi." tanımlanır "Hizmet Yöneticisi" olarak [Azure portalında](https://portal.azure.com), [Microsoft 365 Yönetim merkezini](https://admin.microsoft.com)ve Intune portalı.

* **[SharePoint Yöneticisi](#sharepoint-service-administrator)** : Bu role sahip kullanıcılar Microsoft SharePoint hizmet mevcut olduğunda Online içinde genel izinlere sahip olmanın yanı sıra oluşturun ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir. Daha fazla bilgiye [hakkında Office 365 Yönetici rolleri](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "SharePoint Hizmet Yöneticisi olarak." tanımlanır "SharePoint Yöneticisi" olarak [Azure portalında](https://portal.azure.com).

* **[İşletme için Skype / Lync Yöneticisi](#lync-service-administrator)** : Bu role sahip olan kullanıcılar hizmet olduğunda Microsoft Skype Kurumsal, içinde genel izinlere sahip, hem de Azure Active Directory'de Skype özgü kullanıcı özniteliklerini yönetin. Ayrıca, bu rol, destek biletlerini yönetebilir ve hizmet durumunu izleyebilir ve takımlar ve Skype kurumsal iş Yönetim Merkezi erişim olanağı verir. Hesap, takımlar için lisanslanmalıdır veya ekipler PowerShell cmdlet'leri çalıştırılamaz. Daha fazla bilgi [hakkında Skype kurumsal iş yöneticisi rolüne](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) ve lisans bilgileri takımlar [iş ve Microsoft Teams eklenti lisansı için Skype](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "Lync Hizmet Yöneticisi" olarak tanımlanır. "Skype Kurumsal Yöneticisi" olarak [Azure portalında](https://portal.azure.com/).

* **[Takımlar, yönetici](#teams-service-administrator)** : Bu roldeki kullanıcılar Microsoft Teams ve Skype üzerinden Microsoft Teams iş yükü iş Yönetim Merkezi ve ilgili PowerShell modülleri için tüm özelliklerini yönetebilir. Bu, diğer alanları arasında telefon, Mesajlaşma, toplantılar ve takımlar için ilgili tüm Yönetim Araçlar içerir. Bu rol, ayrıca oluşturun ve tüm Office 365 grupları yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı verir.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell'de bu rol "takımlar Hizmet Yöneticisi" olarak tanımlanır. "Takımlar Yönetici" olarak [Azure portalında](https://portal.azure.com).

* **[Takımlar iletişimleri Yöneticisi](#teams-communications-administrator)** : Bu roldeki kullanıcılar, ilgili ses & telefon için Microsoft Teams iş yükü özelliklerini yönetebilir. Bu telefon numarası ataması, ses ve toplantı ilkeleri ve çağrı analizi araç takımı tam erişim için yönetim araçlarını içerir.

* **[Takımlar iletişimleri destek mühendisi](#teams-communications-support-engineer)** : Bu roldeki kullanıcılar, Microsoft Teams & Skype Kurumsal'a kullanarak sorun giderme araçları Microsoft Teams ve Skype kurumsal iş Yönetim Merkezi için kullanıcı çağrısı içinde iletişim sorunları giderebilirsiniz. Bu roldeki kullanıcılar, ilgili tüm katılımcılar için tam çağrı kayıt bilgileri görüntüleyebilirsiniz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[Takımlar iletişimleri destek uzmanı](#teams-communications-support-specialist)** : Bu roldeki kullanıcılar, Microsoft Teams & Skype Kurumsal'a kullanarak sorun giderme araçları Microsoft Teams ve Skype kurumsal iş Yönetim Merkezi için kullanıcı çağrısı içinde iletişim sorunları giderebilirsiniz. Bu roldeki kullanıcılar yalnızca bunlar Aranan kullanıcıyı çağrıda kullanıcı ayrıntıları da görüntüleyebilirsiniz. Bu rol, görüntülemek, oluşturmak veya destek biletlerini yönetme erişimi vardır.

* **[Kullanıcı Yöneticisi](#user-administrator)** : Bu rolü olan kullanıcılar kullanıcıları, oluşturmanız ve bazı kısıtlamalar (aşağıya bakın) sahip kullanıcılar tüm özelliklerini yönetebilir ve parola süre sonu ilkeleri güncelleştirebilirsiniz. Ayrıca, bu role sahip kullanıcılar oluşturun ve tüm gruplarını yönetin. Bu rolü, oluşturma ve kullanıcı görünümleri yönetme, destek biletlerini yönetebilir ve hizmet durumu izleme olanağı da içerir.

  | | |
  | --- | --- |
  |Genel izinler|<p>Kullanıcılar ve gruplar oluşturma</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Office destek biletlerini yönetebilir<p>Parola süre sonu ilkeleri güncelleştirin|
  |<p>Tüm yöneticilerin tüm kullanıcılar dahil</p>|<p>Lisanslarını yönetme</p><p>Kullanıcı asıl adı dışındaki tüm kullanıcı özelliklerini yönetme</p>
  |Yönetici olmayan ya da aşağıdakilerden birini sınırlı yönetici rolleri yalnızca kullanıcılar üzerinde:<ul><li>Dizin okuyucular<li>Konuk davet eden<li>Yardım Masası Yöneticisi<li>İleti Merkezi okuyucusu<li>Rapor okuyucu<li>Kullanıcı Yöneticisi|<p>Silme ve geri yükleme</p><p>Devre dışı bırakma ve etkinleştirme</p><p>Geçersiz belirteç yenileme</p><p>Kullanıcı asıl adı dahil olmak üzere tüm kullanıcı özelliklerini yönetme</p><p>Parola sıfırla</p><p>Güncelleştirme (FIDO) cihaz anahtarları</p>
  
  <b>Önemli</b>: Bu role sahip kullanıcılar, gizli veya özel bilgiler veya kritik yapılandırması içinde ve dışında Azure Active Directory erişim sahibi kişiler için parolaları değiştirebilirsiniz. Bir kullanıcının parolasını değiştirmek, kullanıcının kimliğine ve izinleri tutarlılığı varsayma olanağı anlamına gelir. Örneğin:
  * Oldukları uygulamaları kimlik bilgilerini yöneten uygulama kaydı ve kurumsal uygulama sahipleri. Bu uygulamaları Azure AD'deki izinleri ayrıcalıklı ve kullanıcı yöneticileri izni başka bir yerde değil. Kullanıcı Yöneticisi uygulama sahibinin kimliğini varsayar ve daha sonra da mümkün olabilir. Bu yol üzerinden uygulama için kimlik bilgilerini güncelleştirerek ayrıcalıklı bir uygulamanın kimliğini varsayılır.
  * Azure aboneliği sahiplerine, gizli veya özel bilgiler veya kritik yapılandırma Azure'da erişebilirsiniz.
  * Grup üyeliği yönetebilen güvenlik grubu ve Office 365 grubu sahipler. Bu gruplar, gizli veya özel bilgiler veya Azure AD'de önemli yapılandırma ve diğer yerlerde erişim izni verebilir.
  * Yöneticiler diğer hizmetleri Azure AD dışında Exchange Online, Office güvenlik ve uyumluluk Merkezi'nde ve İnsan Kaynakları sistemler ister.
  * Yönetici olmayan kullanıcılar, Yöneticiler, yasal Konseyi ve gizli veya özel bilgiler erişebilirsiniz İnsan Kaynakları çalışanları gibi.

## <a name="role-permissions"></a>Rol izinleri
Aşağıdaki tablolarda her rol için belirtilen Azure Active Directory'de özel izinler açıklanmaktadır. Bazı roller, Azure Active Directory dışında Microsoft hizmetlerindeki ek izinlere sahip olabilir.

### <a name="application-administrator"></a>Uygulama Yöneticisi
Oluşturabilir ve uygulama kayıtlarını ve kurumsal uygulamaları tüm özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de Applications.Audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de Applications.Authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Basic/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
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
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="application-developer"></a>Uygulama geliştirici
Uygulama kayıtları 'kullanıcılar uygulamaları kaydedebilir' bağımsız oluşturabilirsiniz ayarı.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Azure Active Directory'de uygulamalar oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Azure Active Directory'de appRoleAssignments oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory'de oAuth2PermissionGrants oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Azure Active Directory'de servicePrincipals oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |

### <a name="authentication-administrator"></a>Kimlik doğrulama Yöneticisi
Görüntüleyebilir, ayarlayabilir ve herhangi bir yönetici olmayan kullanıcı kimlik doğrulama yöntemi bilgileri sıfırlama izni.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| microsoft.aad.directory/users/strongAuthentication/update | Güncelleştirme güçlü kimlik doğrulama özellikleri MFA kimlik bilgilerini ister. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="azure-information-protection-administrator"></a>Azure Information Protection Yöneticisi
Azure Information Protection hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="b2c-user-flow-administrator"></a>B2C Kullanıcı Akışı Yöneticisi
Oluşturabilir ve kullanıcı Akışları'nın tüm özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.B2C/userFlows/allTasks | Okuma ve Azure Active Directory B2C'de kullanıcı akışları yapılandırın. |

### <a name="b2c-user-flow-attribute-administrator"></a>B2C Kullanıcı Akışı Öznitelik Yöneticisi
Oluşturun ve tüm kullanıcı akışları için kullanılabilir özniteliği şema yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.B2C/userAttributes/allTasks | Okuma ve Azure Active Directory B2C'de kullanıcı öznitelikleri yapılandırabilirsiniz. |

### <a name="b2c-ief-keyset-administrator"></a>B2C IEF Anahtar kümesi Yöneticisi
Federasyon ve şifreleme, kimlik deneyimi çerçevesi için gizli dizilerini yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.B2C/trustFramework/keySets/allTasks | Okuma ve Azure Active Directory B2C'de anahtar kümesi yapılandırın. |

### <a name="b2c-ief-policy-administrator"></a>B2C IEF İlkesi Yöneticisi
Oluşturun ve kimlik deneyimi çerçevesi güven framework ilkelerini yönetin.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.B2C/trustFramework/Policies/allTasks | Okuma ve Azure Active Directory B2C'de özel ilkeler yapılandırın. |

### <a name="billing-administrator"></a>Faturalama Yöneticisi
Ödeme bilgilerini güncelleştirme gibi sık kullanılan faturalandırma görevlerini gerçekleştirebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Organization/Basic/Update | Kuruluşunuz Azure Active Directory'de temel özelliklerini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalama tüm özelliklerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="desktop-analytics-administrator"></a>Masaüstü Analytics Yöneticisi
Erişebilir ve Masaüstü Yönetimi Araçları ve Hizmetleri Intune dahil olmak üzere yönetebilirsiniz.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.desktopAnalytics/allEntities/allTasks | Masaüstü Analytics tüm özelliklerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="cloud-application-administrator"></a>Bulut uygulaması Yöneticisi
Oluşturabilir ve uygulama kayıtlarını ve kurumsal uygulamaları uygulama ara sunucusu hariç tüm özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Audience/Update | Azure Active Directory'de Applications.Audience özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Authentication/Update | Azure Active Directory'de Applications.Authentication özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Basic/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Create | Azure Active Directory'de uygulamalar oluşturun. |
| Microsoft.aad.Directory/Applications/credentials/Update | Azure Active Directory'de Applications.credentials özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.aad.Directory/Applications/Owners/Update | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Permissions/Update | Azure Active Directory'de Applications.Permissions özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments güncelleştirin. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments silin. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory'de policies.applicationConfiguration özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory'de policies.applicationConfiguration özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="cloud-device-administrator"></a>Bulut cihaz Yöneticisi
Azure AD'de cihazları yönetmek için tam erişim.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory cihazları silin. |
| Microsoft.aad.Directory/Devices/disable | Cihazları Azure Active Directory'de devre dışı bırakın. |
| Microsoft.aad.Directory/Devices/Enable | Cihazların Azure Active Directory'de sağlayın. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="company-administrator"></a>Şirket Yöneticisi
Azure AD tüm özelliklerini yönetebilir ve bu Azure AD kimliklerini kullanan Microsoft Hizmetleri.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.aad.cloudAppSecurity standart özellikleri. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Oluşturma ve administrativeUnits, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/applications/allProperties/allTasks | Oluşturma ve uygulamaları, silin ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Oluşturma ve appRoleAssignments, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
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
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Oluşturma ve subscribedSkus, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directory/users/allProperties/allTasks | Oluşturma ve kullanıcıları, silme ve okuma ve Azure Active Directory'de tüm özelliklerini güncelleştirir. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'e bağlanan tüm eylemleri gerçekleştirin. |
| microsoft.aad.identityProtection/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.aad.identityProtection standart özellikleri. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Tüm kaynakların microsoft.azure.advancedThreatProtection okuyun. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection tüm özelliklerini yönetebilir. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 faturalama tüm özelliklerini yönetebilir. |
| microsoft.intune/allEntities/allTasks | Intune tüm özelliklerini yönetebilir. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi tüm özelliklerini yönetebilir |
| Microsoft.Office365.desktopAnalytics/allEntities/allTasks | Masaüstü Analytics tüm özelliklerini yönetebilir. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online tüm özelliklerini yönetebilir. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 müşteri kasa tüm özelliklerini yönetebilir |
| microsoft.office365.messageCenter/messages/read | Microsoft.office365.messageCenter iletilerini okuyun. |
| Microsoft.Office365.messageCenter/securityMessages/Read | SecurityMessages microsoft.office365.messageCenter içinde okuyun. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Office 365 koruma merkezinde tüm özelliklerini yönetebilir. |
| Microsoft.Office365.securityComplianceCenter/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.securityComplianceCenter standart özellikleri. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365 tüm özelliklerini yönetebilir. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI tüm özelliklerini yönetebilir. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Tüm kaynakların microsoft.windows.defenderAdvancedThreatProtection okuyun. |

### <a name="compliance-administrator"></a>Uyumluluk Yöneticisi
Okuyup uyumluluk yapılandırmasını ve raporları, Azure AD'de yönetebilir ve Office 365.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.complianceManager/allEntities/allTasks | Office 365 uyumluluk Yöneticisi tüm özelliklerini yönetebilir |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online tüm özelliklerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi
Koşullu erişim özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Azure Active Directory'de policies.conditionalAccess özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/create | Azure Active Directory'de ilkeleri oluşturun. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Azure Active Directory ilkeleri silin. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Azure Active Directory'de policies.conditionalAccess özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory'de policies.conditionalAccess özelliği okuyun. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | Azure Active Directory'de policies.conditionalAccess özelliğini güncelleştirin. |

### <a name="crm-service-administrator"></a>CRM Hizmet Yöneticisi
Dynamics 365 ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365 tüm özelliklerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="customer-lockbox-access-approver"></a>Müşteri kasası erişim onaylayıcı
Müşteri kuruluş verilerine erişmek için Microsoft destek isteklerini onaylayabilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.lockbox/allEntities/allTasks | Office 365 müşteri kasa tüm özelliklerini yönetebilir |

### <a name="device-administrators"></a>Cihaz yöneticileri
Bu role atanan kullanıcılar, Azure AD'ye katılmış cihazlarda yerel Yöneticiler grubuna eklenir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory'de groupSettings temel özelliklerini okuyun. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory'de groupSettingTemplates temel özelliklerini okuyun. |

### <a name="directory-readers"></a>Dizin okuyucular
Temel dizin bilgileri okuyabilir. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Azure Active Directory'de administrativeUnits temel özelliklerini okuyun. |
| microsoft.aad.directory/administrativeUnits/members/read | Azure Active Directory'de administrativeUnits.members özelliği okuyun. |
| Microsoft.aad.Directory/Applications/Basic/Read | Uygulamaları Azure Active Directory'de temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Applications/Owners/Read | Azure Active Directory'de Applications.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Applications/Policies/Read | Azure Active Directory'de Applications.Policies özelliği okuyun. |
| Microsoft.aad.Directory/Contacts/Basic/Read | Azure Active Directory'de kişiler üzerinde temel özelliklerini okuyun. |
| microsoft.aad.directory/contacts/memberOf/read | Azure Active Directory'de contacts.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Contracts/Basic/Read | Azure Active Directory'de sözleşmelerinde temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Devices/Basic/Read | Azure Active Directory'de cihazlarda temel özelliklerini okuyun. |
| microsoft.aad.directory/devices/memberOf/read | Azure Active Directory'de devices.memberOf özelliği okuyun. |
| microsoft.aad.directory/devices/registeredOwners/read | Azure Active Directory'de devices.registeredOwners özelliği okuyun. |
| microsoft.aad.directory/devices/registeredUsers/read | Azure Active Directory'de devices.registeredUsers özelliği okuyun. |
| microsoft.aad.directory/directoryRoles/basic/read | Azure Active Directory'de directoryRoles temel özelliklerini okuyun. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Azure Active Directory'de directoryRoles.eligibleMembers özelliği okuyun. |
| microsoft.aad.directory/directoryRoles/members/read | Azure Active Directory'de directoryRoles.members özelliği okuyun. |
| Microsoft.aad.Directory/Domains/Basic/Read | Azure Active Directory etki alanlarında temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory'de groups.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Basic/Read | Azure Active Directory'de grupları temel özelliklerini okuyun. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory'de groups.memberOf özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Read | Azure Active Directory'de Groups.Members özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Owners/Read | Azure Active Directory'de Groups.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Settings/Read | Azure Active Directory'de Groups.Settings özelliği okuyun. |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory'de groupSettings temel özelliklerini okuyun. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory'de groupSettingTemplates temel özelliklerini okuyun. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Azure Active Directory'de oAuth2PermissionGrants temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Organization/Basic/Read | Kuruluşunuz Azure Active Directory'de temel özelliklerini okuyun. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory'de organization.trustedCAsForPasswordlessAuth özelliği okuyun. |
| microsoft.aad.directory/roleAssignments/basic/read | Azure Active Directory'de rol temel özelliklerini okuyun. |
| microsoft.aad.directory/roleDefinitions/basic/read | Azure Active Directory'de roleDefinitions temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory'de servicePrincipals temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliği okuyun. |
| microsoft.aad.directory/subscribedSkus/basic/read | Azure Active Directory'de subscribedSkus temel özelliklerini okuyun. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/Basic/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |

### <a name="directory-synchronization-accounts"></a>Dizin eşitlemesi hesapları
Yalnızca Azure AD Connect hizmeti tarafından kullanılır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Azure Active Directory'de organization.dirSync özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeleri oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Basic/Read | İlkeleri Azure Active Directory'de temel özelliklerini okuyun. |
| Microsoft.aad.Directory/Policies/Basic/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Owners/Read | Azure Active Directory'de policies.Owners özelliği okuyun. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Azure Active Directory'de policies.policiesAppliedTo özelliği okuyun. |
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory'de policies.tenantDefault özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de servicePrincipals.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory'de servicePrincipals temel özelliklerini okuyun. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory'de servicePrincipals oluşturun. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory'de servicePrincipals.memberOf özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory'de servicePrincipals.owners özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory'de servicePrincipals.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory'de servicePrincipals.policies özelliği okuyun. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect'e bağlanan tüm eylemleri gerçekleştirin. |

### <a name="directory-writers"></a>Dizin yazıcılar
Okuma ve yazma temel dizin bilgileri kullanabilirsiniz. Uygulamalara erişim vermek için kullanıcılar için tasarlanmamıştır.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/groupSettings/basic/update | Azure Active Directory'de groupSettings temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/groupSettings/create | Azure Active Directory'de groupSettings oluşturun. |
| microsoft.aad.directory/groupSettings/delete | Azure Active Directory'de groupSettings silin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |

### <a name="exchange-service-administrator"></a>Exchange Hizmeti Yöneticisi
Exchange ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.Exchange/allEntities/allTasks | Exchange Online tüm özelliklerini yönetebilir. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="external-identity-provider-administrator"></a>Dış kimlik sağlayıcı Yöneticisi
Kimlik sağlayıcıları kullanmak için doğrudan federasyona yapılandırın.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.B2C/identityProviders/allTasks | Okuma ve Azure Active Directory B2C'de kimlik sağlayıcılarını yapılandırma. |

### <a name="guest-inviter"></a>Konuk davet eden
'Üyeler davet edebilir' bağımsız Konuk kullanıcıları davet etmeden ayarı.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory'de users.appRoleAssignments özelliği okuyun. |
| Microsoft.aad.Directory/Users/Basic/Read | Azure Active Directory Kullanıcıları temel özelliklerini okuyun. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory'de users.directReports özelliği okuyun. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory'de Konuk kullanıcılar davet edin. |
| Microsoft.aad.Directory/Users/Manager/Read | Azure Active Directory'de Users.Manager özelliği okuyun. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory'de users.memberOf özelliği okuyun. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory'de users.oAuth2PermissionGrants özelliği okuyun. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory'de users.ownedDevices özelliği okuyun. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory'de users.ownedObjects özelliği okuyun. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory'de users.registeredDevices özelliği okuyun. |

### <a name="helpdesk-administrator"></a>Yardım Masası Yöneticisi
Yönetici olmayan kullanıcıların ve Yardım Masası yöneticileri için parolaları sıfırlayabilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="intune-service-administrator"></a>Intune Hizmet Yöneticisi
Intune ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| Microsoft.aad.Directory/Devices/Basic/Update | Azure Active Directory'de cihazlarda temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| Microsoft.aad.Directory/Devices/Create | Cihazları Azure Active Directory'de oluşturun. |
| Microsoft.aad.Directory/Devices/DELETE | Azure Active Directory cihazları silin. |
| microsoft.aad.directory/devices/registeredOwners/update | Azure Active Directory'de devices.registeredOwners özelliğini güncelleştirin. |
| microsoft.aad.directory/devices/registeredUsers/update | Azure Active Directory'de devices.registeredUsers özelliğini güncelleştirin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliği okuyun. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.intune/allEntities/allTasks | Intune tüm özelliklerini yönetebilir. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |

### <a name="kaizala-administrator"></a>Kaizala yönetici
Microsoft Kaizala ayarlarını yönetebilirsiniz.  

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >  
  
| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Okuma Office 365 Yönetim Merkezi. |

### <a name="license-administrator"></a>Lisans Yöneticisi
Kullanıcılar ve gruplar ürün lisanslarını yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| microsoft.aad.directory/users/usageLocation/update | Azure Active Directory'de users.usageLocation özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="lync-service-administrator"></a>Lync Hizmet Yöneticisi
Skype Kurumsal ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.skypeForBusiness/allEntities/allTasks | Tüm yönlerini Skype Kurumsal çevrimiçi yönetin. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="message-center-privacy-reader"></a>İleti Merkezi gizlilik okuyucusu
İleti Merkezi gönderileri, veri gizliliği iletileri, grupları, etki alanları ve abonelikler okuyabilirsiniz.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| microsoft.office365.messageCenter/messages/read | Microsoft.office365.messageCenter iletilerini okuyun. |
| Microsoft.Office365.messageCenter/securityMessages/Read | SecurityMessages microsoft.office365.messageCenter içinde okuyun. |

### <a name="message-center-reader"></a>İleti Merkezi okuyucusu
Kuruluşlarında Office 365 ileti Merkezi'nde güncelleştirmelerini ve iletileri okuyabilir. 

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| microsoft.office365.messageCenter/messages/read | Microsoft.office365.messageCenter iletilerini okuyun. |

### <a name="partner-tier1-support"></a>Partner Tier1 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory'de grupları oluşturun. Oluşturucu, ilk sahibi olarak eklenir ve oluşturulan nesnesi oluşturan kişinin 250'den oluşturulan nesne kotaya sayılmaktadır. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="partner-tier2-support"></a>Partner Tier2 Desteği
Kullanmayın - genel kullanım için tasarlanmamıştır.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| microsoft.aad.directory/domains/allTasks | Oluşturma ve etki alanlarını silmek ve okuma ve güncelleştirme, Azure Active Directory'de standart özellikleri. |
| Microsoft.aad.Directory/Groups/Create | Azure Active Directory'de grupları oluşturun. |
| Microsoft.aad.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| Microsoft.aad.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.aad.Directory/Organization/Basic/Update | Kuruluşunuz Azure Active Directory'de temel özelliklerini güncelleştirin. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory'deki kullanıcı lisansları yönetin. |
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="power-bi-service-administrator"></a>Power BI Hizmet Yöneticisi
Power BI ürününün tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI tüm özelliklerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="privileged-authentication-administrator"></a>Ayrıcalıklı kimlik yöneticisi
Görüntüleyebilir, ayarlayabilir ve herhangi bir kullanıcı (yönetici veya yönetici olmayan) için kimlik doğrulama yöntemi bilgileri sıfırlama izni.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| microsoft.aad.directory/users/strongAuthentication/update | Güncelleştirme güçlü kimlik doğrulama özellikleri MFA kimlik bilgilerini ister. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="privileged-role-administrator"></a>Ayrıcalıklı Rol Yöneticisi
Azure AD'de rol atamalarını ve ayrıcalıklı Kimlik Yönetimi'nin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.aad.privilegedIdentityManagement standart özellikleri. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/allTasks | Okuma ve Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini yapılandırın. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Okuma ve Azure Active Directory'de servicePrincipals.oAuth2PermissionGrants özelliğini yapılandırın. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Oluşturma ve yönetme (üyeleri dahil) yönetim birimleri |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Oluşturabilir ve rol atamalarını yönetebilir. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Oluşturun ve rol tanımları yönetin. |

### <a name="reports-reader"></a>Rapor okuyucu
Oturum açma okuyabilir ve Denetim raporları.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |

### <a name="search-administrator"></a>Arama yönetici
Oluşturabilir ve Microsoft Search ayarları tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Microsoft.office365.messageCenter iletilerini okuyun. |
| Microsoft.Office365.Search/allEntities/allProperties/allTasks | Oluşturun ve tüm kaynakları silmek ve okuma ve microsoft.office365.search tüm özelliklerini güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |

### <a name="search-editor"></a>Arama Düzenleyicisi
Oluşturabilir ve editör içerikleri yer işaretleri, Q gibi ve olarak, konumları ve kat planı yönetin.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Microsoft.office365.messageCenter iletilerini okuyun. |
| Microsoft.Office365.Search/Content/allProperties/allTasks | Oluşturun ve içerik, silebilir ve okuma ve microsoft.office365.search tüm özellikleri güncelleştirin. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
Güvenlik bilgilerini ve raporları okuma ve Azure AD'de yapılandırmasını yönetmek ve Office 365.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.aad.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| Microsoft.aad.Directory/Policies/Basic/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Policies/Create | Azure Active Directory'de ilkeleri oluşturun. |
| Microsoft.aad.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.aad.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory'de policies.tenantDefault özelliğini güncelleştirin. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.identityProtection/allEntities/read | Tüm kaynakların microsoft.aad.identityProtection okuyun. |
| microsoft.aad.identityProtection/allEntities/update | Tüm kaynakların microsoft.aad.identityProtection güncelleştirin. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 koruma merkezinde tüm yönlerini okuyun. |
| microsoft.office365.protectionCenter/allEntities/update | Tüm kaynakların microsoft.office365.protectionCenter güncelleştirin. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="security-reader"></a>Güvenlik okuyucusu
Azure AD'de güvenlik bilgilerini ve raporları okuyabilir ve Office 365.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| microsoft.aad.identityProtection/allEntities/read | Tüm kaynakların microsoft.aad.identityProtection okuyun. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Tüm kaynakların microsoft.aad.privilegedIdentityManagement okuyun. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 koruma merkezinde tüm yönlerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="service-support-administrator"></a>Hizmet desteği Yöneticisi
Hizmet durumu bilgilerini okuyabilir ve Destek biletlerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="sharepoint-service-administrator"></a>SharePoint Hizmet Yöneticisi
SharePoint hizmetinin tüm özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.SharePoint/allEntities/allTasks | Oluşturma ve tüm kaynakları silmek ve okuma ve güncelleştirme microsoft.office365.sharepoint standart özellikleri. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

### <a name="teams-communications-administrator"></a>Takımlar iletişimleri Yöneticisi
Arama ve Microsoft Teams hizmet içinde toplantıları özelliklerini yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |

### <a name="teams-communications-support-engineer"></a>Takımlar iletişimleri destek mühendisi
Gelişmiş araçlar kullanarak takımlar içinde iletişim sorunlarını giderebilirsiniz.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="teams-communications-support-specialist"></a>Takımlar iletişimleri Destek Uzmanı
Temel araçları kullanarak takımlar içinde iletişim sorunlarını giderebilirsiniz.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |

### <a name="teams-service-administrator"></a>Takımlar Hizmet Yöneticisi
Microsoft Teams hizmeti yönetebilir.

  > [!NOTE]
  > Bu rol, Azure Active Directory dışında ek izinlere sahiptir. Daha fazla bilgi için yukarıdaki rol tanımı bakın.
  >
  >

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory'de groups.hiddenMembers özelliği okuyun. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory'de Groups.Unified özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Basic/Update | Office 365 gruplarının temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Create | Office 365 grupları oluşturun. |
| Microsoft.aad.Directory/Groups/Unified/DELETE | Office 365 grupları silin. |
| Microsoft.aad.Directory/Groups/Unified/Members/Update | Office 365 gruplarının üyeliklerini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Unified/Owners/Update | Office 365 grupları sahipliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |
| Microsoft.Office365.usageReports/allEntities/Read | Kullanım raporlarını okuma Office 365. |

### <a name="user-administrator"></a>Kullanıcı Yöneticisi
Kullanıcılar ve gruplar, sınırlı yöneticilerin parolalarını sıfırlama dahil olmak üzere tüm özelliklerini yönetebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory'de appRoleAssignments oluşturun. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory'de appRoleAssignments silin. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory'de appRoleAssignments güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Basic/Update | Azure Active Directory'de kişiler üzerinde temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Contacts/Create | Azure Active Directory'de kişiler oluşturma. |
| Microsoft.aad.Directory/Contacts/DELETE | Azure Active Directory'de kişiler silin. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Groups/Basic/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
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
| Microsoft.aad.Directory/Users/Basic/Update | Azure Active Directory Kullanıcıları temel özelliklerini güncelleştirin. |
| Microsoft.aad.Directory/Users/Create | Azure Active Directory'de kullanıcıları oluşturun. |
| Microsoft.aad.Directory/Users/DELETE | Azure Active Directory'de kullanıcıları silin. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory tüm kullanıcı yenileme belirteçleri geçersiz. |
| Microsoft.aad.Directory/Users/Manager/Update | Azure Active Directory'de Users.Manager özelliğini güncelleştirin. |
| Microsoft.aad.Directory/Users/Password/Update | Azure Active Directory'de tüm kullanıcıların parolalarının güncelleştirin. Daha fazla ayrıntı için çevrimiçi belgelerine bakın. |
| Microsoft.aad.Directory/Users/Restore | Azure Active Directory silinen kullanıcıları geri yükleyin. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory'de users.userPrincipalName özelliğini güncelleştirin. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Okuma ve Azure hizmet durumu yapılandırın. |
| microsoft.azure.supportTickets/allEntities/allTasks | Oluşturun ve Azure destek biletlerini yönetebilir. |
| Microsoft.Office365.webPortal/allEntities/Basic/Read | Tüm kaynaklara microsoft.office365.webPortal temel özelliklerini okuyun. |
| Microsoft.Office365.serviceHealth/allEntities/allTasks | Okuma ve Office 365 hizmet durumunu yapılandırın. |
| Microsoft.Office365.supportTickets/allEntities/allTasks | Oluşturun ve Office 365 destek biletlerini yönetebilir. |

## <a name="role-template-ids"></a>Rolü şablonu kimlikleri

Rol şablon kimlikleri belirtilmeli çoğunlukla Graph API'si veya PowerShell kullanıcılar tarafından kullanılır.

Graf displayName | Azure portalında görünen adı | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Uygulama Yöneticisi | Uygulama Yöneticisi | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Uygulama geliştirici | Uygulama geliştirici | CF1C38E5-3621-4004-A7CB-879624DCED7C
Kimlik doğrulama Yöneticisi | Kimlik doğrulama Yöneticisi | c4e39bd9-1100-46d3-8c65-fb160da0071f
Azure Information Protection Yöneticisi | Azure Information Protection Yöneticisi | 7495fdc4-34c4-4d15-a289-98788ce399fd
B2C kullanıcı akışı yönetici | B2C kullanıcı akışı yönetici | 6e591065-9bad-43ed-90f3-e9424366d2f0
B2C Kullanıcı Akışı Öznitelik Yöneticisi | B2C Kullanıcı Akışı Öznitelik Yöneticisi | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
B2C IEF Anahtar kümesi Yöneticisi | B2C IEF Anahtar kümesi Yöneticisi | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C IEF İlkesi Yöneticisi | B2C IEF İlkesi Yöneticisi | 3edaf663-341e-4475-9f94-5c398ef6c070
Faturalama Yöneticisi | Faturalama yöneticisi | b0f54661-2d74-4c50-afa3-1ec803f12efe
Masaüstü Analytics Yöneticisi | Masaüstü Analytics Yöneticisi | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Bulut uygulaması Yöneticisi | Bulut uygulaması Yöneticisi | 158c047a-c907-4556-b7ef-446551a6b5f7
Bulut cihaz Yöneticisi | Bulut cihaz Yöneticisi | 7698a772-787b-4ac8-901f-60d6b08affd2
Şirket Yöneticisi | Genel yönetici | 62e90394-69f5-4237-9190-012177145e10
Uyumluluk Yöneticisi | Uyumluluk Yöneticisi | 17315797-102d-40b4-93e0-432062caca18
Koşullu Erişim Yöneticisi | Koşullu Erişim Yöneticisi | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM Hizmet Yöneticisi | Dynamics 365 Yöneticisi | 44367163-eba1-44c3-98af-f5787879f96a
Müşteri kasası erişim onaylayıcı | Müşteri kasası erişim onaylayıcı | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Cihaz yöneticileri | Cihaz yöneticileri | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Cihaz birleştirme | Cihaz birleştirme | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Cihaz yöneticileri | Cihaz yöneticileri | 2b499bcd-da44-4968-8aec-78e1674fa64d
Aygıt kullanıcıları | Aygıt kullanıcıları | d405c6df-0af8-4e3b-95e4-4d06e542189e
Dizin okuyucular | Dizin okuyucular | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Dizin eşitlemesi hesapları | Dizin eşitlemesi hesapları | d29b2b05-8046-44ba-8758-1e26182fcf32
Dizin yazıcılar | Dizin yazıcılar | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Hizmeti Yöneticisi | Exchange Yöneticisi | 29232cdf-9323-42fd-ade2-1d097af3e4de
Dış kimlik sağlayıcı Yöneticisi | Dış kimlik sağlayıcı Yöneticisi | be2f45a1-457d-42af-a067-6ec1fa63bc45
Konuk davet eden | Konuk davet eden | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Yardım Masası Yöneticisi | Parola Yöneticisi | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Intune Hizmet Yöneticisi | Intune Yöneticisi | 3a2c62db-5318-420d-8d74-23affee5d9d5
Kaizala yönetici | Kaizala yönetici | 74ef975b-6605-40af-a5d2-b9539d836353
Lisans Yöneticisi | Lisans Yöneticisi | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync Hizmet Yöneticisi | Skype Kurumsal Yöneticisi | 75941009-915a-4869-abe7-691bff18279e
İleti Merkezi gizlilik okuyucusu | İleti Merkezi gizlilik okuyucu | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
İleti Merkezi okuyucusu | İleti Merkezi okuyucu | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Partner Tier1 Desteği | Partner tier1 desteği | 4ba39ca4-527c-499a-b93d-d9b492c50246
Partner Tier2 Desteği | Partner tier2 desteği | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Power BI Hizmet Yöneticisi | Power BI Yöneticisi | a9ea8996-122f-4c74-9520-8edcd192826c
Ayrıcalıklı kimlik yöneticisi | Ayrıcalıklı kimlik yöneticisi | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Ayrıcalıklı Rol Yöneticisi | Ayrıcalıklı Rol Yöneticisi | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapor okuyucu | Rapor okuyucu | 4a5d8f65-41da-4de4-8968-e035b65339cf
Arama yönetici | Arama yönetici | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Arama Düzenleyicisi | Arama Düzenleyicisi | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Güvenlik Yöneticisi | Güvenlik yöneticisi | 194ae4cb-b126-40b2-bd5b-6091b380977d
Güvenlik okuyucusu | Güvenlik okuyucusu | 5d6b6bb7-de71-4623-b4af-96380a352509
Hizmet desteği Yöneticisi | Hizmet yöneticisi | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint Hizmet Yöneticisi | SharePoint Yöneticisi | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Takımlar iletişimleri Yöneticisi | Takımlar iletişimleri Yöneticisi | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Takımlar iletişimleri destek mühendisi | Takımlar iletişimleri destek mühendisi | f70938a0-fc10-4177-9e90-2178f8765737
Takımlar iletişimleri Destek Uzmanı | Takımlar iletişimleri Destek Uzmanı | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Takımlar Hizmet Yöneticisi | Takımlar Hizmet Yöneticisi | 69091246-20e8-4a56-aa4d-066075b2a7a8
Kullanıcı | Kullanıcı | a0b1b346-4d3e-4e8b-98f8-753987be4970
Kullanıcı Hesabı Yöneticisi | Kullanıcı Yöneticisi | fe930be7-5e62-47db-91af-98c3a49a38b1
Cihazla çalışma alanına katılma | Cihazla çalışma alanına katılma | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar, kullanım dışı bırakıldı ve gelecekte Azure AD'deki kaldırıldı.

* AdHoc Lisans Yöneticisi
* Cihaz birleştirme
* Cihaz yöneticileri
* Aygıt kullanıcıları
* E-posta doğrulanan kullanıcı Oluşturucu
* Posta kutusu Yöneticisi
* Cihazla çalışma alanına katılma

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliği Yöneticisi olarak kullanıcı atama hakkında daha fazla bilgi için bkz: [RBAC ve Azure portalını kullanarak erişimini yönetme](../../role-based-access-control/role-assignments-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
