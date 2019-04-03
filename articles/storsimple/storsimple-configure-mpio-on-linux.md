---
title: StorSimple Linux ana bilgisayarında MPIO yapılandırma | Microsoft Docs
description: CentOS 6.6 çalıştıran bir Linux konağına bağlı StorSimple MPIO yapılandırma
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
ms.openlocfilehash: bc1e8a5abc85af95448570497177030f17649d87
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877593"
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>CentOS çalıştıran bir StorSimple ana bilgisayarında MPIO yapılandırma
Bu makalede, Centos 6.6 ana bilgisayar sunucusunda çoklu yol oluşturma g/ç (MPIO) yapılandırmak için gereken adımları açıklar. Ana bilgisayar sunucusu, iSCSI başlatıcılarının aracılığıyla yüksek kullanılabilirlik için Microsoft Azure StorSimple cihazınıza bağlıdır. Bu, çok yollu cihazlar ve yalnızca StorSimple birimlerini için özel kurulum otomatik olarak bulunmasını ayrıntılı olarak açıklanmaktadır.

Bu yordam, StorSimple 8000 serisi cihazlar'ın tüm modelleri için geçerlidir.

> [!NOTE]
> Bu yordam, StorSimple Cloud Appliance için kullanılamaz. Bulut gereciniz için ana bilgisayar sunucularının nasıl yapılandırılacağı daha fazla bilgi için bkz.


## <a name="about-multipathing"></a>Çoklu yol oluşturma hakkında
Çoklu yol oluşturma özelliği, bir konak sunucu ve bir depolama cihazı arasında birden çok g/ç yolu yapılandırmanıza olanak sağlar. Bu g/ç yolu ayrı kablo, anahtarlar, ağ arabirimleri ve denetleyicileri dahil edebileceğiniz fiziksel SAN bağlantılardır. Çoklu yol oluşturma tüm toplanan yollar ile ilişkili olan yeni bir cihaz yapılandırmak için g/ç yolu toplar.

Çoklu yol oluşturma amacı iki kat:

* **Yüksek kullanılabilirlik**: G/ç yolunu (örneğin, bir kablo, anahtarı, ağ arabirimi veya denetleyicisi) herhangi bir öğe başarısız olursa başka bir yol sağlar.
* **Yük Dengeleme**: Depolama Cihazınızı yapılandırmasına bağlı olarak, g/ç yolu yükleri algılama ve bu yükleri dinamik olarak yeniden dengelenmesi, performansı geliştirebilir.

### <a name="about-multipathing-components"></a>Çoklu yol oluşturma bileşenleri hakkında
Çoklu yol oluşturma Linux'ta çekirdek bileşenleri ve aşağıdaki tabloda gibi kullanıcı alanı bileşenleri oluşur.

* **Çekirdek**: Ana bileşen *cihaz Eşleyici* g/ç yeniden yönlendirmeler ve yolları ve yol grupları için yük devretmeyi destekler.

* **Kullanıcı alanı**: Bunlar *çok yollu Araçları* , multipathed cihazları yönetme çok yollu cihaz Eşleyici modülü yönlendirerek yapmanız gerekenler. Araçları şunlardan oluşur:
   
   * **Çok yollu**: multipathed cihazları yapılandırır ve listeler.
   * **Multipathd**: çok yollu yürütür ve yolları izler arka plan programı.
   * **Devmap adı**: devmaps için udev için anlamlı bir cihaz adı sağlar.
   * **Kpartx**: çok yollu haritalar bölümlenebilir yapmak için cihaz bölümlere doğrusal devmaps eşler.
   * **MultiPath.conf**: yerleşik yapılandırma tablo üzerine yazmak için kullanılan çok yollu arka plan programı için yapılandırma dosyası.

### <a name="about-the-multipathconf-configuration-file"></a>Multipath.conf yapılandırma dosyası hakkında
Yapılandırma dosyası `/etc/multipath.conf` çoklu yol oluşturma özelliklerinin çoğu kullanıcı tarafından yapılandırılabilir hale getirir. `multipath` Komut ve çekirdek arka plan programı `multipathd` bu dosyada bulunan bilgileri kullanın. Dosya, yalnızca çok yollu cihazların yapılandırma sırasında alınmadığında. Çalıştırmadan önce tüm değişiklikleri yapıldığından emin olun `multipath` komutu. Dosyayı daha sonra değiştirirseniz, durdurmak ve başlatmak için değişikliklerin etkili olması için yeniden multipathd gerekecektir.

Multipath.conf beş bölümü vardır:

