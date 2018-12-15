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
ms.openlocfilehash: 26789a12053fa6275b09836e706c391e181c8efd
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430053"
---
## <a name="create-a-function"></a>İşlev oluşturma

Aşağıdaki komut HTTP ile tetiklenen `MyHttpTrigger` adlı bir işlev oluşturur.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```
