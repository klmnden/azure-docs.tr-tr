---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: fa1046e8d752e133813924957b439b63fc2acfbd
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988011"
---
## <a name="create-a-function"></a>İşlev oluşturma

Aşağıdaki komut HTTP ile tetiklenen `MyHtpTrigger` adlı bir işlev oluşturur.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```