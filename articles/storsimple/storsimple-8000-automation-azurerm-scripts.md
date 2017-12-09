---
title: "StorSimple cihazları yönetmek için Azure Resource Manager betiklerini kullanın | Microsoft Docs"
description: "StorSimple işleri otomatikleştirmek için Azure Resource Manager komut dosyaları kullanmayı öğrenin"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/03/2017
ms.author: alkohli
ms.openlocfilehash: 4bb6becd0b664b9287a1973d5221cff46dca57da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-resource-manager-sdk-based-scripts-to-manage-storsimple-devices"></a>StorSimple cihazları yönetmek için Azure Resource Manager SDK tabanlı komut dosyaları kullanma

Bu makalede Azure Resource Manager SDK tabanlı nasıl betikler StorSimple 8000 serisi Cihazınızı yönetmek için kullanılabilir. Bu komut dosyalarını çalıştırmak üzere ortamınızı yapılandırma adımları boyunca size yol için bir örnek komut dosyası da bulunur.

Bu makale, yalnızca Azure portalında çalışan StorSimple 8000 serisi cihazlar için geçerlidir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Aşağıdaki örnek komut dosyaları çeşitli StorSimple işleri otomatikleştirmek kullanılabilir.

#### <a name="table-of-azure-resource-manager-sdk-based-sample-scripts"></a>Azure Resource Manager SDK tabanlı örnek betikler tablosu

| Azure Kaynak Yöneticisi komut dosyası                    | Açıklama                                                                                                                                                                                                       |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Yetki ServiceEncryptionRollover.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Authorize-ServiceEncryptionRollover.ps1)          | Bu komut, hizmet verileri şifreleme anahtarı değiştirmek için StorSimple Cihazınızı yetkilendirmek sağlar.                                                                                                           |
| [Oluşturma StorSimpleCloudAppliance.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Create-StorSimpleCloudAppliance.ps1)              | Bu komut dosyasını bir 8010 veya 8020 bir StorSimple bulut uygulaması oluşturur. Bulut uygulaması yapılandırılmış ve StorSimple veri Yöneticisi hizmetiniz ile kayıtlı.                                                       |
| [CreateOrUpdate Volume.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/CreateOrUpdate-Volume.ps1)                        | Bu komut dosyasını oluşturur veya StorSimple birimlerini değiştirir.                                                                                                                                                             |
| [Get-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackup.ps1)                             | Bu komut, StorSimple cihaz Yöneticisi hizmetine kayıtlı bir cihaz için tüm yedeklemeler listeler.                                                                                                          |
| [Get-DeviceBackupPolicy.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackupPolicy.ps1)                       | Bu yedekleme ilkelerini StorSimple cihazınız için komut dosyası.                                                                                                                                                 |
| [Get-DeviceJobs.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1)                               | Bu komut, StorSimple cihaz Yöneticisi hizmeti üzerinde çalışan tüm StorSimple işleri alır.                                                                                                                     |
| [Get-DeviceUpdateAvailability.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceUpdateAvailability.ps1)                 | Bu komut dosyasını güncelleştirme sunucusunu tarar ve, StorSimple Cihazınızda yüklemek kullanılabilir güncelleştirmeler varsa bilmenizi sağlar.                                                                                          |
| [Yükleme DeviceUpdate.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Install-DeviceUpdate.ps1)                         | Bu komut, StorSimple Cihazınızda kullanılabilir güncelleştirmeleri yükler.                                                                                                                                           |
| [Yönetme CloudSnapshots.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Manage-CloudSnapshots.ps1)                        | Bu komut dosyasını el ile bulut anlık görüntüsü başlatır ve bulut anlık görüntüleri belirtilen bekletme günden eski siler.                                                                                                   |
| [İzleyici Backups.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1)                              | Bu Azure Automation Runbook PowerShell komut dosyası tüm yedekleme işleri durumunu raporlar.                                                                                                              |
| [Remove-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Remove-DeviceBackup.ps1)                          | Bu komut dosyasını tek bir yedekleme nesneyi siler.                                                                                                                                                           |
| [Start-DeviceBackupJob.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Start-DeviceBackupJob.ps1)                        | Bu komut dosyasını el ile yedekleme StorSimple Cihazınızda başlatır.                                                                                                                                       |
| [Güncelleştirme CloudApplianceServiceEncryptionKey.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Update-CloudApplianceServiceEncryptionKey.ps1)    | Bu komut, 8010/8020 StorSimple bulut cihazları StorSimple cihaz Yöneticisi hizmetiniz ile kaydedilen tüm hizmet verileri şifreleme anahtarı güncelleştirir.                                     |
| [Doğrulama BackupScheduleAndBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Verify-BackupScheduleAndBackup.ps1)               | Bu komut dosyasını eksik yedeklemeler yedekleme ilkeleri ile ilişkili tüm zamanlamalar çözümledikten sonra vurgular. Ayrıca, yedekleme kataloğu kullanılabilir yedeklemeleri listesi ile doğrular.             |




