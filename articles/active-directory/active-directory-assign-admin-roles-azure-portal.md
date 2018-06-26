---
title: Azure Active Directory'de yönetici rolleri atama | Microsoft Docs
description: Yönetici rolü kullanıcı ekleme, yönetici rolleri atamak, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme veya etki alanlarını yönetme. Yönetici rolü atanan bir kullanıcı, kuruluşunuzun abone olduğu tüm bulut hizmetleri arasında aynı izinleri vardır.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 1de2482b7795bbed82874b6eea29f89f1ff52560
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938508"
---
# <a name="assigning-administrator-roles-in-azure-active-directory"></a>Azure Active Directory’de yönetici rolü atama

Azure Active Directory (Azure AD) kullanarak, farklı işlevler hizmet için ayrı Yöneticiler belirleyebilirsiniz. Yöneticiler Azure portalında ve rollerine bağlı olarak çeşitli özelliklerine erişime sahiptir, oluşturabilir veya kullanıcıların Düzenle, başkalarına yönetici rolleri atamak, kullanıcı parolalarını sıfırlama, kullanıcı lisanslarını yönetme ve başka şeylerin etki alanlarını yönetme. Yönetici rolü atanan bir kullanıcı için kuruluşunuzun için rolü Office 365 portalında veya Azure portalında veya için Azure AD modülünü kullanarak atamış olup abone olduğu bulut Hizmetleri tümüne aynı izinlere sahip Windows PowerShell.

## <a name="details-about-the-global-administrator-role"></a>Genel yönetici rolüne hakkındaki ayrıntıları
Genel yönetici tüm yönetim özelliklerine erişebilir. Varsayılan olarak, Azure aboneliği için kaydolan kişi dizin için genel Yönetici rolüne atanır. Yalnızca küresel Yöneticiler diğer yönetici rollerini atayabilir.

