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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38943052"
---
İşlevleri yerel olarak geliştirme yaptığınızda, Azure işlevleri çekirdek araçları Terminal veya komut satırından kullanarak gereksinim duyduğunuz uzantıların yükleyebilirsiniz. 

Güncelleştirdikten sonra *function.json* işlevinizi çalıştırmak gerekli tüm bağlamaları içeren dosyaya `func extensions install` proje klasöründeki komutu. Komut okur *function.json* gerekir ve ardından bunları yükler paketler görmek için dosya.

Bir paketin belirli bir sürümünü yüklemek istediğiniz veya düzenlemeden önce paketleri yüklemek isterseniz *function.json* dosya, kullanın `func extensions install` aşağıdaki örnekte gösterildiği gibi paket adı ile komutu:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Değiştirin `<target_version>` belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org).
