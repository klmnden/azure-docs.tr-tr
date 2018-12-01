---
title: Azure Application Insights, birden çok bileşen, mikro hizmetler ve kapsayıcılar için destek | Microsoft Docs
description: Birden çok bileşenleri veya rolleri için performans ve kullanım oluşur izleme uygulamaları.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: mbullwin
ms.openlocfilehash: dc776dcfe6fa2e03225649d856ae161c9f23c737
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727282"
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Çok bileşenli uygulamalar Application Insights (Önizleme) ile izleme

Birden çok sunucu bileşenlerini, rolleri veya Hizmetleri ile oluşur uygulamaları izleyebilirsiniz [Azure Application Insights](app-insights-overview.md). Bileşenler ve aralarındaki ilişkilerin durumunu tek bir uygulama harita üzerinde görüntülenir. Tek işlemler birden çok bileşen otomatik HTTP bağıntıyla aracılığıyla izleyebilirsiniz. Kapsayıcı tanılama tümleşik ve uygulama telemetrisini ile ilişkili. Uygulamanızın tüm bileşenleri için tek bir Application Insights kaynağı kullanın. 

![Çok bileşenli Uygulama Haritası](./media/app-insights-monitor-multi-role-apps/application-map-001.png)

'Bileşen' burada büyük bir uygulamanın çalışan herhangi bir bölümünü auto'yu kullanırız. Örneğin, tipik iş uygulaması bir konuşma web tarayıcılarında çalışan istemci kodunun oluşabilir veya hizmetleri sırayla geri kullanan daha fazla web uygulama hizmetleri bitemez. Sunucu bileşenleri şirket içi bulut üzerinde olabilir ya da Azure web ve çalışan rolleri olabilir veya kapsayıcıları Docker veya Service Fabric gibi çalışabilir. 

### <a name="sharing-a-single-application-insights-resource"></a>Tek bir Application Insights kaynak paylaşımı 

Anahtar burada her bileşen aynı Application Insights kaynağına, uygulamanızda telemetri gönderen, ancak kullanmak için bir tekniktir `cloud_RoleName` bileşenleri gerektiğinde ayırt etmek için özellik. Application Insights SDK'sını ekler `cloud_RoleName` telemetri bileşenleri özelliğine yayması. Örneğin, SDK'sı bir web sitesi adı ya da hizmet rolü adına ekleyeceksiniz `cloud_RoleName` özelliği. Bu değer bir telemetryınitializer ile geçersiz kılabilirsiniz. Uygulama Haritası kullanan `cloud_RoleName` eşlemedeki bileşenleri tanımlamak için özellik.

