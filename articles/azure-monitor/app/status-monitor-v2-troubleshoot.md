---
title: Azure Durum İzleyicisi v2 giderme ve bilinen sorunlar | Microsoft Docs
description: Durum İzleyicisi v2 ve örnekler sorun giderme bilinen sorunlar. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. ASP.NET web uygulamaları ile çalışır, şirket içi Vm'leri içinde veya azure'da barındırılan.
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
ms.openlocfilehash: df59766ce38ac81568570cd6544ee28808ff8249
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807024"
---
# <a name="troubleshooting-status-monitor-v2"></a>Sorun giderme durumu izleme v2

İzleme'yi etkinleştirdikten sonra veri toplamayı kapatmasına sorunlarıyla karşılaşabilirsiniz.
Bu makalede, tüm bilinen sorunları listeler ve sorun giderme örnekler sağlar.
Burada listelenmeyen bir sorun arasında geliyorsa, bize üzerinde başvurabilirsiniz [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor ve bazıları kısıtlı yeteneklere sahip.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>Bir uygulamanın bin dizininde çakışan DLL'leri

Bu DLL'leri hiçbirini bin dizininde mevcut olması durumunda, izleme başarısız olabilir:

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Uygulamanızı bunları kullanmıyor olsa bile bu DLL'leri bazı Visual Studio varsayılan uygulama şablonları dahil edilir.
Belirtisi davranışını görmek için sorun giderme araçları kullanabilirsiniz:

- PerfView:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- IISReset ve uygulama (telemetri) yükleyin. Sysinternals (Handle.exe ve ListDLLs.exe) araştırın:
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>IIS paylaşılan yapılandırması ile çakışma

Web sunucularının bir kümesi varsa kullanabilecek bir [paylaşılan yapılandırma](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
HttpModule paylaşılan yapılandırmanın eklenen olamaz.
Her bir sunucunun GAC içine DLL'yi yüklemek için her web sunucusunda etkinleştir komutu çalıştırın.

Enable komutunu çalıştırdıktan sonra aşağıdaki adımları tamamlayın:
1. Paylaşılan yapılandırma dizinine gidin ve applicationHost.config dosyasını bulun.
2. Bu satırı yapılandırmanızı modülleri bölümüne ekleyin:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
## <a name="troubleshooting"></a>Sorun giderme
    
### <a name="troubleshooting-powershell"></a>PowerShell sorunlarını giderme

#### <a name="determine-which-modules-are-available"></a>Hangi modüllerin kullanılabilir olduğunu belirleyin
Kullanabileceğiniz `Get-Module -ListAvailable` hangi modüllerin yüklü olduğunu saptamak için komutu.

#### <a name="import-a-module-into-the-current-session"></a>Bir modülü geçerli oturuma içeri aktarın.
Bir PowerShell oturumuna bir modül yüklenmedi, el ile kullanarak yüklemeden `Import-Module <path to psd1>` komutu.


### <a name="troubleshooting-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülü sorunlarını giderme

#### <a name="list-the-commands-available-in-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülünde kullanılabilir komutları listesi
Komutunu çalıştırın `Get-Command -Module Az.ApplicationMonitor` kullanılabilir komutları almak için:

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.4.0      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.4.0      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Start-ApplicationInsightsMonitoringTrace           0.4.0      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülü geçerli sürümünü belirleme
Çalıştırma `Get-ApplicationInsightsMonitoringStatus` komut modülü hakkında aşağıdaki bilgileri görüntülemek için:
   - PowerShell modülü sürüm
   - Application Insights SDK sürümü
   - PowerShell modülünün dosya yolları
    
Gözden geçirme [API Başvurusu](status-monitor-v2-api-get-status.md) bu cmdlet'in nasıl kullanılacağı ayrıntılı bir açıklaması.


### <a name="troubleshooting-running-processes"></a>Çalışan işlemler sorunlarını giderme

Tüm DLL'ler yüklü olduğunda belirlemek için izleme eklenmiş bilgisayardaki işlemler inceleyebilirsiniz.
İzleme çalışıyorsa, en az 12 DLL'leri yüklenmesi gerekir.

Kullanım `Get-ApplicationInsightsMonitoringStatus -InspectProcess` DLL'leri denetlemek için komutu.

Gözden geçirme [API Başvurusu](status-monitor-v2-api-get-status.md) bu cmdlet'in nasıl kullanılacağı ayrıntılı bir açıklaması.


### <a name="collect-etw-logs-by-using-perfview"></a>PerfView kullanarak ETW günlük toplama

#### <a name="setup"></a>Kurulum

1. PerfView.exe ve gelen PerfView64.exe indirme [GitHub](https://github.com/Microsoft/perfview/releases).
2. PerfView64.exe başlatın.
3. Genişletin **Gelişmiş Seçenekler**.
4. Bu onay kutularını temizleyin:
    - **Zip**
    - **Birleştir**
    - **.NET sembol koleksiyonu**
5. Bu ayarla **ek sağlayıcılar**: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Günlükleri toplama

1. Yönetici ayrıcalıklarıyla bir komut konsolunda `iisreset /stop` IIS Aç komutunu ve tüm web uygulamaları.
2. PerfView içinde seçin **toplamaya Başla**.
3. Yönetici ayrıcalıklarıyla bir komut konsolunda `iisreset /start` IIS başlatmak için komutu.
4. Uygulamanız için göz atmayı deneyin.
5. Uygulama yüklendikten sonra PerfView ve seçin için iade **toplamasını Durdur**.



## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [API Başvurusu](status-monitor-v2-overview.md#powershell-api-reference) kaçırdığınıza parametreleri hakkında bilgi edinmek için.
- Burada listelenmeyen bir sorun arasında geliyorsa, bize üzerinde başvurabilirsiniz [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).
