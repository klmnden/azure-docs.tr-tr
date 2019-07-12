---
title: Windows sanal masaüstü PowerShell - Azure
description: Bir Windows sanal masaüstü Kiracı ortamını ayarlarken PowerShell ile ilgili sorunları giderme
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 06b955365ffc7c0a1dff93db95932d8696293e9f
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605236"
---
# <a name="windows-virtual-desktop-powershell"></a>Windows Sanal Masaüstü PowerShell

PowerShell ile Windows sanal masaüstü kullanırken hataları ve sorunları gidermek için bu makaleyi kullanın. Uzak Masaüstü Hizmetleri PowerShell hakkında daha fazla bilgi için bkz. [Windows sanal masaüstü Powershell](https://docs.microsoft.com/powershell/module/windowsvirtualdesktop/).

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows sanal masaüstü Önizleme aşamasındayken biz şu anda destek alma değildir. Ziyaret [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) etkin topluluk üyeleri ve ürün ekibine Windows sanal masaüstü hizmetiyle tartışmak için.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>Sanal Masaüstü Windows Kurulumu sırasında kullanılan PowerShell komutları

Bu bölüm, genellikle Windows sanal masaüstünü ayarlanırken kullanılır ve bunları kullanırken oluşabilecek sorunların çözülmesine yönelik yöntemler sağlayan PowerShell komutlarını listeler.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-is-already-assigned-to-a-remoteapp-app-group-in-the-specified-host-pool"></a>Hata: Belirtilen konak havuzdaki RemoteApp uygulama grubuna ekleme RdsAppGroupUser komutu--belirtilen UserPrincipalName zaten atanmış

```Powershell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName 'Desktop Application Group' -UserPrincipalName <UserName>
```

**Neden:** Kullanılan kullanıcı adı zaten farklı bir tür için bir uygulama grubuna atandı. Kullanıcılar, aynı oturum ana havuzuna altında hem de Uzak Masaüstü ve Uzaktan uygulama grubuna atanamaz.

**Düzeltme:** Kullanıcının uzak uygulamaları hem de Uzak Masaüstü gerekirse, farklı ana bilgisayar havuzları oluşturmak veya herhangi bir uygulama kullanımını oturumu konakta VM izin verecek bir Uzak Masaüstü Kullanıcı erişim.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-doesnt-exist-in-the-azure-active-directory-associated-with-the-remote-desktop-tenant"></a>Hata: Azure Active Directory'de Uzak Masaüstü Kiracı ile ilişkilendirilen ekleme RdsAppGroupUser komutu--belirtilen UserPrincipalName yok

```PowerShell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName “Desktop Application Group” -UserPrincipalName <UserPrincipalName>
```

**Neden:** Azure Active Directory'de Windows sanal masaüstü kiracısına bağlı - UserPrincipalName tarafından belirtilen kullanıcı bulunamıyor.

**Düzeltme:** Aşağıdaki listedeki öğeleri doğrulayın.

- Kullanıcı Azure Active Directory ile eşitlenmiş.
- Kullanıcı işletmeden müşteriye satış (B2C) ya da işletmeler arası (B2B) ticaret bağlı değil.
- Windows sanal masaüstü Kiracı doğru Azure Active Directory'ye bağlanır.

### <a name="error-get-rdsdiagnosticactivities----user-isnt-authorized-to-query-the-management-service"></a>Hata: Get-RdsDiagnosticActivities--Yönetim hizmeti sorgulamak kullanıcı yetkili değil

```PowerShell
Get-RdsDiagnosticActivities -ActivityId <ActivityId>
```

**Neden:** - Kiracıadı parametresi

**Düzeltme:** Get-RdsDiagnosticActivities - Kiracıadı ile sorun <TenantName>.

### <a name="error-get-rdsdiagnosticactivities----the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Get-RdsDiagnosticActivities--kullanıcı Yönetim hizmetini sorgulama yetkisine sahip değil

```PowerShell
Get-RdsDiagnosticActivities -Deployment -username <username>
```

**Neden:** Kullanarak - dağıtım anahtarı.

**Düzeltme:** -dağıtım anahtarı yalnızca dağıtım yöneticileri tarafından kullanılabilir. Bu genellikle Uzak Masaüstü Hizmetleri/Windows sanal masaüstü ekibi üyelerinin yöneticilerdir. Değiştir - Kiracıadı dağıtım anahtarıyla <TenantName>.

### <a name="error-new-rdsroleassignment----the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Yeni-RdsRoleAssignment--kullanıcı Yönetim hizmetini sorgulama yetkisine sahip değil

**1. neden:** Kullanılan hesap, Kiracı'da Uzak Masaüstü Hizmetleri sahip iznine sahip değil.

**1 düzeltin:** Rol ataması yürütmek Uzak Masaüstü Hizmetleri sahip izinlerine sahip bir kullanıcı gerekir.

**2. neden:** Kullanılmakta olan hesap Uzak Masaüstü Hizmetleri sahip izinlerine sahiptir, ancak kiracının Azure Active Directory'nin parçası değil veya Azure Active Directory'ı kullanıcının bulunduğu sorgulama izni yok.

**2 düzeltin:** Rol ataması yürütmek Active Directory izinleri olan bir kullanıcı gerekir.

>[!Note]
>RdsRoleAssignment yeni Azure Active Directory (AD) içinde mevcut olmayan bir kullanıcıya izin veremez.

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme parçaları sorun giderme hakkında genel bir bakış için bkz: [genel bakış, geri bildirim ve destek sorunlarını giderme](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında bir kiracı ve konak havuzu oluşturulurken sorunlarını gidermek için bkz: [Kiracı ve konak havuz oluşturma](troubleshoot-set-up-issues.md).
- Bir sanal makine (VM) Windows sanal masaüstü yapılandırılırken sorunlarını gidermek için bkz: [oturumu ana bilgisayar Sanal Makine Yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantıları ile ilgili sorunları gidermek için bkz: [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md).
- Önizleme hizmeti hakkında daha fazla bilgi için bkz: [Windows Desktop Önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablonu dağıtımlardaki sorunları çözmek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).