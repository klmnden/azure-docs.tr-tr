---
title: Windows sanal masaüstü için (Önizleme) - Azure uygulama grupları yönetme
description: Windows sanal masaüstü kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 455e3b4ac4a5020f68b5201bc19f85892ef62cb1
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368995"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü (Önizleme) için uygulama gruplarını yönetme

Yeni bir ana makine havuzu için oluşturulan varsayılan uygulama grubu de tam masaüstü yayımlar. Ayrıca, bir veya daha fazla RemoteApp (Önizleme) uygulama grupları konak havuz oluşturabilirsiniz. Tek başlangıç menüsünde uygulama RemoteApp uygulama grubu oluşturmak ve yayımlamak için bu öğreticiden yararlanın.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir RemoteApp grubu oluşturun.
> * RemoteApps erişim izni verin.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) Windows sanal masaüstü modülü henüz yapmadıysanız, PowerShell oturumunuzda kullanın.

## <a name="create-a-remoteapp-group"></a>Bir RemoteApp grubu oluşturun

1. Yeni ve boş bir RemoteApp grubu oluşturmak için aşağıdaki PowerShell cmdlet'ini çalıştırın.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (İsteğe bağlı) Uygulama grubunun oluşturulduğu doğrulamak için ana makine havuzu için tüm uygulama grupları listesini görmek için aşağıdaki cmdlet'i çalıştırabilirsiniz.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Başlangıç listesi, menü uygulamaları konak havuzun sanal makine görüntüsüne almak aşağıdaki cmdlet'i çalıştırın. Değerleri azaltma **FilePath**, **IconPath**, **IconIndex**ve diğer önemli bilgileri, yayımlamak istediğiniz uygulama için.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```

4. 1. adımda oluşturduğunuz uygulama grubu için yeni bir RemoteApp yayımlamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

5. (İsteğe bağlı) Appalias alarak uygulamayı yüklemek için aşağıdaki cmdlet'i çalıştırın. 3. adımdaki çıktı çalıştırdığınızda appalias görünür hale gelir.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> <remoteappname> -AppAlias <appalias>
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

Uygulama gruplarınızı oluşturduktan sonra hizmet sorumluları oluşturur ve kullanıcılarınıza roller atayabilirsiniz. Bunu öğrenmek için PowerShell ile hizmet sorumluları ve rol atamalarını oluşturmayı öğrenmek için bkz.

> [!div class="nextstepaction"]
> [PowerShell ile hizmet sorumluları ve rol atamalarını oluşturma](create-service-principal-role-powershell.md)
