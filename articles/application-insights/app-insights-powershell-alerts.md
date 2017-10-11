---
title: "Application Insights'ta uyarıları ayarlamak için PowerShell kullanma | Microsoft Docs"
description: "Ölçüm değişiklikler hakkında e-postaları almak için Application Insights yapılandırmasını otomatikleştirin."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: 64675c51abf80daa3a55220f910aa8fdee1042ca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Application Insights uyarıları ayarlamak için PowerShell kullanma
Yapılandırmasını otomatikleştirebilirsiniz [uyarıları](app-insights-alerts.md) içinde [Application Insights](app-insights-overview.md).

Buna ek olarak, şunları yapabilirsiniz [ayarlamak yanıtınız bir uyarıya otomatik hale getirmek için Web kancası](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Aynı anda kaynakları ve Uyarıları oluşturmak istiyorsanız, göz önünde bulundurun [bir Azure Resource Manager şablonu kullanarak](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Tek seferlik Kurulumu
Önce Azure aboneliğinizle PowerShell kullanmadıysanız:

Komut dosyalarını çalıştırmak istediğiniz Azure Powershell modülünü yükleyin.

* Yükleme [Microsoft Web Platformu Yükleyicisi (v5 veya üzeri)](http://www.microsoft.com/web/downloads/platform.aspx).
* Microsoft Azure PowerShell'i yüklemek için kullanın

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Azure PowerShell'i başlatın ve [aboneliğinize bağlanma](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Uyarıları alma
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Uyarı ekleme
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Örnek 1
Sunucu yanıtı üzerinde 5 dakika ortalaması HTTP isteklerine 1 saniyeden daha yavaş ise bana e-posta. My Application Insights kaynağı IceCreamWebApp denir ve Fabrikam kaynak grubunda bulunuyor. Azure aboneliğinin sahibi ben.

Abonelik kimliği (değil uygulamanın izleme anahtarı) GUID'dir.

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Örnek 2
Bir uygulama içinde kullanmam sahip [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) "salesPerHour." adlı bir ölçüm bildirmek için "SalesPerHour" 100 düşerse bir e-posta my arkadaşlarınıza üzerinde 24 saat ortalaması gönderin.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Aynı kural kullanılarak bildirilen ölçümü için kullanılabilir [ölçüm parametresi](app-insights-api-custom-events-metrics.md#properties) TrackEvent veya trackPageView gibi başka bir izleme çağrısı.

## <a name="metric-names"></a>Ölçüm adları
| Ölçüm adı | Ekran adı | Açıklama |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Tarayıcı özel durumları |Tarayıcıda oluşturulan Yakalanmayan Özel durumların sayısı. |
| `basicExceptionServer.count` |Sunucu özel durumları |Uygulama tarafından oluşturulan işlenmemiş özel durum sayısı |
| `clientPerformance.clientProcess.value` |İstemci işlem süresi |DOM yüklenene kadar belgeyi son baytını alma arasındaki süre. Zaman uyumsuz isteği hala işliyor olabilir. |
| `clientPerformance.networkConnection.value` |Sayfa yükleme ağ bağlantı süresi |Tarayıcı ağa bağlanmak için gereken süreyi. Önbelleğe alınmış 0 olabilir. |
| `clientPerformance.receiveRequest.value` |Alıcı yanıt süresi |Yanıt almayı başlatma isteği gönderilirken tarayıcı arasındaki süre. |
| `clientPerformance.sendRequest.value` |İstek süresi Gönder |İsteği göndermek için tarayıcı tarafından harcanan süre. |
| `clientPerformance.total.value` |Tarayıcı sayfa yükleme süresi |DOM, stil sayfaları, betikler ve görüntüleri yüklenene kadar kullanıcı isteği süresi. |
| `performanceCounter.available_bytes.value` |Kullanılabilir bellek |Bir işlem veya sistem kullanımı için hemen kullanılabilir fiziksel bellek. |
| `performanceCounter.io_data_bytes_per_sec.value` |İşlemin g/ç hızı |Okunabilir ve yazılabilir dosyaları, ağ ve aygıtlar için saniye başına toplam bayt sayısı. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |özel durum oranı |Saniye başına oluşturulan özel durumları. |
| `performanceCounter.percentage_processor_time.value` |İşlem CPU |Tüm işlem iş parçacıklarının yönergeleri yürütmek için işlemciyi tarafından uygulamaları işlemi için kullanılan geçen sürenin yüzdesi. |
| `performanceCounter.percentage_processor_total.value` |İşlemci zamanı |İşlemcinin boşta olmayan iş parçacıklarında harcadığı zamanı yüzdesi. |
| `performanceCounter.process_private_bytes.value` |İşlem özel bayt sayısı |Özel olarak izlenen uygulamanın işlemlere atanan bellek. |
| `performanceCounter.request_execution_time.value` |ASP.NET isteği yürütme süresi |En son isteği yürütme süresi. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET isteklerini yürütme kuyruğundaki |Uygulama istek sırası uzunluğu. |
| `performanceCounter.requests_per_sec.value` |ASP.NET isteği hızı |Saniye başına uygulamaya ASP.NET gelen tüm isteklerin oranı. |
| `remoteDependencyFailed.durationMetric.count` |Bağımlılık hataları |Sunucu uygulaması için dış kaynak tarafından yapılan başarısız çağrı sayısı. |
| `request.duration` |Sunucu yanıt süresi |Bir HTTP isteği alma ve yanıtı göndermeyi bitirmeden arasındaki süre. |
| `request.rate` |İstek oranı |Saniye başına uygulama için tüm istekleri oranı. |
| `requestFailed.count` |Başarısız istekler |Bir yanıt kodu ile sonuçlandı sayısı, HTTP isteklerini > = 400 |
| `view.count` |Sayfa görünümleri |Bir web sayfası için istemci kullanıcı isteklerini sayısı. Yapay trafiği filtrelenmelidir. |
| {, özel ölçüm adı} |{Ölçüm adı} |Ölçüm değeri tarafından bildirilen [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) veya [izleme çağrı ölçümleri parametresinin](app-insights-api-custom-events-metrics.md#properties). |

Ölçümleri farklı telemetri modülleri tarafından gönderilir:

| Ölçüm grubu | Toplayıcı Modülü |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>görünüm |[Tarayıcı JavaScript](app-insights-javascript.md) |
| performanceCounter |[Performans](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Bağımlılık](app-insights-configuration-with-applicationinsights-config.md) |
| isteği<br/>requestFailed |[Sunucu isteği](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Web kancaları
Yapabilecekleriniz [bir uyarı yanıtınızı otomatikleştirmek](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Bir uyarı ortaya çıktığında azure tercih ettiğiniz bir web adresini çağırır.

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights yapılandırmak için komut dosyası](app-insights-powershell-script-create-resource.md)
* [Application Insights ve web test kaynakları Şablondan Oluştur](app-insights-powershell.md)
* [Microsoft Azure tanılama Application Insights'a Kuplaj otomatikleştirme](app-insights-powershell-azure-diagnostics.md)
* [Bir uyarı yanıtınızı otomatikleştirme](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
