---
title: Azure yedekleme sunucusu v2 sessiz yüklemesi
description: Azure yedekleme sunucusu v2 sessizce yüklemek için bir PowerShell betiğini kullanın. Bu tür bir yükleme, katılımsız bir yükleme olarak da adlandırılır.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2017
ms.author: markgal
ms.openlocfilehash: 126c1971d83a8874c096caf407231fb6dee2ff59
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606418"
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Azure yedekleme sunucusu v2 katılımsız yüklemesini çalıştırın

Azure yedekleme sunucusu v2 katılımsız yüklemesini çalıştırma hakkında bilgi edinin. 

Azure yedekleme sunucusu v1 yüklüyorsanız, bu adımları uygulamayın.

## <a name="install-backup-server-v2"></a>Yedek sunucu v2 yükleyin

1. Azure yedekleme sunucusu v2 barındıran sunucuda, bir metin dosyası oluşturun. (Dosyasını Not Defteri'nde veya başka bir metin düzenleyicisinde oluşturabilirsiniz.) Dosyayı MABSSetup.ini kaydedin. 

2. Aşağıdaki kod MABSSetup.ini dosyasına yapıştırın. Köşeli ayraçlar içindeki metni değiştirin (\< \>), ortamınızdan değerlere sahip. Aşağıdaki metni bir örnek verilmiştir:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Dosyayı kaydedin. Ardından, bir yükseltilmiş komut isteminde yükleme sunucusunda şu komutu girin:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Bu bayrakların yükleme için kullanabilirsiniz:</br>
**/f**: .ini dosya yolu</br>
**/l**: günlük dosyası yolu</br>
**/ı**: yükleme yolu</br>
**/x**: yol kaldırma</br>

## <a name="next-steps"></a>Sonraki adımlar
Yedekleme sunucusu yükledikten sonra sunucunuzu hazırlama ya da bir iş yükü korumaya başlamak öğrenin.

- [Yedekleme sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusunu yedeklemek için yedekleme sunucusu kullanın](backup-azure-backup-server-vmware.md)
- [SQL Server için yedekleme sunucusu kullanın](backup-azure-sql-mabs.md)
- [Modern yedekleme depolama yedekleme sunucusuna Ekle](backup-mabs-add-storage.md)
