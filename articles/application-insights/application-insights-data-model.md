---
title: Azure uygulama Insights Telemetri veri modeli | Microsoft Docs
description: "Uygulama Öngörüler veri modeline genel bakış"
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
ms.openlocfilehash: b14eea46e773a4b92ba20cd3121cd258f86307c9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-telemetry-data-model"></a>Uygulama Insights telemetri veri modeli

[Azure Application Insights](app-insights-overview.md) uygulamanızın kullanımını ve performansını analiz edebilirsiniz böylece web uygulamanızdan Azure portalına telemetri gönderir. Böylece platform ve dilden bağımsız izleme oluşturmak mümkün telemetri modeli standartlaştırılmıştır. 

Application Insights tarafından toplanan verileri bu genel uygulama yürütme desen modelleri:

![Uygulama Öngörüler uygulama modeli](./media/application-insights-data-model/application-insights-data-model.png)

Aşağıdaki telemetri türlerini uygulamanızın yürütülmesini izlemek için kullanılır. Aşağıdaki üç tür genellikle otomatik olarak tarafından Application Insights SDK'sı web uygulama çerçevesinden toplanır:

* [**İstek** ](application-insights-data-model-request-telemetry.md) - uygulamanız tarafından alınan isteği oturum oluşturulmuş. Örneğin, Application Insights web SDK'sı web uygulamanızı alır her HTTP isteği için bir istek telemetri öğesi otomatik olarak oluşturur. 

    Bir **işlemi** bir isteği işler yürütme iş parçacıklarının sayısıdır. Ayrıca [kod yazmayı](app-insights-api-custom-events-metrics.md#trackrequest) bir "uyandırmak" bir web işi veya, düzenli aralıklarla işlev gibi işlem, başka türlerde izlemek için verileri işler.  Her bir işlemin bir kimliği vardır. İçin kullanılabilir bu kimliği [grup](application-insights-correlation.md) uygulamanızı isteği işlerken oluşturulan tüm telemetri. Her işlem ya da başarılı veya başarısız olur ve bir süre vardır.
* [**Özel durum** ](application-insights-data-model-exception-telemetry.md) -genellikle bir işlemin başarısız olmasına neden olan bir özel durumu temsil eder.
* [**Bağımlılık** ](application-insights-data-model-dependency-telemetry.md) -çağrı uygulamanızdan bir dış hizmet ya da bir REST API veya SQL gibi depolama temsil eder. ASP.NET, SQL bağımlılık çağrıları tarafından tanımlanan `System.Data`. HTTP uç noktaları çağrıları tarafından tanımlanan `System.Net`. 

Application Insights üç ek veri türleri için özel telemetri sağlar:

* [İzleme](application-insights-data-model-trace-telemetry.md) - ya da doğrudan kullanılan ya da tanılama günlük uygulamak için bağdaştırıcıyı aşina olduğu gibi bir araç framework kullanarak `Log4Net` veya `System.Diagnostics`.
* [Olay](application-insights-data-model-event-telemetry.md) - hizmetinizle kullanım desenlerini çözümlemek için kullanıcı etkileşimi yakalamak için genellikle kullanılan.
* [Ölçüm](application-insights-data-model-metric-telemetry.md) - rapor düzenli skaler ölçümler için kullanılan.

Her telemetri öğesi tanımlayabilirsiniz [bağlam bilgilerini](application-insights-data-model-context.md) uygulama sürümü ya da kullanıcı oturum kimliği gibi. Bağlam belirli senaryolar engelini kaldırır kesin türü belirtilmiş alanları kümesidir. Uygulama sürümü düzgün başlatılmadı, Application Insights yeni desenleri çözümünüzün yeniden dağıtımını ile ilişkili uygulama davranış algılayabilir. Oturum kimliği kesinti veya kullanıcıların sorun etkisini hesaplamak için kullanılır. Oturum kimliği değerlerin ayrı sayım belirli hesaplama bağımlılık başarısız oldu, hata izleme veya kritik bir özel durumla bir etkisi iyi anlamış sağlar.

Uygulama Insights telemetri modeli tanımlayan bir şekilde [bağıntısını](application-insights-correlation.md) işleminin bir parçası olmasından telemetri. Örneğin, bir istek bir SQL veritabanı çağrılarını yapabilirsiniz ve tanılama bilgileri kaydedilir. İstek telemetri bağlamanın telemetriyi öğelerden bağıntı bağlamının ayarlayabilirsiniz.

## <a name="schema-improvements"></a>Şema geliştirmeleri

Uygulama Öngörüler veri modeli, uygulama telemetrinizi modellemek için basit ve basit ancak güçlü bir yoludur. Modelin basit ve temel senaryoları desteklemek için ince tutmak ve Gelişmiş kullanım için şemayı genişletmek için izin vermek çalışmalarımızı.

Veri modeli veya şema sorunları ve önerileri kullanın GitHub bildirmek için [Applicationınsights giriş](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) deposu.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazın](app-insights-api-custom-events-metrics.md)
- Bilgi edinmek için nasıl [genişletmek ve filtre telemetri](app-insights-api-filtering-sampling.md).
- Kullanım [örnekleme](app-insights-sampling.md) veri modelini temel alan telemetri en aza indirmek için.
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
