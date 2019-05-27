---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 48bb91b3b2e9a31de63e515edb857bc2a170ea79
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132250"
---
## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

Azure işlev uygulaması oluşturulduktan sonra kullanabileceğiniz [ `func azure functionapp publish` ](../articles/azure-functions/functions-run-local.md#project-file-deployment) proje kodunuzu Azure'a dağıtmak için temel araçları komutu. Aşağıdaki komutta `<APP_NAME>` uygulamanızı önceki adımdan adı.

```bash
func azure functionapp publish <APP_NAME>
```

Okunabilirliği artırmak için kesilmiştir aşağıdakine benzer bir çıktı görürsünüz.

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Artık Azure'da işlevinizi test etmek için kullanabilirsiniz, HttpTrigger için çağırma URL'si değerini kopyalayın. URL içeren bir `code` dize değeri işlevi anahtarınızı sorgudur. Bu anahtar, başkalarının Azure'da HTTP tetikleyici bitiş çağrısı zorlaştırır.