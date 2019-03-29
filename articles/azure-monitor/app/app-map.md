---
title: Azure Application ınsights'da Uygulama Haritası | Microsoft Docs
description: Uygulama Haritası ile karmaşık bir uygulama topolojisini izleyin
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: a42eb7b57319df7de4c5277cdcdd93eb777f376c
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622119"
---
# <a name="application-map-triage-distributed-applications"></a>Uygulama Haritası: Dağıtılmış uygulamalar Önceliklendirme

Uygulama Haritası, nokta performans sorunlarını veya hata noktalarına dağıtılmış uygulamanızı tüm bileşenler genelinde yardımcı olur. Harita üzerinde her düğüm, bir uygulama bileşeni veya bağımlılıkları temsil eder; KPI durum ve durum uyarır. Aracılığıyla herhangi bir bileşene Application Insights olayları gibi daha ayrıntılı tanılama için tıklayabilirsiniz. Uygulamanızı Azure hizmetlerini kullanıyorsa, aracılığıyla SQL veritabanı Danışmanı önerilerini gibi Azure tanılama tıklatabilirsiniz.

## <a name="what-is-a-component"></a>Bir bileşen nedir?

Bağımsız bir şekilde dağıtılabilen dağıtılmış mikro uygulamanızın parçalarını bileşenlerdir. Geliştiriciler ve işlem ekipleri bu uygulama bileşenleri tarafından oluşturulan telemetri erişimi veya kod düzeyinde görünürlüğe sahip. 

* Bileşenleri "gözlemlenen" dış bağımlılıkları SQL gibi farklı takım/kuruluşunuz olmayabilir EventHub vb. erişim (kod veya telemetri).
* Bileşenleri rol/sunucu/kapsayıcı örnekleri herhangi bir sayıda çalışır.
* Bileşenleri ayrı bir Application Insights izleme anahtarı (abonelikler farklı olsa bile) olması ya da farklı roller için tek bir Application Insights izleme anahtarı raporlama gerçekleştirebilirsiniz. Önizleme eşlemesi deneyimi bileşenlerini nasıl ayarladıktan bakılmaksızın gösterir.

## <a name="composite-application-map"></a>Bileşik uygulama eşlemesi

Tam uygulama topolojisinin birden fazla seviyede ilgili uygulama bileşenleri arasında görebilirsiniz. Bileşenler, farklı Application Insgihts kaynakları veya tek bir kaynaktaki farklı roller olabilir. Uygulama Haritası Application Insights SDK'yi içeren sunucular arasında yapılan aşağıdaki HTTP bağımlılık çağrıları tarafından bileşenlerini bulur. 

Bu deneyim, bileşenlerin aşamalı bulma ile başlar. Uygulama Haritası'ı ilk yüklediğinizde, bir sorgu kümesi bu bileşenle ilgili bileşenler bulmak için tetiklenir. Bulundukları anda sol üst köşesinde bir düğme, uygulamanızda bileşen sayısı ile güncelleştirir. 

"Güncelleştirme eşleme bileşenlerini" a tıklandığında, harita noktasında kadar bulunan tüm bileşenleri ile yenilenir. Uygulamanızı karmaşıklığına bağlı olarak, bu yükleme için bir dakika sürebilir.

Tüm bileşenleri tek bir Application Insights kaynağı içindeki rol varsa, bu bulma adım gerekli değildir. Bu tür bir uygulama için ilk yükleme işlemi, tüm bileşenleri sahip olur.

![Uygulama Haritası ekran görüntüsü](media/app-map/app-map-001.png)

Bu deneyim ile anahtar hedeflerinden bileşenleri yüzlerce karmaşık topolojiler görselleştirmek sağlamaktır.

Öngörüleri görmek ve performans ve söz konusu bileşen için hata önceliklendirme deneyimi için herhangi bir bileşeni tıklayın.

![Açılır öğesi](media/app-map/application-map-002.png)

### <a name="investigate-failures"></a>Hataları araştır

Seçin **hataları Araştır** hataları bölmesinde başlatmak için.

![Ekran görüntüsü araştırma hataları düğmesi](media/app-map/investigate-failures.png)

![Hataları deneyiminin ekran görüntüsü](media/app-map/failures.png)

### <a name="investigate-performance"></a>Performansı araştır

Performans sorunlarını gidermek için seçin **performansını araştırın**.

![Ekran görüntüsü araştırmak performans düğmesi](media/app-map/investigate-performance.png)

![Performans deneyimi ekran görüntüsü](media/app-map/performance.png)

### <a name="go-to-details"></a>Ayrıntılara git

Seçin **ayrıntıları Git** çağrı yığın düzeyi için gerçekleştirilen görünümleri sunan uçtan uca işlem deneyimi keşfedin.

![Git ayrıntıları düğmesinin Ekran görüntüsü](media/app-map/go-to-details.png)

