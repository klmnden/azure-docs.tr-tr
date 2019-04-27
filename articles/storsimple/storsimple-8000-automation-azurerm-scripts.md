---
title: StorSimple cihazları yönetmek için Azure Resource Manager betikleri kullanma | Microsoft Docs
description: StorSimple işleri otomatik hale getirmek için Azure Resource Manager betiklerini kullanmayı öğrenin
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/03/2017
ms.author: alkohli
ms.openlocfilehash: 646b862733e8727c9c8729f1ac038fa88cfa0580
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60726426"
---
# <a name="use-azure-resource-manager-sdk-based-scripts-to-manage-storsimple-devices"></a>StorSimple cihazları yönetmek için Azure Resource Manager SDK tabanlı betik kullanma

Bu makalede Azure Resource Manager SDK tabanlı nasıl betikler StorSimple 8000 serisi Cihazınızı yönetmek için kullanılabilir. Örnek betik, bu betikleri çalıştırmak için ortamınızı yapılandırma adımları uygulamak için de bulunur.

Bu makale, yalnızca Azure Portal'da çalıştıran StorSimple 8000 serisi cihazlar için geçerlidir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Aşağıdaki örnek komut, çeşitli StorSimple işlemleri otomatik hale getirmek kullanılabilir.

#### <a name="table-of-azure-resource-manager-sdk-based-sample-scripts"></a>Azure Resource Manager SDK tabanlı örnek betikler tablosu

| Azure Resource Manager betiği                    | Açıklama                                                                                                                                                                                                       |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Yetkilendirme ServiceEncryptionRollover.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Authorize-ServiceEncryptionRollover.ps1)          | Bu betik, hizmet veri şifreleme anahtarını değiştirmek için StorSimple cihaz yetkilendirme sağlar.                                                                                                           |
| [Create-StorSimpleCloudAppliance.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Create-StorSimpleCloudAppliance.ps1)              | Bu betik bir 8010 veya 8020 bir StorSimple Cloud Appliance oluşturur. Bulut Gereci yapılandırılmış ve ile StorSimple veri Yöneticisi hizmetine kayıtlı.                                                       |
| [CreateOrUpdate-Volume.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/CreateOrUpdate-Volume.ps1)                        | Bu betik oluşturur veya StorSimple birimlerini değiştirir.                                                                                                                                                             |
| [Get-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackup.ps1)                             | Bu betik, StorSimple cihaz Yöneticisi hizmetine kayıtlı bir cihaz için tüm yedeklemeler listelenir.                                                                                                          |
| [Get-DeviceBackupPolicy.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackupPolicy.ps1)                       | Bu yedekleme ilkelerini StorSimple cihazınız için komut dosyası.                                                                                                                                                 |
| [Get-DeviceJobs.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1)                               | Bu betik, StorSimple cihaz Yöneticisi hizmetinizde çalıştıran tüm StorSimple işleri alır.                                                                                                                     |
| [Get-DeviceUpdateAvailability.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceUpdateAvailability.ps1)                 | Bu betik, güncelleştirme sunucusunu tarar ve güncelleştirmeler StorSimple cihazınıza yüklemeniz varsa bildirir.                                                                                          |
| [Yükleme DeviceUpdate.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Install-DeviceUpdate.ps1)                         | Bu betik, StorSimple Cihazınızda kullanılabilir güncelleştirmeleri yükler.                                                                                                                                           |
| [CloudSnapshots.ps1 yönetme](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Manage-CloudSnapshots.ps1)                        | Bu betik, el ile bulut anlık görüntüsü başlatır ve belirtilen bekletme gün sayısından eski bulut anlık görüntüleri siler.                                                                                                   |
| [İzleyici Backups.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1)                              | Bu Azure Otomasyonu Runbook PowerShell Betiği, tüm yedekleme işlerinin durumunu raporlar.                                                                                                              |
| [Remove-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Remove-DeviceBackup.ps1)                          | Bu betik, tek bir yedekleme nesneyi siler.                                                                                                                                                           |
| [Başlangıç DeviceBackupJob.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Start-DeviceBackupJob.ps1)                        | Bu betik, StorSimple Cihazınızda el ile yedekleme başlatır.                                                                                                                                       |
| [Update-CloudApplianceServiceEncryptionKey.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Update-CloudApplianceServiceEncryptionKey.ps1)    | Bu betik, 8010/8020 StorSimple bulut Gereçleri, StorSimple cihaz Yöneticisi hizmetine kayıtlı tüm hizmet veri şifreleme anahtarını güncelleştirir.                                     |
| [BackupScheduleAndBackup.ps1 doğrulayın](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Verify-BackupScheduleAndBackup.ps1)               | Bu betik, yedekleme ilkeleri ile ilişkili tüm zamanlamalar çözümledikten sonra eksik yedeklemeleri vurgular. Ayrıca, yedekleme kataloğunu kullanılabilir yedekler listesiyle doğrular.             |




