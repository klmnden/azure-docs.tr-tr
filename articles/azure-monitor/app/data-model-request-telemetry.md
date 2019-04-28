---
title: Azure Application Insights Telemetri veri modeli - Telemetri isteği | Microsoft Docs
description: İstek telemetrisi için Application Insights veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/07/2019
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: fef016d87cc60bc916fdcb08f92171e115221fe5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60900536"
---
# <a name="request-telemetry-application-insights-data-model"></a>İstek telemetrisi: Application Insights veri modeli

Bir istek telemetri öğesinin (içinde [Application Insights](../../azure-monitor/app/app-insights-overview.md)) mantıksal uygulamanız için bir dış istek tarafından tetiklenen yürütme sırası temsil eder. Her istek yürütme benzersiz tarafından tanımlanır `ID` ve `url` yürütme parametreleri içeren. İstekler mantıksal olarak gruplayabilirsiniz `name` ve tanımlama `source` bu isteği. Kod yürütmeyi sonuçlanabilir `success` veya `fail` ve belirli bir `duration`. Hem başarılı hem de hata yürütme gruplandırılmış tarafından daha fazla `resultCode`. Başlangıç saati Zarf düzeyinde tanımlanan istek telemetrisi.

İstek telemetri destekleyen özel kullanarak standart genişletilebilirlik modeli `properties` ve `measurements`.

## <a name="name"></a>Ad

İstek adının isteği işlemek için geçen kod yolu temsil eder. Daha iyi isteklerinin gruplandırma izin vermek için düşük kardinalite değeri. HTTP istekleri için HTTP yöntemi ve gibi URL yolu şablonunu temsil eden `GET /values/{id}` gerçek olmadan `id` değeri.

Application Insights web SDK "olduğu gibi" istek adı harf bakımından gönderir. Kullanıcı arabiriminde gruplandırma küçük harfe duyarlı şekilde `GET /Home/Index` alanından ayrı olarak sayılır `GET /home/INDEX` genellikle bunlar aynı denetleyici ve eylem yürütülmesine neden olsa bile. Bu URL'leri genel olduğunu sebebi [büyük/küçük harfe](https://www.w3.org/TR/WD-html40-970708/htmlweb.html). Tüm görmek isteyebilirsiniz `404` harfle yazılmış URL'ler için oldu. ASP.NET Web SDK'sı tarafından üzerinde daha fazla istek adı koleksiyonunu edinebilirsiniz [blog gönderisi](https://apmtips.com/blog/2015/02/23/request-name-and-url/).

En fazla uzunluk: 1024 karakter

## <a name="id"></a>Kimlik

Bir istek çağrısını örneği tanımlayıcısı. İstek ve diğer telemetri öğeleri arasında bağıntı kullanılır. Kimliği'nın, genel olarak benzersiz olması. Daha fazla bilgi için [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

En fazla uzunluk: 128 karakter

## <a name="url"></a>Url

Tüm sorgu dizesi parametreleri ile istek URL'si.

En fazla uzunluk: 2048 karakter

## <a name="source"></a>Kaynak

İstek kaynağı. Arayanın izleme anahtarını veya IP adresini arayanın verilebilir. Daha fazla bilgi için [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

En fazla uzunluk: 1024 karakter

## <a name="duration"></a>Süre

İstek süresi biçimde: `DD.HH:MM:SS.MMMMMM`. Pozitif ve daha az olmalıdır daha `1000` gün. İstek telemetrisi başında ve sonunda işlemi temsil ettiğinden bu gerekli bir alandır.

## <a name="response-code"></a>Yanıt kodu

İstek yürütme sonucu. HTTP isteklerini HTTP durum kodu. Olabilir `HRESULT` diğer istek türleri için değer ya da özel durum türü.

En fazla uzunluk: 1024 karakter

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi. Bu alan zorunludur. Açıkça ayarlandığında değil `false` -bir istek başarılı olarak kabul edilir. Bu değer kümesine `false` işlemi özel durum tarafından kesildi veya hata sonuç kodunu döndürdü.

Yanıt kodu olduğunda web uygulamaları için Application Insights istek başarılı tanımlamak küçüktür `400` veya buna eşit `401`. Ancak bu varsayılan eşleme anlam uygulamanın eşleşmediğinde durumlar vardır. Yanıt kodu `404` "normal bir akışın parçası olabilecek hiç kayıt" gösterebilir. Bozuk bağlantı da işaret edebilir. Bozuk bağlantılar için bile daha gelişmiş mantığı uygulayabilir. Yalnızca bu bağlantıları aynı sitede url başvuran analiz ederek bulunduğunda bağlantıların hatalar olarak işaretleyebilirsiniz. Veya bunları şirketin mobil uygulamasından erişildiğinde hatası olarak işaretleyin. Benzer şekilde `301` ve `302` yeniden yönlendirme desteği olmayan istemciden erişildiğinde başarısız olduğunu gösterir.

İçerik'kısmen kabul `206` genel bir istek bir hata gösterebilir. Örneğin, Application Insights uç nokta telemetri öğelerini toplu tek bir istek alır. Döndürür `206` olduğunda toplu işlem kümesindeki bazı öğeler değil başarıyla işlendi. Artan oranını `206` araştırılması gereken bir sorun olduğunu gösterir. Benzer bir mantık uygular `207` başarı ayrı yanıt kodları en kötü olabilir burada birden fazla durum.

Daha fazla üzerinde istek sonucu okuyabilirsiniz kodu ve durum kodunu [blog gönderisi](https://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bir istek telemetri yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)
- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- Bilgi edinmek için nasıl [ASP.NET Core yapılandırma](../../azure-monitor/app/asp-net.md) Application Insights ile uygulama.
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
