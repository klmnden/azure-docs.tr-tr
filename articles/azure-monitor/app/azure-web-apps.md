---
title: Azure uygulama hizmetleri performansını izleme | Microsoft Docs
description: Azure uygulama hizmetleri için uygulama performansı izleme. Yük, yanıt süresi ve bağımlılık bilgilerinin grafiğini çıkarın ve performansa bağlı uyarılar ayarlayın.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: mbullwin
ms.openlocfilehash: 19e0e5797e05589baa1e104f3e9ab8b4d9cc2d6c
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267300"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service performansını izleme
İçinde [Azure portalında](https://portal.azure.com) web uygulamaları, mobil arka uçlar ve API apps için uygulama performans izleme özelliğini ayarlayabilirsiniz [Azure App Service](../../app-service/overview.md). [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), uygulamanızı izleyerek uygulama etkinlikleriyle ilgili telemetriyi Application Insights hizmetine gönderir ve telemetri burada depolanıp analiz edilir. Burada, sorunların tanılanmasına, performansın geliştirilmesine ve kullanımın değerlendirilmesine yardımcı olan ölçüm grafikleri ve arama araçları kullanılabilir.

## <a name="run-time-or-build-time"></a>Çalışma zamanı veya derleme zamanı
Uygulamanız için şu iki yoldan biriyle izleme yapılandırabilirsiniz:

* **Çalışma zamanı** -izleme uzantısı app service'inizin zaten yayındayken bir performans seçebilirsiniz. Yeniden oluşturmanız veya uygulamanızı yeniden yüklemeniz gerekmez. Yanıt süreleri, başarı oranları, özel durumlar ve bağımlılıklar gibi değişkenleri izleyen standart bir paket kümesine sahip olursunuz. 
* **Derleme zamanı**: Geliştirme sırasında uygulamanıza bir paket yükleyebilirsiniz. Bu seçenek daha kullanışlıdır. Aynı standart paketlere ek olarak telemetriyi özelleştirmek için kod yazabilir ya da kendi telemetrinizi gönderebilirsiniz. Uygulama etki alanınızın semantiğine göre belirli etkinlikleri günlüğe kaydedebilir veya olayların kaydını tutabilirsiniz. 

## <a name="run-time-instrumentation-with-application-insights"></a>Application Insights ile çalışma zamanında izleme
Azure'da zaten bir app service çalıştırıyorsanız, zaten bazı izleme alırsınız: istek ve hata oranları. Yanıt süreleri gibi almak için Application Insights ekleyin, bağımlılıklar, akıllı algılama ve güçlü yapılan çağrılar Kusto sorgu dili. 

1. **Application Insights'ı seçin** uygulama hizmetiniz için Azure Denetim Masası'nda.

    ![Ayarlarda, Application ınsights'ı seçin.](./media/azure-web-apps/settings-app-insights.png)

   * Yeni bir kaynak oluşturmak bu uygulama için bir Application Insights kaynağı zaten ayarlamadıysanız'ı seçin. 

    > [!NOTE]
    > Tıkladığınızda **Tamam** için istenir yeni kaynak oluşturmak için **izleme ayarlarını uygula**. Seçme **devam** app Service'e kadar olacak de yapmak, yeni Application Insights kaynağınıza bağlayacaksınız **app service'inizi yeniden tetikleyin**. 

    ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource.png)

2. Hangi kaynağı kullanacağını belirlemesinde belirttikten sonra her platformun uygulamanız için veri toplamak için application ınsights'ı nasıl istediğinizi seçebilirsiniz. (ASP.NET uygulamasını izleme üzerinde varsayılan olarak koleksiyon iki farklı düzeylerine sahip.)

    ![Platform başına seçenekleri belirleyin](./media/azure-web-apps/choose-options-new.png)

    * .NET **temel koleksiyonu** düzeyini temel Tek Örnekli APM özellikleri sunar.
    
    * .NET **koleksiyon önerilen** düzeyi:
        * CPU, bellek ve g/ç kullanım eğilimlerini ekler.
        * Mikro hizmetler, istek/bağımlılık sınırlarında ilişkilendirir.
        * Kullanım eğilimlerini toplar ve işlemler için kullanılabilirlik sonuçlarından bağıntı sağlar.
        * Ana bilgisayar işlemi tarafından işlenmemiş özel durumlar toplar.
        * Örnekleme kullanıldığında yük altında APM ölçümleri doğruluğu artırır.
    
    .NET core sunar **koleksiyon önerilen** veya .NET Core 2.0 ve 2.1 için devre dışı bırakıldı.

3. **App service'inizin izleme** Application Insights yüklendikten sonra.

   **İstemci-tarafı izlemeyi etkinleştir** sayfa görüntülemesi ve kullanıcı telemetrisi için.

    (Bu ile .NET Core uygulamaları için varsayılan olarak etkin **koleksiyon önerilen**uygulama ayarı 'APPINSIGHTS_JAVASCRIPT_ENABLED' bulunup bulunmadığını bakılmaksızın. İstemci tarafı izleme devre dışı bırakmak için destek ayrıntılı kullanıcı Arabirimi tabanlı .NET Core için şu anda kullanılabilir değil.)
    
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
Application Insights, uygulamanıza bir SDK yükleyerek daha ayrıntılı telemetri sağlayabilir. Bunların başında izleme günlüklerini toplama, [özel telemetri yazma](../../azure-monitor/app/api-custom-events-metrics.md) ve daha ayrıntılı özel durum raporları alma gelir.

1. **Visual Studio**’da (2013 güncelleştirme 2 veya sonraki sürümler), projeniz için Application Insights’ı yapılandırın.

    Web projesine sağ tıklayın ve seçin **Ekle > Application Insights** veya **proje** > **Application Insights**  >  **Konfigurovat Application Insights**.

    ![Web projesine sağ tıklayıp Application Insights Ekle veya Yapılandır’ı seçin](./media/azure-web-apps/03-add.png)

    Oturum açmanız istenirse Azure hesabınızda kullandığınız kimlik bilgilerini kullanın.

    Bu işlemin iki etkisi olur:

   1. Telemetri burada depolanır, analiz ve görüntülenen Azure'da Application Insights kaynağı oluşturur.
   2. Application Insights NuGet paketi kodunuza eklenir (henüz eklenmediyse) ve kodunuz Azure kaynağına telemetri gönderilecek şekilde yapılandırılır.
2. Uygulamayı geliştirme makinenizde çalıştırarak (F5) **telemetriyi test edin**.
3. Normal yöntemle **uygulamayı Azure’da yayımlayın**. 

*Farklı bir Application Insights kaynağına nasıl geçiş yapabilirim?*

* Visual Studio'da projeye sağ tıklayın, seçin **Application ınsights'ı Yapılandır**ve istediğiniz kaynağı seçin. Yeni kaynak oluşturma seçeneğini görürsünüz. Yeniden derleyin ve dağıtın.

## <a name="more-telemetry"></a>Daha fazla telemetri

* [Web sayfası yükleme verileri](../../azure-monitor/app/javascript.md)
* [Özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED tamamlanmamış HTML yanıtını NET CORE web uygulamaları neden olur.

JavaScript uygulama hizmetleri aracılığıyla etkinleştirme html yanıtları kesiliyor neden olabilir.

* Geçici Çözüm 1: APPINSIGHTS_JAVASCRIPT_ENABLED uygulama ayarını false olarak ayarlayın ya da tamamen kaldırın ve yeniden başlatın
* Geçici Çözüm 2: kod aracılığıyla SDK'sını ekleyin ve uzantısını kaldırın (Bu yapılandırma ile olmaz Profiler ve anlık görüntü hata ayıklayıcısı)

Bu sorunu takip ettiğimiz [burada](https://github.com/Microsoft/ApplicationInsights-Home/issues/277)

.NET Core için aşağıdakiler şu anda, **desteklenmiyor**:

* Kendi başına dağıtım.
* .NET Framework'ü hedefleyen uygulamalar.
* .NET core 2.2 uygulamalar.

> [!NOTE]
> .NET core 2.0 ve .NET Core 2.1 desteklenir. .NET Core 2.2 Destek eklendiğinde, bu makale güncelleştirilecektir.

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](../../azure-monitor/app/profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../../azure-monitor/platform/data-collection.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../../azure-monitor/platform/alerts-overview.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](../../azure-monitor/app/javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md) ayarlayın.