- **Sistem düzeyinde varsayılanlarını** *(varsayılan)*: Sistem düzeyinde varsayılanlarını geçersiz kılabilirsiniz.
- **Cihazları kara listede** *(kara liste)*: Cihaz Eşleyicisi tarafından denetlenmelidir olmayan cihazların listesini belirtebilirsiniz.
- **Özel durumlar veya kara listeye** *(blacklist_exceptions)*: Kara liste içinde bile çok yollu cihazlar olarak kabul edilmesi için belirli cihazları tanımlayabilir.
- **Depolama denetleyicisi belirli ayarları** *(cihazlar)*: Üretici ve ürün bilgilerine sahip cihazlara uygulanacak yapılandırma ayarlarını belirtebilirsiniz.
- **Özel cihaz ayarları** *(multipaths)*: Bu bölümde, tek tek LUN'ları için yapılandırma ayarlarını ince ayar yapmak için kullanabilirsiniz.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Çoklu yol oluşturma StorSimple Linux konağına bağlı yapılandırın
Bir Linux konağına bağlı bir StorSimple cihazına, yüksek kullanılabilirlik ve Yük Dengeleme için yapılandırılabilir. Örneğin, iki arabirim SAN'a bağlı Linux ana varsa ve cihazın bu arabirimleri aynı alt ağda olan şekilde SAN'a bağlı iki arabirim, ardından sunulacağına 4 yol. Cihaz ve ana bilgisayar arabirimi her veri arabiriminde farklı bir IP alt ağı (ve değil yönlendirilebilir) varsa, ancak ardından yalnızca 2 yolları kullanılabilir. Otomatik olarak kullanılabilir tüm yolları Bul, bu yollar için bir Yük Dengeleme algoritması seçin, birimler, yalnızca StorSimple için belirli yapılandırma ayarlarını uygulayın ve ardından etkinleştirmek ve çoklu yol oluşturma doğrulamak için çoklu yol oluşturma yapılandırabilirsiniz.

Aşağıdaki yordam bir ana bilgisayara iki ağ arabirimi ile iki ağ arabirimi ile bir StorSimple cihazı bağlı olduğunda çoklu yol oluşturma yapılandırma açıklar.

## <a name="prerequisites"></a>Önkoşullar
Bu bölümde CentOS sunucu ve StorSimple cihazınız için yapılandırma önkoşulları açıklanmaktadır.

### <a name="on-centos-host"></a>CentOS konakta
1. CentOS konağınız 2 ağ arabirimi etkin olduğundan emin olun. Şunu yazın:
   
    `ifconfig`
   
    İki ağ arabirimleri, aşağıdaki örnek çıktı gösterir (`eth0` ve `eth1`) konakta yok.
   
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
1. Yükleme *iSCSI başlatıcısı utils* CentOS sunucunuzdaki. Yüklemek için aşağıdaki adımları gerçekleştirin *iSCSI başlatıcısı utils*.
   
   1. Oturum açma `root` , CentOS konağı.
   1. Yükleme *iSCSI başlatıcısı utils*. Şunu yazın:
      
       `yum install iscsi-initiator-utils`
   1. Sonra *iSCSI başlatıcısı utils* başarıyla yüklenen, iSCSI Hizmeti başlatın. Şunu yazın:
      
       `service iscsid start`
      
       Gereksinimlerdeki `iscsid` gerçekten başlatılamayabilir ve `--force` seçeneği gerekebilir
   1. Önyükleme işlemi sırasında iSCSI başlatıcısı etkinleştirildiğinden emin olmak için kullanın `chkconfig` hizmetini etkinleştirmek için komutu.
      
       `chkconfig iscsi on`
   1. Bu kurulum düzgün olduğunu doğrulamak için komutu çalıştırın:
      
       `chkconfig --list | grep iscsi`
      
       Örnek çıktı aşağıda gösterilmiştir.
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       Yukarıdaki örnekte, iSCSI ortamınızı önyükleme zamanında çalışma düzeyleri 2, 3, 4 ve 5 çalışacağını görebilirsiniz.
1. Yükleme *cihaz Eşleyici multipath*. Şunu yazın:
   
    `yum install device-mapper-multipath`
   
    Yükleme başlar. Tür **Y** onaylamanız istendiğinde devam etmek için.

### <a name="on-storsimple-device"></a>StorSimple cihazında
StorSimple Cihazınızı sahip olmanız gerekir:

