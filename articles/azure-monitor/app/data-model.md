---
title: Azure Application Insights Telemetri veri modeli | Microsoft Docs
description: Application Insights veri modeline genel bakış
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 749b4077b457eff836ec515f21d97e892e663156
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60899205"
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetri veri modeli

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) uygulamanızın kullanımını ve performansını analiz etmek Azure portalında web uygulamanızdan telemetri gönderir. Telemetri modeli, böylece platform ve dilden bağımsız izleme oluşturmak mümkündür standartlaştırılmıştır. 

Application Insights tarafından toplanan veriler, bu normal bir uygulama yürütme düzeni modelleri:

![Application Insights uygulama modeli](./media/data-model/application-insights-data-model.png)

Aşağıdaki tür telemetri, uygulamanızın yürütülmesini izlemek için kullanılır. Aşağıdaki üç tür genellikle otomatik olarak Application Insights SDK'sı tarafından web Uygulama Çerçevesi ' toplanır:

* [**İstek** ](data-model-request-telemetry.md) - uygulamanız tarafından alınan isteği açmak için oluşturulan. Örneğin, Application Insights web SDK'sı, web uygulamasının aldığı her HTTP isteği için bir istek telemetri öğesinin otomatik olarak oluşturur. 

    Bir **işlemi** bir isteği işleyen yürütme iş parçacığıdır. Ayrıca [kod yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) bir "uyandır" bir web veya işi, düzenli aralıklarla işlev gibi diğer türde bir işlemi izlemek için verileri işler.  Her işlem, bir kimliği vardır. İçin kullanılabilir bu kimliği [grubu](../../azure-monitor/app/correlation.md) uygulamanızı isteği işlerken oluşturulan tüm telemetri. Her işlem ya da başarılı veya başarısız olur ve bir süre vardır.
* [**Özel durum** ](data-model-exception-telemetry.md) -tipik bir işlemin başarısız olmasına neden olan bir özel durumu temsil eder.
* [**Bağımlılık** ](data-model-dependency-telemetry.md) -bir dış hizmet ya da depolama REST API veya SQL gibi uygulamanızdan bir çağrısını temsil eder. Tarafından tanımlanan, ASP.NET SQL bağımlılık çağrıları `System.Data`. HTTP uç noktalarına çağrı tarafından tanımlanan `System.Net`. 

Application Insights özel telemetri için üç ek veri türlerini sağlar:

* [İzleme](data-model-trace-telemetry.md) - doğrudan ya da kullanılan veya tanılama günlüğünü uygulamak için bağdaştırıcıyı aşina olduğu gibi bir izleme framework kullanarak `Log4Net` veya `System.Diagnostics`.
* [Olay](data-model-event-telemetry.md) : genellikle hizmetiniz kullanım desenlerini analiz etmek, kullanıcı etkileşimi yakalamak için kullanılır.
* [Ölçüm](data-model-metric-telemetry.md) - rapor düzenli skaler ölçümler için kullanılır.

Her telemetri öğesine tanımlayabilirsiniz [bağlam bilgilerini](data-model-context.md) uygulama sürümü ya da kullanıcı oturum kimliği gibi. Belirli senaryolar engellemesinin kaldırıldığı bir dizi türü kesin belirlenmiş alanları bağlamıdır. Uygulama sürümü düzgün şekilde başlatıldığından, Application ınsights'ı yeniden dağıtma işlemi ile ilişkili uygulama davranışında yeni desenlerini algılayabilir. Oturum kimliği, kesinti veya kullanıcıların bir sorun etkisi hesaplamak için kullanılabilir. Oturum kimliği değerleri ayrı sayım belirli hesaplama bağımlılık başarısız oldu, hata izleme ya da kritik özel durum etkisi iyi bir anlayış verir.

Application Insights telemetri modeli için bir yol tanımlar [bağıntısını](../../azure-monitor/app/correlation.md) işlemi bir parçası olduğu için telemetri. Örneğin, bir istek bir SQL veritabanı aramaları ve tanılama bilgileri kaydedilir. İstek telemetri tie telemetri öğeleri için bağıntı bağlamı ayarlayabilirsiniz.

## <a name="schema-improvements"></a>Şema geliştirmeleri

Application Insights veri modeli, uygulama telemetrinizi modellemek için basit ve basit ama güçlü bir yoludur. Modelin basit ve ince önemli senaryoları desteklemek için tutmak ve Gelişmiş kullanım için şemayı genişletmek için izin almak için elimizden.

Veri modeli veya şema sorunları ve önerileri kullanın GitHub bildirmek için [Applicationınsights giriş](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) depo.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazma](../../azure-monitor/app/api-custom-events-metrics.md)
- Bilgi edinmek için nasıl [genişletmek ve telemetri filtreleme](../../azure-monitor/app/api-filtering-sampling.md).
- Kullanım [örnekleme](../../azure-monitor/app/sampling.md) telemetri veri modelini temel alan miktarını en aza indirmek için.
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
