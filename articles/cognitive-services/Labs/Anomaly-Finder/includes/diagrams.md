---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 506270b1828e98f14e3fe7a84b7f780e209e2669
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165115"
---
Döndürülen veri beklenen değere sahip olmasının yanı sıra varsayılan üst ve alt marjların içindeydi. Uygulamada bir [duyarlılık] parametresi belirleyip üst sınır olarak (Beklenen Değer + duyarlılık * Üst Marj) ve alt sınır olarak (Beklenen Değer - duyarlılık * Alt Marj) kullanıp anomali noktasını kendiniz belirleyebilirsiniz. [Duyarlılık] değeri 1'den büyük olmalıdır. Aşağıda ayarlayabileceğiniz bazı diyagramlar verilmiştir.

> [!NOTE]
> Bu diyagramlar örnek uygulama ile oluşturulamamıştır. Örnek uygulama ile birlikte kullanılan ayrı bir araçla oluşturulmuştur.

![Ayar: duyarlılık = 1.0](../media/sensitivity_1.png)
![Ayar: duyarlılık = 1.5](../media/sensitivity_1.5.png)
![Ayar: duyarlılık = 2](../media/sensitivity_2.png)
![Ayar: duyarlılık = 3.5](../media/sensitivity_3.5.png)