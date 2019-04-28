---
title: Azure İzleyici'de VMware izleme çözümü | Microsoft Docs
description: VMware izleme çözümü günlüklerini yönetmek ve nasıl ESXi konaklarını izlemek yardımcı olabileceği hakkında bilgi edinin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: magoedte
ms.openlocfilehash: eac6a27c3bcf64462a9f3d9a57da6df736f30c78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386226"
---
# <a name="vmware-monitoring-deprecated-solution-in-azure-monitor"></a>Azure İzleyici çözümde VMware izleme (kullanım dışı)

![VMware simgesi](./media/vmware/vmware-symbol.png)

> [!NOTE]
> VMware izleme çözümü kullanım dışıdır.  Çözüm'i zaten yüklemiş olan müşteriler, onu kullanmaya devam edebilirsiniz, ancak VMware izleme için oluşturulan yeni çalışma alanlarında eklenemez.

VMware izleme çözümü Azure İzleyici'de, Merkezi günlük kaydı ve büyük VMware günlükleri için izleme yaklaşımı oluşturmanıza yardımcı olan bir çözümdür. Bu makalede nasıl sorun giderme, yakalama ve ESXi konaklarının çözümünü kullanarak tek bir konumda açıklanır. Çözümü ile tek bir konumda tüm ESXi konakları için ayrıntılı verileri görebilirsiniz. En yüksek olay sayıları, durumu ve eğilimleri, VM ve ESXi konaklarının ESXi konağı günlükleri sağlanan görebilirsiniz. Görüntüleme ve Arama Merkezi ESXi konak günlüklerini giderebilirsiniz. Ve günlük arama sorgularına dayalı uyarılar oluşturabilirsiniz.

Çözüm, veri göndermek için bir hedef, Log Analytics Aracısı VM ESXi ana bilgisayarının yerel syslog işlevselliğini kullanır. Ancak, çözümü hedef sanal makine içinde syslog içine dosyaları yazma değil. Log Analytics aracısını 1514 bağlantı noktasını açar ve bunun için dinler. Log Analytics aracısını verileri aldıktan sonra Azure İzleyici ile verileri anında bildirim gönderir.

## <a name="install-and-configure-the-solution"></a>Yükleme ve çözüm yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

