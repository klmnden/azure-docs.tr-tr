---
title: Azure Stack yedekleme | Microsoft Docs
description: İsteğe bağlı yedekleme yerinde yedekleme ile Azure Stack üzerinde gerçekleştirin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 578bb864f56b788db77d1201533e73d3b9616669
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056541"
---
# <a name="back-up-azure-stack"></a>Azure yığını yedekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

İsteğe bağlı yedekleme yerinde yedekleme ile Azure Stack üzerinde gerçekleştirin. PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure Stack için PowerShell yükleme ](azure-stack-powershell-install.md). Azure Stack'e oturum açmak için bkz: [Yönetici portalını kullanarak Azure Stack'te](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Azure Stack yedekleme Başlat

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Yeni bir yedekleme iş ilerleme durumunu izlemeyi başlatın
Yeni bir yedekleme işi ilerleme izleme ile hemen başlatmak için başlangıç AzSBackup kullanın.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Azure Stack yedekleme işi ilerleme durumu izleme Başlat
Yedekleme işinin ilerleme durumunu izlemek için - AsJob değişkeni ile yeni bir yedekleme başlatmak için başlangıç AzSBackup kullanın.

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob 
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " `
    + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) `
    -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output

    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

## <a name="confirm-backup-has-completed"></a>Yedekleme tamamlandı onaylayın

### <a name="confirm-backup-has-completed-using-powershell"></a>PowerShell kullanarak yedekleme gerçekleştirilene onaylayın
Yedekleme başarıyla tamamlandığından emin olmak için aşağıdaki PowerShell komutlarını kullanın:

```powershell
   Get-AzsBackup
```

Sonuç aşağıdaki çıktı gibi görünmelidir:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administration-portal"></a>Yönetim Portalı'nda yedekleme gerçekleştirilene onaylayın
Aşağıdaki adımları izleyerek bu Yedekleme başarıyla tamamlandığını doğrulayın için Azure Stack yönetim portalını kullanın:

1. Açık [Azure Stack Yönetim Portalı](azure-stack-manage-portals.md).
2. Seçin **diğer hizmetler** > **altyapı yedeklemesine**. Seçin **yapılandırma** içinde **altyapı yedeklemesine** dikey penceresi.
3. Bulma **adı** ve **tamamlanma tarihi** yedeğin **kullanılabilir yedekler** listesi.
4. Doğrulama **durumu** olduğu **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

Bir veri kaybı olayından kurtarmak için iş akışı hakkında daha fazla bilgi edinin. Bkz: [geri dönülemez veri kaybından kurtarma](azure-stack-backup-recover-data.md).
