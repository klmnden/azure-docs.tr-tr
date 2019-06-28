---
title: Varsayılan kullanıcı izinleri - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'de bulunan farklı kullanıcı izinleri hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: lizross
ms.reviewer: vincesm
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 206e501860691cccc0578a0df4eec2b161b99b4c
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341360"
---
# <a name="what-are-the-default-user-permissions-in-azure-active-directory"></a>Varsayılan kullanıcı izinleri Azure Active Directory nelerdir?
Azure Active Directory'de (Azure AD) tüm kullanıcılara varsayılan olarak belirli izinler verilir. Bir kullanıcının erişim kullanıcı türünü oluşur, [rol atamaları](active-directory-users-assign-role-azure-portal.md)ve bunların tek tek nesnelerin sahipliğini. Bu makalede bu varsayılan izinler ve açıklanmakta ve üye ile konuk varsayılan değerleri karşılaştırılmaktadır. Varsayılan kullanıcı izinleri yalnızca kullanıcı ayarları, Azure AD'de değiştirilebilir.

## <a name="member-and-guest-users"></a>Üyeler ve konuk kullanıcılar
Alınan varsayılan izinler kümesini kullanıcının Kiracı (üye kullanıcı) yerel üyesi olup olmadığını veya kullanıcı başka bir dizinden B2B işbirliği Konuk (Konuk kullanıcı) olarak hazırlanmıştır varsa bağlıdır. Bkz: [Azure AD B2B işbirliği nedir?](../b2b/what-is-b2b.md) Konuk kullanıcı ekleme hakkında daha fazla bilgi için.
* Üye kullanıcılar uygulama kaydedebilir, kendi profil fotoğraflarını ve cep telefonu numaralarını yönetebilir, kendi parolalarını değiştirebilir ve B2B konuklarını davet edebilirler. Ayrıca kullanıcılar dizindeki tüm bilgileri okuyabilir (birkaç özel durum haricinde). 
* Konuk kullanıcılar sınırlı dizin izinlere. Örneğin konuk kullanıcılar kendi profil bilgileri dışındaki kiracı bilgilerine ulaşamaz. Ancak konuk kullanıcılar Kullanıcı Asıl Adı veya nesne kimliği kullanarak diğer kullanıcılar hakkında bilgi alabilir. Konuk kullanıcı Grup ait oldukları için Grup üyeliği de dahil olmak üzere açmamasından özelliklerini okuyabilir **konuk kullanıcıların izinleri sınırlıdır** ayarı. Konuk, diğer bir kiracı nesneler hakkındaki bilgileri görüntüleyemezsiniz.

Konuklar için varsayılan izinler varsayılan olarak kısıtlanmıştır. Konuklar yönetici rollerine eklenerek rol kapsamındaki tüm yazma ve okuma izinlerine erişmeleri sağlanabilir. Konuklara uygulanabilen diğer bir kısıtlama da başka konukları davet etme özelliğidir. **Konuklar davet edebilir** ayarını **Hayır** olarak belirlerseniz konuklar başka konuk davet edemez. Nasıl yapılacağını öğrenmek için bkz. [B2B işbirliği için davetiye temsilcisi seçme](../b2b/delegate-invitations.md). Konuk kullanıcılara varsayılan olarak üye kullanıcılarla aynı izinleri vermek için **Konuk kullanıcıların izinleri sınırlıdır** ayarını **Hayır** olarak belirleyin. Bu ayar varsayılan olarak konuk kullanıcılara tüm üye kullanıcı izinlerini vermenin yanı sıra konuklara yönetici rollerinin verilmesini sağlar.

## <a name="compare-member-and-guest-default-permissions"></a>Üye ve konuk varsayılan izinlerini karşılaştırma

