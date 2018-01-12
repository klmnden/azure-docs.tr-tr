---
title: "StorSimple Linux ana bilgisayarına MPIO yapılandırma | Microsoft Docs"
description: "MPIO CentOS 6.6 çalıştıran bir Linux konağına bağlı StorSimple yapılandırın"
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 2fbae15c1c6a9ec886f57f9df903612ae10d8e12
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>CentOS çalıştıran bir StorSimple konakta MPIO'yu yapılandırın
Bu makalede, Centos 6.6 ana bilgisayar sunucusunda çoklu yol oluşturma g/ç (MPIO) yapılandırmak için gereken adımları açıklar. Ana bilgisayar sunucusu Microsoft Azure StorSimple Cihazınızı iSCSI başlatıcıları aracılığıyla yüksek kullanılabilirlik için bağlı. Bu, çok yollu aygıtlar ve yalnızca StorSimple birimler için özel kurulum otomatik olarak bulmayı ayrıntılı olarak açıklanmaktadır.

Bu yordam, StorSimple 8000 serisi cihazlar, tüm modelleri için geçerlidir.

> [!NOTE]
> Bu yordam bir StorSimple bulut uygulaması için kullanılamaz. Daha fazla bilgi için ana bilgisayar sunucularının, bulut uygulaması için nasıl yapılandırılacağı bakın.


## <a name="about-multipathing"></a>Çoklu yol oluşturma hakkında
Çoklu yol oluşturma özelliği, bir konak sunucusu ile bir depolama aygıtı arasında birden çok g/ç yollar yapılandırmanıza olanak sağlar. Bu g/ç yolları ayrı kablo, anahtarlar, ağ arabirimleri ve denetleyicileri dahil edebileceğiniz fiziksel SAN bağlantılardır. Çoklu yol oluşturma tüm toplanan yollar ile ilişkili yeni bir cihaz yapılandırmak için g/ç yolu toplar.

Çoklu yol oluşturma amacı iki Katlama şöyledir:

* **Yüksek kullanılabilirlik**: (örneğin, bir kablo, anahtar, ağ arabirimi veya denetleyicisi) g/ç yolunun herhangi bir öğeyi başarısız olursa alternatif bir yol sağlar.
* **Yük Dengeleme**: depolama Cihazınızı yapılandırmasına bağlı olarak, g/ç yolu yükleri algılama ve dinamik olarak bu yükleri dengelenmesi performansı artırabilir.

### <a name="about-multipathing-components"></a>Çoklu yol oluşturma bileşenleri hakkında
Çoklu yol oluşturma Linux içindeki çekirdek bileşenleri ve aşağıdaki tabloda gibi kullanıcı alanı bileşenleri oluşur.

* **Çekirdek**: ana bileşeni *aygıt Eşleyici* g/ç yeniden yönlendirmeler ve yolları ve yol grupları için yük devretmeyi destekler.

* **Kullanıcı alanı**: Bunlar *çok yollu Araçları* , multipathed cihazları yönetme aygıt Eşleyici çok yollu modülü yönlendirerek yapmanız gerekenler. Araçları şunlardan oluşur:
   
   * **Çok yollu**: listeler ve multipathed cihazları yapılandırır.
   * **Multipathd**: çok yollu yürütür ve yolları izler arka plan programı.
   * **Devmap adı**: devmaps için udev için anlamlı bir aygıt adı sağlar.
   * **Kpartx**: Doğrusal devmaps çok yollu eşlemeleri bölümlenebilir olmak için aygıt bölümlere eşler.
   * **MultiPath.conf**: yerleşik yapılandırma tablosunun üzerine yazmak için kullanılan çok yollu arka plan programı için yapılandırma dosyası.

### <a name="about-the-multipathconf-configuration-file"></a>Multipath.conf yapılandırma dosyası hakkında
Yapılandırma dosyası `/etc/multipath.conf` çoklu yol oluşturma özelliklerinin çoğu kullanıcı tarafından yapılandırılabilir hale getirir. `multipath` Komut ve çekirdek arka plan programı `multipathd` bu dosyada bulunan bilgileri kullanın. Dosya, yalnızca çok yollu aygıtları yapılandırma sırasında alınmadığında. Çalıştırmadan önce tüm değişiklikleri yapıldığından emin olun `multipath` komutu. Dosyayı daha sonra değiştirirseniz, durdurma ve multipathd için değişikliklerin etkili olması için yeniden başlatma gerekir.

Multipath.conf beş bölümü vardır:

