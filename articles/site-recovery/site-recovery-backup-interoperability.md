---
title: Azure Site Recovery, Azure Backup ile kullanma desteği | Microsoft Docs
description: Nasıl Azure Site Recovery ve Azure Backup birlikte kullanılabileceğini genel bir bakış sağlar.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: e902f70225ec0eb0caa98f7e19a16c87220cb6f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035784"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Site Recovery, Azure Backup ile kullanma desteği

Bu makalede kullanma desteği özetler [Site Recovery hizmeti](site-recovery-overview.md) ile birlikte [Azure Backup hizmeti](https://docs.microsoft.com/azure/backup/backup-overview).

**Eylem** | **Site kurtarma desteği** | **Ayrıntılar**
--- | --- | ---
**Hizmetlerini birlikte dağıtın** | Desteklenen | Hizmet, birlikte çalışabilir ve birlikte yapılandırılabilir.
**Dosya yedekleme/geri yükleme** | Desteklenen | Yedeklemeleri alınır ve bir VM için yedekleme ve çoğaltma etkin olduğunda hiçbir sorun kaynak tarafı VM'ler veya VM dosyaları geri yüklenmesi. Çoğaltma, zamanki çoğaltma durumu değişiklik ile devam eder.
**Disk yedekleme/geri yükleme** | Geçerli desteği yok | Yedeklenen bir diski geri yükleme, devre dışı bırakın ve sanal makine için çoğaltmayı yeniden etkinleştirmeniz gerekir.
**VM yedekleme/geri yükleme** | Geçerli desteği yok | Yedeklemek veya bir VM veya VM geri yükleme, devre dışı bırakın ve sanal makine için çoğaltmayı etkinleştirmeniz gerekir.  


