---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: df7326cb8e671d0f71924e813a1354dfef1e20c7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353302"
---
Beklenen değer ve varsayılan üst ve alt kenar boşlukları ile döndürülen veriler. Uygulamada, [duyarlılık] parametresi tanımlayabilir ve ardından (ExpectedValue + duyarlılık * UpperMargin) üst sınırı olarak ve (ExpectedValue - duyarlılık * LowerMargin) anomali ayarlamak için alt sınır yourselves tarafından gelin. [Duyarlılık] değeri 1'den büyük olmalıdır. Ayarlama bazı diyagramları aşağıda verilmiştir.

> [!NOTE]
> Diyagramlar örnek uygulama tarafından oluşturulmaz. Örnek uygulama ile ayrı bir araç tarafından oluşturulur.

![Ayar: duyarlılık = 1.0](../media/sensitivity_1.png)
![ayar: duyarlılık 1.5 =](../media/sensitivity_1.5.png)
![ayar: duyarlılık = 2](../media/sensitivity_2.png)
![ayar: duyarlılık 3.5 =](../media/sensitivity_3.5.png)