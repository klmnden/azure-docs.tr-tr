---
title: include dosyası
description: include dosyası
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: c98a2146a019817152be9fae76638dbaa4d9de3d
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49458885"
---
| **Kaynak** | **Varsayılan Sınır** | **Üst Sınır** |
| --- | --- | --- |
| Her abonelikteki bölge başına Batch hesabı sayısı | 1 - 3 |50 |
| Batch hesabı başına adanmış çekirdekler | 10 - 100 | N/A<sup>1</sup> |
| Batch hesabı başına düşük öncelikli çekirdekler | 10 - 100 | YOK<sup>2</sup> |
| Etkin işler ve iş zamanlamaları<sup>3</sup> Batch hesabı başına | 100 - 300 | 1000<sup>4</sup> |
| Batch hesabı başına havuz sayısı | 20 - 100 | 500<sup>4</sup> |

> [!NOTE]
> Varsayılan sınırları bir Batch hesabı oluşturmak için kullandığınız abonelik türüne bağlı olarak değişir. Batch hizmeti modunda Batch hesapları gösterilen çekirdek kotaları içindir. [Batch hesabınızda kotaları görüntüleme](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup> Batch hesabı başına adanmış çekirdekler sayısı artırılabilir, ancak en büyük sayı belirtilmemiş. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

<sup>2</sup> Batch hesabı başına düşük öncelikli çekirdek sayısı artırılabilir, ancak en büyük sayı belirtilmemiş. Artırma seçeneklerini görüşmek için Azure desteğine başvurun.

<sup>3</sup> tamamlanan işler ve iş zamanlamaları sınırlı değildir.

<sup>4</sup> istediğinizde bu sınırı aşan miktarda bir artış istemek Azure desteğine başvurun.
