---
title: Windows sanal masaüstü Önizleme - Azure uygulama grupları yönetme
description: Windows sanal masaüstü Önizleme kiracılar, Azure Active Directory'de ayarlama açıklanır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 73425df1f0cfedd2a681650fc2b536a652b621d5
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206669"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Öğretici: Windows sanal masaüstü Önizleme için uygulama gruplarını yönetme

Yeni bir Windows sanal masaüstü Önizleme ana makine havuzu için oluşturulan varsayılan uygulama grubu de tam masaüstü yayımlar. Ayrıca, bir veya daha fazla RemoteApp uygulama grupları konak havuz oluşturabilirsiniz. Bir RemoteApp uygulama grubu oluşturun ve tek tek yayımlamak için bu öğreticiden yararlanın **Başlat** menü uygulamalar.

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir RemoteApp grubu oluşturun.
> * RemoteApp programları için erişim verin.

Başlamadan önce [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="create-a-remoteapp-group"></a>Bir RemoteApp grubu oluşturun

1. Yeni bir boş RemoteApp uygulama grubu oluşturmak için aşağıdaki PowerShell cmdlet'ini çalıştırın.

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. (İsteğe bağlı) Uygulama grubunun oluşturulduğunu doğrulamak için ana makine havuzu için tüm uygulama grupları listesini görmek için aşağıdaki cmdlet'i çalıştırabilirsiniz.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Bir listesini almak için aşağıdaki cmdlet'i çalıştırın **Başlat** menü apps'de konak havuzun sanal makine görüntüsü. Değerleri azaltma **FilePath**, **IconPath**, **IconIndex**ve diğer önemli bilgiler yayımlamak istediğiniz uygulama için.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Temel uygulamayı yüklemek için aşağıdaki cmdlet'i çalıştırın `AppAlias`. `AppAlias` 3. adımdaki çıktı çalıştırdığınızda görünür hale gelir.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. (İsteğe bağlı) 1. adımda oluşturduğunuz uygulama grubu için yeni bir RemoteApp programı yayımlamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Uygulama yayımlandığını doğrulamak için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Bu uygulama grubu için yayımlamak istediğiniz her bir uygulama için 1 – 5 adımlarını tekrarlayın.
8. Kullanıcılar uygulama grubu RemoteApp programları erişim vermek için aşağıdaki cmdlet'i çalıştırın.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulama grubu oluşturma, RemoteApp programları ile doldurmak ve kullanıcılar için uygulama grubu atamak öğrendiniz. Doğrulama konak havuzu oluşturmayı öğrenmek için aşağıdaki öğreticiye bakın. Hizmet güncelleştirmeleri üretim ortamınıza sunulmadan önce izlemek için bir doğrulama konak havuzu kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Hizmet güncelleştirmeleri doğrulamak için bir konak havuzu oluşturma](./create-validation-host-pool.md)
