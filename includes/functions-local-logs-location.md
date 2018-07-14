---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/01/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88c01e8e57d4a92478b8b1ca0689ff0f8e499b39
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38739636"
---
İşlevleri konağa yerel olarak çalıştığında, aşağıdaki yola günlükler Yazar:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Windows, şirket `<DefaultTempDirectory>` TMP, TEMP USERPROFILE ilk bulunan değeri ortam değişkenleri ya da Windows dizini.
MacOS veya Linux üzerinde `<DefaultTempDirectory>` TMPDIR ortam değişkenidir.

> [!NOTE]
> İşlevleri ana bilgisayar başladığında, dizinde mevcut dosya yapısı üzerine yazar.