---
title: Parametreli URL'lerle Azure Time Series Insights özel görünümlerini paylaşma | Microsoft Docs
description: Bu makalede, özelleştirilmiş görünümün kolayca paylaşılabilmesi için Azure Time Series Insights'ta parametreli URL'lerin nasıl geliştirileceği açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: conceptual
ms.workload: big-data
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: dfc04397b1d7e9f3256810cbe469067ae52c99bd
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66238970"
---
# <a name="share-a-custom-view-using-a-parameterized-url"></a>Parametreli URL'yi kullanarak özel görünümü paylaşma

Time Series Insights Gezgini özel bir görünümü paylaşmak için programlama yoluyla Özel görünümün parametreli bir URL'sini oluşturabilirsiniz.

Time Series Insights Gezgini, doğrudan URL'den gerçekleştirilen bir deneyimde görünümleri belirtmek için URL sorgu parametrelerini destekler. Örneğin, yalnızca URL'yi kullanarak hedef ortamı, arama koşulunu ve istenen zaman aralığını belirtebilirsiniz. Kullanıcı özelleştirilmiş URL'ye tıkladığında, arabirim Time Series Insights portalında doğrudan bu varlığın bağlantısını sağlar. Veri erişimi ilkeleri uygulanır.

> [!TIP]
> * Ücretsiz görüntülemek [Time Series Insights tanıtım](https://insights.timeseries.azure.com/samples).
> * Eşlik eden okuma [Time Series Insights Gezgini](./time-series-insights-explorer.md) belgeleri.

## <a name="environment-id"></a>Ortam Kimliği

`environmentId=<guid>` parametresi hedef ortam kimliğini belirtir. Veri erişim FQDN'SİNİN bir bileşenidir ve Azure portalında ortam özeti sağ üst köşesinde bulabilirsiniz. `env.timeseries.azure.com` bölümünden önce gelen her şey, bu değeri oluşturur.

Örnek bir ortam kimliği parametresi olarak `?environmentId=10000000-0000-0000-0000-100000000108` verilebilir.

## <a name="time"></a>Time

Parametreli URL ile mutlak veya göreli zaman değerleri belirtebilirsiniz.

### <a name="absolute-time-values"></a>Mutlak zaman değerleri

Mutlak zaman değerleri için, `from=<integer>` ve `to=<integer>` parametrelerini kullanın.

* `from=<integer>`, arama aralığının başlangıç zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.
* `to=<integer>`, arama aralığının bitiş zamanını gösteren JavaScript milisaniyesi cinsinden bir değerdir.

Tarihe ilişkin JavaScript milisaniyesini belirlemek için bkz. [Epoch ve Unix Zaman Damgası Dönüştürücüsü](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).

### <a name="relative-time-values"></a>Göreli zaman değerleri

Göreli bir zaman değeri için `relativeMillis=<value>` kullanın; burada *değer*, arka uçtaki en son verilerden gelen JavaScript milisaniyesi cinsinden bir değerdir.

Örneğin, `&relativeMillis=3600000` en 60 dakikanın verilerini görüntüler.

Değerler, Time Series Insights Gezgini karşılık gelen kabul **hızlı süresi** menüsünde ve içerir:

* `1800000` (Son 30 dakika)
* `3600000` (Son 60 dakika)
* `10800000` (Son 3 saat)
* `21600000` (Son 6 saat)
* `43200000` (Son 12 saat)
* `86400000` (Son 24 saat)
* `604800000` (Son 7 gün)
* `2592000000` (Son 30 saat)

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

`timeSeriesDefinitions=<collection of term objects>` Parametresi Time Series Insights görünümünün dönemlerini belirtir:

| Parametre | URL öğesi | Açıklama |
| --- | --- | --- |
| **name** | `\<string>` | *Dönem* adı. |
| **splitBy** | `\<string>` | *Bölme ölçütü* sütunun adı. |
| **measureName** | `\<string>` | *Ölçü* sütununun adı. |
| **Karşılaştırma** | `\<string>` | Sunucu tarafı filtrelemesi için *where* yan tümcesi. |
| **useSum** | `true` | Toplam için toplam kullanmayı belirten isteğe bağlı parametre. </br>  Unutmayın, `Events` seçilen ölçü sayısı, varsayılan olarak seçilidir.  </br>  Varsa `Events` olan seçilmedi, ortalama varsayılan olarak seçilidir. |

* `multiChartStack=<true/false>` Anahtar-değer çifti grafikte yığın oluşturmayı sağlar.
* `multiChartSameScale=<true/false>` Anahtar-değer çifti, isteğe bağlı bir parametre içindeki terimler arasında aynı y ekseni ölçeğini sağlar.  
* `timeBucketUnit=<Unit>&timeBucketSize=<integer>` Aralık kaydırıcısını daha ayrıntılı veya düz, sayesinde daha toplu bir grafik görünümü.  
* `timezoneOffset=<integer>` Parametresi, grafiğin içinde UTC'ye uzaklık cinsinden görüntülenecek saat dilimini ayarlamanızı sağlar.

| Çiftlerini | Açıklama |
| --- | --- |
| `multiChartStack=false` | `true` Varsayılan olarak etkin şekilde geçirin `false` yığın için. |
| `multiChartStack=false&multiChartSameScale=true` | Terimler arasında aynı Y ekseni ölçeğini kullanmak için yığın oluşturmanın etkinleştirilmesi gerekir.  Sahip `false` varsayılan olarak, bu nedenle 'true' geçirerek bu işlevi etkinleştirir. |
| `timeBucketUnit=<Unit>&timeBucketSize=<integer>` | Birimler = gün, saat, dakika, saniye, milisaniye.  Her zaman birimin ilk harfini büyük yapın. </br> timeBucketSize için istediğiniz tamsayıyı geçererek birim sayısını tanımlayın.  En fazla 7 güne kadar düzleştirebilirsiniz.  |
| `timezoneOffset=-<integer>` | Bu tamsayı her zaman milisaniye cinsindendir. </br> Bu işlevsellik, ne biz yerel (tarayıcı saati) ya da UTC seçmenizi etkinleştiririz burada Time Series Insights Gezgininde etkinleştirdiğimiz değerinden biraz farklıdır. |

### <a name="examples"></a>Örnekler

Bir zaman serisi görüşleri ortamına bir URL parametresi olarak zaman serisi tanımları eklemek için aşağıdakileri ekleyin:

```plaintext
&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},
{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

Örnek zaman serisi tanımları için kullanın:

* Ortam kimliği
* Son 60 dakikalık veriler
* İsteğe bağlı parametreleri koşulları (f1pressureıd, F2TempStation ve F3VibrationPL)

bir görünüm için aşağıdaki parametreli URL'yi oluşturabilirsiniz:

```plaintext
https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]
```

> [!TIP]
> Canlı gezginini görebilirsiniz [URL'yi kullanarak](https://insights.timeseries.azure.com/samples?environmentId=10000000-0000-0000-0000-100000000108&relativeMillis=3600000&timeSeriesDefinitions=[{"name":"F1PressureId","splitBy":"Id","measureName":"Pressure","predicate":"'Factory1'"},{"name":"F2TempStation","splitBy":"Station","measureName":"Temperature","predicate":"'Factory2'"},{"name":"F3VibrationPL","splitBy":"ProductionLine","measureName":"Vibration","predicate":"'Factory3'"}]).

Yukarıdaki URL'yi açıklar ve Time Series Insights Gezgini görünümü oluşturur:

[![Zaman serisi görüşleri Gezgini dönemler](media/parameterized-url/url1.png)](media/parameterized-url/url1.png#lightbox)

Tam görünüm (grafik dahil):

[![Grafik görünümü](media/parameterized-url/url2.png)](media/parameterized-url/url2.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [sorgu kullanarak verileri C# ](time-series-insights-query-data-csharp.md).

* Hakkında bilgi edinin [zaman serisi öngörüleri Gezgininde](./time-series-insights-explorer.md).