## <a name="assign-or-remove-administrator-roles"></a>Atama veya yönetici rolleri kaldırma
Azure Active Directory'de bir kullanıcıya yönetici rolleri atama hakkında bilgi edinmek için [kullanıcı Azure Active Directory'de yönetici rolleri atama](fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="available-roles"></a>Kullanılabilir roller
Aşağıdaki yönetici rolleri kullanılabilir:

* **Uygulama Yöneticisi**: Bu roldeki kullanıcılar oluşturabilir ve kurumsal uygulamalar, uygulama kayıtlar ve uygulama proxy ayarları tüm yönlerini yönetebilirsiniz. Bu rolü de izinlere temsilci ve Microsoft Graph ve Azure AD grafik hariç olmak üzere uygulama izinleri onayı olanağı verir. Bu rolün üyeleri, yeni uygulama kayıtlar veya kuruluş uygulamalarında oluştururken sahipleri olarak eklenmez.

* **Uygulama geliştiricisi**: Bu roldeki kullanıcılar, uygulama kayıtlar oluşturabilir, "Kullanıcılar uygulamaları kaydolabilir" ayarı Hayır olarak ayarlayın Bu rolü üyelerinin kendi adınıza onayı için de sağlar, "Kullanıcılar izin şirket adına şirket verilerine erişen uygulamaları" ayarı Hayır olarak ayarlayın Bu rolün üyeleri, yeni uygulama kayıtlar veya kuruluş uygulamalarında oluştururken sahipleri olarak eklenir.

* **Faturalama Yöneticisi**: satın alma işlemleri yapar, abonelikleri yönetir, destek biletlerini yönetir ve hizmetin sistem durumunu izler.

* **Bulut Uygulama Yöneticisi**: Bu roldeki kullanıcılar uygulama proxy'si yönetme olanağı hariç olmak üzere uygulama Yönetici rolü aynı izinlere sahip. Bu rolü oluşturmak ve kurumsal uygulamalar ve uygulama kayıtlar tüm yönlerini yönetmek için yeteneği verir. Bu rolü de izinlere temsilci ve Microsoft Graph ve Azure AD grafik hariç olmak üzere uygulama izinleri onayı olanağı verir. Bu rolün üyeleri, yeni uygulama kayıtlar veya kuruluş uygulamalarında oluştururken sahipleri olarak eklenmez.

* **Uyumluluk Yöneticisi**: Bu rolüne sahip kullanıcılar Office 365 güvenlik ve Uyumluluk Merkezi ve Exchange yönetici merkezini içinde yönetim izinlerine sahip. Daha fazla bilgi "[Office 365 Yönetici rolleri hakkında](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)."

* **Koşullu Erişim Yöneticisi**: kullanıcılar bu rol ile Azure Active Directory koşullu erişim ayarlarını yönetme olanağı.
  > [!NOTE]
  > Exchange ActiveSync koşullu erişim ilkesi azure'da dağıtmak için kullanıcının da genel yönetici olması gerekir.
  
* **Cihaz yöneticileri**: Bu rolün yalnızca içinde ek bir yerel yönetici olarak atamasını kullanılabilir [aygıt ayarları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Bu role sahip kullanıcılar yerel makine Yöneticiler için Azure Active Directory'ye katılmış tüm Windows 10 cihazlarda haline gelir. Azure Active Directory'de cihaz nesnelerini yönetme yeteneği sahip değil.

* **Dizin okuyucular**: Bu desteklemeyen uygulamalar için atanmış olan, eski bir rolüdür [onayı Framework](active-directory-integrating-applications.md). Tüm kullanıcılara atanmamalıdır.

* **Dizin eşitleme hesapları**: kullanmayın. Bu rol Azure AD Connect hizmetine otomatik olarak atanır ve değil kullanılmaya veya diğer kullanım için desteklenir.

* **Directory yazıcılarının**: Bu desteklemeyen uygulamalar için atanmış olan, eski bir rolüdür [onayı Framework](active-directory-integrating-applications.md). Tüm kullanıcılara atanmamalıdır.

* **Dynamics 365 Yönetici**: Bu rol ile kullanıcınız içinde Microsoft Dynamics 365 hizmet mevcut olduğunda, genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Exchange Hizmet Yöneticisi**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Exchange Online içinde genel izinlere sahiptir. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Genel yönetici / şirket Yöneticisi / Kiracı Yöneticisi**: kullanıcılar bu rol ile Exchange Online gibi Azure Active Directory'ye birleştirmek hizmetlerinin yanı sıra Azure Active Directory, tüm yönetim özelliklerine erişimi SharePoint Online ve Skype Kurumsal çevrimiçi sürüm. Azure Active Directory Kiracı için kaydolan kişi genel yönetici olur. Yalnızca küresel Yöneticiler diğer yönetici rollerini atayabilir. Şirketinizde birden fazla genel yönetici olabilir. Genel yönetici parolası herhangi bir kullanıcı ve diğer tüm yöneticilere sıfırlayabilirsiniz.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell Bu rolün "Şirket Yönetici" olarak tanımlanır. "Genel yönetici" olarak [Azure portal](https://portal.azure.com).
  >
  >

* **Konuk davet eden**: Bu roldeki kullanıcılar, Azure Active Directory B2B Konuk kullanıcı davetleri yönetebilir, "Üyeleri davet edebilirsiniz" Kullanıcı ayarı Hayır olarak ayarlandığında geçerlidir B2B işbirliğinin hakkında daha fazla bilgi [hakkında Azure AD B2B işbirliği](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Diğer izinler içermez.

* **Bilgi Koruma Yöneticisi**: Bu rolüne sahip kullanıcılar Azure Information Protection hizmetini yalnızca kullanıcı haklarına sahip. Kullanıcı haklarını kimlik koruma Merkezi, Privileged Identity Management, İzleyici Office 365 hizmet durumu veya Office 365 güvenlik ve Uyumluluk Merkezi izin verilmez. Azure Information Protection ilkesi için etiket yapılandırma, koruma şablonlarını yönetme ve koruma etkinleştirin.

* **Intune hizmet yöneticisinin**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Intune çevrimiçi içinde genel izinlere sahiptir. Ayrıca, bu rol İlkesi ilişkilendirme yanı sıra grupları oluşturmak ve yönetmek için kullanıcıları ve cihazları yönetme becerisini içerir.

* **Posta kutusu yönetici**: Bu rol yalnızca RIM Blackberry cihazlar için Exchange Online e-posta desteği bir parçası olarak kullanılır. Kuruluşunuz RIM Blackberry cihazlarda Exchange Online e-posta kullanmıyorsa, bu rolü kullanmayın.

* **İleti Merkezi okuyucu**: Bu roldeki kullanıcılar bildirimleri ve danışma sistem güncelleştirmelerini izleyebilirsiniz [Office 365 ileti Merkezi](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) kuruluşlarında Exchange, Intune ve Microsoft gibi yapılandırılmış hizmetleri Ekipler. İleti Merkezi okuyucular Haftalık e-posta özetler posta, güncelleştirmeleri almak ve Office 365 ileti merkezi gönderilerde paylaşabilirsiniz. Azure AD'de bu role atanan kullanıcılar yalnızca salt okunur erişim Azure AD hizmetlerinde kullanıcılar ve gruplar gibi üzerinde sahip olur. 

* **İş ortağı Katman 1 destek**: kullanmayın. Bu rol, kullanım dışı bırakıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft satışı iş ortakları tarafından kullanılmaya yöneliktir ve genel kullanıma yönelik değildir.

* **İş ortağı Katman 2 Destek**: kullanmayın. Bu rol, kullanım dışı bırakıldı ve gelecekte Azure AD'den kaldırılacak. Bu rol, az sayıda Microsoft satışı iş ortakları tarafından kullanılmaya yöneliktir ve genel kullanıma yönelik değildir.

* **Parola Yöneticisi / Yardım Masası Yöneticisi**: Bu rolüne sahip kullanıcılar parolalarını değiştirme, hizmet istekleri yönetebilir ve hizmetin sistem durumunu izleme. Yardım Masası yöneticileri yalnızca kullanıcılar ve diğer Yardım Masası yöneticileri için parolaları değiştirebilirsiniz. 

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell, bu rolün "Yardım Masası Yönetici" olarak tanımlanır. "Parola yönetici" olarak [Azure portal](https://portal.azure.com/).
  >
  >
  
* **Power BI Hizmet Yöneticisi**: Bu rol ile kullanıcınız içinde Microsoft Power BI hizmeti mevcut olduğunda, genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Ayrıcalıklı Rol Yöneticisi**: Bu rolüne sahip kullanıcılar, Azure Active Directory'de yanı sıra Azure AD Privileged Identity Management içinde rol atamalarını yönetebilir. Ayrıca, bu rolün tüm yönlerini Privileged Identity Management yönetilmesine izin verir.

* **Raporları okuyucu**: Bu rolüne sahip kullanıcılar, kullanım verileri ve Office 365 Yönetim Merkezi ve benimseme içerik raporları panosunda pack içinde Powerbı raporu görüntüleyebilirsiniz. Ayrıca, rol için oturum açma erişim sağlayan raporlar ve Azure AD etkinliğinde ve Microsoft Graph tarafından döndürülen veri raporlama API. Raporları okuyucu rolüne atanmış bir kullanıcı yalnızca ilgili kullanım ve benimseme ölçümleri erişebilir. Bunlar ayarları veya Exchange gibi ürün belirli Yönetim Merkezleri erişimi yapılandırmak için tüm yönetim izinleri yok. 

* **Güvenlik Yöneticisi**: Bu rol ile kullanıcınız tüm güvenlik okuyucu rolüne yanı sıra güvenlikle ilgili hizmetler için yapılandırma yönetme olanağı salt okunur izinleri: Azure Active Directory kimlik koruması, Azure Information Protection, ayrıcalıklı kimlik yönetimi ve Office 365 güvenlik ve Uyumluluk Merkezi. Office 365 izinleri hakkında daha fazla bilgi için bkz. [izinleri Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Güvenlik okuyucu**: kullanıcılar bu rol ile Azure Active Directory, kimlik koruması, Privileged Identity Management, aynı zamanda Azure Active Directory oturum açma raporları okuma özelliği tüm bilgileri de dahil olmak üzere genel salt okunur erişim ve Denetim günlükleri. Rol, ayrıca Office 365 güvenlik ve Uyumluluk Merkezi salt okunur izni verir. Office 365 izinleri hakkında daha fazla bilgi için bkz. [izinleri Office 365 güvenlik ve Uyumluluk Merkezi](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Hizmet desteği Yöneticisi**: Bu rolü olan kullanıcılar açabilir destek istekleri ile Microsoft Azure ve Office 365 Hizmetleri ve hizmet panosunu ve ileti merkezi Azure portal ve Office 365 Yönetici portalı görünümler için. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint Hizmet Yöneticisi**: Bu rol ile kullanıcınız Microsoft SharePoint hizmet mevcut olduğunda çevrimiçi içinde genel izinleri yanı sıra, destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype Kurumsal / Lync Hizmet Yöneticisi**: Bu rolü olan kullanıcılar hizmet mevcut olduğunda Microsoft Skype Kurumsal, genel izinlere sahip yanı sıra Azure Active Directory'de Skype özgü kullanıcı özniteliklerini yönetme. Ayrıca, bu rolü destek biletlerini yönetme ve hizmet sistem durumu izleme olanağı verir. Daha fazla bilgi [hakkında Office 365 Yönetici rollerine](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API ve Azure AD PowerShell, bu rolün "Lync Hizmet Yöneticisi olarak" tanımlanır. "Skype için iş Hizmet Yöneticisi" olarak [Azure portal](https://portal.azure.com/).
  >
  >

* **Kullanıcı hesabı yönetici**: Bu rolüne sahip kullanıcılar oluşturabilir ve kullanıcıların ve grupların tüm yönlerini yönetebilirsiniz. Ayrıca, bu rolü, destek biletlerini yönetme becerisini içerir ve izleyicileri Sağlık Hizmeti. Bazı kısıtlamalar uygulanır. Örneğin, bu rol bir genel yönetici silinmesine izin vermiyor değil. Kullanıcı hesap yöneticileri, kullanıcıların, Yardım Masası yöneticileri ve yalnızca diğer kullanıcı hesabı yöneticileri için parolaları değiştirebilirsiniz.

## <a name="administrator-permissions"></a>Yönetici izinleri

### <a name="application-administrator"></a>Uygulama Yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
| Tüm dizin bilgilerinin okuma<br>Uygulama kaydı oluşturun<br>Uygulama kayıt özelliklerini güncelleştir<br>Kuruluş uygulamaları alma<br>Uygulama kayıt izinleri yönetme<br>Uygulama kayıtları silme<br>Kurumsal uygulama tek oturum açma ayarlarını yönet<br>Kurumsal uygulama ayarlarını sağlama yönetme<br>Self Servis Kurumsal uygulama ayarlarını yönetme<br>Kurumsal uygulama izin ayarlarını yönetme<br>Uygulama erişimini yönetme<br>Sağlama ayarlarını yönet<br>Kuruluş uygulamaları silin<br>Herkesin tüm temsilci izni isteklerini adına onayı<br>Herkes için Azure AD grafik veya Microsoft Graph dışındaki tüm uygulama izni isteklerini adına onayı<br>Uygulama proxy ayarlarını yönet<br>Erişim Hizmetleri ayarları<br>İzleme hizmeti durumu<br>Destek biletlerini yönetme<br>Gizli okuma grup üyeliği | Oluşturma, düzenleme ve grupları silme<br>Kullanıcı lisanslarını yönetme<br>Dizin eşitleme kullanma<br>Oturum açma raporları görüntülemek ve Denetim günlükleri | 

### <a name="application-developer"></a>Uygulama geliştiricisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
| Tüm dizin bilgilerinin okuma<br>Uygulama kaydı oluşturun<br>Kendi adına onayı | Oturum açma görüntülemek ve Denetim günlükleri<br>Gizli okuma grup üyeliği |

### <a name="billing-administrator"></a>Faturalama Yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p> |

### <a name="cloud-application-administrator"></a>Bulut Uygulama Yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
| Tüm dizin bilgilerinin okuma<br>Uygulama kaydı oluşturun<br>Uygulama kayıt özelliklerini güncelleştir<br>Kuruluş uygulamaları alma<br>Uygulama kayıt izinleri yönetme<br>Uygulama kayıtları silme<br>Kurumsal uygulama tek oturum açma ayarlarını yönet<br>Kurumsal uygulama ayarlarını sağlama yönetme<br>Self Servis Kurumsal uygulama ayarlarını yönetme<br>Kurumsal uygulama izin ayarlarını yönetme<br>Uygulama erişimini yönetme<br>Sağlama ayarlarını yönet<br>Kuruluş uygulamaları silin<br>Herkesin tüm temsilci izni isteklerini adına onayı<br>Herkes için Azure AD grafik veya Microsoft Graph dışındaki tüm uygulama izni isteklerini adına onayı<br>Erişim Hizmetleri ayarları<br>İzleme hizmeti durumu<br>Destek biletlerini yönetme<br>Gizli okuma grup üyeliği | Uygulama proxy ayarlarını yönet<br>Oluşturma, düzenleme ve grupları silme<br>Kullanıcı lisanslarını yönetme<br>Dizin eşitleme kullanma<br>Oturum açma raporları görüntülemek ve Denetim günlükleri |

### <a name="conditional-access-administrator"></a>Koşullu Erişim Yöneticisi

| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Koşullu erişim ayarlarını yönet</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p>|

### <a name="global-administrator"></a>Genel yönetici
| Yapabilirsiniz | Yapamaz |
| --- | --- |
|<p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Kullanıcı parolalarını sıfırlama</p><p>Diğer yöneticiler parolalarını sıfırlama</p> <p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Etkinleştirmek veya çok faktörlü kimlik doğrulamasını devre dışı bırakma</p><p>Denetim günlüklerini görüntüleme</p> |Yok |

### <a name="password-administrator--helpdesk-administrator"></a>Parola Yöneticisi / Yardım Masası Yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Kullanıcılar ve diğer Yardım Masası yöneticileri için parolaları değiştirme</p>|<p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Raporları görüntüleme</p>|

### <a name="information-protection-administrator"></a>Bilgi Koruma Yöneticisi
İçinde | Yapabilirsiniz
-------- | ---------
Azure Information Protection | <li>Genel ve kapsamlı ilkelerinde etiketleri ve ayarlarını yapılandırma<li>Yapılandırma ve koruma şablonlarını yönetme<li>Etkinleştirme veya devre dışı koruması
 
### <a name="reports-reader"></a>Raporları okuyucusu 
Yapabilirsiniz | Yapamaz
------ | ----------
Azure AD oturum açma raporları ve Denetim günlükleri görüntüle<br>Şirket ve kullanıcı bilgilerini görüntüleme<br>Erişim Office 365 kullanım Panosu | Oluşturma ve kullanıcı görünümleri yönetme<br>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme<br>Başkalarını yönetici rollerine temsilci seçme<br>Şirket bilgilerini yönetme

### <a name="security-reader"></a>Güvenlik Okuyucu
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

### <a name="service-administrator"></a>Hizmet Yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p> |<p>Kullanıcı parolalarını sıfırlama</p><p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kullanıcı lisanslarını yönetme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Denetim günlüklerini görüntüleme</p> |

### <a name="user-account-administrator"></a>Kullanıcı Hesabı Yöneticisi
| Yapabilirsiniz | Yapamaz |
| --- | --- |
| <p>Şirket ve kullanıcı bilgilerini görüntüleme</p><p>Office destek biletlerini yönetme</p><p>Kullanıcılar, Yardım Masası Yöneticiler ve yalnızca diğer kullanıcı hesabı yöneticileri için parolaları değiştirme</p><p>Oluşturma ve kullanıcı görünümleri yönetme</p><p>Oluşturmak, düzenlemek ve kullanıcıları ve grupları silme ve kısıtlamalarla kullanıcı lisanslarını yönetme. İsterse bir genel yöneticiyi silemez veya başka Yöneticiler oluşturamaz.</p> |<p>Office ürünlerinin faturalama ve satın alma işlemleri gerçekleştirme</p><p>Etki alanlarını yönetme</p><p>Şirket bilgilerini yönetme</p><p>Başkalarını yönetici rollerine temsilci seçme</p><p>Dizin eşitleme kullanma</p><p>Etkinleştirmek veya çok faktörlü kimlik doğrulamasını devre dışı bırakma</p><p>Denetim günlüklerini görüntüleme</p> |

### <a name="to-add-a-user-as-a-global-administrator"></a>Genel yönetici olarak bir kullanıcı eklemek için

1. Oturum [Azure Active Directory Yönetim Merkezi'ni](https://aad.portal.azure.com) Kiracı dizinini için genel yönetici olan bir hesapla.

   ![Azure AD Yönetim Merkezi açma](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Seçin **kullanıcılar** > **tüm kullanıcılar**.

3. Genel yönetici olarak atamak istediğiniz kullanıcı sayfasını açın.

4. Komut çubuğunda seçin **dizin rolünü**.

5. Seçin **Rol Ekle**.

6. Dizin rolü sayfasında seçin **genel yönetici** rolünü ve ardından **seçin** kaydetmek için.

## <a name="deprecated-roles"></a>Kullanım dışı rolleri

Aşağıdaki roller kullanılmamalıdır. Bunlar kullanım dışıdır ve gelecekteki Azure AD'den kaldırılacak.

* Geçici Lisans Yöneticisi
* E-postayla Doğrulanan Kullanıcı Oluşturucu
* Cihaz birleştirme
* Cihaz yöneticileri
* Aygıt kullanıcıları
* Çalışma alanına cihaz katılma

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure aboneliği yöneticileri değiştirme hakkında daha fazla bilgi için bkz: [ekleme veya değiştirme Azure aboneliği yöneticileri](../billing-add-change-azure-subscription-administrator.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](active-directory-create-users.md)
* [Parolaları yönetme](active-directory-manage-passwords.md)
* [Grupları yönetme](fundamentals/active-directory-manage-groups.md)
