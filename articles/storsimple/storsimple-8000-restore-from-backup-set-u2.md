---
title: "Bir birim üzerinde bir StorSimple 8000 serisi yedekten | Microsoft Docs"
description: "StorSimple cihaz Yöneticisi hizmeti yedekleme kataloğu bir yedeklemek kümesinden StorSimple birimini geri için nasıl kullanılacağını açıklar."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: aff0710ead4f76bb80c38e2d88fe9cd3ce6a7b48
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Bir yedeklemek kümesinden StorSimple birimini geri

## <a name="overview"></a>Genel Bakış

Bu öğretici, var olan bir yedekleme kümesi kullanarak bir StorSimple 8000 serisi cihazda gerçekleştirilen geri yükleme işlemi açıklanmaktadır. Kullanım **yedekleme kataloğu** dikey penceresine geri yükleme yerel bir birimden veya Bulut yedekleme. **Yedekleme kataloğu** dikey penceresinde el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Hemen veri arka planda yüklenirken bir yedekleme kümesi geri yükleme işleminin birimi çevrimiçi duruma getirir.

Geri yüklemeyi başlatmak için bir alternatif gitmek için yöntemidir **cihazlar > [Cihazınızı] > birimleri**. İçinde **birimleri** dikey penceresinde, bir birim seçin sağ bağlam menüsü çağırma ve ardından **geri**.

## <a name="before-you-restore"></a>Geri yüklemeden önce

Bir geri yükleme başlamadan önce aşağıdaki uyarılarla gözden geçirin:

* **Birim çevrimdışı duruma** – geri yükleme işlemini başlatmadan önce birim hem konak hem de cihaz çevrimdışı gerçekleştirin. Geri yükleme işlemi, birimi çevrimiçi otomatik olarak cihazda duruma getirir. rağmen ana bilgisayarda el ile cihaz çevrimiçi getirmeniz gerekir. Cihazda çevrimiçi bir birimdir hemen konakta birimin çevrimiçi kullanıma sunabilirsiniz. (Geri yükleme işlemi tamamlanana kadar bekleyin gerek yok.) Yordamlar için Git [bir birim çevrimdışına](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline).

* **Birim türü geri yükleme sonrasında** – silinen birimler geri yüklenir anlık görüntüdeki türüne göre; diğer bir deyişle, yerel olarak sabitlenmiş birimleri yerel olarak sabitlenmiş birimleri olarak geri yüklenir ve katmanlı birimleri katmanlı birimler olarak geri yüklenir.

    Var olan birimler için geçerli kullanım türü biriminin anlık depolanan türü geçersiz kılar. Birim türü katmanlı ve birim türü artık yerel olarak (gerçekleştirilen bir dönüştürme işlemi nedeniyle) sabitlenmiş alındığı anlık bir birime geri yüklerseniz, örneğin, ardından birimin yerel olarak sabitlenmiş bir birim geri yüklenir. Benzer şekilde, var olan yerel olarak sabitlenmiş bir birim genişletilmiş ve daha sonra birim daha küçük olduğunda gerçekleştirilecek daha eski bir anlık görüntüden geri, geri yüklenen toplu geçerli genişletilmiş boyutu korur.

    Birimi geri yüklenirken yerel olarak sabitlenmiş bir birim için katmanlı birim veya yerel olarak sabitlenmiş bir birim katmanlı birim için bir birim dönüştürülemiyor. Geri yükleme işlemi tamamlandıktan ve daha sonra başka bir türüne birim dönüştürebilirsiniz kadar bekleyin. Bir birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type). 

* **Birim boyutu geri yüklenen birimde yansıtılır** – (yerel olarak sabitlenmiş birimlerin tam olarak sağlandıktan nedeniyle), silinmiş olan yerel olarak sabitlenmiş bir birim geri yüklüyorsanız bu önemli bir konudur. Daha önce silinmiş yerel olarak sabitlenmiş bir birim geri yüklemeyi denemeden önce yeterli alan olduğundan emin olun.

