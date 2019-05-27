---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b657ee32e76dd90671f7e91337ced01b925889a1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161208"
---
Cihaz sorunları yaşarsanız, sistem günlüklerinden bir destek paketi oluşturabilirsiniz. Microsoft Support sorunlarını gidermek için bu paketi kullanır. Destek paketi oluşturmak için aşağıdaki adımları izleyin:

1. [Cihazınızın PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Kullanım `Get-HcsNodeSupportPackage` bir destek paketi oluşturmak için komutu. Kullanım cmdlet aşağıdaki gibidir:

    ```powershell
    Get-HcsNodeSupportPackage [-Path] <string> [-Zip] [-ZipFileName <string>] [-Include {None | RegistryKeys | EtwLogs
            | PeriodicEtwLogs | LogFiles | DumpLog | Platform | FullDumps | MiniDumps | ClusterManagementLog | ClusterLog |
            UpdateLogs | CbsLogs | StorageCmdlets | ClusterCmdlets | ConfigurationCmdlets | KernelDump | RollbackLogs |
            Symbols | NetworkCmdlets | NetworkCmds | Fltmc | ClusterStorageLogs | UTElement | UTFlag | SmbWmiProvider |
            TimeCmds | LocalUILogs | ClusterHealthLogs | BcdeditCommand | BitLockerCommand | DirStats | ComputeRolesLogs |
            ComputeCmdlets | DeviceGuard | Manifests | MeasuredBootLogs | Stats | PeriodicStatLogs | MigrationLogs |
            RollbackSupportPackage | ArchivedLogs | Default}] [-MinimumTimestamp <datetime>] [-MaximumTimestamp <datetime>]
            [-IncludeArchived] [-IncludePeriodicStats] [-Credential <pscredential>]  [<CommonParameters>]
    ```

    Cmdlet, cihazınızın günlükleri toplar ve o günlüklerin bir belirtilen ağ veya yerel paylaşım için kopyalar.

    Kullanılan parametreler aşağıdaki gibidir:

    - `-Path` -Ağ veya destek paketi kopyalamak için yerel yol belirtin. (gerekli)
    - `-Credential` -Korumalı yola erişmek için kimlik bilgilerini belirtin.
    - `-Zip` -Bir zip dosyası oluşturmak için belirtin.
    - `-Include` -Destek paket içerisine dâhil bileşenleri dahil edileceğini belirtin. Belirtilmezse, `Default` varsayılır.
    - `-IncludeArchived` -Arşivlenmiş günlükleri desteği paket içerisine dâhil etmek belirtin.
    - `-IncludePeriodicStats` -Stat düzenli günlükleri desteği paket içerisine dâhil etmek belirtin.

    
