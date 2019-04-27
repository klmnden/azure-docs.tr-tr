---
title: StorSimple 8000 serisi üzerindeki bir birimi kopyalama | Microsoft Docs
description: Kullanım ve farklı kopya türleri ve bir yedekleme kümesi bir StorSimple 8000 serisi Cihazınızı tek bir birimde kopyalamak için nasıl kullanabileceğiniz açıklanmaktadır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/05/2017
ms.author: alkohli
ms.openlocfilehash: 84734aefb72a3330d99c5707b461de2cd5e30484
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637921"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>Bir birimi kopyalama için Azure portalında StorSimple cihaz Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir yedekleme kümesi aracılığıyla tek bir birimin kopyalamak için nasıl kullanabileceğiniz açıklanır **yedekleme kataloğu** dikey penceresi. Ayrıca arasındaki farkı açıklar *geçici* ve *kalıcı* kopyalar. Bu öğreticideki yönergeler Update 3 veya sonraki sürümü çalıştıran tüm StorSimple 8000 serisi cihaz için geçerlidir.

StorSimple cihaz Yöneticisi hizmeti **yedekleme kataloğu** dikey penceresinde el ile veya otomatik yedeklemeler alındığında, oluşturulan tüm yedekleme kümelerini görüntüler. Ardından, bir birim kopyalamak için bir yedekleme seçebilirsiniz.

 ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Bir birim kopyalama için dikkat edilmesi gerekenler

Birim kopyalama gerçekleştirilirken aşağıdaki bilgileri göz önünde bulundurun.

- Bir kopya, normal bir birim olarak aynı şekilde davranır. Bir birimde mümkündür herhangi bir işlem, bir kopya için kullanılabilir.

- İzleme ve varsayılan yedekleme otomatik olarak klonlanmış bir birimde devre dışı. Kopyalanan bir birim tüm yedeklemeler için yapılandırmanız gerekir.

