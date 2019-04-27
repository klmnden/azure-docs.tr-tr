---
title: Microsoft Azure StorSimple 8100 cihaz yükleme | Microsoft Docs
description: Cihazınızı kutusundan çıkarma, rafa takma ve dağıtmak ve yazılım yapılandırma önce StorSimple 8100 cihazınızın kablolarını bağlama açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: b367b6e7126a442dc68646ff52a29c955f50b798
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631235"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Cihazınızı kutusundan çıkarma, rafa monte ve StorSimple 8100 cihazınızın kablolarını bağlama
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8100 tek kutu, rafa monte edilen cihaz olur. Bu öğreticide, ambalajını açmak açıklanmaktadır rafa monte yanı sıra, yapılandırmak ve StorSimple Cihazınızı dağıtma önce StorSimple 8100 kablo cihaz donanım.

## <a name="unpack-your-storsimple-8100-device"></a>StorSimple 8100 model Cihazınızı kutusundan çıkarma
Aşağıdaki adımlar, StorSimple 8100 depolama Cihazınızı paketinden çıkarma hakkında NET, ayrıntılı yönergeler sağlar. Cihaz tek bir kutuda gönderilir.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızı paketinden çıkarma hazırlama
Cihazınızı paketinden çıkarma önce aşağıdaki bilgileri gözden geçirin.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

