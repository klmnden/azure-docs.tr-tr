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
ms.date: 02/26/2019
ms.author: mbullwin
ms.openlocfilehash: 92a7c1a45655f8804aa1f81b1a77ebf7cd5197e8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58122174"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service performansını izleme
İçinde [Azure portalında](https://portal.azure.com) web uygulamaları, mobil arka uçlar ve API apps için uygulama performans izleme özelliğini ayarlayabilirsiniz [Azure App Service](../../app-service/overview.md). [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), uygulamanızı izleyerek uygulama etkinlikleriyle ilgili telemetriyi Application Insights hizmetine gönderir ve telemetri burada depolanıp analiz edilir. Burada, sorunların tanılanmasına, performansın geliştirilmesine ve kullanımın değerlendirilmesine yardımcı olan ölçüm grafikleri ve arama araçları kullanılabilir.

## <a name="runtime-or-build-time"></a>Çalışma zamanı veya derleme zamanı
Uygulamanız için şu iki yoldan biriyle izleme yapılandırabilirsiniz:

* **Çalışma zamanı** -izleme uzantısı app service'inizin zaten yayındayken bir performans seçebilirsiniz. Yeniden oluşturmanız veya uygulamanızı yeniden yüklemeniz gerekmez. Yanıt süreleri, başarı oranları, özel durumlar ve bağımlılıklar gibi değişkenleri izleyen standart bir paket kümesine sahip olursunuz.

* **Derleme zamanı**: Geliştirme sırasında uygulamanıza bir paket yükleyebilirsiniz. Bu seçenek daha kullanışlıdır. Aynı standart paketlere ek olarak telemetriyi özelleştirmek için kod yazabilir ya da kendi telemetrinizi gönderebilirsiniz. Uygulama etki alanınızın semantiğine göre belirli etkinlikleri günlüğe kaydedebilir veya olayların kaydını tutabilirsiniz. Bu ayrıca en son kararlı sürüme kısıtlı çalışma zamanını izleme ise beta SDK'ları değerlendirmek seçebileceğiniz gibi Application Insights SDK'SININ en son sürümünü test etme olanağı sağlar.

## <a name="runtime-instrumentation-with-application-insights"></a>Application Insights ile çalışma zamanı izleme
Bir app service şu anda Azure'da çalıştırıyorsanız, zaten bazı izleme alırsınız: varsayılan olarak istek ve hata oranları. Yanıt süreleri gibi almak, çağrıları izleme bağımlılıklara, akıllı algılama ve güçlü Kusto sorgu diline erişmek için Application Insights ekleyin. 

1. **Application Insights'ı seçin** uygulama hizmetiniz için Azure Denetim Masası'nda.

    ![Ayarlarda, Application ınsights'ı seçin.](./media/azure-web-apps/settings-app-insights.png)

   * Yeni bir kaynak oluşturmak bu uygulama için bir Application Insights kaynağı zaten ayarlamadıysanız'ı seçin. 

     > [!NOTE]
     > Tıkladığınızda **Tamam** için istenir yeni kaynak oluşturmak için **izleme ayarlarını uygula**. Seçme **devam** app Service'e kadar olacak de yapmak, yeni Application Insights kaynağınıza bağlayacaksınız **app service'inizi yeniden tetikleyin**. 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource.png)

2. Hangi kaynağı kullanacağını belirlemesinde belirttikten sonra her platformun uygulamanız için veri toplamak için application ınsights'ı nasıl istediğinizi seçebilirsiniz. ASP.NET uygulamasını izleme üzerinde varsayılan olarak koleksiyon iki farklı düzeylerine sahip.

    ![Platform başına seçenekleri belirleyin](./media/azure-web-apps/choose-options-new.png)

   * .NET **temel koleksiyonu** düzeyini temel Tek Örnekli APM özellikleri sunar.
    
   * .NET **koleksiyon önerilen** düzeyi:
       * CPU, bellek ve g/ç kullanım eğilimlerini ekler.
       * Mikro hizmetler, istek/bağımlılık sınırlarında ilişkilendirir.
       * Kullanım eğilimlerini toplar ve işlemler için kullanılabilirlik sonuçlarından bağıntı sağlar.
       * Ana bilgisayar işlemi tarafından işlenmemiş özel durumlar toplar.
       * Örnekleme kullanıldığında yük altında APM ölçümleri doğruluğu artırır.
    
     .NET core sunar **koleksiyon önerilen** veya **devre dışı bırakılmış** .NET Core 2.0 ve 2.1 için.

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

## <a name="automate-monitoring"></a>İzlemeyi otomatikleştirin

Application Insights ile telemetri koleksiyonunu etkinleştirmek için yalnızca uygulama ayarlarını ayarlamanız gerekir:

   ![App Service uygulama ayarları kullanılabilir Application Insights ayarlardan](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Uygulama ayarları tanımları

|Uygulama ayarı adı |  Tanım | Değer |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Ana uzantısı, çalışma zamanını izleme denetler. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  Varsayılan modu yalnızca, temel özelliklerini en iyi performans sağlamak için etkinleştirilir. | `default` veya `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Göndermediğini denetler ikili yeniden yazma motoru `InstrumentationEngine` alınır. Bu ayar, performans şifrelemelerini ve soğuk başlangıç/başlangıç süresini etkileyecek. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Metin, SQL ve Azure tablo, denetimleri, bağımlılık çağrılarını yakalanan. Performans Uyarısı: Bu gerektirir `InstrumentationEngine`. | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>App Service uygulama ayarları ile Azure Resource Manager

Uygulama hizmetleri için uygulama ayarları yönetilen ve yapılandırılmış [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Bu yöntem, yeni App Service kaynaklarının mevcut kaynakların ayarlarını değiştirmek için veya Azure Resource Manager otomasyon ile dağıtım yaparken kullanılabilir.

Bir app Service uygulama ayarları JSON temel yapısını aşağıda verilmiştir:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]

```

Uygulama ayarları şablonuyla bir örnek için bir Azure Resource Manager için Application Insights bu yapılandırılmış [şablon](https://github.com/Andrew-MSFT/BasicImageGallery) özellikle başlangıç bölüm yarayabilir [satır 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Bir Application Insights kaynağı ve yeni oluşturulan uygulama hizmetiniz için bağlantı oluşturulmasını otomatikleştirin.

Yapılandırılan tüm varsayılan Application Insights ayarları ile bir Azure Resource Manager şablonu oluşturmak için işlem alacağı Application Insights ile yeni bir Web uygulaması oluşturmak için kullanmaya başlayın.

Seçin **Otomasyon seçenekleri**

   ![App Service web uygulaması oluşturma menüsü](./media/azure-web-apps/create-web-app.png)

Bu, yapılandırılan tüm gerekli ayarları ile en son Azure Resource Manager şablonu oluşturur.

  ![App Service web uygulaması şablonu](./media/azure-web-apps/arm-template.png)

> [!NOTE]
> Şablon "varsayılan" modunda uygulama ayarları oluşturur. Tercih ettiğiniz hangi özelliklerini etkinleştirmek için şablonu değiştirebilirsiniz ancak bu performans için iyileştirilmiş, modudur.

## <a name="more-telemetry"></a>Daha fazla telemetri

* [Web sayfası yükleme verileri](../../azure-monitor/app/javascript.md)
* [Özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="do-i-still-need-to-go-to-extensions---add---application-insights-extension-for-new-app-service-apps"></a>Hala ihtiyacım uzantıları için Git-Ekle - yeni App Service uygulamaları için Application Insights uzantısı var?

Hayır, artık uzantıyı el ile eklemeniz gerekmez. Ayarlar dikey penceresi Application Insights'ı etkinleştirme izlemeyi etkinleştirmek için tüm gerekli uygulama ayarları ekler. Daha önce uzantısı tarafından eklenen dosyalar artık olduğundan bu artık, mümkündür [önceden](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) App Service görüntünün bir parçası olarak. Dosyalar bulunur `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent`.

### <a name="if-runtime-and-build-time-monitoring-are-both-enabled-do-i-end-up-with-duplicate-data"></a>Çalışma zamanı ve derleme zamanı izleme hem de etkinleştirilip etkinleştirilmediğini miyim yinelenen verilerle düştüğünden?

Hayır, varsayılan derleme zamanı izleme algılanırsa, çalışma zamanı izleme uzantısı veri gönderme durdurur ve izleme yapılandırması derleme zamanı olarak kullanılacaktır. Çalışma zamanını izleme devre dışı bırakılıp bırakılmayacağını belirlenmesi, bu üç dosyayı birinin algılamayı dayanır:

* Microsoft.applicationınsights dll
* Microsoft.ASP.NET.TelemetryCorrelation dll
* System.Diagnostics.DiagnosticSource dll

Birçok Visual Studio sürümlerinde, bazılarını veya tümünü bu dosyalar varsayılan olarak ASP.NET ve ASP.NET Core Visual Studio şablon dosyaları eklendiğini unutmayın önemlidir. Application Insights açıkça etkinleştirmediniz bile projenizi şablonlardan birini dışına göre oluşturulmuş olsa bile, dosya bağımlılık varlığını etkinleştirmesini çalışma zamanını izleme engeller.

### <a name="appinsightsjavascriptenabled-causes-incomplete-html-response-in-net-core-web-applications"></a>APPINSIGHTS_JAVASCRIPT_ENABLED tamamlanmamış HTML yanıtını NET CORE web uygulamaları neden olur.

JavaScript uygulama hizmetleri aracılığıyla etkinleştirme html yanıtları kesiliyor neden olabilir.

* Geçici Çözüm 1: APPINSIGHTS_JAVASCRIPT_ENABLED uygulama ayarını false olarak ayarlayın ya da tamamen kaldırın ve yeniden başlatın
* Geçici Çözüm 2: kod aracılığıyla SDK'sını ekleyin ve uzantısını kaldırma (Profiler ve anlık görüntü hata ayıklayıcı bu yapılandırma ile çalışmaz)

Bu sorunu izlemek için Git [tamamlanmamış HTML yanıtını neden Azure uzantısı](https://github.com/Microsoft/ApplicationInsights-Home/issues/277).

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