---
title: "Günlük analizi veri çözümde wire | Microsoft Docs"
description: "Kablo verileri birleştirilmiş ağ ve performans OMS aracısının, Operations Manager ve Windows bağlı aracılar dahil olmak üzere bilgisayar verilerdir. Ağ verileri verilerin bağıntısını yardımcı olması için günlük verilerinizi birleştirilir."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: magoedte;banders
ms.openlocfilehash: 3bb4c82268fe7805227c213000dc803307876fe7
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Kablo verileri 2.0 (Önizleme) çözümüne günlük analizi

![Kablo verileri simgesi](./media/log-analytics-wire-data/wire-data2-symbol.png)

Kablo, birleştirilmiş ağ ve performans verilerini ortamınızda Operations Manager tarafından izlenen dahil olmak üzere OMS Aracısı ile Windows bağlı ve Linux bağlı bilgisayarlardan toplanan verilerdir. Ağ verileri verilerin bağıntısını yardımcı olması için diğer günlük verilerinizi ile birleştirilir.

OMS Aracısı ek olarak kablo verileri çözüm BT altyapınızın bilgisayarlara yüklemek Microsoft bağımlılık Aracısı kullanır. Bağımlılık aracıları izlemek için ve bilgisayarlarınızı 2-3'te ağ düzeyleri için gönderilen ağ verilerini [OSI modeli](https://en.wikipedia.org/wiki/OSI_model)dahil çeşitli protokoller ve bağlantı noktaları kullanılır. Veriler için günlük analizi gönderilir ve aracıları kullanma.  

> [!NOTE]
> Kablo verileri çözümü önceki sürümü için yeni çalışma alanı ekleyemezsiniz. Etkin özgün kablo verileri çözüm varsa, onu kullanmaya devam edebilirsiniz. Ancak, kablo verileri 2.0 kullanmak için öncelikle özgün sürümü da kaldırmanız gerekir.

Varsayılan olarak, günlük analizi veri CPU, bellek, disk ve ağ performansını verilerden belirtebilirsiniz diğer performans sayaçları yanı sıra Windows ve Linux yerleşik sayaçları günlüğe kaydeder. Ağ ve diğer veri toplama gerçekleştirilir gerçek zamanlı alt ağları ve bilgisayar tarafından kullanılan uygulama düzeyi protokolleri de dahil olmak üzere her bir aracının için.  Kablo verileri ağ verilerinin uygulama düzeyinde TCP Aktarım katmanında bakan aşağı değil.  Çözüm, tek tek ack'lerini gösteriyor ve SYN istekleri gibi görünmüyor.  Anlaşma tamamlandığında, canlı bağlantı olarak kabul ve bağlı işaretlenmiş. Her iki yuvada kabul sürece bağlantı Canlı kalır açık olduğundan ve veri ve geriye geçirebilirsiniz.  Bağlantının her iki tarafında kapatır sonra bağlantı kesildi işaretlenir.  Bu nedenle, onu yalnızca sayar başarıyla tamamlanan paketlerin bant genişliği, onu bildirmez yeniden gönderir veya paket başarısız oldu.

