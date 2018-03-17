---
title: "Azure Backup Aracısı yükseltme"
description: "Bu bilgiler neden Azure Backup aracısını yükseltmeniz gerekir ve nereden yükseltme yükleneceğini açıklar."
services: backup
cloud: 
suite: 
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 3/16/2018
ms.author: markgal
ms.openlocfilehash: 4142f88c3ccb376acbdd2af07c5cfdf8fd275780
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
## <a name="upgrade-the-mars-agent"></a>MARS Aracısı'nı yükseltmek
Microsoft Azure kurtarma hizmeti (MARS) Aracısı 2.0.9083.0 aşağıda sürümlerinde Azure erişim denetimi hizmeti (ACS) bir bağımlılık vardır. Azure içinde 2018 olacak [Azure erişim denetimi hizmeti (ACS) alanı onaylanamadı](../articles/active-directory/develop/active-directory-acs-migration.md). 19 Mart 2018 başlayarak 2.0.9083.0 aşağıda MARS Aracısı'nin tüm sürümleri yedekleme hataları karşılaşırsınız. Önlemenize veya yedekleme hataları gidermek için [MARS Aracısı en son sürüme yükseltme](https://go.microsoft.com/fwlink/?linkid=229525). Bir MARS Aracısı yükseltme gerektiren sunucuları belirlemek için adımları [Azure Backup aracısının yükseltilmesi için yedekleme blog](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). MARS Aracısı'nı aşağıdaki veri türlerini Azure'a yedekleme için kullanılır:
- Dosyalar 
- Sistem Durumu verileri
- System Center DPM bir yedekleme Azure
- Azure Backup Sunucusu (MABS)
