---
title: EventFlow ile Azure Service Fabric olay toplama | Microsoft Docs
description: Toplama ve izleme ve tanılama Azure Service Fabric kümelerinin EventFlow kullanarak olaylarını toplama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: bdc6c9476529b986f425d56544fd4b1afd8a864e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668305"
---
# <a name="event-aggregation-and-collection-using-eventflow"></a>Olay toplama ve koleksiyon EventFlow kullanma

[Microsoft Diagnostics EventFlow](https://github.com/Azure/diagnostics-eventflow) olayları bir düğümünden bir veya daha fazla izleme hedeflere yönlendirebilirsiniz. Hizmet projenizi bir NuGet paketi olarak dahil olduğu için Azure Tanılama hakkında daha önce bahsedilen düğüm başına yapılandırma sorunu ortadan hizmetiyle EventFlow kod ve yapılandırma seyahat. EventFlow, hizmet işlemi içinde çalıştırılan ve yapılandırılmış çıktıları doğrudan bağlanır. Azure, kapsayıcı ve şirket içi hizmet dağıtımları için doğrudan bağlantı nedeniyle EventFlow çalışır. Her EventFlow işlem hattı, bir dış bağlantısı yaptığından EventFlow yüksek yoğunluklu senaryolarda, örneğin bir kapsayıcıda çalıştırırsanız dikkatli olun. Bu nedenle, çeşitli işlemlerin barındırıyorsanız, birkaç giden bağlantılar alın! Bu, Service Fabric uygulamaları kadar önemli değildir, tüm çoğaltmalar bir `ServiceType` aynı işlem içinde çalıştırın ve bu giden bağlantı sayısını kısıtlar. Belirtilen filtreyle eşleşen olaylar gönderilir, böylece olay filtrelemeyi, EventFlow de sunar.

## <a name="set-up-eventflow"></a>EventFlow ayarlayın

EventFlow ikili dosyaları, bir dizi NuGet paketleri kullanılabilir. EventFlow bir Service Fabric hizmet projesine eklemek için Çözüm Gezgini'nde projeye sağ tıklayın ve "Manage NuGet paketlerini." Geçiş için "Gözatma türü" sekmesinde ve arama "`Diagnostics.EventFlow`":

![Visual Studio NuGet Paket Yöneticisi UI EventFlow NuGet paketleri](./media/service-fabric-diagnostics-event-aggregation-eventflow/eventflow-nuget.png)

Çeşitli paketler, "Giriş" ve "Çıkaran" ile etiketlenmiş Göster listesini görürsünüz. EventFlow çeşitli farklı günlük sağlayıcıları ve çözümleyiciler destekler. EventFlow barındırma hizmeti, uygulama günlükleri için hedef ve kaynak bağlı olarak uygun paketleri içermelidir. Çekirdek ServiceFabric paketi yanı sıra ayrıca en az bir giriş gerekir ve çıkış yapılandırılır. Örneğin, Application Insights'a EventSource olayları göndermek için aşağıdaki paketler ekleyebilirsiniz:

* `Microsoft.Diagnostics.EventFlow.Inputs.EventSource` hizmetin EventSource sınıfı ve standart EventSources veri gibi yakalamak için *Microsoft'la ServiceFabric* ve *Microsoft ServiceFabric aktörler*)
* `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights` (bir Azure Application Insights kaynağına günlük gönderme kullanacağız)
* `Microsoft.Diagnostics.EventFlow.ServiceFabric`(Service Fabric hizmeti yapılandırması EventFlow hattından başlatmasını sağlar ve Service Fabric sistem durumu raporları gibi tanılama verilerini gönderme herhangi bir sorunu raporlar)

>[!NOTE]
>`Microsoft.Diagnostics.EventFlow.Inputs.EventSource` Paket, .NET Framework 4.6 veya daha yeni hizmet projede gerektirir. Bu paketi yüklemeden önce uygun bir hedef çerçeve proje özelliklerinde ayarladığınızdan emin olun.

Tüm paketler yüklendikten sonra sonraki adıma yapılandırın ve hizmeti EventFlow etkinleştirin sağlamaktır.

## <a name="configure-and-enable-log-collection"></a>Yapılandırma ve günlük toplamayı etkinleştir
Günlükleri göndermek için sorumlu EventFlow işlem hattı, bir yapılandırma dosyasında depolanan belirtiminden oluşturulur. `Microsoft.Diagnostics.EventFlow.ServiceFabric` Paket yükler altında bir başlangıç EventFlow yapılandırma dosyası `PackageRoot\Config` adlı Çözüm klasörü `eventFlowConfig.json`. Bu yapılandırma dosyasını varsayılan hizmetinden verileri yakalamak için değiştirilmesi gereken `EventSource` sınıfı ve yapılandırmak ve uygun bir konuma veri göndermek istediğiniz diğer girişler.

>[!NOTE]
>Visual Studio 2017 biçimi proje dosyanız varsa `eventFlowConfig.json` dosya otomatik olarak eklenmeyecek. Bu düzeltmek için dosyayı oluşturmak `Config` klasörü ve derleme eylemi kümesine `Copy if newer`. 

Bir örneği aşağıdadır *eventFlowConfig.json* yukarıda belirtilen NuGet paketlerini göre:
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

Hizmetin ServiceEventSource adını ad özelliği değeri `EventSourceAttribute` ServiceEventSource sınıfa uygulanır. Bunu tüm belirtildiğinden `ServiceEventSource.cs` hizmet kodunun bir parçası olan dosya. Örneğin, aşağıdaki kod parçacığında ServiceEventSource adıdır *Şirketim Application1 Stateless1*:

```csharp
[EventSource(Name = "MyCompany-Application1-Stateless1")]
internal sealed class ServiceEventSource : EventSource
{
    // (rest of ServiceEventSource implementation)
}
```

Unutmayın `eventFlowConfig.json` dosya hizmeti yapılandırma paketi bir parçasıdır. Bu dosyada yapılan değişiklikler, Service Fabric yükseltme sistem durumu denetimleri ve yükseltme hatası olması durumunda otomatik geri alma tabi hizmetinin tam veya yapılandırma-yalnızca yükseltmelerinde eklenebilir. Daha fazla bilgi için [Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md).

*Filtreleri* yapılandırma bölümü, daha fazla EventFlow işlem hattı üzerinden çıktıları bırakın veya belirli bilgileri dahil etmenize imkan sağlar, Git yükleneceği bilgisini özelleştirin veya yapısını değiştirmek sağlar Olay verileri. Filtreleme hakkında daha fazla bilgi için bkz: [EventFlow filtreleri](https://github.com/Azure/diagnostics-eventflow#filters).

EventFlow hizmetinizin başlatma kodunu bulunan, işlem hattında örneklemek için son adımdır `Program.cs` dosyası:

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

Adı parametre olarak geçirilen `CreatePipeline` yöntemi `ServiceFabricDiagnosticsPipelineFactory` adıdır *sistem durumu varlık* EventFlow günlük toplama işlem hattı temsil eden. EventFlow ile karşılaşırsa, bu ad kullanılır ve hata ve Service Fabric sistem durumu alt sistemini bildirir.

### <a name="use-service-fabric-settings-and-application-parameters-in-eventflowconfig"></a>Service Fabric ayarları ve uygulama parametreleri eventFlowConfig içinde kullanın.

EventFlow EventFlow ayarlarını yapılandırmak için Service Fabric ayarları ve uygulama parametreleri kullanarak destekler. Service Fabric ayarlar parametreleri için değerleri bu özel bir sözdizimi kullanarak başvurabilirsiniz:

```json
servicefabric:/<section-name>/<setting-name>
```

`<section-name>` Service Fabric yapılandırma bölümünün adını ve `<setting-name>` EventFlow ayarını yapılandırmak için kullanılacak değeri sağlayarak yapılandırma ayarı. Okunacak, bunun nasıl yapılacağı hakkında daha fazla Git [Service Fabric ayarları ve uygulama parametreleri için destek](https://github.com/Azure/diagnostics-eventflow#support-for-service-fabric-settings-and-application-parameters).

## <a name="verification"></a>Doğrulama

Hizmetinizi başlatın ve hata ayıklama uyun Visual Studio çıktı penceresinde. Hizmet başlatıldıktan sonra hizmetinizin kayıtları yapılandırdığınız çıktı gönderen kanıt görmeye başlamanız gerekir. Olay çözümleme ve görselleştirme platformunuza gidin ve günlükleri göster başladıysanız onaylayın Yukarı (birkaç dakika sürebilir).

## <a name="next-steps"></a>Sonraki adımlar

* [Olay analizi ve Application Insights ile Görselleştirme](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Olay çözümleme ve görselleştirme ile Azure izleme günlükleri](service-fabric-diagnostics-event-analysis-oms.md)
* [EventFlow belgeleri](https://github.com/Azure/diagnostics-eventflow)
