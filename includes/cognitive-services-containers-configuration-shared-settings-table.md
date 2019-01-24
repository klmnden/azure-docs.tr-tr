---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2019
ms.openlocfilehash: 1a3de57446c5c92afbc5dd5901e284bc3580f9db
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479020"
---
Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Faturalandırma](#billing-setting)|Azure'da uç nokta hizmet kaynağın URI'sini belirtir.|
|Evet|[EULA'sı](#eula-setting)| Kapsayıcı lisansını kabul ettiğiniz gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|[HTTP Ara sunucusu](#http-proxy-credentials-settings)|Bir HTTP Proxy'si Giden istekleri yapmak için yapılandırın.|
|Hayır|[Günlüğe kaydetme](#logging-settings)|ASP.NET Core günlüğü kapsayıcınız için destek sağlar. |
|Hayır|[Bağlar](#mount-settings)|Okuyup veri kapsayıcısı ana bilgisayarından kapsayıcısından geri ana bilgisayara yazma.|
