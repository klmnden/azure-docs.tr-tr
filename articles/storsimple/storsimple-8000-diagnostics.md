---
title: StorSimple 8000 cihaz sorunlarını gidermek için tanılama aracı | Microsoft Docs
description: StorSimple cihaz modları ve cihaz modunu değiştirmek için StorSimple için Windows PowerShell'i kullanmayı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 5cce4337e3ef95c6407d46d9b8b6401fe4f6600b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576195"
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>8000 serisi cihaz sorunlarını gidermek için StorSimple Tanılama aracını kullanma

## <a name="overview"></a>Genel Bakış

StorSimple Tanılama aracı, sistem, performansı, ağ ve donanım bileşeni sistem durumu için StorSimple cihaz sorunları tanılar. Tanılama aracı, çeşitli senaryolarda kullanılabilir. Bu senaryolar, planlama, bir StorSimple cihazını dağıtma, ağ ortamı değerlendiriliyor ve işletimsel bir cihaz performansını belirleme iş yükü içerir. Bu makalede tanılama aracına genel bakış sağlar ve aracı ile StorSimple cihazını nasıl kullanılabileceğini anlatmaktadır.

Tanılama aracı, birincil olarak StorSimple 8000 serisi şirket içi cihazlar için (8100 ve 8600) yöneliktir.

## <a name="run-diagnostics-tool"></a>Tanılama aracını çalıştırın

Bu araç, StorSimple cihazınızın Windows PowerShell arabirimi üzerinden çalıştırılabilir. Cihazınızın yerel arabirim erişmenin iki yolu vardır:

* [Cihaz seri konsoluna bağlanmak için PuTTY kullanın](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [Windows PowerShell aracılığıyla aracı için StorSimple uzaktan erişim](storsimple-8000-remote-connect.md).

Bu makalede, cihaz seri konsoluna PuTTY üzerinden bağlanmış olduğunuz varsayılır.

#### <a name="to-run-the-diagnostics-tool"></a>Tanılama aracını çalıştırmak için

Cihazın Windows PowerShell arabirimine bağlandıktan sonra Bu cmdlet'i çalıştırmak için aşağıdaki adımları gerçekleştirin.
1. Cihaz seri konsoluna adımları izleyerek oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Aşağıdaki komutu yazın:

    `Invoke-HcsDiagnostics`

    Kapsam parametresi belirtilmezse, cmdlet tüm tanılama testleri yürütür. Bu testler, sistem, donanım bileşeni sistem durumu, ağ ve performans içerir. 
    
    Yalnızca belirli bir testi çalıştırmak için kapsam parametresi belirtin. Örneğin, yalnızca ağ testi çalıştırmak için yazın

    `Invoke-HcsDiagnostics -Scope Network`

3. Seçin ve daha fazla analiz için bir metin dosyasına PuTTY penceresinden çıktısını kopyalayın.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Tanılama aracını kullanma senaryoları

Sistem, ağ, performans, sistem ve donanım sistem durumu sorunlarını gidermek için Tanılama aracını kullanın. Bazı olası senaryolar şunlardır:

* **Cihaz çevrimdışı** -bilgisayarınızı StorSimple 8000 serisi cihaz çevrimdışı. Ancak, Windows PowerShell arabiriminden her iki denetleyici ve çalışıyor olduğunu hatırlıyorum.
    * Ardından ağ durumu belirlemek için bu aracı kullanabilirsiniz.
         
         > [!NOTE]
         > Bu araç, bir cihaz kayıt (veya Kurulum Sihirbazı aracılığıyla yapılandırma) önce performans ve ağ ayarlarını değerlendirmek için kullanmayın. Geçerli bir IP, cihaz için Kurulum Sihirbazı'nı ve kayıt sırasında atanır. Bu cmdlet, kayıtlı değil, bir cihaz donanımı sistem durumunu ve sistem üzerinde çalıştırabilirsiniz. Kapsam parametresi, örneğin kullanın:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Sürekli cihaz sorunları** -kalıcı hale getirmek için görünen cihaz sorunları yaşıyoruz. Örneğin, kayıt başarısız oluyor. Cihaz bir süredir başarıyla kaydedilmiş ve işletimsel olduktan sonra ayrıca cihaz sorunları yaşıyor olabilir.
    * Bu durumda, bir hizmet isteği Microsoft Support oturum önce ön gidermek için bu aracı kullanın. Bu aracı çalıştırın ve bu aracın çıktısının yakalama öneririz. Bu çıkış, ardından sorun giderme hızlandırmak için destek sağlayabilirsiniz.
    * Donanım bileşeni veya küme hataları varsa, bir destek isteği oturum açmanız gerekir.

* **Cihaz performansı düşük** -bilgisayarınızı StorSimple cihaz yavaş.
    * Bu durumda, bu cmdlet için performans ayarlama, kapsam parametresiyle çalıştırın. Çıkış analiz edin. Okuma-yazma gecikmeleri bulut olursunuz. En fazla ulaşılabilir hedefi olarak raporlanan gecikme süreleriyle kullanmak, iç veri işleme için bazı ek yüküne faktörü ve iş yükleri sistem üzerindeki dağıtabilirsiniz. Daha fazla bilgi için Git [cihaz performans sorunlarını gidermek için ağ testi kullanın](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Tanılama testi ve örnek çıktı

### <a name="hardware-test"></a>Donanım test

Bu test, donanım bileşenleri, USM üretici yazılımı ve disk üretici yazılımı sisteminizde çalışan durumunu belirler.

* Bildirilen donanım bileşenleri testin başarısız veya sistemde olmayan bu bileşenlerdir.
* USM üretici yazılımı ve disk üretici yazılımı sürümlerine, denetleyici 0, denetleyici 1 için bildirilen ve sisteminizdeki paylaşılan bileşenleri. Donanım bileşenleri tam listesi için şuraya gidin:

    * [Birincil bileşenleri](storsimple-8000-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [EBOD muhafazası bileşenleri](storsimple-8000-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Donanım test arızalı bileşenlere bildirirse [Microsoft Support olan hizmet isteği oturum](storsimple-8000-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>Örnek çıktı donanım testin bir 8100 cihazda çalıştırma

Bir StorSimple 8100 CİHAZDAN örnek çıktı aşağıda verilmiştir. 8100 model aygıt EBOD muhafazası mevcut değil. Bu nedenle, EBOD denetleyicisi bileşenleri raporlanmaz.

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a>Sistem Testi

Bu test, sistem bilgisi, güncelleştirmeleri, küme bilgilerini ve cihazınız için hizmet bilgileri raporlar.

* Model, cihaz seri numarası, saat dilimi, denetleyici durumu ve sistem üzerinde çalışan ayrıntılı yazılım sürüm sistem bilgilerini içerir. Çıktı olarak bildirilen çeşitli sistem parametreleri anlamak için Git [sistem bilgisi yorumlanırken](#appendix-interpreting-system-information).

* Güncelleştirme kullanılabilirlik normal ve Bakım modu kullanılabilir olup olmadığını bildirir ve ilişkili paket adları. Varsa `RegularUpdates` ve `MaintenanceModeUpdates` olan `false`, bu güncelleştirmeleri kullanılamaz olduğunu belirtir. Cihazınız güncel.
* Küme bilgilerini, tüm HCS küme gruplarını ve bunların ilgili durumlarını çeşitli mantıksal bileşenler hakkında bilgiler içerir. Bu bölümde, raporun bir çevrimdışı küme grubu görürseniz [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).
* Hizmet bilgileri, cihaz üzerinde çalışan tüm HCS ve CIS hizmetlerinin durumları ve adlarını içerir. Bu bilgiler Microsoft Support cihaz sorunun giderilmesine yardımcı olur.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Örnek çıktı sistemi testin bir 8100 cihazda çalıştırma

Sistem test bir 8100 cihazında çalıştırmasının örnek çıktı aşağıda verilmiştir.

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_service        Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a>Ağ testi

Bu test, ağ arabirimleri, bağlantı noktaları, DNS ve NTP sunucu bağlantısı, SSL sertifikası, depolama hesabı kimlik bilgileri, Update sunucularına bağlantı ve StorSimple cihazınızdaki web proxy bağlantı durumunu doğrular.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Yalnızca DATA0 etkinleştirildiğinde test ağının örnek çıktısı

8100 model Cihazınızı, örnek çıktı aşağıda verilmiştir. Çıkışta gördüğünüz:
* Yalnızca DATA 0 ve 1 DATA ağ arabirimleri etkinleştirilmiş ve yapılandırılmış.
* 2-5 verileri portalda etkin değil.
* DNS sunucusu yapılandırması geçerli değil ve cihaz, DNS sunucusu bağlanabilirsiniz.
* NTP sunucusu bağlantısı da uygundur.
* 80 ve 443 bağlantı noktaları açıktır. Ancak, 9354 bağlantı noktasının engellenir. Temel [sistem ağ gereksinimleri](storsimple-system-requirements.md), service bus iletişimi için bu bağlantı noktası açmanız gerekir.
* SSL sertifikası geçerli değil.
* Cihazın depolama hesabına bağlanabildiğini: _myss8000storageacct_.
* Update sunucularına bağlantı geçerli değil.
* Web Ara sunucusu, bu cihaz üzerinde yapılandırılmamış.

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a>Örnek çıktı DATA0 ve Veri1 etkin olduğunda ağ testi

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a>Performans testi

Bu test, cihazınız için bulut okuma-yazma gecikmeleri aracılığıyla bulut performans bildirir. Bu araç, StorSimple ile elde edebileceğiniz bulut performansının taban çizgisini oluşturmak için kullanılabilir. Aracı, bağlantınız için size en yüksek performans (okuma-yazma gecikme için en iyi Durum senaryosu) bildirir.

En fazla ulaşılabilir performans aracın bildirdiği olarak bildirilen okuma-yazma gecikme hedefleri olarak iş yüklerini dağıtırken kullanabiliriz.

Test cihazdaki farklı bir birime türleriyle ilişkili blob boyutları benzetimini yapar. Normal katmanlı ve yerel olarak sabitlenmiş birimlerin yedekleri 64 KB blob boyutu kullanın. Blob veri boyutu 512 KB katmanlı birimlerin işaretli arşiv seçeneğiyle kullanın. Cihazınız varsa, katmanlı ve yerel olarak sabitlenmiş birimleri yapılandırılmış, yalnızca 64 KB blob'a veri boyutu çalıştırılan karşılık gelen test.

Bu aracı kullanmak için aşağıdaki adımları gerçekleştirin:

1.  İlk olarak, katmanlı birimlerin ve katmanlı birimlerin bir karışımını işaretli arşivlenmiş seçeneği ile oluşturun. Bu eylem aracı testleri 64 KB ve 512 KB için blob boyutları çalıştırmasını sağlar.

2. Oluşturulan ve birimler yapılandırılmış sonra cmdlet'ini çalıştırın. Şunu yazın:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Araç tarafından bildirilen okuma-yazma gecikmeleri not edin. Bu test sonuçları raporlamadan önce çalıştırmak için birkaç dakika sürebilir.

4. Bağlantı gecikmeleri tüm beklenen aralığın altında ise sonra araç tarafından raporlanan gecikme süreleri en fazla ulaşılabilir hedef olarak iş yüklerini dağıtırken kullanılabilir. İç veri işleme için bazı ek yüküne faktörü.

    Tanılama aracı tarafından raporlanan okuma-yazma gecikme süresi yüksek ise:

    1. Depolama blob Hizmetleri için yapılandırın ve Azure depolama hesabı için gecikme anlamak için çıkış analiz edin. Ayrıntılı yönergeler için Git [etkinleştirme ve depolama analizi yapılandırma](../storage/common/storage-enable-and-view-metrics.md). Ardından bu gecikme süreleri de yüksek ve StorSimple Tanılama Aracı alınan sayılara karşılaştırılabilir varsa, Azure depolama ile bir hizmet isteği bağlanmanız gerekir.

    2. Depolama hesabı gecikme süresi düşük, ağınızda herhangi bir gecikme sorunlarını araştırmak için ağ yöneticinize başvurun.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Örnek çıktı performans testinin bir 8100 cihazda çalıştırma

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a>Ek: Sistem bilgisi yorumlanırken

Çeşitli Windows PowerShell parametreleri sistem bilgileri eşlenecek bir tablo aşağıda verilmiştir. 

| PowerShell parametresi    | Açıklama  |
|-------------------------|------------------|
| Örnek Kimliği             | Her denetleyici benzersiz bir tanımlayıcı sahip veya bu bir GUID ile ilişkili.|
| Ad                    | Azure portalı üzerinden cihaz dağıtım sırasında yapılandırdığınız cihaz kolay adı. Varsayılan kolay adı, cihaz seri numarasını şeklindedir. |
| Model                   | StorSimple 8000 serisi Cihazınızı modeli. Model 8100 veya 8600 olabilir.|
| seri numarası            | Cihaz seri numarasını bir fabrikada atanır ve 15 karakterden uzun. Örneğin, 8600 SHX0991003G44HT gösterir:<br> 8600 – cihaz modelidir.<br>Üretim sitesini SHX – var.<br> 0991003 - belirli bir ürün olan. <br> G44HT benzersiz seri numaraları oluşturmak için son 5 rakamlı artırılır-. Bu, bir sıralı küme olmayabilir.|
| TimeZone                | Cihaz saat cihaz dağıtım sırasında Azure portalında yapılandırıldığı şekilde dilimi.|
| CurrentController       | StorSimple cihazınızın Windows PowerShell arabirimi üzerinden bağlıyken denetleyici.|
| ActiveController        | Cihazınızda etkin ve tüm ağ ve disk işlemleri denetleme denetleyici. Bu, denetleyici 0 veya denetleyici 1 olabilir.  |
| Controller0Status       | Denetleyici 0 Cihazınızda durumu. Denetleyici durumu, normal, kurtarma modunda ya da erişilemez olabilir.|
| Controller1Status       | Denetleyici 1 durumu Cihazınızda.  Denetleyici durumu, normal, kurtarma modunda ya da erişilemez olabilir.|
| SystemMode              | StorSimple Cihazınızı genel durumu. Cihaz durumu normal, Bakım veya yetkisi alınmış (Azure portalında devre dışı karşılık gelir).|
| FriendlySoftwareVersion | Cihaz yazılımı sürümüne karşılık gelen kolay dize. Güncelleştirme 4 ile birlikte çalışan bir sistem için StorSimple 8000 serisi güncelleştirme 4.0 kolay yazılım sürümü olacaktır.|
| HcsSoftwareVersion      | HCS yazılım sürümü, cihaz üzerinde çalışan. Örneğin, StorSimple 8000 serisi güncelleştirme 4.0 için karşılık gelen HCS yazılım 6.3.9600.17820 sürümüdür. |
| ApiVersion              | HCS cihazı Windows PowerShell API'si yazılım sürümü.|
| VhdVersion              | Cihaz ile birlikte gelen bir Fabrika görüntüsünden yazılım sürümü. Cihazınızı fabrika ayarlarına sıfırlarsanız, sonra bu yazılım sürümünü çalıştırır.|
| OSVersion               | Cihaz üzerinde çalışan Windows Server işletim sistemi yazılım sürümü. StorSimple cihazı kapatmak için 6.3.9600 karşılık gelen Windows Server 2012 R2 temel alır.|
| CisAgentVersion         | StorSimple Cihazınızda çalışan CIS aracınızı sürümü. Bu aracı, Azure'da çalışan StorSimple Yöneticisi hizmeti ile iletişim kurmasına yardımcı olur.|
| MdsAgentVersion         | StorSimple Cihazınızda çalışan Mds aracısının karşılık gelen sürümü. Bu aracı, izleme ve Tanılama Hizmeti (MDS) için veri taşır.|
| Lsisas2Version          | StorSimple Cihazınızda LSI sürücülere karşılık gelen sürümü.|
| Kapasite                | Cihaz bayt cinsinden toplam kapasitesi.|
| RemoteManagementMode    | Cihaz, Windows PowerShell arabirimi üzerinden uzaktan yönetilebilir olup olmadığını gösterir. |
| FipsMode                | Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) mod'ın Cihazınızda etkin olup olmadığını gösterir. FIPS 140 standardı, hassas verilerin korunması için ABD Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar. Update 4 veya sonraki sürümlerini çalıştıran cihazlar için FIPS modunda varsayılan olarak etkindir. |

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Invoke-HcsDiagnostics cmdlet'inin söz dizimi](https://technet.microsoft.com/library/mt795371.aspx).

* Kullanma hakkında daha fazla bilgi edinin [dağıtım sorunlarını giderme](storsimple-troubleshoot-deployment.md) StorSimple Cihazınızda.
