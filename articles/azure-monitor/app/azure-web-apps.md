---
title: Azure uygulama hizmetleri performansını izleme | Microsoft Docs
description: Azure uygulama hizmetleri için uygulama performansı izleme. Yük ve yanıt süresi, bağımlılık bilgilerinin grafiğini oluşturun ve performansa bağlı uyarılar ayarlayın.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: mbullwin
ms.openlocfilehash: 9d121146924eb153227e35d608a3c6c33aae31a1
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862616"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service performansını izleme

.NET ve .NET Core üzerinde Azure App Services'ta çalışan tabanlı web uygulamaları izlemeyi etkinleştirme artık her zamankinden daha kolaydır. Daha önce el ile bir site uzantısını yüklemek için gereken diğer yandan en son uzantısı/aracı artık varsayılan olarak app service görüntüye oluşturulmuştur. Bu makalede, Application Insights izleme ile etkinleştirme işleminde size yol yanı sıra büyük ölçekli dağıtımlar için işlemini otomatik hale getirmek için başlangıç rehberlik sağlar.

> [!NOTE]
> Bir Application Insights site uzantısı aracılığıyla el ile ekleme **geliştirme araçları** > **uzantıları** kullanım dışı bırakılmıştır. Uzantının en son kararlı sürüm sunulmuştur [önceden](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) App Service görüntünün bir parçası olarak. Dosyalar bulunur `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` ve kararlı her sürümde otomatik olarak güncelleştirilir. İzlemeyi etkinleştirmek için aracı tabanlı yönergeleri izlerseniz aşağıda bunu otomatik olarak kullanım dışı uzantı sizin için kaldırılır.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Azure App Services, barındırılan uygulamalar için uygulama izlemeyi etkinleştirmek için iki yolu vardır:

* **Aracı tabanlı uygulama izleme** (ApplicationInsightsAgent).  
    * Bu yöntem etkinleştirmek en kolay olan ve Gelişmiş Yapılandırma gerekmiyor. Bu genellikle "çalışma zamanı" izleme olarak adlandırılır. Azure uygulama hizmetleri için Biz bu düzeyde izlemeyi etkinleştirme en azından önerilir ve ardından daha gelişmiş el ile izleme izleme gerekli olup olmadığını değerlendirebilirsiniz, belirli senaryo temel alınarak.