**Alan** | **Üye kullanıcı izinleri** | **Konuk kullanıcı izinleri**
------------ | --------- | ----------
Kullanıcılar ve kişiler | Kullanıcıların ve kişilerin tüm genel özelliklerini okuma<br>Konuk davet etme<br>Kendi parolasını değiştirme<br>Kendi cep telefonu numarasını yönetme<br>Kendi fotoğrafını yönetme<br>Kendi yenileme belirteçlerini geçersiz kılma | Kendi özelliklerini okuma<br>Görünen ad okuyun, e-posta, ad, fotoğraf, kullanıcı asıl adı ve kullanıcı türü özelliklerinin diğer kullanıcıları ve kişileri oturum<br>Kendi parolasını değiştirme
Gruplar | Güvenlik grubu oluşturma<br>Office 365 grubu oluşturma<br>Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen grup için gizli Office 365 grup üyeliklerini okuma<br>Özellikler, sahipliği ve kullanıcının sahip olduğu grupları üyeliğini Yönet<br>Sahip olunan gruplara konuk ekleme<br>Dinamik üyelik ayarlarını yönetme<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme | Grupların tüm özelliklerini okuma<br>Gizli olmayan grup üyeliklerini okuma<br>Birleştirilen gruplar için gizli Office 365 grup üyeliklerini okuma<br>Sahip olunan grupları yönetme<br>Sahip olunan gruplara konuk ekleme (izin veriliyorsa)<br>Sahip olunan grupları silme<br>Sahip olunan Office 365 gruplarını geri yükleme<br>İçin üyeliği de dahil olmak üzere ait oldukları gruplar özelliklerini okuyun.
Uygulamalar | Yeni uygulama kaydetme (oluşturma)<br>Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Kullanıcı için uygulama parolasını oluşturma veya silme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme | Kayıtlı ve kurumsal uygulamaların özelliklerini okuma<br>Sahip olunan uygulamaların uygulama özelliklerini, atamalarını ve kimlik bilgilerini yönetme<br>Sahip olunan uygulamaları silme<br>Sahip olunan uygulamaları geri yükleme
Cihazlar | Cihazların tüm özelliklerini okuma<br>Sahip olunan cihazların tüm özelliklerini yönetme<br> | İzin yok<br>Sahip olunan cihazları silme<br>
Dizin | Tüm şirket bilgilerini okuma<br>Tüm etki alanlarını okuma<br>Tüm iş ortağı sözleşmelerini okuma | Görünen ad ve onaylı etki alanlarını okuma
Roller ve Kapsamlar | Tüm yönetim rollerini ve üyelikleri okuma<br>Tüm yönetim birimlerinin özelliklerini ve üyeliklerini okuma | İzin yok 
Abonelikler | Tüm abonelikleri okuma<br>Hizmet Planı Üyesini etkinleştirme | İzin yok
İlkeler | İlkelerin tüm özelliklerini okuma<br>Sahip olunan ilkelerin tüm özelliklerini yönetme | İzin yok

## <a name="to-restrict-the-default-permissions-for-member-users"></a>Üye kullanıcıların varsayılan izinlerini kısıtlama

Üye kullanıcıların varsayılan izinleri aşağıdaki yöntemlerle kısıtlanabilir.

İzin | Ayar açıklaması
---------- | ------------
Kullanıcılar uygulama kaydedebilir | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar uygulama kayıtları oluşturmasını engeller. Özelliği, uygulama geliştiricisi rolüne ekleyerek belirli kişilere ardından verilebilir.
Kullanıcıların iş veya Okul hesabını Linkedın'e bağlanmasına izin ver | Bu seçenek Hayır olarak ayarlanırsa, kullanıcılar uygulamaya iş veya Okul hesabı, LinkedIn hesabıyla bağlanmasını engeller. Daha fazla bilgi için [LinkedIn hesabı bağlantıları veri paylaşımı ve onay](https://docs.microsoft.com/azure/active-directory/users-groups-roles/linkedin-user-consent).
Güvenlik grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar güvenlik grubu oluşturamaz. Hala genel Yöneticiler ve kullanıcı yöneticileri güvenlik grupları oluşturabilirsiniz. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Office 365 grubu oluşturma olanağı | Bu seçenek Hayır olarak ayarlanırsa kullanıcılar Office 365 grubu oluşturamaz. Bu seçeneğin Bazı olarak ayarlanması belirli bir kullanıcı kümesinin Office 365 grubu oluşturmasını sağlar. Genel Yöneticiler ve kullanıcı yine de Office 365 grupları oluşturmak oluşturabileceksiniz. Nasıl yapılacağını öğrenmek için bkz. [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](../users-groups-roles/groups-settings-cmdlets.md).
Azure AD yönetim portalına erişimi sınırlayın | Bu seçenek Evet olarak ayarlandığında, kullanıcıların Azure Active Directory yalnızca Azure portalı üzerinden erişmesini engeller.
Diğer kullanıcıları okuma olanağı | Bu ayar yalnızca PowerShell ile kullanılabilir. $False olarak bu bayrak ayarlandığında, yönetici olmayan tüm kullanıcı bilgilerini dizinden okumasını önler. Bu bayrak Engellemiyor Exchange Online gibi diğer Microsoft Hizmetleri içindeki kullanıcı bilgileri okunuyor. Bu ayar, özel durumlar için tasarlanmıştır ve bu bayrağı ayarlamanın $false olarak önerilmez.

