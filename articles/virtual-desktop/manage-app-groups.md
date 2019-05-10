---
title: Windows sanal masaüstü Önizleme - Azure uygulama grupları yönetme
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: f0cdd28be8c6e7390aa26fdc2dfbf32ec5542c2d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233914"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü Önizleme için uygulama gruplarını yönetme

Yeni bir Windows sanal masaüstü Önizleme ana makine havuzu için oluşturulan varsayılan uygulama grubu de tam masaüstü yayımlar. Ayrıca, bir veya daha fazla RemoteApp uygulama grupları konak havuz oluşturabilirsiniz. Tek başlangıç menüsünde uygulama RemoteApp uygulama grubu oluşturmak ve yayımlamak için bu öğreticiden yararlanın.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir RemoteApp grubu oluşturun.
> * RemoteApps erişim izni verin.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) Windows sanal masaüstü modülü henüz yapmadıysanız, PowerShell oturumunuzda kullanın.

## <a name="create-a-remoteapp-group"></a>Bir RemoteApp grubu oluşturun

1. Yeni bir boş RemoteApp uygulama grubu oluşturmak için aşağıdaki PowerShell cmdlet'ini çalıştırın.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (İsteğe bağlı) Uygulama grubu oluşturuldu doğrulamak için ana makine havuzu için tüm uygulama grupları listesini görmek için aşağıdaki cmdlet'i çalıştırabilirsiniz.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Başlangıç listesi, menü uygulamaları konak havuzun sanal makine görüntüsüne almak aşağıdaki cmdlet'i çalıştırın. Değerleri azaltma **FilePath**, **IconPath**, **IconIndex**ve diğer önemli bilgileri, yayımlamak istediğiniz uygulama için.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Kendi appalias alarak uygulamayı yüklemek için aşağıdaki cmdlet'i çalıştırın. 3. adımdaki çıktı çalıştırdığınızda appalias görünür hale gelir.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. (İsteğe bağlı) 1. adımda oluşturduğunuz uygulama grubu için yeni bir RemoteApp yayımlamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Uygulama yayımlandığını doğrulamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Bu uygulama grubu için yayımlamak istediğiniz her uygulama için 1 – 5 adımlarını tekrarlayın.
8. Kullanıcılar uygulama grubundaki RemoteApps erişim vermek için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanıcıların uygulama grubuna atayın uygulama grupları oluşturma ve RemoteApps ile doldurmak öğrendiniz. Windows sanal masaüstü oturum açma hakkında daha fazla bilgi için Windows sanal masaüstü bilgi belgeleri Bağlan devam edin.

- [Uzak Masaüstü İstemcisi Windows 7 ve Windows 10 bağlanma](connect-windows-7-and-10.md)
- [Windows sanal masaüstü Önizleme web istemcisi için Bağlan](connect-web.md)
