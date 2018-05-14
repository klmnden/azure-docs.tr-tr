---
title: Azure yığın yedekleme | Microsoft Docs
description: Yerinde yedeklemeyle Azure yığında bir talep üzerine yedekleme gerçekleştirin.
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
ms.openlocfilehash: c2a6727692a7a74b3e5fe32de8800722a9ed91b5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="back-up-azure-stack"></a>Azure yığınına yedekleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Yerinde yedeklemeyle Azure yığında bir talep üzerine yedekleme gerçekleştirin. Altyapı Yedekleme hizmetini etkinleştirmek gerekirse bkz [Azure yığınının Yönetim Portalı'ndan yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  PowerShell ortamını yapılandırma ile ilgili yönergeler için bkz: [Azure yığını için PowerShell yükleme ](azure-stack-powershell-install.md).

## <a name="start-azure-stack-backup"></a>Azure yığın Yedeklemeyi Başlat

Yükseltilmiş bir istemi işleci yönetim ortamında ile Windows PowerShell'i açın ve aşağıdaki komutları çalıştırın:

```powershell
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Yedekleme Yönetim Portalı'nda tamamlandı onaylayın

1. Azure yığın Yönetim Portalı'ndaki açmak [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Seçin **daha fazla hizmet** > **altyapı yedekleme**. Seçin **yapılandırma** içinde **altyapı yedekleme** dikey.
3. Bul **adı** ve **tamamlanma tarihi** yedeğin **kullanılabilir yedeklemeleri** listesi.
4. Doğrulama **durumu** olan **başarılı**.

<!-- You can also confirm the backup completed from the administration portal. Navigate to `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

In ‘Confirm backup completed’ section, the path at the end doesn’t make sense (ie relative to what, datetime format, etc?)
\MASBackup\<datetime>\<backupid>\BackupInfo.xml -->


## <a name="next-steps"></a>Sonraki adımlar

- Bir veri kaybı olayından kurtarmak için iş akışı hakkında daha fazla bilgi edinin. Bkz: [geri dönülemez veri kaybına karşı kurtarmak](azure-stack-backup-recover-data.md).
