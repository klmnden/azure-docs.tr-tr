---
title: StorSimple yük devretme, 8000 serisi cihazlar için olağanüstü durum kurtarma | Microsoft Docs
description: StorSimple Cihazınızı kendisi, başka bir fiziksel aygıt veya Bulut uygulaması için yük devri öğrenin.
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
ms.openlocfilehash: 5dc4a98bf889d38c62c76364289c2d58c14d771e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874986"
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınız için yük devretme ve olağanüstü durum kurtarma

## <a name="overview"></a>Genel Bakış

Bu makalede aygıt yük devretme özelliği StorSimple 8000 serisi cihazlar ve olağanüstü bir durum oluşursa, StorSimple cihazları kurtarmak için bu özelliği nasıl kullanılabileceğini açıklanmaktadır. StorSimple cihaz yük devretme başka bir hedef aygıta veri merkezinde bir kaynak aygıttan verileri geçirmek için kullanır. Bu makalede yer alan yönergeleri, StorSimple 8000 serisi fiziksel cihazlar ve yazılım sürümlerini güncelleştirme 3 ve sonraki sürümleri çalıştıran bulut uygulamaları için geçerlidir.

StorSimple kullanan **aygıtları** bir olağanüstü durum durumunda aygıt yük devretme özelliğini başlatmak için dikey. Bu dikey StorSimple cihaz Yöneticisi hizmetinize bağlı tüm StorSimple aygıtlarını listeler.

