---
title: "Günlük analizi VMware izleme çözümünde | Microsoft Docs"
description: "Nasıl VMware izlemesi çözümü günlüklerini yönetmek ve ESXi konakları izlemenize yardımcı olabilir hakkında bilgi edinin."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: magoedte
ms.openlocfilehash: f54d24659ad13aa02462938711482326c5bf763c
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Günlük analizi çözümüne VMware izleme (Önizleme)

![VMware simgesi](./media/log-analytics-vmware/vmware-symbol.png)

Günlük analizi VMware izleme çözümde, Merkezi günlük kaydı ve büyük VMware günlükleri için izleme yaklaşımı oluşturmanıza yardımcı olan bir çözümdür. Bu makalede nasıl sorun giderme, yakalama ve çözümü kullanarak tek bir konumda ESXi konaklarının açıklanmaktadır. Çözümü ile tek bir konumda tüm ESXi konakları için ayrıntılı veriler görebilirsiniz. Üst olay sayısı, durumu ve eğilimleri ESXi ana günlükleri ile sağlanan VM ve ESXi konakları görebilirsiniz. Görüntüleme ve merkezi ESXi ana günlüklerini arama giderebilirsiniz. Ve günlük arama sorgularına dayalı uyarılar oluşturabilirsiniz.

Çözüm ESXi ana hedef OMS Aracısı VM veri göndermek için yerel syslog işlevselliğini kullanır. Bununla birlikte, çözümü hedef VM içinde syslog içine dosyaları yazma değil. OMS Aracısı bağlantı noktası 1514 açar ve bu dinler. Verileri aldıktan sonra OMS Aracısı günlük analizi veri iter.

## <a name="install-and-configure-the-solution"></a>Yükleyin ve yapılandırın
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

