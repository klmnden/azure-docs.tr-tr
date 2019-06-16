---
title: Microsoft Azure StorSimple 8600 cihaz yükleme | Microsoft Docs
description: Cihazınızı kutusundan çıkarma, rafa takma ve dağıtmak ve yazılım yapılandırma önce StorSimple 8600 cihazınızın kablolarını bağlama açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: be3f68a00647840801e7c205d7abb34b718bd61c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630946"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Cihazınızı kutusundan çıkarma, rafa monte ve StorSimple 8600 cihazınızın kablolarını bağlama
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8600 bir çift muhafaza cihazıdır ve birincil ve EBOD muhafazası oluşur. Bu öğreticide paketten açıklanmaktadır, rafa monte ve StorSimple 8600 kablo cihaz donanım, önce StorSimple yazılım yapılandırın.

## <a name="unpack-your-storsimple-8600-device"></a>StorSimple 8600 model Cihazınızı kutusundan çıkarma
Aşağıdaki adımlar, StorSimple 8600 depolama Cihazınızı paketinden çıkarma konusunda açık ve ayrıntılı yönergeler sağlar. Bu cihaz, iki kutularında, biri muhafaza birincil ve EBOD muhafazası için başka sevk edilir. Bu iki kutu daha sonra tek bir kutu içinde yer alır.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızı paketinden çıkarma hazırlama
Cihazınızı paketinden çıkarma önce aşağıdaki bilgileri gözden geçirin.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