1. El ile işlemekte olduğunuz kasa ağırlığı yönetmek için kullanılabilecek iki kişinin bilgisayarınızda yüklü olduğundan emin olun. Tam olarak yapılandırılmış bir kutu, en fazla 32 kg (70'ten lb.) Tart.
2. Kutuyu düz ve sabit bir yüzeye yerleştirin.

Ardından, Cihazınızı paketinden çıkarma için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızı paketinden çıkarma için
1. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalajda ciddi hasar varsa kutuyu açmayın. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) , cihazın iyi çalışma sırayla olup değerlendirmenize yardımcı olmak için.
2. Kutuyu açın. Aşağıdaki görüntüde, StorSimple Cihazınızı paketten çıkarılan görünümünü gösterir.
   
     ![Depolama Cihazınızı paketinden çıkarma](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Depolama Cihazınızı paketten çıkarılan görünümü**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Paketleme kutusu |
   |   2 |Alt köpük |
   |   3 |Cihaz |
   |   4 |Üst köpük |
   |   5 |Aksesuar kutusu |
3. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
   
   * 1 tek kutu cihaz
   * 2 güç kablosu
   * 1 çakışma Ethernet kablosu
   * 2 seri konsol kabloları
   * seri erişim için 1 USB seri dönüştürücü
   * 1 artıklığının T10 tornavida
   * 4 QSFP-için-SFP + bağdaştırıcıları 10 GbE ağ arabirimleri ile kullanmak için
   * 1 raf-Seti (donanım bağlama ile 2 yan rails) bağlama
   * Başlangıç belgeleri alma
     
     Yukarıda listelenen öğelerden herhangi birini almadı, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

Bir sonraki adım cihazınızı rafa yerleştirmektir.

## <a name="rack-mount-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızın rafa monte
StorSimple 8100 depolama cihazınızın standart 19 inç raf ön ve arka gönderiler ile yüklemek için sonraki adımları izleyin. StorSimple 8100 cihazda tek bir birincil kutu vardır.

Her biri aşağıdaki yordamlarda açıklanan birden çok adım yükleme oluşur.

> [!IMPORTANT]
> StorSimple cihazları, rafa monte düzgün çalışması için olmalıdır.
> 
> 

### <a name="prepare-the-site"></a>Site hazırlama
Cihazın ön ve arka gönderileri olan standart bir 19 inç rafa yüklü olması gerekir. Raf yüklemesine hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Site, raf yüklemesine hazırlanmak için
1. Cihazın düz, sabit ve dengeli bir çalışma yüzeyi (veya benzeri) üzerinde güvenli bir şekilde durduğundan emin olun.
2. Burada ayarlamak için istediğinize site bağımsız bir kaynak veya bir raf güç dağıtım birimi (PDU) bir kesintisiz güç kaynağı (UPS) ile standart AC gücü sahip olduğunu doğrulayın.
3. 2U yuvanın bir cihaz bağlamak istediğiniz rafa kullanılabilir olduğundan emin olun.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

Cihaz Kurulumu el ile işleniyorsa ağırlığı yönetmek için kullanılabilen iki kişinin bilgisayarınızda yüklü olduğundan emin olun. Tam olarak yapılandırılmış bir kutu, en fazla 32 kg (70'ten lb.) Tart.

### <a name="rack-prerequisites"></a>Raf önkoşulları
8100 muhafaza dolap ile standart 19 inç raf yüklenmesi için tasarlanmıştır:

* Raf post için post 27.84 inç en düşük derinliği.
* En yüksek ağırlığı cihazın 32 kg
* En fazla ters baskı denen, 5 Pascal (0,5 mm su ölçer).

### <a name="rack-mounting-rail-kit"></a>Raf montaj parmaklık Kiti
Rails bağlama kümesi 19 inç raf dolap ile kullanım için sağlanır. Rails maksimum kasa ağırlığı işlemek üzere test edilmiştir. Bu rails raftaki alanının herhangi bir kayıp olmadan birden çok kasaları yüklenmesini de izin verir.

#### <a name="to-install-the-device-on-the-rails"></a>Cihaz on rails yüklemek için
1. Yalnızca iç rails Cihazınızda yüklü değilse, bu adımı gerçekleştirin. Genellikle, iç rails fabrikada yüklenir. Rails yüklü değilse, parmaklık sol ve sağ parmaklık slaytları raylar yanlarını yükleyin. Bunlar, her bir tarafta altı ölçüm Vida kullanarak ekleyin. Parmaklık slaytları yönle yardımcı olmak için işaretlenmiş **LH – ön** ve **RH – ön**, ve muhafaza arka yapıştırılmış son Konik son sahiptir.<br/>
   
    ![Kutu gövdesine Sürgülü raylar ekleme](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **İç kutu gövdesine Sürgülü muhafaza yanlarını ekleme**
   
    Etiket | Açıklama
    ----- | -----------
    1     | M 3 x 4 düğmesi baş Vida
    2     | Kasa slayt

2. Sol dış parmaklık ve dış doğru parmaklık derlemeleri raf dolap dikey üyeleri ekleyin. Köşeli ayraçlar işaretlenmiş **LH**, **RH**, ve **bu tarafı yukarı** doğru yönlendirmeyi size yol gösterecek.
3. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Parmaklık arasında rafa gönderileri uygun ve PIN'leri ön ve arka raf post dikey üye boşluklarını eklemek için genişletin. Parmaklık derleme düzeyinde olduğundan emin olun.
4. İki sağlanan ölçüm Vida rafa parmaklık derlemeye dikey üyeleri güvenliğini sağlamak için kullanın. Ön ve arka birinde tek Sarmal kullanın.
5. Diğer parmaklık derleme için bu adımları yineleyin.<br/>
   
     ![Raflı dolaba Sürgülü raylar ekleme](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Dış parmaklık derlemeler için raf ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Clamping Sarmal |
   |   2 |Köşeli çift yuvalı ön raf post Sarmal |
   |   3 |Sol parmaklık ön konumu PIN'ler |
   |   4 |Clamping Sarmal |
   |   5 |Sol parmaklık arka konumu PIN'ler |

### <a name="mounting-the-device-in-the-rack"></a>Cihazı rafa takma
Yeni yüklenen raf rails kullanarak cihazı rafa bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-device"></a>Bir cihaz bağlamak için
1. Bir yardımcı olan muhafaza lift- and ile raf rails Hizala.
2. Dikkatli bir şekilde cihaz rails eklemek ve ardından cihazın tamamen rafa dolap gönderin.<br/>
   
    ![Cihazı rafa yerleştirme](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Cihazı rafa takma**
3. Flanş sol ve sağ ön uçlara ücretsiz caps çekerek kaldırın. Flanş caps yalnızca çıkıntıları yapışır.
4. Rafa muhafazada sol ve sağ her Flanş aracılığıyla sağlanan bir Phillips baş Sarmal yükleyerek güvenli hale getirin.
5. Bunları konumuna tuşuna basarak ve bunları yerinde yaslama Flanş caps yükleyin.<br/>
   
     ![Flanş başlıklarını](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Kasa birleşme Sarmal |

Sonraki adım, güç, ağ ve seri erişim için cihazınızın kablolarını bağlama sağlamaktır.

## <a name="cable-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızın kablolarını bağlama
Aşağıdaki yordamlarda, güç, ağ ve seri bağlantı için StorSimple 8100 cihazınızın kablolarını bağlama işlemleri açıklanmaktadır.

### <a name="prerequisites"></a>Önkoşullar
Cihazınızın kablolarını bağlamaya başlamadan önce şunlara ihtiyacınız olacaktır:

* Depolama cihazı tamamen açılmış ve raf bağlanır.
* cihazınızla birlikte gelen 2 güç kabloları
* 2 güç dağıtım birimleri (önerilen) erişim.
* Ağ kablosu
* Sağlanan seri kablo
* (Gerekirse) Bilgisayarınızda yüklü uygun sürücüsü ile seri USB dönüştürücü
* 4 QSFP sağlanan-için-SFP + bağdaştırıcıları 10 GbE ağ arabirimleri ile kullanmak için
* [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Güç kabloları
Cihazınız, gereksiz güç ve soğutma modülleri (PCMs) içerir. Hem PCMs yüklenmeli ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlandınız.

Güç için cihazınızın kablolarını bağlama için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Ağ kablosunun
Cihazınızı bir etkin bekleme yapılandırmadır: herhangi bir zamanda bir denetleyici modülü etkin ve bekleme olan tüm disk ve ağ Denetleyici Modülü hata işleme. Bir denetleyici başarısız olursa, hazır bekleyen denetleyicinin hemen etkinleştirilir ve tüm disk ve ağ işlemleri devam eder.

Bu yedekli denetleyici yük devretmesi desteklemek için aşağıdaki adımlarda açıklandığı gibi cihaz ağınıza kablo gerekir.

#### <a name="to-cable-for-network-connection"></a>Kablo ağ bağlantısı
1. Cihazınızı her denetleyicisinde altı ağ arabirimi bulunur: dört 1 Gbps ve iki 10 GB/sn Ethernet bağlantı noktası. Devre kartına cihazınızın çeşitli veri noktalarına belirleyin.
   
    ![8100 cihazının devre kartı](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Veri bağlantı noktaları gösteren cihazının arkasına**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Aşağıdaki diyagramda ağ kablosunun için bkz. (En düşük ağ yapılandırmasını düz mavi çizgilerle gösterilir. Yüksek kullanılabilirlik ve performans için gereken ek yapılandırma noktalı çizgiyle gösterilir.)

    ![Kabloyla 2U cihazınızın ağ bağlantısını yapın](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Cihazınız için kablo ağı**

   |Etiket | Açıklama |
   |----- | ----------- |
   | A    | Internet erişimi olan LAN |
   | B    | Denetleyici 0 |
   | C    | PCM 0'DA |
   | D    | Denetleyici 1 |
   | E    | PCM 1 |
   | F, G | Ana bilgisayarlar |
   | 0-5  | Ağ arabirimleri |



Cihaz kablo, en düşük yapılandırmayı gerektirir:

* Her Denetleyici ile bulut erişimi için diğeri iSCSI için en az iki ağ arabirimi bağlı. Veri 0 bağlantı noktası otomatik olarak etkin ve cihaz seri Konsolu yapılandırılır. Veri 0 dışında başka bir veri bağlantı noktası da Klasik Azure portalı yapılandırılması gerekir. Bu durumda, veri 0 bağlama bağlantı noktası birincil yerel ağa (Internet erişimi olan ağ). Diğer veri bağlantı noktalarına SAN/iSCSI LAN (VLAN) kesimi hedeflenen rolü bağlı olarak ağ bağlantılı olabilir.
* Her denetleyici aynı arabirimlerde, denetleyici yük devretme gerçekleşirse kullanılabilirliğini sağlamak için aynı ağa bağlı. Örneğin, DATA 0 ve veri 3 denetleyicilerin birine bağlanmayı seçerseniz, ilgili veri 0 ve veri 3 diğer denetleyiciye bağlanmanız gerekmez.

Yüksek kullanılabilirlik ve performans için göz önünde bulundurun:

* Mümkün olduğunda, bir çift ağ arabirimi için bulut erişimi'ni (1 GbE) ve iSCSI (10 GbE önerilir) için başka bir çift her denetleyicisinde yapılandırın.
* Mümkün olduğunda, ağ arabirimlerinin her denetleyicisinden anahtar hataya karşı kullanılabilirliğini sağlamak için iki farklı anahtarlara bağlayın. Şekilde gösterilmektedir iki 10 GbE ağ arabirimleri, veri 2 ve DATA 3 ', iki farklı anahtarlara bağlı her denetleyicisinden.

Daha fazla bilgi için **ağ arabirimleri** altında [StorSimple cihazınız için yüksek kullanılabilirlik gereksinimleri](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> İle 10 GbE ağ arabirimleri SFP + vericilerinin kullanıyorsanız, sağlanan QSFP kullanın-SFP + bağdaştırıcıları. Daha fazla bilgi için Git [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı noktası kabloları
Seri bağlantı kablolarını bağlama için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kablo
1. Cihazınızın seri bağlantı noktası İngiliz anahtarı simgesi tarafından tanımlanan her denetleyicisinde sahiptir. Çizimde başvurmak [ağ kablosunun](#network-cabling) devre kartına cihazınızın seri bağlantı noktalarını bulmak için bölüm.
2. Etkin denetleyicide, aygıt devre kartı belirleyin. Yanıp sönen bir mavi LED denetleyici etkin olduğunu gösterir.
3. (Gerekirse, dizüstü bilgisayarınız için USB seri dönüştürücü) sağlanan seri kablo kullanın ve konsolu veya (terminal öykünme cihaza ile) bilgisayarınızın etkin denetleyiciyi seri bağlantı noktasına bağlanın.
4. (Cihazı ile birlikte gelen) seri USB sürücüleri bilgisayarınıza yükleyin.
5. Seri bağlantı aşağıdaki gibi ayarlayın: 115.200 baud, 8 veri bitleri, 1 dur bit, eşlik yok ve akış kümesi hiçbiri denetler.
6. Bağlantı Konsolu'nda Enter tuşuna basarak çalıştığını doğrulayın. Seri konsol menüsünde görüntülenmelidir.

> [!NOTE]
> **Uzaktan Yönetim**: Cihaz uzak bir veri merkezinde veya sınırlı erişimli bilgisayar odasında yüklendiğinde, her iki denetleyicilerinin seri bağlantıları her zaman bir seri konsol anahtarı veya benzer ekipman bağlandığınızdan emin olun. Ağ kesintileri veya beklenmeyen hatalar varsa bu bant uzaktan denetim ve Destek işlemleri sağlar.
> 
> 

Cihazınız artık güç, ağ erişimi ve seri bağlantı kablolu. Sonraki adım, Cihazınızı dağıtmak ve yazılım yapılandırma sağlamaktır.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [şirket içi StorSimple Cihazınızı yapılandırmak ve dağıtmak](storsimple-8000-deployment-walkthrough-u2.md).

