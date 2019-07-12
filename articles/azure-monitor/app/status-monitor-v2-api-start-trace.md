---
title: 'Azure Durum İzleyicisi v2 API Başvurusu: İzlemeyi Başlat | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API'si başvurusu. Start-izleme. Durum İzleyicisi'ni ve Application Insights SDK'sı ETW günlükleri toplayın.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: b6787134707273a76290adb723a9bc9012252ebd
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807051"
---
# <a name="status-monitor-v2-api-start-applicationinsightsmonitoringtrace-v040-alpha"></a>Durum İzleyicisi'ni v2 API'si: Start-ApplicationInsightsMonitoringTrace (v0.4.0-alpha)

Bu makalede bir üyesi olan bir cmdlet [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>Açıklama

Toplar [ETW olayları](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) Kodsuz çalışma zamanı ekleyin. Bu cmdlet'in çalışması için bir alternatifidir [PerfView](https://github.com/microsoft/perfview).

Toplanan olayları gerçek zamanlı ve kaydedilen bir ETL dosyası içindeki konsola yazdırılır. Çıkış ETL dosyası tarafından açılabilir [PerfView](https://github.com/microsoft/perfview) araştırılması için.

Zaman aşımı süresini (varsayılan 5 dakika) ulaştığında veya el ile durduruldu kadar bu cmdlet'i çalışır (`Ctrl + C`).

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

### <a name="how-to-collect-events"></a>Olayları toplama

Normalde olayları neden uygulamanızın izleme eklenmiş olmayan araştırmak için topladığınız sorduğu.

Çalışma zamanı Kodsuz ekleme ETW olayları IIS başlatıldığında ve uygulamanızı başladığında yayar.

Bu olayları toplamak için:
1. Yönetici ayrıcalıklarıyla bir komut konsolunda yürütün `iisreset /stop` tüm web uygulamaları ile IIS açıp kapatmak için.
2. Bu cmdlet'ini yürütün
3. Yönetici ayrıcalıklarıyla bir komut konsolunda yürütün `iisreset /start` IIS başlatmak için.
4. Uygulamanız için göz atmayı deneyin.
5. Uygulamanızı yükleme tamamlandıktan sonra el ile durdurabilirsiniz (`Ctrl + C`) veya zaman aşımı'için bekleyin.

### <a name="what-events-to-collect"></a>Hangi olayları toplamak için

Olayları toplanırken üç seçeneğiniz vardır:
1. Anahtarını kullanarak `-CollectSdkEvents` Application Insights SDK'sından yayılan olaylarını toplamak için.
2. Anahtarını kullanarak `-CollectRedfieldEvents` Durum İzleyicisi'ni ve Redfield çalışma zamanı tarafından oluşturulan olayları toplamak için. IIS özel durumunda tanılama yaparken bu günlükler yararlı olur ve uygulama başlatma.
3. Her iki olay türlerini, toplanacak her iki anahtarlarını kullanın.
4. Her iki olay türleri toplanan hiçbir anahtarı belirtilmediyse, varsayılan olarak.


## <a name="parameters"></a>Parametreler

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**İsteğe bağlı.** Bu betik, olayları ne kadar süreyle toplamanız gerekir ayarlamak için bu parametreyi kullanın. Varsayılan değer 5 dakikadır.

### <a name="-logdirectory"></a>-LogDirectory
**İsteğe bağlı.** ETL dosyasının çıkış dizinine ayarlamak için bu anahtarı kullanın. Varsayılan olarak, bu dosya PowerShell modülleri dizininde oluşturulur. Betik yürütme sırasında tam yolunu görüntülenir.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**İsteğe bağlı.** Application Insights SDK'sı olaylarını toplamak için bu anahtarı kullanın.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**İsteğe bağlı.** Durum İzleyicisi'ni ve Redfield çalışma zamanı olayları toplamak için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.



## <a name="output"></a>Output


### <a name="example-of-application-startup-logs"></a>Uygulama başlatma günlükleri örneği
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme:

- Burada ek sorun giderme adımlarını gözden geçirin: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Gözden geçirme [API Başvurusu](status-monitor-v2-overview.md#powershell-api-reference) kaçırdığınıza parametreleri hakkında bilgi edinmek için.
- Ek yardıma ihtiyacınız varsa, bize üzerinde başvurabilirsiniz [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).



 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Kılavuzunu kullanın [sorun giderme](status-monitor-v2-troubleshoot.md) Durum İzleyicisi v2.
 - [Yapılandırmasını al](status-monitor-v2-api-get-config.md) ayarlarınızı doğru kaydedilmiş olduğunu onaylamak için.
 - [Durumu Al](status-monitor-v2-api-get-status.md) izleme incelemek için.
