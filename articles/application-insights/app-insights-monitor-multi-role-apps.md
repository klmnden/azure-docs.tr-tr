---
title: Azure Application Insights, birden çok bileşenleri, mikro ve kapsayıcıları için desteği | Microsoft Docs
description: Birden çok bileşenleri veya performans ve kullanım için rolleri oluşur uygulamaları izleme.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: mbullwin
ms.openlocfilehash: 9b03aff140eec5b355383447f0a815220d6408e3
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Application Insights (Önizleme) ile birden çok bileşen uygulamaları izleme

Birden çok sunucu bileşenlerini, roller veya hizmetlerle oluşur uygulamaları izleyebilirsiniz [Azure Application Insights](app-insights-overview.md). Bileşenler ve aralarındaki ilişkilerin durumunu tek bir uygulama harita üzerinde görüntülenir. Tek tek işlemleri otomatik HTTP bağıntı ile birden çok bileşeni aracılığıyla izleyebilirsiniz. Kapsayıcı tanılama tümleşiktir ve uygulama telemetri ile ilişkili. Tek bir Application Insights kaynağı, uygulamanızın tüm bileşenler için kullanın. 

![Birden çok bileşen uygulama eşlemesi](./media/app-insights-monitor-multi-role-apps/app-map.png)

'Bileşen' burada büyük bir uygulamanın düzgün herhangi bir kısmını anlamına için kullanırız. Örneğin, tipik iş uygulaması bir konuşma web tarayıcılarında çalışan istemci kodunun oluşabilir veya hizmetleri sırayla geri kullanan daha fazla web uygulama hizmetleri bitemez. Sunucu bileşenleri içi barındırılan buluta üzerinde olabilir ya da Azure web ve çalışan rolleri olabilir veya kapsayıcılarında Docker veya Service Fabric gibi çalışabilir. 

### <a name="sharing-a-single-application-insights-resource"></a>Tek bir Application Insights kaynağı paylaşma 

Anahtar burada her bileşen aynı Application Insights kaynağı için uygulamanızda telemetri gönderen, ancak kullanmak için bir tekniktir `cloud_RoleName` bileşenleri gerektiğinde ayırt etmek için özellik. Application Insights SDK'sı ekler `cloud_RoleName` telemetri bileşenleri özelliğine yayma. Örneğin, SDK'yı bir web sitesi adı ya da hizmet rol adı için ekleyecek `cloud_RoleName` özelliği. Bu değer bir telemetryinitializer ile geçersiz kılabilirsiniz. Uygulama eşlemesi kullanan `cloud_RoleName` harita üzerinde bileşenleri tanımlamak için özellik.