* VMware izleme çözümü, açıklanan işlemi kullanarak aboneliğinize ekleyin [bir izleme çözümü yükleme](../insights/solutions.md#install-a-monitoring-solution).

#### <a name="supported-vmware-esxi-hosts"></a>Desteklenen VMware ESXi konakları
vSphere ESXi konağına 5.5, 6.0 ve 6.5

#### <a name="prepare-a-linux-server"></a>Bir Linux sunucusu hazırlama
Bir Linux işletim sistemi ESXi konağından tüm syslog verileri almak için bir VM oluşturun. [Log Analytics Linux Aracısı](../learn/quick-collect-linux-computer.md) tüm ESXi konağı syslog veriler için koleksiyon noktasıdır. Birden çok ESXi konakları, günlükleri aşağıdaki örnekteki gibi tek bir Linux sunucusuna iletmek için kullanabilirsiniz.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

   ![Syslog akış](./media/vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog koleksiyonunu yapılandırma
1. VSphere için syslog iletmeyi ayarlayın. Syslog iletmeyi ayarlamanıza yardımcı olması ayrıntılı bilgi için bkz: [syslog ESXi 5.0 ve üzeri (2003322) yapılandırma](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Git **ESXi ana bilgisayar yapılandırması** > **yazılım** > **Gelişmiş ayarlar** > **Syslog**.
   ![vsphereconfig](./media/vmware/vsphere1.png)  
1. İçinde *Syslog.global.logHost* Linux sunucunuza ve bağlantı noktası numarası Ekle, alan *1514*. Örneğin, `tcp://hostname:1514` veya `tcp://123.456.789.101:1514`
1. Syslog için ESXi konak Güvenlik Duvarı'nı açın. **ESXi ana bilgisayar yapılandırması** > **yazılım** > **güvenlik profili** > **Güvenlik Duvarı** açın**Özellikleri**.  

    ![vspherefw](./media/vmware/vsphere2.png)  

    ![vspherefwproperties](./media/vmware/vsphere3.png)  
1. Bu syslog doğru şekilde ayarlandığını doğrulamak için konsol vSphere denetleyin. Bu bağlantı noktasını ESXI ana bilgisayarındaki onaylayın **1514** yapılandırılır.
1. İndirin ve Linux sunucusu üzerinde Linux için Log Analytics aracısını yükleyin. Daha fazla bilgi için [Linux için Log Analytics aracısını belgelerine](https://github.com/Microsoft/OMS-Agent-for-Linux).
1. Log Analytics aracısını Linux yüklenebileceği konusunda olduktan sonra /etc/opt/microsoft/omsagent/sysconf/omsagent.d dizin ve kopyalama vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d dizin ve dosya değişikliği için sahibi/grup ve dosya izinleri. Örneğin:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
1. Linux için Log Analytics aracısını çalıştırarak yeniden `sudo /opt/microsoft/omsagent/bin/service_control restart`.
1. Linux sunucunun ESXi ana bilgisayar arasındaki bağlantıyı kullanarak test `nc` ESXi ana bilgisayarındaki komutu. Örneğin:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

1. Azure portalında günlük sorgusu gerçekleştirmek `VMware_CL`. Azure İzleyici syslog verileri toplar, syslog biçimine korunur. Portalda, bazı belirli alanları, gibi yakalanır *Hostname* ve *ProcessName*.  

    ![type](./media/vmware/type.png)  

    Yukarıdaki resimde günlük arama sonuçlarını görüntüle benzerse VMware izleme Panosu kullanmak işinizi tamamlandı.  

## <a name="vmware-data-collection-details"></a>VMware veri koleksiyonu ayrıntıları
VMware izleme çözümü ESXi konakları etkinleştirdiğiniz Linux için Log Analytics aracılarını kullanarak çeşitli performans ölçümleri ve günlük verilerini toplar.

Veri toplama metotlarını ve verileri nasıl toplanır hakkında diğer ayrıntıları aşağıdaki tabloda gösterilmektedir.

| Platform | Linux için log Analytics aracısını | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |3 dakikada bir |

Aşağıdaki tabloda, VMware izleme çözümü tarafından toplanan veri alanları örneklerini gösterir:

| alan adı | açıklama |
| --- | --- |
| Device_s |VMware depolama cihazları |
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
| SyslogMessage_s |Syslog verileri |
| UserName_s |oluşturulan veya sanal makine silinmiş kullanıcılar |
| VMName_s |VM adı |
| Bilgisayar |ana bilgisayar |
| TimeGenerated |verilerin oluşturulduğu zaman |
| DataCenter_s |VMware veri merkezi |
| StorageLatency_s |Depolama gecikmesi (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware izleme çözümüne genel bakış
VMware kutucuk, Log Analytics çalışma alanınızda görünür. Bu, hataları üst düzey bir görünümünü sağlar. Kutucuğa tıkladığınızda Pano görünümüne gidin.

![tile (döşeme)](./media/vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Pano görünümü gidin
İçinde **VMware** Pano görünümü dikey pencereleri göre düzenlenir:

* Hata durumu sayısı
* Üst konak olay sayısı
* En yüksek olay sayıları
* Sanal makine etkinlikleri
* ESXi konağı Disk olayları

![solution1](./media/vmware/solutionview1-1.png)

![solution2](./media/vmware/solutionview1-2.png)

Ayrıntılı bilgi dikey belirli gösteren Log Analytics arama bölmesini açmak için istediğiniz dikey pencereye tıklayın.

Buradan, belirli bir şey değiştirmek için günlük sorgusu düzenleyebilirsiniz. Günlük sorguları oluşturma hakkında daha fazla bilgi edinmek için bkz. [Bul günlük sorguları Azure İzleyici'de kullanarak verileri](../log-query/log-query-overview.md).

#### <a name="find-esxi-host-events"></a>ESXi ana etkinlik bulun
Tek bir ESXi ana bilgisayarı, kendi işlemleri temel alan birden fazla günlük oluşturur. VMware izleme çözümü, bunları merkezileştirir ve de olay sayılarını özetler. Bu merkezi görünüm hangi ESXi ana bilgisayarına fazla miktarda olay varsa ve hangi olayların ortamınızda en sık gerçekleşen anlamanıza yardımcı olur.

![event](./media/vmware/events.png)

Ayrıntıya inebilir başka bir ESXi ana bilgisayarı veya bir olay türü'ı tıklatarak.

Bir ESXi ana bilgisayar adını tıklattığınızda, o ESXi konağı bilgileri görüntüleyin. Olay türü sonuçları daraltmak isterseniz, ekleme `“ProcessName_s=EVENT TYPE”` arama sorgunuzda. Seçebileceğiniz **ProcessName** arama filtre. Bu bilgileri daraltır.

![Detaya gidin](./media/vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Yüksek VM etkinlikleri bulun
Bir sanal makine oluşturulur ve herhangi bir ESXi ana bilgisayarındaki silindi. Bir yöneticinin bir ESXi ana bilgisayarı oluşturur kaç tane Vm'niz tanımlamak için yararlıdır. Bu içinde dönüş, performans ve kapasite planlaması anlamanıza yardımcı olur. Ortamınızı yönetirken VM etkinlik olayları izlemek önemlidir.

![Detaya gidin](./media/vmware/vmactivities1.png)

Ek ESXi ana VM oluşturmayı verileri görmek istiyorsanız, bir ESXi ana bilgisayar adına tıklayın.

![Detaya gidin](./media/vmware/createvm.png)

#### <a name="common-log-queries"></a>Ortak günlük sorguları
Çözüm, yüksek depolama alanı, depolama gecikmesi ve yol hatası gibi ESXi ana bilgisayarları yönetmenize yardımcı olabilecek diğer kullanışlı sorgular içerir.

![sorgu](./media/vmware/queries.png)


#### <a name="save-queries"></a>Sorguları Kaydet
Günlük sorguları kaydetme, Azure İzleyici'de standart bir özelliktir ve yararlı buldunuz tüm sorguları sağlamanıza yardımcı olabilir. Yararlı bulabileceğiniz bir sorguyu oluşturduktan sonra Kaydet'e tıklayarak **Sık Kullanılanlar**. Kayıtlı bir sorguyu sayesinde kolayca daha sonra yeniden [Panom'u](../learn/tutorial-logs-dashboards.md) kendi özel panolar oluşturabileceğiniz sayfası.

![DockerDashboardView](./media/vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Uyarı sorguları oluşturma
Sorgularınızın oluşturduktan sonra belirli olaylar meydana geldiğinde, sizi uyarmak için sorguları kullanmak isteyebilirsiniz. Bkz: [Log analytics'teki uyarılar](../platform/alerts-overview.md) uyarılar oluşturma hakkında daha fazla bilgi için. Sorguları ve diğer sorgu örnekleri uyarı örnekleri için bkz: [İzleyici Log Analytics kullanarak VMware](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) blog gönderisi.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Ayarı üzerinde ESXi ana bilgisayar ne gerekiyor? Hangi etki alanım geçerli ortamda gerekir mi?
Çözüm, yerel ESXi konağı Syslog iletmeyi mekanizması kullanır. Herhangi bir ek Microsoft yazılım günlükleri tutmak için ESXi ana bilgisayarındaki gerekmez. Bu, var olan ortamınıza düşük bir etkisi olmaması gerekir. Ancak, ESXI işlevselliği syslog iletmeyi ayarlamanız gerekir.

### <a name="do-i-need-to-restart-my-esxi-host"></a>My ESXi ana bilgisayarı yeniden başlatmanız gerekiyor mu?
Hayır. Bu işlem, yeniden başlatma gerektirmez. Bazı durumlarda, vSphere syslog düzgün şekilde güncelleştirilmez. Böyle bir durumda ESXi ana bilgisayara oturum açın ve syslog yeniden yükleyin. Bu işlem ortamınız için karışıklığa neden bu nedenle konak yeniden başlatmanız gerekmez.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-log-analytics"></a>Ben artırabilir veya günlük verileri Log Analytics'e gönderilen azaltmak?
Evet, kullanabilirsiniz. VSphere ESXi ana günlük düzeyi ayarları kullanabilirsiniz. Günlük toplama temel *bilgisi* düzeyi. VM oluşturulurken veya silinirken denetlemek istiyorsanız, bu nedenle, çalışır durumda bulundurmanıza gerek *bilgisi* Hostd düzeyi. Daha fazla bilgi için [VMware Bilgi Bankası](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-log-analytics-my-log-setting-is-set-to-info"></a>Neden Hostd, verilerini Log Analytics'e bulunmaması? My günlük ayarı bilgileri ayarlanır.
Bir syslog zaman damgası için ESXi konağı hata oluştu. Daha fazla bilgi için [VMware Bilgi Bankası](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Geçici çözüm uygulandıktan sonra Hostd normal çalışması.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Birden çok ESXi konakları omsagent ile tek bir VM için syslog veri ileten sahip olabilir miyim?
Evet. Birden çok ESXi konakları omsagent ile tek bir VM için iletme olabilir.

### <a name="why-dont-i-see-data-flowing-into-log-analytics"></a>Veri akışının Log Analytics'e neden göremiyorum?
Birden çok nedeni olabilir:

* ESXi konağı doğru veri omsagent çalıştıran VM dağıtmaya değil. Test etmek için aşağıdaki adımları gerçekleştirin:

  1. Onaylamak için ESXi konağı ssh kullanarak oturum açın ve aşağıdaki komutu çalıştırın: `nc -z ipaddressofVM 1514`

      Bu başarılı olmazsa, Gelişmiş yapılandırma ayarlarında vSphere olasılıkları doğru değil. Bkz: [syslog koleksiyonu yapılandırmak](#configure-syslog-collection) ESXi konağı için syslog iletmeyi ayarlama hakkında bilgi için.
  1. Syslog bağlantı başarılı olur, ancak yine de herhangi bir veri görmüyorsanız, syslog ESXi ana bilgisayarındaki ssh aşağıdaki komutu çalıştırarak kullanarak yeniden: `esxcli system syslog reload`
* Log Analytics aracısını VM doğru şekilde ayarlanmadı. Bunu test etmek için aşağıdaki adımları gerçekleştirin:

  1. Log Analytics'e 1514 bağlantı noktasını dinler. Açık olduğunu doğrulamak için aşağıdaki komutu çalıştırın: `netstat -a | grep 1514`
  1. Bağlantı noktası görmelisiniz `1514/tcp` açın. Bunu yapmazsanız, omsagent doğru yüklendiğini doğrulayın. Bağlantı noktası bilgileri görmüyorsanız, syslog bağlantı noktasını VM açık değil.

    a. Log Analytics aracısını kullanarak çalıştığını doğrulamak `ps -ef | grep oms`. Komutunu çalıştırarak işlemi çalışmıyorsa, başlatmak `sudo /opt/microsoft/omsagent/bin/service_control start`

     b. `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` dosyasını açın.

     c. Uygun kullanıcı ve grup ayarı geçerli ve benzer şekilde doğrulayın: `-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

     d. Dosya yok veya kullanıcı ve grup ayarı yanlış tarafından düzeltici varsa [bir Linux sunucusu hazırlama](#prepare-a-linux-server).

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [oturum sorguları](../log-query/log-query-overview.md) ayrıntılı VMware görüntülemek için Log Analytics'te veri barındırın.
* [Kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) VMware konak veriler gösteriliyor.
* [Uyarı oluşturma](../platform/alerts-overview.md) belirli VMware konak olaylar gerçekleştiğinde.