Kullandıysanız [sFlow](http://www.sflow.org/) veya diğer yazılımlarla [Cisco'nın NetFlow Protokolü](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), sonra da veri kablo verileri görmek ve istatistikleri size tanıdık gelecektir.

Yerleşik günlük arama sorguları türlerinden bazıları şunlardır:

- Kablo verileri sağlayan aracılar
- Kablo verileri sağlayan aracıların IP adresi
- IP adresleriyle giden iletişimler
- Uygulama protokolleri tarafından gönderilen bayt sayısı
- Bir uygulama hizmeti tarafından gönderilen bayt sayısı
- Farklı protokoller tarafından alınan bayt
- Gönderilen ve IP sürümü tarafından alınan toplam bayt sayısı
- Güvenilir bir şekilde ölçülen bağlantıları için ortalama gecikme süresi
- Bilgisayar başlatan veya alan ağ trafiğinin işler
- Bir işlem için ağ trafiği miktarı

Kablo verileri kullanarak arama yaparken, filtre uygulayabilirsiniz ve üst protokolleri ve üst aracıları hakkındaki bilgileri görüntülemek için Grup verileri. Veya ne zaman görüntüleyebilirsiniz birbirleri ile nasıl için uzun iletilen belirli bilgisayarlara (IP adresleri/MAC adresleri) ve ne kadar veri gönderildiği — temel olarak, arama tabanlı ağ trafiğiyle ilgili meta verileri görüntüleyin.

Meta verileri görüntülediğiniz olduğundan, ancak bunu ayrıntılı sorun giderme için mutlaka kullanışlı değildir. Günlük analizi kablo verileri ağ verilerin tam bir yakalama değil.  Paket düzeyinde ayrıntılı sorun giderme için tasarlanmamıştır. Diğer koleksiyon yöntemlerine göre bu aracı kullanmanın avantajı, uygulamaları yüklemek, ağ anahtarlarını yeniden yapılandırma veya karmaşık yapılandırmalar yeniden gerekmemesidir. Kablo verileri yalnızca Aracısı tabanlı — aracıyı bir bilgisayara yükleyin ve kendi ağ trafiğini izlemek. Başka bir avantajı, bulut sağlayıcılarının veya barındırma hizmeti sağlayıcısı veya kullanıcı doku katman burada sahip değil, Microsoft Azure çalışan iş yüklerini izlemek istediğiniz durumdur.

## <a name="connected-sources"></a>Bağlı kaynaklar

Kablo verileri Microsoft bağımlılık Aracısı'ndan verileri alır. Günlük analizi için kendi bağlantıları için OMS aracısının bağımlılık Aracısı'nı bağlıdır. Bu, bir sunucuya yüklenir ve yapılandırılır. ilk OMS Aracısı olmalıdır ve bağımlılık Aracısı'nı yükledikten sonra anlamına gelir. Aşağıdaki tabloda kablo verileri çözümünü destekler bağlı kaynakları açıklanmaktadır.

| **Bağlı kaynak** | **Destekleniyor** | **Açıklama** |
| --- | --- | --- |
| Windows aracıları | Evet | Kablo verileri analiz eder ve Windows Aracısı bilgisayarlardan verileri toplar. <br><br> Ek olarak [OMS Aracısı](log-analytics-windows-agents.md), Windows aracıları Microsoft bağımlılık Aracısı gerektirir. Bkz: [desteklenen işletim sistemleri](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) işletim sistemi sürümleri tam bir listesi. |
| Linux aracıları | Evet | Kablo verileri analiz eder ve Linux Aracısı bilgisayarlardan verileri toplar.<br><br> Ek olarak [OMS Aracısı](log-analytics-quick-collect-linux-computer.md), Linux aracılarını Microsoft bağımlılık Aracısı gerektirir. Bkz: [desteklenen işletim sistemleri](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) işletim sistemi sürümleri tam bir listesi. |
| System Center Operations Manager yönetim grubu | Evet | Kablo verileri analiz eder ve Windows ve Linux aracıları bağlı bir veri toplar [System Center Operations Manager yönetim grubu](log-analytics-om-agents.md). <br><br> System Center Operations Manager Aracısı bilgisayardan doğrudan bağlantı günlük analizi için gereklidir. Veri yönetim grubu için günlük analizi iletilir. |
| Azure depolama hesabı | Hayır | Bu yüzden, Azure depolama biriminden toplamak için hiçbir veri kablo verileri Aracısı bilgisayarlardan verileri toplar. |

Windows, Microsoft İzleme Aracısı'nı (MMA) hem System Center Operations Manager hem de günlük analizi toplamak ve veri göndermek için kullanılır. Bağlamın bağlı olarak, aracıyı System Center Operations Manager Aracısı, OMS Aracısı, günlük analizi Aracısı, MMA veya doğrudan Aracısı çağrılır. System Center Operations Manager ve günlük analizi MMA biraz farklı sürümleri sağlar. Bu sürümleri her System Center Operations Manager, günlük analizi için veya her ikisine de bildirebilirsiniz.

Linux üzerinde Linux için OMS aracısının toplar ve günlük analizi için verileri gönderir. Kablo verileri OMS doğrudan aracılarıyla sunucularda veya günlük analizi için System Center Operations Manager Yönetim grupları bağlı olan sunucuları kullanabilirsiniz.

Bu makalede, tüm aracılar için başvuran olup olmadığını Linux veya Windows, System Center Operations Manager yönetim grubuna bağlı veya doğrudan Log Analytics'e bağlama olarak ifade edilmektedir _OMS Aracısı_. Bağlam için yalnızca ihtiyaç duyduğunuzda aracısının belirli dağıtım adı kullanacağız.

Bağımlılık Aracısı'nı tüm verileri aktarmaz ve güvenlik duvarları veya bağlantı noktalarını herhangi bir değişiklik gerektirmez. Kablo verileri, her zaman günlük analizi için OMS aracısı tarafından ya da doğrudan verinin veya OMS ağ geçidini kullanma.

![Aracı diyagramı](./media/log-analytics-wire-data/agents.png)

Günlük analizi için bağlı yönetim grubu ile System Center Operations Manager kullanıcı varsa:

- System Center Operations Manager aracıları günlük Analizi'ne bağlanmak için Internet erişebiliyorsa ek yapılandırma gerekli değildir.
- System Center Operations Manager aracıları Internet üzerinden günlük analizi erişemediğinde System Center Operations Manager ile çalışmak için OMS ağ geçidi yapılandırmanız gerekir.

Doğrudan aracı kullanıyorsanız, günlük analizi veya OMS ağ geçidine bağlanmak için OMS aracısının kendisi yapılandırmanız gerekir. OMS ağ geçidi'nden indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Ön koşullar

- Gerektirir [Insight and Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) çözümü sunar.
- Kablo verileri çözüm'ın önceki sürümünü kullanıyorsanız, önce onu kaldırmanız gerekir. Ancak, özgün kablo verileri çözüm yakalanan tüm veriler kablo verileri 2.0 ve günlük arama hala mevcut değil.
- Yüklemek veya bağımlılık Aracısı'nı kaldırmak için yönetici ayrıcalıkları gerekir.
- Bağımlılık Aracısı'nı bir 64-bit işletim sistemine sahip bir bilgisayara yüklenmesi gerekir.

### <a name="operating-systems"></a>İşletim sistemleri

Aşağıdaki bölümlerde bağımlılık aracısı için desteklenen işletim sistemleri listelenmektedir. Kablo verileri 32-bit mimariler için herhangi bir işletim sistemini desteklemiyor.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows masaüstü

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux ve Oracle Linux (ile RHEL çekirdek)

- Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
- PAE ve Xen, desteklenmez için tüm Linux dağıtım gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle _2.6.16.21-0.8-xen_ desteklenmiyor.
- Standart çekirdekleri yeniden derlemelerinin dahil olmak üzere özel çekirdekleri desteklenmez.
- CentOSPlus çekirdek desteklenmiyor.
- Oracle kesilemeyen kurumsal çekirdek (UEK), bu makalenin sonraki bölümlerde ele alınmıştır.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux kesilemeyen kurumsal çekirdek ile

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Bağımlılık Aracısı indirir

| **Dosya** | **İŞLETİM SİSTEMİ** | **Sürüm** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Yapılandırma

Alanlarınızı kablo verileri çözümü yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Etkinlik günlük analizi çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Bağımlılık aracısı veri almak istediğiniz her bilgisayara yükleyin. Her bilgisayarda bir aracı gerekmeyebilir şekilde bağımlılık Aracısı'nı hemen Komşuları bağlantıları izleyebilirsiniz.

### <a name="install-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık Aracısı'nı yükleme

Yüklemek veya aracıyı kaldırmak için yönetici ayrıcalıkları gerekir.

Bağımlılık Aracısı'nı InstallDependencyAgent Windows.exe Windows çalıştıran bilgisayarlara yüklenir. Bu yürütülebilir dosya seçenekleri olmadan çalıştırırsanız, etkileşimli olarak yüklemek için izleyebileceğiniz Sihirbazı'nı başlatır.

Windows çalıştıran her bilgisayarda bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1. İçindeki adımları izleyerek OMS Aracısı [ortamınızda barındırılan Windows bilgisayarlarından verileri toplama](log-analytics-windows-agents.md).
2. Önceki bölümde bağlantıyı kullanarak Windows bağımlılık aracısı yükleyin ve sonra aşağıdaki komutu kullanarak çalıştırın:`InstallDependencyAgent-Windows.exe`
3. Aracıyı yüklemek için sihirbazı izleyin.
4. Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Windows aracılar için %Programfiles%\Microsoft bağımlılık Agent\logs günlük dizindir.

#### <a name="windows-command-line"></a>Windows komut satırı

Bir komut satırından yüklemek için aşağıdaki tablodaki seçenekleri kullanın. Yükleme bayrakları listesini görmek için yükleyiciyi kullanarak çalıştırmak /? aşağıdaki gibi bayrak.

InstallDependencyAgent Windows.exe /?

| **Bayrağı** | **Açıklama** |
| --- | --- |
| <code>/?</code> | Komut satırı seçeneklerinin listesini alın. |
| <code>/S</code> | Kullanıcı etkileşimi ile sessiz bir yükleme gerçekleştirin. |

Dosyalar Windows bağımlılık aracısı için varsayılan olarak C:\Program Files\Microsoft bağımlılık Aracısı yerleştirilir.

### <a name="install-the-dependency-agent-on-linux"></a>Linux'ta bağımlılık Aracısı'nı yüklemek

Kök erişimi yüklemek veya aracıyı yapılandırmak için gereklidir.

Bağımlılık Aracısı'nı InstallDependencyAgent Linux64.bin, kendiliğinden açılan bir ikili içeren bir kabuk betiği aracılığıyla Linux bilgisayarlara yüklenir. Kullanarak dosya çalıştırabilirsiniz _paylaş_ veya ekleme yürütme dosya izinlerinin.

Her bir Linux bilgisayarda bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1. İçindeki adımları izleyerek OMS Aracısı [ortamınızda barındırılan Linux bilgisayarlardan verileri toplama](log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key).
2. Önceki bölümde bağlantıyı kullanarak Linux bağımlılık aracıyı indirin ve ardından aşağıdaki komutu kullanarak kök olarak yükleyin: Paylaş InstallDependencyAgent Linux64.bin
3. Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları, günlük dizini:: /var/opt/microsoft/dependency-agent/log.

Yükleme bayrakları listesini görmek için yükleme programla çalıştırma `-help` gibi bayrak.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Bayrağı** | **Açıklama** |
| --- | --- |
| <code>-help</code> | Komut satırı seçeneklerinin listesini alın. |
| <code>-s</code> | Kullanıcı etkileşimi ile sessiz bir yükleme gerçekleştirin. |
| <code>--check</code> | İzinler ve işletim sistemi denetle, ancak aracı yüklemeyin. |

Bağımlılık aracısı için dosyalar aşağıdaki dizinlerindeki yerleştirilir:

| **Dosyaları** | **Konum** |
| --- | --- |
| Çekirdek dosyaları | /OPT/Microsoft/Dependency-Agent |
| Günlük dosyaları | /var/OPT/Microsoft/Dependency-Agent/log |
| Yapılandırma dosyaları | /etc/OPT/Microsoft/Dependency-Agent/config |
| Hizmet yürütülebilir dosyalar | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br><br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| İkili depolama dosyaları | /var/OPT/Microsoft/Dependency-Agent/Storage |

### <a name="installation-script-examples"></a>Yükleme komut dosyası örnekleri

Kolayca bağımlılık Aracısı pek çok sunucu üzerinde aynı anda dağıtmak için bir komut dosyası kullanmak yararlı olur. Aşağıdaki komut örnekleri indirin ve Windows ya da Linux bağımlılık Aracısı'nı yüklemek için kullanabilirsiniz.

#### <a name="powershell-script-for-windows"></a>Windows PowerShell Betiği

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Linux için kabuk komut dosyası

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması

Bağımlılık Aracısı istenen durum yapılandırması üzerinden dağıtmak için xPSDesiredStateConfiguration modülü ve biraz kod aşağıdaki gibi kullanabilirsiniz:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-the-dependency-agent"></a>Bağımlılık Aracısı'nı kaldırma

Bağımlılık Aracısı'nı kaldırmanıza yardımcı olması için aşağıdaki bölümlere bakın.

#### <a name="uninstall-the-dependency-agent-on-windows"></a>Windows üzerinde bağımlılık Aracısı'nı kaldırın

Bir yönetici için bağımlılık Aracısı Windows Denetim Masası'ndan kaldırabilirsiniz.

Bir yönetici, %Programfiles%\Microsoft Agent\Uninstall.exe bağımlılık Aracısı'nı kaldırmak için bağımlılık olarak da çalıştırabilirsiniz.

#### <a name="uninstall-the-dependency-agent-on-linux"></a>Linux üzerinde bağımlılık Aracısı'nı kaldırın

Bağımlılık Aracısı'nı Linux tamamen kaldırmak için aracı ve otomatik olarak bir aracı yüklü bağlayıcı kaldırmanız gerekir. Aşağıdaki komutu kullanarak her ikisi de kaldırabilirsiniz:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Yönetim paketleri

Kablo verileri bir günlük analizi çalışma alanındaki etkinleştirildiğinde, 300-KB Yönetim Paketi bu çalışma alanındaki tüm Windows sunucularına gönderilir. System Center Operations Manager aracıları kullanıyorsanız, bir [bağlı yönetim grubu](log-analytics-om-agents.md), bağımlılık İzleyicisi Yönetim Paketi System Center Operations Manager'dan dağıtılır. Aracıları doğrudan bağlıysanız, günlük analizi Yönetim Paketi sunar.

Yönetim Paketi Microsoft.IntelligencePacks.ApplicationDependencyMonitor olarak adlandırılır. Öğesine yazılır: %Programfiles%\Microsoft izleme Agent\Agent\Health hizmet State\Management paketleri. Yönetim Paketi kullanan veri kaynağı: % Program files%\Microsoft izleme Agent\Agent\Health hizmet State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-the-solution"></a>Çözümü kullanma

**Yükleme ve çözüm yapılandırılıyor**

Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

- Kablo verileri çözüm Windows Server 2012 R2, Windows 8.1 ve sonraki işletim sistemleri çalıştıran bilgisayarların verilerini alır.
- Kablo verileri almak istediğiniz bilgisayarlarda Microsoft .NET Framework 4.0 veya sonraki sürümü gerekir.
- Kablo verileri çözümü açıklanan işlemi kullanarak günlük analizi çalışma alanınıza ekleyin [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Başka bir yapılandırma işlemi gerekmez.
- Kablo verileri belirli bir çözüm için görüntülemek istiyorsanız, zaten alanınıza eklenen çözüm gerekir.

Aracıları yüklü olan ve çözüm yükledikten sonra çalışma alanınızda kablo verileri 2.0 kutucuğu görüntülenir.

> [!NOTE]
> Şu anda, kablo verileri görüntülemek için OMS Portalı'nı kullanmanız gerekir. Kablo verileri görüntülemek için Azure Portalı'nı kullanamazsınız.

![Kablo verileri döşeme](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>Kablo verileri 2.0 çözümünü kullanarak

OMS Portalı'nda tıklatın **kablo verileri 2.0** döşeme kablo verileri panosunu açın. Pano Kanatlar aşağıdaki tabloda içerir. Her dikey penceresinde belirtilen kapsam ve zaman aralığı için o dikey 's ölçütlerle eşleşen en fazla 10 öğeleri listeler. Tıklayarak tüm kayıtları döndüren bir günlük arama çalıştırabilirsiniz **tümünü görmek** alt dikey veya dikey başlığını tıklatarak.

| **Dikey penceresi** | **Açıklama** |
| --- | --- |
| Ağ trafiğini yakalayan aracılar | Ağ trafiğini yakalayan aracılar sayısını gösterir ve trafiği yakalama üst 10 bilgisayarları listeler. Günlük aramasını çalıştırmak üzere numarasını tıklatın <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>. Toplam Yakalanan bayt sayısı döndüren bir günlük arama çalıştırmak için listedeki bir bilgisayar'ı tıklatın. |
| Yerel alt ağları | Aracıları bulunmuş yerel alt ağları sayısını gösterir.  Günlük aramasını çalıştırmak üzere numarasını tıklatın <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> , listeleriyle tüm alt ağların her biri gönderilen bayt sayısı. Alt ağ gönderilen bayt sayısı toplam döndüren bir günlük arama çalıştırmak için listedeki bir alt ağ'ı tıklatın. |
| Uygulama düzeyi protokolleri | Uygulama düzeyi protokolleri sayısını kullanımda, aracıları tarafından bulunan olarak gösterir. Günlük aramasını çalıştırmak üzere numarasını tıklatın <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>. Toplam protokolü kullanılarak gönderilen bayt sayısı döndüren bir günlük arama çalıştırmak için bir iletişim kuralı'ı tıklatın. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Kablo verileri Panosu](./media/log-analytics-wire-data/wire-data-dash.png)

Kullanabileceğiniz **ağ trafiğini yakalayan aracılar** bilgisayarlar tarafından kullanılan ağ bant genişliğini belirlemek için dikey. Bu dikey kolayca bulmanıza yardımcı olabilir _chattiest_ bilgisayar ortamınızdaki. Bu bilgisayarlar, anormal olarak davranan veya normalden daha fazla ağ kaynaklarını kullanan aşırı yüklenmiş.

![Günlük arama örneği](./media/log-analytics-wire-data/log-search-example01.png)

Benzer şekilde, kullanabileceğiniz **yerel alt ağları** ne kadar ağ trafiğini, alt ağlar taşıma olduğunu belirlemek için dikey. Kullanıcıların uygulamaları için kritik alanları geçici alt ağlar genellikle tanımlayın. Bu dikey bu alanlara görünüme sunar.

![Günlük arama örneği](./media/log-analytics-wire-data/log-search-example02.png)

**Uygulama düzeyi protokolleri** dikey yararlıdır yardımcı olduğundan ne protokolleri kullanımda olduğunu biliyor. Örneğin, ağ ortamınızda kullanımda olmaması için SSH bekleyebilirsiniz. Dikey penceresinde kullanılabilir bilgilerini görüntüleme hızlı bir şekilde Onayla veya, Beklenti disprove.

![Günlük arama örneği](./media/log-analytics-wire-data/log-search-example03.png)

Bu örnekte, hangi bilgisayarların SSH ve diğer birçok iletişim ayrıntılarını kullanmakta olduğunu görmek için-ayrıntıya SSH ayrıntıları alınamadı.

![Paylaş arama sonuçları](./media/log-analytics-wire-data/ssh-details.png)

Protokol trafiğini artırarak veya zamanla azaltarak olmadığını bilmek yararlıdır. Bir uygulama tarafından aktarılan veri miktarını artırmayı, örneğin, bilincinde olmanız gereken bir şey olması veya dikkate değer bulabileceğiniz emin olabilir.

## <a name="input-data"></a>Giriş verisi

Kablo verileri etkinleştirdiğiniz aracıları kullanarak ağ trafiğini hakkındaki meta verileri toplar. Her bir aracının 15 dakikada hakkındaki verileri gönderir.

## <a name="output-data"></a>Çıktı verileri

Türüne sahip bir kayıt _WireData_ her giriş veri türü için oluşturulur. WireData kayıtları aşağıdaki tabloda gösterilen özelliklere sahiptir:

| Özellik | Açıklama |
|---|---|
| Bilgisayar | Burada veri toplanan bilgisayar adı |
| TimeGenerated | Kayıt zamanı |
| Yerel IP | Yerel bilgisayarın IP adresi |
| SessionState | Bağlantısı kesilmiş veya bağlı |
| ReceivedBytes | Alınan bayt miktarı |
| ProtocolName | Kullanılan ağ protokolün adı |
| İpversion değeri | IP sürümü |
| Yön | Gelen veya giden |
| MaliciousIP | Bilinen kötü amaçlı bir kaynak IP adresi |
| Önem Derecesi | Amaçlı olduğundan kuşkulanılan yazılımın önem derecesi |
| RemoteIPCountry | Ülke / bölge uzak IP adresi |
| ManagementGroupName | Operations Manager yönetim grubunun adı |
| SourceSystem | Burada veri toplanan kaynak |
| SessionStartTime | Oturum başlangıç saati |
| SessionEndTime | Oturumun bitiş saati |
| LocalSubnet | Burada veri toplanan alt ağ |
| LocalPortNumber | Yerel bağlantı noktası numarası |
| Uzak IP | Uzak bilgisayar tarafından kullanılan uzak IP adresi |
| RemotePortNumber | Uzak IP adresiyle kullanılan bağlantı noktası numarası |
| SessionID | İki IP adresi arasındaki iletişimi oturumunu tanımlayan benzersiz bir değer |
| SentBytes | Gönderilen bayt sayısı |
| TotalBytes | Toplam oturum sırasında gönderilen bayt sayısı |
| ApplicationProtocol | Kullanılan ağ protokol türü   |
| İşlem kimliği | Windows işlem kimliği |
| İşlem adı | İşlemin yolu ve dosya adı |
| RemoteIPLongitude | IP boylam değeri |
| RemoteIPLatitude | IP enlem değeri |


## <a name="next-steps"></a>Sonraki adımlar

- [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı kablo veri arama kayıtları görüntülemek için.
