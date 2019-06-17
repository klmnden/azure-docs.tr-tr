---
title: Application Insights uyarıları ayarlamak için PowerShell kullanma | Microsoft Docs
description: Ölçüm değişiklikler hakkında e-postaları almak için Application Insights yapılandırmasını otomatikleştirin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: 5dfbc6fa18b5d1b5b3058db14eb1232be27a0c40
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66130971"
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Application Insights uyarıları ayarlamak için PowerShell kullanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Yapılandırılmasını otomatikleştirebilirsiniz [uyarılar](../../azure-monitor/app/alerts.md) içinde [Application Insights](../../azure-monitor/app/app-insights-overview.md).

Ayrıca, aşağıdakileri yapabilirsiniz [ayarlanmış bir uyarı yanıtları otomatik hale getirmek için Web kancaları](../../azure-monitor/platform/alerts-webhooks.md).

> [!NOTE]
> Kaynaklar ve uyarılar aynı anda oluşturmak isterseniz, göz önünde bulundurun [bir Azure Resource Manager şablonu kullanarak](powershell.md).

## <a name="one-time-setup"></a>Bir kerelik Kurulum
Azure aboneliğiniz önce PowerShell kullanmadıysanız:

Azure Powershell modülü, komut dosyalarını çalıştırmak istediğiniz makineye yükleyin.

* Yükleme [Microsoft Web Platformu Yükleyicisi (v5 veya üzeri)](https://www.microsoft.com/web/downloads/platform.aspx).
* Microsoft Azure PowerShell'i yüklemek için kullanın

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Azure PowerShell'i başlatın ve [aboneliğinize bağlanma](/powershell/azure/overview):

```powershell

    Add-AzAccount
```


## <a name="get-alerts"></a>Uyarılar alın
    Get-AzAlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Uyarı Ekle
    Add-AzMetricAlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
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
HTTP isteklerini, üzerinde 5 dakika ortalama sunucu yanıtı 1 saniye yavaş ise bana e-posta. My Application Insights kaynağı IceCreamWebApp denir ve Fabrikam kaynak grubunda bulunuyor. Azure aboneliğinin sahibi ortağıyım.

Abonelik kimliği (değil uygulamanızın izleme anahtarını) GUID'dir.

    Add-AzMetricAlertRule -Name "slow responses" `
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
Bir uygulama içinde kullanmam sahibim [TrackMetric()](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric) "salesPerHour." adlı bir ölçüm bildirmek için "SalesPerHour" 100 düşerse bir e-posta arkadaşlarım 24 saat içinde ortalama gönderin.

    Add-AzMetricAlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Aynı kural kullanılarak bildirilen ölçüm için kullanılabilir [ölçüm parametresi](../../azure-monitor/app/api-custom-events-metrics.md#properties) TrackEvent veya trackPageView gibi başka bir izleme çağrısı.

## <a name="metric-names"></a>Ölçüm adları
| Ölçüm adı | Ekran adı | Açıklama |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Tarayıcı özel durumları |Tarayıcıda oluşan Yakalanmayan Özel durum sayısı. |
| `basicExceptionServer.count` |Sunucu özel durumları |Uygulama tarafından oluşturulan yakalanamayan özel durum sayısı |
| `clientPerformance.clientProcess.value` |İstemci işlem süresi |DOM'un yüklenmesi arasında bir belgenin son bayt alma süresi. Zaman uyumsuz istekler hala işleniyor. |
| `clientPerformance.networkConnection.value` |Sayfa yükleme ağ bağlantı süresi |Ağa bağlanmak için tarayıcıya geçen süre. Önbelleğe alınmış 0 olabilir. |
| `clientPerformance.receiveRequest.value` |Yanıt süresi alınıyor |Yanıt almak başlatma isteği gönderilirken tarayıcı arasındaki süre. |
| `clientPerformance.sendRequest.value` |İstek gönderme süresi |İsteği göndermek için tarayıcı tarafından harcanan süre. |
| `clientPerformance.total.value` |Tarayıcı sayfa yükleme süresi |Kullanıcı isteğinden DOM, stil sayfaları, betikler ve resimler yüklenene kadar süre. |
| `performanceCounter.available_bytes.value` |Kullanılabilir bellek |Bir işlem veya sistem kullanımı için hemen kullanılabilir fiziksel bellek. |
| `performanceCounter.io_data_bytes_per_sec.value` |İşlem GÇ hızı |Saniyede okunan ve dosyaları, ağ ve cihazlar için yazılan toplam bayt sayısı. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |özel durum oranı |Saniye başına oluşturulan bir özel durumlar. |
| `performanceCounter.percentage_processor_time.value` |İşlem CPU'su |Tüm işlem iş parçacıklarının yönergeleri yürütmek için işlemciyi tarafından uygulama işlemi için kullanılan geçen süre yüzdesi. |
| `performanceCounter.percentage_processor_total.value` |İşlemci zamanı |İşlemcinin boşta olmayan iş parçacıklarında geçirdiği sürenin yüzdesi. |
| `performanceCounter.process_private_bytes.value` |İşleme özel bayt sayısı |İzlenen uygulama işlemleri için özel olarak atanan bellek. |
| `performanceCounter.request_execution_time.value` |ASP.NET isteği yürütme süresi |En son isteği yürütme süresi. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET isteklerini yürütme sırası |Uygulama istek kuyruğunun uzunluğu. |
| `performanceCounter.requests_per_sec.value` |ASP.NET isteği hızı |ASP.net'ten saniyede uygulamaya yapılan tüm isteklerin oranı. |
| `remoteDependencyFailed.durationMetric.count` |Bağımlılık hataları |Sunucu uygulama tarafından dış kaynaklara yapılan başarısız çağrıların sayısı. |
| `request.duration` |Sunucu yanıt süresi |Bir HTTP isteğinin alınmasıyla yanıtın gönderilmesi tamamlama arasındaki süre. |
| `request.rate` |İstek oranı |Saniyede uygulamaya yapılan tüm isteklerin oranı. |
| `requestFailed.count` |Başarısız istekler |Bir yanıt kodunda sonuçlanan HTTP isteği sayısı > 400 = |
| `view.count` |Sayfa görüntülemeleri |Bir web sayfası için istemci kullanıcı isteklerini sayısı. Yapay trafik filtrelendi. |
| {, özel ölçüm adı} |{Ölçüm adı} |Ölçüm, değer tarafından bildirilen [TrackMetric](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric) veya [ölçümleri parametresi bir izleme çağrısının](../../azure-monitor/app/api-custom-events-metrics.md#properties). |

Ölçümler, farklı telemetri modülleri tarafından gönderilir:

| Ölçüm grubu | Toplayıcı Modülü |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>görünüm |[JavaScript tarayıcı](../../azure-monitor/app/javascript.md) |
| PerformanceCounter |[Performans](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Bağımlılık](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |
| İstek,<br/>requestFailed |[Sunucu isteği](../../azure-monitor/app/configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Web Kancaları
Yapabilecekleriniz [yanıtınızı bir uyarıya](../../azure-monitor/platform/alerts-webhooks.md). Bir uyarı oluşturulduğunda azure, tercih ettiğiniz bir web adresini çağırır.

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights'ı yapılandırmak için komut dosyası](powershell-script-create-resource.md)
* [Application ınsights'ı ve web testi kaynakları şablonlardan oluşturma](powershell.md)
* [Application ınsights'ı Microsoft Azure tanılama eşlenmesiyle otomatikleştirin](powershell-azure-diagnostics.md)
* [Bir uyarıya yanıt otomatikleştirin](../../azure-monitor/platform/alerts-webhooks.md)
