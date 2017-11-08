---
title: Bir StorSimple biriminin yedekten geri | Microsoft Docs
description: "StorSimple Yöneticisi hizmet yedekleme kataloğu sayfasında yedekleme kümesinden StorSimple birimini geri için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 76fa3dd8fa2f9775dc166686e699a8dd66399aff
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Bir StorSimple biriminin bir yedekleme kümesi (güncelleştirme 2) geri yükleme
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [bir yedekleme kümesi (güncelleştirme 2) bir StorSimple birimini geri](storsimple-8000-restore-from-backup-set-u2.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Genel Bakış
**Yedekleme kataloğu** sayfası el ile veya otomatik yedeklemeler durumdayken, oluşturulan tüm yedekleme kümelerini görüntüler. Liste ve yedeklemeleri yönetmeniz, yedekleme kümesinden geri yüklemek veya bir birim kopyalamak için bu sayfayı kullanın.

 ![Yedekleme kataloğu sayfası](./media/storsimple-restore-from-backup-set-u2/restore.png)

Bu öğretici nasıl kullanılacağı açıklanmaktadır **yedekleme kataloğu** Cihazınızı yedekleme kümesinden geri yüklemek için sayfa.

Yerel bir birimden veya Bulut yedeklemesini geri yükleyebilirsiniz. Hemen veri arka planda yüklenirken her iki durumda da geri yükleme işlemi birimi çevrimiçi duruma getirir. 

## <a name="before-you-restore"></a>Geri yüklemeden önce
Bir geri yükleme işlemini başlatmadan önce aşağıdaki uyarılarla bilmeniz gerekir:

* **Birim çevrimdışına** – geri yükleme işlemini başlatmadan önce birim hem konak hem de cihaz çevrimdışı gerçekleştirin. Geri yükleme işlemi, birimi çevrimiçi otomatik olarak cihazda duruma getirir. rağmen ana bilgisayarda el ile cihaz çevrimiçi getirmeniz gerekir. Birim cihazda çevrimiçi olduğunda ana bilgisayarda birimin çevrimiçi kullanıma sunabilirsiniz. (Geri yükleme işlemi tamamlanana kadar bekleyin gerek yok.) Yordamlar için Git [bir birim çevrimdışına](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Birim türü geri yükleme sonrasında** – silinen birimler geri yüklenir anlık görüntüdeki türüne göre. Yerel olarak sabitlenmiş birimleri yerel olarak sabitlenmiş birimleri olarak geri yüklenir ve katmanlı birimleri katmanlı birimler olarak geri yüklenir.
  
    Var olan birimler için geçerli kullanım türü biriminin anlık depolanan türü geçersiz kılar. Örneğin, bir birim birim türü katmanlı ve birim türü artık yerel olarak (dönüştürme işlemi nedeniyle) sabitlenmiş alınan anlık görüntü geri, birimin yerel olarak sabitlenmiş bir birim geri yüklenir. Benzer şekilde, var olan yerel olarak sabitlenmiş bir birim genişletilmiş ve daha sonra birim daha küçük olduğunda gerçekleştirilecek daha eski bir anlık görüntüden geri, geri yüklenen toplu geçerli genişletilmiş boyutu korur.
  
    Katmanlı bir birimden yerel olarak sabitlenmiş bir birim için bir birim dönüştürülemiyor veya _tam tersini_ birim geri yüklenirken. Geri yükleme işlemi tamamlandıktan ve daha sonra başka bir türüne birim dönüştürebilirsiniz kadar bekleyin. Bir birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirme](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Birim boyutu geri yüklenen birimde yansıtılır** – (yerel olarak sabitlenmiş birimlerin tam olarak sağlandıktan nedeniyle), silinmiş olan yerel olarak sabitlenmiş bir birim geri yüklüyorsanız bu önemli bir konudur. Daha önce silinmiş yerel olarak sabitlenmiş bir birim geri yüklemeyi denemeden önce yeterli alan olduğundan emin olun. 
* **Bu geri yüklenirken bir birim genişletilemiyor** – birimini genişletin girişiminde bulunmadan önce geri yükleme işlemi tamamlanana kadar bekleyin. Bir birim genişletme hakkında daha fazla bilgi için Git [bir birim değiştirmek](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Yerel bir birime geri yüklediğiniz sırada bir yedekleme gerçekleştirebilirsiniz** – yordamları için şu adrese gidin [yedekleme ilkelerini yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manage-backup-policies.md).
* **Bir geri yükleme işlemi iptal** – içindeydi durumuna geri yükleme başlatılan önce birim geri sonra geri yükleme işi iptal ederseniz. Yordamlar için Git [bir işi iptal](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Nasıl iş geri yükleme
Güncelleştirme 4 veya üstünü çalıştıran cihazlarda heatmap tabanlı geri yükleme uygulanır. Konak ister gibi cihaz erişim verilerini ulaşmak, bu istekleri izlenir ve bir heatmap oluşturulur. Alt istek hızı alt ısı ile öbekleri çevirir ancak yüksek istek hızı veri öbekleri yüksek ısı ile sonuçlanır. İki kez olarak işaretlenmesi için verilerin en az erişmesi gereken _etkin_. Değiştirilen bir dosya da olarak işaretlenmiş _etkin_. Geri yüklemeyi başlatmak sonra veri öngörülü hidrasyonu heatmap göre gerçekleşir. Sürümleri için yalnızca erişimini kullanarak geri yükleme sırasında veri güncelleştirme 4'ten önceki yüklendi. 

Katmanlı birimlerin ve yerel olarak sabitlenmiş birimleri desteklenmez yalnızca Heatmap tabanlı izleme etkinleştirilir. Heatmap tabanlı geri yükleme de başka bir aygıt için bir birim kopyalama desteklenmez. (Veri zaten yerel olarak kullanılabilir olduğu gibi) bir yerinde geri yükleme ve cihazda yerel bir anlık görüntü geri yüklenmesi birimin var, ardından biz rehydrate değil. Geri yüklendiğinde varsayılan olarak, önceden üzerinde heatmap göre verileri rehydrate rehydration işleri başlatılan. Güncelleştirme 4'te çalışan rehydration işleri sorgulamak, rehydration işini iptal et ve rehydration işinin durumunu almak için Windows PowerShell cmdlet'leri kullanılabilir.

* `Get-HcsRehydrationJob`-Bu cmdlet rehydration işinin durumunu alır. Tek bir birim için bir tek rehydration işi tetiklenir.
* `Set-HcsRehydrationJob`-Bu cmdlet, duraklatmak, durdurmak, rehydration devam ederken rehydration işi sürdürme olanak sağlar. 

Rehydration cmdlet'leri hakkında daha fazla bilgi için Git [StorSimple için Windows PowerShell cmdlet başvurusu](https://technet.microsoft.com/library/dn688168.aspx).

Otomatik rehdyration ile genellikle yüksek geçici okuma performansı beklenir. Geliştirme gerçek magniutde erişim düzeni, veri dalgalanmasına ve veri türü gibi çeşitli etkenlere bağlıdır. Rehydration işini iptal etmek için PowerShell cmdlet'ini kullanabilirsiniz. Kalıcı olarak rehydration işler gelecekteki tüm geri yüklemeler için devre dışı bırakmak isterseniz, Microsoft Support başvurun.

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğunu kullanma
**Yedekleme kataloğu** sayfası, yedekleme daraltmaya yardımcı olacak bir sorgu olarak seçimi sağlar. Aşağıdaki parametreleri temel alınarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Aygıt** – yedekleme kümesi oluşturulduğu aygıt.
* **Yedekleme İlkesi** veya **birim** – yedekleme İlkesi veya bu yedekleme kümesiyle ilişkili birim.
* **Gelen** ve **için** – yedekleme kümesi oluşturduğunuzda tarih ve saat aralığı.

Filtrelenmiş yedekleme kümelerini, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:

* **Ad** – yedekleme İlkesi veya yedekleme kümesiyle ilişkili birim adı.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturulan** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Tür** – yedekleme kümelerini yerel anlık görüntüleri olması veya Bulut anlık görüntüler. Bir yedekleme verilerinizin cihazda yerel olarak depolanan tüm birim yerel bir anlık görüntüdür. Bir bulut anlık görüntüsü bulutta bulunan birim verilerin yedeğini ifade eder. Bulut anlık görüntüleri veri dayanıklılığı için seçilen ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Tarafından başlatılan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya sizin tarafınızdan el ile başlatılabilir. (Bir yedekleme İlkesi yedeklemeleri zamanlamak için kullanabilirsiniz. Alternatif olarak, kullanabileceğiniz **yedek alın** etkileşimli bir yedek almak için seçeneği.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple birim bir yedekten geri yükleme
Kullanabileceğiniz **yedekleme kataloğu** sayfasında StorSimple biriminiz belirli bir yedekten geri yükleyin. Ancak unutmayın, bu bir birime geri birimin yedeğin alındığı duruma geri döner. Yedekleme işlemi sonra eklenen tüm veriler kaybolur.

> [!WARNING]
> Bir yedekten geri yükleme, var olan birimler yedekten yerini alır. Bu, yedeğin alındığı sonra yazıldı herhangi bir veri kaybına neden olabilir.
> 
> 

### <a name="to-restore-your-volume"></a>Biriminiz geri yüklemek için
1. StorSimple Yöneticisi hizmet sayfasında, tıklatın **yedekleme kataloğu** sekmesi.
   
    ![Yedekleme kataloğu](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Bir yedekleme kümesi aşağıdaki gibi seçin:
   
   1. Uygun cihazı seçin.
   2. Aşağı açılan listesinde, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığı belirtin.
   4. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) Bu sorguyu yürütmek için.
      
      Yedekleme seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme kümelerini listesinde görüntülenmelidir.
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Geri yüklemeden önce bu birimleri ana bilgisayar ve aygıt çevrimdışına alınması gerekir. Birimleri erişimi **birim kapsayıcıları** sayfasında ve adımları izleyin [bir birim çevrimdışına](storsimple-manage-volumes-u2.md#take-a-volume-offline) çevrimdışı duruma getirmek için.
   
   > [!IMPORTANT]
   > Çevrimdışı birimler cihazda olabilmesi birimlerde çevrimdışı konak ilk olarak, gerçekleştirdiğinizden emin olun. Konakta birimler çevrimdışı almazsanız, olası veri bozulmasına yol açabilir.
   > 
   > 
4. Geri gidin **yedekleme kataloğu** sekmesinde ve bir yedekleme kümesi seçin.
5. Tıklatın **geri** sayfanın sonundaki.
6. Onayınız istenir. Geri yükleme bilgilerini gözden geçirin ve ardından onay onay kutusunu seçin.
   
    ![Onay sayfası](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Onay simgesine ![onay simgesi](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Bir geri yükleme işi başlar. İş erişerek görüntüleyebileceğiniz **işleri** sayfası. 
8. Geri yükleme tamamlandıktan sonra birimlerinizi içeriğini yedekleme birimlerden değiştirilir olduğunu doğrulayabilirsiniz.

![Video var](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video var**

Nasıl kopya kullanın ve geri yükleme silinmiş dosyaları kurtarmak için StorSimple özellikleri gösteren bir video izlemek için tıklatın [burada](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Geri yükleme başarısız olursa
Geri yükleme işlemi için herhangi bir nedenle başarısız olursa bir uyarı alırsınız. Bu meydana gelirse, yedekleme hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Ardından yedeğinin geçerli olduğundan ve buluttan geri yüklüyorsanız, bağlantı sorunları soruna neden. 

Geri yükleme işlemini tamamlamak için ana bilgisayarda birimin çevrimdışına alın ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerini yapılan tüm değişiklikler kaybolur.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [yönetmek StorSimple birimlerini](storsimple-manage-volumes-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

