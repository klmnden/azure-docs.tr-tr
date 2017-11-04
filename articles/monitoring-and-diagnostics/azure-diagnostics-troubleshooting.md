---
title: "Azure tanılama sorunlarını giderme | Microsoft Docs"
description: "Azure Virtual Machines, Service Fabric veya Bulut Hizmetleri Azure tanılama kullanırken sorunlarını giderin."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.openlocfilehash: b03265b52886b30e4b9de0b0293e5dadd6d2413a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure tanılama sorunlarını giderme
Bu makalede Azure Tanılama'yı kullanarak için ilgili sorun giderme bilgileri açıklar. Azure Tanılama hakkında daha fazla bilgi için bkz: [Azure tanılama genel bakış](azure-diagnostics.md).

## <a name="logical-components"></a>Mantıksal bileşenleri
**Tanılama eklentisi Başlatıcısı (DiagnosticsPluginLauncher.exe)**: Azure tanılama uzantısını başlatır. Giriş gören işlem gelin.

**Tanılama Eklentisi (DiagnosticsPlugin.exe)**: yapılandırır, başlatır ve İzleme Aracısı'nın ömrü yönetir. Bu başlatıcı tarafından başlatılan ana işlemidir.

**İzleme aracısını (MonAgent\*.exe işlemler)**: izleyiciler, toplar ve tanılama veri aktarır.  

## <a name="logartifact-paths"></a>Günlük/yapı yolları
Bazı önemli günlükleri ve yapıları yollara aşağıda verilmiştir. Biz, belgenin geri kalanında bu bilgilere bakın.

### <a name="azure-cloud-services"></a>Azure Cloud Services
| Yapı | Yol |
| --- | --- |
| **Azure tanılama yapılandırma dosyası** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürüm > \Config.txt |
| **Günlük dosyaları** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürüm > \ |
| **Tanılama verileri için yerel depolama** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< Rol adı >. DiagnosticStore\WAD0107\Tables |
| **İzleme Aracısı Yapılandırma dosyası** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< Rol adı >. DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Azure tanılama uzantısını paketi** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<sürüm > |
| **Günlük toplama yardımcı programı yolu** | %SystemDrive%\Packages\GuestAgent\ |
| **MonAgentHost günlük dosyası** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< Rol adı >. DiagnosticStore\WAD0107\Configuration\MonAgentHost. < seq_num > .log |

### <a name="virtual-machines"></a>Sanal makineler
| Yapı | Yol |
| --- | --- |
| **Azure tanılama yapılandırma dosyası** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<sürüm > \RuntimeSettings |
| **Günlük dosyaları** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<sürüm > \Logs\ |
| **Tanılama verileri için yerel depolama** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Tables |
| **İzleme Aracısı Yapılandırma dosyası** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MaConfig.xml |
| **Durum dosyası** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<sürüm > \Status |
| **Azure tanılama uzantısını paketi** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion >|
| **Günlük toplama yardımcı programı yolu** | C:\WindowsAzure\Packages |
| **MonAgentHost günlük dosyası** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MonAgentHost. < seq_num > .log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Ölçüm verileri Azure Portalı'nda görünmüyor
Azure tanılama Azure portalında görüntülenen ölçüm verileri sağlar. Verileri portalında görmesini sorunlarınız varsa, WADMetrics denetleyin\* tablosunda karşılık gelen ölçüm kayıt olup olmadığını görmek için Azure tanılama depolama hesabı. 

Burada, **PartitionKey** kaynak kimliği, sanal makine veya sanal makine ölçek kümesi tablosu verilmiştir. **RowKey** ölçüm adı (performans sayacı adı olarak da bilinir).

Kaynak Kimliği yanlış ise, kontrol edin **tanılama** **yapılandırma** > **ölçümleri** > **ResourceId**kaynak kimliği doğru olarak ayarlanmış olup olmadığını görmek için.

