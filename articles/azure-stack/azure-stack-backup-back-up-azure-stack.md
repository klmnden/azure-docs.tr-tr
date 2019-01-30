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
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 0fed6751d326c5da4431e953f7ded9c12688871f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250053"
---
# <a name="back-up-azure-stack"></a>Azure yığını yedekleme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

İsteğe bağlı yedekleme yerinde yedekleme ile Azure Stack üzerinde gerçekleştirin. PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure Stack için PowerShell yükleme ](azure-stack-powershell-install.md). Azure Stack'e oturum açmak için bkz: [Yönetici portalını kullanarak Azure Stack'te](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Azure Stack yedekleme Başlat

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Yeni bir yedekleme iş ilerleme durumunu izlemeyi başlatın
Yeni bir yedekleme işi ilerleme izleme ile hemen başlatmak için başlangıç AzSBackup kullanın.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Azure Stack yedekleme işi ilerleme durumu izleme Başlat
Yeni bir yedekleme başlatmak için başlangıç AzSBackup kullanın **- AsJob** parametresi ve yedekleme işinin ilerleme durumunu izlemek için bir değişken olarak kaydedin.

> [!NOTE]
> Yedekleme işi başarıyla görünür portalda yaklaşık 10-15 işi tamamlayacak dakika önce tamamlandı.
>
> Bu nedenle, gerçek durum kodu aracılığıyla daha iyi dikkate alınır.

> [!IMPORTANT]
> İlk 1 milisaniyelik gecikme kod işi doğru olarak kaydetmek hızlı olduğundan ve olmadan geri gelir sunulan **PSBeginTime** ve sırayla içermeyen **durumu** iş.

```powershell
    $BackupJob = Start-AzsBackup -Force -AsJob
    While (!$BackupJob.PSBeginTime) {
        Start-Sleep -Milliseconds 1
    }
    Write-Host "Start time: $($BackupJob.PSBeginTime)"
    While ($BackupJob.State -eq "Running") {
        Write-Host "Job is currently: $($BackupJob.State) - Duration: $((New-TimeSpan -Start ($BackupJob.PSBeginTime) -End (Get-Date)).ToString().Split(".")[0])"
        Start-Sleep -Seconds 30
    }

    If ($BackupJob.State -eq "Completed") {
        Get-AzsBackup | Where-Object {$_.BackupId -eq $BackupJob.Output.BackupId}
        $Duration = $BackupJob.Output.TimeTakenToCreate
        $Pattern = '^P?T?((?<Years>\d+)Y)?((?<Months>\d+)M)?((?<Weeks>\d+)W)?((?<Days>\d+)D)?(T((?<Hours>\d+)H)?((?<Minutes>\d+)M)?((?<Seconds>\d*(\.)?\d*)S)?)$'
        If ($Duration -match $Pattern) {
            If (!$Matches.ContainsKey("Hours")) {
                $Hours = ""
            } 
            Else {
                $Hours = ($Matches.Hours).ToString + 'h '
            }
            $Minutes = ($Matches.Minutes)
            $Seconds = [math]::round(($Matches.Seconds))
            $Runtime = '{0}{1:00}m {2:00}s' -f $Hours, $Minutes, $Seconds
        }
        Write-Host "BackupJob: $($BackupJob.Output.BackupId) - Completed with Status: $($BackupJob.Output.Status) - It took: $($Runtime) to run" -ForegroundColor Green
    }
    ElseIf ($BackupJob.State -ne "Completed") {
        $BackupJob
        $BackupJob.Output
    }
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
2. Seçin **tüm hizmetleri**ve ardından altındaki **Yönetim** kategorisi seçin > **altyapı yedeklemesine**. Seçin **yapılandırma** içinde **altyapı yedeklemesine** dikey penceresi.
3. Bulma **adı** ve **tamamlanma tarihi** yedeğin **kullanılabilir yedekler** listesi.
4. Doğrulama **durumu** olduğu **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

Bir veri kaybı olayından kurtarmak için iş akışı hakkında daha fazla bilgi edinin. Bkz: [geri dönülemez veri kaybından kurtarma](azure-stack-backup-recover-data.md).
