---
title: Windows sanal masaüstü Önizleme - Azure Temsilcili erişim
description: Örnekler de dahil olmak üzere bir Windows sanal masaüstü Önizleme dağıtımdaki yönetici yetkileri nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 250aea52de63a6397ce00e9cadcadf3a8ba39858
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60870515"
---
# <a name="delegated-access-in-windows-virtual-desktop-preview"></a>Windows sanal masaüstü önizlemede Temsilcili erişim

Windows sanal masaüstü Önizleme, belirli bir kullanıcıya bir rol atayarak için izin verilen erişim miktarını tanımlamanıza olanak sağlayan bir temsilci erişimi modeline sahiptir. Bir rol ataması üç bileşenden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam. Azure RBAC modelinde Windows sanal masaüstü Temsilcili erişim modeli temel alır. Özel rol atamaları ve bileşenleri hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimine genel bakış](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

Windows Sanal Masaüstü yönetici temsilcisi erişimi destekleyen her öğenin bir rol ataması için aşağıdaki değerleri:

* Güvenlik sorumlusu
    * Kullanıcılar
    * Hizmet sorumluları
* Rol tanımı
    * Yerleşik roller
* `Scope`
    * Kiracı grupları
    * Kiracılar
    * Ana bilgisayar havuzları
    * Uygulama grupları

## <a name="built-in-roles"></a>Yerleşik roller

Windows sanal masaüstü Temsilcili erişim, kullanıcıların ve hizmet sorumlusu atamak için birkaç yerleşik rol tanımları vardır.

* RDS sahibi kaynaklara erişim dahil her şeyi yönetebilir.
* Bir RDS katkıda bulunan her şeyi kaynaklarına erişimi yönetebilir.
* RDS okuyucu her şeyi görüntüleyebilir ancak değişiklik yapamazsınız.
* RDS operatörün tanılama etkinlikleri görüntüleyebilirsiniz.

## <a name="powershell-cmdlets-for-role-assignments"></a>Rol atamaları için PowerShell cmdlet'leri

Oluşturun, görüntüleyin ve rolü atamalarını düzenlemek için aşağıdaki cmdlet'leri çalıştırabilirsiniz:

* **Get-RdsRoleAssignment** rol atamaları listesini görüntüler.
* **Yeni RdsRoleAssignment** yeni bir rol ataması oluşturur.
* **Set-RdsRoleAssignment** rol atamaları düzenler.

### <a name="accepted-parameters"></a>Kabul edilen parametreleri

Aşağıdaki parametrelerle temel üç cmdlet değiştirebilirsiniz:

* **AadTenantId**: hizmet sorumlusu üyesi olan Azure Active Directory Kiracı Kimliğini belirtir.
* **AppGroupName**: Uzak Masaüstü uygulama grubunun adı.
* **Tanılama**: Tanılama kapsamını belirtir. (İle eşleştirilmelidir **altyapı** veya **Kiracı** parametreler.)
* **HostPoolName**: Uzak Masaüstü'nü konak havuzunun adı.
* **Altyapı**: altyapı kapsamını belirtir.
* **RoleDefinitionName**: kullanıcı, Grup veya uygulama atanan Uzak Masaüstü Hizmetleri rol tabanlı erişim denetimi rolün adı. (Örneğin, Uzak Masaüstü Hizmetleri sahibi, Uzak Masaüstü Hizmetleri okuyucu ve benzeri.)
* **ServerPrincipleName**: Azure Active Directory uygulamasının adı.
* **SignInName**: kullanıcının e-posta adresi veya kullanıcı asıl adı.
* **Kiracıadı**: Uzak Masaüstü Kiracı adı.

## <a name="next-steps"></a>Sonraki adımlar

Her rol kullanabileceğiniz PowerShell cmdlet'leri daha eksiksiz bir listesi için bkz. [PowerShell başvurusu](/powershell/windows-virtual-desktop/overview).

Bir Windows sanal masaüstü ortamını ayarlama ilişkin yönergeler için bkz: [Windows sanal masaüstü Önizleme ortamı](environment-setup.md).
