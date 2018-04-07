---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4c9b579534d9a7f2c55e9c589b1738fe060b1cf2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
Yerel olarak işlevleri geliştirirken, Terminal veya bir komut isteminden Azure işlevleri çekirdek araçları kullanarak gereksinim uzantıları yükleyebilirsiniz. Aşağıdaki `func extensions install` komutu Azure Cosmos DB bağlama uzantısı yükler:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Değiştir `<taget_version>` paketi belirli bir sürümüne sahip. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org).