1. El ile işleme cihaz ağırlığını yönetmek için kullanılabilecek iki kişinin bilgisayarınızda yüklü olduğundan emin olun. Tam olarak yapılandırılmış bir kutu, en fazla 32 kg (70'ten lb.) Tart.
2. Kutuyu düz ve sabit bir yüzeye yerleştirin.

Ardından, Cihazınızı paketinden çıkarma için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızı paketinden çıkarma için
1. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalajda ciddi hasar varsa kutuyu açmayın. Lütfen [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md) , cihazın iyi çalışma sırayla olup değerlendirmenize yardımcı olmak için.
2. Dış kutusunu açın ve ardından birincil ve ebod için karşılık gelen iki kutu kullanıma alın. Artık birincil ve ebod monte edebilir. Aşağıdaki şekilde kasaları birini paketten çıkarılan görünümünü gösterir.
   
    ![Depolama Cihazınızı paketinden çıkarma](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Depolama Cihazınızı paketten çıkarılan görünümü**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Paketleme kutusu |
   |   2 |SAS kablolu jbod'lere (tepsisinde Donatılar ve kablolar) |
   |   3 |Alt köpük |
   |   4 |Cihaz |
   |   5 |Üst köpük |
   |   6 |Aksesuar kutusu |
3. İki kutu akışının paketi açılırken sonra sahip olduğunuzdan emin olun:
   
   * 1 birincil kasası (birincil ve EBOD muhafazası iki ayrı kutularında olan)
   * EBOD muhafazası 1
   * Her kutusunda 2, 4 güç kablosu
   * (EBOD muhafazası için birincil kasası bağlamak için) 2 SAS kabloları
   * 1 çakışma Ethernet kablosu
   * 2 seri konsol kabloları
   * seri erişim için 1 USB seri dönüştürücü
   * 4 QSFP-için-SFP + bağdaştırıcıları 10 GbE ağ arabirimleri ile kullanmak için
   * 2 bağlama setleri (donanım, 2 her birincil ve EBOD muhafazası bağlama ile 4 yan rails), raf 1 her kutusu
   * Kullanmaya başlama belgeleri alma
     
     Yukarıda listelenen öğelerden herhangi birini almadı, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).  

Bir sonraki adım cihazınızı rafa yerleştirmektir.

## <a name="rack-mount-your-storsimple-8600-device"></a>Rafa monte StorSimple 8600 cihazınızın
Standart 19 inç raf ön ve arka gönderiler ile StorSimple 8600 depolama cihazınızın yüklemek için sonraki adımları izleyin. Bu cihaz iki kasaları ile birlikte gelir: birincil muhafaza ve EBOD muhafazası. Bunların her ikisi de rafa monte edilen gerekir.

Her biri aşağıdaki yordamlarda açıklanan birden çok adım yükleme oluşur.

> [!IMPORTANT]
> StorSimple cihazları, rafa monte düzgün çalışması için olmalıdır.
> 
> 

### <a name="site-preparation"></a>Site Hazırlık
Ön ve arka gönderileri olan standart bir 19 inç rafa kapsayıcıları'nın yüklü olması gerekir. Raf yüklemesine hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Site, raf yüklemesine hazırlanmak için
1. Birincil ve ebod işi olmayan güvenli bir şekilde düz, kararlı ve düzeyi çalışma yüzeyinde (veya benzeri) olduğundan emin olun.
2. Burada ayarlamak için istediğinize site bağımsız bir kaynak veya bir kesintisiz güç kaynağı (UPS) ile bir raf üstü güç dağıtım birimi (PDU) standart AC gücü sahip olduğunu doğrulayın.
3. Bu bir 4U (2 X 2U) yuvası kasaları bağlamak istediğiniz rafa kullanılabilir olduğundan emin olun.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

 Cihaz Kurulumu el ile işleniyorsa ağırlığı yönetmek için kullanılabilen iki kişinin bilgisayarınızda yüklü olduğundan emin olun. Tam olarak yapılandırılmış bir kutu, en fazla 32 kg (70'ten lb.) Tart.

### <a name="rack-prerequisites"></a>Raf önkoşulları
Kasaları dolap ile standart 19 inç raf yüklenmesi için tasarlanmıştır:

* Raf post için post 27.84 inç en düşük derinliği
* En yüksek ağırlığı cihazın 32 kg
* En fazla ters baskı denen, 5 Pascal (su ölçer 0,5 mm)

### <a name="rack-mounting-rail-kit"></a>Raf montaj parmaklık Kiti
Rails bağlama kümesi 19 inç raf dolap ile kullanım için sağlanır. Rails maksimum kasa ağırlığı işlemek üzere test edilmiştir. Bu rails raftaki alanı kaybı olmadan birden çok kasaları yüklenmesini de izin verir. EBOD muhafazası ilk yükleyin.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>EBOD muhafazası on rails yüklemek için
1. Yalnızca iç rails Cihazınızda yüklü değilse, bu adımı gerçekleştirin. Genellikle, iç rails fabrikada yüklenir. Rails yüklü değilse, parmaklık sol ve sağ parmaklık slaytları raylar yanlarını yükleyin. Bunlar, her bir tarafta altı ölçüm Vida kullanarak ekleyin. Parmaklık slaytları yönle yardımcı olmak için işaretlenmiş **LH – ön** ve **RH – ön**, ve muhafaza arka yapıştırılmış son Konik son sahiptir.
   
    ![Kutu gövdesine Sürgülü raylar ekleme](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Kutu gövdesine Sürgülü muhafaza yanlarını ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |  1 |M 3 x 4 düğmesi baş Vida |
   |  2 |Kasa slayt |
2. Doğru parmaklık derlemeleri ve Sol parmaklık raf dolap dikey üyeleri ekleyin. Köşeli ayraçlar işaretlenmiş **LH**, **RH**, ve **bu tarafı yukarı** doğru yön size yol gösterecek.
3. Ray tertibatının ön ve arkasındaki ray pimlerini bulun. Parmaklık arasında rafa gönderileri uygun ve PIN'leri ön ve arka raf post dikey üye boşluklarını eklemek için genişletin. Parmaklık derleme düzeyinde olduğundan emin olun.
4. Rafa parmaklık derlemeye dikey üyeleri iki sağlanan ölçüm Vida kullanarak güvenli hale getirin. Ön ve arka birinde tek Sarmal kullanın.
5. Diğer parmaklık derleme için bu adımları yineleyin.
   
     ![Raflı dolaba Sürgülü raylar ekleme](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Parmaklık derlemeler için raf ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Clamping Sarmal |
   |   2 |Köşeli çift yuvalı ön raf post Sarmal |
   |   3 |Ön parmaklık konumu PIN'ler sol |
   |   4 |Clamping Sarmal |
   |   5 |Sol arka parmaklık konumu PIN'ler |

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>EBOD muhafazası raftaki bağlama
Yeni yüklenen raf rails kullanarak raftaki EBOD muhafazası bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-ebod-enclosure"></a>EBOD muhafazası bağlamak için
1. Bir yardımcı olan muhafaza lift- and ile raf rails Hizala.
2. Dikkatli bir şekilde muhafaza rails eklemek ve ardından bunu tamamen rafa dolap gönderin.
   
    ![Cihazı rafa yerleştirme](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Raf, kasa oluşturma**
3. Flanş sol ve sağ ön uçlara ücretsiz caps çekerek kaldırın. Flanş caps yalnızca çıkıntıları yapışır.
4. Kutu, sol ve sağ her Flanş aracılığıyla sağlanan bir Phillips baş Sarmal yükleyerek bir rafa güvenliğini sağlayın.
5. Bunları konumuna tuşuna basarak ve bunları bulundukları yere yaslanması anlamındadır Flanş caps yükleyin.
   
     ![Flanş başlıklarını](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Kasa birleşme Sarmal |

### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Raftaki birincil kasası oluşturma
EBOD muhafazası oluşturma işlemini tamamladıktan sonra aynı adımları izleyerek birincil kasası bağlamak gerekir.

> [!NOTE]
> * Raftaki muhafaza birincil ve EBOD muhafazası arasında birkaç boş yuvaya sahip mümkündür.
> * EBOD muhafazası için birincil muhafaza bağlanmak için sağlanan 2 milyon SAS kablosu kullanın.
> * EBOD birim baş birimine göreli yerleşimini hiçbir kısıtlamaları vardır. Üst yuvası ve aşağıdaki EBOD muhafazası birincil muhafaza bu nedenle, yerleştirilebilir — ya da tam tersi.
> 
> 

Sonraki adım, güç, ağ ve seri erişim için cihazınızın kablolarını bağlama sağlamaktır.

## <a name="cable-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızın kablolarını bağlama
Aşağıdaki yordamlarda, güç, ağ ve seri bağlantı için StorSimple 8600 cihazınızın kablolarını bağlama işlemleri açıklanmaktadır.

### <a name="prerequisites"></a>Önkoşullar
Cihazınızın kablolarını bağlama başlamadan önce ihtiyacınız olacak:

* Birincil, kasa ve EBOD muhafazası tamamen açılmış
* cihazınızla birlikte gelen 4 güç kabloları (2 her birincil ve EBOD muhafazası için)
* Cihaz EBOD muhafazası için birincil muhafaza bağlanmak için sağlanan 2 SAS kabloları
* 2 güç dağıtım birimleri (Pdu'lar) (önerilen) erişim
* Ağ kablosu
* Sağlanan seri kablo
* (Gerekirse) Bilgisayarınızda yüklü uygun sürücüsü ile USB seri dönüştürücü
* 4 QSFP sağlanan-için-SFP + bağdaştırıcıları 10 GbE ağ arabirimleri ile kullanmak için
* [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS ve güç kabloları
Cihazınızı birincil muhafaza hem EBOD muhafazası sahiptir. Bu birimlerin birlikte seri ekli SCSI (SAS) bağlantı ve güç için kablolu gerektirir.

Bu cihazı ilk kez ayarlarken, ilk SAS kabloları adımları uygulayın ve ardından power kablo adımlarını tamamlayın.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Ağ kablosunun
Cihazınız bir etkin bekleme yapılandırmasında.: herhangi bir zamanda bir denetleyici modülü etkin ve bekleme olan tüm disk ve ağ Denetleyici Modülü hata işleme. Bir denetleyici hata oluşursa, hazır bekleyen denetleyicinin hemen etkinleştirir ve tüm disk ve ağ işlemleri devam eder.

Bu yedekli denetleyici yük devretmesi desteklemek için aşağıdaki adımlarda gösterildiği gibi cihaz ağ kablosu gerekir.

#### <a name="to-cable-for-network-connection"></a>Kablo ağ bağlantısı
1. Cihazınızı her denetleyicisinde altı ağ arabirimi bulunur: dört 1 GB/sn ve iki 10 GB/sn Ethernet bağlantı noktası. Devre kartına cihazınızın veri bağlantı noktalarını tanımlamak için aşağıdaki resimde bakın.
   
     ![8600 cihazının devre kartı](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Cihazınızın geri veri bağlantı noktalarını gösterme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Aşağıdaki diyagramda ağ kablosunun için bkz. (En düşük ağ yapılandırmasını düz mavi çizgilerle gösterilir. Yüksek kullanılabilirlik ve performans için ek yapılandırma gerekli noktalı çizgiyle gösterilen.)

![Kabloyla 4U cihazınızın ağ bağlantısını yapın](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Cihazınız için kablo ağı**

| Etiket | Açıklama |
| --- | --- |
| A |Internet erişimi olan LAN |
| B |Denetleyici 0 |
| C |PCM 0 |
| D |Denetleyici 1 |
| E |PCM 1 |
| F |EBOD Denetleyicisi 0 |
| G |EBOD denetleyicisi 1 |
| H, I |Ana bilgisayar (örneğin, dosya sunucuları) |
| 0-5 |Ağ arabirimleri |
| 6 |Birincil |
| 7 |EBOD muhafazası |

Cihaz kablo, en düşük yapılandırmayı gerektirir:

* Her Denetleyici ile bulut erişimi için diğeri iSCSI için en az iki ağ arabirimi bağlı. Veri 0 bağlantı noktası otomatik olarak etkin ve cihaz seri Konsolu yapılandırılır. Veri 0 dışında başka bir veri bağlantı noktası da Klasik Azure portalı yapılandırılması gerekir. Bu durumda, veri 0 bağlama bağlantı noktası birincil yerel ağa (Internet erişimi olan ağ). Diğer veri bağlantı noktalarına SAN/iSCSI LAN (VLAN) kesimi hedeflenen rolü bağlı olarak ağ bağlantılı olabilir.
* Her denetleyici aynı arabirimlerde, denetleyici yük devretme gerçekleşirse kullanılabilirliğini sağlamak için aynı ağa bağlı. Örneğin, DATA 0 ve veri 3 denetleyicilerin birine bağlanmayı seçerseniz, ilgili veri 0 ve veri 3 diğer denetleyiciye bağlanmanız gerekmez.

Yüksek kullanılabilirlik ve performans için göz önünde bulundurun:

* Mümkün olduğunda, bir çift ağ arabirimi için bulut erişimi'ni (1 GbE) ve iSCSI (10 GbE önerilir) için başka bir çift her denetleyicisinde yapılandırın.
* Mümkün olduğunda, ağ arabirimlerinin her denetleyicisinden anahtar hataya karşı kullanılabilirliğini sağlamak için iki farklı anahtarlara bağlayın. Şekilde gösterilmektedir iki 10 GbE ağ arabirimleri, veri 2 ve DATA 3 ', iki farklı anahtarlara bağlı her denetleyicisinden. Daha fazla bilgi için **ağ arabirimleri** altında [StorSimple cihazınız için yüksek kullanılabilirlik gereksinimleri](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> SFP + vericilerinin, 10 GbE ağ arabirimleri ile kullanıyorsanız, sağlanan QSFP kullanın-SFP + bağdaştırıcıları. Daha fazla bilgi için Git [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı noktası kabloları
Seri bağlantı kablolarını bağlama için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kablo
1. Cihazınızın seri bağlantı noktası İngiliz anahtarı simgesi tarafından tanımlanan her denetleyicisinde sahiptir. Seri bağlantı noktalarını bulmak için veri bağlantı noktaları cihazınızın arkasında gösteren çizimde bakın.
2. Etkin denetleyicide, aygıt devre kartı belirleyin. Yanıp sönen bir mavi LED denetleyici etkin olduğunu gösterir.
3. (Gerekirse, dizüstü bilgisayarınız için USB seri dönüştürücü) sağlanan seri kabloyu kullanın ve konsolu veya (terminal öykünme cihaza ile) bilgisayarınızın etkin denetleyiciyi seri bağlantı noktasına bağlanın.
4. (Cihazı ile birlikte gelen) seri USB sürücüleri bilgisayarınıza yükleyin.
5. Seri bağlantı aşağıdaki gibi ayarlayın:
   
   * 115\.200 baud
   * 8 veri bitleri
   * 1 dur bit
   * Eşlik yok
   * Akış denetimi kümesine **yok**
6. Bağlantı Konsolu'nda Enter tuşuna basarak çalıştığını doğrulayın. Seri konsol menüsünde görüntülenmelidir.

> [!NOTE]
> **Uzaktan Yönetimi:** Cihaz uzak bir veri merkezinde veya sınırlı erişimli bilgisayar odasında yüklendiğinde, her iki denetleyicilerinin seri bağlantıları her zaman bir seri konsol anahtarı veya benzer ekipman bağlandığınızdan emin olun. Bu, bant dışı uzaktan denetim ve Destek işlemleri ağ kesintisi veya beklenmeyen arıza durumunda sağlar.
> 
> 

Cihazınızın güç, ağ erişimi ve seri bağlantı kabloları tamamladınız. Sonraki adım, Cihazınızda yazılım yapılandırmaktır.

## <a name="next-steps"></a>Sonraki adımlar
Artık hazırsınız [şirket içi StorSimple Cihazınızı yapılandırmak ve dağıtmak](storsimple-8000-deployment-walkthrough-u2.md).

