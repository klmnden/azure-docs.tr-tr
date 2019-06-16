---
title: Azure tanılama uzantı sorunlarını giderme
description: Azure Tanılama'da Azure sanal makineler, Service Fabric ve Cloud Services'ı kullanarak sorunları giderin.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: robb
ms.openlocfilehash: 99ac4ffc288773e52183d371ef2c20f6153bc0f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65471776"
---
# <a name="azure-diagnostics-troubleshooting"></a>Azure tanılama sorunlarını giderme
Bu makalede, Azure Tanılama'yı kullanarak ilgili sorun giderme bilgileri açıklar. Azure Tanılama hakkında daha fazla bilgi için bkz: [Azure tanılama genel bakış](diagnostics-extension-overview.md).

## <a name="logical-components"></a>Mantıksal bileşenler
**Tanılama eklentisi Başlatıcısı (DiagnosticsPluginLauncher.exe)** : Azure tanılama uzantısını başlatır. Noktası işleminin giriş olarak görev yapar.

**Tanılama Eklentisi (DiagnosticsPlugin.exe)** : Yapılandırır, başlatır ve İzleme Aracısı'nın ömrü yönetir. Bu başlatıcı tarafından başlatılan ana işlemidir.

**İzleme Aracısı (MonAgent\*.exe işlemleri)** : İzler, toplar ve Tanılama verileri aktarır.  

## <a name="logartifact-paths"></a>Günlük/yapıt yolları
Bazı önemli günlükleri ve yapıtları yolları aşağıda verilmiştir. Belgenin geri kalanında bu bilgileri diyoruz.

### <a name="azure-cloud-services"></a>Azure bulut Hizmetleri
| Yapıt | `Path` |
| --- | --- |
| **Azure tanılama yapılandırma dosyası** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\Config.txt |
| **Günlük dosyaları** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\ |
| **Yerel depo için Tanılama verileri** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Tables |
| **Aracı yapılandırma dosyasını izleme** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Azure tanılama uzantı paketi** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version> |
| **Günlük toplama yardımcı program yolu** | %SystemDrive%\Packages\GuestAgent\ |
| **MonAgentHost günlük dosyası** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

### <a name="virtual-machines"></a>Sanal makineler
| Yapıt | `Path` |
| --- | --- |
| **Azure tanılama yapılandırma dosyası** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\RuntimeSettings |
| **Günlük dosyaları** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\ |
| **Yerel depo için Tanılama verileri** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Tables |
| **Aracı yapılandırma dosyasını izleme** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MaConfig.xml |
| **Durum dosyası** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Status |
| **Azure tanılama uzantı paketi** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>|
| **Günlük toplama yardımcı program yolu** | C:\WindowsAzure\Logs\WaAppAgent.log |
| **MonAgentHost günlük dosyası** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MonAgentHost.<seq_num>.log |

## <a name="metric-data-doesnt-appear-in-the-azure-portal"></a>Ölçüm verilerini Azure portalında görünmüyor
Azure Tanılama, Azure portalında görüntülenebilir ölçüm verileri sağlar. Portalı'nda verileri görme hakkındaki sorunları varsa, WADMetrics denetleyin\* tablosunda karşılık gelen bir ölçüm kayıt olup olmadığını görmek için Azure tanılama depolama hesabı.

Burada, **PartitionKey** , kaynak Kimliğini, sanal makine veya sanal makine ölçek kümesi tablosudur. **RowKey** ölçüm adı (performans sayacı adı olarak da bilinir).

Kaynak Kimliği yanlış ise, kontrol **tanılama** **yapılandırma** > **ölçümleri** > **ResourceId**kaynak kimliği doğru şekilde ayarlanıp ayarlanmadığını görmek için.

