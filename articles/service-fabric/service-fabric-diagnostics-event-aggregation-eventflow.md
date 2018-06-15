---
title: Azure Service Fabric olay toplama EventFlow | Microsoft Docs
description: Toplama ve izleme ve tanılama Azure Service Fabric kümeleri için EventFlow kullanarak olay toplama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 9b851b2d75cf78a02dd223788085ac9a0963376e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34211363"
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>Olay toplama ve EventFlow kullanarak koleksiyonu

[Microsoft tanılama EventFlow](https://github.com/Azure/diagnostics-eventflow) olayları bir düğümden bir veya daha fazla izleme hedeflere yönlendirebilir. Hizmet projenizdeki NuGet paketi olarak dahil olduğundan EventFlow kod ve yapılandırma hizmetiyle Azure Tanılama hakkında daha önce bahsedilen düğüm başına yapılandırma sorunu ortadan ilerler. EventFlow hizmet işlemi içinde çalıştırılan ve yapılandırılmış çıktıları doğrudan bağlanır. Doğrudan bağlantı nedeniyle EventFlow Azure, kapsayıcı ve şirket içi hizmet dağıtımları için çalışır. Her EventFlow ardışık düzen bir dış bağlantısı yaptığından EventFlow yüksek yoğunluklu senaryolarda gibi bir kapsayıcıda çalıştırırsanız dikkatli olun. Bu nedenle, çeşitli işlemleri barındırıyorsanız, birkaç giden bağlantılar alın! Bu Service Fabric uygulamaları için kadar bir sorun olduğundan değil tüm çoğaltmaları bir `ServiceType` aynı işlem içinde çalıştırın ve bu giden bağlantı sayısını sınırlar. Böylece yalnızca belirtilen filtreyle eşleşen olaylar gönderilen olay filtreleme, EventFlow de sunar.

## <a name="set-up-eventflow"></a>EventFlow ayarlayın

EventFlow ikili dosyaları NuGet paketlerini bir dizi kullanılabilir. Bir Service Fabric hizmeti projesine EventFlow eklemek, Çözüm Gezgini'nde projeye sağ tıklayın ve "Manage NuGet paketlerini." Arayın ve geçiş için "Gözat" sekmesinde "`Diagnostics.EventFlow`":

![Visual Studio NuGet Paket Yöneticisi kullanıcı Arabirimi EventFlow NuGet paketleri](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

Çeşitli paketler, "Girdi" ve "Çıkaran" etiketli gösteri listesini görürsünüz. EventFlow çeşitli farklı günlüğü sağlayıcıları ve çözümleyiciler destekler. EventFlow barındırma hizmeti için kaynak ve hedef uygulama günlüklerini bağlı olarak uygun paketleri içermelidir. Çekirdek ServiceFabric paketi yanı sıra da en az bir giriş gerekir ve çıktı yapılandırılır. Örneğin, Application Insights'a EventSource olaylarını göndermek için aşağıdaki paketler ekleyebilirsiniz:

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` verileri hizmetin EventSource sınıfı ve standart EventSources gibi yakalamak için *Microsoft ServiceFabric Hizmetleri* ve *Microsoft ServiceFabric aktörler*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` (biz günlükleri için Azure Application Insights kaynağı göndereceğiniz)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(Service Fabric hizmet yapılandırmasını EventFlow ardışık düzen başlatma sağlar ve Service Fabric sistem durumu raporlarının Tanılama verileri gönderme ile herhangi bir sorun raporları)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` Paketi, .NET Framework 4.6 veya daha yeni hedeflemek için hizmet projesi gerektiriyor. Bu paketi yüklemeden önce Proje Özellikleri'nde uygun hedef Framework'ü ayarladığınızdan emin olun.

Tüm paketler yüklendikten sonra sıradaki adım yapılandırın ve hizmeti EventFlow etkinleştirin olacak.

## <a name="configure-and-enable-log-collection"></a>Yapılandırma ve oturum toplamayı etkinleştir
Günlükleri göndermek için sorumlu EventFlow ardışık düzen yapılandırma dosyasında depolanan belirtiminden oluşturulur. `Microsoft.Diagnostics.EventFlow.ServiceFabric` Paketi yükler altında başlangıç EventFlow yapılandırma dosyası `PackageRoot\Config` adlı Çözüm klasörü `eventFlowConfig.json`. Bu yapılandırma dosyası varsayılan hizmet verilerini yakalamak için değiştirilmesi gereken `EventSource` sınıfı ve yapılandırmak ve uygun bir konuma veri göndermek istediğiniz girdi.

>[!NOTE]
>Proje dosyası Visual Studio 2017 biçimi varsa `eventFlowConfig.json` dosyası değil otomatik olarak eklenir. Düzeltmek için dosyasında oluşturma `Config` klasörü ve yapı eylemi kümesine `Copy if newer`. 

Örnek bir işte *eventFlowConfig.json* yukarıda belirtilen NuGet paketlerini göre:
```json
{
  "inputs": [
    {
      "type": "EventSource",
      "sources": [
        { "providerName": "Microsoft-ServiceFabric-Services" },
        { "providerName": "Microsoft-ServiceFabric-Actors" },
        // (replace the following value with your service's ServiceEventSource name)
        { "providerName": "your-service-EventSource-name" }
      ]
    }
  ],
  "filters": [
    {
      "type": "drop",
      "include": "Level == Verbose"
    }
  ],
  "outputs": [
    {
      "type": "ApplicationInsights",
      // (replace the following value with your AI resource's instrumentation key)
      "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
  ],
  "schemaVersion": "2016-08-11"
}
```

Hizmetin ServiceEventSource adını adı özniteliğinin değeri `EventSourceAttribute` ServiceEventSource sınıfına uygulanan. Bunu tüm belirtilen `ServiceEventSource.cs` hizmet kod parçası olan dosya. Örneğin, aşağıdaki kod parçacığında ServiceEventSource adıdır *Şirketim Application1 Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Unutmayın `eventFlowConfig.json` dosya hizmeti yapılandırma paketi bir parçasıdır. Bu dosyadaki değişiklikler tam veya yapılandırma-salt yükseltmeler Service Fabric yükseltme sistem durumu denetimlerinin ve yükseltme hatası olursa otomatik geri alma tabi hizmetinin eklenebilir. Daha fazla bilgi için bkz: [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md).

*Filtreleri* yapılandırma bölümünde daha fazla EventFlow ardışık düzen üzerinden bırakma veya belirli bilgileri dahil izin veren çıktıları giderek bilgileri özelleştirin veya olay verilerinin yapısını değiştirme olanak tanır. Filtreleri hakkında daha fazla bilgi için bkz: [EventFlow filtreleri](https://github.com/Azure/diagnostics-eventflow#filters).

EventFlow ardışık düzeninde bulunan Hizmetinizin başlangıç kodu örneği oluşturmak için son adımdır `Program.cs` dosyası:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric;
using Microsoft.ServiceFabric.Services.Runtime;

// **** EventFlow namespace
using Microsoft.Diagnostics.EventFlow.ServiceFabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                // **** Instantiate log collection via EventFlow
                using (var diagnosticsPipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MyApplication-MyService-DiagnosticsPipeline"))
                {

                    ServiceRuntime.RegisterServiceAsync("Stateless1Type",
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
}
```

Name parametresi olarak geçirilen `CreatePipeline` yöntemi `ServiceFabricDiagnosticsPipelineFactory` adı *sistem durumu varlık* EventFlow günlük toplama ardışık temsil eden. Bu ad EventFlow karşılaşırsa kullanılır ve hata ve Service Fabric sistem durumu alt raporlar.

### <a name="use-service-fabric-settings-and-application-parameters-in-eventflowconfig"></a>Service Fabric ayarları ve uygulama parametreleri eventFlowConfig içinde kullanma

EventFlow EventFlow ayarlarını yapılandırmak için Service Fabric ayarları ve uygulama parametreler kullanarak destekler. Service Fabric ayarlar parametreleri için değerleri bu özel sözdizimini kullanarak başvurabilirsiniz:

```json
servicefabric:/<section-name>/<setting-name>
```

`<section-name>` Service Fabric yapılandırma bölümü adıdır ve `<setting-name>` EventFlow ayarını yapılandırmak için kullanılan değer sağlayan yapılandırma ayarı. Okumak için bunun nasıl yapılacağı hakkında daha fazla Git [Service Fabric ayarları ve uygulama parametreleri desteği](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Doğrulama

Hizmetinizi başlatın ve hata ayıklama uyun Visual Studio çıktı penceresinde. Hizmeti başlatıldıktan sonra hizmetiniz kayıtları yapılandırdığınız çıktı gönderiyor kanıt görmeye başlayacaksınız. Olay çözümleme ve görselleştirme platformunuz için gidin ve günlükleri göstermek başlatılmış olduğunu onaylayın Yukarı (birkaç dakika sürebilir).

## <a name="next-steps"></a>Sonraki adımlar

* [Olay çözümleme ve görselleştirme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Olay çözümleme ve görselleştirme günlük analizi](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow belgeleri](https://github.com/Azure/diagnostics-eventflow)
