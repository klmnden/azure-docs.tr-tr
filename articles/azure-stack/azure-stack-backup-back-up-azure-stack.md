---
title: "Azure yığın yedekleme | Microsoft Docs"
description: "Yerinde yedeklemeyle Azure yığında bir talep üzerine yedekleme gerçekleştirin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 955b286967ca2bc8450e8988ec16c6a5c352aa8a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="back-up-azure-stack"></a>Azure yığınına yedekleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Yerinde yedeklemeyle Azure yığında bir talep üzerine yedekleme gerçekleştirin. Altyapı Yedekleme hizmetini etkinleştirmek gerekirse bkz [Azure yığınının Yönetim Portalı'ndan yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Azure yığın araçları içeren **başlangıç AzSBackup** cmdlet'i. Araçları yükleme ile ilgili yönergeler için bkz: [Azure yığınında PowerShell ile başlamak ve çalıştırmak](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

## <a name="start-azure-stack-backup"></a>Azure yığın Yedeklemeyi Başlat

Yükseltilmiş bir istemi işleci yönetim ortamında ile Windows PowerShell'i açın ve aşağıdaki komutları çalıştırın:

```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1

    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Yedekleme Yönetim Portalı'nda tamamlandı onaylayın

1. Azure yığın Yönetim Portalı'ndaki açmak [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Seçin **daha fazla hizmet** > **altyapı yedekleme**. Seçin **yapılandırma** içinde **altyapı yedekleme** dikey.
3. Bul **adı** ve **tamamlanma tarihi** yedeğin **kullanılabilir yedeklemeleri** listesi.
4. Doğrulama **durumu** olan **başarılı**.

Yönetim Portalı'ndan tamamlandı yedekleme de onaylayabilirsiniz. Gidin`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Sonraki adımlar

- Bir veri kaybı olayından kurtarmak için iş akışı hakkında daha fazla bilgi edinin. Bkz: [geri dönülemez veri kaybına karşı kurtarmak](azure-stack-backup-recover-data.md).
