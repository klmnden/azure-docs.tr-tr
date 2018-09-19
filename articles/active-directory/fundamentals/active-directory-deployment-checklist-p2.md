---
title: Azure AD dağıtım denetim listesi 30 gün sonra 90 gün ve sonraki süreci desteleyen
description: Azure Active Directory Premium P2 özellik dağıtım denetim listesi
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: ''
ms.openlocfilehash: 7931cd8a6f8b3de826e8dd563a837f80fc15d88a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315122"
---
# <a name="azure-active-directory-premium-p2-licensing-feature-checklist"></a>Azure Active Directory Premium P2 lisans özelliği denetim listesi

Kuruluşunuz için Azure Active Directory (Azure AD) dağıtma ve güvenli tutmaya göz korkutucu görünebilir. Bu makalede bazı genel görevleri tanımlar müşterilere 30 gün sonra 90 gün boyunca tamamlanması veya sonrasını kendi güvenlik duruşunu geliştirmek için yararlı bulabilirsiniz. Azure AD zaten dağıtmış olan kuruluşlar bile, bunlar en iyi yatırımları aldıklarından emin olmak için bu denetim listesini kullanabilirsiniz.

İyi planlanmış ve yürütülen kimlik altyapısını daha güvenli erişim için üretkenlik iş yüklerini ve verileri yalnızca kimliği doğrulanmış kullanıcılar ve cihazlar tarafından bir hazırlık niteliğindedir.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzda, Azure AD Premium P2 lisansı, Enterprise Mobility + Security E5, Microsoft 365 E5'e veya bir eşdeğer lisans paketi sahip olduğunuz varsayılmaktadır.

[Azure AD lisanslama](https://azure.microsoft.com/pricing/details/active-directory/)

[Microsoft 365 Kurumsal](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise.aspx)

[Enterprise Mobility + Security](https://www.microsoft.com/licensing/product-licensing/enterprise-mobility-security.aspx)

## <a name="plan-and-deploy-day-1-30"></a>Planlama ve dağıtma: 1-30 gün

- Birden fazla genel yönetici (kesme cam hesabı) belirtme
   - [Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
- Azure AD Privileged Identity Management (PIM) raporları görüntülemek için üzerinde Aç
   - [PIM kullanmaya başlayın](../privileged-identity-management/pim-getting-started.md)
- Mümkün olduğunda genel olmayan yönetici rollerini kullanın.
   - [Azure Active Directory’de yönetici rolü atama](../users-groups-roles/directory-assign-admin-roles.md)
- Kimlik Doğrulaması
   - [Self servis parola sıfırlamayı dağıtma](../authentication/howto-sspr-deployment.md)
   - Azure AD parola koruması (Önizleme) dağıtma
      - [Hatalı parola kuruluşunuzdaki ortadan kaldırın](../authentication/concept-password-ban-bad.md)
      - [Windows Server Active Directory için Azure AD parola koruması zorlama](../authentication/concept-password-ban-bad-on-premises.md)
   - Hesap kilitleme ilkeleri yapılandırma
      - [Azure Active Directory akıllı kilitleme](../authentication/howto-password-smart-lockout.md)
      - [AD FS Extranet kilitleme ve akıllı Extranet kilitleme](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)
   - [Koşullu erişim ilkeleri kullanarak Azure AD multi-Factor Authentication'ı dağıtma](../authentication/howto-mfa-getstarted.md)
   - [Self Servis parola sıfırlama ve Azure AD multi-Factor Authentication (Önizleme) için yakınsanmış kaydını etkinleştirin](../authentication/concept-registration-mfa-sspr-converged.md)
   - [Azure Active Directory kimlik Koruması'nı etkinleştir](../identity-protection/enable.md)
      - [Tetikleyici çok faktörlü kimlik doğrulaması ve parola değişiklikleri için risk olayları kullanın](../authentication/tutorial-risk-based-sspr-mfa.md)
   - [Parola yönergeleri](https://www.microsoft.com/research/publication/password-guidance/)
      - Bir sekiz karakter uzunluğundaki en az uzunluk gereksinimini bakımını yapmak, artık gerekmeyen daha iyi değil.
      - Karakter-birleştirme gereksinimleri ortadan kaldırır.
      - [Kullanıcı hesapları için zorunlu düzenli parola sıfırlama ortadan kaldırır.](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire)
- Şirket içi Active Directory'den kullanıcıları eşitlemeye
   - [Azure AD Connect'i yükleme](../connect/active-directory-aadconnect-select-installation.md)
   - [Parola karma eşitlemesi uygulama](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
   - [Parola geri yazma uygulama](../authentication/howto-sspr-writeback.md)
   - [Uygulama Azure AD Connect Health'i](../connect-health/active-directory-aadconnect-health.md)
- [Azure Active Directory'de Grup üyeliği kullanıcıları için lisans atama](../users-groups-roles/licensing-groups-assign.md)

## <a name="plan-and-deploy-day-31-90"></a>Planlama ve dağıtma: 31-90 gün

- [Konuk kullanıcı erişimini planlama](../b2b/what-is-b2b.md)
   - [Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme](../b2b/add-users-administrator.md)
   - [İzin verme veya davetleri B2B kullanıcıları belirli kuruluşlardan engelleme](../b2b/allow-deny-list.md)
   - [GRANT B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](../b2b/hybrid-cloud-to-on-premises.md)
- Kullanıcı yaşam döngüsü yönetimi stratejisi hakkında kararlar
- [Cihaz yönetim stratejinize karar verin](../devices/overview.md)
   - [Kullanım senaryoları ve Azure AD katılım için dağıtım konuları](../devices/azureadjoin-plan.md)
- [Windows iş için Hello, kuruluşunuzda yönetme](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization)

## <a name="plan-and-deploy-day-90-and-beyond"></a>Planlama ve dağıtma: 90 gün ve sonraki süreci desteleyen

- [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)
   - [PIM'de Azure AD dizini rol ayarlarını yapılandırma](../privileged-identity-management/pim-how-to-change-default-settings.md)
   - [Azure AD dizin rollerini PIM atayın](../privileged-identity-management/pim-how-to-add-role-to-user.md)
- [Azure AD dizin rollerini PIM için erişim değerlendirmesi tamamlama](../privileged-identity-management/pim-how-to-start-security-review.md)
- Kullanıcı yaşam döngüsü bütünlüklü olarak yönetme
   - Azure AD kimlik yaşam döngüsünü yönetmek için bir yaklaşım vardır.
   - El ile yapılacak adımlar, yetkisiz erişimi önlemek için çalışan hesabı döngüsü, kaldırın:
      - Getirilir (ik sistemine) kaynağınızdan Azure AD kimlikleri eşitleyin. bağlantı ik sistemleri desteklenir)
      - [İK (veya bir kaynak sağlar), departman, başlık, bölge, bunların özniteliklerini ve diğer özniteliklerini göre gruplara otomatik olarak kullanıcılara atamak için dinamik grupları kullanın.](../users-groups-roles/groups-dynamic-membership.md)
      - [Grup tabanlı erişim denetimi, otomatik olarak sağlama kullanıcılara SaaS uygulamaları için sağlama kullanın.](../manage-apps/what-is-access-management.md)

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Ortak kimlik ve cihaz erişim ilkeleri önerilir.](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