* Açıklanan işlemi kullanarak aboneliğinizi VMware izlemesi çözümü eklemek [bir yönetim çözümü eklemek](log-analytics-add-solutions.md#add-a-management-solution).

#### <a name="supported-vmware-esxi-hosts"></a>Desteklenen VMware ESXi konakları
vSphere ESXi ana 5.5 ve 6.0

#### <a name="prepare-a-linux-server"></a>Linux sunucusunu hazırlama
Linux işletim sistemi ESXi ana bilgisayarlardan tüm syslog verileri almak için VM oluşturun. [OMS Linux Aracısı](log-analytics-linux-agents.md) tüm ESXi syslog verileri barındırmak için koleksiyon noktasıdır. Günlükleri aşağıdaki örnekteki gibi tek bir Linux sunucusuna iletmek için birden çok ESXi konakları kullanabilirsiniz.  

   ![Syslog akışı](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog koleksiyonunu yapılandırma
1. Syslog iletme VSphere için ayarlayın. Syslog iletme yukarı ayarlamanıza yardımcı olması ayrıntılı bilgi için bkz: [syslog ESXi üzerinde yapılandırma 5.x ve 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Git **ESXi ana bilgisayar yapılandırması** > **yazılım** > **Gelişmiş ayarları** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. İçinde *Syslog.global.logHost* alan, Linux sunucunuza ve bağlantı noktası numarasını ekleyin *1514*. Örneğin, `tcp://hostname:1514` veya `tcp://123.456.789.101:1514`
3. Syslog ESXi ana bilgisayar Güvenlik Duvarı'nı açın. **ESXi ana bilgisayar yapılandırması** > **yazılım** > **güvenlik profili** > **Güvenlik Duvarı** ve açın**Özellikleri**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. VSphere bu syslog doğru şekilde kurulduğundan emin olmak için konsolunu denetleyin. Bu bağlantı noktasını ESXI ana bilgisayarda onaylayın **1514** yapılandırılır.
5. Karşıdan yükleyip OMS Aracısı'nı Linux için Linux sunucusu üzerinde. Daha fazla bilgi için bkz: [Linux için OMS aracısının belgelere](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. Linux için OMS Aracısı yüklendikten sonra /etc/opt/microsoft/omsagent/sysconf/omsagent.d dizinine gidin ve vmware_esxi.conf dosyasını /etc/opt/microsoft/omsagent/conf/omsagent.d dizin ve değiştirme izinleri ve sahibi/Grup kopyalayın dosyası. Örneğin:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. OMS Aracısı'nı çalıştırarak Linux için yeniden `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Linux sunucusuna ve ESXi ana bilgisayar arasındaki bağlantıyı kullanarak test `nc` ESXi ana bilgisayarda komutu. Örneğin:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. Azure portalında günlük aramasını gerçekleştirin `VMware_CL`. Günlük analizi syslog veri topladığında, syslog biçimine korunur. Portalda, bazı belirli alanları, gibi yakalanır *ana bilgisayar adı* ve *işlemadı*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Yukarıdaki resimde görünüm günlük arama sonuçlarınızı benzerse, VMware izleme çözüm panosunu kullanmak için hazırsınız.  

## <a name="vmware-data-collection-details"></a>VMware veri toplama ayrıntıları
VMware izlemesi çözümü ESXi konakları etkinleştirdiğiniz Linux için OMS aracıları kullanarak çeşitli performans ölçümleri ve günlük verilerini toplar.

Aşağıdaki tabloda, veri toplama yöntemleri ve verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Linux için OMS Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |3 dakikada bir |

Aşağıdaki tabloda VMware izlemesi çözümü tarafından toplanan veri alanları örnekleri göster:

| Alan adı | açıklama |
| --- | --- |
| Device_s |VMware depolama aygıtları |
| ESXIFailure_s |hata türleri |
| EventTime_t |Olay gerçekleştiği zaman |
| HostName_s |ESXi ana bilgisayar adı |
| Operation_s |VM oluşturma veya VM silme |
| ProcessName_s |Olay adı |
| ResourceId_s |VMware ana bilgisayar adı |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI durumu |
| SyslogMessage_s |Syslog verilerinin |
| UserName_s |oluşturulan veya silinen VM kullanıcı |
| VMName_s |VM adı |
| Bilgisayar |ana bilgisayar |
| TimeGenerated |verilerin oluşturulduğu zaman |
| DataCenter_s |VMware datacenter |
| StorageLatency_s |Depolama gecikme süresi (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware izleme çözümüne genel bakış
VMware döşeme günlük analizi çalışma alanında görüntülenir. Hataları üst düzey bir görünümünü sağlar. Kutucuğa tıkladığınızda bir pano görünümü'ne gidin.

![Döşeme](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Pano görünümü gidin
İçinde **VMware** Pano görünümü Kanatlar göre düzenlenir:

* Hata durum sayısı
* Üst konak olay tarafından sayar
* Üst olay sayısı
* Sanal makine etkinlikleri
* ESXi ana Disk olayları

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Ayrıntılı bilgi için dikey belirli gösterir günlük analizi arama bölmesini açmak için dikey pencereye'ı tıklatın.

Buradan, belirli bir şey için değiştirmek için arama sorgusu düzenleyebilirsiniz. Günlük aramaları oluşturma hakkında daha fazla bilgi için bkz: [Bul günlük aramaları günlük analizi kullanarak verileri](log-analytics-log-searches.md).

#### <a name="find-esxi-host-events"></a>ESXi ana bilgisayar olaylarını Bul
Tek bir ESXi ana kendi işlemleri temel alan birden çok günlükleri oluşturur. VMware izleme çözümü bunları merkezi hale getirir ve olay sayıları özetler. Merkezi bu görünüm, yüksek hacimli olayları hangi ESXi ana bilgisayarı vardır ve ortamınızda hangi olayların sık karşılaşılan anlamanıza yardımcı olur.

![event](./media/log-analytics-vmware/events.png)

Ayrıntıya inebilir başka bir ESXi ana bilgisayar veya bir olay türü'ı tıklatarak.

Bir ESXi ana bilgisayar adını tıklattığınızda, o ESXi ana bilgisayardan bilgileri görüntüleyin. Olay türü ile sonuçları daraltmak istiyorsanız ekleyin `“ProcessName_s=EVENT TYPE”` arama Sorgunuzdaki. Seçebileceğiniz **işlemadı** arama filtresi. Bu bilgileri daraltır.

![Detaya gitme](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Yüksek VM etkinlikleri Bul
Bir sanal makine oluşturulur ve herhangi bir ESXi ana silinir. Bir yönetici ESXi ana bilgisayar oluşturur kaç Vm'leri tanımlamak yararlıdır. Bu içinde dönüş, performans ve kapasite planlaması anlamak için sağlar. Ortamınızı yönetirken VM etkinlik olayları izlemek önemlidir.

![Detaya gitme](./media/log-analytics-vmware/vmactivities1.png)

Ek ESXi ana VM oluşturma verileri görmek istiyorsanız, bir ESXi ana bilgisayar adına tıklayın.

![Detaya gitme](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Ortak arama sorguları
Çözüm, yüksek depolama alanı, depolama gecikmesi ve yol hatası gibi ESXi konakları yönetmenize yardımcı olabilir diğer yararlı sorgular içerir.

[!INCLUDE[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Sorguları](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Sorguları Kaydet
Arama sorguları kaydetme günlük analizi standart bir özelliktir ve yararlı buldunuz herhangi bir sorgu tutmanıza yardımcı olabilir. Size kullanışlı bir sorgu oluşturduktan sonra tıklayarak Kaydet **Sık Kullanılanlar**. Kayıtlı bir sorguyu kolayca ondan daha sonra yeniden olanak sağlayan [My Pano](log-analytics-dashboards.md) kendi özel panolar oluşturabileceğiniz sayfası.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Uyarı sorgularından oluşturma
Sorgularınızın oluşturduktan sonra belirli olaylar oluştuğunda sizi uyarmak için sorguları kullanmak isteyebilirsiniz. Bkz: [günlük analizi uyarılarını](log-analytics-alerts.md) uyarı oluşturma hakkında bilgi için. Sorguları ve diğer sorgu örnekleri örnekler için bkz: [İzleyicisi günlük analizi kullanarak VMware](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blog postası.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Üzerinde ESXi ayarı barındırmak için neler gerekir? Hangi, my geçerli ortamda etkileyecek?
Çözüm yerel ESXi ana mekanizma iletme Syslog kullanır. Herhangi bir ek Microsoft yazılım günlükleri yakalamak için ESXi ana gerekmez. Var olan ortamınıza düşük bir etkisi olması gerekir. Ancak, ESXI işlevselliği syslog iletme ayarlamak gerekir.

### <a name="do-i-need-to-restart-my-esxi-host"></a>My ESXi ana bilgisayarı yeniden başlatmanız gerekiyor mu?
Hayır. Bu işlem, yeniden başlatma gerektirmez. Bazı durumlarda, vSphere syslog düzgün şekilde güncelleştirmez. Böyle bir durumda ESXi ana bilgisayara oturum açın ve syslog yeniden yükleyin. Bu işlem, ortamınıza kesintiye uğratan değil şekilde konak yeniden gerekmez.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-log-analytics"></a>I artırabilir veya günlük verileri için günlük analizi gönderilen azaltmak?
Evet, engelleyebilirsiniz. VSphere içinde ESXi ana günlük düzeyi ayarlarını kullanabilirsiniz. Günlük toplama temel *bilgisi* düzeyi. VM oluşturma veya silme denetlemek istiyorsanız, bu nedenle, tutmak ihtiyacınız *bilgisi* Hostd düzeyi. Daha fazla bilgi için bkz: [VMware Bilgi Bankası](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-log-analytics-my-log-setting-is-set-to-info"></a>Neden Hostd için günlük analizi verileri sağlayan değil mi? My günlüğü ayar bilgileri için ayarlanır.
Syslog zaman damgası için bir ESXi ana bilgisayar hata oluştu. Daha fazla bilgi için bkz: [VMware Bilgi Bankası](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Geçici çözüm uygulandıktan sonra Hostd normal olarak çalışması.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Birden çok ESXi konakları omsagent ile tek bir VM syslog veri ileten olabilir mi?
Evet. Birden çok ESXi konakları omsagent ile tek bir VM iletme olabilir.

### <a name="why-dont-i-see-data-flowing-into-log-analytics"></a>Günlük analizi akan veri neden göremiyorum?
Birden çok nedeni olabilir:

* ESXi ana doğru veri omsagent çalıştıran VM Ftp'den değil. Test etmek için aşağıdaki adımları gerçekleştirin:

  1. Onaylamak için ssh kullanarak ESXi ana bilgisayara oturum açın ve aşağıdaki komutu çalıştırın: `nc -z ipaddressofVM 1514`

      Bu başarılı olmazsa, Gelişmiş yapılandırma ayarlarında vSphere büyük olasılıkla doğru değil. Bkz: [syslog koleksiyonunu yapılandırma](#configure-syslog-collection) ESXi ana bilgisayar syslog iletme için ayarlama hakkında bilgi için.
  2. Syslog bağlantı noktası bağlantı başarılı olur, ancak yine de hiçbir veri görmezsiniz, syslog ESXi ana bilgisayarda ssh aşağıdaki komutu kullanarak yeniden: ` esxcli system syslog reload`
* OMS Aracısı ile VM düzgün ayarlanmadı. Bunu test etmek için aşağıdaki adımları gerçekleştirin:

  1. Günlük analizi 1514 bağlantı noktasını dinler. Açık olduğunu doğrulamak için aşağıdaki komutu çalıştırın: `netstat -a | grep 1514`
  2. Bağlantı noktası görmelisiniz `1514/tcp` açın. Bunu yapmazsanız, omsagent düzgün yüklendiğini doğrulayın. Bağlantı noktası bilgileri görmüyorsanız, syslog bağlantı noktası VM açık değil.

    a. OMS Aracısı'nı kullanarak çalışır durumda olduğunu doğrulama `ps -ef | grep oms`. Çalışmıyorsa, işlem komutu çalıştırarak Başlat ` sudo /opt/microsoft/omsagent/bin/service_control start`

    b. `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` dosyasını açın.

    c. Uygun kullanıcı ve grup ayarı geçerli ve benzer şekilde doğrulayın: `-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

    d. Dosya yok veya tarafından düzeltme eylemi gerçekleştirin kullanıcı ve grup ayarı yanlış olmadığını [Linux sunucusu hazırlama](#prepare-a-linux-server).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük aramaları](log-analytics-log-searches.md) Analytics ayrıntılı VMware görüntülemek için günlük verileri ana bilgisayar.
* [Kendi panolar oluşturun](log-analytics-dashboards.md) VMware ana verileri gösterme.
* [Uyarı oluşturma](log-analytics-alerts.md) belirli VMware konak olayları olduğunda.
