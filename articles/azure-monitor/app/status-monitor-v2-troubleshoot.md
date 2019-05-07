---
title: Azure Durum İzleyicisi v2 giderme ve bilinen sorunlar | Microsoft Docs
description: Durum İzleyicisi v2 ve örnekler sorun giderme bilinen sorunlar. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
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
ms.openlocfilehash: f8e057bf2e71d5a00c1e2a8ef72a5c9415a5a207
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145115"
---
# <a name="troubleshooting-status-monitor-v2"></a>Sorun giderme durumu izleme v2

İzleme'yi etkinleştirdikten sonra veri toplamayı kapatmasına sorunlarla karşılaşabilirsiniz. Bu belge, tüm bilinen sorunlar ve sorun giderme örnekleri listeler.
Burada listelenmeyen bir sorun arasında geliyorsa, bize başvurabilirsiniz [burada](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="conflicting-dlls-in-an-applications-bin-directory"></a>Çakışan bir uygulamasının bin dizinine DLL'lerde

Bu DLL'leri hiçbirini bin dizininde mevcut olması durumunda, izleme başarısız olabilir.

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Uygulamanızı bunları kullanmıyor olsa bile bu DLL'leri bazıları Visual Studio'nun varsayılan uygulama şablonlarında dahil edilir.
Sorun giderme araçları kullanarak belirtisi davranışı görülebilir:

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

- iisreset + uygulama (Hayır TELEMETRİ) yükleyin. Sysinternals (Handle.exe ve ListDLLs.exe) araştırın
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>IIS paylaşılan yapılandırması ile çakışma

Web sunucularının bir kümesi varsa kullanabilecek bir [paylaşılan yapılandırma](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211). Bizim HttpModule otomatik olarak bu paylaşılan yapılandırma eklenemiyor. Her web sunucusu, önce kendi GAC içine bizim DLL yüklemek için etkinleştir komutu çalıştırmanız gerekir.

Enable komutunu çalıştırdıktan sonra 
1. Paylaşılan yapılandırma dizininize Gözat ve Bul, `applicationHost.config` dosya.
2. Modules bölümü yapılandırmanızda bu satırı ekleyin:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
    


## <a name="troubleshooting"></a>Sorun giderme
    
### <a name="troubleshooting-powershell"></a>PowerShell sorunlarını giderme

#### <a name="how-to-inspect-what-modules-are-available"></a>Hangi modüllerin incelemek nasıl kullanılabilir?
Yüklü modülleri komutunu kullanarak denetleyebilirsiniz: `Get-Module -ListAvailable`

#### <a name="how-to-import-a-module-into-the-current-session"></a>Bir modülü geçerli oturuma içeri aktarmak nasıl?
Komutunu kullanarak bir PowerShell oturumuna Modül yüklenmedi, el ile yükleyebilirsiniz: `Import-Module <path to psd1>`


### <a name="troubleshooting-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülü sorunlarını giderme

#### <a name="how-to-review-what-commands-are-available-in-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülünde kullanılabilen hangi komut gözden geçirmek nasıl?
- Komutu çalıştırın: `Get-Command -Module Az.ApplicationMonitor` kullanılabilir komutları almak için:

    ```
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
    Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
    Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    ```

#### <a name="what-is-the-current-version-of-the-status-monitor-v2-module"></a>Durum İzleyicisi v2 modülü geçerli sürümü nedir?
- Komutu çalıştırın: `Get-ApplicationInsightsMonitoringStatus` bu modülle ilgili bilgileri bir çıktısını almak için:

    ```
    PowerShell Module version:
    0.2.1-alpha

    Application Insights SDK version:
    2.9.0.3872

    Executing PowerShell Module Assembly:
    Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.9129, Culture=neutral, PublicKeyToken=31bf3856ad364e35

    PowerShell Module Directory:
    C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\PowerShell

    Runtime Paths:
    ParentDirectory: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content Exists: True
    ConfigurationPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\applicationInsights.ikey.config Exists: False
    ManagedHttpModuleHelperPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll Exists: True
    RedfieldIISModulePath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll Exists: True
    InstrumentationEngine86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll Exists: True
    InstrumentationEngine64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll Exists: True
    InstrumentationEngineExtensionHost86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll Exists: True
    InstrumentationEngineExtensionHost64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll Exists: True
    InstrumentationEngineExtensionConfig86Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config Exists: True
    InstrumentationEngineExtensionConfig64Path: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config Exists: True
    ApplicationInsightsSdkPath: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.1\content\Runtime\Microsoft.ApplicationInsights.dll Exists: True
    ```




### <a name="troubleshooting-running-processes"></a>Çalışan işlemler sorunlarını giderme

İşlem izleme eklenmiş makinedeki tüm DLL'ler yüklü olmadığını inceleyebilirsiniz.
İzleme çalışıyorsa, en az 12 DLL'leri yüklenmesi gerekir.

- Cmd: `Get-ApplicationInsightsMonitoringStatus -InspectProcess`

    ```
    iisreset.exe /status
    Status for IIS Admin Service ( IISADMIN ) : Running
    Status for Windows Process Activation Service ( WAS ) : Running
    Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
    Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
    Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
    Status for World Wide Web Publishing Service ( W3SVC ) : Running

    handle64.exe -accepteula -p w3wp
      BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
      C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
      C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
      C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
      C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
      CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
      DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
      B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
      BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
      BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
      BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

    listdlls64.exe -accepteula w3wp
    0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
    ```

### <a name="collect-etw-logs-with-perfview"></a>PerfView ile ETW günlükleri toplama

#### <a name="setup"></a>Kurulum

1. PerfView.exe ve gelen PerfView64.exe indirin https://github.com/Microsoft/perfview/releases
2. PerfView64.exe başlatın
3. "Gelişmiş Seçenekleri" genişletin
4. Seçeneğinin işaretini kaldırın:
    - Zip
    - Birleştir
    - .NET sembol koleksiyonu
5. Ek sağlayıcılar ayarlayın: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Günlükler toplanıyor

1. Yönetici ayrıcalıklarıyla bir komut konsolunda yürütün `iisreset /stop` tüm web uygulamaları ile IIS açıp kapatmak için.
2. PerfView içinde "Koleksiyonu Başlat"'a tıklayın.
3. Yönetici ayrıcalıklarıyla bir komut konsolunda yürütün `iisreset /start` IIS başlatmak için.
4. Uygulamanız için göz atmayı deneyin.
5. Uygulamanızı yükleme tamamlandıktan sonra PerfView için dönüş ve "Toplamasını Durdur" tıklayın



## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme bizim [API Başvurusu](status-monitor-v2-overview.md#powershell-api-reference) kaçırdığınıza bir parametre bulunamadı.
- Burada listelenmeyen bir sorun arasında geliyorsa, bize başvurabilirsiniz [burada](https://github.com/Microsoft/ApplicationInsights-Home/issues).
