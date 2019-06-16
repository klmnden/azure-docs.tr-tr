---
title: StorSimple sanal dizisi olağanüstü durum kurtarma ve cihaz yük devretme | Microsoft Docs
description: Yük devretme hakkında daha fazla StorSimple Virtual Array'iniz öğrenin.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: be3d98dc0b3a8119fb853493440c6fc78d65c5a2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61409634"
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Olağanüstü durum kurtarma ve cihaz yük devretme için Azure portal aracılığıyla, StorSimple sanal dizisi

## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple sanal ayrıntılı adımlar da dahil olmak üzere başka bir sanal diziye yük devretmek Diziniz için olağanüstü durum kurtarma açıklanır. Bir yük devretme verilerinizden taşımanıza izin veren bir *kaynak* cihaz için bir veri merkezinde bir *hedef* cihaz. Hedef cihaz, aynı veya farklı bir coğrafi konumda bulunabilir. Cihaz yük devretme için tüm cihazdır. Yük devretme sırasında kaynak cihazdaki bulut verilerini sahipliği, hedef cihazın değiştirir.

Bu makalede, yalnızca StorSimple sanal diziler için geçerlidir. Bir 8000 serisi cihaz başarısız kılmak için Git [cihaz yük devretme ve olağanüstü durum kurtarma StorSimple cihazınızın](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Olağanüstü durum kurtarma ve cihaz yük devretme nedir?

Bir olağanüstü durum kurtarma (DR) senaryosunda, kullanıcının birincil aygıtı çalışmayı durdurur. Bu senaryoda, başka bir cihaza başarısız cihazla ilgili bulut verilerinin taşıyabilirsiniz. Birincil cihaz olarak kullanabileceğiniz *kaynak* ve başka bir cihazı olarak belirtmek *hedef*. Bu işlem olarak adlandırılır *yük devretme*. Yük devretme sırasında tüm birimler veya paylaşımlar kaynak cihazdaki sahipliğini değiştirmek ve hedef cihaza aktarılır. Hiçbir veri filtreleme izin verilir.

DR harita tabanlı ısı katmanlama kullanma ve izleme cihazın tam geri yükleme modellenir. Isı Haritası ısı değeri verileri için atayarak tanımlanan okuma ve yazma modelleri. Bu ısı harita ardından katmanları en düşük ısı veri öbekleri buluta ilk (en çok kullanılan) yüksek ısı veri öbekleri için yerel katmanında tutarken. DR sırasında geri yüklemek ve buluttaki verileri yeniden doldurma ısı Haritası StorSimple kullanır. Cihaz tüm birimleri/paylaşımları son son yedeklemesine (dahili olarak belirlenen) getirir ve o yedekten geri yükleme gerçekleştirir. Sanal dizi DR sürecinin tamamını düzenler.

> [!IMPORTANT]
> Kaynak cihaz, cihaz yük devretme sonunda silinir ve bu nedenle bir yeniden çalışma desteklenmez.
> 
> 

Olağanüstü durum kurtarma cihaz yük devretme özelliği aracılığıyla düzenlenen ve başlatıldığı **cihazları** dikey penceresi. Bu dikey pencere, StorSimple cihaz Yöneticisi hizmetinize bağlı olan tüm StorSimple cihazları tablo haline getirir. Her cihaz için kolay ad, durum, sağlanan ve en yüksek kapasite, tür ve model görebilirsiniz.

## <a name="prerequisites-for-device-failover"></a>Cihaz yük devretme için Önkoşullar

### <a name="prerequisites"></a>Önkoşullar

Cihaz yük devretme için aşağıdaki önkoşulların karşılandığından emin olun:

* Kaynak cihaz olması gereken bir **devre dışı bırakılmış** durumu.
* Hedef cihaz olarak görünmesi gereken **kuruluma hazır** Azure portalında. Aynı veya daha yüksek kapasite bir hedef sanal dizin sağlayın. Yerel web kullanıcı Arabirimi, yapılandırmak ve hedef sanal dizi başarıyla kaydetmek için kullanın.
  
  > [!IMPORTANT]
  > Hizmeti aracılığıyla kaydedilmiş sanal cihaz yapılandırmak çalışmayın. Herhangi bir cihaz yapılandırma hizmet aracılığıyla gerçekleştirilmelidir.
  > 
  > 
* Hedef cihaz kaynak cihazla aynı ada sahip olamaz.
* Kaynak ve hedef cihaz sahip aynı türde olacak. Yalnızca başka bir dosya sunucusu için dosya sunucusu olarak yapılandırılmış bir sanal dizin devredebilir. Aynı bir iSCSI sunucusu için geçerlidir.
* Dosya sunucusu DR'sini, hedef cihaz kaynak ile aynı etki alanına katılma öneririz. Bu yapılandırma, paylaşım izinleri otomatik olarak çözümlenir sağlar. Yalnızca Yük devretme aynı etki alanında bir hedef cihaz.
* DR için kullanılabilir hedef kaynak cihaza karşılaştırıldığında aynı veya daha büyük kapasiteye sahip cihazlar cihazlardır. Hizmetinize bağlı, ancak yeterli alan ölçütleri karşılamayan cihazlar, hedef cihazları olarak kullanılamaz.

### <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

* Planlanmış bir yük devretme için 
  
  * Tüm birimler veya paylaşımlar çevrimdışı kaynağı cihazda almanızı öneririz.
  * Cihazın yedekleyin ve ardından veri kaybını en aza indirmek için yük devretme devam öneririz. 
* Cihaz, planlanmamış bir yük devretme için verileri geri yüklemek için en yeni yedeğinin kullanır.

### <a name="device-failover-prechecks"></a>Cihaz yük devretme ön denetimler

DR başlamadan önce cihazın ön denetimler gerçekleştirir. Bu denetimler, DR işlemi sırasında bir hata oluşmamasına emin olun yardımcı olur. Ön denetimler şunlardır:

* Depolama hesabı doğrulanıyor.
* Azure için bulut bağlantısı denetleniyor.
* Hedef cihazın kullanılabilir alanı denetleniyor.
* Bir iSCSI sunucusu kaynak cihaz birimi olup olmadığı denetleniyor
  
  * Geçerli ACR adı.
  * Geçerli bir IQN (220 karakter geçmeyen).
  * Geçerli CHAP parolaları (12-16 karakter).

Herhangi bir önceki prechecks, başarısız, DR ile devam edemiyor. Bu sorunların giderilmesine ve DR yeniden deneyin.

DR başarıyla tamamlandıktan sonra bulut veri kaynağı cihazın sahipliğini hedef cihaza aktarılır. Kaynak cihaz sonra artık portalda kullanılabilir. Tüm birimleri/paylaşımları kaynak cihaz erişimi engellenir ve hedef cihaza etkin hale gelir.

> [!IMPORTANT]
> Cihaz artık kullanılabilir olsa da, konak sisteminde sağlanan sanal makine kaynakları hala tüketiyor. DR başarıyla tamamlandıktan sonra bu sanal makinenin ana bilgisayar sisteminizden silebilirsiniz.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Bir sanal dizin için yük devretme

Sağlama, yapılandırma ve bu yordamı çalıştırmadan önce başka bir StorSimple Virtual Array, StorSimple cihaz Yöneticisi hizmeti ile kaydetme öneririz.

> [!IMPORTANT]
> 
> * Bir StorSimple 8000 serisi CİHAZDAN 1200 sanal cihaza yük olamaz.
> * Federal Bilgi İşleme Standardı (etkin FIPS) sanal bir CİHAZDAN başka bir FIPS etkin cihaz veya kamu Portalı'nda dağıtılan bir FIPS olmayan cihaz devredebilir.


Cihazı hedef bir StorSimple sanal cihazını geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Sağlama ve uygun bir hedef cihaz yapılandırma [cihaz yük devretme için Önkoşullar](#prerequisites). Yerel web UI aracılığıyla cihaz yapılandırmasını tamamlayın ve StorSimple cihaz Yöneticisi hizmetinize kaydedin. Bir dosya sunucusu oluşturuyorsanız 1 adım Git [dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). İSCSI sunucusu oluşturuyorsanız 1 adım Git [iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Konakta birimleri/paylaşımları çevrimdışı duruma alın. Birimleri/paylaşımları çevrimdışı duruma getirmek için konak işletim sistemine özgü yönergeleri bakın. Zaten çevrimdışı olursa, aşağıdakileri yaparak, cihazda tüm birimleri/paylaşımları çevrimdışı olması gerekir.
   
    1. Git **cihazları** dikey penceresinde ve Cihazınızı seçin.
   
    2. Gidin **Ayarları > Yönet > paylaşımları** (veya **Ayarları > Yönet > birimleri**). 
   
    3. Bir paylaşıma/birime seçin, sağ tıklatın ve seçin **çevrimdışına**. 
   
    4. Onayınız istendiğinde denetleyin **Bu paylaşımı çevrimdışı duruma getirmenin etkilerini anlıyorum.** 
   
    5. Tıklayın **çevrimdışına**.

3. StorSimple cihaz Yöneticisi hizmetinize gidin **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde, seçin ve kaynak cihazınız tıklayın.

4. İçinde **cihaz Panosu** dikey penceresinde tıklayın **devre dışı bırak**.

5. İçinde **devre dışı bırak** dikey penceresinde onaylamanız istenir. Cihaz devre dışı bırakma olan bir *kalıcı* işlem geri alınamaz. Konakta paylaşımları/birimlerinizi çevrimdışı yapılacak anımsatma yapılır. ' A tıklayın ve onaylamak için cihaz adı **devre dışı bırak**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. Devre dışı bırakmaya başlar. Devre dışı bırakma başarıyla tamamlandıktan sonra bir bildirim alırsınız.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Cihazlar sayfasında, cihaz durumu şimdi değişir **devre dışı bırakılmış**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. İçinde **cihazları** dikey penceresinde, seçin ve yük devretme için devre dışı bırakılmış kaynak cihaz'a tıklayın. 
9. İçinde **cihaz Panosu** dikey penceresinde tıklayın **yük devretme**. 
10. İçinde **cihaz başarısız** dikey penceresinde aşağıdakileri yapın:
    
    1. Kaynak cihaz alanı otomatik olarak doldurulur. Kaynak cihaz toplam veri boyutu unutmayın. Veri boyutu hedef cihazın kullanılabilir kapasitesi'den küçük olmalıdır. Cihaz adı, toplam kapasite ve yük devretme paylaşımları adları gibi kaynak cihaz ile ilgili ayrıntıları gözden geçirin.

    2. Kullanılabilir cihazları açılan listesinden seçim bir **hedef cihaz**. Yalnızca yeterli kapasiteye sahip cihazlar, açılan listede görüntülenir.

    3. Bu maddeyi **hedef cihaza veriler üzerinde bu işlemi başarısız olur anlıyorum**. 

    4. Tıklayın **yük devretme**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Yük devretme işi başlatır ve bir bildirim alırsınız. Git **cihazlar > işler** yük devretme izlemek için.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. İçinde **işleri** dikey penceresinde, kaynak cihaz için oluşturulan bir yük devretme işine bakın. Bu iş, DR ön denetimler gerçekleştirir.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     Yük devretme işi, DR ön denetimler başarılı olduktan sonra kaynak Cihazınızda var. her paylaşıma/birime geri yükleme işleri üretme.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Yük devretme işlemi tamamlandıktan sonra Git **cihazları** dikey penceresi.
    
    1. Seçin ve yük devretme işlemi için hedef cihazla kullanılan StorSimple cihaz seçeneğine tıklayın.
    2. Git **Ayarları > Yönetim > paylaşımları** (veya **birimleri** , iSCSI sunucusu). İçinde **paylaşımları** dikey penceresinde eski bir CİHAZDAN paylaşımları (birimler) görüntüleyebilirsiniz.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Şunları yapmanız gerekir [bir DNS diğer adı oluşturma](https://support.microsoft.com/kb/168322) böylece bağlanmaya çalıştığınız tüm uygulamalar için yeni cihaz yönlendirilen.

## <a name="errors-during-dr"></a>DR sırasında hatalar

**DR sırasında bulut bağlantı kesilmesi**

DR bulut bağlantısı DR başlatıldı ve cihaz önce geri yükleme tamamlandıktan sonra kesintiye uğrarsa, başarısız olur. Bir başarısızlık bildirimi alırsınız. DR için hedef cihazla olarak işaretlenmiş *kullanılamaz.* Aynı hedef cihaz için gelecekteki DRs kullanamazsınız.

**Uyumlu bir hedef cihaz yok**

Kullanılabilir hedef cihazlar yeterli alan yoksa, uyumlu bir hedef cihaz olduğunu etkili olması için bir hata görürsünüz.

**Precheck hataları**

Bir ön denetimler karşılanmıyor, precheck hataları konusuna bakın.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği, olağanüstü durum kurtarma (BCDR)

İş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu, veri merkezinin tamamı Azure çalışmıyor oluşur. Bu, StorSimple cihaz Yöneticisi hizmetiniz ve ilişkili StorSimple cihazları etkileyebilir.

Yalnızca bir olağanüstü durum oluşmadan önce kaydolan StorSimple cihazlar varsa, bu StorSimple cihazları silinmesi gerekebilir. Olağanüstü durum sonrası yeniden oluşturun ve aygıtları yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [StorSimple sanal yerel web UI aracılığıyla dizininiz yönetmek](storsimple-ova-web-ui-admin.md).

