---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2019
ms.openlocfilehash: 4f9300ab1d688e1f5043f5b919e70c5a36c7c0e7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064000"
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
