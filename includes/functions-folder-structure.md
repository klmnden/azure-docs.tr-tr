---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9f74365f3fe935be45fa9c45e5b12c45b97b2f8a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068443"
---
Belirli bir işlev uygulamasında tüm işlevleri için kod ana bilgisayar yapılandırma dosyası içeren bir kök proje klasörü ve bir veya daha fazla alt klasörleri içinde yer alır. Her alt aşağıdaki gösterimi olduğu gibi ayrı bir işlev kodunu içerir:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

İçinde sürüm 2.x işlev uygulamasında tüm işlevleri işlevler çalışma zamanının aynı dil yığını paylaşım gerekir.  

[Host.json](../articles/azure-functions/functions-host-json.md) bazı çalışma zamanı özel yapılandırmaları içeren bir dosyadır işlev uygulaması kök klasöründe. A `bin` klasörü paketler ve işlev uygulaması için gereken diğer kitaplık dosyalarını içerir. İşlev uygulaması projesi için dile özgü gereksinimleri bölümünü inceleyin:

* [C# sınıf kitaplığı (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#komut dosyası](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



