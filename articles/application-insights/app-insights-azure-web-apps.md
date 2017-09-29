---
title: "Azure web uygulaması performansını izleme | Microsoft Docs"
description: "Azure web uygulamaları için uygulama performansını izleme. Yük, yanıt süresi ve bağımlılık bilgilerinin grafiğini çıkarın ve performansa bağlı uyarılar ayarlayın."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 110a4d26e90f46e823a3e1c9ebece3360fbdf0c9
ms.contentlocale: tr-tr
ms.lasthandoff: 09/20/2017

---
# <a name="monitor-azure-web-app-performance"></a>Azure web uygulaması performansını izleme
[Azure Portal](https://portal.azure.com)’da [Azure web uygulamalarınız](../app-service/app-service-web-overview.md) için uygulama performansını izleme özelliğini ayarlayabilirsiniz. [Azure Application Insights](app-insights-overview.md), uygulamanızı izleyerek uygulama etkinlikleriyle ilgili telemetriyi Application Insights hizmetine gönderir ve telemetri burada depolanıp analiz edilir. Burada, sorunların tanılanmasına, performansın geliştirilmesine ve kullanımın değerlendirilmesine yardımcı olan ölçüm grafikleri ve arama araçları kullanılabilir.

## <a name="run-time-or-build-time"></a>Çalışma zamanı veya derleme zamanı
Uygulamanız için şu iki yoldan biriyle izleme yapılandırabilirsiniz:

* **Çalışma zamanı**: Web uygulamanız zaten yayındayken bir performans izleme uzantısını seçebilirsiniz. Uygulamanızı yeniden derlemeniz ya da yüklemeniz gerekmez. Yanıt süreleri, başarı oranları, özel durumlar ve bağımlılıklar gibi değişkenleri izleyen standart bir paket kümesine sahip olursunuz. 
* **Derleme zamanı**: Geliştirme sırasında uygulamanıza bir paket yükleyebilirsiniz. Bu seçenek daha kullanışlıdır. Aynı standart paketlere ek olarak telemetriyi özelleştirmek için kod yazabilir ya da kendi telemetrinizi gönderebilirsiniz. Uygulama etki alanınızın semantiğine göre belirli etkinlikleri günlüğe kaydedebilir veya olayların kaydını tutabilirsiniz. 

## <a name="run-time-instrumentation-with-application-insights"></a>Application Insights ile çalışma zamanında izleme
Azure’da çalışmakta olan bir web uygulamanız varsa zaten bazı izleme verileri alırsınız: İstek ve hata oranları. Yanıt süreleri, bağımlılıklara yapılan çağrılar, akıllı algılama ve güçlü Log Analytics sorgu dili gibi ek özelliklere sahip olmak için Application Insights’ı ekleyin. 

1. Web uygulamanızın Azure denetim masasında **Application Insights’ı seçin**.
   
    ![İzleme bölümünden Application Insights’ı seçin](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Bu uygulama için başka yollardan zaten bir Application Insights kaynağı oluşturmadıysanız, yeni kaynak oluşturma seçeneğini belirleyin.
2. Application Insights yüklendikten sonra **web uygulamanızı izleyin** . 
   
    ![Web uygulamanızı izleme](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   Sayfa görüntülemesi ve kullanıcı telemetrisi için **istemci tarafı izlemeyi etkinleştirin**.

   * Ayarlar > Uygulama Ayarları'nı seçin.
   * Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin: 
   
    Anahtar: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Değer:`true`
   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.
3. **Uygulamanızı izleyin**.  [Verileri keşfedin](#explore-the-data).

İsterseniz, daha sonra uygulamayı Application Insights ile derleyebilirsiniz.

*Application Insights’ı nasıl kaldırabilirim veya başka bir kaynağa nasıl geçiş yapabilirim?*

* Azure’da web uygulaması denetimi dikey penceresini açın ve Geliştirme Araçları bölümünden **Uzantılar**’ı açın. Application Insights uzantısını silin. Sonra, İzleme bölümünden Application Insights’ı seçin ve istediğiniz kaynağı oluşturun veya seçin.

## <a name="build-the-app-with-application-insights"></a>Uygulamayı Application Insights ile derleme
Application Insights, uygulamanıza bir SDK yükleyerek daha ayrıntılı telemetri sağlayabilir. Bunların başında izleme günlüklerini toplama, [özel telemetri yazma](app-insights-api-custom-events-metrics.md) ve daha ayrıntılı özel durum raporları alma gelir.

1. **Visual Studio**’da (2013 güncelleştirme 2 veya sonraki sürümler), projeniz için Application Insights’ı yapılandırın.

    Web projesine sağ tıklayıp **Ekle > Application Insights**’ı veya **Application Insights’ı Yapılandır**’ı seçin.
   
    ![Web projesine sağ tıklayıp Application Insights Ekle veya Yapılandır’ı seçin](./media/app-insights-azure-web-apps/03-add.png)
   
    Oturum açmanız istenirse Azure hesabınızda kullandığınız kimlik bilgilerini kullanın.
   
    Bu işlemin iki etkisi olur:
   
   1. Azure’da telemetrinin depolanacağı, analiz edileceği ve görüntüleneceği bir Application Insights kaynağı oluşturulur.
   2. Application Insights NuGet paketi kodunuza eklenir (henüz eklenmediyse) ve kodunuz Azure kaynağına telemetri gönderilecek şekilde yapılandırılır.
2. Uygulamayı geliştirme makinenizde çalıştırarak (F5) **telemetriyi test edin**.
3. Normal yöntemle **uygulamayı Azure’da yayımlayın**. 

*Farklı bir Application Insights kaynağına nasıl geçiş yapabilirim?*

* Visual Studio’da projeye sağ tıklayıp **Application Insights’ı Yapılandır**’ı seçin ve istediğiniz kaynağı seçin. Yeni kaynak oluşturma seçeneğini görürsünüz. Yeniden derleyin ve dağıtın.

## <a name="explore-the-data"></a>Verileri keşfetme
1. Web uygulamanızın denetim masasındaki Application Insights dikey penceresinde, istek ve hataları gerçekleştikten bir veya iki saniye sonra gösteren Canlı Akış’ı görürsünüz. Uygulamanızı yeniden yayımlıyorsanız herhangi bir sorunu anında görmenizi sağlayan bu ekran çok kullanışlıdır.
2. Seçeneklere tıklayarak tam Application Insights kaynağına gidin.

    ![Tıklayarak gitme](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    Kaynağa doğrudan Azure kaynak gezintisinden de gidebilirsiniz.

1. Daha fazla ayrıntı görmek için herhangi bir grafikte tıklayarak gezinin:
   
    ![Application Insights’a genel bakış dikey penceresinde bir grafiğe tıklayın](./media/app-insights-azure-web-apps/07-dependency.png)
   
    [Ölçüm dikey pencerelerini özelleştirme](app-insights-metrics-explorer.md) olanağınız vardır.
2. Tek tek olayları ve bunların özelliklerini görmek için tıklamaya devam edin:
   
    ![Bir olay türüne göre filtrelenen bir arama açmak için olay türüne tıklayın](./media/app-insights-azure-web-apps/08-requests.png)
   
    Tüm özellikleri açmaya yönelik "..." seçeneğini görebilirsiniz.
   
    [Aramaları özelleştirme](app-insights-diagnostic-search.md) olanağınız vardır.

Telemetriniz üzerinde daha güçlü aramalar gerçekleştirmek için [Log Analytics sorgu dilini](app-insights-analytics-tour.md) kullanın.

## <a name="more-telemetry"></a>Daha fazla telemetri

* [Web sayfası yükleme verileri](app-insights-javascript.md)
* [Özel telemetri](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](app-insights-profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](app-insights-azure-diagnostics.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](app-insights-javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) ayarlayın.