Hakkında daha fazla bilgi için geçersiz kılma `cloud_RoleName` özelliği bakın [özellikleri ekleyin: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

Bazı durumlarda, bu uygun olmayabilir ve farklı bileşenleri grupları için ayrı kaynakları kullanmayı tercih edebilirsiniz. Örneğin, farklı kaynaklarının yönetim veya faturalandırma amaçları için kullanmanız gerekebilir. Ayrı kaynakları kullanarak tek bir uygulama haritada görüntülenmelerini tüm bileşenleri görmüyorum anlamına gelir; ve bileşenler arasında sorgulanamıyor [Analytics](app-insights-analytics.md). Ayrıca ayrı kaynakları ayarlamanız gerekir.

Bu uyarı ile bir Application Insights kaynağı için birden çok bileşenlerinden veri göndermek istediğiniz bu belgenin geri kalanında varsayıyoruz.

## <a name="configure-multi-component-applications"></a>Birden çok bileşen uygulamaları yapılandır

Birden çok bileşen uygulama eşlemesi almak için bu hedefleri başarmanın gerekir:

* **En son ön sürümü yüklemek** her bileşen uygulamanın Application Insights paketinde. 
* **Tek bir Application Insights kaynağı paylaşma** uygulamanızın tüm bileşenler için.
* **Bileşik uygulama eşlemesi etkinleştirmek** önizlemeleri dikey penceresinde.

Her bileşen türü için uygun yöntemi kullanarak, uygulamanızın yapılandırın. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-the-latest-pre-release-package"></a>1. En son sürüm öncesi paketini yükle

Güncelleştirme veya projesinde her sunucu bileşeni için Application Insights paketlerini yükleyin. Visual Studio kullanıyorsanız:

1. Bir projeye sağ tıklayın ve seçin **NuGet paketlerini Yönet**. 
2. Seçin **dahil et**.
3. Application Insights paketleri güncelleştirmelerinde görünmüyorsa, bunları seçin. 

    Aksi takdirde, göz atın ve uygun paketi yükleyin:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - for components running as guest executables and Docker containers running a in Service Fabric application
    * Microsoft.ApplicationInsights.ServiceFabric.Native - for reliable services in ServiceFabric applications
    * Docker içinde Kubernetes üzerinde çalışan bileşenleri için Microsoft.ApplicationInsights.Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Tek bir Application Insights kaynağı paylaşma

* Visual Studio'da bir projeye sağ tıklayın ve seçin **yapılandırma Application Insights**, veya **Application Insights > yapılandırma**. İlk projeniz için Application Insights kaynağı oluşturmak için sihirbazı kullanın. Sonraki projeleri için aynı kaynak seçin.
* Application Insights menü ise, el ile yapılandırın:

   1. İçinde [Azure portal](https://portal,azure.com), başka bir bileşen için önceden oluşturulmuş Application Insights kaynağı açın.
   2. Genel Bakış dikey penceresinde, açık Essentials açılan sekmesi ve kopyalama **izleme anahtarı.**
   3. Projenizdeki Applicationınsights.config açın ve Ekle: `<InstrumentationKey>your copied key</InstrumentationKey>`

![.Config dosyasına izleme anahtarını kopyalama](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-composite-application-map"></a>3. Bileşik uygulama eşlemesi etkinleştir

Azure Portal'da, uygulamanız için kaynak açın. Yapılandırma alt başlığı altında önizlemeleri önizlemeleri dikey penceresini açmak için tıklatın. Önizlemeler dikey penceresinde, etkinleştirme *bileşik uygulama eşlemesi*.

### <a name="4-enable-docker-metrics-optional"></a>4. Docker ölçümleri (isteğe bağlı) etkinleştir 

Bir bileşen bir Azure Windows VM üzerinde barındırılan bir Docker çalıştırıyorsa, kapsayıcıdan ek ölçümler toplayabilirsiniz. Bu konuda eklemek, [Azure tanılama](../monitoring-and-diagnostics/azure-diagnostics.md) yapılandırma dosyası:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-to-separate-components"></a>Bileşenleri ayırmak için cloud_RoleName kullanın

`cloud_RoleName` Özelliği tüm telemetri eklenmiş. Telemetri kaynaklanan bileşen - rol veya hizmete - tanımlar. (Bunu paralel olarak birden çok sunucu işlemlerini veya makinelerde çalışan aynı roller ayıran cloud_RoleInstance ile aynı değildir.)

Portalda, filtre ya da bu özelliği kullanan telemetrinizi segmentlere. Bu örnekte, hataları dikey CRM API arka uç hatalarından çıkışı filtreleme ön uç web hizmetinden sadece bilgi göstermek için filtrelenir:

![Bulut rolü adıyla kesimli ölçüm grafik](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Bileşenleri arasında izleme işlemleri

Başka bir tek bir işlem işlenirken yapılan çağrılar bir bileşenden izleyebilirsiniz.


![İşlem için telemetri Göster](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Aracılığıyla bu işlem için telemetri bağıntılı listesine ön uç web sunucusu ve arka uç API'si üzerinden tıklatın:

![Bileşenler genelinde arama](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirme, Test ve üretim ayrı telemetri](app-insights-separate-resources.md)