- **Sistem düzeyinde varsayılanlarını** *(varsayılan)*: sistem düzeyinde varsayılanlarını geçersiz kılabilirsiniz.
- **Kara listede aygıtları** *(kara liste)*: cihaz Eşleyicisi tarafından denetlenmeyen cihazların listesini belirtebilirsiniz.
- **Özel durumlar kara listeye** *(blacklist_exceptions)*: kara listeye listelenen olsa bile çok yollu cihaz olarak kabul edilmesi için belirli cihazlar tanımlayabilirsiniz.
- **Depolama denetleyicisi belirli ayarları** *(aygıtlar)*: Satıcı ve ürün bilgileri bulunduğu cihazlara uygulanacak olan yapılandırma ayarları belirtebilirsiniz.
- **Aygıt belirli ayarları** *(multipaths)*: tek tek LUN'ları için yapılandırma ayarlarını ince ayar yapmak için bu bölümü kullanın.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Çoklu yol oluşturma Linux konağına bağlı StorSimple yapılandırın
Bir Linux konağına bağlı bir StorSimple cihazı, yüksek kullanılabilirlik ve Yük Dengeleme için yapılandırılabilir. Örneğin, iki arabirim SAN'a bağlı Linux ana bilgisayar varsa ve bu arabirimleri aynı alt ağda gibi olduğuna göre SAN'a bağlı iki arabirim aygıtın, ardından olacaktır 4 yolları kullanılabilir. Her veri arabirimi cihaz ve ana bilgisayar arabirimde farklı bir IP alt ağ (ve değil yönlendirilebilir) varsa, ancak sonra yalnızca 2 yolları kullanılabilir. Çoklu yol oluşturma otomatik olarak kullanılabilen tüm yolları Bul, bu yollar için bir Yük Dengeleme algoritması seçin, yalnızca StorSimple birimler için belirli yapılandırma ayarları uygulamak ve sonra etkinleştirin ve çoklu yol oluşturma doğrulayın yapılandırabilirsiniz.

Aşağıdaki yordamda, iki ağ arabirimine sahip bir konağa iki ağ arabirimi ile bir StorSimple cihazı bağlı olduğunda çoklu yol oluşturma yapılandırmak açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar
Bu bölümde, CentOS sunucu ve StorSimple cihazınız için yapılandırma önkoşulları ayrıntıları verilmektedir.

### <a name="on-centos-host"></a>CentOS konakta
1. CentOS ana bilgisayar 2 ağ arabirimleri etkin olduğundan emin olun. Şunu yazın:
   
    `ifconfig`
   
    İki ağ arabirimleri, aşağıdaki örnek çıkış şunları gösterir (`eth0` ve `eth1`) konakta yok.
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. Yükleme *iSCSI-Başlatıcı-yardımcı programları* CentOS sunucunuzda. Yüklemek için aşağıdaki adımları gerçekleştirin *iSCSI-Başlatıcı-yardımcı programları*.
   
   1. Oturum açma `root` CentOS ana bilgisayarınızın INTO.
   2. Yükleme *iSCSI-Başlatıcı-yardımcı programları*. Şunu yazın:
      
       `yum install iscsi-initiator-utils`
   3. Sonra *iSCSI-Başlatıcı-yardımcı programları* başarıyla yüklendiyse, iSCSI hizmetini başlatın. Şunu yazın:
      
       `service iscsid start`
      
       Olaylar üzerinde `iscsid` gerçekten başlatılamayabilir ve `--force` seçeneği gerekebilir
   4. İSCSI başlatıcısı önyükleme süresince etkinleştirildiğinden emin olmak için kullanmak `chkconfig` hizmeti etkinleştirmek için komutu.
      
       `chkconfig iscsi on`
   5. Oluştu, düzgün şekilde Kur'un doğrulamak için komutu çalıştırın:
      
       `chkconfig --list | grep iscsi`
      
       Örnek çıktı aşağıda gösterilmiştir.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       Yukarıdaki örnekte, iSCSI ortamınızı önyükleme zamanında çalışma Düzey 2, 3, 4 ve 5 çalışacağını görebilirsiniz.
3. Yükleme *aygıt-Eşleyici-çok yollu*. Şunu yazın:
   
    `yum install device-mapper-multipath`
   
    Yükleme başlar. Tür **Y** onaylamanız istendiğinde devam etmek için.

### <a name="on-storsimple-device"></a>StorSimple cihazında
StorSimple Cihazınızı olmalıdır:

* İSCSI için iki arabirimin en az. İki arabirim StorSimple Cihazınızda iSCSI etkin olduğunu doğrulamak için StorSimple cihazınız için Azure Klasik portalında aşağıdaki adımları gerçekleştirin:
  
  1. StorSimple cihazınız için Klasik portalında oturum açın.
  2. StorSimple Yöneticisi hizmetiniz seçin, **aygıtları** ve belirli StorSimple cihazı seçin. Tıklatın **yapılandırma** ve ağ arabirimi ayarlarını doğrulayın. İki iSCSI etkin ağ arabirimlerine sahip bir ekran görüntüsü aşağıda gösterilmiştir. Burada veri 2 ve veri 3, her iki 10 GbE arabirimleri iSCSI için etkinleştirilir.
     
      ![MPIO StorsSimple veri 2 yapılandırma](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorSimple veri 3 yapılandırma](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      İçinde **yapılandırma** sayfası
     
     1. Her iki ağ arabirimleri iSCSI etkin olduğundan emin olun. **İSCSI etkin** alan ayarlanmalıdır **Evet**.
     2. Ağ arabirimleri aynı hızınız, ikisi de 1 GbE veya 10 GbE olmalıdır emin olun.
     3. İSCSI etkin arabirimlerin IPv4 adreslerini not alın ve ana bilgisayarda daha sonra kullanmak için kaydedin.
* StorSimple Cihazınızı iSCSI arabirimlerde CentOS sunucudan erişilebilir olmalıdır.
      Bunu doğrulamak için StorSimple iSCSI etkin ağ arabirimlerinin IP adresleri, ana bilgisayar sunucusunda sağlamanız gerekir. Kullanılan komutlar ve karşılık gelen çıktı veri2 ile (10.126.162.25) ve DATA3 (10.126.162.26) aşağıda gösterilmiştir:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Donanım yapılandırması
Artıklık için ayrı yollar iki iSCSI ağ arabirimlerindeki bağlanmasını öneririz. Aşağıdaki şekilde, yüksek kullanılabilirlik için önerilen donanım yapılandırması ve CentOS sunucunuz ve StorSimple cihazı için çoklu yol oluşturma Yük Dengeleme gösterilmektedir.

![Linux ana bilgisayara StorSimple için MPIO donanım yapılandırması](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Yukarıdaki şekilde gösterildiği gibi:

* StorSimple Cihazınızı iki denetleyicileri Aktif-Pasif yapılandırmayla kullanılıyor.
* İki SAN anahtarları aygıt denetleyicilerinizi bağlanır.
* StorSimple Cihazınızda iki iSCSI başlatıcıları etkinleştirilir.
* İki ağ arabirimi, CentOS ana bilgisayarda etkin.

Ana bilgisayar ve veri arabirimleri yönlendirilebilir olması durumunda Yukarıdaki yapılandırma Cihazınızı ve ana bilgisayar arasında 4 ayrı yollar sunacak.

> [!IMPORTANT]
> * 1 GbE ve çoklu yol oluşturma için 10 GbE ağ arabirimleri karıştırmamanızı öneririz. İki ağ arabirimleri kullanılırken, her iki arabirimde aynı türde olmalıdır.
> * Veri2 ve DATA3 10 GbE ağ arabirimleri ise, StorSimple Cihazınızda DATA0, Veri1, DATA4 ve DATA5 1 GbE arabirimlerdir. |
> 
> 

## <a name="configuration-steps"></a>Yapılandırma adımları
Yapılandırma adımları çoklu yol oluşturma için çoklu yol oluşturma etkinleştirme ve son olarak yapılandırmasını doğrulama kullanmak için Yük Dengeleme algoritması belirtme, otomatik bulma için mevcut yolları yapılandırmayı içerir. Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>1. adım: çoklu yol oluşturma otomatik bulma için yapılandırma
Çok yollu desteklenen cihazları otomatik olarak bulunan ve yapılandırılmış.

1. Initialize `/etc/multipath.conf` dosya. Şunu yazın:
   
     `mpathconf --enable`
   
    Yukarıdaki komut oluşturacak bir `sample/etc/multipath.conf` dosyası.
2. Çok yollu hizmetini başlatın. Şunu yazın:
   
    `service multipathd start`
   
    Şu çıktı görürsünüz:
   
    `Starting multipathd daemon:`
3. Multipaths otomatik olarak bulmayı etkinleştirin. Şunu yazın:
   
    `mpathconf --find_multipaths y`
   
    Bu varsayılan bölümünü değiştirmek, `multipath.conf` aşağıda gösterildiği gibi:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>2. adım: StorSimple birimler için çoklu yol oluşturmayı yapılandırma
Varsayılan olarak, tüm cihazlar multipath.conf dosyasında listelenen siyah ve atlanır. Çoklu yol oluşturma StorSimple cihazları birimlerin izin veren kara liste özel durumlar oluşturmanız gerekir.

1. Düzen `/etc/mulitpath.conf` dosya. Şunu yazın:
   
    `vi /etc/multipath.conf`
2. Blacklist_exceptions bölüm multipath.conf dosyasında bulun. StorSimple Cihazınızı Bu bölümde bir kara liste özel olarak listelenmesi gerekir. Bu dosyadaki (kullanın, kullanmakta olduğunuz cihaz yalnızca belirli modelinin) aşağıda gösterildiği gibi değiştirmek için ilgili satırlara açıklamadan çıkarın:
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>3. adım: hepsini çoklu yol oluşturmayı yapılandırma
Bu Yük Dengeleme algoritması etkin denetleyiciye tüm kullanılabilir multipaths Dengeli, hepsini bir biçimde kullanır.

1. Düzen `/etc/multipath.conf` dosya. Şunu yazın:
   
    `vi /etc/multipath.conf`
2. Altında `defaults` bölümünde, `path_grouping_policy` için `multibus`. `path_grouping_policy` Belirtilmeyen multipaths uygulamak için ilke gruplandırma varsayılan yolunu belirtir. Varsayılanları bölümü, aşağıda gösterildiği gibi görünecektir.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> En yaygın değerlerini `path_grouping_policy` içerir:
> 
> * Yük devretme öncelik grubu başına 1 yolu =
> * multibus 1 önceliği grubundaki tüm geçerli yolu =
> 
> 

### <a name="step-4-enable-multipathing"></a>4. adım: Etkinleştirme çoklu yol oluşturma
1. Yeniden `multipathd` arka plan programı. Şunu yazın:
   
    `service multipathd restart`
2. Çıktı aşağıda gösterildiği gibi olacaktır:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>5. adım: çoklu yol oluşturma doğrulayın
1. Önce iSCSI bağlantı StorSimple cihazı gibi kurulduğundan emin olun:
   
   a. StorSimple Cihazınızı bulur. Şunu yazın:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    Aşağıda gösterildiği gibi 10.126.162.25 DATA0 için IP adresi olduğundan ve bağlantı noktası 3260 giden iSCSI trafiği için StorSimple cihazında açıldığında çıktısı şöyledir:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    StorSimple Cihazınızı IQN kopyalama `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, önceki çıktısından.

   b. Aygıtına hedef IQN kullanarak bağlanın. StorSimple cihazı iSCSI burada hedefidir. Şunu yazın:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Aşağıdaki örnek çıkış hedefiyle IQN gösterir, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. Çıktı Cihazınızda iki iSCSI etkin ağ arabirimi başarıyla bağlandıysanız gösterir.

    ```
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    Yalnızca bir ana bilgisayar arabirimi ve iki yollarını burada görürseniz, arabirimler konakta iSCSI için etkinleştirmeniz gerekir. İzleyebileceğiniz [ayrıntılı Linux belgelerindeki yönergeleri](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

2. Bir birim StorSimple cihazı CentOS sunucuya açıktır. Daha fazla bilgi için bkz: [6. adım: birim oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) , StorSimple Cihazınızda Azure Portalı aracılığıyla.

3. Kullanılabilir yollar doğrulayın. Şunu yazın:

      ```
      multipath –l
      ```

      Aşağıdaki örnek çıkış iki ağ arabirimi için iki kullanılabilir yola sahip bir tek ana bilgisayar ağ arabirimi için bağlı bir StorSimple cihazına gösterir.

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        The following example shows the output for two network interfaces on a StorSimple device connected to two host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After the paths are configured, refer to the specific instructions on your host operating system (Centos 6.6) to mount and format this volume.

## <a name="troubleshoot-multipathing"></a>Çoklu yol oluşturma sorunlarını giderme
Çoklu yol oluşturma yapılandırması sırasında herhangi bir sorunla çalıştırırsanız, bu bölümde bazı yararlı ipuçları sağlanır.

Q. Değişiklikleri görüntülenmemesini `multipath.conf` etkisi alma dosyası.

A. Herhangi bir değişiklik yaptıysanız `multipath.conf` dosya, çoklu yol oluşturma hizmeti yeniden başlatmanız gerekir. Aşağıdaki komutu yazın:

    service multipathd restart

Q. StorSimple cihazında iki ağ arabirimi ve ana bilgisayarda iki ağ arabirimi etkin. Kullanılabilir yollar listelediğinizde, yalnızca iki yolu bakın. Bkz. dört kullanılabilir yolları beklenir.

A. Yönlendirilebilir ve iki yolu aynı alt ağda olduğundan emin olun. Ağ arabirimleri farklı VLAN'ları ve değil yönlendirilebilir varsa, yalnızca iki yolu görürsünüz. Bu doğrulamanın bir yolu, her iki ana arabirimlerinden StorSimple cihazında bir ağ arabirimi ulaşabildiğimizden emin olmaktır. Etmeniz [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) gibi bu doğrulama yalnızca bir destek oturumu yapılabilir.

Q. Kullanılabilir yollar listelediğinizde, herhangi bir çıktı görmezsiniz.

A. Genellikle, öneren çoklu yol oluşturma arka plan programı ile ilgili bir sorun multipathed yollar görmesini değil ve bu büyük olasılıkla burada herhangi bir sorun arasındadır olduğunu `multipath.conf` dosya.

Ayrıca, çok yollu listelerini yanıt ayrıca herhangi bir diski yok anlamına gelebilir gibi aslında bazı disklerin hedefe bağlandıktan sonra görebileceğini denetleme değer olacaktır.

* SCSI veri yolu yeniden taramak için aşağıdaki komutu kullanın:
  
    `$ rescan-scsi-bus.sh `(sg3_utils paketinin bir parçası)
* Aşağıdaki komutları yazın:
  
    `$ dmesg | grep sd*`
     
     Veya
  
    `$ fdisk –l`
  
    Bu yakın zamanda eklenen diskler ayrıntılarını döndürür.
* Bir StorSimple disk olup olmadığını belirlemek için aşağıdaki komutları kullanın:
  
    `cat /sys/block/<DISK>/device/model`
  
    Bu, bir StorSimple disk olup olmadığını belirleyen bir dize döndürür.

Büyük olasılıkla ancak olası nedeni daha az A eski iscsid da olabilir PID. İSCSI oturumlardan oturumunu kapatmak için aşağıdaki komutu kullanın:

    iscsiadm -m node --logout -p <Target_IP>

StorSimple cihazınız iSCSI hedef tüm bağlı ağ arabirimleri için bu komutu yineleyin. Tüm iSCSI oturumlardan kapattınız sonra iSCSI oturumu yeniden kurmak için iSCSI hedef IQN kullanın. Aşağıdaki komutu yazın:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Cihazımı Güvenilenler listesine olup olmadığından emin değilim.

A. Cihazınızı Güvenilenler listesine olup olmadığını doğrulamak için aşağıdaki sorun giderme etkileşimli komutu kullanın:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Daha fazla bilgi için Git [etkileşimli komutu çoklu yol oluşturma için sorun giderme kullanmak](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).

## <a name="list-of-useful-commands"></a>Yararlı komutların listesi
| Tür | Komut | Açıklama |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |İSCSI Hizmeti |
| &nbsp; |`service iscsid stop` |İSCSI hizmetini durdurun |
| &nbsp; |`service iscsid restart` |İSCSI hizmetini yeniden başlatın |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Belirtilen adres kullanılabilir hedeflerde Bul |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |İSCSI hedef oturum açın |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |İSCSI hedef Oturumu Kapat |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |İSCSI Başlatıcı adı yazdırma |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |İSCSI oturumu ve ana bilgisayarda bulunan birim durumunu denetleyin |
| &nbsp; |`iscsi –m session` |Konak ve StorSimple cihazı arasında kurulan tüm iSCSI oturumları gösterir |
|  | | |
| **Çoklu yol oluşturma** |`service multipathd start` |Çok yollu arka plan programı Başlat |
| &nbsp; |`service multipathd stop` |Çok yollu arka plan programı durdurun |
| &nbsp; |`service multipathd restart` |Çok yollu arka plan programı yeniden başlatın |
| &nbsp; |`chkconfig multipathd on` </br> OR </br> `mpathconf –with_chkconfig y` |Önyükleme sırasında başlatmak çok yollu arka plan programı etkinleştir |
| &nbsp; |`multipathd –k` |Sorun giderme için etkileşimli konsolunu başlatın |
| &nbsp; |`multipath –l` |Liste çok yollu bağlantıları ve cihazlar |
| &nbsp; |`mpathconf --enable` |Bir örnek mulitpath.conf dosya oluşturma`/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Sonraki adımlar
Linux ana bilgisayarına MPIO yapılandırma gibi aşağıdaki CentoS 6.6 belgelerine bakın gerekebilir:

* [CentOS MPIO ayarlama](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Linux eğitim Kılavuzu](http://linux-training.be/files/books/LinuxAdm.pdf)