## <a name="configure-and-run-a-sample-script"></a>Yapılandırma ve bir örnek komut dosyası çalıştırma

Bu bölümde, örnek komut dosyası alır ve komut dosyasını çalıştırmak için gereken çeşitli adımları ayrıntıları.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapın:

*   Azure PowerShell yüklü. Azure PowerShell modüllerini yüklemek için:
    * Bir Windows ortamı adımları [yükleyin ve Azure PowerShell yapılandırma](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0). Azure PowerShell, Windows Server konağında StorSimple için birini kullanıyorsanız yükleyebilirsiniz.
    * Bir Linux veya MacOS ortamında adımları [yükleyin ve MacOS veya Linux Azure PowerShell yapılandırın](https://docs.microsoft.com/en-us/powershell/azure/install-azurermps-maclinux?view=azurermps-4.4.0).

Azure PowerShell'i kullanma hakkında daha fazla bilgi için Git [kullanarak Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps?view=azurermps-4.4.0).

### <a name="run-azure-powershell-script"></a>Azure PowerShell komut dosyasını çalıştır

Bu örnekte kullanılan komut dosyasını bir StorSimple cihaz üzerindeki tüm işleri listeler. Bu, başarılı, başarısız oldu veya devam eden işler içerir. Karşıdan yükleme ve komut dosyasını çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure PowerShell’i çalıştırın. Yeni bir klasör oluşturun ve yeni klasöre dizini değiştirin.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet CLI karşıdan](http://www.nuget.org/downloads) önceki adımda oluşturduğunuz klasörü altında. Çeşitli sürümü vardır _nuget.exe_. SDK'sına karşılık gelen sürümünü seçin. Her karşıdan yükleme bağlantısı doğrudan işaret eden bir _.exe_ dosya. Sağ tıklayın ve dosyayı bilgisayarınıza kaydedin emin yerine tarayıcıdan çalışıyor.

    Ayrıca, karşıdan yüklemek ve betik daha önce oluşturduğunuz aynı klasörde depolamak için aşağıdaki komutu çalıştırabilirsiniz.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Bağımlı SDK'sını indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Komut dosyası örneği GitHub projeden indirin.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1

    ```

5. Betiği çalıştırın. Kimlik doğrulaması yapmak isteyip istemediğiniz sorulduğunda Azure kimlik bilgilerinizi sağlayın. Bu komut, StorSimple Cihazınızda tüm işlerin filtre uygulanmış bir listesini çıktı.
           
    ```           
        .\Get-StorSimpleJob.ps1 -SubscriptionId [subid] -TenantId [tenant id] -DeviceName [name of device] -ResourceGroupName [name of resource group] -ManagerName[name of device manager] -FilterByStatus [Filter for job status] -FilterByJobType [Filter for job type] -FilterByStartTime [Filter for start date time] -FilterByEndTime [Filter for end date time]

    ```

### <a name="sample-output"></a>Örnek çıktı

Örnek komut dosyasını çalıştırdığınızda, aşağıdaki çıkış sunulur. Çıkış, 25 Eylül 2017 üzerinde başlatıldı ve 2 Ekim 2017 tamamlanmış bir kayıtlı cihazda çalışan tüm işleri içerir.

```
-----------------------------------------
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

PS C:\Scripts\StorSimpleSDKTools> wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1
PS C:\Scripts\StorSimpleSDKTools> .\Get-DeviceJobs.ps1 -SubscriptionId 1234ab5c-678d-910e-9fc4-0accc9c0166e -TenantId 12a345bc-67d8-91ef-01ab-2c7cd123ef45 -DeviceName 8600-ABC1234567D89EF -ResourceGroupName Contoso -ManagerName ContosoDeviceMgr -FilterByStartTime "09/25/2017 08:10:02" -FilterByEndTime "10/02/2017 08:10:02"


Status            : Succeeded
StartTime         : 10/2/2017 10:30:02 PM
EndTime           : 10/2/2017 10:31:36 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : ss-asr-policy1
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC/jobs/75905c48-b153-4af1-8b21-4b9a2ff9
                    825b
Name              : 75905c48-b153-4af1-8b21-4b9a2ff9825b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000
-------------------------------------------
CUT              CUT  
-------------------------------------------
Status            : Succeeded
StartTime         : 9/26/2017 5:00:02 PM
EndTime           : 9/26/2017 5:01:20 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : 8010 policy
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF/jobs/3cfd8108-db60-4e9a-a8da-6d8fe457
                    8d2b
Name              : 3cfd8108-db60-4e9a-a8da-6d8fe4578d2b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000



PS C:\Scripts\StorSimpleSDKTools>
--------------------------------------------

```


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Cihazınızı yönetmek için Aygıt Yöneticisi'ni StorSimple hizmeti](storsimple-8000-manager-service-administration.md).