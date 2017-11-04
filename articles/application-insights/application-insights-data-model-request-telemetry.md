---
title: "Azure Application Insights Telemetri Data Model - Telemetri isteği | Microsoft Docs"
description: "İstek telemetri için uygulama Öngörüler veri modeli"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: 0073f38097ffbebd669754eac5f2d48a620941bf
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="request-telemetry-application-insights-data-model"></a>Telemetri isteği: Application Insights veri modeli

Bir istek telemetri öğesi (içinde [Application Insights](app-insights-overview.md)) uygulamanız için bir dış istek tarafından tetiklenen yürütme mantıksal dizisini temsil eder. Her istek yürütme benzersiz tarafından tanımlanan `ID` ve `url` tüm yürütme parametreleri içeren. İsteği mantıksal olarak gruplayabilirsiniz `name` ve tanımlayın `source` bu isteğin. Kod yürütmeyi sonuçlanabilir `success` veya `fail` ve belirli bir sahip `duration`. Başarı ve başarısızlık yürütmeleri gruplandırılmış tarafından daha fazla `resultCode`. Zarf düzeyinde tanımlanan istek telemetri için başlangıç saati.

Telemetri özel kullanarak standart genişletilebilirlik modelini destekleyen isteği `properties` ve `measurements`.

## <a name="name"></a>Ad

İstek adının isteğinin işlenmesi için geçen kod yolu temsil eder. Daha iyi isteklerinin gruplandırma izin vermek için düşük önem düzeyi değeri. HTTP istekleri için URL yolu şablonu gibi ve HTTP yöntemi temsil eden `GET /values/{id}` gerçek olmadan `id` değeri.

Uygulama Insights web SDK "olduğu gibi" isteği adı harf göre gönderir. UI gruplandırılması büyük küçük harfe duyarlı şekilde `GET /Home/Index` gelen ayrı olarak sayılır `GET /home/INDEX` rağmen çoğunlukla bunlar aynı denetleyici ve eylem yürütme sonucu. Bu URL'leri genel olduğunu nedeni [büyük küçük harfe duyarlı](http://www.w3.org/TR/WD-html40-970708/htmlweb.html). Tüm görmek isteyebilirsiniz `404` büyük yazılan URL'ler için oldu. ASP.Net Web SDK'sı tarafından üzerinde daha fazla isteği adı koleksiyonu okuyabilirsiniz [blog gönderisi](http://apmtips.com/blog/2015/02/23/request-name-and-url/).

En fazla uzunluk: 1024 karakter

## <a name="id"></a>Kimlik

Bir istek çağrısı örneği tanımlayıcısı. İstek ve diğer telemetri öğeleri arasında bağıntı için kullanılır. Kimliği küresel olarak benzersiz olmalıdır. Daha fazla bilgi için bkz: [bağıntı](application-insights-correlation.md) sayfası.

En fazla uzunluk: 128 karakter

## <a name="url"></a>Url

Tüm sorgu dizesi parametreleri ile istek URL'si.

En fazla uzunluk: 2048 karakter

## <a name="source"></a>Kaynak

İstek kaynağı. Arayanın izleme anahtarı veya IP adresi arayanın örnektir. Daha fazla bilgi için bkz: [bağıntı](application-insights-correlation.md) sayfası.

En fazla uzunluk: 1024 karakter

## <a name="duration"></a>Süre

Süre biçimde istek: `DD.HH:MM:SS.MMMMMM`. Pozitif ve daha az olmalıdır daha `1000` gün. İstek telemetri başında ve sonunda işlemi temsil eden gibi bu gerekli bir alandır.

## <a name="response-code"></a>Yanıt kodu

Bir istek yürütme sonucu. HTTP isteklerini HTTP durum kodu. Bu olabilir `HRESULT` diğer istek türleri için değer veya özel durum türü.

En fazla uzunluk: 1024 karakter

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrının gösterimi. Bu alan gereklidir. Açıkça ayarlandığında değil `false` -istek kabul başarılı olması için. Bu değer ayarlanırsa `false` işlemi özel durum nedeniyle kesildi veya bir hata sonucu kodu döndürdü.

Web uygulamaları için Application Insights istek yanıt kodu daha küçük olduğunda başarısız olarak tanımlamak `400` veya buna eşit `401`. Ancak bu varsayılan eşleme anlamsal uygulamanın eşleşmediğinde durumlar vardır. Yanıt kodu `404` "Normal akışının parçası olabilen hiç kayıt" gösteriyor olabilir. Bozuk bir bağlantı da işaret edebilir. Bozuk bağlantıları için daha gelişmiş mantık bile uygulayabilirsiniz. Yalnızca bu bağlantıları aynı sitede url başvuran çözümleyerek bulunduğunda bağlantıların hataları olarak işaretleyebilirsiniz. Ya da onları şirketin mobil uygulamadan erişildiğinde hataları olarak işaretleyin. Benzer şekilde `301` ve `302` yeniden yönlendirme desteklemiyor istemciden erişildiğinde başarısız olduğunu gösterir.

Kısmen, içerik kabul `206` genel bir istek bir hata göstergesi olabilir. Örneğin, Application Insights endpoint telemetri öğeleri toplu tek bir istek alır. Döndürdüğü `206` zaman toplu işleminde bazı öğeler değil başarıyla işlendi. Artan oranını `206` araştırılması gereken bir sorun olduğunu gösterir. Benzer mantığı uygular `207` burada başarılı olabilir ayrı yanıt kodları en kötü birden fazla durum.

Daha fazla üzerinde istek sonucu okuyabilir kodu ve durum kodunu [blog gönderisi](http://apmtips.com/blog/2016/12/03/request-success-and-response-code/).

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümleri

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bir istek telemetri yazma](app-insights-api-custom-events-metrics.md#trackrequest)
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Bilgi edinmek için nasıl [ASP.NET Core yapılandırmak](app-insights-asp-net.md) Application Insights ile uygulama.
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
