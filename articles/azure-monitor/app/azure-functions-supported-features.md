---
title: Azure Application Insights - Azure işlevleri desteklenen özellikler | Microsoft Docs
description: Azure işlevleri için Application ınsights'ı desteklenen özellikler
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 0199d8f0c4a76a10fffcab7cf2819643d0ac2d68
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075351"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application ınsights'ı Azure işlevleri için desteklenen özellikler

Azure işlevleri tekliflerini [yerleşik tümleştirme](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) Application Insights ile olan ILogger arabirimi aracılığıyla kullanılabilir. Şu anda desteklenen özelliklerin listesi aşağıda verilmiştir. Azure işlevleri Kılavuzu gözden [Başlarken](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Desteklenen özellikler

| Azure İşlevleri                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application ınsights'ı .NET SDK'sı**   | **2.5.0**       | **2.9.1**         |
| | | | 
| **Otomatik olarak toplama**        |                 |                   |               
| &bull; İstekleri                     | Evet             | Evet               | 
| &bull; Özel durumlar                   | Evet             | Evet               | 
| &bull; Performans sayaçları         | Evet             | Evet               |
| &bull; Bağımlılıkları                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Evet               | 
| | | | 
| **Desteklenen özellikler**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Evet             | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; Güvenli denetim kanalı|                 | Evet               | 
| &bull; Örnekleme                     | Evet             | Evet               | 
| &bull; Sinyal                   |                 | Evet               | 
| | | | 
| **Bağıntı**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Evet               | 
| &bull; EventHub                       |                   | Evet               | 
| | | | 
| **Yapılandırılabilir**                      |                   |                   |           
| &bull;Tam olarak yapılandırılabilir.<br/>Bkz: [Azure işlevleri](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) yönergeler için.<br/>Bkz: [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) tüm seçenekler için.               |                   | Evet                   | 


## <a name="performance-counters"></a>Performans Sayaçları

Otomatik performans sayaçlarını toplamayı yalnızca Windows makineleri çalışır.


## <a name="live-metrics--secure-control-channel"></a>Canlı Ölçümler ve güvenli denetim kanalı

Belirttiğiniz özel filtreler ölçütlere geri Application Insights SDK'sı Canlı ölçümleri bileşeni gönderilir. Filtreler, büyük olasılıkla customerIDs gibi hassas bilgiler içerebilir. Kanal güvenli sahip gizli bir API anahtarı yapabilirsiniz. Bkz: [denetim kanalı güvenli](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) yönergeler için.

## <a name="sampling"></a>Örnekleme

Azure işlevleri, varsayılan olarak, yapılandırmada örnekleme sağlar. Daha fazla bilgi için [örnekleme yapılandırma](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

Projeniz el ile telemetri izleme yapmak için Application Insights SDK üzerinde bir bağımlılık alırsa, örnekleme yapılandırmanızı işlevler örnekleme yapılandırmadan farklı ise, garip davranışlar karşılaşabilirsiniz. 

İşlevleri aynı yapılandırmayı kullanmanızı öneririz. İle **işlevler v2**, oluşturucu, bağımlılık ekleme kullanılarak yapılandırmanın alabilirsiniz:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
