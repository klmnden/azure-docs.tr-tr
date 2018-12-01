---
title: Azure web uygulaması performansını izleme | Microsoft Docs
description: Azure web uygulamaları için uygulama performansını izleme. Yük, yanıt süresi ve bağımlılık bilgilerinin grafiğini çıkarın ve performansa bağlı uyarılar ayarlayın.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: mbullwin
ms.openlocfilehash: f5179730223609def5ddb0e45317c2f986584139
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680489"
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

    ![Ayarlarda, Application ınsights'ı seçin.](./media/app-insights-azure-web-apps/settings-app-insights.png)

   * Yeni bir kaynak oluşturmak bu uygulama için bir Application Insights kaynağı zaten ayarlamadıysanız'ı seçin. 

    > [!NOTE]
    > Tıkladığınızda **Tamam** için istenir yeni kaynak oluşturmak için **izleme ayarlarını uygula**. Seçme **devam** yeni Application Insights kaynağınıza de bunu yapılması, web uygulamasına bağlayacaksınız **web uygulamanızı yeniden tetikleyin**. 

    ![Web uygulamanızı izleme](./media/app-insights-azure-web-apps/create-resource.png)

2. Hangi kaynağı kullanacağını belirlemesinde belirttikten sonra her platformun uygulamanız için veri toplamak için application ınsights'ı nasıl istediğinizi seçebilirsiniz.

    ![Platform başına seçenekleri belirleyin](./media/app-insights-azure-web-apps/choose-options.png)

3. Application Insights yüklendikten sonra **web uygulamanızı izleyin** .

   Sayfa görüntülemesi ve kullanıcı telemetrisi için **istemci tarafı izlemeyi etkinleştirin**.

   * Ayarlar > Uygulama Ayarları'nı seçin.
   * Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin:

    Anahtar: `APPINSIGHTS_JAVASCRIPT_ENABLED`

    Değer:`true`
   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.
4. Seçerek uygulama izleme verilerini keşfedin **ayarları** > **Application Insights** > **içinde daha fazla Application Insights görüntülemek**.

İsterseniz, daha sonra uygulamayı Application Insights ile derleyebilirsiniz.

*Application Insights’ı nasıl kaldırabilirim veya başka bir kaynağa nasıl geçiş yapabilirim?*

* Azure, web uygulaması denetimi dikey penceresini açın ve ayarları altında **Application Insights**. Application ınsights'ı tıklatarak açabilirsiniz **devre dışı** , üst veya yeni bir kaynak seçin **kaynağınızı değişiklik** bölümü.

## <a name="build-the-app-with-application-insights"></a>Uygulamayı Application Insights ile derleme
Application Insights, uygulamanıza bir SDK yükleyerek daha ayrıntılı telemetri sağlayabilir. Bunların başında izleme günlüklerini toplama, [özel telemetri yazma](app-insights-api-custom-events-metrics.md) ve daha ayrıntılı özel durum raporları alma gelir.

1. **Visual Studio**’da (2013 güncelleştirme 2 veya sonraki sürümler), projeniz için Application Insights’ı yapılandırın.

    Web projesine sağ tıklayın ve seçin **Ekle > Application Insights** veya **proje** > **Application Insights**  >  **Konfigurovat Application Insights**.

    ![Web projesine sağ tıklayıp Application Insights Ekle veya Yapılandır’ı seçin](./media/app-insights-azure-web-apps/03-add.png)

    Oturum açmanız istenirse Azure hesabınızda kullandığınız kimlik bilgilerini kullanın.

    Bu işlemin iki etkisi olur:

   1. Azure’da telemetrinin depolanacağı, analiz edileceği ve görüntüleneceği bir Application Insights kaynağı oluşturulur.
   2. Application Insights NuGet paketi kodunuza eklenir (henüz eklenmediyse) ve kodunuz Azure kaynağına telemetri gönderilecek şekilde yapılandırılır.
2. Uygulamayı geliştirme makinenizde çalıştırarak (F5) **telemetriyi test edin**.
3. Normal yöntemle **uygulamayı Azure’da yayımlayın**. 

*Farklı bir Application Insights kaynağına nasıl geçiş yapabilirim?*

* Visual Studio’da projeye sağ tıklayıp **Application Insights’ı Yapılandır**’ı seçin ve istediğiniz kaynağı seçin. Yeni kaynak oluşturma seçeneğini görürsünüz. Yeniden derleyin ve dağıtın.

## <a name="more-telemetry"></a>Daha fazla telemetri

* [Web sayfası yükleme verileri](app-insights-javascript.md)
* [Özel telemetri](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](app-insights-profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../azure-monitor/platform/data-collection.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../monitoring-and-diagnostics/monitoring-overview-alerts.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](app-insights-javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) ayarlayın.

