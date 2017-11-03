---
title: "Microsoft Azure StorSimple 8100 aygıt yükleme | Microsoft Docs"
description: "Kutusundan çıkarma, rafa monte etme ve dağıtma ve yazılım yapılandırmadan önce StorSimple 8100 cihazınız kablo açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 102dffcd73f3d3b9362d7b2853faa060e9c645dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Kutusundan çıkarma, rafa monte ve StorSimple 8100 cihazınızın kablolarını bağlama
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple 8100 tek kasası, rafa monte edilen aygıt olur. Bu öğretici, açmak açıklanmaktadır rafa monte ve yapılandırmak ve StorSimple Cihazınızı dağıtma önce kablo StorSimple 8100 aygıt donanım.

## <a name="unpack-your-storsimple-8100-device"></a>StorSimple 8100 model Cihazınızı kutusundan çıkarma
Aşağıdaki adımlar StorSimple 8100 depolama Cihazınızı paketinden çıkarma hakkında NET, ayrıntılı yönergeler sağlar. Bu cihazı tek bir kutusunda geliyordu.

### <a name="prepare-to-unpack-your-device"></a>Cihazınızı paketinden çıkarma hazırlanma
Cihazınızı paketinden çıkarma önce aşağıdaki bilgileri gözden geçirin.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

1. İki kişinin el ile işleme muhafaza ağırlığını yönetmek kullanılabilir olduğundan emin olun. Tam olarak yapılandırılmış bir muhafaza 32 kg (70 lb.) tartmanız.
2. Kutuya bir düz, düzey yüzeyine koyun.

Ardından, Cihazınızı paketinden çıkarma için aşağıdaki adımları tamamlayın.

