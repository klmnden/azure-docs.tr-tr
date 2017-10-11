---
title: "Microsoft Azure StorSimple 8600 aygıt yükleme | Microsoft Docs"
description: "Kutusundan çıkarma, rafa monte etme ve dağıtma ve yazılım yapılandırmadan önce StorSimple 8600 model Cihazınızı kablo açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/24/2016
ms.author: alkohli
ms.openlocfilehash: 309ceba2d65c0745ba1acac698acb62526ab8078
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Kutusundan çıkarma, rafa monte ve StorSimple 8600 cihazınızın kablolarını bağlama
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8600 çift muhafaza cihaz ve birincil ve EBOD muhafazası oluşur. Bu öğretici paketten açıklanmaktadır, rafa monte ve kablo StorSimple 8600 aygıt donanım, önce StorSimple yazılım yapılandırın.

## <a name="unpack-your-storsimple-8600-device"></a>StorSimple 8600 model Cihazınızı paketinden çıkarma
Aşağıdaki adımlar StorSimple 8600 depolama Cihazınızı paketinden çıkarma nasıl NET, ayrıntılı yönergeler sağlar. Bu aygıtın iki kutularında, biri birincil muhafaza ve EBOD muhafazası için başka bir gönderilir. Bu iki kutu içinde tek bir kutu daha sonra yerleştirilir.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızı paketinden çıkarma hazırlanma
Cihazınızı paketinden çıkarma önce aşağıdaki bilgileri gözden geçirin.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

1. İki kişinin el ile işleme aygıt ağırlığını yönetmek kullanılabilir olduğundan emin olun. Tam olarak yapılandırılmış bir muhafaza 32 kg (70 lb.) tartmanız.
2. Kutuya bir düz, düzey yüzeyine koyun.

