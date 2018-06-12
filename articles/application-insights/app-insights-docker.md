---
title: Azure Application Insights Docker uygulamalarında izleme | Microsoft Docs
description: Docker performans sayaçları, olaylar ve özel durumları Application Insights üzerinde kapsayıcılı uygulamalardan telemetri ile birlikte görüntülenebilir.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 53ade76b9dbdc27df90da1f7e197464816529d1d
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295770"
---
# <a name="monitor-docker-applications-in-application-insights"></a>Application ınsights'ta Docker uygulama izleme
Yaşam döngüsü olayları ve performans sayaçları [Docker](https://www.docker.com/) kapsayıcıları Application Insights grafiğinin. Yükleme [Application Insights](https://hub.docker.com/r/microsoft/applicationinsights/) ana bilgisayarınız ve kapsayıcısında görüntü diğer görüntüleri yanı sıra, ana bilgisayar için performans sayaçlarını görüntüler.

Docker ile uygulamalarınızı basit kapsayıcılarında tüm bağımlılıkları ile tam dağıtın. Bunlar, Docker altyapısına çalıştıran herhangi bir ana makinede çalıştıracaksınız.

Çalıştırdığınızda [Application Insights görüntü](https://hub.docker.com/r/microsoft/applicationinsights/) , Docker ana bilgisayarda bu avantajlarını elde edersiniz:

* Yaşam döngüsü telemetri çalıştıran tüm kapsayıcıları hakkında konakta - başlatmak, durdurmak ve benzeri.
* Tüm kapsayıcıları için performans sayaçları. CPU, bellek, ağ kullanımını ve daha fazlası.
* Varsa, [Java için Application Insights SDK'sı yüklü](app-insights-java-live.md) kapsayıcılarında çalışan uygulamalar, bu uygulamaların tüm telemetri kapsayıcı ve ana bilgisayar makine tanımlayan ek özellikleri sahip olur. Dolayısıyla örneğin bir uygulama içinde birden fazla ana çalışan örneklerini varsa, ana bilgisayar tarafından uygulama telemetrinizi kolayca filtreleyebilirsiniz.

![Örnek](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a>Application Insights kaynağı ayarladıysanız
1. Oturum [Microsoft Azure portal](https://azure.com) ve; uygulamanız için Application Insights kaynağını açma veya [yeni bir tane oluşturun](app-insights-create-new-resource.md). 
   
    *Hangi kaynak kullanmalıyım?* Konağınız üzerinde çalışan uygulamalar başka birisi tarafından geliştirilen sonra yapmanız [yeni bir Application Insights kaynağı oluşturma](app-insights-create-new-resource.md). Burada görüntüleyebilir ve telemetriyi Çözümle budur. (Uygulama türü için ' genel' seçin.)
   
    Ancak uygulama geliştiricisi olduğunuz sonra umuyoruz [Application Insights SDK'sı eklenen](app-insights-java-live.md) bunların her biri için. Gerçekten'ın tüm bileşenleri tek bir iş uygulaması olup olmadıklarını, sonra tüm bir kaynağa telemetri gönderecek şekilde yapılandırmanız ve Docker yaşam döngüsü ve performans verilerini görüntülemek için aynı kaynak kullanacaksınız. 
   
    Üçüncü senaryo uygulamaları çoğunu geliştirilmiş, ancak bunların telemetri görüntülemek için ayrı kaynakları kullanarak ' dir. Bu durumda, Docker verileri için ayrı bir kaynak oluşturmak için büyük olasılıkla de istersiniz. 
2. Docker kutucuğu ekleyin: seçin **eklemek döşeme**, Docker döşeme Galeriden sürükleyin ve ardından **Bitti**. 
   
    ![Örnek](./media/app-insights-docker/03.png)

> [!NOTE]
> Application Insights genel bakış bölmesinde artık kilitli ve döşeme Galeriden ekleme izin vermez. Yukarıda Azure Pano arabirimi açıklanan Docker kutucuk eklemeye devam edebilirsiniz.

3. Tıklatın **Essentials** açılır ve izleme anahtarını kopyalayın. Bu SDK, telemetri gönderileceği yeri bildirmek için kullanın.

    ![Örnek](./media/app-insights-docker/02-props.png)

En kısa sürede, telemetrisini görünmesini için geri gelin gibi bu tarayıcı penceresini elinizin altında tutun.

## <a name="run-the-application-insights-monitor-on-your-host"></a>Ana bilgisayarda Application Insights İzleyicisi'ni çalıştırın
Telemetri görüntülemek için bir yere olduğuna göre toplar ve göndermeden kapsayıcılı uygulama ayarlayabilirsiniz.

1. Docker ana bilgisayara bağlanın. 
2. Bu komutta, izleme anahtarını düzenleyin ve ardından çalıştırın:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Docker ana bilgisayar başına yalnızca bir Application Insights görüntü gereklidir. Uygulamanız birden çok Docker ana bilgisayarda dağıtılırsa, her ana bilgisayarda komutu yineleyin.

## <a name="update-your-app"></a>Uygulamanızı güncelleştirme
Uygulamanız ile işaretlenir varsa [Java için Application Insights SDK](app-insights-java-get-started.md), altında aşağıdaki satırı projenizdeki Applicationınsights.xml dosyasına ekleyin `<TelemetryInitializers>` öğe:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Bu kapsayıcı ve ana bilgisayar kimliği gibi Docker bilgileri uygulamanızdan gönderilen her telemetri öğesi ekler.

## <a name="view-your-telemetry"></a>Telemetrinizi görüntüleme
Azure portalında Application Insights kaynağınıza dönün.

Docker kutucuğa tıklayın.

Özellikle, Docker altyapısı üzerinde çalışan diğer kapsayıcılar varsa, kısa süre içinde Docker uygulamadan gelen veri görürsünüz.

Alabileceğiniz görünümleri bazıları aşağıda verilmiştir.

### <a name="perf-counters-by-host-activity-by-image"></a>Ana bilgisayar, görüntü göre etkinlik tarafından performans sayaçları
![Örnek](./media/app-insights-docker/10.png)

![Örnek](./media/app-insights-docker/11.png)

Herhangi bir ana bilgisayar veya görüntü adıyla daha fazla ayrıntı için'ı tıklatın.

Görünümü özelleştirmek için herhangi Grafik başlığı kılavuz'a tıklayın veya ekleme grafik kullanın. 

[Ölçüm Gezgini hakkında daha fazla bilgi](app-insights-metrics-explorer.md).

### <a name="docker-container-events"></a>Docker kapsayıcısı olayları
![Örnek](./media/app-insights-docker/13.png)

Olayları tek tek incelemek için tıklatın [arama](app-insights-diagnostic-search.md). Arama ve istediğiniz olayları bulmak için filtre. Daha fazla ayrıntı almak için herhangi bir etkinliğe tıklayın.

### <a name="exceptions-by-container-name"></a>Kapsayıcı adı tarafından özel durumlar
![Örnek](./media/app-insights-docker/14.png)

### <a name="docker-context-added-to-app-telemetry"></a>Uygulama telemetri eklenen docker bağlamı
AI Docker bağlamla zenginleştirilmiş SDK'sı, izlenmiş uygulamasından gönderilen telemetri isteği:

![Örnek](./media/app-insights-docker/16.png)

İşlemci zamanı ve kullanılabilir bellek performans sayaçları, zenginleştirilmiş ve Docker kapsayıcısı adına göre gruplandırılır:

![Örnek](./media/app-insights-docker/15.png)

## <a name="q--a"></a>Soru-Cevap
*Ne Application Insights Docker elde edilemiyor bana veriyor?*

* Performans sayaçları kapsayıcı ve görüntü tarafından ayrıntılı dökümü.
* Kapsayıcı ve uygulama verilerini tek bir Panoda tümleştirin.
* [Telemetri verme](app-insights-export-telemetry.md) daha fazla çözümleme için bir veritabanı, Power BI veya diğer Pano için.

*Telemetri uygulamasından nasıl sağlarım?*

* Application Insights SDK uygulamada yükleyin. Bilgi edinmek için nasıl: [Java web uygulamaları](app-insights-java-get-started.md), [Windows web uygulamaları](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

* [Java için Application Insights](app-insights-java-get-started.md)
* [Node.js için Application Insights](app-insights-nodejs.md)
* [ASP.NET için Application Insights](app-insights-asp-net.md)
