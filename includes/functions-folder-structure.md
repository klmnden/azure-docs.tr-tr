---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
origin.date: 09/12/2018
ms.date: 10/19/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 27dc1b1315a8e33b8ac13b34d4a86ad0343388b4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731252"
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

İçinde sürüm 2.x işlev uygulamasında tüm işlevleri işlevler çalışma zamanının aynı dil çalışan paylaşım gerekir.  

[Host.json](../articles/azure-functions/functions-host-json.md) bazı çalışma zamanı özel yapılandırmaları içeren bir dosyadır işlev uygulaması kök klasöründe. A `bin` klasörü paketler ve işlev uygulaması için gereken diğer kitaplık dosyalarını içerir. İşlev uygulaması projesi için dile özgü gereksinimleri bölümünü inceleyin:

- [C# sınıf kitaplığı (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
- [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
- [F#komut dosyası](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
- [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
- [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)


<!-- ms.date: 10/19/2018 -->

