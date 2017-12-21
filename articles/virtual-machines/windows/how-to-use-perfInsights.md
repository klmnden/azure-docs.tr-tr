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
ms.openlocfilehash: f15875610e2035c6f4c10c36e19c02f3e045b3ea
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-use-perfinsights"></a>PerfInsights kullanma 

[PerfInsights](http://aka.ms/perfinsightsdownload) yararlı tanı bilgilerini toplayan bir otomatik komut dosyasıdır. Ayrıca g/ç yoğun yüklerini çalışır ve Azure Windows sanal makine performans sorunlarını gidermenize yardımcı olması için bir analiz raporu sağlar. Bu sanal makinelerde tek başına komut dosyası olarak ya da doğrudan portalından yükleyerek çalıştırılabilir [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md).

Destek ile irtibat kurmadan önce sanal makineleri performans sorunları yaşıyorsanız, bu komut dosyasını çalıştırın.

## <a name="supported-troubleshooting-scenarios"></a>Sorun giderme senaryoları desteklenir

PerfInsights toplamak ve birkaç tür bilgiyi analiz edin. Aşağıdaki bölümlerde, yaygın senaryolar kapsar.

### <a name="collect-basic-configuration"></a>Temel yapılandırma Topla 

Bu senaryo disk yapılandırması ve diğer önemli bilgileri toplar dahil olmak üzere:

-   Olay günlükleri

-   Tüm gelen ve giden bağlantıları için ağ durumu

-   Ağ ve güvenlik duvarı yapılandırma ayarları

-   Sistem üzerinde çalışmakta olan tüm uygulamalar için görev listesi

-   Sanal makine için msinfo32 tarafından oluşturulan bilgi dosyası

-   (VM SQL Server çalıştıran bir sunucu belirlenirse) Microsoft SQL Server veritabanı yapılandırma ayarları

-   Depolama güvenilirlik sayaçları

-   Önemli Windows düzeltmeleri

-   Yüklü filtre sürücüleri

Bu edilgen sistem etkileyen döndürmemelidir bilgi koleksiyonudur. 

>[!Note]
>Bu senaryo aşağıdaki senaryolardan her otomatik olarak dahil edilir.

### <a name="benchmarking"></a>Değerlendirmesi

Bu senaryo çalıştıran [Diskspd](https://github.com/Microsoft/diskspd) VM'ye bağlı olan tüm sürücüleri için Kıyaslama test (IOPS ve MB/sn). 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

### <a name="slow-vm-analysis"></a>Yavaş VM çözümleme 

Bu senaryo çalıştıran bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) Generalcounters.txt dosyasında belirtilen sayaçları kullanarak izleme. SQL Server çalıştıran bir sunucu VM tanımlanırsa, bir performans sayacı izlemesi çalışır. Bunu Sqlcounters.txt dosyasında bulunan sayaçlarını kullanarak yapar ve performans tanılama verilerini de içerir.

### <a name="slow-vm-analysis-and-benchmarking"></a>Yavaş VM çözümleme ve değerlendirmesi

Bu senaryo çalıştıran bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) tarafından izlenen izleme bir [Diskspd](https://github.com/Microsoft/diskspd) Kıyaslama test. 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

### <a name="azure-files-analysis"></a>Azure dosyaları çözümleme 

Bu senaryo, bir özel performans sayacı yakalama ağ izleme ile birlikte çalışır. Yakalama tüm sunucu ileti bloğu (SMB) istemci paylaşımları sayaçları içerir. Yakalama parçası olan bazı anahtar SMB istemci paylaşımı performans sayaçları verilmiştir:

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
| Kuyruk Uzunluğu | Ort. Okuma sırası uzunluğu        |
|              | Ort. Kuyruk uzunluğu yazma       |
|              | Ort. Veri sırası uzunluğu        |

### <a name="custom-slow-vm-analysis"></a>Özel yavaş VM çözümleme 

Özel bir yavaş VM analizi çalıştırdığınızda, paralel olarak çalıştırmak için izlemeleri seçin. İstiyorsanız, bunları tüm (performans sayacı, XPerf'in, ağ ve StorPort) çalıştırabilirsiniz. İzleme tamamlandıktan sonra aracı seçiliyse Diskspd Kıyaslama çalışır. 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalıştırılması gerekir. Gerekirse, bu senaryo sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya Kıyaslama test tarafından neden iş yükünün artmasına VM performansını olumsuz etkileyebilir.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Ne tür bilgiler komut dosyası tarafından toplanır?

Windows VM, disk veya depolama havuzlarını yapılandırma, performans sayaçları, ilgili bilgileri günlüğe kaydeder ve çeşitli izlemeleri toplanır. Kullanmakta olduğunuz performans senaryoya bağlıdır. Aşağıdaki tabloda Ayrıntılar verilmiştir:

|Toplanan veriler                              |  |  | Performans senaryoları |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Temel yapılandırma Topla | Değerlendirmesi | Yavaş VM çözümleme | Yavaş VM çözümleme ve değerlendirmesi | Azure dosyaları çözümleme | Özel yavaş VM çözümleme |
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
| Performans Tanılama izlemelerini * | Evet                        | Evet                                | Evet                      |                                | Evet                  | Evet                  |
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

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk ve \LogicalDisk
- \Cache\Dirty sayfaları, \Cache\Lazy diske yazma/sn, \Server\Pool belleği olmayan havuz, hataları ve disk belleği \Server\Pool hataları
- Seçili sayaçları \Network arabirimi, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network bağdaştırıcısı, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, altında \ QoS Policy\Packets, \Per işlemci ağ arabirimi kartı etkinliği ve \Microsoft Winsock BSP ağ

#### <a name="for-sql-server-instances"></a>SQL Server örnekleri için
- \SQL sunucu: arabellek Yöneticisi, \SQLServer:Resource havuzu istatistikleri ve \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, istatistikleri
- \SQLServer:Access yöntemleri

#### <a name="for-azure-files"></a>Azure dosyaları
\SMB istemci paylaşımları

### <a name="diskspd-benchmark-trace-"></a>Diskspd Kıyaslama izleme (*)
Diskspd g/ç iş yükü testleri (işletim sistemi diski [yazma] ve [okuma/yazma] havuzu sürücüleri)

## <a name="run-the-perfinsights-script-on-your-vm"></a>VM üzerinde PerfInsights betiği çalıştırın

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Önce komut dosyasını çalıştırmak bilebilirim ne var? 

#### <a name="script-requirements"></a>Komut dosyası gereksinimleri

-  Bu komut, performans sorunu olan VM çalıştırmanız gerekir. 

-  Aşağıdaki işletim sistemleri desteklenmektedir: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016; Windows 8.1 ve Windows 10.

#### <a name="possible-problems-when-you-run-the-script-on-production-vms"></a>Sanal makineleri üretimde komut dosyasını çalıştırdığınızda olası sorunlar

-  Herhangi bir Kıyaslama senaryosunda veya XPerf'in veya Diskspd kullanmak üzere yapılandırılmış "Özel yavaş VM analiz" senaryosu için komut dosyası VM'in performansını olumsuz etkileyebilir. Bir üretim ortamında bu senaryoları çalışmamalıdır.

-  Herhangi bir Kıyaslama senaryosunda veya Diskspd kullanmak üzere yapılandırılmış "Özel yavaş VM analiz" senaryosu için başka bir arka plan etkinliği g/ç iş yükü ile uğratan emin olun.

-  Varsayılan olarak, komut dosyası verilerini toplamak için geçici depolama birimi sürücüsünü kullanır. Uzun bir süre boyunca etkin kalır izleme, toplanan veri miktarını ilgili olabilir. Bu geçici disk alanı kullanılabilirliğini azaltabilir ve bu nedenle bu sürücüde güvenen herhangi bir uygulama etkileyebilir.

### <a name="how-do-i-run-perfinsights"></a>PerfInsights nasıl çalışır? 

Yükleyerek bir sanal makinede PerfInsights çalıştırabilirsiniz [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md). Ayrıca tek başına komut dosyası olarak çalıştırabilirsiniz. 

**Yükleyin ve Azure portalından PerfInsights çalıştırın**

Bu seçenek hakkında daha fazla bilgi için bkz: [Azure performans tanılama VM uzantısı yüklemek](performance-diagnostics-vm-extension.md#install-the-extension).  

**Tek başına modunda PerfInsights betiği çalıştırın**

PerfInsights komut dosyasını çalıştırmak için aşağıdaki adımları izleyin:


1. Karşıdan [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. PerfInsights.zip dosya engellemesini. Bunu yapmak için PerfInsights.zip dosyasını sağ tıklatın ve seçin **özellikleri**. İçinde **genel** sekmesine **Engellemeyi Kaldır**ve ardından **Tamam**. Bu, ek güvenlik ister olmadan komut dosyası çalıştırmasını sağlar.  

    ![Ekran görüntüsü, PerfInsights özellikleriyle, vurgulanmış Engellemeyi Kaldır](media/how-to-use-perfInsights/unlock-file.png)

3.  Geçici sürücünüze sıkıştırılmış PerfInsights.zip dosyasını genişletin (varsayılan olarak, bu genellikle D sürücüdür). Sıkıştırılmış dosyayı aşağıdaki dosyaları ve klasörleri içermelidir:

    ![Zip klasöründeki dosyaların ekran görüntüsü](media/how-to-use-perfInsights/file-folder.png)

4.  Windows PowerShell'i yönetici olarak açın ve ardından PerfInsights.ps1 komut dosyasını çalıştırın.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Yürütme ilkesini değiştirmek istediğinizi onaylamak için "y" girmeniz gerekebilir.

    İçinde **dikkat edin ve kullanıcının onaylaması** iletişim kutusu, Microsoft Support tanılama bilgilerini paylaşacak seçeneğiniz vardır. Devam etmek için lisans sözleşmesini aynı zamanda onaylaması gerekir. Seçimlerinizi yapın ve ardından **komut dosyasını Çalıştır**.

    ![Ekran görüntüsü, bildirim ve onay iletişim kutusu](media/how-to-use-perfInsights/disclaimer.png)

5.  Komut dosyasını çalıştırdığınızda, varsa olay numarasını gönderin. Ardından **Tamam**.
    
    ![Destek kimliği iletişim kutusunun ekran görüntüsü](media/how-to-use-perfInsights/enter-support-number.png)

6.  Geçici depolama sürücüsünü seçin. Betik otomatik-sürücüsünün sürücü harfini algılayabilir. Bu aşamada herhangi bir sorun meydana gelirse, (D varsayılan sürücüdür) sürücüyü seçmeniz istenebilir. Oluşturulan günlüklerin burada günlüğünde depolanan\_koleksiyon klasörü. Girin veya sürücü harfini kabul edin sonra seçin **Tamam**.

    ![Geçici sürücü iletişim kutusunun ekran görüntüsü](media/how-to-use-perfInsights/enter-drive.png)

7.  Bir sorun giderme senaryosu sağlanan listeden seçin.

       ![Sorun giderme senaryoları listesi işleminin ekran görüntüsü](media/how-to-use-perfInsights/select-scenarios.png)

PerfInsights kullanıcı Arabirimi olmadan da çalıştırabilirsiniz. Aşağıdaki komut, "kullanıcı Arabirimi olmadan senaryo sorun giderme yavaş VM analiz" çalıştırır. Aynı sorumluluk reddi ve 4. adımda anlatılan EULA onay ister.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

Sessiz modda çalıştırmak için PerfInsights istiyorsanız kullanın **- AcceptDisclaimerAndShareDiagnostics** parametresi. Örneğin, aşağıdaki komutu kullanın:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Sorunları betik çalıştırılırken nasıl gidermek?

Komut dosyası sonlandırılırsa temizleme anahtarı ile birlikte komut aşağıdaki gibi çalıştırarak tutarlı bir duruma döndürebilirsiniz:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Geçici sürücü otomatik algılama sırasında herhangi bir sorun meydana gelirse, (D varsayılan sürücüdür) sürücüyü seçmeniz istenebilir.

![Geçici sürücü iletişim kutusunun ekran görüntüsü](media/how-to-use-perfInsights/enter-drive.png)

Betik yardımcı program araçları kaldırır ve geçici klasör kaldırır.

### <a name="troubleshoot-other-script-issues"></a>Diğer komut dosyası sorunlarını giderme 

Komut dosyasını çalıştırdığınızda herhangi bir sorun oluşursa, komut dosyasını kesmek için Ctrl + C tuşlarına basın. Birkaç denemeden sonra bile komut dosyası hatası yaşamaya devam ederseniz, komut dosyası hata ayıklama modunda kullanarak çalıştırmak "-Debug" Başlangıçta parametre seçeneği.

Hata oluştuktan sonra PowerShell Konsolu tam çıktısını kopyalayın ve sorunu gidermesine yardımcı olmak için size yardım Microsoft Support aracı gönderebilirsiniz.

### <a name="how-do-i-run-the-script-in-custom-slow-vm-analysis-mode"></a>"Özel yavaş VM analiz" modunda nasıl komut dosyasını çalıştırmak?

Seçerek **özel yavaş VM analiz**, birkaç izlemeleri (birden çok izlemeleri seçmek için SHIFT tuşunu kullanın) paralel etkinleştirebilirsiniz:

![Ekran görüntüsü senaryoların listesi](media/how-to-use-perfInsights/select-scenario.png)

Performans sayacı izleme, XPerf'in izleme, ağ izleme veya Storport izleme senaryoları seçtiğinizde, sonraki iletişim kutularını'ndaki yönergeleri izleyin. İzlemeler başlattıktan sonra yavaş performans sorunu yeniden oluşturmayı deneyin.

Aşağıdaki iletişim kutusunu izlemek başlatın:

![Ekran görüntüsü, başlangıç performans sayacı izleme iletişim kutusu](media/how-to-use-perfInsights/start-trace-message.png)

İzlemeler durdurmak için ikinci bir iletişim kutusu komutta onaylamak sahip.

![Ekran görüntüsü, durdurma performans sayacı izleme iletişim kutusu](media/how-to-use-perfInsights/stop-trace-message.png)
![iletişim kutusunun ekran görüntüsü, durdurma tüm izler](media/how-to-use-perfInsights/ok-trace-message.png)

İzlemeler veya işlem tamamlandığında, yeni bir dosya D: görünür\\günlük\_koleksiyonu (veya geçici sürücüsü). Dosya adı **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip.** Analiz için destek aracı bu dosyayı gönderebilir.

## <a name="review-the-diagnostics-report"></a>Tanılama raporu gözden geçirin

İçinde **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip** dosyası PerfInsights bulgularını ayrıntıları bir HTML raporu bulabilir. Raporu gözden geçirmek için genişletme **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip** dosya ve ardından açın **PerfInsights raporu.HTML** dosya.

Seçin **bulgularını** sekmesi.

![PerfInsights raporunun ekran](media/how-to-use-perfInsights/findingtab.png)
![PerfInsights raporunun ekran görüntüsü](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Kritik olarak kategorilere bulgularını performans sorunlarına neden ilgili bilinen sorunlardır. Bulguları mutlaka performans sorunlarına neden olmaz önemli temsil en iyi olmayan yapılandırmalar kategorilere. Bilgi olarak kategorilere bulgularını bilgilendirici deyimleri yalnızca ' dir.

Tüm kritik ve önemli bulguları için bağlantıları ve önerileri gözden geçirin. Performans nasıl etkileyebileceği hakkında ve performansı optimize yapılandırmaları için en iyi uygulamalar hakkında bilgi edinin.

### <a name="storage-tab"></a>Depolama Sekmesi

**Bulgularını** bölümünde çeşitli bulgularını ve depolama birimine ilgili öneriler görüntülenir.

**Disk eşleme** ve **birim harita** bölümlerde nasıl mantıksal birimler ve fiziksel diskleri birbirleriyle ilişkilidir.

Fiziksel disk açısından (Disk eşleme), tablo disk üzerinde çalışan tüm mantıksal birimleri gösterir. Aşağıdaki örnekte, **PhysicalDrive2** birden çok bölüm (J ve Y) oluşturulan iki mantıksal birimler çalışır:

![Disk sekmesinin Ekran görüntüsü](media/how-to-use-perfInsights/disktab.png)

Birim açısından (toplu eşleme), her bir mantıksal birim altındaki tüm fiziksel diskleri tablolarda gösterilmektedir. RAID/dinamik diskler için dikkat edin, mantıksal birim birden çok fiziksel diskler üzerinde çalışabilir. Aşağıdaki örnekte, *C:\\bağlama* bir bağlama noktası olarak yapılandırılan *SpannedDisk* fiziksel disklerde 2 ve 3:

![Birim sekmesinin Ekran görüntüsü](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>SQL sekmesi

Hedef VM hiç SQL Server örneği barındırıyorsa, rapordaki adlı ek bir sekme bkz **SQL**:

![Ekran görüntüsü, SQL sekmesi](media/how-to-use-perfInsights/sqltab.png)

Bu bölüm içeren bir **bulgularını** sekmesi ve ek sekmeleri her VM üzerinde barındırılan SQL Server örnekleri.

**Bulgularını** sekmesi tüm SQL listesini içeren ilgili performans sorunları, öneriler birlikte bulundu.

Aşağıdaki örnekte, **PhysicalDrive0** (C sürücüsü çalıştıran) görüntülenir. Bunun nedeni, hem **modeldev** ve **modellog** dosyaları C sürücüsünde bulunur ve bunlar farklı türlerde (veri dosyası ve işlem günlüğü gibi sırasıyla).

![Günlük bilgilerini ekran görüntüsü](media/how-to-use-perfInsights/loginfo.png)

SQL Server'ın belirli örnekleri için sekmeler seçilen örnek hakkındaki temel bilgileri görüntüleyen genel bir bölüm içerir. Sekmeler ayarları, yapılandırmaları ve kullanıcı seçenekleri de dahil olmak üzere Gelişmiş bilgi için ek bölümler de içerir.

### <a name="diagnostic-tab"></a>Tanılama sekmesi
**Tanılama** sekme içerir hakkında bilgi üst CPU, disk ve bellek tüketiciler bilgisayarda çalışan süresince PerfInsights. Sistem eksik, görev listesi ve önemli sistem olayları olabilir da kritik düzeltme ekleri hakkında bilgi bulabilirsiniz. 

## <a name="references-to-the-external-tools-used"></a>Kullanılan dış araçları başvurular

### <a name="diskspd"></a>Diskspd

Diskspd bir depolama yük oluşturucuyu ve performans testi Microsoft'tan aracıdır. Daha fazla bilgi için bkz: [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf'in

XPerf'in, Windows Performans araç setinden izlemelerini yakalama için bir komut satırı aracıdır. Daha fazla bilgi için bkz: [Windows Performans Araç Seti – XPerf'in](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla inceleme için Microsoft Support tanılama günlüklerini ve raporları yükleyebilirsiniz. Destek, sorun giderme işlemine yardımcı olmak için PerfInsights tarafından oluşturulan çıkış aktarmak isteyebilir.

Aşağıdaki ekran görüntüsü almayı benzer bir ileti gösterir:

![Microsoft Support örnek iletisinin ekran görüntüsü](media/how-to-use-perfInsights/supportemail.png)

Dosya aktarımı çalışma alanına erişmek için iletideki yönergeleri izleyin. Ek güvenlik için ilk kullanım parolanızı değiştirmeniz gerekir.

Oturum açtıktan sonra karşıya yüklemek için bir iletişim kutusu bulacaksınız **CollectedData\_yyyy-aa-gg\_hh\_mm\_ss.zip** PerfInsights tarafından toplanan dosya.
