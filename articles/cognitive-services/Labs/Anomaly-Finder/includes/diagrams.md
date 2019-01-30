---
title: include dosyası
description: include dosyası
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 79ae38db73d55021572d04f693e5cb809e9bd056
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228976"
---
Döndürülen veri beklenen değere sahip olmasının yanı sıra varsayılan üst ve alt marjların içindeydi. Uygulamada bir [duyarlılık] parametresi belirleyip üst sınır olarak (Beklenen Değer + duyarlılık * Üst Marj) ve alt sınır olarak (Beklenen Değer - duyarlılık * Alt Marj) kullanıp anomali noktasını kendiniz belirleyebilirsiniz. [Duyarlılık] değeri 1'den büyük olmalıdır. Aşağıda ayarlayabileceğiniz bazı diyagramlar verilmiştir.

> [!NOTE]
> Bu diyagramlar örnek uygulama ile oluşturulamamıştır. Örnek uygulama ile birlikte kullanılan ayrı bir araçla oluşturulmuştur.

![Ayar: duyarlılık = 1.0](../media/sensitivity_1.png)
![Ayar: duyarlılık = 1.5](../media/sensitivity_1.5.png)
![Ayar: duyarlılık = 2](../media/sensitivity_2.png)
![Ayar: duyarlılık = 3.5](../media/sensitivity_3.5.png)
