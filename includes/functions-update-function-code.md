---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 03/12/2019
ms.author: glenga
ms.custom: include file, fasttrack-edit
ms.openlocfilehash: 5b009fafc818a06bdda309b3e025251cc0997e47
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737324"
---
## <a name="update-the-function"></a>İşlevi güncelleştirme

Varsayılan olarak şablon, istekte bulunurken bir işlev anahtarı gerektiren bir işlev oluşturur. Azure’da işlevi test etmeyi kolaylaştırmak üzere, işlevi anonim erişime izin vermek için güncelleştirmeniz gerekir. Bu değişikliği yapmanızın yolu işlev projenizin diline bağlıdır.

### <a name="c"></a>C\#

Yeni işleviniz olan MyHttpTrigger.cs kod dosyasını açın ve işlev tanımındaki **AuthorizationLevel** özniteliğini `Anonymous` değerine güncelleştirin ve değişikliklerinizi kaydedin.

```csharp
[FunctionName("MyHttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req,
    ILogger log)
```

### <a name="javascript"></a>JavaScript

Yeni işlevinizde function.json dosyasını bir metin düzenleyicisinde açın, güncelleştirme **authLevel** özelliğinde **bağlamaları** için `anonymous`ve değişikliklerinizi kaydedin.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Artık işlev anahtarını sağlamanıza gerek kalmadan işlevi Azure’da çağırabilirsiniz. İşlevi anahtarı, yerel olarak çalıştırılırken hiçbir zaman gerekli değildir.