* En az iki arabirimin iSCSI etkin. İki arabirim, StorSimple Cihazınızda iSCSI etkin olduğunu doğrulamak için StorSimple cihazınız için Azure Klasik portalında aşağıdaki adımları gerçekleştirin:
  
  1. StorSimple cihazınız için Klasik Portalı'nda oturum açın.
  1. StorSimple Yöneticisi hizmetine seçip **cihazları** ve belirli StorSimple cihazı seçin. Tıklayın **yapılandırma** ve ağ arabirimi ayarları doğrulayın. İki iSCSI etkin ağ arabirimine sahip bir ekran görüntüsü aşağıda gösterilmiştir. Burada veri 2 ve DATA 3 arabirimleri için iSCSI etkin hem de 10 GbE.
     
      ![MPIO StorsSimple veri 2 yapılandırma](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO, StorSimple veri 3 yapılandırma](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      İçinde **yapılandırma** sayfası
     
     1. Her iki ağ arabirimi iSCSI özellikli olduğundan emin olun. **İSCSI özellikli** alan ayarlanmalıdır **Evet**.
     1. Ağ arabirimleri aynı hızınız ve her ikisi de 1 GbE veya 10 GbE olmalıdır emin olun.
     1. İSCSI etkin arabirimlerin IPv4 adreslerini Not ve konakta daha sonra kullanmak için kaydedin.
* StorSimple Cihazınızda iSCSI arabirimleri CentOS sunucudan erişilebilir olmalıdır.
      Bunu doğrulamak için ana bilgisayar sunucunuz üzerinde StorSimple iSCSI etkin ağ arabirimi IP adreslerini sağlamanız gerekir. Kullanılan komutlar ve veri2 karşılık gelen çıktıyla (10.126.162.25) ve DATA3 (10.126.162.26) aşağıda gösterilmiştir:
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a>Donanım yapılandırması
Artıklık için ayrı yollarında iki iSCSI ağ arabirimi bağlanmak öneririz. Aşağıdaki şekilde, yüksek kullanılabilirlik için önerilen donanım yapılandırması ve CentOS sunucunuz ile StorSimple cihazı için çoklu yol oluşturma Yük Dengeleme gösterilmektedir.

![Linux barındırması için StorSimple MPIO donanım yapılandırması](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Önceki şekilde gösterildiği gibi kullanın:

* StorSimple Cihazınızı bir Aktif-Pasif yapılandırmayı iki denetleyicileriyle bileşenidir.
* İki SAN anahtarları cihaz denetleyicileriniz ile bağlanır.
* StorSimple Cihazınızda iki iSCSI başlatıcılarının etkinleştirilir.
* İki ağ arabirimi, CentOS konakta etkin.

Yukarıdaki yapılandırma, ana bilgisayar ve veri arabirimleri yönlendirilebilir olması durumunda Cihazınızı ile konak arasındaki 4 ayrı yollar ortaya çıkarır.

> [!IMPORTANT]
> * 1 GbE ve 10 GbE ağ arabirimleri için çoklu yol oluşturma karıştırmamanızı öneririz. İki ağ arabirimi kullanırken her iki arabirimde aynı türde olmalıdır.
> * Oysa veri2 ve DATA3 10 GbE ağ arabirimleri StorSimple Cihazınızda DATA0, Veri1 DATA4 ve DATA5 1 GbE arabirimleri olan. |
> 
> 

## <a name="configuration-steps"></a>Yapılandırma adımları
Çoklu yol oluşturma etkinleştirme ve son olarak yapılandırma doğrulama, yapılandırma adımları için çoklu yol oluşturma kullanmak için Yük Dengeleme algoritması belirtme, otomatik bulma için kullanılabilen yolları yapılandırmayı içerir. Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak ele alınmıştır.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>1. Adım: Otomatik bulma için çoklu yol oluşturma yapılandırma
Çok yollu desteklenen cihazları otomatik olarak bulunan ve yapılandırılmış.

1. Başlatma `/etc/multipath.conf` dosya. Şunu yazın:
   
     `mpathconf --enable`
   
    Yukarıdaki komutu oluşturacak bir `sample/etc/multipath.conf` dosya.
1. Çok yollu hizmetini başlatın. Şunu yazın:
   
    `service multipathd start`
   
    Aşağıdaki çıktıyı görürsünüz:
   
    `Starting multipathd daemon:`
1. Multipaths otomatik olarak bulunmasını sağlar. Şunu yazın:
   
    `mpathconf --find_multipaths y`
   
    Bu varsayılan değerleri bölümünü değiştirir, `multipath.conf` aşağıda gösterildiği gibi:
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>2. Adım: StorSimple birimler için çoklu yol oluşturma yapılandırma
Varsayılan olarak, tüm cihazlar multipath.conf dosyasında listelenen siyah olur ve atlanır. StorSimple cihazlardan birimler için çoklu yol oluşturma izni kara liste özel durumlar oluşturmanız gerekir.

1. Düzen `/etc/mulitpath.conf` dosya. Şunu yazın:
   
    `vi /etc/multipath.conf`
1. Multipath.conf dosyasında blacklist_exceptions bölümünü bulun. StorSimple Cihazınızı Bu bölümde bir kara liste özel durum olarak listelenmesi gerekir. Bu dosyadaki (yalnızca belirli modeli kullandığınız cihazın kullanın) aşağıda gösterilen değiştirmek için ilgili satırlara açıklamasını kaldırın:
   
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

### <a name="step-3-configure-round-robin-multipathing"></a>3. Adım: Hepsini bir kez deneme çoklu yol oluşturma yapılandırma
Bu Yük Dengeleme algoritması etkin denetleyici için tüm kullanılabilir multipaths Dengeli, hepsini bir biçimde kullanır.

1. Düzen `/etc/multipath.conf` dosya. Şunu yazın:
   
    `vi /etc/multipath.conf`
1. Altında `defaults` bölümünde, `path_grouping_policy` için `multibus`. `path_grouping_policy` Belirtilmeyen multipaths için uygulanacak ilke gruplandırma varsayılan yolunu belirtir. Varsayılanları bölümü, aşağıda gösterildiği gibi görünecektir.
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> En yaygın değerlerini `path_grouping_policy` içerir:
> 
> * Yük devretme öncelik grubu başına 1 yol =
> * multibus = 1 öncelikli grubundaki tüm geçerli yolları
> 
> 

### <a name="step-4-enable-multipathing"></a>4. Adım: Çoklu yol oluşturma etkinleştir
1. Yeniden `multipathd` arka plan programı. Şunu yazın:
   
    `service multipathd restart`
1. Çıkış, aşağıda gösterildiği gibi olacaktır:
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a>5. Adım: Çoklu yol oluşturma doğrulayın
1. Önce iSCSI bağlantı ile StorSimple cihazı gibi kurulduğundan emin olun:
   
   a. StorSimple Cihazınızı keşfedin. Şunu yazın:
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>
    ```
    
    Çıktı 10.126.162.25 DATA0 için IP adresi ve bağlantı noktası 3260'ın giden iSCSI trafiği için StorSimple cihazında açıldığında, aşağıda gösterildiği gibi olacaktır:
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    StorSimple Cihazınızı IQN'ini kopyalama `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, yukarıdaki çıktıya.

   b. Hedef IQN kullanarak cihaza bağlayın. StorSimple, iSCSI hedefi burada cihazdır. Şunu yazın:

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    Aşağıdaki örnek, bir hedef IQN çıktıyla gösterir, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. Cihazınızda iki iSCSI etkin ağ arabirimlerine başarıyla bağlandınız çıktıyı gösterir.

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

    Yalnızca bir sunucu arabirimi ve iki yollarını buradan görürseniz, arabirimler konakta iSCSI için etkinleştirmeniz gerekir. İzleyebileceğiniz [ayrıntılı Linux belgelerindeki yönergeleri](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).

1. Bir birim CentOS sunucunun StorSimple cihazından kullanıma sunulur. Daha fazla bilgi için [adım 6: Birim oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple cihazınızdaki Azure portal aracılığıyla.

1. Kullanılabilir yolları doğrulayın. Şunu yazın:

      ```
      multipath –l
      ```

      Aşağıdaki örnek iki kullanılabilir yolları ile tek bir konak ağ arabirimi için bağlı bir StorSimple cihazına iki ağ arabirimi için çıktıyı gösterir.

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
Bu bölümde, çoklu yol oluşturma yapılandırması sırasında herhangi bir sorunla karşılaşırsanız çalıştırırsanız, bazı yararlı ipuçları sağlanır.

S. Değişiklikleri görüntülenmemesini `multipath.conf` devreye dosya.

A. Herhangi bir değişiklik yaptıysanız `multipath.conf` dosyası oluşturmanız gerekir çoklu yol oluşturma hizmetini yeniden başlatın. Aşağıdaki komutu yazın:

    service multipathd restart

S. StorSimple cihazında iki ağ arabirimi ve iki ağ arabirimi konakta etkin. Ben kullanılabilir yolları listesi, yalnızca iki yolu konusuna bakın. Dört kullanılabilir yollarına da bakın beklenir.

A. Yönlendirilebilir ve iki yolu aynı alt ağda olduğundan emin olun. Ağ arabirimleri farklı VLAN'lara ve yönlendirilebilir değil, yalnızca iki yolu görürsünüz. Bu doğrulamanın bir yolu, StorSimple cihazında bir ağ arabirimi hem de konak arabirimlerden erişebildiğinden emin olmaktır. Şunları yapmanız gerekir [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) olarak bu doğrulama yalnızca bir destek oturumu yapılabilir.

S. Ben kullanılabilir yolları listelediğinizde, herhangi bir çıktı görmezsiniz.

A. Genellikle, multipathed yollar göremiyor çoklu yol oluşturma daemon ile ilgili bir sorun önerir ve büyük olasılıkla burada herhangi bir sorun olduğunda emin olan `multipath.conf` dosya.

Ayrıca, çok yollu listelerden yanıt, ayrıca tüm diskleri yok gelebilir gibi gerçekten bazı diskler hedefine bağlandıktan sonra gördüğünüzden denetimi değer olacaktır.

* SCSI veri yoluna yeniden taramak için aşağıdaki komutu kullanın:
  
    `$ rescan-scsi-bus.sh` (sg3_utils paketinin bir parçası)
* Aşağıdaki komutları yazın:
  
    `$ dmesg | grep sd*`
     
     Veya
  
    `$ fdisk –l`
  
    Bu son eklenen diskler ayrıntılarını döndürür.
* Bir StorSimple disk olup olmadığını belirlemek için aşağıdaki komutları kullanın:
  
    `cat /sys/block/<DISK>/device/model`
  
    Bu, StorSimple disk olup olmadığını belirleyen bir dize döndürür.

Büyük olasılıkla ancak olası nedeni daha az bir eski iscsid da olabilir PID. ' Dan iSCSI oturumları oturumunu kapatmak için aşağıdaki komutu kullanın:

    iscsiadm -m node --logout -p <Target_IP>

StorSimple cihazınız iSCSI hedef tüm bağlı ağ arabirimleri için bu komutu yineleyin. Tüm iSCSI oturumları açtıktan sonra iSCSI hedef IQN iSCSI oturumu yeniden kurmak için kullanın. Aşağıdaki komutu yazın:

    iscsiadm -m node --login -T <TARGET_IQN>


S. Cihazımı izin verilenler listesinde olup olmadığından emin değilim.

A. Cihazınızı izin verilenler listesinde olup olmadığını doğrulamak için aşağıdaki sorun giderme etkileşimli komutunu kullanın:

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

## <a name="list-of-useful-commands"></a>Yararlı komut listesi
| Type | Komut | Açıklama |
| --- | --- | --- |
| **iSCSI** |`service iscsid start` |İSCSI Hizmeti |
| &nbsp; |`service iscsid stop` |İSCSI hizmetini durdurun |
| &nbsp; |`service iscsid restart` |İSCSI hizmetini yeniden başlatın |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |Belirtilen adres kullanılabilir hedeflerde keşfedin |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |İSCSI hedef oturum açın |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |İSCSI hedef Oturumu Kapat |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |İSCSI başlatıcısı adı yazdırma |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |Konakta bulunan birim ve iSCSI oturumu durumunu denetleyin |
| &nbsp; |`iscsi –m session` |Konak ve StorSimple cihaz arasında kurulan tüm iSCSI oturumları gösterir |
|  | | |
| **Çoklu yol oluşturma** |`service multipathd start` |Çok yollu arka plan programı Başlat |
| &nbsp; |`service multipathd stop` |Çok yollu arka plan programı durdurun |
| &nbsp; |`service multipathd restart` |Çok yollu Daemon programını yeniden başlatın |
| &nbsp; |`chkconfig multipathd on` </br> OR </br> `mpathconf –with_chkconfig y` |Önyükleme sırasında başlatmak çok yollu arka plan programı etkinleştir |
| &nbsp; |`multipathd –k` |Sorun giderme için etkileşimli Konsolu Başlat |
| &nbsp; |`multipath –l` |Liste çok yollu bağlantılar ve cihazları |
| &nbsp; |`mpathconf --enable` |Bir örnek mulitpath.conf dosyasında oluşturma `/etc/mulitpath.conf` |
|  | | |

## <a name="next-steps"></a>Sonraki adımlar
Linux ana bilgisayarında MPIO yapılandırma gibi aşağıdaki CentoS 6.6 belgelere bakın gerekebilir:

* [CentOS MPIO ayarlama](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [Linux eğitim Kılavuzu](http://linux-training.be/linuxsys.pdf)

