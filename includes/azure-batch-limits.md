---
title: include dosyası
description: include dosyası
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 22bfc3c86605f4c2eed4c022919b3643f394ea95
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080918"
---
| **Kaynak** | **Varsayılan sınır** | **Üst sınırı** |
| --- | --- | --- |
| Abonelik başına bölgeye göre Azure Batch hesapları | 1-3 |50 |
| Batch hesabı başına adanmış çekirdekler | 90-900 | Desteğe başvurun |
| Batch hesabı başına düşük öncelikli çekirdekler | 10-100 | Desteğe başvurun |
| **[Etkin](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)**  işler ve iş zamanlamaları Batch hesabı başına (**tamamlandı** işlere sahip sınır yok) | 100-300 | 1,000<sup>1</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> Varsayılan sınırları bir Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişir. Batch hizmeti modunda Batch hesapları gösterilen çekirdek kotaları içindir. [Batch hesabınızda kotaları görüntüleme](../articles/batch/batch-quota-limit.md#view-batch-quotas).

<sup>1</sup>bu sınırı aşan miktarda bir artış istemek için Azure desteğine başvurun.
