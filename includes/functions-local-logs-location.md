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
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "33814637"
---
İşlevler konak yerel olarak çalıştığında, aşağıdaki yola günlükler Yazar:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

Windows, `<DefaultTempDirectory>` ilk bulunan TMP, TEMP USERPROFILE değeri ortam değişkenleri veya Windows dizinidir.
MacOS ya da Linux `<DefaultTempDirectory>` TMPDIR ortam değişkenidir.

> [!NOTE]
> İşlevler ana bilgisayar başladığında, varolan dosya yapısı dizininde üzerine yazar.