Hakkında daha fazla bilgi için geçersiz kılma `cloud_RoleName` özelliği bakın [özellikleri ekleyin: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

Bazı durumlarda, bu uygun olmayabilir ve farklı bileşenleri grupları için ayrı kaynaklar'ı kullanmayı tercih edebilirsiniz. Örneğin, yönetim veya faturalandırma amaçları için farklı kaynaklar kullanmanız gerekebilir.

Bu uyarı ile birden çok bileşenlerini bir Application Insights kaynağına veri göndermek istediğiniz bu belgenin geri kalanında varsayıyoruz.

## <a name="configure-multi-component-applications"></a>Çok bileşenli uygulamalar yapılandırma

Çok bileşenli Uygulama Haritası almak için bu hedefleri gerçekleştirmeye gerekir:

* **Yayın öncesi son yükleme** uygulama'nın her bileşeninin içinde Application Insights paketini. 
* **Tek bir Application Insights kaynağı** uygulamanızın tüm bileşenleri için.
* **Bileşik uygulama eşlemesi** önizlemeler dikey penceresinden.

Her bir bileşeninin türü için uygun yöntemi kullanarak uygulamanızı yapılandırın. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-the-latest-pre-release-package"></a>1. En son sürüm öncesi paketini yükleme

Güncelleştirme veya her sunucu bileşeni için projesinde Application Insights paketlerini yükleyin. Visual Studio kullanıyorsanız:

1. Bir projeyi sağ tıklayıp **NuGet paketlerini Yönet**. 
2. Seçin **ön sürümü dahil et**.
3. Application Insights paketlerini güncelleştirmeleri görünmüyorsa, bunları seçin. 

    Aksi takdirde, göz atmak ve uygun paketi yükleyin:
    
    * Microsoft.applicationınsights.windowsserver
    * Microsoft.ApplicationInsights.ServiceFabric - Konuk yürütülebilir dosyaları ve içinde bir Service Fabric uygulaması çalıştıran bir Docker kapsayıcıları çalışan bileşenler için
    * -ServiceFabric uygulamalarda reliable services için Microsoft.applicationınsights.servicefabric.Native
    * Docker, Kubernetes üzerinde çalışan bileşenler için Microsoft.ApplicationInsights.Kubernetes

### <a name="2-share-a-single-application-insights-resource"></a>2. Tek bir Application Insights kaynağı

* Visual Studio'da bir projeye sağ tıklayıp **Application ınsights'ı Yapılandır**, veya **Application Insights > yapılandırma**. İlk projeniz için Application Insights kaynağı oluşturmak için Sihirbazı'nı kullanın. Sonraki projeleri için aynı kaynak seçin.
* Application Insights menü yok ise, el ile yapılandırın:

   1. İçinde [Azure portalında](https://portal,azure.com), zaten oluşturduğunuz başka bir bileşen için Application Insights kaynağını açın.
   2. Genel Bakış dikey penceresinde, açık temel bileşenler açılan sekmesi ve kopyalama **izleme anahtarı.**
   3. Projenizde, applicationınsights.config dosyasını açın ve Ekle: `<InstrumentationKey>your copied key</InstrumentationKey>`

![İzleme anahtarını .config dosyasına kopyalama](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-composite-application-map"></a>3. Bileşik uygulama eşlemesi etkinleştir

Azure portalında, uygulamanız için kaynak açın. Alt yapılandırma başlığı altında önizlemeler önizlemeler dikey penceresini açmak için tıklayın. Önizlemeler dikey penceresinden etkinleştirme *bileşik uygulama eşlemesi*.

### <a name="4-enable-docker-metrics-optional"></a>4. (İsteğe bağlı) Docker ölçümlerini etkinleştir 

Bir bileşen bir Azure Windows sanal makinesinde barındırılan bir Docker çalışıyorsa, ek ölçümler kapsayıcıdan toplayabilirsiniz. Bu ekleme, [Azure tanılama](../monitoring-and-diagnostics/azure-diagnostics.md) yapılandırma dosyası:

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

## <a name="use-cloudrolename-to-separate-components"></a>Bileşenleri ayrı cloud_RoleName kullanın

`cloud_RoleName` Tüm telemetri ekli özellik. Bu, telemetri kaynağı bileşen - rol veya hizmete - tanımlar. (Bu paralel olarak birden çok sunucu işlemlerini veya makinelerde çalışan aynı rollerin ayıran cloud_RoleInstance ile aynı değildir.)

Portalda, filtreleyin veya bu özelliği kullanan telemetrinizi segmentlere ayırın. Bu örnekte, hataları dikey penceresinde, ön uç web hizmetinden kullanıma CRM API arka ucunu hatalardan filtreleme yalnızca bilgi gösterecek şekilde filtrelenir:

![Bulut rolü adına göre segmentlere ölçüm grafiği](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Bileşenler arasındaki işlemleri izleme

Bir bileşenden diğerine, tek bir işlem işlenirken yapılan çağrıları izleyebilir.


![İşlem için telemetriyi Göster](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Aracılığıyla bağıntılı telemetri bu işlem için listesini ön uç web sunucusu ve arka uç API'si tıklayın:

![Bileşenlerinde arama](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirme, Test ve üretim ayrı telemetri](app-insights-separate-resources.md)
