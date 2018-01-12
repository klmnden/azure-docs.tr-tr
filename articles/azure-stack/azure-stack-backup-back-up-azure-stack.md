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
ms.openlocfilehash: daea97c0f5ee6ef855dc50c1ed6c7934aa85a1c4
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="back-up-azure-stack"></a>Azure yığınına yedekleyin

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Yerinde yedeklemeyle Azure yığında bir talep üzerine yedekleme gerçekleştirin. Altyapı Yedekleme hizmetini etkinleştirmek gerekirse bkz [Azure yığınının Yönetim Portalı'ndan yedeklemeyi etkinleştir](azure-stack-backup-enable-backup-console.md).

## <a name="start-azure-stack-backup"></a>Azure yığın Yedeklemeyi Başlat

Windows PowerShell ile yükseltilmiş istemi açın ve aşağıdaki komutları çalıştırın:

   ```powershell
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
