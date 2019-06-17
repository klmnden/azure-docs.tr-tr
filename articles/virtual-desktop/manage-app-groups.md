---
title: Windows sanal masaüstü Önizleme - Azure uygulama grupları yönetme
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: cba13012bf165a097bd1382da8ef9897b0584d28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066871"
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
   
4. Kendi appalias alarak uygulamayı yüklemek için aşağıdaki cmdlet'i çalıştırın. 3\. adımdaki çıktı çalıştırdığınızda appalias görünür hale gelir.

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

Bu öğreticide, kullanıcıların uygulama grubuna atayın uygulama grupları oluşturma ve RemoteApps ile doldurmak öğrendiniz. Hizmet güncelleştirmeleri üretim ortamınıza sunulmadan önce izlemenize izin verir, doğrulama konak havuz oluşturma hakkında bilgi edinmek için hizmet güncelleştirmeleri öğretici doğrulamak için bir ana makine havuzu oluştur bakın.

> [!div class="nextstepaction"]
> [Hizmet güncelleştirmeleri öğretici doğrulamak için bir konak havuzu oluşturma](./create-validation-host-pool.md)
