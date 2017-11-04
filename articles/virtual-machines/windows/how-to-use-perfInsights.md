---
title: Microsoft Azure'da PerfInsights kullanma | Microsoft Docs
description: "Windows VM performans sorunlarını gidermek için PerfInsights kullanmayı öğrenir."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 36e204c73e62e950c3f40eab7e1ce6bccd7abd83
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-use-perfinsights"></a>PerfInsights kullanma 

[PerfInsights](http://aka.ms/perfinsightsdownload) yararlı tanılama bilgileri toplar, g/ç yoğun yüklerini çalıştıran ve Microsoft Azure Windows VM performans sorunlarını gidermenize yardımcı olması için bir analiz raporunu sağlar otomatik bir komut dosyası. Bu sanal makinelerde tek başına komut dosyası olarak veya doğrudan portalından yükleyerek çalıştırılabilir [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md).

VM performans sorunları için Microsoft ile bir destek bileti açmadan önce bu betik çalıştırmanızı öneririz.

## <a name="supported-troubleshooting-scenarios"></a>Sorun giderme senaryoları desteklenir

PerfInsights toplamak ve benzersiz senaryolarına gruplandırılır birkaç tür bilgiyi analiz edin.

### <a name="collect-disk-configuration"></a>Disk yapılandırmasını Topla 

Bu senaryo, disk yapılandırması ve aşağıdaki öğeler de dahil olmak üzere diğer önemli bilgileri toplar:

-   Olay günlükleri

-   Tüm gelen ve giden bağlantıları için ağ durumu

-   Ağ ve güvenlik duvarı yapılandırma ayarları

-   Sistem üzerinde çalışmakta olan tüm uygulamalar için görev listesi

-   Sanal makine (VM) msinfo32 tarafından oluşturulan bilgi dosyası

-   (VM SQL Server çalıştıran bir sunucu belirlenirse) Microsoft SQL Server veritabanı yapılandırma ayarları

-   Depolama güvenilirlik sayaçları

-   Önemli Windows düzeltmeleri

-   Yüklü filtre sürücüleri

Bu edilgen sistem etkileyen döndürmemelidir bilgi koleksiyonudur. 

>[!Note]
>Bu senaryo aşağıdaki senaryolardan her otomatik olarak dahil edilir.

### <a name="benchmarkstorage-performance-test"></a>Kıyaslama/depolama performans testi

Bu senaryo çalıştıran [diskspd](https://github.com/Microsoft/diskspd) VM'ye bağlı olan tüm sürücüleri için Kıyaslama test (IOPS ve MB/sn). 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

### <a name="general-vm-slow-analysis"></a>Genel VM yavaş çözümleme 

Bu senaryo çalıştıran bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) Generalcounters.txt dosyasında belirtilen sayaçları kullanarak izleme. VM SQL Server çalıştıran bir sunucu belirlenirse, bir performans sayacı izlemesi Sqlcounters.txt dosyasında bulunan sayaçlarını kullanarak çalıştırır. Ayrıca, performans tanılama verilerini içerir.

### <a name="vm-slow-analysis-and-benchmark"></a>VM yavaş çözümleme ve Kıyaslama

Bu senaryo çalıştıran bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) tarafından izlenen izleme bir [diskspd](https://github.com/Microsoft/diskspd) Kıyaslama test. 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

### <a name="azure-files-analysis"></a>Azure dosyaları çözümleme 

Bu senaryo, bir özel performans sayacı yakalama ağ izleme ile birlikte çalışır. Yakalama tüm "SMB istemci paylaşımları" sayaçları içerir. Yakalama parçası olan bazı anahtar SMB istemci paylaşımı performans sayaçları verilmiştir:

| **Tür**     | **SMB istemci paylaşımları sayacı** |
|--------------|-------------------------------|
| IOPS         | Veri isteği/sn             |
|              | Okuma isteği/sn             |
|              | Yazma İsteği/sn            |
| Gecikme süresi      | Ortalama sn/veri isteği         |
|              | Ortalama sn/Okuma                 |
|              | Ortalama sn/yazma                |
| G/ç boyutu      | Ort. Bayt/veri isteği       |
|              | Ort. Okunan bayt               |
|              | Ort. Bayt/yazma              |
| Aktarım hızı   | Veri bayt/sn                |
|              | Okuma Bayt/sn                |
|              | Yazma Bayt/sn               |
| Sırası uzunluğu | Ort. Okuma sırası uzunluğu        |
|              | Ort. Kuyruk uzunluğu yazma       |
|              | Ort. Veri sırası uzunluğu        |

### <a name="custom-configuration"></a>Özel yapılandırma 

Özel yapılandırma çalıştırdığınızda, kaç tane farklı izlemeleri seçili bağlı olarak tüm izlemeleri (Performans Tanılama, performans sayacı, XPerf'in, ağ, storport) paralel olarak çalışıyor. İzleme tamamlandıktan sonra aracı seçiliyse diskspd Kıyaslama çalışır. 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Ne tür bilgiler komut dosyası tarafından toplanır?

Windows VM, disk veya depolama havuzlarını yapılandırma, performans sayaçları, hakkındaki bilgileri kaydeder ve kullanılan performans senaryoyu bağlı olarak çeşitli izlemeleri toplanır:

|Toplanan veriler                              |  |  | Performans senaryoları |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Disk yapılandırması Topla | Kıyaslama/depolama performans testi | Genel VM yavaş çözümleme | VM yavaş çözümleme ve Kıyaslama | Azure dosyaları çözümleme | Özel yapılandırma |
| Olay günlükleri bilgileri      | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Sistem bilgileri               | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Birim eşleme                       | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Disk eşleme                         | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Çalışan görevleri                    | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Depolama güvenilirlik sayaçları     | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Depolama birimi bilgileri              | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Fsutil çıktı                    | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Filtre sürücüsü bilgileri               | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Netstat çıktı                   | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Ağ yapılandırması            | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Güvenlik duvarı yapılandırması           | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| SQL Server yapılandırma         | Evet                        | Evet                                | Evet                      | Evet                            | Evet                  | Evet                  |
| Performans Tanılama izlemelerini * |                            |                                    | Evet                      |                                |                      | Evet                  |
| Performans sayacı izleme **     |                            |                                    |                          |                                |                      | Evet                  |
| SMB sayaç izleme **             |                            |                                    |                          |                                | Evet                  |                      |
| SQL Server sayaç izleme **      |                            |                                    |                          |                                |                      | Evet                  |
| XPerf'in izleme                      |                            |                                    |                          |                                |                      | Evet                  |
| StorPort izleme                   |                            |                                    |                          |                                |                      | Evet                  |
| Ağ izleme                    |                            |                                    |                          |                                | Evet                  | Evet                  |
| Diskspd Kıyaslama izleme ***      |                            | Evet                                |                          | Evet                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Performans Tanılama izleme (*)

Bir kural tabanlı altyapısı veri toplamak ve devam eden performans sorunları tanılamak için arka planda çalışır. Aşağıdaki kuralları şu anda desteklenir:

- HighCpuUsage kuralı: yüksek CPU kullanım dönemlerini algılar ve üst CPU kullanımı tüketicileri bu dönemlerde gösterir.
- HighDiskUsage kuralı: yüksek disk kullanım dönemlerini fiziksel disklerdeki algılar ve üst diski bu dönemlerde kullanım tüketicileri gösterir.
- HighResolutionDiskMetric kuralı: gösterir IOPS, üretilen iş ve g/ç gecikmesi ölçümleri başına 50 milisaniye her fiziksel disk için. Nokta azaltma disk hızla tanımlamanıza yardımcı olabilir.
- HighMemoryUsage kuralı: yüksek bellek kullanım dönemlerini algılar ve üst bellek kullanımı tüketicileri bu dönemlerde gösterir.

> [!NOTE] 
> Şu anda, .NET Framework 3.5 dahil Windows sürümleri veya sonraki sürümler desteklenir.

### <a name="performance-counter-trace-"></a>Performans sayacı izleme (*)

Aşağıdaki performans sayaçlarını toplar:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty sayfaları, \Cache\Lazy diske yazma/sn, \Server\Pool belleği olmayan havuz, hataları, disk belleğine alınan \Server\Pool hataları
- Seçili sayaçları \Network arabirimi, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network bağdaştırıcısı, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, altında \Per \Microsoft Winsock BSP işlemci ağ arabirimi kartı etkinliği

#### <a name="for-sql-server-instances"></a>SQL Server örnekleri için
- \SQL sunucu: arabellek Yöneticisi, \SQLServer:Resource havuzu istatistikleri, \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, istatistikleri
- \SQLServer:Access yöntemleri

#### <a name="for-azure-files"></a>Azure dosyaları
\SMB istemci paylaşımları

### <a name="diskspd-benchmark-trace-"></a>Diskspd Kıyaslama izleme (*)
Diskspd g/ç iş yükü testleri [işletim sistemi diski (yazma) ve havuzu sürücüler (okuma/yazma)]

## <a name="run-the-perfinsights-on-your-vm"></a>PerfInsights, VM üzerinde çalışan

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Önce komut dosyasını çalıştırmak bilebilirim ne var? 

**Komut dosyası gereksinimleri**

1.  Bu komut, performans sorunu olan VM çalıştırmanız gerekir. 

2.  Aşağıdaki işletim sistemleri desteklenir: Windows Server 2008 R2, 2012, 2012 R2'de, 2016; Windows 8.1 ve Windows 10.

**Sanal makineleri üretimde komut dosyasını çalıştırdığınızda olası sorunlar:**

1.  XPerf'in veya DiskSpd kullanarak yapılandırılan "Kıyaslama" veya "Özel" senaryo ile birlikte kullanıldığında betik VM'in performansını olumsuz etkileyebilir. Bir üretim ortamında komut dosyasını çalıştırdığınızda dikkatli olun.

2.  Komut dosyası DiskSpd kullanarak yapılandırılan "Kıyaslama" veya "Özel" senaryo ile birlikte kullandığınızda, başka bir arka plan etkinliği g/ç iş yükü test edilmiş disklerde işlemini uğratan emin olun.

3.  Varsayılan olarak, komut dosyası verilerini toplamak için geçici depolama birimi sürücüsünü kullanır. Uzun bir süre boyunca etkin kalır izleme, toplanan veri miktarını ilgili olabilir. Bu nedenle bu sürücüde güvenen herhangi bir uygulama etkileyen geçici disk alanı kullanılabilirliğini azaltabilir.

### <a name="how-do-i-run-perfinsights"></a>PerfInsights nasıl çalışır? 

Yükleyerek bir sanal makinede PerfInsights çalıştırabilirsiniz [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md) veya tek başına komut dosyası olarak çalıştırın. 

**Yükleyin ve Azure portalından PerfInsights çalıştırın**

PerfInsights artık Azure performans tanılama uzantısını adlı VM uzantısı kullanılarak çalıştırılabilir. Daha fazla bilgi için bkz: [yükleme Azure performans tanılama uzantısını](performance-diagnostics-vm-extension.md#install-the-extension).  

**Tek başına modunda PerfInsights betiği çalıştırın**

PerfInsights komut dosyasını çalıştırmak için aşağıdaki adımları izleyin:


1. Karşıdan [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. PerfInsights.zip dosya engellemesini. Bunu yapmak için PerfInsights.zip dosyaya sağ tıklayın, **özellikleri**. İçinde **genel** sekmesine **Engellemeyi Kaldır** ve ardından **Tamam**. Bu ek güvenlik ister olmadan komut dosyasını çalıştırır emin olmanızı sağlar.  

    ![Zip dosyası kilidini aç](media/how-to-use-perfInsights/unlock-file.png)

3.  (Varsayılan olarak, genellikle D sürücüsü), geçici sürücüsüne sıkıştırılmış PerfInsights.zip dosyasını genişletin. Sıkıştırılmış dosyayı aşağıdaki dosyaları ve klasörleri içermelidir:

    ![zip klasöründeki dosyaları](media/how-to-use-perfInsights/file-folder.png)

4.  Windows PowerShell'i yönetici olarak açın ve ardından PerfInsights.ps1 komut dosyasını çalıştırın.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Yürütme ilkesini değiştirmek isteyip istemediğinizi onaylamanız istenirse "y" için girmek zorunda kalabilirsiniz.

    Vazgeçme iletişim kutusunda Microsoft Support tanılama bilgileri paylaşmayı seçeneği sunulur. Devam etmek için lisans sözleşmesini aynı zamanda onaylaması gerekir. Seçimlerinizi yapın ve ardından **komut dosyasını Çalıştır**.

    ![Vazgeçme kutusu](media/how-to-use-perfInsights/disclaimer.png)

5.  (Bizim istatistiklerini budur) komut dosyasını çalıştırdığınızda, varsa olay numarasını gönderin. Ardından **Tamam**.
    
    ![Destek kimliği girin](media/how-to-use-perfInsights/enter-support-number.png)

6.  Geçici depolama sürücüsünü seçin. Betik otomatik-sürücüsünün sürücü harfini algılayabilir. Bu aşamada herhangi bir sorun meydana gelirse, (D varsayılan sürücüdür) sürücüyü seçmeniz istenebilir. Oluşturulan günlüklerin burada günlüğünde depolanan\_koleksiyon klasörü. Girin ya da sürücü harfini kabul edin sonra seçeneğini **Tamam**.

    ![Sürücü girin](media/how-to-use-perfInsights/enter-drive.png)

7.  Bir sorun giderme senaryosu sağlanan listeden seçin.

       ![Destek senaryoları seçin](media/how-to-use-perfInsights/select-scenarios.png)

8.  PerfInsights kullanıcı Arabirimi olmadan da çalıştırabilirsiniz.

    Aşağıdaki "Genel VM yavaş analiz UI istemi olmadan senaryo sorun giderme" komutunu çalıştırır veya 30 saniye veri yakalama. Aynı sorumluluk reddi ve 4. adımda anlatılan EULA onay ister.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Sessiz modda çalıştırmak için PerfInsights istiyorsanız kullanın **- AcceptDisclaimerAndShareDiagnostics** parametresi. Örneğin, aşağıdaki komutu kullanın:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Sorunları betik çalıştırılırken nasıl gidermek?

Komut dosyası sonlandırılırsa betik birlikte temizleme anahtarıyla gibi çalıştırarak tutarsız bir duruma temizleyebilirsiniz:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Geçici sürücü otomatik algılama sırasında herhangi bir sorun meydana gelirse, (D varsayılan sürücüdür) sürücüyü seçmeniz istenebilir.

![Sürücü girin](media/how-to-use-perfInsights/enter-drive.png)

Betik yardımcı program araçları kaldırır ve geçici klasör kaldırır.

### <a name="troubleshooting-other-script-issues"></a>Diğer komut dosyası sorunlarını giderme 

Komut dosyasını çalıştırdığınızda herhangi bir sorun oluşursa, komut dosyası yürütme kesmek için Ctrl + C tuşlarına basın. Geçici nesneler kaldırmak için "anormal şekilde sonlanmasından sonra temizleme" bölümüne bakın.

Komut dosyası hatası birkaç denemeden sonra bile yaşamaya devam ediyorsanız, "hata ayıklama modunda" komut dosyasını çalıştırmak kullanarak öneririz "-Debug" Başlangıçta parametre seçeneği.

Hata oluştuktan sonra PowerShell Konsolu tam çıktısını kopyalayın ve sorunu gidermesine yardımcı olmak için size yardım Microsoft Support aracı gönderebilirsiniz.

### <a name="how-do-i-run-the-script-in-custom-configuration-mode"></a>Özel yapılandırma modunda nasıl komut dosyasını çalıştırmak?

Seçerek **özel** yapılandırma, paralel (Çoklu Seçim kaydırma kullanın) birkaç izlemeleri etkinleştirebilirsiniz:

![senaryoları seçin](media/how-to-use-perfInsights/select-scenario.png)

Performans Tanılama'yı seçtiğinizde, performans sayacı izleme, XPerf'in izleme, ağ izleme veya Storport izleme senaryoları iletişim kutularındaki yönergeleri izleyin ve izlemeleri başlattıktan sonra yavaş performans sorunu yeniden oluşturmayı deneyin.

Aşağıdaki iletişim kutusunu, bir izleme başlatmanızı sağlar:

![Start-izleme](media/how-to-use-perfInsights/start-trace-message.png)

İzlemeler durdurmak için ikinci bir iletişim kutusu komutta onaylamak sahip.

![İzlemeyi Durdur](media/how-to-use-perfInsights/stop-trace-message.png)
![İzlemeyi Durdur](media/how-to-use-perfInsights/ok-trace-message.png)

İzlemeler veya işlem tamamlandığında, yeni bir dosya D: içinde oluşturulan\\günlük\_koleksiyonu (veya geçici sürücü) adlandırılan **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip.** Analiz için destek aracı bu dosyayı gönderebilir.

## <a name="review-the-diagnostics-report-created-by-perfinsights"></a>PerfInsights tarafından oluşturulan tanılama raporu gözden geçirin

İçinde **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip dosya** PerfInsights tarafından oluşturulan, PerfInsights bulgularını ayrıntıları bir HTML raporu bulabilirsiniz. Raporu gözden geçirmek için genişletme **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip** dosya ve ardından açın **PerfInsights raporu.HTML** dosya.

Seçin **bulgularını** sekmesi.

![Sekme Bul](media/how-to-use-perfInsights/findingtab.png)

**Notlar**

-   Kırmızı iletilerinde performans sorunlarına neden olabilir yapılandırma ilgili bilinen sorunlardır.

-   Sarı iletilerinde mutlaka performans sorunlarına neden olmaz en iyi olmayan yapılandırmalar temsil uyarılar var.

-   Mavi iletileri yalnızca bilgilendirici deyimleri edilir.

Bulguları ve nasıl bunlar performans ya da performans için iyileştirilmiş yapılandırmaları için en iyi uygulamaları etkileyebilir hakkında daha ayrıntılı bilgi almak tüm hata iletilerinin kırmızı için HTTP bağlantıları gözden geçirin.

### <a name="disk-configuration-tab"></a>Disk yapılandırma sekmesi

**Genel bakış** bölümü Diskpart ve depolama alanları bilgileri de dahil olmak üzere depolama yapılandırmasının farklı görünümleri görüntüler

**DiskMap** ve **VolumeMap** bölümlerde çift açısı nasıl mantıksal birimler ve fiziksel diskleri ilgili diğer için.

PhysicalDisk perspektife (DiskMap) tablo disk üzerinde çalışan tüm mantıksal birimleri gösterir. Aşağıdaki örnekte, birden çok bölüm (J ve Y) oluşturulan iki mantıksal birimler PhysicalDrive2 çalıştırır:

![Veri sekmesi](media/how-to-use-perfInsights/disktab.png)

Birim perspektif (*VolumeMap*), her bir mantıksal birim altındaki tüm fiziksel diskleri tablolarda gösterilmektedir. RAID/dinamik diskler için dikkat edin, mantıksal birimi birden fazla fiziksel diske bağlı çalışabilir. Aşağıdaki örnekte *C:\\bağlama* olduğu olarak yapılandırılan bir mountpoint *SpannedDisk* PhysicalDisks üzerinde \#2 ve \#3:

![Birim sekmesi](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>SQL Server sekmesi

Hedef VM hiç SQL Server örneği barındırıyorsa, adlı raporu bir ek sekmesine bakın **SQL Server**:

![SQL sekmesi](media/how-to-use-perfInsights/sqltab.png)

Bu bölüm, her bir VM üzerinde barındırılan SQL Server örnekleri için bir "Genel" ve ek alt sekme içerir.

"Genel bakış" bölümüne çalıştıran ve veri dosyalarını ve işlem günlüğü dosyalarını bir karışımını içeren tüm fiziksel diskleri (sistem ve veri diskleri) özetler yararlı bir tablo içeriyor.

Aşağıdaki örnekte, *PhysicalDrive0* (C sürücüsü çalıştıran) gösterilir çünkü hem *modeldev* ve *modellog* dosyaları C sürücüsünde bulunur ve bunlar farklı türlerde (veri dosyası ve işlem günlüğü gibi sırasıyla):

![ILogInformation](media/how-to-use-perfInsights/loginfo.png)

SQL Server örneği özgü sekmeler, seçilen örnek hakkındaki temel bilgileri görüntüleyen genel bir bölüm ve ayarları, yapılandırmaları ve kullanıcı seçenekleri de dahil olmak üzere Gelişmiş bilgi için ek bölümler içerir.

## <a name="references-to-the-external-tools-used"></a>Kullanılan dış araçları başvurular

### <a name="diskspd"></a>Diskspd

DISKSPD bir depolama yük oluşturucuyu ve performans testi Windows ve Windows Server ve bulut sunucu altyapısı mühendislik ekiplerinden aracıdır. Daha fazla bilgi için bkz: [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf'in

XPerf'in, Windows Performans araçları Seti izlemeleri yakalamak için bir komut satırı aracıdır.

Daha fazla bilgi için bkz: [Windows Performans Araç Seti – XPerf'in](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Sonraki Adımlar

### <a name="upload-diagnostics-logs-and-reports-to-microsoft-support-for-further-review"></a>Tanılama günlüklerini ve raporları Microsoft Support daha fazla inceleme için karşıya yükleme

Microsoft Support personeli ile çalışırken, sorun giderme işlemini yardımcı olmak için PerfInsights tarafından oluşturulan çıkış iletmeye istenebilir.

Destek Aracı DTM çalışma alanı sizin için oluşturur ve bir bağlantı içeren bir e-posta iletisi alırsınız [DTM portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) benzersiz bir kullanıcı kimliği ve parolası.

Bu iletinin gönderildiği **CTS otomatik tanılama Hizmetleri** (ctsadiag@microsoft.com).

![İleti örneği](media/how-to-use-perfInsights/supportemail.png)

Ek güvenlik için ilk kullanımda parolanızı değiştirmeniz gerekir.

DTM için oturum açtıktan sonra karşıya yüklemek için bir iletişim kutusu bulacaksınız **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip** PerfInsights tarafından toplanan dosya.
