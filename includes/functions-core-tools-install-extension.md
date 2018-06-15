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
ms.openlocfilehash: 6fb497a5b6da00dece43c7f41ea3c411f385a2ba
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726896"
---
Yerel olarak işlevleri geliştirirken, Terminal veya bir komut isteminden Azure işlevleri çekirdek araçları kullanarak gereksinim uzantıları yükleyebilirsiniz. 

Güncelleştirdikten sonra *function.json* işlevinizi çalıştırmak gerekli tüm bağlamaları eklenecek dosyası `func extensions install` proje klasöründeki komutu. Komut okur *function.json* gerekir ve bunları yükler hangi paketlerin görmek için dosya.

Bir paketin belirli bir sürümünü yüklemek istediğiniz veya düzenlemeye başlamadan önce paketleri yüklemek istiyorsanız *function.json* dosya, kullanın `func extensions install` aşağıdaki örnekte gösterildiği gibi paket adı ile komutu:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Değiştir `<target_version>` paketi belirli bir sürümüne sahip. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org).
