---
title: "StorSimple 8000 aygıtı sorun giderme için tanılama aracı | Microsoft Docs"
description: "StorSimple cihaz modlarını tanımlar ve StorSimple için Windows PowerShell aygıt modunu değiştirmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 8fae7bb357f8e5e8eff249edfe3a2aaafe04283c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-diagnostics-tool-to-troubleshoot-8000-series-device-issues"></a>8000 serisi cihaz sorunlarını gidermek için StorSimple Tanılama Aracı'nı kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Tanılama Aracı'nı bir StorSimple cihazı için sistem, performans, ağ ve donanım bileşen durumu ile ilgili sorunlar tanılar. Tanılama Aracı'nı çeşitli senaryolarda kullanılabilir. Bu senaryolar, planlama, StorSimple cihazını dağıtma, ağ ortamını değerlendiriliyor ve işletimsel bir aygıtı performansını belirleme iş yükü içerir. Bu makalede Tanılama Aracı'nı genel bir bakış sağlar ve aracı StorSimple cihazı ile nasıl kullanılacağını açıklar.

Tanılama Aracı öncelikle StorSimple 8000 serisi şirket içi cihazlar için (8100 ve 8600) yöneliktir.

## <a name="run-diagnostics-tool"></a>Tanılama aracını çalıştırın

Bu araç, StorSimple Cihazınızı Windows PowerShell arabirimi çalıştırılabilir. Cihazınızı yerel arabiriminin erişmek için iki yol vardır:

* [Cihaz seri konsoluna bağlanmak için PuTTY kullanın](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).
* [StorSimple için Windows PowerShell aracılığıyla aracı uzaktan erişim](storsimple-remote-connect.md).

Bu makalede, cihaz seri konsoluna PuTTY üzerinden bağlı olduğunuzu varsayar.

#### <a name="to-run-the-diagnostics-tool"></a>Tanılama Aracı'nı çalıştırmak için

Aygıt Windows PowerShell arabirimine bağlandıktan sonra cmdlet'i çalıştırmak için aşağıdaki adımları gerçekleştirin.
1. İçindeki adımları izleyerek cihaz seri konsoluna oturum [kullan cihaz seri konsoluna bağlanmak için PuTTY](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).

2. Aşağıdaki komutu yazın:

    `Invoke-HcsDiagnostics`

    Kapsam parametresi belirtilmezse, cmdlet tüm tanılama testleri yürütür. Bu testler, sistem, donanım bileşen durumu, ağ ve performans içerir. 
    
    Yalnızca belirli bir testi çalıştırmak için kapsam parametresi belirtin. Örneğin, yalnızca ağ testi çalıştırmak için şunu yazın

    `Invoke-HcsDiagnostics -Scope Network`

3. Seçin ve daha fazla çözümleme için bir metin dosyasına PuTTY penceresinden çıkış kopyalayın.

## <a name="scenarios-to-use-the-diagnostics-tool"></a>Tanılama Aracı'nı kullanmak için senaryolar

Sistem ağ, performans, sistem ve donanım durumunu gidermek için tanılama aracı kullanın. Bazı olası senaryolar şunlardır:

* **Cihaz çevrimdışı** -bilgisayarınızı StorSimple 8000 serisi cihaz çevrimdışı. Ancak, Windows PowerShell arabiriminden hem denetleyicileri ve çalışıyor olduğunu görünüyor.
    * Ağ durumu belirlemek için bu aracı kullanabilirsiniz.
         
         > [!NOTE]
         > Bu araç, bir cihazda kayıt (veya Kurulum Sihirbazı yapılandırma) önce performans ve ağ ayarlarını değerlendirmek için kullanmayın. Geçerli bir IP cihaz için Kurulum Sihirbazı'nı ve kayıt sırasında atanır. Bu cmdlet, donanım durumunu ve sistem için kayıtlı değil, bir cihaz çalıştırabilirsiniz. Kapsam parametresi, örneğin kullanın:
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* **Sürekli cihaz sorunlarını** -kalıcı göründüğü cihaz sorunları yaşamaktadır. Örneğin, kayıt başarısız oluyor. Cihaz başarıyla kayıtlı ve işletimsel biraz olduktan sonra aynı zamanda aygıt sorunları yaşıyor olabilir.
    * Bu durumda, bir hizmet isteği Microsoft Support oturum açmadan önce ön gidermek için bu aracı kullanın. Bu aracı çalıştırın ve bu aracın çıktısının yakalama öneririz. Bu gibi durumlarda, bu çıkış sonra sorun giderme hızlandırmak için destek sağlayabilirsiniz.
    * Donanım bileşeni veya küme hataları varsa, bir destek isteği oturum açmanız gerekir.