Özel ölçüm için veri yoksa denetleyin **tanılama Yapılandırması** > **PerformanceCounter** ölçüm (performans sayacı) dahil olup olmadığını görmek için. Biz aşağıdaki sayaçlar varsayılan olarak etkinleştir:
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET uygulamaları (__toplam__) \Requests/Sec
- \ASP.NET uygulamaları (__toplam__) \Errors toplam/sn
- \ASP.NET\Requests kuyruğa alındı
- \ASP.NET\Requests reddetti
- \Processor(W3wp)\% işlemci zamanı
- (Yalnızca w3wp) \Process \Private baytlar
- \Process(WaIISHost)\% işlemci zamanı
- \Process (WaIISHost) \Private baytlar
- \Process(WaWorkerHost)\% işlemci zamanı
- \Process (WaWorkerHost) \Private baytlar
- \Memory\Page Hatası/sn
- \.NET CLR bellek (_genel_)\% gc'de zaman
- \LogicalDisk (C:) \Disk Yazma Bayt/sn
- \LogicalDisk (C:) \Disk Okuma Bayt/sn
- \LogicalDisk (D:) \Disk Yazma Bayt/sn
- \LogicalDisk (D:) \Disk Okuma Bayt/sn

Yapılandırma doğru olarak ayarlanmış, ancak yine de ölçüm verilerinin göremiyorsanız, gidermenize yardımcı olması için aşağıdaki yönergeleri kullanın.


## <a name="azure-diagnostics-is-not-starting"></a>Azure tanılama başlamıyor
Azure tanılama başlatmak başarısız olma nedenine ilişkin daha fazla bilgi için bkz: **DiagnosticsPluginLauncher.log** ve **DiagnosticsPlugin.log** dosyaları daha önce sağlanan günlük dosyalarının konumu.

Bu günlükler gösteriyorsa `Monitoring Agent not reporting success after launch`, MonAgentHost.exe başlatılırken bir hata oluştu anlamına gelir. Günlükleri için belirtilen konumda bakın `MonAgentHost log file` önceki bölümde.