## <a name="object-ownership"></a>Nesne sahipliği

### <a name="application-registration-owner-permissions"></a>Uygulama kaydı sahibi izinleri
Uygulamayı kaydeden kullanıcılar otomatik olarak uygulamanın sahibi olarak eklenir. Bu uygulama sahipleri ad ve uygulama isteği izinleri gibi uygulama meta verilerini yönetebilir. Bu kullanıcılar ayrıca SSO yapılandırması ve kullanıcı atamaları gibi kiracıya özgü uygulama yapılandırmalarını da yönetebilir. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticilerden farklı olarak sahipler yalnızca kendilerine ait uygulamaları yönetebilir.

### <a name="enterprise-application-owner-permissions"></a>Kurumsal uygulama sahibi izinleri
Bir kullanıcı yeni bir kuruluş uygulaması eklediğinde, bunlar bir sahibi olarak otomatik olarak eklenir. Sahip olarak, bunlar uygulama SSO yapılandırma, sağlama ve kullanıcı atamalarını gibi kiracıya özgü yapılandırmasını yönetebilirsiniz. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Genel Yöneticiler, sahipleri yalnızca oldukları uygulamaları yönetebilir.

### <a name="group-owner-permissions"></a>Grup sahibi izinleri
Grup oluşturan kullanıcılar otomatik olarak o grubun sahibi olur. Sahip olarak, bunlar grubunun adı gibi özellikleri yönetmenize, yapabilir grup üyeliğini yönetme. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Sahipleri, yalnızca genel Yöneticiler ve kullanıcı yöneticileri aksine, sahip olduğu grupları yönetebilirsiniz. Grup sahibi atamak için bkz. [Grup sahiplerini yönetme](active-directory-accessmanagement-managing-group-owners.md).

### <a name="ownership-permissions"></a>Sahipliği izinleri
Aşağıdaki tablolarda, üye kullanıcı sahip olunan nesneler üzerinde sahip Azure Active Directory'de özel izinler açıklanmaktadır. Kullanıcı, sahip oldukları nesneleri yalnızca bu izinlere sahiptir.

#### <a name="owned-application-registrations"></a>Sahibi uygulama kayıtları
Kullanıcılar sahibi uygulama kayıtları aşağıdaki eylemleri gerçekleştirebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| Microsoft.Directory/Applications/Audience/Update | Azure Active Directory'de Applications.Audience özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/Authentication/Update | Azure Active Directory'de Applications.Authentication özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/Basic/Update | Uygulamaları Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.Directory/Applications/credentials/Update | Azure Active Directory'de Applications.credentials özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/DELETE | Azure Active Directory'de uygulamaları silin. |
| Microsoft.Directory/Applications/Owners/Update | Azure Active Directory'de Applications.Owners özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/Permissions/Update | Azure Active Directory'de Applications.Permissions özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/Policies/Update | Azure Active Directory'de Applications.Policies özelliğini güncelleştirin. |
| Microsoft.Directory/Applications/Restore | Azure Active Directory'de uygulamalar geri yükleyin. |

