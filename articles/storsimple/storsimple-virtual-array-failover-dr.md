---
title: "StorSimple sanal dizinin olağanüstü durum kurtarma ve aygıt yük devretme | Microsoft Docs"
description: "Yük devretme hakkında daha fazla StorSimple sanal dizinizi öğrenin."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12079f8dbc409afe5acc274fa08bda878c90b76e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Olağanüstü durum kurtarma ve cihaz için yük devretme StorSimple sanal dizinizi Azure Portalı aracılığıyla

## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple sanal ayrıntılı adımlar da dahil olmak üzere başka bir sanal dizisine üzerinden vermesine dizinizi için olağanüstü durum kurtarma açıklanmaktadır. Bir yük devretme verilerinizden taşımanızı sağlayan bir *kaynak* için veri merkezindeki cihaz bir *hedef* aygıt. Hedef aygıt, aynı veya farklı bir coğrafi konum içinde bulunabilir. Cihaz yük devretme için tüm aygıttır. Yük devretme sırasında bulut veri kaynağı cihaz için sahipliği, hedef aygıt değiştirir.

Bu makalede yalnızca StorSimple sanal diziler için geçerlidir. 8000 serisi aygıt üzerinden vermesine Git [aygıt yük devretme ve olağanüstü durum kurtarma StorSimple cihazınızın](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Olağanüstü durum kurtarma ve aygıt yük devretme nedir?

Bir olağanüstü durum kurtarma (DR) senaryosunda birincil cihaz çalışmamaya başlar. Bu senaryoda, başka bir aygıt için başarısız aygıtla ilişkili bulut verilerini taşıyabilirsiniz. Birincil cihaz olarak kullanabileceğiniz *kaynak* ve başka bir aygıt olarak belirtin *hedef*. Bu işlem olarak adlandırılır *yük devretme*. Yük devretme sırasında tüm birimler veya paylaşımlar kaynak cihazdaki sahipliğini değiştirmek ve hedef aygıta aktarılır. Hiçbir verinin filtreleme izin verilir.

DR harita tabanlı ısı katmanlama kullanarak ve izleme cihazın tamamen geri yükleme modellenir. Isı Haritası göre verileri ısı değer atayarak tanımlanan üzerinde okuma ve desenler yazma. Bu ısı harita sonra katmanları en düşük ısı veri öbekleri buluta ilk yerel katmanda (en kullanılan) yüksek ısı veri öbekleri tutarken. Bir DR sırasında geri yüklemek ve bulut verilerden rehydrate ısı Haritası StorSimple kullanır. Cihaz (dahili olarak belirlenen) tüm birimleri/paylaşımları son son yedekleme getirir ve o yedekten geri yükleme gerçekleştirir. Sanal dizinin tüm DR işlemini düzenler.

> [!IMPORTANT]
> Kaynak aygıt aygıt yük devretme sonunda silinir ve bu nedenle bir yeniden çalışma desteklenmez.
> 
> 

Olağanüstü durum kurtarma aygıt yük devretme özelliği düzenlenir ve başlatıldığına **aygıtları** dikey. Bu dikey StorSimple cihaz Yöneticisi hizmetinize bağlı tüm StorSimple cihazlar tablo haline getirir. Her cihaz için kolay ad, durumu, sağlanan ve maksimum kapasite, türünü ve model görebilirsiniz.

## <a name="prerequisites-for-device-failover"></a>Cihaz yük devretme için Önkoşullar

### <a name="prerequisites"></a>Ön koşullar

Cihaz yük devretme için aşağıdaki önkoşullara uyduğunuzdan emin olun:

* Kaynak aygıt olması gerektiğinde bir **devre dışı** durumu.
* Hedef aygıt olarak göstermek gereken **ayarlamak hazır** Azure portalında. Aynı veya daha yüksek kapasite hedef sanal dizisi sağlayın. Yerel web kullanıcı Arabirimi, yapılandırmak ve hedef sanal dizinin başarıyla kaydetmek için kullanın.
  
  > [!IMPORTANT]
  > Hizmeti aracılığıyla kayıtlı sanal Cihazınızı yapılandırmak üzere çalışmayın. Herhangi bir aygıt yapılandırma hizmeti aracılığıyla yapılmalıdır.
  > 
  > 
* Hedef aygıt kaynak aygıt aynı ada sahip olamaz.
* Kaynak ve hedef aygıt aynı türden olması gerekir. Yalnızca başka bir dosya sunucusu için dosya sunucusu olarak yapılandırılmış sanal bir dizi üzerinden başarısız olabilir. Aynı iSCSI sunucusu için geçerlidir.
* Bir dosya sunucusu DR için hedef aygıt, kaynağı olarak aynı etki alanına öneririz. Bu yapılandırma, paylaşım izinleri otomatik olarak çözümlenir sağlar. Yalnızca Yük devretme aynı etki alanında bir hedef cihaz için.
* DR için kullanılabilir hedef kaynak cihaza karşılaştırıldığında aynı veya daha büyük kapasitede olan aygıtları aygıtlardır. Hizmetinize bağlı, ancak yeterli alan ölçütleri karşılamayan cihazlar hedef cihazlar olarak kullanılabilir değil.

### <a name="other-considerations"></a>Diğer konular

* Planlanmış bir yük devretme için 
  
  * Çevrimdışı kaynak cihazda tüm birimler veya paylaşımlar almanızı öneririz.
  * Cihaz yedekleyin ve veri kaybını en aza indirmek için yük devretme kümelemesiyle devam öneririz. 
* Planlanmamış bir yük devretme için aygıt en son yedekleme verileri geri yüklemek için kullanır.

### <a name="device-failover-prechecks"></a>Cihaz yük devretme prechecks

DR başlamadan önce aygıt prechecks gerçekleştirir. Bu denetimler, DR başladığı zaman hata oluşmamasına sağlamaya yardımcı olur. Prechecks şunları içerir:

* Depolama hesabı doğrulanıyor.
* Azure bulut bağlantısı denetleniyor.
* Hedef aygıt kullanılabilir alanı denetleniyor.
* Bir iSCSI sunucu kaynak aygıt birim olup olmadığı denetleniyor
  
  * Geçerli ACR adları.
  * Geçerli IQN (220 karakteri aşan değil).
  * Geçerli CHAP parolalar (12-16 karakter uzunluğunda).

Yukarıdaki hiçbirini prechecks, başarısız DR devam edemiyor. Bu sorunları çözün ve sonra kurtarma işlemini yeniden deneyin.

DR başarıyla tamamlandıktan sonra bulut veri kaynağı cihazın sahipliğini hedef aygıta aktarılır. Kaynak cihaz sonra artık Portalı'nda kullanılamıyor. Tüm birimlerin/paylaşımlarına kaynak cihaz erişimi engellenir ve hedef aygıt etkin hale gelir.

> [!IMPORTANT]
> Cihaz artık kullanılabilir olsa da, ana bilgisayar sisteminde sağlanan sanal makine hala kaynakları tüketilmesine neden olabilir. DR başarıyla tamamlandıktan sonra bu sanal makine ana bilgisayar sisteminizden silebilirsiniz.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Sanal bir dizi yük devri

Sağlamak, yapılandırmak ve bu yordamı çalıştırmadan önce başka bir StorSimple sanal dizinin, StorSimple cihaz Yöneticisi Hizmeti'ne kaydedin öneririz.

> [!IMPORTANT]
> 
> * Bir StorSimple 8000 serisi aygıttan 1200 sanal cihaza yük devri yapılamıyor.
> * Üzerinden bir Federal Bilgi İşleme Standardı (etkin FIPS) sanal aygıttan başka bir etkin FIPS aygıt veya kamu Portalı'nda dağıtılan FIPS olmayan aygıt başarısız olabilir.


Hedef bir StorSimple sanal cihaz için cihaz geri yüklemek için aşağıdaki adımları gerçekleştirin.

1. Sağlamak ve karşılayan bir hedef cihaz yapılandırma [aygıt yük devretme için Önkoşullar](#prerequisites). Yerel web kullanıcı Arabirimi aracılığıyla cihaz yapılandırmasını tamamlamak ve StorSimple cihaz Yöneticisi hizmetinize kaydedin. Bir dosya sunucusu oluşturuyorsanız 1 adıma gidin [dosya sunucusu olarak ayarlanmış](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Bir iSCSI sunucusu oluşturuyorsanız 1 adıma gidin [iSCSI sunucusu olarak ayarlanmış](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Konakta birimler/paylaşımları çevrimdışı olur. Birimleri/paylaşımları çevrimdışına almak için ana bilgisayar işletim sistemi – özel yönergelerine bakın. Zaten çevrimdışı olursa, aşağıdakileri yaparak cihazda tüm birimleri/paylaşımları çevrimdışı bulundurmanız gerekir.
   
    1. Git **aygıtları** dikey ve Cihazınızı seçin.
   
    2. Git **Ayarları > Yönet > paylaşımları** (veya **Ayarları > yönetmek > birimleri**). 
   
    3. Bir paylaşım/birimi seçin, sağ tıklatın ve seçin **çevrimdışına**. 
   
    4. Onayınız istendiğinde denetleyin **bu paylaşım çevrimdışı duruma getirmeden etkisini anlama.** 
   
    5. Tıklatın **çevrimdışına**.

3. StorSimple cihaz Yöneticisi hizmetinize Git **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde, seçin ve kaynak aygıtınızı tıklatın.

4. İçinde **cihaz Pano** dikey penceresinde tıklatın **etkinliğini**.

5. İçinde **etkinliğini** dikey penceresinde onaylamanız istenir. Aygıt devre dışı bırakma olan bir *kalıcı* işlem geri alınamaz. Ana bilgisayarda paylaşımları/birimlerinizi çevrimdışı yapılacak anımsatma yapılır. Onayla tıklatıp aygıt adı yazın **etkinliğini**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. Devre dışı bırakma işlemini başlatır. Devre dışı bırakma işlemi başarıyla tamamlandıktan sonra bir bildirim alırsınız.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Cihazlar sayfasında, cihaz durumu şimdi değiştirir **devre dışı**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. İçinde **aygıtları** dikey penceresinde, seçin ve yük devretme için devre dışı bırakılmış kaynak cihaz seçeneğine tıklayın. 
9. İçinde **cihaz Pano** dikey penceresinde tıklatın **yük devri**. 
10. İçinde **aygıt üzerinden başarısız** dikey penceresinde aşağıdakileri yapın:
    
    1. Kaynak aygıt alanı otomatik olarak doldurulur. Kaynak cihaz için toplam veri boyutuna dikkat edin. Veri boyutu hedef aygıttaki kullanılabilir kapasiteden daha az olmalıdır. Aygıt adı, toplam kapasite ve yük devredildi paylaşımları adları gibi kaynak aygıtla ilişkili ayrıntılarını gözden geçirin.

    2. Kullanılabilir aygıtları açılır listeden seçin bir **hedef aygıt**. Yalnızca yeterli kapasiteye sahip cihazları açılır listede görüntülenir.

    3. Denetleyin **hedef aygıta veriler üzerinde bu işlemi yapamaz anlıyorum**. 

    4. Tıklatın **yük devri**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Bir yük devretme iş başlatır ve bir bildirim alırsınız. Git **cihazlar > işleri** yük devretme izlemek için.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. İçinde **işleri** dikey penceresinde kaynak cihaz için oluşturulan bir yük devretme işine bakın. Bu iş, DR prechecks gerçekleştirir.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     DR prechecks başarılı olduktan sonra Yük devretme işine kaynak Cihazınızda var. her paylaşım/birim için geri yükleme işleri oluşturma.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Yük devretme işlemi tamamlandıktan sonra Git **aygıtları** dikey.
    
    1. Seçin ve yük devretme işlemi için hedef cihaz olarak kullanılan StorSimple cihazı tıklayın.
    2. Git **ayarlar > Yönetim > paylaşımları** (veya **birimleri** varsa iSCSI sunucusu). İçinde **paylaşımları** dikey penceresinde, eski aygıttan paylaşımlarını (birimler) görüntüleyebilirsiniz.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Etmeniz [bir DNS diğer adı oluşturma](https://support.microsoft.com/kb/168322) böylece bağlanmaya çalıştığınız tüm uygulamalar için yeni cihaz yönlendirilirsiniz.

## <a name="errors-during-dr"></a>DR sırasında hatalar

**DR sırasında bulut bağlantı kesintisi**

Bulut bağlantı DR başlatıldı ve cihaz önce geri yükleme tamamlandıktan sonra kesintiye uğrarsa, DR başarısız olur. Failore bildirim alırsınız. Bir hedef cihaz DR için işaretlenmiş *kullanılamaz.* Aynı hedef cihaz için gelecekteki DRs kullanamazsınız.

**Uyumlu hedef cihaz yok**

Kullanılabilir hedef cihazlar yeterli alan yoksa hata uyumlu hedef aygıt yok etkili olması için bkz.

**Precheck hataları**

Prechecks birini memnun değil, precheck hataları konusuna bakın.

## <a name="business-continuity-disaster-recovery-bcdr"></a>İş sürekliliği olağanüstü durum kurtarma (BCDR)

Bir iş sürekliliği olağanüstü durum kurtarma (BCDR) senaryosu, tüm Azure veri merkezi çalışmıyor oluşur. Bu, StorSimple cihaz Yöneticisi hizmetiniz ve ilişkili StorSimple cihazları etkileyebilir.

Yalnızca bir olağanüstü durum oluşmadan önce kaydedilen StorSimple cihazlar varsa, bu StorSimple cihazlar silinmesi gerekebilir. Olağanüstü durum sonra yeniden oluşturun ve bu aygıtların yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılır hakkında daha fazla bilgi [StorSimple sanal yerel web kullanıcı arabirimini kullanarak dizinizi yönetmek](storsimple-ova-web-ui-admin.md).

