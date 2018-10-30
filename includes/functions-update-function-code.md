---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/16/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d264477693458ff4132c1f69704a480eed2756e0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988022"
---
## <a name="update-the-function"></a>İşlevi güncelleştirme

Varsayılan olarak şablon, istekte bulunurken bir işlev anahtarı gerektiren bir işlev oluşturur. Azure’da işlevi test etmeyi kolaylaştırmak üzere, işlevi anonim erişime izin vermek için güncelleştirmeniz gerekir. Bu değişikliği yapmanızın yolu işlev projenizin diline bağlıdır.

### <a name="c"></a>C\#

Yeni işleviniz olan MyHttpTrigger.cs kod dosyasını açın ve işlev tanımındaki **AuthorizationLevel** özniteliğini `anonymous` değerine güncelleştirin ve değişikliklerinizi kaydedin.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>JavaScript

Yeni işlevinizin function.json dosyasını açın, dosyayı bir metin düzenleyicide açın, **bindings.httpTrigger**’daki **authLevel** özelliğini `anonymous`’e güncelleştirin ve değişikliklerinizi kaydedin.

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
