---
title: Windows sunucu ve çalışan rolleri için Azure Application Insights | Microsoft Docs
description: Kullanım, kullanılabilirlik ve performansı analiz etmek için Application Insights SDK’sını ASP.NET uygulamanıza el ile ekleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 85764c0ee5b8ed117fb191657d54abe5bd10a703
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60784495"
---
# <a name="manually-configure-application-insights-for-net-applications"></a>.NET uygulamaları için Application Insights’ı el ile yapılandırma

[Application Insights](../../azure-monitor/app/app-insights-overview.md)’ı, çok çeşitli uygulamaları veya uygulama rolleri, bileşenler veya mikro hizmetleri izlemek üzere yapılandırabilirsiniz. Web uygulamaları ve hizmetleri için, Visual Studio [tek adımlı yapılandırma](../../azure-monitor/app/asp-net.md) sunar. Arka uç sunucu rolleri veya masaüstü uygulamaları gibi diğer .NET uygulaması türleri için Application Insights’ı el ile yapılandırabilirsiniz.

![Örnek performans izleme grafikleri](./media/windows-services/10-perf.png)

#### <a name="before-you-start"></a>Başlamadan önce

Gerekenler:

* Bir [Microsoft Azure](https://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](https://live.com) kullanarak sizi buna ekleyebilir.
* Visual Studio 2013 veya üstü.

## <a name="add"></a>1. Application Insights kaynağı seçme

'Kaynak', verilerinizin toplandığı ve Azure portalında gösterildiği yerdir. Yeni bir tane oluşturmaya veya mevcut olanı paylaşmaya karar vermeniz gerekir.

### <a name="part-of-a-larger-app-use-existing-resource"></a>Daha büyük bir uygulamanın parçası: Mevcut kaynağı kullan

Web uygulamanızın ön uç web uygulaması ve bir ya da daha fazla arka uç hizmeti gibi birkaç bileşeni varsa, tüm bileşenlerden aynı kaynağa telemetri verileri göndermeniz gerekir. Bunun yapılması, verilerin tek bir Uygulama Eşlemesinde gösterilmesini sağlar ve bir bileşenden diğerine gönderilen isteğin izlenmesini mümkün hale getirir.

Bu nedenle, bu uygulamanın diğer bileşenlerini izliyorsanız aynı kaynağı kullanın.

Kaynağı [Azure portalında](https://portal.azure.com/) açın. 

### <a name="self-contained-app-create-a-new-resource"></a>Kendi içinde uygulama: Yeni kaynak oluşturma

Yeni uygulama başka uygulamalarla ilgisiz ise, kendi kaynağına sahip olmalıdır.

[Azure portalında](https://portal.azure.com/) oturum açın ve yeni bir Application Insights kaynağı oluşturun. Uygulama türü olarak ASP.NET’i seçin.

![Yeni, Application Insights öğesine tıklayın](./media/windows-services/01-new-asp.png)

Uygulama türü seçimi, kaynak dikey pencerelerinin varsayılan içeriğini belirler.

## <a name="2-copy-the-instrumentation-key"></a>2. İzleme Anahtarını Kopyalama
Anahtar, kaynağı tanımlar. Bu anahtarı kısa bir süre sonra verileri kaynağa yönlendirmek için SDK’ya yükleyeceksiniz.

![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/windows-services/02-props-asp.png)

## <a name="sdk"></a>3. Uygulamanıza Application Insights paketi yükleme
Application Insights paketinin yüklenmesi ve yapılandırılması, üzerinde çalıştığınız platforma bağlı olarak değişir. 

1. Visual Studio’da projenize sağ tıklayıp **NuGet Paketlerini Yönet**’i seçin.
   
    ![Projeye sağ tıklayın ve Nuget Paketlerini Yönet’i seçin](./media/windows-services/03-nuget.png)
2. Windows sunucu uygulamaları için "Microsoft.ApplicationInsights.WindowsServer" adlı Application Insights paketini yükleyin.
   
    !["Application Insights" araması yapın](./media/windows-services/04-ai-nuget.png)
   
    *Hangi sürüm?*

    En son özelliklerimizi denemek istiyorsanız **Ön sürümleri dahil et**’i işaretleyin. Ön sürümün gerekip gerekmediği ilgili belge veya bloglarda belirtilir.
    
    *Diğer paketleri kullanabilir miyim?*
   
    Evet. API’yi yalnızca kendi telemetrinizi göndermek için kullanmak istiyorsanız “Microsoft.ApplicationInsights” öğesini seçin. Windows Server paketi API’nin yanı sıra performans sayacı koleksiyonu ve bağımlılık izlemesi gibi birkaç paketi daha içerir. 

### <a name="to-upgrade-to-future-package-versions"></a>Gelecekteki paket sürümlerine yükseltmek için
SDK’nın yeni sürümü zaman zaman yayınlanmaktadır.

[Paketin yeni sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.WindowsServer** ve **Yükselt**’i seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.

## <a name="4-send-telemetry"></a>4. Telemetri gönderme
**Yalnızca API paketini yüklediyseniz:**

* Koddaki izleme anahtarını ayarlayın, örneğin `main()` içinde: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *anahtarınız* `";` 
* [API'yi kullanarak kendi telemetrinizi yazın](../../azure-monitor/app/api-custom-events-metrics.md#ikey).

**Diğer Application Insights paketlerini yüklediyseniz** izleme anahtarını ayarlamak için isterseniz .config dosyasını kullanabilirsiniz:

* ApplicationInsights.config dosyasını (NuGet yüklemesinde eklenen dosya) düzenleyin. Kapanış etiketinin hemen önüne şunu ekleyin:
  
    `<InstrumentationKey>` *kopyaladığınız izleme anahtarı* `</InstrumentationKey>`
* Çözüm Gezgini’nde ApplicationInsights.config dosyası özelliklerinin **Build Action = Content, Copy to Output Directory = Copy** olarak ayarlandığından emin olun.

[Farklı derleme yapılandırmalarında anahtarı değiştirmek istiyorsanız](../../azure-monitor/app/separate-resources.md) kodda izleme anahtarı belirlemeniz faydalı olacaktır. Kodda anahtarı ayarladıysanız `.config` dosyasında ayarlamanız gerekmez.

## <a name="run"></a> Projenizi çalıştırma
**F5** ile uygulamanızı çalıştırın ve şunu deneyin: birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da gönderilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’da olay sayımı](./media/windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Telemetrinizi görüntüleme
[Azure portal](https://portal.azure.com/)’a geri dönün ve Application Insights kaynağınıza göz atın.

Genel Bakış grafiklerinde veri arayın. İlk olarak yalnızca bir veya iki nokta görürsünüz. Örneğin:

![Daha fazla veri için tıklayın](./media/windows-services/12-first-perf.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın. [Ölçümler hakkında daha fazla bilgi edinin.](../../azure-monitor/app/web-monitor-performance.md)

### <a name="no-data"></a>Veri yok mu?
* Birkaç telemetri oluşturması için farklı sayfaları açarak uygulamayı kullanın.
* Olayları tek tek görmek için [Ara](../../azure-monitor/app/diagnostic-search.md) kutucuğunu açın. Bazı durumlarda olayların ölçüm ardışık düzenine ulaşması biraz daha uzun sürer.
* Birkaç saniye bekleyin ve **Yenile**’ye tıklayın. Grafikler kendilerini düzenli olarak yeniler, ancak bazı verilerin görüntülenmesini bekliyorsanız el ile yenileyebilirsiniz.
* Bkz. [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md).

## <a name="publish-your-app"></a>Uygulamanızı yayımlama
Şimdi uygulamanızı sunucunuza veya Azure’a dağıtın ve verilerin birikmesini izleyin.

![Visual Studio kullanarak uygulamanızı yayımlama](./media/windows-services/15-publish.png)

Hata ayıklama modunda çalıştırdığınızda telemetri ardışık düzen üzerinden gönderilir, böylece görüntülenen verileri saniyeler içinde görebilirsiniz. Uygulamanızı Sürüm yapılandırmasında dağıttığınızda veriler daha yavaş birikir.

### <a name="no-data-after-you-publish-to-your-server"></a>Sunucunuza yayımladıktan sonra veri yok mu?
Sunucunuzun güvenlik duvarında giden trafik için bağlantı noktalarını açın. Gerekli adreslerin listesi için [bu sayfaya](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) bakın 

### <a name="trouble-on-your-build-server"></a>Derleme sunucunuzda sorun mu yaşıyorsunuz?
Lütfen [bu Sorun Giderme maddesine](../../azure-monitor/app/asp-net-troubleshoot-no-data.md#NuGetBuild) bakın.

> [!NOTE]
> Uygulamanız çok sayıda telemetri oluşturuyorsa, uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak, aynı istekle ilişkili olaylar grup olarak seçilir ya da seçimi kaldırılır, böylece ilgili olaylar arasında gezinebilirsiniz. 
> [Örnekleme hakkında bilgi edinin](../../azure-monitor/app/sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* Uygulamanızın 360 derecelik tam görünümünü elde etmek üzere [daha fazla telemetri ekleyin](../../azure-monitor/app/asp-net-more.md).

