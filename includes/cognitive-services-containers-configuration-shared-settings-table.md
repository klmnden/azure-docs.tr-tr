---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2019
ms.openlocfilehash: c4abc5fa89b48b6fc55637e9ff3b259387d0d410
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796383"
---
Bu kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgileri izlemek için kullanılır.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Eklemenizi sağlayan [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Billing](#billing-configuration-setting)|Azure'daki hizmet kaynağının uç nokta URI'sini belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcı lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|HTTP Ara sunucusu|Bir HTTP Proxy'si Giden istekleri yapmak için yapılandırın.|
|Hayır|[Logging](#logging-settings)|Kapsayıcınız için ASP.NET Core günlük kaydı desteği sunar. |
|Hayır|[Mounts](#mount-settings)|Ana bilgisayardaki verileri okuyup kapsayıcıya, kapsayıcıdaki verileri okuyup ana bilgisayara yazar.|