* **Düşük cihaz performans** -bilgisayarınızı StorSimple cihaz yavaş.
    * Bu durumda, bu cmdlet için performans ayarlama kapsam parametresi ile çalıştırın. Çıktı analiz edin. Bulut okuma-yazma gecikmeleri alın. Raporlanan gecikme maksimum ulaşılabilir hedefi olarak kullanmak, iç veri işleme için bazı ek faktörü ve iş yükleri sistem üzerindeki dağıtın. Daha fazla bilgi için Git [aygıt performans sorunlarını gidermek için ağ testi kullanma](#network-test).


## <a name="diagnostics-test-and-sample-outputs"></a>Tanılama test ve örnek çıkış

### <a name="hardware-test"></a>Donanım sınaması

Bu test donanım bileşenleri, USM bellenim ve sisteminizde çalışan disk bellenim durumunu belirler.

* Bildirilen donanım bileşenleri testin başarısız veya sistemde olmayan bu bileşenlerdir.
* USM bellenim ve disk bellenim sürümleri denetleyici 0, denetleyici 1 için bildirilen ve sisteminizdeki paylaşılan bileşenleri. Donanım bileşenleri tam listesi için aşağıdaki adrese gidin:

    * [Birincil muhafazada bileşenleri](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [EBOD muhafazası bileşenleri](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> Donanım sınaması başarısız bileşenleri bildirirse [Microsoft Support olan hizmet isteği oturum](storsimple-contact-microsoft-support.md).

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a>8100 cihazda donanım testi örnek çıktısı

Burada, StorSimple 8100 cihazınız dosyasından örnek çıktı verilmiştir. 8100 model cihaz EBOD muhafazası mevcut değil. Bu nedenle, EBOD denetleyicisi bileşenleri raporlanmaz.

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

### <a name="system-test"></a>Sistem test

Bu test, sistem bilgisi, güncelleştirmelere, küme bilgilerini ve cihazınız için hizmet bilgileri raporlar.

* Model, cihaz seri numarasını, saat dilimi, denetleyici durumu ve sistem üzerinde çalışan ayrıntılı yazılım sürüm sistem bilgilerini içerir. Çıkış olarak bildirilen çeşitli sistem parametreleri anlamak için şu adrese gidin [sistem bilgileri yorumlama](#appendix-interpreting-system-information).

* Normal ve Bakım modu kullanılabilir olup olmadığını güncelleştirme kullanılabilirlik raporlar ve ilişkili paket adları. Varsa `RegularUpdates` ve `MaintenanceModeUpdates` olan `false`, bu güncelleştirmeleri kullanılabilir olmadığını gösterir. Cihazınız güncel olduğundan.
* Küme bilgilerini tüm HCS küme gruplarını ve bunlarla ilgili durumlar çeşitli mantıksal bileşenler hakkında bilgiler içerir. Bu bölümdeki çevrimdışı küme grubu raporunun görürseniz [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).
* Hizmet bilgileri, cihazda çalışan tüm HCS ve CIS Hizmetleri durumları ve adlarını içerir. Bu bilgileri cihaz sorunu gidermeye Microsoft Support yardımcı olur.

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a>Örnek çıktı sistemi test 8100 cihazda çalıştırın

Burada, örnek bir çıktı 8100 bir cihazda çalıştırma sistem testinin verilmiştir.

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
hcs_management_servic         Online HCS Cluster Group
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

### <a name="network-test"></a>Ağ sınaması

Bu test ağ arabirimleri, bağlantı noktaları, DNS ve NTP sunucu bağlantısı, SSL sertifikasını, depolama hesabının kimlik bilgilerini, güncelleştirme sunucularına bağlantısı ve web proxy bağlantısı, StorSimple Cihazınızda durumunu doğrular.

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a>Örnek çıktı ağın test yalnızca DATA0 etkinleştirildiğinde

Burada, örnek bir çıktı 8100 cihazın verilmiştir. Çıktıda görebilirsiniz:
* Yalnızca veri 0 ve 1 DATA ağ arabirimleri etkin ve yapılandırılmış.
* Veri 2-5 Portalı'nda etkin değil.
* DNS sunucu yapılandırması geçerli değil ve aygıt DNS sunucusuna bağlanabilir.
* NTP sunucusu bağlantısı de uygundur.
* 80 ve 443 numaralı bağlantı noktalarını açın. Ancak, 9354 bağlantı engellenir. Temel [sistem ağ gereksinimleri](storsimple-system-requirements.md), hizmet veri yolu iletişimi için bu bağlantı noktası açmanız gerekir.
* SSL sertifika geçerli değil.
* Cihaz depolama hesabına bağlanabilir: _myss8000storageacct_.
* Güncelleştirme sunucularına bağlantısı geçerli değil.
* Web proxy, bu aygıtta yapılandırılmamış.

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

Bu test, cihazınız için bulut okuma-yazma gecikmeleri aracılığıyla bulut performans bildirir. Bu araç, StorSimple ile elde edebilirsiniz bulut performansının taban çizgisini oluşturmak için kullanılabilir. Aracı bağlantınız için alabileceğiniz en yüksek performans (okuma-yazma gecikme için en iyi Durum senaryosu) bildirir.

En yüksek elde edilebilir performans araç raporlar gibi biz raporlanan okuma-yazma gecikme hedefleri olarak iş yüklerini dağıtırken kullanabilirsiniz.

Test cihaza farklı bir birim türleri ile ilişkili blob boyutları benzetimini yapar. Normal katmanlı ve yerel olarak sabitlenmiş birimlerin yedekleri, 64 KB blob boyutu kullanın. Arşiv seçeneği işaretli olan katmanlı birimler 512 KB blob veri boyutu kullanın. Cihazınız varsa, katmanlı ve yerel olarak sabitlenmiş birimleri yapılandırılmış, yalnızca 64 KB blob veri boyutu çalıştırmak için karşılık gelen test.

Bu aracı kullanmak için aşağıdaki adımları gerçekleştirin:

1.  İlk olarak, katmanlı birimleri ve katmanlı birimlerin bir karışımını işaretli arşivlenmiş seçeneğiyle oluşturun. Bu eylem aracı testleri 64 KB ve 512 KB için kabarcık boyutları çalıştırmasını sağlar.

2. Oluşturulan ve birimleri yapılandırdıktan sonra cmdlet'ini çalıştırın. Şunu yazın:

    `Invoke-HcsDiagnostics -Scope Performance`

3. Aracı tarafından raporlanan okuma-yazma gecikmeleri not edin. Bu test sonuçları raporlar önce birkaç dakika sürebilir.

4. Bağlantı gecikmeleri tüm beklenen aralığından varsa, daha sonra aracı tarafından raporlanan gecikme maksimum ulaşılabilir hedef olarak iş yüklerini dağıtırken kullanılabilir. İç veri işleme için bazı ek faktörü.

    Tanılama aracı tarafından raporlanan okuma-yazma gecikmeleri yüksekse:

    1. Storage Analytics blob Hizmetleri için yapılandırma ve Azure depolama hesabı için gecikme anlamak için çıktı analiz edin. Ayrıntılı yönergeler için Git [etkinleştirme ve Storage Analytics yapılandırma](../storage/common/storage-enable-and-view-metrics.md). Bu gecikme, aynı zamanda yüksek ve StorSimple tanılama aracından alınan sayılara karşılaştırılabilir varsa, Azure storage ile bir hizmet isteği bağlanmanız gerekir.

    2. Depolama hesabı gecikmeleri düşükse, ağınızdaki gecikmesi sorunları araştırmak için ağ yöneticinize başvurun.

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a>Örnek çıktı performans testi 8100 cihazda çalıştırın

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

## <a name="appendix-interpreting-system-information"></a>Ek: Sistem bilgileri yorumlama

Burada, hangi çeşitli Windows PowerShell parametreleri için sistem bilgileri eşlemesindeki açıklayan bir tablo verilmiştir. 

| PowerShell parametresi    | Açıklama  |
|-------------------------|------------------|
| Örnek Kimliği             | Benzersiz bir tanımlayıcı her denetleyici yok veya bir GUID ile ilişkili.|
| Ad                    | Azure portalı üzerinden aygıt dağıtımı sırasında yapılandırılan cihaz kolay adı. Varsayılan kolay cihaz seri numarasını adıdır. |
| modeli                   | StorSimple 8000 serisi Cihazınızı modeli. Model 8100 veya 8600 olabilir.|
| seri numarası            | Cihaz seri numarasını fabrikada atanır ve 15 karakterden uzun. Örneğin, 8600 SHX0991003G44HT gösterir:<br> 8600 – aygıt modelidir.<br>SHX – üretim sitesi olur.<br> 0991003 - belirli bir ürün var. <br> G44HT benzersiz seri numaraları oluşturmak için en son 5 rakamlı artırılır-. Bu ardışık olmayabilir.|
| saat dilimi                | Aygıt saat aygıt dağıtımı sırasında Azure Portalı'nda yapılandırılan dilimi.|
| CurrentController       | StorSimple Cihazınızı Windows PowerShell arabirimi üzerinden bağlanan denetleyici.|
| ActiveController        | Cihazınızda etkin olduğunda ve tüm ağ ve disk işlemleri denetleme denetleyicisi. Bu denetleyici 0 veya denetleyici 1 olabilir.  |
| Controller0Status       | Denetleyici 0 aygıtınızda durumu. Denetleyici durum normal, kurtarma modunda veya erişilemiyor olabilir.|
| Controller1Status       | Denetleyici 1 durumunu Cihazınızda.  Denetleyici durum normal, kurtarma modunda veya erişilemiyor olabilir.|
| SystemMode              | StorSimple Cihazınızı genel durumu. Cihaz durumu normal, Bakım veya yetkisi alınmış (Azure Portalı'nda devre dışı karşılık gelir).|
| FriendlySoftwareVersion | Aygıt yazılımı sürümüne karşılık gelen kolay dize. Güncelleştirme 4 ile çalışan bir sistem için kolay yazılım sürümü StorSimple 8000 serisi güncelleştirme 4.0 olacaktır.|
| HcsSoftwareVersion      | HCS yazılım sürümü, cihazda çalışan. Örneğin, StorSimple 8000 serisi güncelleştirme 4.0 için karşılık gelen HCS yazılım 6.3.9600.17820 sürümüdür. |
| ApiVersion              | HCS cihazı Windows PowerShell API'si yazılım sürümü.|
| VhdVersion              | Aygıt ile birlikte gelen Fabrika görüntüsünden yazılım sürümü. Cihazınızı fabrika varsayılan ayarlarına sıfırlamak, sonra bu yazılım sürümü çalışır.|
| OSVersion               | Aygıtta çalışan Windows Server işletim sistemi yazılım sürümü. StorSimple cihazı kapatmak için 6.3.9600 karşılık gelen Windows Server 2012 R2 temel alır.|
| CisAgentVersion         | StorSimple Cihazınızda çalıştıran CIS aracınızı sürümü. Bu aracı, Azure'da çalışan StorSimple Yöneticisi hizmetiyle iletişim yardımcı olur.|
| MdsAgentVersion         | StorSimple Cihazınızda çalışan Mds aracısını karşılık gelen sürüm. Bu aracı izleme ve Tanılama Hizmeti (MDS) verileri taşır.|
| Lsisas2Version          | StorSimple Cihazınızda LSI sürücülere karşılık gelen sürüm.|
| Kapasite                | Cihaz bayt cinsinden toplam kapasitesini.|
| RemoteManagementMode    | Aygıt, Windows PowerShell arabirimi uzaktan yönetilebilir olup olmadığını gösterir. |
| FipsMode                | Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) mod aygıtınızda etkinleştirilip etkinleştirilmediğini gösterir. FIPS 140 standardı, önemli verilerin korunması için BİZE Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar. Güncelleştirme 4 veya üstünü çalıştıran cihazlar için FIPS modunda varsayılan olarak etkindir. |

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Invoke-HcsDiagnostics cmdlet sözdizimi](https://technet.microsoft.com/library/mt795371.aspx).

* Nasıl yapılır hakkında daha fazla bilgi [dağıtım sorunlarını giderme](storsimple-troubleshoot-deployment.md) , StorSimple Cihazınızda.
