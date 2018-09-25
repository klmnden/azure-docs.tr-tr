---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 09/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1b53c53b1e5fb089eb9b8a9b816b11a1eea126d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044518"
---
İşlevleri yerel olarak geliştirme yaptığınızda, Azure işlevleri çekirdek araçları Terminal veya komut satırından kullanarak gereksinim duyduğunuz uzantıların yükleyebilirsiniz.

Güncelleştirdikten sonra *function.json* dosyasını proje klasöründe aşağıdaki komutu çalıştırın, işlevinizi gereken tüm bağlamaları içerecek şekilde.

```bash
func extensions install
```

Komut okur *function.json* ihtiyacınız paketler görmek için bir dosya yükler ve uzantıları projesi oluşturur. Geçerli sürümde yeni bağlamalar ekler, ancak var olan bağlamaları güncelleştirmez. Kullanım `--force` yenilerini yüklerken mevcut bağlamaları en son sürüme güncelleştirmek için seçeneği.

Bir paketin belirli bir sürümünü yüklemek istediğiniz veya düzenlemeden önce paketleri yüklemek isterseniz *function.json* dosya, kullanın `func extensions install` aşağıdaki örnekte gösterildiği gibi paket adı ile komutu:

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Değiştirin `<target_version>` belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org).
