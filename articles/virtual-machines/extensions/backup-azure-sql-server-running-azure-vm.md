---
title: Azure VM'de çalışan SQL Server için Azure yedekleme
description: Azure VM'de çalışan Azure yedekleme SQL Server'ı kaydetme
services: backup
author: swatisachdeva
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: swatisachdeva
ms.openlocfilehash: 8710242e04156c8af6e5882a3cb4d42cc31e3677
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607617"
---
# <a name="azure-backup-for-sql-server-running-in-azure-vm"></a>Azure VM'de çalışan SQL Server için Azure yedekleme

Azure Backup, diğer teklifler arasında desteği SQL Server gibi iş yüklerini yedeklemek için Azure Vm'lerinde çalışan sağlar. SQL uygulaması içinde bir Azure VM çalışıyor olduğundan, yedekleme hizmeti uygulamaya erişim ve gerekli bilgileri getirmek için izniniz gerekiyor.
Bunu yapmak için Azure Backup yükler **AzureBackupWindowsWorkload** içinde SQL Server çalıştıran, kullanıcı tarafından tetiklenen kayıt işlemi sırasında VM uzantısı.

## <a name="prerequisites"></a>Önkoşullar

Desteklenen senaryolar listesi için başvurmak [desteklenebilirliği matris](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#scenario-support) Azure Backup tarafından desteklenir.

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure yedekleme, NSG etiketler, bir proxy sunucusu dağıtma destekler veya IP aralıkları listelenen; Bu yöntemlerin her biri hakkında daha fazla bilgi için bkz [makale](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#establish-network-connectivity).

## <a name="extension-schema"></a>Uzantı şeması

Uzantı şema ve özellik değerlerini CRP API'sine geçen hizmet yapılandırma (çalışma zamanı ayarları) değerlerdir. Bu yapılandırma değerleri, kayıt ve yükseltme sırasında kullanılır. **AzureBackupWindowsWorkload** uzantısı da bu şemayı kullanın. Şemanın önceden ayarlanmış; Yeni bir parametre objectStr alanına eklenebilir

  ```json
      "runtimeSettings": [{
      "handlerSettings": {
      "protectedSettingsCertThumbprint": "",
      "protectedSettings": {
      "objectStr": "",
      "logsBlobUri": "",
      "statusBlobUri": ""
      }
      },
      "publicSettings": {
      "locale": "en-us",
      "taskId": "1c0ae461-9d3b-418c-a505-bb31dfe2095d",
      "objectStr": "",
      "commandStartTimeUTCTicks": "636295005824665976",
      "vmType": "vmType"
      }
      }]
      }
  ```

Aşağıdaki JSON şema WorkloadBackup uzantısı gösterir.  

  ```json
  {
    "type": "extensions",
    "name": "WorkloadBackup",
    "location":"<myLocation>",
    "properties": {
      "publisher": "Microsoft.RecoveryServices",
      "type": "AzureBackupWindowsWorkload",
      "typeHandlerVersion": "1.1",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "locale":"<location>",
        "taskId":"<TaskId used by Azure Backup service to communicate with extension>",

        "objectStr": "<The configuration passed by Azure Backup service to extension>",

        "commandStartTimeUTCTicks": "<Scheduled start time of registration or upgrade task>",
        "vmType": "<Type of VM where registration got triggered Eg. Compute or ClassicCompute>"
      },
      "protectedSettings": {
        "objectStr": "<The sensitive configuration passed by Azure Backup service to extension>",
        "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
        "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
      }
    }
  }
  ```

### <a name="property-values"></a>Özellik değerleri

Ad | Değeri/örneği | Veri türü
 --- | --- | ---
Yerel ayar | En-us  |  dize
taskId | "1c0ae461-9d3b-418c-a505-bb31dfe2095d"  | dize
objectStr <br/> (publicSettings)  | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5O DI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ == " | dize
commandStartTimeUTCTicks | "636967192566036845"  | dize
vmType  | "microsoft.compute/virtualmachines"  | dize
objectStr <br/> (protectedSettings) | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIjoiMzVjMjQxYTItOGRjNy00ZGE5LWI4NTMtMjdjYTJhNDZlM2ZkIiwiSWRNZ210Q29udGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTkwOWMyMGVjNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJkNGEzOTliNy1iYjAyLTQ2MWMtODdmYS1jNTM5O DI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZjODUtNTQ4OS00NmNhLWEyZTctNWMzNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdmMuY29tIn1dfQ == " | dize
logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | dize
statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | dize


## <a name="template-deployment"></a>Şablon dağıtımı

Bir sanal makine AzureBackupWindowsWorkload uzantı sanal makinede SQL Server Yedekleme etkinleştirerek olduğu ekleme önerilir. Bu, aracılığıyla gerçekleştirilebilir [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) bir SQL Server sanal makinede yedeklemeyi otomatik hale getirmek için tasarlanmıştır.


## <a name="powershell-deployment"></a>PowerShell dağıtım

'Bir kurtarma Hizmetleri kasası ile SQL uygulamayı içeren bir Azure VM kaydetmek ' gerekir. Kayıt sırasında AzureBackupWindowsWorkload uzantısı VM'de yüklü. Kullanım [Register-AzRecoveryServicesBackupContainerPS](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) cmdlet'i, VM kaydedilemiyor.
 
```powershell
$myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
```
 
Komut bir **yedekleme kapsayıcısı** kaynak ve durum bu olacaktır **kayıtlı**.


## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-sql-server-azure-troubleshoot) Azure SQL Server VM yedeklemesi hakkında sorun giderme kılavuzları
- [Sık sorulan sorular](https://docs.microsoft.com/azure/backup/faq-backup-sql-server) Azure sanal makinelerinde (VM'ler) çalıştıran ve Azure Backup hizmeti kullanan SQL Server veritabanlarını yedekleme hakkında.