#### <a name="owned-enterprise-applications"></a>Sahip olunan kurumsal uygulamalar
Kullanıcılar, sahip olunan kurumsal uygulamalar üzerinde aşağıdaki eylemleri gerçekleştirebilir. Kurumsal bir uygulamanın hizmet sorumlusu, bir veya daha fazla uygulama ilkeleri ve bazen bir uygulama nesnesi aynı kiracıda hizmet sorumlusu olarak oluşur.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/auditLogs/allProperties/read | Azure Active Directory'de bulunan tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |
| Microsoft.Directory/Policies/Basic/Update | İlkeleri Azure Active Directory'de temel özelliklerini güncelleştirin. |
| Microsoft.Directory/Policies/DELETE | Azure Active Directory ilkeleri silin. |
| Microsoft.Directory/Policies/Owners/Update | Azure Active Directory'de policies.Owners özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory'de servicePrincipals.appRoleAssignedTo özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory'de users.appRoleAssignments özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/audience/update | Azure Active Directory'de servicePrincipals.audience özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/authentication/update | Azure Active Directory'de servicePrincipals.authentication özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/basic/update | Azure Active Directory'de servicePrincipals temel özelliklerini güncelleştirin. |
| microsoft.directory/servicePrincipals/credentials/update | Azure Active Directory'de servicePrincipals.credentials özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/delete | Azure Active Directory'de servicePrincipals silin. |
| microsoft.directory/servicePrincipals/owners/update | Azure Active Directory'de servicePrincipals.owners özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/permissions/update | Azure Active Directory'de servicePrincipals.permissions özelliğini güncelleştirin. |
| microsoft.directory/servicePrincipals/policies/update | Azure Active Directory'de servicePrincipals.policies özelliğini güncelleştirin. |
| microsoft.directory/signInReports/allProperties/read | Azure Active Directory'de signInReports tüm özelliklerde (Ayrıcalıklı özellikleri dahil) okuyun. |

#### <a name="owned-devices"></a>Şirkete ait cihazlar
Kullanıcıların, şirkete ait cihazlarda aşağıdaki eylemleri gerçekleştirebilirsiniz.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory'de devices.bitLockerRecoveryKeys özelliği okuyun. |
| Microsoft.Directory/Devices/disable | Cihazları Azure Active Directory'de devre dışı bırakın. |

#### <a name="owned-groups"></a>Sahip olunan gruplar
Kullanıcılar, sahip olunan gruplar aşağıdaki eylemleri gerçekleştirebilir.

| **Eylemler** | **Açıklama** |
| --- | --- |
| microsoft.directory/groups/appRoleAssignments/update | Azure Active Directory'de groups.appRoleAssignments özelliğini güncelleştirin. |
| Microsoft.Directory/Groups/Basic/Update | Azure Active Directory'de grupları temel özelliklerini güncelleştirin. |
| Microsoft.Directory/Groups/DELETE | Azure Active Directory içinde grupları silin. |
| microsoft.directory/groups/dynamicMembershipRule/update | Azure Active Directory'de groups.dynamicMembershipRule özelliğini güncelleştirin. |
| Microsoft.Directory/Groups/Members/Update | Azure Active Directory'de Groups.Members özelliğini güncelleştirin. |
| Microsoft.Directory/Groups/Owners/Update | Azure Active Directory'de Groups.Owners özelliğini güncelleştirin. |
| Microsoft.Directory/Groups/Restore | Azure Active Directory'de gruplar geri yükleyin. |
| Microsoft.Directory/Groups/Settings/Update | Azure Active Directory'de Groups.Settings özelliğini güncelleştirin. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure AD yönetici rollerini atama hakkında daha fazla bilgi için bkz: [bir kullanıcı Azure Active Directory'de yönetici rolleri atama](active-directory-users-assign-role-azure-portal.md)
* Microsoft Azure'da kaynak erişiminin nasıl denetlendiği konusunda daha fazla bilgi için bkz. [Azure'da kaynak erişimini anlama](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure Active Directory ile Azure aboneliğinizin arasındaki ilişki hakkında bilgi için bkz. [Azure aboneliklerinin Azure Active Directory ile ilişkisi](active-directory-how-subscriptions-associated-directory.md)
* [Kullanıcıları yönetme](add-users-azure-active-directory.md)
