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
ms.openlocfilehash: 2808264b4641bda49a53677ebe216a3b53b7d0d9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293485"
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

* [C# sınıf kitaplığı (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# betiği (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F # betiği](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



