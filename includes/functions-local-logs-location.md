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
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
İşlevler konak yerel olarak çalıştığında, aşağıdaki yola günlükler Yazar:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Windows, `<DefaultTempDirectory>` ilk bulunan TMP, TEMP USERPROFILE değeri ortam değişkenleri veya Windows dizinidir.
MacOS ya da Linux `<DefaultTempDirectory>` TMPDIR ortam değişkenidir.

> [!NOTE]
> İşlevler ana bilgisayar başladığında, varolan dosya yapısı dizininde üzerine yazar.