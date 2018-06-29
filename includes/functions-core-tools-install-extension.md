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
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063757"
---
Yerel olarak işlevleri geliştirirken, Terminal veya bir komut isteminden Azure işlevleri çekirdek araçları kullanarak gereksinim uzantıları yükleyebilirsiniz. 

Güncelleştirdikten sonra *function.json* işlevinizi çalıştırmak gerekli tüm bağlamaları eklenecek dosyası `func extensions install` proje klasöründeki komutu. Komut okur *function.json* gerekir ve bunları yükler hangi paketlerin görmek için dosya.

Bir paketin belirli bir sürümünü yüklemek istediğiniz veya düzenlemeye başlamadan önce paketleri yüklemek istiyorsanız *function.json* dosya, kullanın `func extensions install` aşağıdaki örnekte gösterildiği gibi paket adı ile komutu:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Değiştir `<target_version>` paketin belirli bir sürümle gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org).
