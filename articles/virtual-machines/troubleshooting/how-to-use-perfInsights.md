---
title: Microsoft Azure'da Perfınsights kullanma | Microsoft Docs
description: Windows VM performans sorunlarını gidermek için Perfınsights kullanmayı öğrenir.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: cb414abcbbf2db7b7cd6a3d724e50010beeef647
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60318413"
---
# <a name="how-to-use-perfinsights"></a>PerfInsights’ı kullanma

[Perfınsights](https://aka.ms/perfinsightsdownload) toplar ve Tanılama verileri analiz eder ve azure'da Windows sanal makine performans sorunlarını gidermenize yardımcı olmak için bir rapor sağlayan bir kendi kendine yardım Tanılama aracı. Perfınsights kullanarak doğrudan Portalı'ndan bağımsız bir araç olarak sanal makinelerde çalıştırılabilir [Azure sanal makineler için Performans Tanılama](performance-diagnostics.md), veya yükleyerek [Azure performans tanılama VM uzantısı ](performance-diagnostics-vm-extension.md).

Destek ile irtibat kurmadan önce sanal makineler, performans sorunları yaşıyorsanız, bu aracı çalıştırın.

## <a name="supported-troubleshooting-scenarios"></a>Desteklenen sorun giderme senaryoları

Perfınsights, toplar ve çeşitli bilgiler analiz edin. Aşağıdaki bölümlerde, yaygın senaryoları kapsar.

### <a name="quick-performance-analysis"></a>Hızlı Performans Analizi

Bu senaryo, disk yapılandırmasını ve diğer önemli bilgileri toplar dahil olmak üzere:

-   Olay günlükleri

-   Tüm gelen ve giden bağlantılar için ağ durumu

-   Ağ ve güvenlik duvarı yapılandırma ayarları

-   Sistem üzerinde çalışmakta olan tüm uygulamalar için görev listesi

-   Microsoft SQL Server veritabanı yapılandırma ayarları (VM, SQL Server çalıştıran bir sunucu olarak tanımlanması durumunda)

-   Depolama güvenilirliği sayaçları

-   Önemli Windows düzeltmeler

-   Yüklü filtre sürücüleri

Bu sistem Etkilenme bilgi Pasif bir koleksiyonudur. 

>[!Note]
>Bu senaryo aşağıdaki senaryolardan her otomatik olarak eklenir:

### <a name="benchmarking"></a>Değerlendirmesi

Bu senaryo çalıştıran [Diskspd](https://github.com/Microsoft/diskspd) VM'ye bağlı tüm sürücüleri için karşılaştırmalı testte (IOPS ve MB/sn). 

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalışmamalıdır. Bu senaryo, gerekirse sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya karşılaştırmalı test tarafından neden olan bir iş yükünün artmasına, sanal makinenizin performansını olumsuz etkileyebilir.
>

### <a name="performance-analysis"></a>Performans Analizi

Bu senaryo çalışır bir [performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) RuleEngineConfig.json dosyasında belirtilen sayaçları kullanarak izleme. VM, SQL Server çalıştıran bir sunucu olarak belirlenirse, bir performans sayacı izlemesi çalıştırılır. Bunu RuleEngineConfig.json dosyasında bulunan sayaçlarını kullanarak yapar. Bu senaryo ayrıca performans tanılama verilerini içerir.

### <a name="azure-files-analysis"></a>Azure dosyaları analizi

Bu senaryo, bir özel performans sayacı yakalama ağ izleme ile birlikte çalışır. Yakalama, tüm sunucu ileti bloğu (SMB) istemci paylaşımları sayaçlar içerir. Yakalama parçası olan bazı önemli SMB istemci paylaşımı performans sayaçları verilmiştir:

| **Tür**     | **SMB istemci paylaşımları sayacı** |
|--------------|-------------------------------|
| IOPS         | Veri isteği/sn             |
|              | Okuma isteği/sn             |
|              | Yazma İsteği/sn            |
| Gecikme süresi      | Sn/veri isteği Ort.         |
|              | Ortalama sn/Okuma                 |
|              | Ortalama sn/yazma                |
| G/ç boyutu      | Ort. Bayt/veri isteği       |
|              | Ort. Okunan bayt               |
|              | Ort. Bayt/yazma              |
| Aktarım hızı   | Veri bayt/sn                |
|              | Okuma Bayt/sn                |
|              | Yazma Bayt/sn               |
| Kuyruk Uzunluğu | Ort. Okuma kuyruğu uzunluğu        |
|              | Ort. Kuyruk uzunluğu yazma       |
|              | Ort. Veri sırası uzunluğu        |

### <a name="advanced-performance-analysis"></a>Gelişmiş Performans Analizi

Gelişmiş performans analizini çalıştırdığınızda, izlemeleri paralel olarak çalıştırmak için seçin. İsterseniz, bunları tüm (performans sayacı, Xperf, ağ ve StorPort) çalıştırabilirsiniz.  

> [!Note]
> Bu senaryo, sistem etkileyebilir ve canlı üretim sisteminde çalışmamalıdır. Bu senaryo, gerekirse sorunları önlemek için bir adanmış bakım penceresinde çalıştırın. Bir izleme veya karşılaştırmalı test tarafından neden olan bir iş yükünün artmasına, sanal makinenizin performansını olumsuz etkileyebilir.
>

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>Ne tür bilgilerin Perfınsights tarafından toplanır?

Windows VM, disk veya depolama havuzlarını yapılandırma, performans sayaçları, ilgili bilgileri günlüğe kaydeder ve çeşitli izlemeleri toplanır. Bu, kullanmakta olduğunuz performans senaryoya bağlıdır. Aşağıdaki tabloda Ayrıntılar verilmiştir:

|Toplanan veriler                              |  |  | Performans senaryoları |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                               | Hızlı Performans Analizi | Değerlendirmesi | Performans Analizi | Azure dosyaları analizi | Gelişmiş Performans Analizi |
| Olay günlüklerinden bilgi       | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Sistem bilgileri                | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Birim eşleme                        | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Disk eşleme                          | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Çalışan görevler                     | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Depolama güvenilirliği sayaçları      | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Depolama bilgileri               | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Fsutil çıkış                     | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Filtre sürücü bilgileri                | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Netstat çıkış                    | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Ağ yapılandırması             | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Güvenlik duvarı yapılandırması            | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| SQL Server yapılandırması          | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Performans Tanılama izlemeleri *  | Evet                        | Evet                                | Evet                      | Evet                  | Evet                  |
| Performans sayacı izleme **      |                            |                                    | Evet                      |                      | Evet                  |
| SMB sayacı izleme **              |                            |                                    |                          | Evet                  |                      |
| SQL Server sayacı izleme **       |                            |                                    | Evet                      |                      | Evet                  |
| XPerf izleme                       |                            |                                    |                          |                      | Evet                  |
| StorPort izleme                    |                            |                                    |                          |                      | Evet                  |
| Ağ izleme                     |                            |                                    |                          | Evet                  | Evet                  |
| Diskspd Kıyaslama izleme ***       |                            | Evet                                |                          |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Performans Tanılama izleme (*)

Kural tabanlı bir altyapı, veri toplamak ve sürekli performans sorunlarını tanılamak için arka planda çalışır. Aşağıdaki kurallar geçerli desteklenir:

- HighCpuUsage kuralı: Yüksek CPU kullanım dönemlerini algılar ve en çok CPU kullanımı tüketicileri bu dönemlerde gösterir.
- HighDiskUsage kuralı: Yüksek disk kullanım dönemleri fiziksel disklerde algılar ve üst disk kullanımı tüketicileri bu dönemlerde gösterir.
- HighResolutionDiskMetric kuralı: 50 milisaniye her fiziksel disk başına IOPS, aktarım hızı ve g/ç gecikme süresi ölçümlerini gösterir. Nokta azaltma disk hızlıca tanımlamanıza yardımcı olur.
- HighMemoryUsage kuralı: Yüksek bellek kullanım dönemlerini algılar ve en çok bellek kullanım tüketicileri bu dönemlerde gösterir.

> [!NOTE] 
> Şu anda, .NET Framework 4.5 içeren Windows sürümlerini veya sonraki sürümler desteklenir.

### <a name="performance-counter-trace-"></a>Performans sayacı izleme (*)

Aşağıdaki performans sayaçlarını toplar:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk ve \LogicalDisk
- \Cache\Dirty sayfaları, \Cache\Lazy diske yazma/sn, \Server\Pool alınamayan, hataları ve \Server\Pool disk belleğine alınan hatalar
- Seçili sayaçları \Network arabirimi \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network bağdaştırıcısı, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, altında \ Ağ QoS Policy\Packets \Per işlemci ağ arabirimi kartı etkinliği ve \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>SQL Server örnekleri için
- \SQL Server:Buffer Manager, \SQLServer:Resource Pool Stats, and \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, istatistikleri
- \SQLServer:Access yöntemleri

#### <a name="for-azure-files"></a>Azure dosyaları için
\SMB istemci paylaşımları

### <a name="diskspd-benchmark-trace-"></a>Diskspd Kıyaslama izleme (*)
Diskspd g/ç iş yükü testleri (işletim sistemi diski [yazma] ve havuzu sürücüleri okuma/yazma)

## <a name="run-the-perfinsights-tool-on-your-vm"></a>Sanal makinenizde Perfınsights aracı çalıştırın

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>Önce aracı biliyorum ne gerekir? 

#### <a name="tool-requirements"></a>Aracı gereksinimleri

-  Bu araç, performans sorunu olan VM üzerinde çalıştırmanız gerekir. 

-  Aşağıdaki işletim sistemleri desteklenmektedir: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016; Windows 8.1 ve Windows 10.

#### <a name="possible-problems-when-you-run-the-tool-on-production-vms"></a>Üretim Vm'lerinde aracı çalıştırdığınızda olası sorunlar

-  Kıyaslama senaryo veya Xperf veya Diskspd kullanacak şekilde yapılandırılmış "Gelişmiş Performans Analizi" bir senaryo için araç VM'nin performansını olumsuz etkileyebilir. Bu senaryolar, Canlı üretim ortamında çalıştırılmamalıdır.

-  Kıyaslama senaryo veya Diskspd kullanacak şekilde yapılandırılmış "Gelişmiş Performans Analizi" bir senaryo için başka bir arka plan etkinliği g/ç iş yükü uğratan emin olun.

-  Varsayılan olarak, aracı, verileri toplamak için geçici depolama birimi sürücüsünü kullanır. Daha uzun bir süre etkin kalır izleme, toplanan veri miktarı ilgili olabilir. Bu, geçici diskteki kullanılabilirliğini azaltabilir ve bu nedenle, bu sürücüde güvenen herhangi bir uygulama etkileyebilir.

### <a name="how-do-i-run-perfinsights"></a>Perfınsights nasıl çalıştırılır? 

Perfınsights yükleyerek bir sanal makine üzerinde çalıştırarak [Azure performans tanılama VM uzantısı](performance-diagnostics-vm-extension.md). Uygulamayı ayrı bir araç olarak da çalıştırabilirsiniz. 

**Yükleme ve Azure portalından Perfınsights çalıştırma**

Bu seçenek hakkında daha fazla bilgi için bkz. [Azure performans tanılama VM uzantısı yükleme](performance-diagnostics-vm-extension.md#install-the-extension).  

**Perfınsights tek başına modunda çalıştırın.**

Perfınsights aracını çalıştırmak için aşağıdaki adımları izleyin:


1. İndirme [PerfInsights.zip](https://aka.ms/perfinsightsdownload).

2. PerfInsights.zip dosya engeli kaldırın. Bunu yapmak için PerfInsights.zip dosyasını sağ tıklatın ve seçin **özellikleri**. İçinde **genel** sekmesinde **Engellemeyi Kaldır**ve ardından **Tamam**. Bu, herhangi bir ek güvenlik ister olmadan aracı çalıştırmasını sağlar.  

    ![Ekran görüntüsü, Perfınsights özellikleriyle, vurgulanan Engellemeyi Kaldır](media/how-to-use-perfInsights/unlock-file.png)

3.  Sıkıştırılmış PerfInsights.zip dosyayı geçici sürücünüze genişletin (varsayılan olarak, genellikle D sürücüsünü budur). 

4.  Yönetici olarak bir Windows komut istemi açın ve ardından kullanılabilir komut satırı parametreleri görüntülemek için PerfInsights.exe çalıştırın.

    ```
    cd <the path of PerfInsights folder>
    PerfInsights
    ```
    ![Ekran Perfınsights'ın, komut satırı çıkışı](media/how-to-use-perfInsights/PerfInsightsCommandline.png)
    
    Perfınsights senaryoları çalıştırmaya yönelik temel sözdizimi aşağıdaki gibidir:
    
    ```
    PerfInsights /run <ScenarioName> [AdditionalOptions]
    ```

    Kullanabileceğiniz aşağıdaki örnekteki 5 dakika için Performans Analizi senaryosu çalıştırmak için:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Aşağıdaki örnek, Gelişmiş bir senaryo Xperf ve performans sayacı izlemeler için 5 dakika ile çalıştırmak için kullanabilirsiniz:
    
    ```
    PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Kullanabileceğiniz örnek 5 dakika için Performans Analizi senaryosu çalıştırmak ve sonucu zip dosyasını depolama hesabına yüklemek için aşağıda:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>
    ```

    Tüm kullanılabilir senaryolar ve seçenekleri kullanarak göz atabilirsiniz **/list** komutu:
    
    ```
    PerfInsights /list
    ```

    >[!Note]
    >Bir senaryo çalıştırmadan önce Perfınsights tanılama bilgilerini Paylaş ve EULA'yı kabul etmek için kabul etmelerini ister. Kullanım **/AcceptDisclaimerAndShareDiagnostics** seçeneği bu komut istemlerini atlanacak. 
    >
    >Microsoft ve birlikte çalıştığınız destek mühendisi, istek başına çalışan Perfınsights bir etkin bir destek bileti varsa, destek bileti numarası kullanarak sağlamak emin olun **/sr** seçeneği.
    >
    >Varsayılan olarak, en son sürüme güncelleştirme kendisini varsa Perfınsights çalışacaktır. Kullanım **/SkipAutoUpdate** veya **/sau** otomatik güncelleştirmeyi atlama parametresi.  
    >
    >Süresini geçerseniz **/d** belirtilmezse, Perfınsights sizden yineleme için sorun vmslow depolamasını Azure dosyalarına ve Gelişmiş senaryolar çalışırken. 

İzlemeleri ya da işlem tamamlandığında, yeni bir dosya aynı klasörde Perfınsights görünür. Dosyanın adı **PerformanceDiagnostics\_yyyy-aa-gg\_ss dd ss fff.zip.** Analiz için destek aracı bu dosyayı göndermek veya bulguları ve önerileri gözden geçirmek için zip dosyası içinde bir rapor açın.

## <a name="review-the-diagnostics-report"></a>Tanılama raporunu gözden geçirin

İçinde **PerformanceDiagnostics\_yyyy-aa-gg\_ss dd ss fff.zip** dosyası bulguları Perfınsights'ın ayrıntılarını bir HTML raporu bulabilirsiniz. Raporu gözden geçirmek için genişletme **PerformanceDiagnostics\_yyyy-aa-gg\_ss dd ss fff.zip** dosyasını ve ardından açın **Perfınsights raporu.HTML** dosya.

Seçin **bulguları** sekmesi.

![Perfınsights raporunun ekran görüntüsü](media/how-to-use-perfInsights/findingtab.png)
![Perfınsights raporunun ekran görüntüsü](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Yüksek olarak kategorilere bulguları bilinen sorunlardır ve performans sorunlarına neden. Orta temsil gibi mutlaka performans sorunlarına neden uygun olmayan yapılandırmalar bulguları kategorilere ayrılmıştır. Düşük olarak kategorilere bulguları bilgilendirici deyimleri yalnızca ' dir.

Tüm yüksek ve orta ölçekli bulgular için bağlantıları ve önerileri gözden geçirin. Performansı nasıl etkileyebileceğini hakkında ve performans için iyileştirilmiş yapılandırmaları için en iyi uygulamalar hakkında bilgi edinin.

### <a name="storage-tab"></a>Depolama Sekmesi

**Bulguları** çeşitli bulguları ve depolaması ile ilgili öneriler bölümünde görüntülenir.

**Disk harita** ve **birim harita** bölümlerde mantıksal birimler ve fiziksel diskleri birbiriyle ilgilidir.

Fiziksel disk açısından (Disk eşleme), tablo disk üzerinde çalışan tüm mantıksal birimleri gösterir. Aşağıdaki örnekte, **PhysicalDrive2** birden çok bölüm (J ve H) oluşturulan iki mantıksal birimler çalıştırır:

![Disk sekmesinin Ekran görüntüsü](media/how-to-use-perfInsights/disktab.png)

Birim açısından (birim eşleme), tablolar, her bir mantıksal birim altındaki tüm fiziksel diskleri gösterir. RAID/dinamik diskleri için dikkat edin, mantıksal birim birden çok fiziksel disklerde çalıştırabilirsiniz. Aşağıdaki örnekte, *C:\\bağlama* olarak yapılandırılmış bir bağlama noktası *SpannedDisk* fiziksel disklerde 2 ve 3:

![Birim sekmesinin Ekran görüntüsü](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>SQL sekmesi

' % S'hedef sanal Makineyi barındıran SQL Server örnekleri adlı raporu, ek bir sekme görürsünüz **SQL**:

![Ekran görüntüsü, SQL sekmesi](media/how-to-use-perfInsights/sqltab.png)

Bu bölüm içeren bir **bulguları** sekmesi ve her bir VM üzerinde barındırılan SQL Server örnekleri için ek sekmeleri.

**Bulguları** sekmesi tüm SQL listesini içeren ilgili performans sorunları, yanı sıra öneriler bulunamadı.

Aşağıdaki örnekte, **PhysicalDrive0** (C sürücüsü çalıştıran) görüntülenir. Bunun nedeni, hem **modeldev** ve **modellog** dosyaları C sürücüsünde bulunur ve bunlar farklı türde (veri dosyası ve işlem günlüğü gibi sırasıyla).

![Günlük bilgilerini ekran görüntüsü](media/how-to-use-perfInsights/loginfo.png)

SQL Server'ın belirli örnekler için sekmeler, seçilen örnek hakkındaki temel bilgileri görüntüleyen genel bir bölümü içermelidir. Sekmeler, ayrıca kullanıcı seçenekleri ayarları ve yapılandırmaları dahil gelişmiş bilgi ek bölümler içerir.

### <a name="diagnostic-tab"></a>Tanılama sekmesi
**Tanılama** sekmesi hakkında bilgiler içerir üst CPU, disk ve bellek tüketiciler bilgisayarda çalışan süresince Perfınsights'ın. Sistemi eksik görev listesi ve önemli sistem olaylarını olabilir da kritik düzeltme hakkında bilgi bulabilirsiniz. 

## <a name="references-to-the-external-tools-used"></a>Başvurular için kullanılan dış Araçlar

### <a name="diskspd"></a>Diskspd

Diskspd bir depolama yük oluşturucu ve performans testi Microsoft gelen aracıdır. Daha fazla bilgi için [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>Xperf

XPerf, Windows Performans Araç Seti izlemelerinden yakalamak için bir komut satırı aracıdır. Daha fazla bilgi için [Windows Performans Araç Seti – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla inceleme için Microsoft Support tanılama günlüklerini ve raporları yükleyebilirsiniz. Desteği, sorun giderme işlemi ile yardımcı olmak için Perfınsights tarafından oluşturulan çıkış aktarmak isteyebilir.

Aşağıdaki ekran görüntüsünde, ne alabileceğiniz benzer bir ileti gösterir:

![Microsoft Support örnek iletisinin ekran görüntüsü](media/how-to-use-perfInsights/supportemail.png)

Dosya aktarımı çalışma alanına erişmek için iletideki yönergeleri izleyin. Ek güvenlik için ilk kullanımda parolanızı değiştirmeniz gerekir.

Oturum açtıktan sonra karşıya yüklemek için bir iletişim kutusu bulacaksınız **PerformanceDiagnostics\_yyyy-aa-gg\_ss dd ss fff.zip** Perfınsights tarafından toplanan dosya.

