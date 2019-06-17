---
title: StorSimple yük devretme, 8000 serisi cihazlar için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple Cihazınızı kendisi, başka bir fiziksel cihaz veya Bulut Gereci için yük devretme öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 079a2f153f257040d1899a33c9e255d633e526ad
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60576381"
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı için yük devretme ve olağanüstü durum kurtarma

## <a name="overview"></a>Genel Bakış

Bu makalede, cihaz yük devretme özelliği StorSimple 8000 serisi cihazlar ve bir olağanüstü durum oluşursa, StorSimple cihazları kurtarmak için bu özelliği'nın nasıl kullanılabileceğini açıklar. StorSimple, veri merkezindeki bir kaynak CİHAZDAN başka bir hedef cihaza geçirmek için cihaz yük devretme kullanır. Bu makaledeki yönergeler, StorSimple 8000 serisi fiziksel cihazlar ve yazılım sürümlerini güncelleştirme 3 ve üzerini çalıştıran cloud appliance'lar için geçerlidir.

StorSimple kullanan **cihazları** olağanüstü bir cihaza yük devretme özelliğini başlatmak için dikey pencere. Bu dikey pencere, StorSimple cihaz Yöneticisi hizmetinize bağlanan tüm StorSimple cihazları listeler.

![Cihazlar dikey penceresi](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Olağanüstü Durum Kurtarma (DR) ve cihaz yük devretme

Bir olağanüstü durum kurtarma (DR) senaryosunda, kullanıcının birincil aygıtı çalışmayı durdurur. StorSimple, birincil cihaz olarak kullandığı _kaynak_ ve ilişkili bir bulut veri diğerine taşır _hedef_ cihaz. Bu işlem olarak adlandırılır *yük devretme*. Aşağıdaki grafikte, yük devretme işlemi gösterilmektedir.

![Cihaz yük devretme kümesinde ne olacak?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Bir yük devretme için hedef cihazla, fiziksel bir cihaz veya Bulut Gereci olabilir. Hedef cihaz, aynı veya farklı bir coğrafi konumda kaynak cihaz bulunabilir.

Yük devretme sırasında geçiş için birim kapsayıcıları seçebilirsiniz. StorSimple, hedef cihaza kaynak CİHAZDAN bu birim kapsayıcıları sahipliği sonra değişir. Birim kapsayıcıları sahipliği değiştirdikten sonra StorSimple kaynak CİHAZDAN Bu kapsayıcıları siler. Silme işlemi tamamlandıktan sonra hedef cihaza geri dönebilirsiniz. _Yeniden çalışma_ özgün kaynak cihaz sahipliği aktarır.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cihaz yük devretme sırasında kullanılan bulut anlık görüntüsü

Bir DR en son bulut yedekleme verileri hedef cihaza geri yüklemek için kullanılır. Bulut anlık görüntüleri hakkında daha fazla bilgi için bkz. [StorSimple cihaz Yöneticisi hizmetini el ile yedekleme kullanmak](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Bir StorSimple 8000 serisi, yedekleme ilkeleri yedekleme işlemleriyle ilişkilidir. Aynı birim için birden çok yedekleme ilkesi varsa, StorSimple yedekleme ilkesiyle birimlerin en büyük sayı seçer. StorSimple, seçilen yedekleme ilkesi en son yedeklemeden sonra hedef cihazdaki verileri geri yüklemek için kullanır.

İki yedekleme ilkeleri vardır varsayalım *defaultPol* ve *customPol*:

* *defaultPol*: Tek bir birim *vol1*, 10:30 PM günlük başlangıç çalıştırır.
* *customPol*: Dört birim *vol1*, *vol2*, *vol3*, *vol4*, günlük 10: 00'dan başlayarak çalıştırır.

Bu durumda, StorSimple için kilitlenme tutarlılığı önceliklendirir ve kullandığı *customPol* , daha fazla birim içerdiğinden. Bu ilke en son yedekleme verilerini geri yüklemek için kullanılır. Yedekleme ilkeleri oluşturma ve yönetme konusunda daha fazla bilgi için Git [yedekleme ilkelerini yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Cihaz yük devretme için ortak düşünceler

Bir cihaz devretmeden önce aşağıdaki bilgileri gözden geçirin:

* Cihaz yük devretme başlatılmadan önce birim kapsayıcıları içinde tüm birimleri çevrimdışı olması gerekir. Planlanmamış bir yük devretmede StotSimple birimleri otomatik olarak çevrimdışı geçer. Ancak, (DR test etmek için) planlanmış bir yük devretme gerçekleştiriliyorsa, tüm birimleri çevrimdışı duruma getirmeniz gerekir.
* İlişkili olan birim kapsayıcılarını DR için bulut anlık görüntüsü listelenir. En az bir birim kapsayıcısı ile verileri kurtarmak için bir ilişkili bulut anlık görüntüsü olması gerekir.
* StorSimple arasında birden çok birim kapsayıcıları kapsayan bulut anlık görüntüleri varsa, bu birim kapsayıcılarının yükü bir küme olarak başarısız olur. Nadir bir örneği arasında birden çok birim kapsayıcıları span yerel anlık görüntü, ancak ilişkili bir bulut anlık görüntüleri kullanmayın, StorSimple yerel anlık görüntüleri başarısız ve yerel veri DR sonra kaybolur.
* Kullanılabilir hedef cihazlar DR için seçilen birim kapsayıcıları yerleştirmek için yeterli alanı olan cihazlardır. Yeterli alana sahip olmayan herhangi bir cihaza, hedef cihazları olarak listelenmez.
* Aygıt, buluttan veri erişimi ve yerel olarak depolamak gerektiğinde sonra DR (için sınırlı bir süre), veri erişim performansını önemli ölçüde etkilenebilir.

#### <a name="device-failover-across-software-versions"></a>Yazılım sürümleri arasında cihaz yük devretme

Bir StorSimple cihaz Yöneticisi hizmeti bir dağıtımda olabilir birden çok cihaz, fiziksel ve bulut, tüm çalışan farklı yazılım sürümleri.

Yük devretme veya geri farklı yazılım sürümü ve birim türlerinden DR sırasında nasıl davranacağını çalıştıran başka bir cihazda başarısız olursa belirlemek için aşağıdaki tabloyu kullanın.

#### <a name="failover-and-failback-across-software-versions"></a>Yük devretme ve yeniden çalışma yazılım sürümleri arasında

| Yük devretme / geri başarısız | Fiziksel cihaz | Bulut gereci |
| --- | --- | --- |
| Güncelleştirme 3, 4 güncelleştirmek için |Katmanlı birim katmanlı olarak tekrar başarısız. <br></br>Yerel olarak sabitlenmiş üzerinde yerel olarak sabitlenmiş birimler başarısız. <br></br> Güncelleştirme 4 cihazda bir anlık görüntüsünü alırken bir yük devretme aşağıdaki [ısı Haritası tabanlı izleme](storsimple-update4-release-notes.md#whats-new-in-update-4) devreye girer. |Yerel olarak katmanlı birimler yük devretme sabitlendi. |
| Güncelleştirme 3'ü güncelleştirme 4 |Katmanlı birim katmanlı olarak tekrar başarısız. <br></br>Yerel olarak sabitlenmiş üzerinde yerel olarak sabitlenmiş birimler başarısız. <br></br> Yedeklemeleri geri yüklemek için kullanılan ısı Haritası meta verileri korur. <br></br>Isı Haritası tabanlı izleme, güncelleştirme bir geri dönme aşağıdaki 3'te kullanılamıyor. |Yerel olarak katmanlı birimler yük devretme sabitlendi. |


## <a name="device-failover-scenarios"></a>Cihaz yük devretme senaryosu

Olağanüstü bir durum varsa, StorSimple Cihazınızda yük devretme gerçekleştirme tercih edebilirsiniz:

* [Fiziksel bir cihaz için](storsimple-8000-device-failover-physical-device.md).
* [Kendisine](storsimple-8000-device-failover-same-device.md).
* [Bulut Gereci için](storsimple-8000-device-failover-cloud-appliance.md).

Önceki makaleler her yukarıdaki yük devretme durumları için ayrıntılı adımlar sağlar.


## <a name="failback"></a>Yeniden çalışma

Güncelleştirme 3 ve sonraki sürümler için StorSimple, yeniden çalışma da destekler. Yeniden çalışma yalnızca yük devretme tersidir, hedef kaynağı haline gelir ve özgün kaynak cihaz artık yük devretme sırasında hedef cihazı haline gelir. 

Yeniden çalışma sırasında StorSimple verileri geri birincil konumu durur g/ç ve uygulama etkinlik yeniden eşitler ve geçişleri özgün konuma geri.

StorSimple, bir yük devretme işlemi tamamlandıktan sonra aşağıdaki eylemleri gerçekleştirir:

* StorSimple, kaynak CİHAZDAN yük devretme birim kapsayıcıları siler.
* StorSimple birim kapsayıcısı (üzerinden kaynağı cihazda başarısız) başına bir arka plan işi başlatır. Geri işi devam ederken başarısız çalışırsanız, belirten bir bildirim alırsınız. Yeniden başlatmak için iş tamamlanana kadar bekleyin.
* Birim kapsayıcıları silme işlemini tamamlamak için harcanan süre, veri miktarı, işlem için verileri yedekleme sayısı ve kullanılabilir ağ bant genişliği yaşını gibi çeşitli faktörlere bağlıdır.

Yük devretme testleri planlama veya test mantığın, daha az veri (GB) içeren birim kapsayıcılarını test etmenizi öneririz. Genellikle, 24 saat yük devretme işlemi tamamlandıktan sonra yeniden başlatabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

S. **DR başarısız veya kısmen başarılı olursa ne olur?**

A. DR başarısız olursa yeniden denemenizi öneririz. İkinci bir cihaz yük devretme işi ilk işin ilerleme durumunu farkındadır ve sonraki sürümlerde bu noktadan itibaren başlar.

S. **Bir cihaz, cihaz yük devretme işlemi devam ederken silebilir miyim?**

A. Bir kurtarma işlemi devam ederken bir cihazı silemezsiniz. DR tamamlandıktan sonra yalnızca Cihazınızı silebilirsiniz. Cihaz yük devretme işi ilerleme durumunu izleyebilirsiniz **işleri** dikey penceresi.

S. **Ne zaman kaynak cihazdaki yerel veriler silinir, böylece çöp toplama kaynağı cihazda başlar?**

A. Çöp toplama, yalnızca cihazın tamamen temizlenmesi sonra kaynağı cihazda etkinleştirilir. Temizleme üzerinde kaynak cihazdaki birim, yedekleme nesneleri (verileri değil), birim kapsayıcıları ve ilkeleri gibi başarısız olan nesnelerin temizleme içerir.

S. **Kaynak cihazdaki birim kapsayıcıları ile ilişkili silme işlemi başarısız olursa ne olur?**

A.  Silme işlemi başarısız olursa, birim kapsayıcıları el ile silebilirsiniz. İçinde **cihazları** dikey penceresinde, kaynak cihaz seçin ve tıklayın **birim kapsayıcıları**. Dikey pencerenin en altındaki ve üzerinden başarısız bir birim kapsayıcıları seçin, **Sil**. Tüm sildikten sonra başarısız kaynak cihazdaki birim kapsayıcıları yeniden başlatabilirsiniz. Daha fazla bilgi için Git [birim kapsayıcısını silme](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği, olağanüstü durum kurtarma (BCDR)

İş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu, veri merkezinin tamamı Azure çalışmıyor oluşur. Bu senaryo, StorSimple cihaz Yöneticisi hizmetiniz ve ilişkili StorSimple cihazları etkileyebilir.

Bir StorSimple cihazı yalnızca bir olağanüstü durum oluşmadan önce kaydedildiyse, bu cihaz, Fabrika sıfırlaması yapmak gerekebilir. Olağanüstü durum sonrası StorSimple cihazı çevrimdışı olarak Azure portalda görüntülenir. Bu cihaz, portaldan silinmesi gerekir. Cihazı fabrika ayarlarına sıfırlama ve hizmetle birlikte yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz yük devretme gerçekleştirmek hazır olduğunuzda, ayrıntılı yönergeler için aşağıdaki senaryolardan birini seçin:

- [Başka bir fiziksel cihaza yük devretme](storsimple-8000-device-failover-physical-device.md)
- [Aynı cihaza yük devretme](storsimple-8000-device-failover-same-device.md)
- [StorSimple bulut Gerecine yük devretme](storsimple-8000-device-failover-cloud-appliance.md)

Cihazınızda yük başarısız olursa, aşağıdaki seçeneklerden birini seçin:

* [StorSimple Cihazınızı silme veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

