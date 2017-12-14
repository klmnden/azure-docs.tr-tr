---
title: "Azure Active Directory'de yönetici rolleri atama | Microsoft Docs"
description: "Yönetici rolü oluşturmak veya kullanıcıları Düzenle, başkalarına yönetici rolleri atamak, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme. Yönetici rolü atanan bir kullanıcı, kuruluşunuzun abone olduğu tüm bulut hizmetleri arasında aynı izinleri vardır."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 7fc27e8e-b55f-4194-9b8f-2e95705fb731
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: curtand
ms.reviewer: Vince.Smith
ms.custom: it-pro;
ms.openlocfilehash: 66df4d709b60f2eb80329b8527b2a6edeb123168
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Azure Active Directory’de yönetici rolü atama

Azure Active Directory (Azure AD) kullanarak, farklı işlevler hizmet için ayrı Yöneticiler belirleyebilirsiniz. Yöneticiler Azure portalında ve rollerine bağlı olarak çeşitli özelliklerine erişime sahiptir, oluşturabilir veya kullanıcıların Düzenle, başkalarına yönetici rolleri atamak, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve başka şeylerin etki alanlarını yönetme. Yönetici rolü atanan bir kullanıcı için kuruluşunuzun için rolü Office 365 portalında veya Azure portalında veya için Azure AD modülünü kullanarak atamış olup abone olduğu bulut Hizmetleri tümüne aynı izinlere sahip Windows PowerShell.

Aşağıdaki yönetici rolleri kullanılabilir:

* **Faturalama Yöneticisi**: satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

* **Uyumluluk Yöneticisi**: Bu rolüne sahip kullanıcılar Office 365 güvenlik ve Uyumluluk Merkezi ve Exchange yönetici merkezini içinde yönetim izinlerine sahip. Daha fazla bilgi "[Office 365 Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Koşullu Erişim Yöneticisi**: kullanıcılar bu rol ile Azure Active Directory koşullu erişim ayarlarını yönetme olanağı.
  > [!NOTE]
  > Exchange ActiveSync koşullu erişim ilkesi azure'da dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **CRM Hizmet Yöneticisi**: Bu rol ile kullanıcınız Microsoft CRM hizmet mevcut olduğunda çevrimiçi içinde genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Cihaz yöneticileri**: Bu rolüne sahip kullanıcılar yerel makine Yöneticiler için Azure Active Directory'ye katılmış tüm Windows 10 cihazlarda haline gelir. Azure Active Directory'de cihaz nesnelerini yönetme yeteneği sahip değil.

* **Dizin okuyucular**: Bu desteklemeyen uygulamalar için atanmış olan, eski bir rolüdür [onayı Framework](active-directory-integrating-applications.md). Tüm kullanıcılara atanmamalıdır.

* **Dizin eşitleme hesapları**: kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil kullanılmaya veya diğer kullanım için desteklenir.

* **Directory yazıcılarının**: Bu desteklemeyen uygulamalar için atanmış olan, eski bir rolüdür [onayı Framework](active-directory-integrating-applications.md). Tüm kullanıcılara atanmamalıdır.

* **Exchange Hizmet Yöneticisi**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Genel yönetici / şirket Yöneticisi**: kullanıcılar bu rol ile Exchange Online, SharePoint Online ve Skype Kurumsal çevrimiçi gibi Azure Active Directory'ye federe Hizmetleri yanı sıra, Azure Active Directory içindeki tüm yönetim özelliklerine erişebilir. Azure Active Directory Kiracı için kaydolan kişi genel yönetici olur. Yalnızca küresel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel yönetici parolası herhangi bir kullanıcı ve diğer tüm yöneticilere sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell Bu rolün "Şirket Yönetici" olarak tanımlanır. "Genel yönetici" olarak [Azure portal](https://portal.azure.com).
  >
  >