#### <a name="to-unpack-your-device"></a>Cihazınızı paketinden çıkarma için
1. Kutunun ve paketleme köpük zemine crushes, keser, su hasar ya da belirgin herhangi bir zarar için inceleyin. Kutusu veya paketleme ciddi bir şekilde bozuksa, kutunun açmayın. Lütfen [Microsoft Support başvurun](storsimple-contact-microsoft-support.md) aygıt iyi çalışma sırayla olup olmadığını değerlendirin yardımcı olacak.
2. Kutunun ayıklayın. Aşağıdaki resimde, StorSimple Cihazınızı paketten görünümünü gösterir.
   
     ![Depolama Cihazınızı paketinden çıkarma](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Depolama Cihazınızı paketten görünümü**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Paketleme kutusu |
   |   2 |Alt köpük zemine |
   |   3 |Cihaz |
   |   4 |Üst köpük zemine |
   |   5 |Aksesuar kutusu |
3. Kutunun paketi açılırken sonra sahip olduğunuzdan emin olun:
   
   * 1 tek muhafaza cihaz
   * 2 güç kablosu
   * 1 çapraz Ethernet kablosu
   * 2 seri konsol kabloları
   * seri erişim için 1 seri USB dönüştürücü
   * 1 yetkisiz değiştirmeye karşı kanıt T10 tornavida
   * 4 QSFP-için-SFP + bağdaştırıcılar 10 GbE ağ arabirimleri ile kullanmak için
   * 1 raf-Seti (donanım bağlama ile 2 yan rayları) bağlama
   * Alma başlatıldı belgeleri
     
     Yukarıda listelenen öğelerden herhangi birini almadı varsa [Microsoft Support başvurun](storsimple-contact-microsoft-support.md).

Sonraki adım rafa monte Cihazınızı olacak.

## <a name="rack-mount-your-storsimple-8100-device"></a>Rafa monte StorSimple 8100 cihazınız
Bir standart 19 inç dolapta ön ve arka postalar ile StorSimple 8100 depolama aygıtı yüklemek için sonraki adımları izleyin. StorSimple 8100 cihazınız tek bir birincil muhafazada sahiptir.

Birden çok adımı, her biri aşağıdaki yordamlarda açıklanan yükleme oluşur.

> [!IMPORTANT]
> StorSimple cihazları düzgün çalışması için rafa monte edilen olması gerekir.
> 
> 

### <a name="prepare-the-site"></a>Site hazırlama
Cihaz ön ve arka gönderileri sahip standart bir 19 inç rafa yüklü olması gerekir. Raf yüklemesine hazırlanmak için aşağıdaki yordamı kullanın.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Site raf yüklemesine hazırlanmak için
1. Cihaz bir düz, kararlı ve düzey çalışmaları yüzey (veya benzeri) güvenli bir şekilde ye dayanıyorsa emin olun.
2. Burada, ayarlamak istediğiniz site bağımsız bir kaynak veya bir raf güç dağıtım birimi (PDU) bir kesintisiz güç kaynağı (UPS) ile standart AC gücünden sahip olduğunu doğrulayın.
3. Bu bir 2U yuvası cihaz bağlamak istediğiniz rafa monte kullanılabilir olduğundan emin olun.

![Uyarı simgesi](./media/storsimple-safety/IC740879.png)![büyük ağırlık simgesi](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **uyarı!**

İki kişinin aygıt ayarları el ile işleniyorsa ağırlık yönetmek kullanılabilir olduğundan emin olun. Tam olarak yapılandırılmış bir muhafaza 32 kg (70 lb.) tartmanız.

### <a name="rack-prerequisites"></a>Raf önkoşulları
8100 kasası ile dolap bir standart 19 inç dolapta yüklemesi için tasarlanmıştır:

* Raf post için post 27.84 inç minimum derinliği.
* Cihaz için 32 kg maksimum ağırlığı
* 5 Pascal (0,5 mm su ölçer) en fazla arka baskısı.

### <a name="rack-mounting-rail-kit"></a>Raf bağlama raylar Seti
Rayları takma kümesi 19 inç raf dolap ile kullanım için sağlanır. Rayları maksimum muhafaza ağırlığı işlemek üzere test edilmiştir. Bu rayları raftaki alanının herhangi bir kayıp olmadan birden fazla kasaları yüklemesini de izin verir.

#### <a name="to-install-the-device-on-the-rails"></a>Cihaz üzerinde rayları yüklemek için
1. Yalnızca iç rayları aygıtınızda yüklü değilse bu adımı gerçekleştirin. Genellikle, iç rayları fabrikada yüklenir. Rayları yüklü değilse raylar sol ve sağ raylar slayt muhafaza kasa yanlarından yükleyin. Bunlar, her bir tarafta altı ölçüm Vida kullanarak ekleyin. Raylar yönü yardımcı olmak için işaretlenen **LH – ön** ve **RH – ön**, ve muhafaza arka yapıştırılmış son Konik bitiş vardır.<br/>
   
    ![Sürgülü raylar ekleme](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Muhafaza yanlarından iç raylar ekleme**
   
    Etiket | Açıklama
    ----- | -----------
    1     | M 3 x 4 düğmesi head Vida
    2     | Kasa slayt

2. Dış sol raylar ve dış sağda derlemeleri raf dolap dikey üyeleri ekleyin. Köşeli ayraçlar işaretlenmiş **LH**, **RH**, ve **bu tarafı yukarı** doğru yönlendirme size yol gösterecek.
3. Ön ve arka raylar derlemenin raylar PIN'ler bulun. Raf gönderimler arasında sığacak ve ön ve arka raf post dikey üye delik halinde PIN'ler eklemek için raylar genişletir. Raylar derleme düzeyi olduğundan emin olun.
4. İki sağlanan ölçüm Vida raf raylar derlemeye dikey üyeleri güvenliğini sağlamak için kullanın. Ön ve arka birinde bir Vidayı kullanın.
5. Diğer raylar derleme için bu adımları yineleyin.<br/>
   
     ![Raflı dolaba Sürgülü raylar ekleme](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Dış raylar derlemeleri dolaba ekleme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Vidayı clamping |
   |   2 |Kare delik ön raf post Vidayı |
   |   3 |Sol raylar ön konumu PIN'ler |
   |   4 |Vidayı clamping |
   |   5 |Sol raylar arka konumu PIN'ler |

### <a name="mounting-the-device-in-the-rack"></a>Cihazı rafa monte
Yeni yüklenen raf rayları kullanarak, raf aygıtı bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-the-device"></a>Cihaz bağlamak için
1. Bir Yardımcısı ile muhafaza kaldırın ve raf rayları ile Hizala.
2. Dikkatle cihaz rayları yerleştirin ve sonra cihaz tamamen rafa dolap itme.<br/>
   
    ![Cihazı rafa yerleştirme](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Cihazı rafa monte**
3. Sol ve sağ ön Flanş başlıklarını ücretsiz caps çekerek kaldırın. Flanş başlıklarını yalnızca çıkıntıları ek.
4. Raf muhafazada sol ve sağ her Flanş aracılığıyla sağlanan bir yıldız head Vidayı yükleyerek güvenli hale getirin.
5. Konumda basarak ve yerinde tutturma Flanş başlıklarını yükleyin.<br/>
   
     ![Flanş başlıklarını takma](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Flanş başlıklarını takma**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   1 |Muhafaza birleşme Vidayı |

Sonraki adım, güç, ağ ve seri erişim için Cihazınızı kablo olacak.

## <a name="cable-your-storsimple-8100-device"></a>StorSimple 8100 cihazınızın kablolarını bağlama
Aşağıdaki yordamlar, StorSimple 8100 cihazınız güç, ağ ve seri bağlantılar için kablo açıklanmaktadır.

### <a name="prerequisites"></a>Ön koşullar
Cihazınızı kablolama başlamadan önce ihtiyacınız:

* Depolama aygıtı, tamamen açılmış ve raf bağladı.
* cihazınızla birlikte gelen 2 güç kabloları
* 2 güç dağıtım birimleri (önerilen) erişim.
* Ağ kabloları
* Seri kablolar sağlanan
* Seri USB Dönüştürücü uygun sürücüsü (gerekirse) Bilgisayarınızda yüklü
* 4 QSFP sağlanan-için-SFP + bağdaştırıcılar 10 GbE ağ arabirimleri ile kullanmak için
* [StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Güç kabloları
Cihazınızı yedek güç ve soğutma modüllerini (PCMs) içerir. Hem PCMs yüklü ve yüksek kullanılabilirlik sağlamak için farklı güç kaynaklarına bağlandınız.

Cihazınızı güç kablosu için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Ağ kabloları
Cihazınızı bir etkin bekleme yapılandırmadır: belirli bir zamanda bir denetleyici modülü etkin ve bir Denetleyici Modülü sırasında tüm disk ve ağ işlemlerini işleme bekleme. Bir denetleyici başarısız olursa, bekleme denetleyicisi hemen etkinleştirilir ve tüm disk ve ağ işlemleri devam eder.

Bu yedek denetleyici yük devretmesi desteklemek için aşağıdaki adımlarda açıklandığı gibi cihaz ağınız kablo gerekir.

#### <a name="to-cable-for-network-connection"></a>Ağ bağlantısı için kablo için
1. Cihazınızı her denetleyicisinde altı ağ arabirimi bulunur: dört 1 GB/sn, ve iki 10 GB/sn Ethernet bağlantı noktaları. Cihazınızı devre kartı çeşitli veri noktalarına tanımlayın.
   
    ![8100 aygıt devre kartı](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Aygıt geri veri bağlantı noktalarını gösterme**
   
   | Etiket | Açıklama |
   | --- | --- |
   |   0,1,4,5 |1 GbE ağ arabirimleri |
   |   2,3 |10 GbE ağ arabirimleri |
   |   6 |Seri bağlantı noktaları |
2. Ağ kablosunun için aşağıdaki diyagramda bakın. (En düşük ağ yapılandırması düz mavi çizgilerle gösterilir. Yüksek kullanılabilirlik ve performans için gereken ek yapılandırma noktalı çizgilerle gösterilir.)

    ![Kabloyla 2U cihazınızın ağ bağlantısını yapma](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Cihazınız için kablo ağ**

   |Etiket | Açıklama |
   |----- | ----------- |
   | A    | Internet erişimi olan LAN |
   | B    | Denetleyici 0 |
   | C    | PCM 0 |
   | D    | Denetleyici 1 |
   | E    | PCM 1 |
   | F, G | Ana bilgisayarlar |
   | 0-5  | Ağ arabirimleri |



Cihaz kablo, en düşük yapılandırmayı gerektirir:

* En az iki ağ arabirimi her denetleyicisi bulut erişimi için diğeri için iSCSI ile bağlı. VERİ bağlantı noktası otomatik olarak etkinleştirilmiş ve cihaz seri Konsolu yapılandırılmış 0. Veri 0 dışında başka bir veri bağlantı noktası da Klasik Azure portalı yapılandırılması gerekir. Bu durumda, veri 0 bağlanmak birincil yerel ağa (Internet erişimi olan ağ) bağlantı noktası. Diğer veri bağlantı noktalarına SAN/iSCSI LAN (VLAN) segment hedeflenen rolü bağlı olarak ağ bağlanabilir.
* Her denetleyicisinde aynı arabirimleri bir denetleyici yük devretme gerçekleşirse kullanılabilirliğini sağlamak için aynı ağa bağlı. Örneğin, veri 0 ve veri 3 denetleyicileri birine bağlanmayı seçerseniz, ilgili veri 0 ve veri 3 diğer denetleyicisinde bağlanmanız gerekir.

Yüksek kullanılabilirlik ve performans için göz önünde bulundurun:

* Mümkün olduğunda, bir çift ağ arabiriminin bulut erişim (1 GbE) ve iSCSI (10 GbE önerilen) için başka bir çifti her denetleyicisinde yapılandırın.
* Mümkün olduğunda, ağ arabirimlerinin her denetleyicisinden anahtar arızasına karşı kullanılabilirliğini sağlamak için iki farklı anahtarlara bağlayın. Şekilde gösterilmektedir iki 10 GbE ağ arabirimleri, veri 2 ve veri 3, iki farklı anahtarlara bağlı her denetleyicisinden.

Daha fazla bilgi için bkz **ağ arabirimleri** altında [StorSimple cihazınız için yüksek kullanılabilirlik gereksinimlerini](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> SFP + vericilerinin, 10 GbE ağ arabirimleri ile kullanıyorsanız, sağlanan QSFP kullanın-SFP + bağdaştırıcıları. Daha fazla bilgi için Git [, StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Seri bağlantı kabloları
Seri bağlantı noktanızın kablo için aşağıdaki adımları gerçekleştirin.

#### <a name="to-cable-for-serial-connection"></a>Seri bağlantı için kablo
1. Cihazınızı bir İngiliz anahtarı simgesi tarafından tanımlanan her denetleyicisinde seri bağlantı noktası var. Çizimde başvurmak [ağ kablo](#network-cabling) Cihazınızı devre kartı seri bağlantı noktalarına bulmak için bölüm.
2. Aygıt devre kartı etkin denetleyicisinde tanımlayın. Yanıp sönen bir mavi LED denetleyicisi etkin olduğunu gösterir.
3. (Gerekirse, dizüstü bilgisayarınız için USB seri dönüştürücü) sağlanan seri kablolar ve konsol veya bilgisayarla (terminal öykünme aygıta) etkin denetleyicisi seri bağlantı noktasına bağlayın.
4. (Aygıtla birlikte gelen) seri USB sürücüleri bilgisayarınıza yükleyin.
5. Seri bağlantı aşağıdaki gibi ayarlayın: 115.200 baud, 8 veri bitleri, 1 dur biti, eşlik yok ve akış denetimi için ayarlanmamış.
6. Bağlantı konsolda Enter tuşuna basarak çalışmakta olduğunu doğrulayın. Seri konsol menüsünde görüntülenmelidir.

> [!NOTE]
> **Uzaktan Yönetim**: cihaz bir uzak veri merkezinde veya sınırlı erişimi olan bir bilgisayar odada yüklendiğinde hem denetleyicileri seri bağlantılarda her zaman bir seri konsol anahtarı veya benzer ekipman bağlı olduğunuzdan emin olun. Ağ kesintilerine veya beklenmeyen arıza varsa bu bant dışı uzaktan denetim ve Destek işlemler sağlar.
> 
> 

Cihazınız artık güç, ağ erişimi ve seri bağlantı kablolu. Yazılım yapılandırmak ve Cihazınızı dağıtmak için sonraki adım olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [şirket içi StorSimple Cihazınızı yapılandırmak ve dağıtmak](storsimple-deployment-walkthrough-u2.md).

