---
title: include dosyası
description: include dosyası
services: functions
author: craigshoemaker
ms.service: functions
ms.topic: include
ms.date: 09/25/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: fc5b43dcdee394fea023124171fb42c1a18224dc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66131428"
---
Uzantı paketleri olun ayarı aracılığıyla kullanılabilen Azure işlevleri ekibi tarafından yayımlanan tüm bağlamaları *host.json* dosya. Yerel geliştirme için en son sürümüne sahip olun [Azure işlevleri çekirdek Araçları](../articles/azure-functions/functions-run-local.md#install-the-azure-functions-core-tools).

Uzantı paketleri kullanmak için güncelleştirme *host.json* eklemek için şu girdiyi dosyaya `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

- `id` Özelliği, Microsoft Azure işlevleri uzantı paketleri için ad alanı başvurur.
- `version` Paket sürümü başvuruyor.

Paket sürümleri artırma paketler paket değiştirir. Ana sürüm değişiklikleri yalnızca paketteki paketleri bir ana sürüm taşıdığınızda gerçekleşir. `version` Özelliği kullanan [sürüm aralıklarını belirtmek için aralığı gösterimi](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). İşlevler çalışma zamanı, her zaman aralığı veya sürüm aralığı tarafından tanımlanan en fazla izin verilen sürüm seçer.

Uzantı paketleri projenizde başvuru sonra tüm varsayılan bağlamaları işlevleriniz için kullanılabilir. Kullanılabilir bağlamaları [uzantı paketini](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) şunlardır:

|Paket  |Version  |
|---------|---------|
|Microsoft.Azure.WebJobs.Extensions.CosmosDB|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.DurableTask|1.8.0|
|Microsoft.Azure.WebJobs.Extensions.EventGrid|2.0.0|
|Microsoft.Azure.WebJobs.Extensions.EventHubs|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SendGrid|3.0.0|
|Microsoft.Azure.WebJobs.Extensions.ServiceBus|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SignalRService|1.0.0|
|Microsoft.Azure.WebJobs.Extensions.Storage|3.0.4|
|Microsoft.Azure.WebJobs.Extensions.Twilio|3.0.0|