---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 57e54c4c3b8ae44d42051c87356f1feeaa5a3f10
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977217"
---
Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Faturalandırma](#billing-setting)|Azure'da uç nokta hizmet kaynağın URI'sini belirtir.|
|Evet|[EULA'sı](#eula-setting)| Kapsayıcı lisansını kabul ettiğiniz gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[Günlüğe kaydetme](#logging-settings)|ASP.NET Core günlüğü kapsayıcınız için destek sağlar. |
|Hayır|[Bağlar](#mount-settings)|Okuyup veri kapsayıcısı ana bilgisayarından kapsayıcısından geri ana bilgisayara yazma.|
