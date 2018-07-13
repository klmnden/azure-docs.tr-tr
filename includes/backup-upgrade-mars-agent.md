---
title: Azure Backup Aracısı'nı yükseltme
description: Bu bilgiler, neden Azure Backup aracısını yükseltmeniz gerekir ve yükseltme karşıdan yükleme konumu açıklar.
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 3/16/2018
ms.author: markgal
ms.openlocfilehash: 4142f88c3ccb376acbdd2af07c5cfdf8fd275780
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38750339"
---
## <a name="upgrade-the-mars-agent"></a>MARS Aracısı'nı yükseltme
Microsoft Azure kurtarma hizmeti (MARS) aracısını 2.0.9083.0 aşağıda Azure erişim denetimi hizmeti (ACS) bir bağımlılık vardır. 2018'de Azure olacak [Azure erişim denetimi hizmeti (ACS) kullanımdan](../articles/active-directory/develop/active-directory-acs-migration.md). 19 Mart 2018'den itibaren yedekleme hataları 2.0.9083.0 aşağıda MARS agent'ın tüm sürümleri karşılaşırsınız. Yedekleme hatalarını gidermek veya önlemek için [MARS aracınızı en son sürüme yükseltme](https://go.microsoft.com/fwlink/?linkid=229525). MARS Aracısı yükseltme gerektiren sunucuları tanımlamak için adımları izleyin. [Azure Backup aracıları yükseltmek için yedekleme blog](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). MARS Aracısı, aşağıdaki veri türlerini Azure'a yedeklemek için kullanılır:
- Dosyalar 
- Sistem Durumu verileri
- System Center DPM'yi Azure'a yedekleme
- Azure Backup Sunucusu (MABS)
