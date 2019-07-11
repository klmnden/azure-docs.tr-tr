---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 46376477aae9f94c1c8f6e1dd6bc718d213cc373
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712483"
---
Kapsayıcı, aşağıdaki yapılandırma ayarları vardır:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgilerini izler.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Sağlar ekleme [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) kapsayıcınızı telemetri desteği.|
|Evet|[Billing](#billing-configuration-setting)|Azure'daki hizmet kaynağının uç nokta URI'sini belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcı lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük yazma ve isteğe bağlı olarak ölçüm verileri Fluentd sunucusuna.|
|Hayır|HTTP Ara sunucusu|Bir HTTP Proxy'si Giden istekleri yapmak için yapılandırır.|
|Hayır|[Logging](#logging-settings)|Kapsayıcınız için ASP.NET Core günlük kaydı desteği sunar. |
|Hayır|[Mounts](#mount-settings)|Okur ve veri kapsayıcısı ana bilgisayar ve kapsayıcısından geri ana bilgisayara yazar.|
