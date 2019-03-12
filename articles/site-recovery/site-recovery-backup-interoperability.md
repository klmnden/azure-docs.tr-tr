---
title: Azure Site Recovery - yedekleme birlikte çalışabilirlik | Microsoft Docs
description: Nasıl Azure Site Recovery ve Azure Backup birlikte kullanılabileceğini genel bir bakış sağlar.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/26/2019
ms.author: sideeksh
ms.openlocfilehash: 6658ab8c967c70ac1deaeba3d1dfeac602515591
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731876"
---
# <a name="about-site-recovery-and-backup-interoperability"></a>Site Recovery ve Backup birlikte çalışabilirlik hakkında

Bu makalede, Azure Iaas VM yedekleme ve Azure VM olağanüstü durum kurtarma başarılı bir şekilde kullanma yönergeleri sağlanır.

## <a name="azure-backup"></a>Azure Backup

Azure yedekleme, şirket içi sunucular, sanal makineler, sanallaştırılmış iş yükleri, SQL Server, SharePoint sunucuları ve daha fazlasını verilerini korumaya yardımcı olur. Azure Site Recovery, düzenler ve Azure Vm'leri, şirket içi Vm'leri ve fiziksel sunucular için olağanüstü durum kurtarma yönetir.

## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Backup ve Azure Site Recovery hem bir VM veya bir grup VM'yi yapılandırmak mümkündür. Her iki ürün birlikte çalışabilir. Backup ve Azure Site Recovery arasında birlikte çalışabilirliği burada önemli hale gelir birkaç senaryolar aşağıdaki gibidir:

### <a name="file-backuprestore"></a>Dosya yedekleme/geri yükleme

Yedekleme ve çoğaltma etkin ve bir yedek varsa, kaynak tarafı VM veya VM grubunun dosyalar geri hiçbir sorun yoktur. Çoğaltma zamanki çoğaltma durumu değişiklik ile devam eder.

### <a name="disk-backuprestore"></a>Disk yedekleme/geri yükleme

Disk yedekten geri yüklerseniz sanal makinenin korumasını yeniden etkinleştirilmesi gerekir.

### <a name="vm-backuprestore"></a>VM yedekleme/geri yükleme

Yedekleme ve geri yükleme bir VM veya VM desteklenmiyor. Çalışması için koruma yeniden etkinleştirilmesi gerekir.

**Senaryo** | **Azure Site Recovery tarafından destekleniyor mu?** | **Geçici çözüm, varsa**  
--- | --- | ---
Dosya/Klasör Yedekleme | Evet | Uygulanamaz
Disk yedekleme | Şu anda | Devre dışı bırakın ve korumayı etkinleştirme
VM yedeklemesi | Hayır | Devre dışı bırakın ve korumayı etkinleştirme
