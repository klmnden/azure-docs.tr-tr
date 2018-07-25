---
title: Azure Stack yedekleme | Microsoft Docs
description: İsteğe bağlı yedekleme yerinde yedekleme ile Azure Stack üzerinde gerçekleştirin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/08/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: c9e7ffae1b988d0940d10acdb1b387a25e0466ec
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242895"
---
# <a name="back-up-azure-stack"></a>Azure yığını yedekleme

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

İsteğe bağlı yedekleme yerinde yedekleme ile Azure Stack üzerinde gerçekleştirin. PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure Stack için PowerShell yükleme ](azure-stack-powershell-install.md). Azure Stack'e oturum açmak için bkz: [işleci ortamı yapılandırmak ve Azure Stack için oturum açma](azure-stack-powershell-configure-admin.md).

## <a name="start-azure-stack-backup"></a>Azure Stack yedekleme Başlat

İlerleme durumunu izlemek için - AsJob değişkenle yeni bir yedekleme başlatmak için başlangıç AzSBackup kullanın. 

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output
```

## <a name="confirm-backup-completed-via-powershell"></a>Yedekleme PowerShell tamamlandı onaylayın

```powershell
    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

- Sonuç aşağıdaki çıktı gibi görünmelidir:

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

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Yedekleme Yönetim Portalı'nda tamamlandı onaylayın

1. Azure Stack Yönetim Portalı'ndaki açın [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Seçin **diğer hizmetler** > **altyapı yedeklemesine**. Seçin **yapılandırma** içinde **altyapı yedeklemesine** dikey penceresi.
3. Bulma **adı** ve **tamamlanma tarihi** yedeğin **kullanılabilir yedekler** listesi.
4. Doğrulama **durumu** olduğu **başarılı**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir veri kaybı olayından kurtarmak için iş akışı hakkında daha fazla bilgi edinin. Bkz: [geri dönülemez veri kaybından kurtarma](azure-stack-backup-recover-data.md).