![Aygıtları dikey penceresi](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Olağanüstü Durum Kurtarma (DR) ve aygıtı yük devretme

Bir olağanüstü durum kurtarma (DR) senaryosunda birincil cihaz çalışmamaya başlar. StorSimple kullandığı birincil cihaz olarak _kaynak_ ve ilişkili bulut verileri diğerine taşır _hedef_ aygıt. Bu işlem olarak adlandırılır *yük devretme*. Aşağıdaki grafikte yük devretme işlemi gösterilmektedir.

![Cihaz yük devretme kümesinde ne olur?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

Bir hedef cihaz için bir yük devretme, bir fiziksel aygıt ya da bulut uygulaması olabilir. Hedef aygıt, aynı veya kaynak aygıtı farklı bir coğrafi konumda bulunabilir.

Yük devretme sırasında geçiş için birim kapsayıcıları seçebilirsiniz. StorSimple sonra bu birim kapsayıcıları sahipliği kaynak aygıttan ile hedef aygıta değiştirir. StorSimple birim kapsayıcıları sahipliği değiştirdiğinizde, bu kapsayıcılar kaynak aygıttan siler. Silme işlemi tamamlandıktan sonra hedef aygıt geri başarısız olabilir. _Yeniden çalışma_ özgün kaynak cihaz sahipliği aktarır.

### <a name="cloud-snapshot-used-during-device-failover"></a>Cihaz yük devretme sırasında kullanılan bulut anlık görüntüsü

Bir DR en son bulut yedekleme ile hedef aygıta verileri geri yüklemek için kullanılır. Bulut anlık görüntüleri hakkında daha fazla bilgi için bkz: [el ile yedek almak için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Bir StorSimple 8000 serisi yedekleme ilkeleri yedeklemelerini ile ilişkilendirilir. Aynı birim için birden çok yedekleme ilkeleri varsa, StorSimple en büyük birimlerin sayısı, yedekleme ilkesiyle seçer. StorSimple, seçili yedekleme ilkesi en son yedeklemeden sonra hedef aygıttaki verileri geri yüklemek için kullanır.

Vardır varsayalım iki yedekleme ilkeleri *defaultPol* ve *customPol*:

* *defaultPol*: tek bir birim *vol1*, günlük 10: 30'pm başlangıç çalıştırır.
* *customPol*: dört birim *vol1*, *vol2*, *vol3*, *vol4*, günlük 10: 00'dan başlayarak çalıştırır.

Bu durumda, StorSimple kilitlenme tutarlılığı için öncelik veren ve kullandığı *customPol* daha fazla birim sahibidir. Bu ilke en son yedekleme verileri geri yüklemek için kullanılır. Yedekleme ilkeleri oluşturun ve yönetin konusunda daha fazla bilgi için Git [yedekleme ilkelerini yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Cihaz yük devretme için ortak ilgili önemli noktalar

Bir aygıt üzerinden başarısız önce aşağıdaki bilgileri gözden geçirin:

* Bir aygıt yük devretmeyi başlatmadan önce birim kapsayıcıları tüm birimlerin çevrimdışı olması gerekir. Planlanmamış yük devretme kümesinde StotSimple birimleri otomatik olarak çevrimdışı. Ancak (DR sınamak için) planlanmış bir yük devretme gerçekleştiriyorsanız, tüm birimleri çevrimdışı duruma getirmeniz gerekir.
* Bir ilişkili olan birim kapsayıcıları bulut anlık görüntüsü, DR için listelenir. En az bir birim kapsayıcısı ile verileri kurtarmak için bir ilişkili bulut anlık görüntüsü olması gerekir.
* StorSimple arasında birden çok birim kapsayıcıları span bulut anlık görüntüleri varsa, bu birim kapsayıcıları bir küme olarak başarısız olur. Nadir bir örneği arasında birden çok birim kapsayıcıları span yerel anlık görüntüleri vardır, ancak ilişkili bulut anlık görüntüleri yapın, StorSimple yerel anlık görüntüleri başarısız ve yerel veri DR sonra kaybolur.
* Seçili birim kapsayıcıları yerleştirmek için yeterli alanı olan aygıtları DR için kullanılabilir hedef aygıtlardır. Yeterli alana sahip değilse, hedef cihaz olarak listelenmeyen tüm cihazlar.
* Sonra DR (için sınırlı bir süre) veri erişim performansını önemli ölçüde, cihaz buluttan veri erişimi ve yerel olarak depolamak gerektiği etkilenebilir.

#### <a name="device-failover-across-software-versions"></a>Cihaz yük devretme yazılım sürümleri arasında

StorSimple cihaz Yöneticisi hizmeti bir dağıtımda olabilir birden çok aygıt, fiziksel ve bulut, tüm çalışan farklı yazılım sürümleri.

Yük devri veya geri farklı yazılım sürümü ve birim türleri DR sırasında nasıl davranacağını çalıştıran başka bir cihazda başarısız olursa belirlemek için aşağıdaki tabloyu kullanın.

#### <a name="failover-and-failback-across-software-versions"></a>Yük devretme ve yeniden çalışma yazılım sürümleri arasında

| Yük devri / geri başarısız | Fiziksel cihaz | Bulut gereci |
| --- | --- | --- |
| Güncelleştirme 3 güncelleştirme 4 |Katmanlı birimlerin katmanlı üzerinde başarısız. <br></br>Yerel olarak sabitlenmiş üzerinde yerel olarak sabitlenmiş birimlerin başarısız. <br></br> Güncelleştirme 4 aygıtta bir anlık görüntüsünü olduğunda bir yük devretme aşağıdaki [heatmap tabanlı izleme](storsimple-update4-release-notes.md#whats-new-in-update-4) devreye girer. |Yerel olarak katmanlı birimler yük devretme sabitlenir. |
| Güncelleştirme 3'ü güncelleştirme 4 |Katmanlı birimlerin katmanlı üzerinde başarısız. <br></br>Yerel olarak sabitlenmiş üzerinde yerel olarak sabitlenmiş birimlerin başarısız. <br></br> Yedekleri geri yüklemek için kullanılan heatmap meta verileri korur. <br></br>Heatmap tabanlı izleme, güncelleştirme bir yeniden çalışma aşağıdaki 3'te kullanılamıyor. |Yerel olarak katmanlı birimler yük devretme sabitlenir. |


## <a name="device-failover-scenarios"></a>Cihaz yük devretme senaryoları

Bir olağanüstü durum varsa, StorSimple Cihazınızı vermesine tercih edebilirsiniz:

* [Bir fiziksel cihaza](storsimple-8000-device-failover-physical-device.md).
* [Kendisine](storsimple-8000-device-failover-same-device.md).
* [Bulut uygulamasına](storsimple-8000-device-failover-cloud-appliance.md).

Önceki makaleler her yukarıdaki yük devretme durumları için ayrıntılı adımlar sağlar.


## <a name="failback"></a>Yeniden çalışma

Güncelleştirme 3 ve sonraki sürümler için StorSimple geri dönme de destekler. Yeniden çalışma yalnızca yük devretme tersidir, hedef kaynağı haline gelir ve özgün kaynak aygıt artık yük devretme sırasında hedef cihaz haline gelir. 

Yeniden çalışma sırasında StorSimple verileri geri birincil konumu, g/ç durur ve uygulama etkinlik yeniden eşitler ve geçişleri özgün konuma geri.

Bir yük devretme işlemi tamamlandıktan sonra StorSimple aşağıdaki eylemleri gerçekleştirir:

* StorSimple kaynak aygıttan devredildi birim kapsayıcıları temizler.
* StorSimple birim kapsayıcısı (kaynak aygıtta devredilir) başına bir arka plan iş başlatır. Geri işi devam ederken başarısız çalışırsanız, bunu belirten bir bildirim alırsınız. İş yeniden başlatma işlemi tamamlanana kadar bekleyin.
* Birim kapsayıcıları silme işlemini tamamlamak için geçen süre veri miktarı, işlemi için verileri, yedekleme sayısı ve kullanılabilir ağ bant genişliği geçerlilik süresi gibi çeşitli etkenlere bağlıdır.

Yük devretme sınaması işlemlerini planlama veya test çalışmaları, daha az veri (GB) ile birim kapsayıcıları test etmenizi öneririz. Genellikle, 24 saat yük devretme işlemi tamamlandıktan sonra yeniden başlatabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Q. **DR başarısız veya kısmi başarılı olup olmadığını ne olur?**

A. DR başarısız olursa, yeniden deneyin öneririz. İkinci aygıt yük devretme işine ilk iş ilerleme durumunu farkındadır ve sonraki sürümlerde, bu noktadan başlar.

Q. **Cihaz yük devretme işlemi devam ederken bir aygıtı silme?**

A. Bir kurtarma işlemi devam ederken, bir cihaz silemezsiniz. DR tamamlandıktan sonra yalnızca cihazınız silebilirsiniz. Cihaz yük devretme iş ilerleme durumunu izleyebilirsiniz **işleri** dikey.

Q. **Böylece kaynak cihazdaki yerel veriler silinir çöp toplama kaynak aygıtta başlattığınızda?**

A. Yalnızca cihazın tamamen temizlenmesi sonra atık toplama kaynak aygıtta etkinleştirilir. Temizleme, birimler, yedek nesnesini (verileri değil), birim kapsayıcıları ve ilkeleri gibi kaynak aygıttan devredilir nesneleri temizleniyor içerir.

Q. **Birim kapsayıcıları kaynak aygıt ile ilişkili silme işi başarısız olursa ne olur?**

A.  Silme işlemi başarısız olursa, birim kapsayıcıları el ile silebilirsiniz. İçinde **aygıtları** dikey penceresinde kaynak Cihazınızı seçin ve tıklatın **birim kapsayıcıları**. Dikey pencerenin altındaki ve üzerinden başarısız birim kapsayıcıları seçin, **silmek**. Tüm sildikten sonra başarısız kaynak cihazdaki birim kapsayıcıları yeniden başlatabilirsiniz. Daha fazla bilgi için Git [birim kapsayıcısı silme](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği olağanüstü durum kurtarma (BCDR)

Bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu, tüm Azure veri merkezi çalışmıyor oluşur. Bu senaryo, StorSimple cihaz Yöneticisi hizmetiniz ve ilişkili StorSimple cihazları etkileyebilir.

Yalnızca bir olağanüstü durum oluşmadan önce StorSimple cihazını kaydedildiyse, bu cihazı fabrika ayarlarına uygulanabilecek gerekebilir. Olağanüstü durum sonra StorSimple cihazı çevrimdışı olarak Azure portalda görüntülenir. Bu cihaz Portalı'ndan silinmesi gerekir. Cihazı fabrika varsayılan ayarlarına ve hizmeti ile yeniden kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

Cihaz yük devretme gerçekleştirmek hazır olduğunuzda aşağıdaki senaryoları ayrıntılı yönergeler için birini seçin:

- [Başka bir fiziksel cihaza yük devri](storsimple-8000-device-failover-physical-device.md)
- [Aynı cihaza yük devri](storsimple-8000-device-failover-same-device.md)
- [StorSimple bulut uygulaması için yük devri](storsimple-8000-device-failover-cloud-appliance.md)

Cihazınızı başarısızsa, aşağıdaki seçeneklerden birini seçin:

* [StorSimple Cihazınızı silmek veya devre dışı bırakma](storsimple-8000-deactivate-and-delete-device.md).
* [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