* **Bu geri yüklenirken bir birim genişletilemiyor** – birimini genişletin girişiminde bulunmadan önce geri yükleme işlemi tamamlanana kadar bekleyin. Bir birim genişletme hakkında daha fazla bilgi için Git [bir birim değiştirmek](storsimple-8000-manage-volumes-u2.md#modify-a-volume).

* **Yerel bir birime geri yükleme sırasında bir yedekleme gerçekleştirebilirsiniz** – yordamları için şu adrese gidin [yedekleme ilkelerini yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).

* **Bir geri yükleme işlemi iptal** – birim geri yükleme işlemi başlattı önce bu durumla durumu geri alınacak sonra geri yükleme işi iptal ederseniz. Yordamlar için Git [bir işi iptal](storsimple-8000-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Nasıl iş geri yükleme

Güncelleştirme 4 veya üstünü çalıştıran cihazlarda heatmap tabanlı geri yükleme uygulanır. Konak ister gibi cihaz erişim verilerini ulaşmak, bu istekleri izlenir ve bir heatmap oluşturulur. Alt istek hızı alt ısı ile öbekleri çevirir ancak yüksek istek hızı veri öbekleri yüksek ısı ile sonuçlanır. İki kez olarak işaretlenmesi için verilerin en az erişmesi gereken _etkin_. Değiştirilen bir dosya da olarak işaretlenmiş _etkin_. Geri yüklemeyi başlatmak sonra veri öngörülü hidrasyonu heatmap göre gerçekleşir. Sürümleri için yalnızca erişimini kullanarak geri yükleme sırasında veri güncelleştirme 4'ten önceki indirilir.

Aşağıdaki uyarılar heatmap tabanlı geri yüklemeler için geçerlidir:

* Heatmap izleme yalnızca katmanlı birimler için etkindir ve yerel olarak sabitlenmiş birimlerin desteklenmez.

* Başka bir aygıt bir birime kopyalarken Heatmap tabanlı geri yükleme desteklenmez. 

* (Veri zaten yerel olarak kullanılabilir olduğu gibi) bir yerinde geri yükleme ve cihazda yerel bir anlık görüntü geri yüklenmesi birimin var, ardından biz rehydrate değil. 

* Geri yüklendiğinde varsayılan olarak, önceden üzerinde heatmap göre verileri rehydrate rehydration işleri başlatılan. 

Güncelleştirme 4'te çalışan rehydration işleri sorgulamak, rehydration işini iptal et ve rehydration işinin durumunu almak için Windows PowerShell cmdlet'leri kullanılabilir.

* `Get-HcsRehydrationJob`-Bu cmdlet rehydration işinin durumunu alır. Tek bir birim için bir tek rehydration işi tetiklenir.

* `Set-HcsRehydrationJob`-Bu cmdlet, duraklatmak, durdurmak, rehydration devam ederken rehydration işi sürdürme olanak sağlar.

Rehydration cmdlet'leri hakkında daha fazla bilgi için Git [StorSimple için Windows PowerShell cmdlet başvurusu](https://technet.microsoft.com/library/dn688168.aspx).

Otomatik rehdyration ile genellikle yüksek geçici okuma performansı beklenir. Geliştirme gerçek magniutde erişim düzeni, veri dalgalanmasına ve veri türü gibi çeşitli etkenlere bağlıdır. 

Rehydration işini iptal etmek için PowerShell cmdlet'ini kullanabilirsiniz. Rehydration işleri tüm gelecekteki geri yükleme, kalıcı olarak devre dışı bırakmak isterseniz, [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğunu kullanma

**Yedekleme kataloğu** dikey sağlar yedekleme daraltmaya yardımcı olacak sorgu seçimi ayarlayın. Aşağıdaki parametreleri temel alınarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Zaman aralığı** – yedekleme kümesi oluşturduğunuzda tarih ve saat aralığı.
* **Aygıt** – yedekleme kümesi oluşturulduğu aygıt.
* **Yedekleme İlkesi** veya **birim** – yedekleme İlkesi veya bu yedekleme kümesiyle ilişkili birim.

Filtrelenmiş yedekleme kümelerini, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:

* **Ad** – yedekleme İlkesi veya yedekleme kümesiyle ilişkili birim adı.
* **Tür** – yedekleme kümelerini yerel anlık görüntüleri olması veya Bulut anlık görüntüler. Bir bulut anlık görüntüsü bulutta bulunan birim verilerin yedeğini başvuruyor ancak yedekleme verilerinizin cihazda yerel olarak depolanan tüm birim yerel bir anlık görüntüdür. Bulut anlık görüntüleri veri dayanıklılığı için seçilen ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturulan** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Birimleri** -yedekleme kümesiyle ilişkili birimlerin sayısı.
* **Başlatılan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. (Bir yedekleme İlkesi yedeklemeleri zamanlamak için kullanabilirsiniz. Alternatif olarak, kullanabileceğiniz **yedek alın** etkileşimli veya isteğe bağlı bir yedek almak için seçeneği.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple birim bir yedekten geri yükleme

Kullanabileceğiniz **yedekleme kataloğu** dikey StorSimple biriminiz belirli bir yedekten geri yükleyin. Ancak unutmayın, bu bir birime geri birimin yedeğin alındığı duruma döner. Yedekleme işlemi sonra eklenen tüm veriler kaybolacak.

> [!WARNING]
> Bir yedekten geri yükleme, var olan birimler yedekten yerini alır. Bu, yedeğin alındığı sonra yazıldı herhangi bir veri kaybına neden olabilir.


### <a name="to-restore-your-volume"></a>Biriminiz geri yüklemek için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **yedekleme kataloğu**.

2. Bir yedekleme kümesi aşağıdaki gibi seçin:
   
   1. Zaman aralığı belirtin.
   2. Uygun cihazı seçin.
   3. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   4. Tıklatın **Uygula** bu sorguyu yürütmek için.

    Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
   
    ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Geri yüklemeden önce bu birimleri ana bilgisayar ve aygıt çevrimdışına alınması gerekir. Birimleri erişimi **birimleri** Cihazınızı dikey ve adımları izleyin [bir birim çevrimdışına](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) çevrimdışı duruma getirmek için.
   
   > [!IMPORTANT]
   > Çevrimdışı birimler cihazda olabilmesi birimlerde çevrimdışı konak ilk olarak, gerçekleştirdiğinizden emin olun. Konakta birimler çevrimdışı almazsanız, olası veri bozulmasına yol açabilir.
   
4. Geri gidin **yedekleme kataloğu** sekmesinde ve bir yedekleme kümesi seçin. Sağ tıklayın ve ardından bağlam menüsünden seçin **geri**.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Onayınız istenir. Geri yükleme bilgilerini gözden geçirin ve ardından onay onay kutusunu seçin.
   
    ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7.  Tıklatın **geri**. Bu erişerek görüntüleyebileceğiniz bir geri yükleme işi başlattığı **işleri** sayfası.

    ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Geri yükleme tamamlandıktan sonra birimlerinizi içeriğini yedekleme birimlerden değiştirilir olduğunu doğrulayın.


## <a name="if-the-restore-fails"></a>Geri yükleme başarısız olursa

Geri yükleme işlemi için herhangi bir nedenle başarısız olursa bir uyarı alırsınız. Bu meydana gelirse, yedekleme hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Ardından yedeğinin geçerli olduğundan ve buluttan geri yüklüyorsanız, bağlantı sorunları soruna neden.

Geri yükleme işlemini tamamlamak için ana bilgisayarda birimin çevrimdışına alın ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerini yapılan tüm değişiklikler kaybolacak unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [yönetmek StorSimple birimlerini](storsimple-8000-manage-volumes-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