Belirli ölçüm için hiçbir veri ise denetleyin **tanılama Yapılandırması** > **PerformanceCounter** ölçüm (performans sayacı) dahil olup olmadığını görmek için. Varsayılan olarak aşağıdaki sayaçları etkinleştirme:
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET uygulamaları (__toplam__) \Requests/Sec
- \ASP.NET uygulamaları (__toplam__) \Errors toplam/sn
- \ASP.NET\Requests sıraya alındı
- \ASP.NET\Requests reddetti
- \Processor(W3wp)\% işlemci zamanı
- \Process (w3wp) \Private bayt
- \Process(WaIISHost)\% işlemci zamanı
- \Process (WaIISHost) \Private bayt
- \Process(WaWorkerHost)\% işlemci zamanı
- \Process (WaWorkerHost) \Private bayt
- \Memory\Page Hatası/sn
- \.NET CLR bellek (_genel_)\% de harcanan % zaman
- \LogicalDisk (C:) \Disk Yazma Bayt/sn
- \LogicalDisk (C:) \Disk Okuma Bayt/sn
- \LogicalDisk (D:) \Disk Yazma Bayt/sn
- \LogicalDisk (D:) \Disk Okuma Bayt/sn

Yapılandırma doğru olarak ayarlanmış, ancak yine de ölçüm verilerinin göremiyorsanız, gidermenize yardımcı olması için aşağıdaki kılavuzları kullanın.


## <a name="azure-diagnostics-isnt-starting"></a>Azure tanılama başlatılıyor değil
Neden Azure tanılama başlatılamadı hakkında daha fazla bilgi için bkz: **DiagnosticsPluginLauncher.log** ve **DiagnosticsPlugin.log** daha önce sağlanan günlük dosyalarını konumun dosyalar. 

Bu günlükler gösteriyorsa `Monitoring Agent not reporting success after launch`, MonAgentHost.exe başlatma bir hata oluştu anlamına gelir. İçin belirtilen konum günlüklerine bakın `MonAgentHost log file` önceki bölümdeki.