Günlük dosyalarının son satır çıkış kodu içerir.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Görürseniz bir **negatif** çıkış kodu, başvurmak [çıkış kodu tablosunu](#azure-diagnostics-plugin-exit-codes) içinde [başvurduğu bölüm](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Tanılama verileri Azure Depolama'ya günlüğe kaydedilmez
Verilerin hiçbiri görüntülenmesini veya bazı verilerin görüntülenmesini belirleyin.

### <a name="diagnostics-infrastructure-logs"></a>Tanılama Altyapısı günlükleri
Tanılama Tanılama Altyapısı günlükleri tüm hataları kaydeder. Etkinleştirdiğinizden emin olun [yakalama Tanılama Altyapısı günlükleri yapılandırmanızda](#how-to-check-diagnostics-extension-configuration). Görünen ilgili hataları hızlıca arayabilirsiniz sonra `DiagnosticInfrastructureLogsTable` yapılandırılmış depolama hesabınızda tablo.

### <a name="no-data-is-appearing"></a>Hiçbir veri görünmesini
Olay verilerini hiç görüntülenmezse en yaygın nedeni, depolama hesabı bilgileri hatalı tanımlanmış olmasıdır.

Çözüm: Tanılama yapılandırmanızı düzeltin ve tanılama yeniden yükleyin.

Depolama hesabının makinede doğru bir şekilde yapılandırıldığını, uzak erişim ve doğrulayın *DiagnosticsPlugin.exe* ve *MonAgentCore.exe* çalışıyor. Bunlar çalıştırmadığınız, adımları [Azure tanılama başlatılmıyor](#azure-diagnostics-is-not-starting).

İşlemler çalıştırıyorsanız, Git [yerel olarak yakalanan veri?](#is-data-getting-captured-locally) iletideki yönergeleri izleyin.

Bu sorunu çözmezse, deneyin:

1. Aracıyı kaldırma
2. Dizini C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics Kaldır
3. Aracıyı yeniden yükleyin


### <a name="part-of-the-data-is-missing"></a>Veri parçası eksik
Bazı veriler, ancak tüm alıyorsanız, veri koleksiyonu/aktarım işlem hattı düzgün şekilde ayarlandığını anlamına gelir. Sorunu daraltmak için burada alt bölümlere izleyin.

#### <a name="is-the-collection-configured"></a>Koleksiyon yapılandırılır?
Tanılama yapılandırması toplanacak veri belirli bir tür için yönergeler içerir. [Yapılandırmanızı inceleyin](#how-to-check-diagnostics-extension-configuration) yalnızca koleksiyon için yapılandırmış olduğunuz veri aradığınız olduğunu doğrulayın.

#### <a name="is-the-host-generating-data"></a>Konak veri oluşturuyor?
- **Performans sayaçları**: Perfmon aracını açın ve sayaç denetleyin.

- **İzleme günlükleri**:  Uzaktan erişim VM'ye ve bir TextWriterTraceListener uygulamanın yapılandırma dosyasına ekleyin.  Bkz: https://msdn.microsoft.com/library/sk36c28t.aspx metin dinleyiciyi ayarlamak için.  Emin `<trace>` öğesinin `<trace autoflush="true">`.<br />
Oluşturulan izleme günlükleri görmüyorsanız, eksik izleme günlükleri hakkında daha fazla.

- **ETW izlemelerini**: Uzaktan erişim VM ve PerfView yükleyin.  PerfView içinde çalıştırma **dosya** > **kullanıcı komutu** > **dinleme etwprovder1** > **etwprovider2**ve benzeri. **Dinleme** komutu büyük/küçük harfe ve ETW sağlayıcıları virgülle ayrılmış liste arasında boşluk olamaz. Çalıştırılacak komutu başarısız olursa, seçebileceğiniz **günlük** ne çalıştırmayı denedi ve hangi sonuç görmek için sağa Perfview aracının alt düğmesi.  Girişin doğru olduğunu varsayarsak, yeni bir pencere açılır. Birkaç saniye içinde ETW izlemelerini görmeye başlar.

- **Olay günlükleri**: VM ile uzak erişim. Açık `Event Viewer`ve ardından olaylar mevcut olduğundan emin olun.

#### <a name="is-data-getting-captured-locally"></a>Verileri yerel olarak yakalanır?
Ardından, verileri yerel olarak yakalanan emin olun.
Verileri yerel olarak depolanan `*.tsf` Tanılama verileri için yerel deposundaki dosyaları. Farklı türde günlükleri toplanan farklı `.tsf` dosyaları. Azure depolama tablo adlarına benzer adlarıdır.

Örneğin, `Performance Counters` içinde toplanan alma `PerformanceCountersTable.tsf`. Olay günlükleri toplanan `WindowsEventLogsTable.tsf`. Yönergeleri kullanın [yerel günlük ayıklama](#local-log-extraction) bölümü yerel koleksiyon dosyaları açmak ve bunları diskte toplanan gördüğünüzü doğrulayın.

Yerel olarak toplanan günlükleri görmüyor ve ana bilgisayar veri oluşturduğunu zaten doğruladıktan, büyük olasılıkla bir yapılandırma sorunu da sahip olursunuz. Yapılandırmanızı dikkatle gözden geçirin.

Ayrıca MonitoringAgent MaConfig.xml için oluşturulan yapılandırmayı gözden geçirin. İlgili günlük kaynağını tanımlayan bir bölüm olduğundan emin olun. Ardından, tanılama yapılandırması ve İzleme Aracısı yapılandırması arasındaki çevirisini kaybolmamasını doğrulayın.

#### <a name="is-data-getting-transferred"></a>Veri aktarılır?
Verileri yerel olarak yakalanır ama yine de, depolama hesabınızı görmüyorsanız doğruladıysanız, aşağıdaki adımları uygulayın:

- Doğru depolama hesabı sağladığınız ve belirli bir depolama hesabı için anahtarlar üzerinde toplama henüz doğrulayın. Azure bulut Hizmetleri için bazen kişiler güncelleştirmez görüyoruz `useDevelopmentStorage=true`.

- Belirtilen depolama hesabının doğru olduğundan emin olun. Bileşenleri, genel depolama uç noktaları ulaşmasını önleyen ağ kısıtlamaları olmayan emin olun. Bunu yapmanın bir yolu, makinede uzaktan erişim için ve aynı depolama hesabına kendiniz bir şeyler yazmak daha sonra deneyin.

- Son olarak, hangi hataları izleme aracı tarafından rapor edilir bakabilirsiniz. İzleme Aracısı, günlükler Yazar `maeventtable.tsf`, Tanılama verileri yerel deposunda bulunur. Bölümündeki yönergeleri [yerel günlük ayıklama](#local-log-extraction) bu dosyayı açmak için bölüm. Ardından olup olmadığını belirlemeye çalışın `errors` depolama alanına yazılmasını yerel dosyalar için okuma hataları gösterir.

### <a name="capturing-and-archiving-logs"></a>Yakalama ve günlüklerini arşivleme
Destek ile iletişim kurarak hakkında düşünmek, makinenizden günlükleri toplamak için gereken ilk şey, isteyebilirler olur. Bu kendiniz yaparak zamandan tasarruf edebilirsiniz. Çalıştırma `CollectGuestLogs.exe` günlük toplama yardımcı programı yolunda yardımcı programı. Bir .zip oluşturduğu tüm ilgili Azure dosyasıyla aynı klasörde günlüğe kaydeder.

## <a name="diagnostics-data-tables-not-found"></a>Tanılama veri tabloları bulunamadı
Aşağıdaki kodu kullanarak ETW olayları barındıran Azure depolama tablolarında adlandırılır:

```csharp
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
Bu kod, dört tablo oluşturur:

| Olay | Tablo adı |
| --- | --- |
| Sağlayıcı = "prov1" &lt;olay kimliği = "1" /&gt; |WADEvent + MD5("prov1") + "1" |
| provider=”prov1” &lt;Event id=”2” eventDestination=”dest1” /&gt; |WADdest1 |
| Sağlayıcı = "prov1" &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt; |WADdest2 |

## <a name="references"></a>Başvurular

### <a name="how-to-check-diagnostics-extension-configuration"></a>Tanılama uzantı yapılandırmasını denetleme
Gitmek için uzantı yapılandırmayı denetlemek için en kolay yolu olan [Azure kaynak Gezgini](http://resources.azure.com), ve ardından Git sanal makine veya Bulut hizmeti nerede Azure tanılama uzantısını (IaaSDiagnostics / PaaDiagnostics) olan.

Alternatif olarak, Uzak Masaüstü Bağlantısı makine ve günlük yapıtları yolu bölümünde açıklanan Azure tanılama yapılandırma dosyası bakın.

Her iki durumda da, arama **Microsoft.Azure.Diagnostics**ve ardından **xmlCfg** veya **WadCfg** alan.

Bir sanal makinede arama yapıyorsanız ve **WadCfg** alan varsa, bu yapılandırma, JSON biçiminde olduğu anlamına gelir. Varsa **xmlCfg** alan varsa, bu yapılandırma XML ve base64 olarak kodlanmış anlamına gelir. Şunları yapmanız [çözülmesi](https://www.bing.com/search?q=base64+decoder) tanılama tarafından yüklenen XML görmek için.

Diskten, yapılandırma çekme böylece, bulut hizmeti rolü için verileri base64 kodlamalı ise [çözülmesi](https://www.bing.com/search?q=base64+decoder) tanılama tarafından yüklenen XML görmek için.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure tanılama eklentisi çıkış kodları
Eklenti aşağıdaki çıkış kodlarını döndürür:

| Çıkış kodu | Açıklama |
| --- | --- |
| 0 |Başarılı. |
| -1 |Genel hata. |
| -2 |Rcf dosyası yüklenemiyor.<p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM üzerinde çağrılırsa, bu iç hata yalnızca gerçekleşmelidir. |
| -3 |Tanılama yapılandırma dosyası yüklenemiyor.<p><p>Çözüm: Şema doğrulaması geçmiyor bir yapılandırma dosyası tarafından neden oldu. Çözüm, şemasıyla uyumlu bir yapılandırma dosyası sağlamaktır. |
| -4 |Tanılama İzleme Aracısı'nın başka bir örneği zaten yerel kaynak dizini kullanıyor.<p><p>Çözüm: İçin farklı bir değer belirtin **LocalResourceDirectory**. |
| -6 |Geçersiz komut satırı ile tanılama başlatmak Konuk Aracısı eklentisi Başlatıcısı çalıştı.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM üzerinde çağrılırsa, bu iç hata yalnızca gerçekleşmelidir. |
| -10 |Tanılama eklentisi işlenmeyen bir özel durum ile çıkıldı. |
| -11 |Konuk Aracısı işlemi başlatma ve İzleme Aracısı'nı izleme sorumlu oluşturamadı.<p><p>Çözüm: Yeterli sistem kaynaklarına yeni işlemleri başlatmak kullanılabilir olduğunu doğrulayın.<p> |
| -101 |Tanılama eklentisi çağırırken geçersiz bağımsız değişkenler.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM üzerinde çağrılırsa, bu iç hata yalnızca gerçekleşmelidir. |
| -102 |Eklentisi olarak kendini başlatamadı işlemidir.<p><p>Çözüm: Yeterli sistem kaynaklarına yeni işlemleri başlatmak kullanılabilir olduğunu doğrulayın. |
| -103 |Eklentisi olarak kendini başlatamadı işlemidir. Özellikle, Günlükçü nesnesini oluşturamadı.<p><p>Çözüm: Yeterli sistem kaynaklarına yeni işlemleri başlatmak kullanılabilir olduğunu doğrulayın. |
| -104 |Konuk aracısı tarafından sağlanan rcf dosyası yüklenemiyor.<p><p>Konuk Aracısı eklentisi Başlatıcısı'nı el ile yanlış VM üzerinde çağrılırsa, bu iç hata yalnızca gerçekleşmelidir. |
| -105 |Tanılama eklentisi tanılama yapılandırma dosyası açılamıyor.<p><p>Bu bir iç hata, yalnızca tanılama eklentisini el ile yanlış VM üzerinde çağrılırsa gerçekleşmelidir. |
| -106 |Tanılama yapılandırma dosyası okunamıyor.<p><p>Şema doğrulaması geçmiyor bir yapılandırma dosyası tarafından neden oldu. <br><br>Çözüm: Şemayla uyumlu bir yapılandırma dosyası sağlayın. Daha fazla bilgi için [tanılama uzantı yapılandırmasını denetleme](#how-to-check-diagnostics-extension-configuration). |
| -107 |İzleme Aracısı kaynak directory geçişine geçersiz.<p><p>Bu bir iç hata, yalnızca izleme Aracısı'nı el ile yanlış VM üzerinde çağrılırsa gerçekleşmelidir.</p> |
| -108 |İzleme Aracısı yapılandırma dosyasına tanılama yapılandırma dosyası dönüştürülemedi.<p><p>Bu bir iç hata, yalnızca tanılama eklentisini el ile geçersiz bir yapılandırma dosyası ile çağrılırsa gerçekleşmelidir. |
| -110 |Genel tanılama yapılandırma hatası.<p><p>Bu bir iç hata, yalnızca tanılama eklentisini el ile geçersiz bir yapılandırma dosyası ile çağrılırsa gerçekleşmelidir. |
| -111 |İzleme Aracısı başlatılamıyor.<p><p>Çözüm: Yeterli sistem kaynaklarına kullanılabilir olduğunu doğrulayın. |
| -112 |Genel hata |

### <a name="local-log-extraction"></a>Yerel günlük ayıklama
İzleme Aracısı günlükleri ve yapıtları olarak toplayan `.tsf` dosyaları. `.tsf` Dosyası okunabilir değil ancak içine dönüştürebilirsiniz bir `.csv` şu şekilde:

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Yeni bir dosya olarak adlandırılan `<relevantLogFile>.csv` ilgili olarak aynı yolda oluşturulan `.tsf` dosya.

>[!NOTE]
> Bu yardımcı program ana .tsf dosyasını (örneğin, PerformanceCountersTable.tsf) karşı çalıştırmak yeterlidir. Eşlik eden dosyaları (örneğin, PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf vb.) otomatik olarak işlenir.

### <a name="more-about-missing-trace-logs"></a>İzleme günlükleri eksik hakkında daha fazla bilgi

>[!NOTE]
> Iaas sanal makinenizde çalışan bir uygulamada DiagnosticsMonitorTraceListener yapılandırmadığınız sürece aşağıdaki bilgiler çoğunlukla Azure bulut Hizmetleri için geçerlidir.

- Emin **DiagnosticMonitorTraceListener** web.config veya app.config yapılandırılır.  Bu, bulut hizmeti projelerinde varsayılan olarak yapılandırılır. Ancak, bazı müşteriler açıklama çıkış, altyapınıza değil toplanacak izleme deyimleri neden olur.

- Günlükleri gelen yazılmaz, **OnStart** veya **çalıştırmak** yöntemi emin **DiagnosticMonitorTraceListener** app.config dosyasında olduğu.  Varsayılan olarak, web.config dosyasında olmakla birlikte, yalnızca içinde w3wp.exe çalışan kod için geçerlidir. Bu nedenle, WaIISHost.exe içinde çalışmakta olan izlemeleri yakalamak amacıyla app.config dosyasındaki gerekir.

- Kullandığınızdan emin olun **Diagnostics.Trace.TraceXXX** yerine **Diagnostics.Debug.WriteXXX.** Hata ayıklama bildirimlerini yayın derleme kaldırılır.

- Aslında derlenmiş kod olduğundan emin olun **Diagnostics.Trace satırları** (doğrulamak için Reflector, ildasm veya yetenek kullanın). **Diagnostics.Trace** izleme koşullu derleme sembol kullanmadığınız sürece, komutları derlenmiş ikili dosyadan kaldırılır. Bu bir projeyi derlemek için msbuild kullanırken oluşan sık karşılaşılan bir sorundur.   

## <a name="known-issues-and-mitigations"></a>Bilinen sorunlar ve riskleri azaltma
Bilinen bir risk azaltma işlemleri ile ilgili bilinen sorunların bir listesi aşağıda verilmiştir:

**1. .NET 4.5 bağımlılık**

Windows Azure tanılama uzantısını framework .NET 4.5 veya sonraki çalışma zamanı bağımlılık vardır. Azure bulut Hizmetleri için sağlanan tüm makineler yanı sıra, Azure sanal makinelerinde temel alan tüm resmi görüntüleri yazma zamanında .NET 4.5 veya üzeri yüklü.

Bu işinize karşılaştığınız .NET 4.5 veya üzeri yüklü olmayan bir makineye Windows Azure tanılama uzantısını çalıştırmayı denediğinizde olduğu bir durumda olur. Bu, makinenize bir eski görüntü veya anlık görüntüsünü oluşturduğunuzda veya kendi özel disk getirdiğinizde gerçekleşir.

Bu genellikle bir çıkış kodu bildirimlerini **255** çalıştırırken **DiagnosticsPluginLauncher.exe.** Aşağıdaki işlenmeyen özel durum nedeniyle başarısız olur:
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Azaltma:** Makinenize .NET 4.5 veya sonraki bir sürümü yükleyin.

**2. Performans sayaçlarını veri depolama, ancak portalda gösterilmiyor kullanılabilir**

Sanal makineler'de portal deneyimi, varsayılan olarak belirli performans sayaçlarını gösterir. Performans sayaçlarını görmüyor ve depolama alanında kullanılabilir olmadığından veri oluşturulduğunu bildiğiniz, aşağıdakileri denetleyin:

- Depolama verileri sayacı adlarını İngilizce dilinde olup olmadığı. Sayaç adları İngilizce değilse, portal ölçüm grafiğini tanıması mümkün olmaz. **Risk azaltma**: Makinenin dili İngilizce'ye sistem hesapları için değiştirin. Bunu yapmak için **Denetim Masası** > **bölge** > **Yönetim** > **kopya ayarlarını**. Ardından, seçimini **Hoş Geldiniz ekranı ve sistem hesapları** böylece özel dil sistem hesabı için uygulanmaz.

- Joker karakterler kullanılıyorsa (\*) performans sayacı adlarını, portal için Azure depolama havuzu performans sayaçlarını gönderildiğinde yapılandırılabilir ve toplanan sayacı ilişkilendirmek mümkün olmaz. **Risk azaltma**: Joker karakter ve genişletin portalı emin olmak için (\*), performans Sayaçlarınızı rota ["Azure İzleyici" havuz](diagnostics-extension-schema.md#diagnostics-extension-111).

