---
title: Windows sanal masaüstü kullanıcıların - Azure akışını özelleştirin
description: PowerShell cmdlet'leri ile Windows Sanal Masaüstü Kullanıcıları için nasıl özelleştirileceğini akış.
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/30/2019
ms.author: v-hevem
ms.openlocfilehash: 869760a12089ed7453199ad8a3fa18a6cca87511
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157096"
---
# <a name="customize-feed-for-windows-virtual-desktop-users"></a>Windows Sanal Masaüstü kullanıcıları için akışı özelleştirme

RemoteApp ve Masaüstü uzak kaynaklar kullanıcılarınız için tanınan bir şekilde görünmesi için akış özelleştirebilirsiniz.

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="customize-the-display-name-for-a-remoteapp"></a>Bir RemoteApp için görünen ad özelleştirme

Kolay ad ayarlayarak, yayımlanan RemoteApp görünen adını değiştirebilirsiniz. Varsayılan olarak, kolay ad RemoteApp programının adı ile aynıdır.

Bir uygulama grubu için yayımlanan RemoteApps listesini almak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![PowerShell cmdlet Get-RDSRemoteApp adı vurgulanmış FriendlyName ile ekran görüntüsü.](media/get-rdsremoteapp.png)

Bir RemoteApp için kolay bir ad atamak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```
![Ekran adı ve yeni vurgulanmış FriendlyName PowerShell cmdlet'i Set-RDSRemoteApp görüntüsü.](media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Uzak Masaüstü için görünen ad özelleştirme

Yayımlanan bir Uzak Masaüstü için görünen ad, kolay bir ad ayarlayarak değiştirebilirsiniz. El ile bir konak havuzu ve PowerShell aracılığıyla Masaüstü uygulama grubu oluşturduysanız, "Oturumu Masaüstü" varsayılan kolay adı. Havuz ve masaüstü uygulaması grubunu GitHub Azure Resource Manager şablonu ya da teklifi Azure Marketi aracılığıyla bir konak oluşturduysanız, varsayılan kolay adı ana makine havuzu adı ile aynıdır.

Uzak Masaüstü kaynak almak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![PowerShell cmdlet Get-RDSRemoteApp adı vurgulanmış FriendlyName ile ekran görüntüsü.](media/get-rdsremotedesktop.png)

Uzak Masaüstü kaynak için kolay bir ad atamak için aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```
![Ekran adı ve yeni vurgulanmış FriendlyName PowerShell cmdlet'i Set-RDSRemoteApp görüntüsü.](media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılar için akış özelleştirdiğinize göre sizin için bir Windows sanal masaüstü istemcisi test etmek için oturum açabilir. Bunu yapmak için Windows sanal masaüstü bilgi belgeleri Bağlan geçin:
    
 * [Windows 10 veya Windows 7 bağlanın](connect-windows-7-and-10.md)
 * [Bir web tarayıcısından bağlanma](connect-web.md) 