* **Konuk davet eden**: Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetleri yönetebilir, "Üyeleri davet edebilirsiniz" Kullanıcı ayarı Hayır olarak ayarlandığında geçerlidir B2B işbirliğinin hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği Önizleme](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Diğer izinler içermez.

* **Intune hizmet yöneticisinin**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Intune çevrimiçi içinde genel izinlere sahiptir. Ayrıca, bu rol İlkesi ilişkilendirme yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme becerisini içerir.

* **Posta kutusu yönetici**: Bu rol yalnızca RIM Blackberry cihazlar için Exchange Online e-posta desteği bir parçası olarak kullanılır. Kuruluşunuz RIM Blackberry cihazlarda Exchange Online e-posta kullanmıyorsa, bu rolü kullanmayın.

* **İş ortağı Katman 1 destek**: kullanmayın. Bu rol, kullanım dışı bırakıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft satışı iş ortakları tarafından kullanılmaya yöneliktir ve genel kullanıma yönelik değildir.

* **İş ortağı Katman 2 Destek**: kullanmayın. Bu rol, kullanım dışı bırakıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft satışı iş ortakları tarafından kullanılmaya yöneliktir ve genel kullanıma yönelik değildir.

* **Parola Yöneticisi / Yardım Masası Yöneticisi**: Bu rolüne sahip kullanıcılar parolalarını sıfırlama, hizmet istekleri yönetebilir ve hizmetin sistem durumunu izleme. Parola yöneticileri yalnızca kullanıcılar ve diğer parola yöneticileri için sıfırlayabilir.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell, bu rolün "Yardım Masası Yönetici" olarak tanımlanır. "Parola yönetici" olarak [Azure portal](https://portal.azure.com/).
  >
  >
  
* **Power BI Hizmet Yöneticisi**: Bu rol ile kullanıcınız içinde Microsoft Power BI hizmeti mevcut olduğunda, genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Ayrıcalıklı Rol Yöneticisi**: Bu rolüne sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rolün tüm yönlerini Privileged Identity Management yönetilmesine izin verir.

* **Güvenlik Yöneticisi**: Bu rol ile kullanıcınız tüm güvenlik okuyucu rolüne yanı sıra güvenlikle ilgili hizmetler için yapılandırma yönetme olanağı salt okunur izinleri: Azure Active Directory kimlik koruması, Azure Information Protection, Privileged Identity Management ve Office 365 güvenlik ve Uyumluluk Merkezi. Office 365 izinleri hakkında daha fazla bilgi için bkz. [izinleri Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Güvenlik okuyucu**: kullanıcılar bu rol ile Azure Active Directory, kimlik koruması, Privileged Identity Management, aynı zamanda Özelliği Azure Active Directory oturum açma raporları okuma ve Denetim günlükleri için tüm bilgileri de dahil olmak üzere genel salt okunur erişim. Rol, ayrıca Office 365 güvenlik ve Uyumluluk Merkezi salt okunur izni verir. Office 365 izinleri hakkında daha fazla bilgi için bkz. [izinleri Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Hizmet desteği Yöneticisi**: Bu rolü olan kullanıcılar açabilir destek istekleri ile Microsoft Azure ve Office 365 Hizmetleri ve hizmet panosunu ve ileti merkezi Azure portal ve Office 365 Yönetici portalı görünümler için. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint Hizmet Yöneticisi**: Bu rol ile kullanıcınız Microsoft SharePoint hizmet mevcut olduğunda çevrimiçi içinde genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype Kurumsal / Lync Hizmet Yöneticisi**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Skype Kurumsal, genel izinlere sahip yanı sıra Azure Active Directory'de Skype özgü kullanıcı özniteliklerini yönetme. Ayrıca, bu rolü destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı verir. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell, bu rolün "Lync Hizmet Yöneticisi olarak" tanımlanır. "Skype için iş Hizmet Yöneticisi" olarak [Azure portal](https://portal.azure.com/).
  >
  >

* **Kullanıcı hesabı yönetici**: Bu rolüne sahip kullanıcılar oluşturabilir ve kullanıcıların ve grupların tüm yönlerini yönetebilirsiniz. Ayrıca, bu rolü, destek biletlerini yönetme becerisini içerir ve izleyicileri Sağlık Hizmeti. Bazı kısıtlamalar uygulanır. Örneğin, bu rol bir genel yönetici silinmesine izin vermiyor ve yönetici olmayanlar için parola değiştirme izin verirken, genel yöneticileri için parolaları veya diğer ayrıcalıklı yöneticileri değiştirilmesine izin vermez.

## <a name="administrator-permissions"></a>Yönetici izinleri

### <a name="billing-administrator"></a>Faturalama yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p>|

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Koşullu erişim ayarlarını yönet</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p>|

### <a name="global-administrator"></a>Genel yönetici
| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Kullanıcı parolalarını sıfırlama</p><p>Diğer yönetici parolalarını sıfırlama</p> <p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Etkinleştirmek veya çok faktörlü kimlik doğrulamasını devre dışı bırakma</p><p>Denetim günlüklerini görüntüleme</p> |Yok |

### <a name="password-administrator"></a>Parola yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Kullanıcı parolalarını sıfırlama</p> <p>Diğer yönetici parolalarını sıfırlama</p>|<p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Raporları görüntüleme</p>|

### <a name="service-administrator"></a>Hizmet yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p> |

### <a name="user-administrator"></a>Kullanıcı Yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Kısıtlamalarla kullanıcı parolalarını sıfırlayın.</p><p>Diğer yönetici parolalarını sıfırlama</p><p>Diğer kullanıcıların parolalarını sıfırlayın</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kısıtlamalarla kullanıcı lisanslarını yönetme. İsterse bir genel yöneticiyi silemez veya başka Yöneticiler oluşturamaz.</p> |<p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Etkinleştirmek veya çok faktörlü kimlik doğrulamasını devre dışı bırakma</p><p>Denetim günlüklerini görüntüleme</p> |

### <a name="security-reader"></a>Güvenlik okuyucusu
| İçinde | Yapabilirsiniz |
| --- | --- |
| Kimlik Koruma Merkezi |Tüm güvenlik raporları ve güvenlik özellikleri için ayarlar bilgilerini okuyun<ul><li>İstenmeyen postadan koruma<li>Şifreleme<li>Veri kaybını önleme<li>Kötü amaçlı yazılım<li>Gelişmiş tehdit koruması<li>Avından<li>Mailflow kuralları |
| Privileged Identity Management |<p>Sahip tüm bilgileri salt okunur erişimi ortaya Azure AD PIM: güvenlik ilkeleri ve Azure AD rol atamaları için raporları, gözden geçirir ve gelecekte senaryoları Azure AD rol ataması yanı sıra ilke veriler ve raporlar için erişimi'ni okuyun.<p>**Olamaz** Azure AD PIM için kaydolun veya herhangi bir değişiklik kolaylaştırır. Kullanıcı bunları aday ise PIM'ın portal veya PowerShell aracılığıyla, birisi Bu roldeki ek roller (örneğin, genel yönetici veya ayrıcalıklı Rol Yöneticisi) etkinleştirebilirsiniz. |
| <p>İzleyici Office 365 hizmeti durumu</p><p>Office 365 güvenlik ve Uyumluluk Merkezi</p> |<ul><li>Okuma ve Uyarıları yönetme<li>Güvenlik ilkelerini oku<li>Tehdit bilgileri, Cloud App Discovery ve arama ve Araştır Karantinadaki okuyun<li>Tüm raporları okuma |

### <a name="security-administrator"></a>Güvenlik Yöneticisi
| İçinde | Yapabilirsiniz |
| --- | --- |
| Kimlik Koruma Merkezi |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>Parola sıfırlama hariç tüm IPC işlemlerini gerçekleştirmek için ek olarak, yeteneği. |
| Privileged Identity Management |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>**Olamaz** Azure AD rol üyeliklerini veya ayarlarını yönetin. |
| <p>İzleyici Office 365 hizmeti durumu</p><p>Office 365 güvenlik ve Uyumluluk Merkezi |<ul><li>Güvenlik okuyucu rolünün tüm izinleri.<li>Tüm ayarlar için Gelişmiş tehdit Koruması özelliği (kötü amaçlı yazılım ve virüs koruması, kötü amaçlı URL yapılandırma, URL izleme, vb.) yapılandırabilirsiniz. |

## <a name="details-about-the-global-administrator-role"></a>Genel yönetici rolüne hakkındaki ayrıntıları
Genel yönetici tüm yönetim özelliklerine erişebilir. Varsayılan olarak, Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca küresel Yöneticiler diğer yönetici rollerini atayabilir.

### <a name="to-add-a-colleague-as-a-global-administrator"></a>Genel yönetici olarak bir iş arkadaşı eklemek için

1. Oturum [Azure Active Directory Yönetim Merkezi'ni](https://aad.portal.azure.com) Kiracı dizinini için genel yönetici olan bir hesapla.

   ![Azure AD Yönetim Merkezi açma](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Seçin **kullanıcılar ve gruplar &gt; tüm kullanıcılar**

3. Genel yönetici olarak belirleyin ve o kullanıcı için dikey penceresini açmak istediğiniz kullanıcıyı bulun.

4. Kullanıcı dikey penceresinde, seçin **dizin rolünü**.
 
5. Dizin rolü dikey penceresinde, seçin **genel yönetici** rolü ve kaydedin.

## <a name="assign-or-remove-administrator-roles"></a>Atama veya yönetici rolleri kaldırma
Azure Active Directory'de bir kullanıcıya yönetici rolleri atama hakkında bilgi edinmek için [kullanıcı Azure Active Directory Önizleme'te yönetici rolleri atama](active-directory-users-assign-role-azure-portal.md).

## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar edilmiş kullanım ve gelecekte Azure AD'den kaldırılacak.

* AdHoc Lisans Yöneticisi
* E-posta Adresi Doğrulanan Kullanıcı Oluşturucu
* Cihaz birleştirme
* Cihaz yöneticileri
* Aygıt kullanıcıları
* Cihazla Çalışma Alanına Katılma

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliğine yönelik olarak yöneticileri değiştirme hakkında daha fazla bilgi için bkz. [Azure yönetici rollerini ekleme veya değiştirme](../billing-add-change-azure-subscription-administrator.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](active-directory-understanding-resource-access.md)
* Azure Active Directory Azure aboneliğinize ilişkilendirilme şekli ile ilgili daha fazla bilgi için bkz: [Azure aboneliklerinin Azure Active Directory ile ilişkili](active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](active-directory-create-users.md)
* [Parolaları yönetme](active-directory-manage-passwords.md)
* [Grupları yönetme](active-directory-manage-groups.md)
