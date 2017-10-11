---
title: StorSimple 8000 serisi bir birimde kopyalama | Microsoft Docs
description: "Farklı kopya türleri ve kullanım tanımlar ve tek tek bir birim bir StorSimple 8000 serisi cihazda kopyalamak için bir yedek nasıl kullanabileceğinizi açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 70c85bcb2c26d2ad3d0515d24e028f84495634c0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>StorSimple cihaz Yöneticisi hizmeti Azure portalında bir birim kopyalamak için kullanın.

## <a name="overview"></a>Genel Bakış

Bu öğretici bir yedekleme kümesi aracılığıyla tek bir birim kopyalamak için nasıl kullanabileceğinizi açıklar **yedekleme kataloğu** dikey. Ayrıca arasındaki farkı açıklar *geçici* ve *kalıcı* klonlar. Bu öğreticideki yönergeler güncelleştirme 3 veya sonraki sürümünü çalıştıran tüm StorSimple 8000 serisi aygıta uygular.

StorSimple cihaz Yöneticisi hizmeti **yedekleme kataloğu** dikey penceresinde el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Bu gibi durumlarda, bir birim sonra bir yedekleme kopyalama kümesi seçebilirsiniz.

 ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Bir birim kopyalama dikkat edilmesi gereken noktalar

Aşağıdaki bilgiler bir birim kopyalarken göz önünde bulundurun.

- Bir kopya normal birimi olarak aynı şekilde davranır. Bir birimde mümkündür herhangi bir işlem kopya için kullanılabilir.

- İzleme ve varsayılan yedekleme otomatik olarak kopyalanan bir birimde devre dışı. Kopyalanan bir birimi tüm yedeklemeler için gerekir.

- Yerel olarak sabitlenmiş bir birim katmanlı birim kopyalandı. Yerel olarak sabitlenmiş için kopyalanan birimi gerekiyorsa, kopyalama işlemi başarıyla tamamlandıktan sonra kopyanın yerel olarak sabitlenmiş bir birim dönüştürebilirsiniz. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Kopyalanan bir birimden dönüştürmeyi deneyin (Bunu yine bir geçici kopya olduğunda) hemen kopyalandıktan sonra yerel olarak sabitlenmiş katmanlı dönüştürme şu hata iletisiyle başarısız olur:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Yalnızca farklı bir cihaz açın kopyalama, bu hata alınır. Yerel olarak sabitlenmiş geçici kopya kalıcı bir kopya ilk dönüştürürseniz birimi başarıyla dönüştürebilirsiniz. Kalıcı bir kopya dönüştürmek için geçici kopya bir bulut anlık görüntüsünü.

## <a name="create-a-clone-of-a-volume"></a>Bir birimin bir kopyasını oluşturun

Yerel kullanarak aynı aygıt, başka bir aygıt veya Bulut uygulaması bir kopya oluşturabilirsiniz veya Bulut anlık görüntüsü.

Aşağıdaki yordamda bir kopya yedekleme Kataloğu'ndan oluşturmayı açıklar.  Gitmek için kopya başlatmak için alternatif bir yöntemi olan **birimleri**, bir birim seçin, sağ tıklatarak bağlam menüsü çağırma ve seçmek için **kopya**.

Yedekleme Kataloğu'ndan biriminiz bir kopyasını oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-volume"></a>Birim kopyalama

1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **yedekleme kataloğu**.

2. Bir yedekleme kümesi aşağıdaki gibi seçin:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığı belirtin.
   4. Tıklatın **Uygula** bu sorguyu yürütmek için.

    Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
   
    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Geri yüklemeden önce bu birimleri ana bilgisayar ve aygıt çevrimdışına alınması gerekir. Birimleri erişimi **birimleri** Cihazınızı dikey ve adımları izleyin [bir birim çevrimdışına](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) çevrimdışı duruma getirmek için.
   
   > [!IMPORTANT]
   > Çevrimdışı birimler cihazda olabilmesi birimlerde çevrimdışı konak ilk olarak, gerçekleştirdiğinizden emin olun. Konakta birimler çevrimdışı almazsanız, olası veri bozulmasına yol açabilir.
   
