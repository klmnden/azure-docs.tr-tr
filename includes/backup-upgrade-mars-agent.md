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
ms.date: 8/29/2018
ms.author: markgal
ms.openlocfilehash: 2a969240da6e16f5a5ba76605a6efa2a96f9e4dd
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285648"
---
## <a name="upgrade-the-mars-agent"></a>MARS Aracısı'nı yükseltme

Microsoft Azure kurtarma hizmeti (MARS) aracısını 2.0.9083.0 aşağıda Azure erişim denetimi hizmeti (ACS) bir bağımlılık vardır. MARS Aracısı Azure Backup aracısı olarak da bilinir. 2018'den Azure içinde [Azure erişim denetimi hizmeti (ACS) kullanım dışı](../articles/active-directory/develop/active-directory-acs-migration.md). 19 Mart 2018'den itibaren yedekleme hataları 2.0.9083.0 aşağıda MARS agent'ın tüm sürümleri karşılaşırsınız. Yedekleme hatalarını gidermek veya önlemek için [MARS aracınızı en son sürüme yükseltme](https://go.microsoft.com/fwlink/?linkid=229525). MARS Aracısı yükseltmesini gerektiren sunucuları tanımlamak için adımları izleyin. [MARS aracıları yükseltmek için yedekleme blog](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/). MARS Aracısı, dosyaları ve klasörleri ve sistem durumu verileri Azure'a yedekleme için kullanılır. System Center DPM ve Azure Backup sunucusu verileri Azure'a yedeklemek için MARS Aracısı'nı kullanın.