Ardından, Cihazınızı paketinden çıkarma için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızı paketinden çıkarma için
1. Kutunun ve paketleme köpük zemine crushes, keser, su hasar ya da belirgin herhangi bir zarar için inceleyin. Kutusu veya paketleme ciddi bir şekilde bozuksa, kutunun açmayın. Lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) aygıt iyi çalışma sırayla olup olmadığını değerlendirin yardımcı olacak.
2. Dış kutusunu açın ve ardından birincil ve EBOD kutularının karşılık gelen iki kutuları kaldırın. Artık birincil ve EBOD kutularının da ayıklayın. Aşağıdaki şekilde kasaları birini paketten görünümünü gösterir.
   
    ![Depolama Cihazınızı paketinden çıkarma](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Depolama Cihazınızı paketten görünümü**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Paketleme kutusu |
   |   2 |SAS kabloları (tepsisinde Donatılar ve kablolar) |
   |   3 |Alt köpük zemine |
   |   4 |Cihaz |
   |   5 |Üst köpük zemine |
   |   6 |Aksesuar kutusu |
3. İki kutuları paketi açılırken sonra sahip olduğunuzdan emin olun:
   
   * 1 birincil kasası (birincil muhafaza ve EBOD muhafazası iki ayrı kutularında olduğunda)
   * 1 EBOD muhafazası
   * 4 güç kablosu, her kutusunda 2
   * (Birincil muhafaza EBOD muhafazası bağlamak için) 2 SAS kabloları
   * 1 çapraz Ethernet kablosu
   * 2 seri konsol kabloları
   * seri erişim için 1 seri USB dönüştürücü
   * 4 QSFP-için-SFP + bağdaştırıcılar 10 GbE ağ arabirimleri ile kullanmak için
   * 2 bağlama setleri (donanım, her 2 EBOD Muhafazası ve birincil kasası için takma ile 4 yan rayları), raf her kutusunda 1
   * Başlatılan belgeleri alma
     
     Yukarıda listelenen öğelerden herhangi birini almadı varsa [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).  

Sonraki adım rafa monte Cihazınızı olacak.

## <a name="rack-mount-your-storsimple-8600-device"></a>Rafa monte StorSimple 8600 model Cihazınızı
Bir standart 19 inç dolapta ön ve arka postalar ile StorSimple 8600 depolama aygıtı yüklemek için sonraki adımları izleyin. Bu aygıtın iki kasa ile gelir: birincil muhafaza ve EBOD muhafazası. Bunların her ikisi de rafa monte edilen gerekir.

Birden çok adımı, her biri aşağıdaki yordamlarda açıklanan yükleme oluşur.

> [!IMPORTANT]
> StorSimple cihazları düzgün çalışması için rafa monte edilen olması gerekir.
> 
> 

### <a name="site-preparation"></a>Site hazırlama
Kutulardaki ön ve arka gönderileri sahip standart bir 19 inç rafa yüklü olması gerekir. Raf yüklemesine hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Site raf yüklemesine hazırlanmak için
1. Birincil ve EBOD kutularının işi olmayan bir düz, kararlı ve düzey çalışma yüzeyinde güvenle (veya benzeri) olduğundan emin olun.
2. Burada, ayarlamak istediğiniz site bağımsız bir kaynak veya bir kesintisiz güç kaynağı (UPS) ile bir rafa monte güç dağıtım birimi (PDU) standart AC gücünden sahip olduğunu doğrulayın.
3. Bu bir 4U (2 X 2U) yuvası kasaları bağlamak istediğiniz rafa monte kullanılabilir olduğundan emin olun.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

 İki kişinin aygıt ayarları el ile işleniyorsa ağırlık yönetmek kullanılabilir olduğundan emin olun. Tam olarak yapılandırılmış bir muhafaza 32 kg (70 lb.) tartmanız.

### <a name="rack-prerequisites"></a>Raf önkoşulları
Ek bir standart 19 inç dolapta ile dolap yüklemesi için tasarlanmıştır:

* Raf post için post 27.84 inç minimum derinliği
* Cihaz için 32 kg maksimum ağırlığı
* 5 Pascal (0,5 mm su ölçer), en fazla arka baskısı

### <a name="rack-mounting-rail-kit"></a>Raf bağlama raylar Seti
Rayları takma kümesi 19 inç raf dolap ile kullanım için sağlanır. Rayları maksimum muhafaza ağırlığı işlemek üzere test edilmiştir. Bu rayları raftaki alanı kaybı olmadan birden fazla kasaları yüklemesini de izin verir. EBOD muhafazası ilk yükleyin.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>EBOD muhafazası üzerinde rayları yüklemek için
1. Yalnızca iç rayları aygıtınızda yüklü değilse bu adımı gerçekleştirin. Genellikle, iç rayları fabrikada yüklenir. Rayları yüklü değilse raylar sol ve sağ raylar slayt muhafaza kasa yanlarından yükleyin. Bunlar, her bir tarafta altı ölçüm Vida kullanarak ekleyin. Raylar yönü yardımcı olmak için işaretlenen **LH – ön** ve **RH – ön**, ve muhafaza arka yapıştırılmış son Konik bitiş vardır.
   
    ![Sürgülü raylar ekleme](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Muhafaza yanlarından raylar ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |  1 |M 3 x 4 düğmesi head Vida |
   |  2 |Kasa slayt |
2. Sağda derlemeler ve sol raylar raf dolap dikey üyeleri ekleyin. Köşeli ayraçlar işaretlenmiş **LH**, **RH**, ve **bu tarafı yukarı** doğru yönlendirme size yol gösterecek.
3. Ön ve arka raylar derlemenin raylar PIN'ler bulun. Raf gönderimler arasında sığacak ve ön ve arka raf post dikey üye delik halinde PIN'ler eklemek için raylar genişletir. Raylar derleme düzeyi olduğundan emin olun.
4. Raf raylar derlemeye dikey üyeleri iki sağlanan ölçüm Vida kullanarak güvenli hale getirin. Ön ve arka birinde bir Vidayı kullanın.
5. Diğer raylar derleme için bu adımları yineleyin.
   
     ![Raflı dolaba Sürgülü raylar ekleme](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Dolaba Sürgülü raylar derlemeler ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Vidayı clamping |
   |   2 |Kare delik ön raf post Vidayı |
   |   3 |Ön raylar konumu PIN'ler sol |
   |   4 |Vidayı clamping |
   |   5 |Sol arka raylar konumu PIN'ler |

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>EBOD muhafazası dolapta bağlama
Yeni yüklenen raf rayları kullanarak dolapta EBOD muhafazası bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-ebod-enclosure"></a>EBOD muhafazası bağlamak için
1. Bir Yardımcısı ile muhafaza kaldırın ve raf rayları ile Hizala.
2. Dikkatle muhafaza rayları yerleştirin ve ardından tam bir rafa dolap kullanıcılarıma.
   
    ![Cihazı rafa yerleştirme](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Raf muhafazada bağlama**
3. Sol ve sağ ön Flanş başlıklarını ücretsiz caps çekerek kaldırın. Flanş başlıklarını yalnızca çıkıntıları ek.
4. Kasa, sol ve sağ her Flanş aracılığıyla sağlanan bir yıldız head Vidayı yükleyerek bir rafa güvenliğini sağlayın.
5. Konumda basarak ve yerine tutturma Flanş başlıklarını yükleyin.
   
     ![Flanş başlıklarını takma](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş başlıklarını takma**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Muhafaza birleşme Vidayı |

### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Raftaki birincil kasası oluşturma
EBOD muhafazası takma bitirdikten sonra aynı adımları izleyerek birincil muhafaza bağlamanız gerekir.

> [!NOTE]
> * Birincil muhafaza EBOD muhafazası arasındaki dolapta birkaç boş yuva olması mümkündür.
> * EBOD muhafazası birincil muhafaza bağlamak için sağlanan 2 m SAS kablosu kullanın.
> * EBOD birim kafası birimine göreli yerleşimini hiçbir kısıtlamalar vardır. Bu nedenle, birincil muhafaza üst yuvası ve aşağıdaki EBOD muhafazası yerleştirilir — veya tam tersi.
> 
> 

Sonraki adım, güç, ağ ve seri erişim için Cihazınızı kablo olacak.

## <a name="cable-your-storsimple-8600-device"></a>StorSimple 8600 cihazınızın kablolarını bağlama
Aşağıdaki yordamlar, StorSimple 8600 model Cihazınızı güç, ağ ve seri bağlantılar için kablo açıklanmaktadır.

### <a name="prerequisites"></a>Ön koşullar
Cihazınızın kablolarını bağlama başlamadan önce gerekir:

* Birincil muhafaza ve EBOD muhafazası tamamen açılmış
* cihazınızla birlikte gelen 4 güç kabloları (2 her birincil ve EBOD muhafazası için)
* EBOD muhafazası birincil muhafaza bağlanmak için aygıtla birlikte sağlanan 2 SAS kabloları
* 2 güç dağıtım birimleri (Pdu'lar) (önerilen) erişimi
* Ağ kabloları
* Seri kablolar sağlanan
* Seri USB Dönüştürücü uygun sürücüsü (gerekirse) Bilgisayarınızda yüklü
* 4 QSFP sağlanan-için-SFP + bağdaştırıcılar 10 GbE ağ arabirimleri ile kullanmak için
* [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS ve güç kabloları
Cihazınızı birincil muhafaza ve EBOD muhafazası sahiptir. Bu seri bağlı SCSI (SAS) bağlantısı ve güç için birlikte kablolu için birimler gerektirir.

Bu cihazı ilk kez ayarlarken, ilk SAS kabloları adımlarını gerçekleştirin ve güç kabloları adımlarını tamamlayın.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Ağ kabloları
Cihazınızı bir etkin bekleme yapılandırmasında olduğu: belirli bir zamanda bir denetleyici modülü etkin ve bir Denetleyici Modülü sırasında tüm disk ve ağ işlemlerini işleme bekleme. Bir denetleyici hatası oluşursa, bekleme denetleyicisi hemen etkinleştirir ve tüm disk ve ağ işlemleri devam eder.

Bu yedek denetleyici yük devretmesi desteklemek için aşağıdaki adımlarda gösterildiği gibi cihaz ağınız kablo gerekir.

#### <a name="to-cable-for-network-connection"></a>Ağ bağlantısı için kablo için
1. Cihazınızı her denetleyicisinde altı ağ arabirimi bulunur: dört 1 GB/sn ve iki 10 GB/sn Ethernet bağlantı noktası. Cihazınızı devre kartı veri noktalarına tanımlamak için aşağıdaki çizimde bakın.
   
     ![8600 aygıt devre kartı](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Geri Cihazınızı veri bağlantı noktalarını gösterme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Ağ kablosunun için aşağıdaki diyagramda bakın. (En düşük ağ yapılandırması düz mavi çizgilerle gösterilir. "Yüksek kullanılabilirlik ve performans için gereken ek yapılandırma noktalı çizgilerle gösterilir.)

![Kabloyla 4U cihazınızın ağ bağlantısını yapma](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Cihazınız için kablo ağ**

| Etiket | Açıklama |
| --- | --- |
| A |Internet erişimi olan LAN |
| B |Denetleyici 0 |
| C |PCM 0 |
| D |Denetleyici 1 |
| E |PCM 1 |
| F |EBOD denetleyici 0 |
| G |EBOD Denetleyici 1 |
| H T |Ana bilgisayar (örneğin, dosya sunucuları) |
| 0-5 |Ağ arabirimleri |
| 6 |Birincil kasası |
| 7 |EBOD muhafazası |

Cihaz kablo, en düşük yapılandırmayı gerektirir:

* En az iki ağ arabirimi her denetleyicisi bulut erişimi için diğeri için iSCSI ile bağlı. VERİ bağlantı noktası otomatik olarak etkinleştirilmiş ve cihaz seri Konsolu yapılandırılmış 0. Veri 0 dışında başka bir veri bağlantı noktası da Klasik Azure portalı yapılandırılması gerekir. Bu durumda, veri 0 bağlanmak birincil yerel ağa (Internet erişimi olan ağ) bağlantı noktası. Diğer veri bağlantı noktalarına SAN/iSCSI LAN (VLAN) segment hedeflenen rolü bağlı olarak ağ bağlanabilir.
* Her denetleyicisinde aynı arabirimleri bir denetleyici yük devretme gerçekleşirse kullanılabilirliğini sağlamak için aynı ağa bağlı. Örneğin, veri 0 ve veri 3 denetleyicileri birine bağlanmayı seçerseniz, ilgili veri 0 ve veri 3 diğer denetleyicisinde bağlanmanız gerekir.

Yüksek kullanılabilirlik ve performans için göz önünde bulundurun:

* Mümkün olduğunda, bir çift ağ arabiriminin bulut erişim (1 GbE) ve iSCSI (10 GbE önerilen) için başka bir çifti her denetleyicisinde yapılandırın.
* Mümkün olduğunda, ağ arabirimlerinin her denetleyicisinden anahtar arızasına karşı kullanılabilirliğini sağlamak için iki farklı anahtarlara bağlayın. Şekilde gösterilmektedir iki 10 GbE ağ arabirimleri, veri 2 ve veri 3, iki farklı anahtarlara bağlı her denetleyicisinden. Daha fazla bilgi için bkz **ağ arabirimleri** altında [StorSimple cihazınız için yüksek kullanılabilirlik gereksinimlerini](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> SFP + vericilerinin, 10 GbE ağ arabirimleri ile kullanıyorsanız, sağlanan QSFP kullanın-SFP + bağdaştırıcıları. Daha fazla bilgi için Git [, StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı kabloları
Seri bağlantı noktanızın kablo için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kablo
1. Cihazınızı bir İngiliz anahtarı simgesi tarafından tanımlanan her denetleyicisinde seri bağlantı noktası var. Seri bağlantı noktalarını bulmak için verileri Cihazınızı arkasında bağlantı noktalarını gösteren çizim bakın.
2. Aygıt devre kartı etkin denetleyicisinde tanımlayın. Yanıp sönen bir mavi LED denetleyicisi etkin olduğunu gösterir.
3. (Gerekirse, dizüstü bilgisayarınız için USB seri dönüştürücü) sağlanan seri kabloyu kullanın ve konsolu veya bilgisayarla (terminal öykünme aygıta) etkin denetleyicisi seri bağlantı noktasına bağlayın.
4. (Aygıtla birlikte gelen) seri USB sürücüleri bilgisayarınıza yükleyin.
5. Seri bağlantı aşağıdaki gibi ayarlayın:
   
   * 115.200 baud
   * 8 veri bitleri
   * 1 dur biti
   * Eşlik yok
   * Akış denetimi kümesine **yok**
6. Bağlantı konsolda Enter tuşuna basarak çalışmakta olduğunu doğrulayın. Seri konsol menüsünde görüntülenmelidir.

> [!NOTE]
> **Uzaktan Yönetimi:** aygıt bir uzak veri merkezinde veya sınırlı erişimi olan bir bilgisayar odada yüklendiğinde hem denetleyicileri seri bağlantılarda her zaman bir seri konsol anahtarı veya benzer ekipman bağlı olduğunuzdan emin olun. Bu, bant dışı uzaktan denetim ve ağ kesintisi veya beklenmeyen arıza durumunda destek işlemler sağlar.
> 
> 

Cihazınızı güç, ağ erişimi ve seri bağlantı kabloları tamamladınız. Sonraki adım, Cihazınızda yazılımı yapılandırmaktır.

## <a name="next-steps"></a>Sonraki adımlar
Artık hazırsınız [şirket içi StorSimple Cihazınızı yapılandırmak ve dağıtmak](storsimple-deployment-walkthrough-u2.md).