4. Geri gidin **yedekleme kataloğu** ve yedekleme kümesinde bir birim seçin. Sağ tıklayın ve ardından bağlam menüsünden seçin **kopya**.

   ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. İçinde **kopya** dikey penceresinde, aşağıdaki adımları uygulayın:
   
    1. Bir hedef cihaz tanımlayın. Bu, kopyanın oluşturulacağı konumdur. Aynı aygıt seçin veya başka bir aygıt belirtin.

      > [!NOTE]
      > Kopya için gereken kapasiteyi hedef cihazda kullanılabilir kapasiteden daha düşük olduğundan emin olun.
       
    2. Kopya için benzersiz birim adı belirtin. Adı 3 ile 127 karakter içermelidir.
      
        > [!NOTE]
        > **Kopya birim olarak** alan **katmanlı** yerel olarak sabitlenmiş bir birim kopyalama olsa bile. Bu ayarı değiştiremezsiniz; Ancak, yerel olarak da sabitlenmelidir kopyalanan birimi gerekiyorsa, kopya yerel olarak sabitlenmiş bir birim dönüştürebilir, kopya başarıyla oluşturduktan sonra. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
    3. Altında **bağlı Konaklar**, kopya için bir erişim denetimi kaydı (ACR) belirtin. Yeni bir ACR ekleyebilir veya var olan listeden seçin. ACR hangi ana bilgisayarların bu kopya erişebilirsiniz belirler.
      
        ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. Tıklatın **kopya** işlem tamamlanamadı.

4. Bir kopya iş başlatılır ve kopya başarıyla oluşturulduğunda size bildirilir. İş bildirimi tıklatın veya gitmek **işleri** kopya işi izlemek için dikey penceresini.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Kopya işi tamamlandıktan sonra aygıtınıza gidin ve ardından **birimleri**. Birimleri listesinde, yeni kaynak biriminin aynı birim kapsayıcısı oluşturuldu kopya görmeniz gerekir.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Bu yolla oluşturulan bir kopya geçici kopya ' dir. Kopya türleri hakkında daha fazla bilgi için bkz: [geçici ve kalıcı klonlar](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Geçici ve kalıcı klonlar karşılaştırması
Yalnızca başka bir cihaza kopyaladığınızda geçici klonlar oluşturulur. Bir yedekleme kümesi için farklı bir cihaz StorSimple cihaz Yöneticisi tarafından yönetilen belirli bir birimden kopyalayabilirsiniz. Geçici kopya özgün birimin verilere başvuru vardır ve okuma ve yerel olarak hedef cihazda yazmak için bu verileri kullanır.

Bir geçici kopya bir bulut anlık görüntüsünü sonra sonuçta elde edilen kopya olan bir *kalıcı* kopya. Bu işlem sırasında verilerin bir kopyasını Bulutu üzerinde oluşturulur ve bu verileri kopyalamak için saat veri ve (bir Azure Azure kopyalama budur) Azure gecikmeleri boyutu tarafından belirlenir. Bu işlem için hafta gün sürebilir. Geçici kopya kalıcı bir kopya olur ve tüm başvuruları gelen kopyalandı özgün birimin verilere sahip değil.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı klonlar senaryoları
Aşağıdaki bölümlerde geçici ve kalıcı klonlar kullanılabilir örnek durumlar açıklanmaktadır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir kopya ile öğe düzeyinde kurtarma
Bir yıl eski Microsoft PowerPoint sunusu dosyası kurtarmak gerekir. BT yöneticiniz bu süre belirli yedekten tanımlar ve birim filtreler. Yönetici daha sonra birim klonlar, aradığınız dosya bulur ve olanak sağlar. Bu senaryoda, bir geçici kopya kullanılır.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Kalıcı bir kopya ile üretim ortamında test etme
Üretim ortamında test hata doğrulamanız gerekir. Üretim ortamında birimin bir kopyasını oluşturun ve sonra bu kopya bağımsız kopyalanan birim oluşturmak için bir bulut anlık görüntüsünü. Bu senaryoda, kalıcı bir kopya kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir yedeklemek kümesinden StorSimple birimini geri](storsimple-8000-restore-from-backup-set-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