## <a name="configure-and-run-a-sample-script"></a>Yapılandırma ve bir örnek betiği çalıştırma

Bu bölümde, bir örnek betiği alır ve betiği çalıştırmak için gereken çeşitli adımlarını ayrıntılı şekilde açıklar.

### <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapın:

*   Azure PowerShell yüklü. Azure PowerShell modüllerini yüklemek için:
    * Bir Windows ortamında adımları [yüklemek ve Azure PowerShell yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). Azure PowerShell, Windows Server konağında StorSimple'ınızı birini kullanıyorsanız yükleyebilirsiniz.
    * Bir Linux veya Macos'ta ortamında adımları [yüklemek ve Azure PowerShell'i MacOS veya Linux'ta yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurermps-maclinux).

Azure PowerShell kullanma hakkında daha fazla bilgi için Git [kullanarak Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

### <a name="run-azure-powershell-script"></a>Azure PowerShell Betiği çalıştırma

Bu örnekte kullanılan betiği, bir StorSimple cihazında tüm işler listelenir. Bu, başarılı, başarısız oldu veya devam eden işleri içerir. Bir betik indirip çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure PowerShell’i çalıştırın. Yeni bir klasör oluşturun ve yeni klasörüne dizin değiştirin.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet CLI'yı indirme](https://www.nuget.org/downloads) önceki adımda oluşturduğunuz klasöre altında. Çeşitli sürümleri vardır _nuget.exe_. SDK'nızı için karşılık gelen sürümü seçin. Her bir indirme bağlantısı doğrudan işaret eden bir _.exe_ dosya. Sağ tıklayın ve dosyayı bilgisayarınıza kaydetmek mutlaka yerine tarayıcıda çalışıyor.

    Ayrıca, karşıdan yüklemek ve betik, daha önce oluşturduğunuz aynı klasöre depolamak için aşağıdaki komutu çalıştırabilirsiniz.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Bağımlı SDK'sını indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Betik örnek GitHub projesinden indirmeniz gerekir.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1

    ```

5. Betiği çalıştırın. Kimlik doğrulaması için istendiğinde Azure kimlik bilgilerinizi sağlayın. Bu betik, StorSimple cihazınızdaki tüm işleri filtrelenmiş bir listesini oluşturmalıdır.
           
    ```           
        .\Get-StorSimpleJob.ps1 -SubscriptionId [subid] -TenantId [tenant id] -DeviceName [name of device] -ResourceGroupName [name of resource group] -ManagerName[name of device manager] -FilterByStatus [Filter for job status] -FilterByJobType [Filter for job type] -FilterByStartTime [Filter for start date time] -FilterByEndTime [Filter for end date time]

    ```

### <a name="sample-output"></a>Örnek çıktı

Örnek komut dosyasını çalıştırdığınızda aşağıdaki çıkış sunulur. Çıktı, 25 Eylül 2017'de başlangıç ve 2 Ekim 2017 tarafından tamamlanan bir kayıtlı cihazda çalışan tüm işleri içerir.

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

[StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi'ni kullanın hizmet](storsimple-8000-manager-service-administration.md).