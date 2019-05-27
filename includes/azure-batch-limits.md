---
title: include dosyası
description: include dosyası
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 9ffb02fce41e8805dfccf1dfd6e982cf107039ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127537"
---
| **Kaynak** | **Varsayılan sınır** | **Üst sınırı** |
| --- | --- | --- |
| Abonelik başına bölgeye göre Azure Batch hesapları | 1-3 |50 |
| Batch hesabı başına adanmış çekirdekler | 10-100 | N/A<sup>1</sup> |
| Batch hesabı başına düşük öncelikli çekirdekler | 10-100 | YOK<sup>2</sup> |
| Etkin işler ve iş zamanlamaları<sup>3</sup> Batch hesabı başına | 100-300 | 1,000<sup>4</sup> |
| Batch hesabı başına havuz sayısı | 20-100 | 500<sup>4</sup> |

> [!NOTE]
> Varsayılan sınırları bir Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişir. Batch hizmeti modunda Batch hesapları gösterilen çekirdek kotaları içindir. [Batch hesabınızda kotaları görüntüleme](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup>Batch hesabı başına adanmış çekirdekler sayısı artırılabilir, ancak en büyük sayı belirtilmemiş. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

<sup>2</sup>Batch hesabı başına düşük öncelikli çekirdek sayısı artırılabilir, ancak en büyük sayı belirtilmemiş. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

<sup>3</sup>tamamlanan işler ve iş zamanlamaları sınırlı değildir.

<sup>4</sup>bu sınırı aşan miktarda bir artış istemek için Azure desteğine başvurun.