Günlük dosyaları son satırının çıkış kodu içerir.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Görürseniz bir **negatif** çıkış kodu, başvurmak [çıkış kodu tablosu](#azure-diagnostics-plugin-exit-codes) içinde [başvuran bölüm](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Tanılama verileri Azure depolama birimine günlüğe kaydedilmez
Verilerin hiçbiri görüntülenmesini veya bazı verilerin görüntülenmesini belirler.

### <a name="diagnostics-infrastructure-logs"></a>Tanılama altyapı günlükleri
Tanılama tüm hataları tanılama altyapı günlüklerine kaydedilir. Etkinleştirdiğinizden emin olun [yakalama tanılama altyapısının günlüklerini yapılandırmanızda](#how-to-check-diagnostics-extension-configuration). Hızlı bir şekilde görünen ilgili hatalar için bakabilirsiniz sonra `DiagnosticInfrastructureLogsTable` yapılandırılan depolama hesabınızdaki tablo.

### <a name="no-data-is-appearing"></a>Hiçbir veri görünmesini
Olay verileri hiç görünmüyor en yaygın nedeni, depolama hesabı bilgilerini hatalı tanımlanmış olmasıdır.

Çözüm: Tanılama yapılandırmanızı düzeltin ve tanılama yeniden yükleyin.

Depolama hesabı makinede doğru şekilde yapılandırıldığından, uzaktan erişim ve DiagnosticsPlugin.exe ve MonAgentCore.exe çalıştığını doğrulayın. Çalışan değil, adımları [Azure tanılama başlamıyor](#azure-diagnostics-is-not-starting). 

İşlemler çalıştırıyorsanız, Git [yerel olarak yakalanan veri?](#is-data-getting-captured-locally) ve yönergeleri izleyin.

### <a name="part-of-the-data-is-missing"></a>Verilerin bir kısmını eksik
Bazı veriler, ancak tüm alıyorsanız, veri toplama/aktarım ardışık doğru ayarlandığından emin anlamına gelir. Sorunu belirlemelerine daraltmak için alt bölümleri burada izleyin.

#### <a name="is-the-collection-configured"></a>Koleksiyon yapılandırılır? 
Tanılama yapılandırması toplanacak bir veri türüne ilişkin yönergeler içermektedir. [Yapılandırmanızı gözden](#how-to-check-diagnostics-extension-configuration) yalnızca, koleksiyon için yapılandırdığınız veri aradığınız olduğunu doğrulayın.

#### <a name="is-the-host-generating-data"></a>Ana bilgisayar veri üretiyor?
- **Performans sayaçları**: perfmon açın ve sayaç denetleyin.

- **İzleme günlükleri**: uzaktan erişim VM'de oturum ve bir olmalıdır uygulamanın yapılandırma dosyasına ekleyin.  Metin dinleyicisi Kur http://msdn.microsoft.com/library/sk36c28t.aspx bakın.  Emin olun `<trace>` öğeye sahip `<trace autoflush="true">`.<br />
Oluşturulan izleme günlükleri görmüyorsanız bkz [eksik izleme günlükleri hakkında daha fazla](#more-about-trace-logs-missing).

- **ETW izlemeleri**: uzaktan erişim yükleme PerfView ve VM.  PerfView içinde çalıştırmak **dosya** > **kullanıcı komutu** > **dinleme etwprovder1** > **etwprovider2**ve benzeri. **Dinleme** komutu büyük küçük harfe duyarlı ve ETW sağlayıcılar virgülle ayrılmış listesi arasında boşluk olamaz. Çalıştırmak komutu başarısız olursa, seçebileceğiniz **günlük** düğmesi sayfanın sağ Perfview aracının ne çalıştırmayı denedi ve hangi sonuç bakın.  Girişin doğru olduğunu varsayarak, yeni bir pencere açılır. Birkaç saniye içinde ETW izlemeleri görmesini başlayın.

- **Olay günlükleri**: uzaktan erişim VM'de oturum. Açık `Event Viewer`ve ardından olayları var olduğundan emin olun.

#### <a name="is-data-getting-captured-locally"></a>Verileri yerel olarak yakalanan?
Ardından, verileri yerel olarak yakalanır emin olun.
Verileri yerel olarak depolanan `*.tsf` dosyalar [Tanılama verileri için yerel deposu](#log-artifacts-path). Farklı türde günlüklerini toplanan farklı `.tsf` dosyaları. Adları, Azure storage'da tablo adları benzerdir. 

Örneğin, `Performance Counters` içinde toplanan almak `PerformanceCountersTable.tsf`. Olay günlüklerini toplanan `WindowsEventLogsTable.tsf`. Konusundaki yönergeleri kullanın [yerel günlük ayıklama](#local-log-extraction) yerel koleksiyon dosyaları açmak ve bunları diskte toplanan görebildiğini doğrulamak için bölüm.

Yerel olarak toplanan günlüklerini görmüyorum ve ana bilgisayar veri oluşturuyor zaten doğruladıktan büyük olasılıkla bir yapılandırma sorunu varsa. Yapılandırmanızı dikkatle gözden geçirin. 

Ayrıca MonitoringAgent için oluşturulan yapılandırmasını gözden geçirin. [MaConfig.xml](#log-artifacts-path). İlgili günlük kaynak açıklayan bir bölümü olduğundan emin olun. Ardından, tanılama yapılandırma ve İzleme Aracısı yapılandırması arasında çevirisini kaybolmamasını doğrulayın.

#### <a name="is-data-getting-transferred"></a>Veri aktarılır?
Verileri yerel olarak yakalanır ancak onu hala depolama hesabınızdaki göremiyorsanız doğruladıysanız, aşağıdaki adımları uygulayın: 

- Doğru depolama hesabı sağladığınız ve belirli depolama hesabı için anahtarlar üzerinden alındı henüz doğrulayın. Azure bulut Hizmetleri için bazen kimliğinizi kişiler güncelleştirmemeniz bkz `useDevelopmentStorage=true`.

- Belirtilen depolama hesabının doğru olduğundan emin olun. Bileşenleri Genel depolama uç noktaları ulaşmasını engellemeniz ağ kısıtlamalarını yok emin olun. Bunu yapmanın bir yolu makinede uzaktan erişim için ve bir şey için aynı depolama hesabındaki kendiniz yazmak deneyin.

- Son olarak, hangi hataları izleme aracısı tarafından rapor edilen olup adresindeki bakabilirsiniz. İzleme Aracısı günlüklerinin Yazar `maeventtable.tsf`, bulunan [Tanılama verileri için yerel deposu](#log-artifacts-path). ' Ndaki yönergeleri izleyin [yerel günlük ayıklama](#local-log-extraction) bu dosyayı açmak için bölüm. Olup olmadığını belirlemek deneyin `errors` depolama alanına yazılmasını yerel dosyalar için okuma hataları gösterir.

### <a name="capturing-and-archiving-logs"></a>Yakalama ve günlükleri arşivleme
Destek uzmanıyla hakkında düşünüyorum, makinenizden günlükleri toplamak için isteyebilir ilk şey olur. Bu kendiniz yaparak zamandan tasarruf edebilirsiniz. Çalıştırma `CollectGuestLogs.exe` adresindeki yardımcı programı [günlük toplama yardımcı programı yolu](#log-artifacts-path). Bir .zip oluşturur tüm ilgili Azure dosyasıyla aynı klasöre kaydeder.

## <a name="diagnostics-data-tables-not-found"></a>Tanılama veri tabloları bulunamadı
ETW olayları tutmak Azure depolama tablolarda, aşağıdaki kodu kullanarak adlandırılır:

```C#
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Örnek aşağıda verilmiştir:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Bu kod, dört tablonun oluşturur:

| Olay | Tablo adı |
| --- | --- |
| Sağlayıcı "prov1" = &lt;olay kimliği = "1" /&gt; |WADEvent + MD5("prov1") + "1" |
| Sağlayıcı "prov1" = &lt;olay kimliği "2" eventDestination = "dest1" = /&gt; |WADdest1 |
| Sağlayıcı "prov1" = &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| Sağlayıcı "prov2" = &lt;DefaultEvents eventDestination "dest2" = /&gt; |WADdest2 |

## <a name="references"></a>Başvurular

### <a name="how-to-check-diagnostics-extension-configuration"></a>Tanılama uzantı yapılandırmasını denetleme
Gitmek için uzantı yapılandırmanızı denetleyin yapmanın en kolay yolu olan [Azure kaynak Gezgini](http://resources.azure.com), ve sanal makine veya Bulut gidin nerede hizmet Azure tanılama uzantısını (IaaSDiagnostics / PaaDiagnostics) değil.

Alternatif olarak, makine ve açıklanan Azure tanılama yapılandırma dosyası bakmak, Uzak Masaüstü'nü [oturum yapıları yol bölümü](#log-artifacts-path).

Her iki durumda da, arama **Microsoft.Azure.Diagnostics**ve ardından **xmlCfg** veya **WadCfg** alan. 

Bir sanal makinede arıyorsanız ve **WadCfg** alan varsa, bu yapılandırma JSON biçiminde olduğu anlamına gelir. Varsa **xmlCfg** alan varsa, bu yapılandırma XML ve base64 ile kodlanmış anlamına gelir. Yapmanız [çözülmesi](http://www.bing.com/search?q=base64+decoder) altyapınıza yüklendi XML görmek için.

Disk, yapılandırmasından seçerseniz, gereken şekilde bulut hizmet rolü için verileri Base64 ile kodlanmış [çözülmesi](http://www.bing.com/search?q=base64+decoder) altyapınıza yüklendi XML görmek için.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure tanılama eklentisi çıkış kodları
Eklentisi aşağıdaki çıkış kodlarını döndürür:

| Çıkış kodu | Açıklama |
| --- | --- |
| 0 |Başarılı. |
| -1 |Genel hata. |
| -2 |Rcf dosyası yüklenemiyor.<p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -3 |Tanılama yapılandırma dosyası yüklenemiyor.<p><p>Çözüm: nedeni, şema doğrulaması geçirme olmayan bir yapılandırma dosyası. Çözüm, şemasıyla uyumlu bir yapılandırma dosyası sağlamaktır. |
| -4 |Tanılama İzleme Aracısı başka bir örneği zaten yerel kaynak dizini kullanıyor.<p><p>Çözüm: için farklı bir değer belirtin **LocalResourceDirectory**. |
| -6 |Konuk Aracısı eklentisi Başlatıcısı geçersiz bir komut satırıyla tanılama başlatma girişiminde bulunuldu.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -10 |Tanılama eklentisi işlenmeyen bir özel durum ile çıkıldı. |
| -11 |Konuk Aracısı işlemi başlatma ve İzleme Aracısı'nı izleme sorumlu oluşturamadı.<p><p>Çözüm: yeterli sistem kaynaklarının yeni işlemleri başlatmak kullanılabilir olduğundan emin olun.<p> |
| -101 |Tanılama eklentisi çağrılırken geçersiz bağımsız değişkenler.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -102 |Eklentisi olarak kendisi başlatamadı işlemidir.<p><p>Çözüm: yeterli sistem kaynaklarının yeni işlemleri başlatmak kullanılabilir olduğundan emin olun. |
| -103 |Eklentisi olarak kendisi başlatamadı işlemidir. Özellikle, Günlükçü nesnesini oluşturamadı.<p><p>Çözüm: yeterli sistem kaynaklarının yeni işlemleri başlatmak kullanılabilir olduğundan emin olun. |
| -104 |Konuk aracısı tarafından sağlanan rcf dosyası yüklenemiyor.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -105 |Tanılama eklentisi tanılama yapılandırma dosyası açılamıyor.<p><p>Tanılama eklentisini el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -106 |Tanılama yapılandırma dosyası okunamıyor.<p><p>Bunun nedeni, şema doğrulaması geçirme değil bir yapılandırma dosyası. <br><br>Çözüm: şemasıyla uyumlu bir yapılandırma dosyası sağlar. Daha fazla bilgi için bkz: [tanılama uzantı yapılandırmasını denetleme](#how-to-check-diagnostics-extension-configuration). |
| -107 |İzleme Aracısı kaynak directory geçişine geçersiz.<p><p>İzleme Aracısı'nı el ile yanlış VM çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir.</p> |
| -108 |İzleme Aracısı yapılandırma dosyasına tanılama yapılandırma dosyası dönüştürülemiyor.<p><p>Tanılama eklentisini el ile geçersiz bir yapılandırma dosyası ile çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -110 |Genel tanılama yapılandırma hatası.<p><p>Tanılama eklentisini el ile geçersiz bir yapılandırma dosyası ile çağrılırsa, bu dahili bir hata yalnızca gerçekleşmelidir. |
| -111 |İzleme Aracısı başlatılamadı.<p><p>Çözüm: yeterli sistem kaynaklarının kullanılabilir olduğundan emin olun. |
| -112 |Genel hata |

### <a name="local-log-extraction"></a>Yerel günlük ayıklama
İzleme Aracısı günlükleri ve yapı olarak toplayan `.tsf` dosyaları. `.tsf` Dosyası okunabilir değil ancak içine dönüştürebilirsiniz bir `.csv` gibi: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Yeni bir dosya olarak adlandırılan `<relevantLogFile>.csv` ilgili olarak aynı yolundaki oluşturulan `.tsf` dosyası.

>[!NOTE] 
> Yalnızca bu yardımcı programı ana .tsf dosyaya (örneğin, PerformanceCountersTable.tsf) karşı çalıştırmanız gerekir. Eşlik eden dosyaları (örneğin, PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf vb.) otomatik olarak işlenir.

### <a name="more-about-missing-trace-logs"></a>İzleme günlükleri eksik hakkında daha fazla bilgi 

>[!NOTE]
> Iaas VM üzerinde çalışan bir uygulama üzerinde DiagnosticsMonitorTraceListener yapılandırmadığınız sürece aşağıdaki bilgileri çoğunlukla Azure bulut Hizmetleri için geçerlidir. 

- Emin olun **DiagnosticMonitorTraceListener** web.config veya app.config yapılandırılır.  Bu, bulut hizmeti projelerinde varsayılan olarak yapılandırılır. Ancak, bazı müşterilerin açıklama altyapınıza değil toplanacak izleme deyimleri neden giden. 

- Günlükleri gelen yazılmaz durumunda **OnStart** veya **çalıştırmak** yöntemi, emin olun **DiagnosticMonitorTraceListener** app.config dosyasında olduğu.  Web.config dosyasında varsayılandır tarafından ancak, yalnızca içinde w3wp.exe çalışan kodu uygular. Bu nedenle, WaIISHost.exe içinde çalışan izlemelerini yakalama App.Config'de gerekir.

- Kullandığınızdan emin olun **Diagnostics.Trace.TraceXXX** yerine **Diagnostics.Debug.WriteXXX.** Hata ayıklama bildirimlerini yayın derleme kaldırılır.

- Gerçekte derlenmiş kod olduğundan emin olun **Diagnostics.Trace satırları** (doğrulamak için Reflector, ildasm veya ILSpy kullanın). **Diagnostics.Trace** izleme koşullu derleme simgenin kullanmadığınız sürece, komutları derlenmiş ikili dosyadan kaldırılır. Bu proje oluşturmak için msbuild kullanırken oluşan sık karşılaşılan bir sorundur.   

## <a name="known-issues-and-mitigations"></a>Bilinen sorunlar ve bunları azaltmanın yollarını
Bilinen Azaltıcı Etkenler ile ilgili bilinen sorunların listesi aşağıdadır:

**1. .NET 4.5 bağımlılık**

Windows Azure tanılama uzantısını .NET 4.5 framework veya sonraki sürümlerde çalışma zamanı bağımlılık vardır. Azure bulut Hizmetleri için sağlanan tüm makineler yanı sıra, Azure sanal makinelere göre tüm resmi görüntüleri yazma zamanında .NET 4.5 veya sonrası yüklü. 

Bu, hala olası karşılaştığınız .NET 4.5 veya üstü yüklü olmayan bir makinede Windows Azure tanılama uzantısını çalıştırmayı denediğinizde burada bir durumda olur. Bu, makinenize bir eski görüntü veya anlık görüntü oluşturduğunuzda ya da kendi özel disk getirdiğinizde gerçekleşir.

Bu genellikle bir çıkış kodu olarak bildirimleri **255** çalıştırırken **DiagnosticsPluginLauncher.exe.** Aşağıdaki işlenmeyen özel durum nedeniyle başarısız olur: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Azaltma:** makinenizde .NET 4.5 veya sonraki bir sürümü yükleyin.

**2. Performans sayaçları veriler depolama ancak portalında gösterilmiyor de kullanılabilir**

Sanal makinelerdeki portal deneyimi, varsayılan olarak belirli performans sayaçlarını gösterir. Performans sayaçlarını görmüyorum ve depolama alanında kullanılabilir olmadığından veri oluştuğunu biliyorsanız aşağıdakileri denetleyin:

- Depolama verileri sayaç adları İngilizce olup olmadığı. Sayaç adları İngilizce değilse, portal ölçüm grafik tanıması mümkün olmaz.

- Joker karakterler kullanıyorsanız (\*), performans sayacı adlarını portal yapılandırılmış ve toplanan sayaç ilişkilendirmek mümkün olmaz.

**Azaltma**: makinenin dil İngilizce'ye sistem hesapları için değiştirin. Bunu yapmak için seçin **Denetim Masası** > **bölge** > **Yönetim** > **kopya ayarlarını**. Ardından, seçimini **Hoş Geldiniz ekranı ve sistem hesapları** böylece özel dil sistem hesabına uygulanmıyor. Ayrıca birincil tüketim deneyiminizi olmasını portal istiyorsanız joker karakterler kullanmayın emin olun.