- Yerel olarak sabitlenmiş bir birim katmanlı birim kopyalanmış olan. Yerel olarak sabitlenmiş birimin kopyalanan gerekiyorsa, kopyalama işlemi başarıyla tamamlandıktan sonra kopya için yerel olarak sabitlenmiş bir birim dönüştürebilirsiniz. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirmek](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Kopyalanan bir birimden dönüştürmeyi denemeden, (hala geçici bir kopya olduğunda) hemen kopyalandıktan sonra yerel olarak sabitlenmiş katmanlı dönüştürme şu hata iletisiyle başarısız olur:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Yalnızca açın farklı bir cihaz kopyalama, bu hata alınır. İlk geçici kopya kalıcı kopyadan dönüştürürseniz, yerel olarak sabitlenmiş için birimi başarıyla dönüştürebilirsiniz. Geçici kopya kalıcı kopyadan dönüştürmek için bulut anlık görüntüsünü alın.

## <a name="create-a-clone-of-a-volume"></a>Bir birimin bir kopyasını oluşturma

Yerel bir kullanarak aynı cihaza, başka bir cihaz veya Bulut Gereci bir kopya oluşturabilirsiniz veya Bulut anlık görüntüsü.

Aşağıdaki yordam bir kopya yedekleme Kataloğu'ndan oluşturmayı açıklar.  Kopya başlatmak için alternatif bir yöntem adresine gitmektir **birimleri**, bir birim seçin ve sonra seçin ve bağlam menüsünü açmak için sağ **kopya**.

Yedekleme Kataloğu'ndan toplu bir kopyasını oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-volume"></a>Bir birimi kopyalama

1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **yedekleme kataloğu**.

2. Bir yedekleme kümesi şu şekilde seçin:
   
   1. Uygun bir cihaz seçin.
   2. Aşağı açılan listeden, birim veya yedekleme ilkesi seçmek için istediğiniz yedekleme için seçin.
   3. Zaman aralığını belirtin.
   4. Tıklayın **Uygula** bu sorguyu yürütmek için.

      Yedeklemeleri seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme ayarları listesinde görüntülenmelidir.
   
      ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. İlişkili birim görüntülemek ve yedekleme kümesinde bir birim seçmek için yedekleme kümesini genişletin. Sağ tıklayın ve ardından bağlam menüsünden **kopya**.

   ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. İçinde **kopya** dikey penceresinde aşağıdaki adımları uygulayın:
   
   1. Bir hedef cihaz belirleyin. Bu, kopya oluşturulacağı konumdur. Aynı cihazı seçin veya başka bir cihaz belirtin.

      > [!NOTE]
      > Bir kopya için gereken hedef cihazda kullanılabilir kapasiteden daha düşük olduğundan emin olun.
       
   2. Kopya için benzersiz bir birim adı belirtin. Ad 3-127 karakter içermelidir.
      
       > [!NOTE]
       > **Toplu olarak kopyalama** alan **katmanlı** bile yerel olarak sabitlenmiş bir birim'nu kopyalama. Bu ayarı değiştiremezsiniz; Ancak, yerel olarak da sabitlenmelidir kopyalanan birim gerekiyorsa, kopya için yerel olarak sabitlenmiş bir birim dönüştürebilirsiniz, kopya başarıyla oluşturduktan sonra. Yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirmek](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
   3. Altında **bağlı Konaklar**, kopya için erişim denetimi kaydı (ACR) belirtin. Yeni ACR ekleyin veya mevcut bir listeden seçim yapabilirsiniz. ACR barındıran bu kopya erişip belirler.
      
       ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

   4. Tıklayın **kopya** işlem tamamlanamadı.

4. Bir kopyalama işi başlatıldı ve kopya başarıyla oluşturulduğunda size bildirilir. İş bildirimi tıklayabilir veya **işleri** kopyalama işi izlemek için dikey pencere.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Kopyalama işlemi tamamlandıktan sonra cihazınıza gidin ve ardından **birimleri**. Birimler listesinde, yeni kaynak biriminde aynı birim kapsayıcısı oluşturulan kopya görmeniz gerekir.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Bu şekilde oluşturulan bir kopya geçici bir kopya olması. Kopya türleri hakkında daha fazla bilgi için bkz. [geçici ve kalıcı klonlar](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Geçici ve kalıcı kopyalar
Geçici klonları yalnızca başka bir cihaza kopyaladığınızda oluşturulur. StorSimple cihaz Yöneticisi tarafından yönetilen başka bir cihaz için bir yedekleme belirli bir birimden kopyalayabilirsiniz. Geçici kopya, özgün birimin verilere başvuru var ve okumak ve hedef cihazda yerel olarak yazmak için bu verileri kullanır.

Geçici bir kopya bulut anlık görüntüsünü alma sonra elde edilen kopya olduğu bir *kalıcı* kopya. Bu işlem sırasında verilerin bir kopyasını bulutta oluşturulur ve bu verileri kopyalama süresi (Bu, Azure'dan Azure'a kopyalama) Azure gecikme süreleri ve veri boyutu tarafından belirlenir. Bu işlem, gün için hafta sürebilir. Geçici kopya kalıcı kopyadan olur ve tüm başvuruları öğesinden kopyalandı özgün birimin verilere sahip değil.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı klonları senaryoları
Aşağıdaki bölümlerde, geçici ve kalıcı kopyaları kullanılabilir örnek durumları açıklanmaktadır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir kopya ile öğe düzeyinde kurtarma
Bir yıl eski Microsoft PowerPoint sunu dosyası kurtarmanız gerekecektir. BT yöneticiniz bu süre belirli yedekten tanımlar ve sonra birim filtreler. Yönetici ardından birimin klonlar, aradığınız dosya bulur ve olanak sağlar. Bu senaryoda, geçici bir kopya kullanılır.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Kalıcı bir kopya ile üretim ortamında test etme
Üretim ortamında test bir hatayı doğrulamak gerekir. Üretim ortamında birimin bir kopyasını oluşturur ve sonra bu kopya bir bağımsız kopyalanan birim oluşturmak için bulut anlık görüntüsünü alın. Bu senaryoda, kalıcı bir kopya kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [bir yedekleme kümesinden StorSimple birimini geri](storsimple-8000-restore-from-backup-set-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