* **El ile kod aracılığıyla uygulama izleme** Application Insights SDK'sını yükleyerek.

    * Bu yaklaşım çok daha fazla özelleştirilebilir, ancak gerektirir [Application Insights SDK'sı NuGet paketleri bağımlılık ekleme](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Bu yöntem aynı zamanda anlamına gelir paketlerin en son sürüme güncelleştirmelerini kendiniz yönetmeniz gerekiyorsa.

    * İzleme olayları/bağımlılıklara aracı tabanlı izleme varsayılan olarak yakalanan değil özel API çağrısı yapmanız gerekirse bu yöntemi kullanmak gerekir. Kullanıma [API'si için özel olaylar ve ölçümler makale](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) daha fazla bilgi için.

> [!NOTE]
> Aracı tabanlı izleme hem el ile SDK Araçları tabanlıysa yalnızca el ile izleme ayarları kullanılacaktır algılandı. Bu yinelenen veri önlemek içindir gönderen. Bu kullanıma hakkında daha fazla bilgi edinmek için [sorun giderme bölümüne](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting) aşağıda.

## <a name="enable-agent-based-monitoring-net"></a>Aracı tabanlı izleme .NET etkinleştir

1. **Application Insights'ı seçin** uygulama hizmetiniz için Azure Denetim Masası'nda.

    ![Ayarlarda, Application ınsights'ı seçin.](./media/azure-web-apps/settings-app-insights-01.png)

   * Yeni bir kaynak oluşturmak bu uygulama için bir Application Insights kaynağı zaten ayarlamadıysanız'ı seçin. 

     > [!NOTE]
     > Tıkladığınızda **Tamam** için istenir yeni kaynak oluşturmak için **izleme ayarlarını uygula**. Seçme **devam** app Service'e kadar olacak de yapmak, yeni Application Insights kaynağınıza bağlayacaksınız **app service'inizi yeniden tetikleyin**. 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağı kullanacağını belirlemesinde belirttikten sonra her platformun uygulamanız için veri toplamak için application ınsights'ı nasıl istediğinizi seçebilirsiniz. ASP.NET uygulamasını izleme üzerinde varsayılan olarak koleksiyon iki farklı düzeylerine sahip.

    ![Platform başına seçenekleri belirleyin](./media/azure-web-apps/choose-options-new.png)

   * .NET **temel koleksiyonu** düzeyini temel Tek Örnekli APM özellikleri sunar.

   * .NET **koleksiyon önerilen** düzeyi:
       * CPU, bellek ve g/ç kullanım eğilimlerini ekler.
       * Mikro hizmetler, istek/bağımlılık sınırlarında ilişkilendirir.
       * Kullanım eğilimlerini toplar ve işlemler için kullanılabilirlik sonuçlarından bağıntı sağlar.
       * Ana bilgisayar işlemi tarafından işlenmemiş özel durumlar toplar.
       * Örnekleme kullanıldığında yük altında APM ölçümleri doğruluğu artırır.

3. Daha önce applicationınsights.config dosyası denetleyebilirsiniz örnekleme gibi ayarları yapılandırmak için artık aynı bu ayarları uygulama ayarlarından karşılık gelen bir önek ile etkileşim kurabilirsiniz. 

    * Örneğin, ilk örnekleme yüzdesini değiştirmek için bir uygulama ayarı oluşturabilirsiniz: `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` değerini `100`.

    * Desteklenen Uyarlamalı örnekleme telemetri işlemci ayarları listesi için başvurabilirsiniz [kod](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/master/src/ServerTelemetryChannel/AdaptiveSamplingTelemetryProcessor.cs) ve [ilişkili belgeler](https://docs.microsoft.com/azure/azure-monitor/app/sampling).

## <a name="enable-agent-based-monitoring-net-core"></a>Aracı tabanlı izleme .NET Core etkinleştir

.NET Core'nın şu sürümleri desteklenir: ASP.NET Core 2.0, ASP.NET Core 2.1, ASP.NET Core 2.2

.NET Core, kendi başına dağıtım ve ASP.NET Core 3.0 tam çerçeve hedefleme şu anda **desteklenmiyor** Aracı/Uzantısı tabanlı izleme. ([El ile izleme](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) kod önceki senaryoların tümünde çalışır.)

1. **Application Insights'ı seçin** uygulama hizmetiniz için Azure Denetim Masası'nda.

    ![Ayarlarda, Application ınsights'ı seçin.](./media/azure-web-apps/settings-app-insights-01.png)

   * Yeni bir kaynak oluşturmak bu uygulama için bir Application Insights kaynağı zaten ayarlamadıysanız'ı seçin. 

     > [!NOTE]
     > Tıkladığınızda **Tamam** için istenir yeni kaynak oluşturmak için **izleme ayarlarını uygula**. Seçme **devam** app Service'e kadar olacak de yapmak, yeni Application Insights kaynağınıza bağlayacaksınız **app service'inizi yeniden tetikleyin**. 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağı kullanacağını belirlemesinde belirttikten sonra her platformun uygulamanız için veri toplamak için Application ınsights'ı nasıl istediğinizi seçebilirsiniz. .NET core sunar **koleksiyon önerilen** veya **devre dışı bırakılmış** .NET Core 2.0 ve 2.1 2.2 için.

    ![Platform başına seçenekleri belirleyin](./media/azure-web-apps/choose-options-new-net-core.png)

## <a name="enable-client-side-monitoring-net"></a>İstemci-tarafı izleme .NET etkinleştir

Katılımı için ASP.NET istemci-tarafı izleme. İstemci-tarafı izlemeyi etkinleştirmek için:

* Seçin **ayarları** > ** ** *** Uygulama ayarları
   * Uygulama ayarları, yeni bir ekleme **uygulama ayarı adı** ve **değer**:

     Ad: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer: `true`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

![Uygulamasının ekran görüntüsü ayarları kullanıcı Arabirimi](./media/azure-web-apps/appinsights-javascript-enabled.png)

İçin istemci tarafı uygulama ayarlarından ilişkili anahtar değer çifti kaldırabilir ya da izleme devre dışı bırakın veya değerini false olarak ayarlayın.

## <a name="enable-client-side-monitoring-net-core"></a>İstemci-tarafı izleme .NET Core etkinleştir

İstemci-tarafı izleme **varsayılan olarak etkin** ile .NET Core uygulamaları için **koleksiyon önerilen**uygulama ayarı 'APPINSIGHTS_JAVASCRIPT_ENABLED' bulunup bulunmadığını bakılmaksızın.

Herhangi bir nedenden dolayı istemci-tarafı izlemeyi devre dışı bırakmak istiyorsanız:

* Seçin **ayarları** > **uygulama ayarları**
   * Uygulama ayarları, yeni bir ekleme **uygulama ayarı adı** ve **değer**:

     Adı: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer: `false`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

![Uygulamasının ekran görüntüsü ayarları kullanıcı Arabirimi](./media/azure-web-apps/appinsights-javascript-disabled.png)

## <a name="automate-monitoring"></a>İzlemeyi otomatikleştirin

Application Insights ile telemetri koleksiyonunu etkinleştirmek için yalnızca uygulama ayarlarını ayarlanması gerekir:

   ![App Service uygulama ayarları kullanılabilir Application Insights ayarlardan](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Uygulama ayarları tanımları

|Uygulama ayarı adı |  Tanım | Değer |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Ana uzantısı, çalışma zamanını izleme denetler. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  Varsayılan modu yalnızca, temel özelliklerini en iyi performans sağlamak için etkinleştirilir. | `default` veya `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Göndermediğini denetler ikili yeniden yazma motoru `InstrumentationEngine` alınır. Bu ayar, performans şifrelemelerini ve soğuk başlangıç/başlangıç süresini etkileyecek. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Metin, SQL ve Azure tablo, denetimleri, bağımlılık çağrılarını yakalanan. Performans Uyarısı: Bu ayar `InstrumentationEngine`. | `~1` |

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

Application Insights için yapılandırılan uygulama ayarları ile bir Azure Resource Manager şablonu örneği için bu [şablon](https://github.com/Andrew-MSFT/BasicImageGallery) özellikle başlangıç bölüm yarayabilir [satır 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Bir Application Insights kaynağı ve yeni oluşturulan uygulama hizmetiniz için bağlantı oluşturulmasını otomatikleştirin.

Yapılandırılan tüm varsayılan Application Insights ayarları ile bir Azure Resource Manager şablonu oluşturmak için işlem alacağı Application Insights ile yeni bir Web uygulaması oluşturmak için kullanmaya başlayın.

Seçin **Otomasyon seçenekleri**

   ![App Service web uygulaması oluşturma menüsü](./media/azure-web-apps/create-web-app.png)

Bu seçenek, yapılandırılan tüm gerekli ayarları ile en son Azure Resource Manager şablonu oluşturur.

  ![App Service web uygulaması şablonu](./media/azure-web-apps/arm-template.png)

Bir örneği aşağıda verilmiştir tüm örneklerinin yerine `AppMonitoredSite` ile site adı:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

> [!NOTE]
> Şablon "varsayılan" modunda uygulama ayarları oluşturur. Tercih ettiğiniz hangi özelliklerini etkinleştirmek için şablonu değiştirebilirsiniz ancak bu performans için iyileştirilmiş, modudur.

### <a name="enabling-through-powershell"></a>PowerShell aracılığıyla etkinleştirme

Uygulaması PowerShell aracılığıyla izlemeyi etkinleştirmek için yalnızca temel uygulama ayarlarının değiştirilmesi gerekir. "AppMonitoredSite" kaynak grubunda "AppMonitoredRG" adlı bir Web sitesi için uygulama izleme sağlayan bir örnek aşağıda verilmiştir ve için "012345678-abcd-ef01-2345-6789abcd" izleme anahtarını gönderilecek verileri yapılandırır.

```powershell
$app = Get-AzureRmWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} #preserve non Application Insights Application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # enable the ApplicationInsightsAgent
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzureRmWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>İzleme uzantısı/aracıyı yükseltme

### <a name="upgrading-from-versions-289-and-up"></a>2.8.9 sürümlerinden ve yedekleme yükseltme

2.8.9 sürümünden yükseltirken, herhangi bir ek eylem otomatik olarak gerçekleşir. Yeni izleme BITS arka planda hedef app Service'e dağıtılır ve uygulama yeniden başlatma sırasında bunlar seçilir.

Uzantı hangi sürümünü denetlemek için şu adresi ziyaret edin çalıştırıyorsanız `http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![Url yolu ekran görüntüsü http://yoursitename.scm.azurewebsites.net/ApplicationInsights](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>Sürümlerden 1.0.0 - 2.6.5 yükseltme

2.8.9 sürümünden başlayarak, önceden yüklenmiş site uzantısı kullanılır. Önceki bir sürümü varsa, iki yoldan biriyle güncelleştirebilirsiniz:

* [Portal üzerinden etkinleştirerek yükseltme](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights). (Kullanıcı Arabirimi yüklü Azure App Service için Application Insights uzantısını olsa bile yalnızca gösterir **etkinleştirme** düğmesi. Arka planda, eski özel site uzantısı kaldırılacak.)

* [PowerShell aracılığıyla yükseltme](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell):

    1. Önceden yüklenmiş site uzantısı ApplicationInsightsAgent etkinleştirmek için uygulama ayarlarını belirleyin. Bkz: [powershell aracılığıyla etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell).
    2. Azure App Service için Application Insights uzantısını adlı özel site uzantısı el ile kaldırın.

2.5.1 önceki bir sürümünden yükseltme yapıldığında ApplicationInsigths DLL'leri uygulama bin klasöründen kaldırılır denetleyin [sorun giderme adımları görmek](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting).

## <a name="troubleshooting"></a>Sorun giderme

.NET için Uzantı/Aracı tabanlı izleme için adım adım sorun giderme kılavuzumuza aşağıda verilmiştir ve .NET Core tabanlı Azure App Services üzerinde çalışan uygulamalar.

> [!NOTE]
> Java ve Node.js uygulamaları el ile Temel SDK Araçları yalnızca Azure App Services'ta desteklenir ve bu nedenle aşağıdaki adımları bu senaryolar için geçerli değildir.

1. Uygulama aracılığıyla izlenen onay `ApplicationInsightsAgent`.
    * Bu maddeyi `ApplicationInsightsAgent_EXTENSION_VERSION` uygulama ayarı "~ 2" değerine ayarlanır.
2. Uygulamanın izlenmesi için gereksinimleri karşıladığından emin olun.
    * Şu konuma gidin: `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`

    ![Ekran görüntüsü https://yoursitename.scm.azurewebsites/applicationinsights sonuçları sayfası](./media/azure-web-apps/app-insights-sdk-status.png)

    * Onaylayın `Application Insights Extension Status` olduğu `Pre-Installed Site Extension, version 2.8.12.1527, is running.`
        * Çalışmıyorsa, izleyin [yönergeleri izleme Application ınsights'ı etkinleştir](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights)

    * Durum kaynak var olduğunu ve benzer olduğunu onaylayın: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Benzer bir değer yoksa, bu uygulama şu anda çalışmıyor veya desteklenmeyen anlamına gelir. Uygulamasının çalıştığından emin olmak için el ile çalışma zamanı bilgilerin kullanılabilir olmasını sağlayan uygulama URL'si/uygulama uç noktaları ziyaret edin.

    * Onaylayın `IKeyExists` olduğu `true`
        * False ise,'ekle ' appınsıghts_ınstrumentatıonkey uygulama ayarlarınıza, ikey GUID'e sahip.

    * Onaylamak için girdi yok `AppAlreadyInstrumented`, `AppContainsDiagnosticSourceAssembly`, ve `AppContainsAspNetTelemetryCorrelationAssembly`.
        * Bu girişlerin varsa, aşağıdaki paketleri uygulamanızdan Kaldır: `Microsoft.ApplicationInsights`, `System.Diagnostics.DiagnosticSource`, ve `Microsoft.AspNet.TelemetryCorrelation`.

Aşağıdaki tabloda bu değerlerin anlamları daha ayrıntılı bir açıklama sağlar, kendi temel neden olur ve düzeltmeleri önerilir:

|Sorun değeri|Açıklama|Düzelt
|---- |----|---|
| `AppAlreadyInstrumented:true` | Bu değer, uzantı SDK'ın bazı yönlerini uygulamada zaten var ve geri alma, algılandığını gösterir. Bir başvuru nedeniyle olabilir `System.Diagnostics.DiagnosticSource`, `Microsoft.AspNet.TelemetryCorrelation`, veya `Microsoft.ApplicationInsights`  | Başvuruları kaldırın. Bu başvurular bazıları varsayılan olarak belirli Visual Studio şablonlardan eklenir ve Visual Studio'nun eski sürümlerini başvuruları ekleyebilir `Microsoft.ApplicationInsights`.
|`AppAlreadyInstrumented:true` | Bu değer aynı zamanda önceki bir dağıtım uygulama klasöründen yukarıdaki DLL'leri varlığını neden olabilir. | Bu DLL'leri kaldırıldığından emin olmak için uygulama klasörü temizleyin.|
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Bu değer uzantısı başvuruları algılandığını gösterir `Microsoft.AspNet.TelemetryCorrelation` uygulamasının ve geri alma. | Başvuruyu kaldırın.
|`AppContainsDiagnosticSourceAssembly**:true`|Bu değer uzantısı başvuruları algılandığını gösterir `System.Diagnostics.DiagnosticSource` uygulamasının ve geri alma.| Başvuruyu kaldırın.
|`IKeyExists:false`|İzleme anahtarı uygulama ayarı mevcut değil, bu değeri gösterir `APPINSIGHTS_INSTRUMENTATIONKEY`. Olası nedenler: Değerleri, yanlışlıkla kaldırılmış olabilir, Otomasyon betiği, vb. değerleri ayarlamak unuttum. | App Service uygulama ayarlarında ayarı bulunduğundan emin olun.

Application Insights Aracısı/uzantı en son bilgiler için kullanıma [sürüm notları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](../app/profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](../platform/diagnostics-extension-to-application-insights.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../platform/data-platform.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../platform/alerts-overview.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](monitor-web-app-availability.md) ayarlayın.
