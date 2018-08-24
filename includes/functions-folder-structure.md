---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cbe634d862682a5f6b06c2cfc77a4d3b03954f9
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811607"
---
Belirli bir işlev uygulamasında tüm işlevleri için kod bir kök klasöründe (`wwwroot`) bir ana bilgisayar yapılandırma dosyası ve bir veya daha fazla alt klasör içerir. Aşağıdaki örnekteki gibi ayrı bir işlev kodunu her alt klasör içerir:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
 | - bin
 | | - mycompiledcsharp.dll
```

Host.json dosya bazı çalışma zamanı özel yapılandırmaları içerir ve işlev uygulaması kök klasöründe bulunur. Kullanılabilir ayarlar hakkında daha fazla bilgi için bkz: [host.json başvurusu](../articles/azure-functions/functions-host-json.md).

Her işlev bir veya daha fazla kod dosyaları, function.json yapılandırma ve diğer bağımlılıklar içeren bir klasörü vardır. C# sınıf kitaplığı projesi, için derlenmiş bir sınıf kitaplığı (.dll) dosyası için dağıtılan `bin` alt.