![Uçtan uca işlem ayrıntıları ekran görüntüsü](media/app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Analytics'de görüntüle

Sorgulamak ve uygulama verilerinizi daha fazla araştırmak için tıklayın **analytics'te görüntüle**.

![Analytics düğmesinde görünümünün ekran görüntüsü](media/app-map/view-in-analytics.png)

![Analytics deneyiminin ekran görüntüsü](media/app-map/analytics.png)

### <a name="alerts"></a>Uyarılar

Etkin uyarıları ve uyarı tetiklenmesi neden temel kurallarını görüntülemek için seçin **uyarılar**.

![Uyarılar düğmesinin Ekran görüntüsü](media/app-map/alerts.png)

![Analytics deneyiminin ekran görüntüsü](media/app-map/alerts-view.png)

## <a name="set-cloudrolename"></a>Kümesi cloud_RoleName

Uygulama Haritası kullanan `cloud_RoleName` eşlemedeki bileşenleri tanımlamak için özellik. Application Insights SDK'sını otomatik olarak ekler `cloud_RoleName` bileşenleri tarafından yayılan telemetri özelliği. Örneğin, SDK'sı bir web sitesi veya hizmet rol adı için ekleyeceksiniz `cloud_RoleName` özelliği. Bununla birlikte, varsayılan değeri geçersiz kılmak için burada isteyebileceğiniz durumlar vardır. Cloud_RoleName geçersiz kılmak ve uygulama eşlemesinde görüntülenen değiştirmek için:

### <a name="net"></a>.NET

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace CustomInitializer.Telemetry
{
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                //set custom role name here
                telemetry.Context.Cloud.RoleName = "RoleName";
            }
        }
    }
}
```

**Yük, başlatıcı**

Applicationınsights.config dosyasında:

```xml
    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="CustomInitializer.Telemetry.MyTelemetryInitializer, CustomInitializer"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>
```

Kodda, örneğin Global.aspx.cs başlatıcısında örneklemek için alternatif bir yöntem verilmiştir:

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
    }
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();
appInsights.defaultClient.context.tags["ai.cloud.role"] = "your role name";
appInsights.defaultClient.context.tags["ai.cloud.roleInstance"] = "your role instance";
```

### <a name="alternate-method-for-nodejs"></a>Node.js için alternatif yöntem

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY').start();

appInsights.defaultClient.addTelemetryProcessor(envelope => {
    envelope.tags["ai.cloud.role"] = "your role name";
    envelope.tags["ai.cloud.roleInstance"] = "your role instance"
});
```

### <a name="java"></a>Java

Spring Boot ile Application Insights Spring Boot Başlatıcı kullanırsanız, yalnızca gerekli değişiklik application.properties dosyasında uygulama için kendi özel adınızı ayarlamaktır.

`spring.application.name=<name-of-app>`

Spring Boot Başlatıcı cloudRoleName spring.application.name özelliği için girdiğiniz değer otomatik olarak atar.

Java bağıntı ve cloudRoleName olmayan SpringBoot uygulamalarını kullanıma almak için bu yapılandırma hakkında daha fazla bilgi için [bölümü](https://docs.microsoft.com/azure/application-insights/application-insights-correlation#role-name) bağıntı üzerinde.

### <a name="clientbrowser-side-javascript"></a>Tarayıcı/istemci-tarafı JavaScript

```javascript
appInsights.queue.push(() => {
appInsights.context.addTelemetryInitializer((envelope) => {
  envelope.tags["ai.cloud.role"] = "your role name";
  envelope.tags["ai.cloud.roleInstance"] = "your role instance";
});
});
```

Telemetri başlatıcıları cloud_RoleName özelliği geçersiz kılma hakkında daha fazla bilgi için bkz. [özellikleri ekleyin: ITelemetryInitializer](api-filtering-sampling.md#add-properties-itelemetryinitializer).

## <a name="troubleshooting"></a>Sorun giderme

Uygulama Haritası, beklendiği gibi çalışmaya başlama ile ilgili sorunlar yaşıyorsanız, aşağıdaki adımları deneyin:

1. Resmi olarak desteklenen bir SDK kullandığınızdan emin olun. Desteklenmeyen/Topluluk SDK’ları bağıntıyı desteklemeyebilir.

    Bu [makale](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) desteklenen Sdk'lardan listesi.

2. Tüm bileşenleri en son SDK sürümüne yükseltin.

3. Azure işlevleri ile kullanıyorsanız, C#yükseltmek için [işlevler V2](https://docs.microsoft.com/azure/azure-functions/functions-versions).

4. Onayla [cloud_RoleName](#set-cloud_rolename) doğru yapılandırılmış.

5. Bir bağımlılık eksikse, bunun [otomatik olarak toplanan bağımlılıklar](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies) listesinde bulunduğundan emin olun. Listede yoksa, bunu yine de [izleme bağımlılık çağrısı](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency) kullanarak el ile izleyebilirsiniz.

## <a name="portal-feedback"></a>Portalı geri bildirim

Geri bildirim sağlamak için geri bildirim seçeneğini kullanın.

![MapLink 1 görüntüsü](./media/app-map/14-updated.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Bağıntı anlama](https://docs.microsoft.com/azure/application-insights/application-insights-correlation)