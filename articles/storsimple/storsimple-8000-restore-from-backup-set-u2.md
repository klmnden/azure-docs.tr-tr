---
title: Bir birim bir StorSimple 8000 serisi bir yedekleme geri yükleme | Microsoft Docs
description: Bir yedekleme kümesinden StorSimple birimini geri yüklemek için StorSimple cihaz Yöneticisi hizmeti yedekleme kataloğunu kullanmayı açıklar.
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 6a2e022697ced90d968075b7a4abe4163be7a539
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723399"
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Bir yedekleme kümesinden StorSimple birimini geri yükleme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, mevcut bir yedekleme kümesi kullanarak StorSimple 8000 serisi cihaz üzerinde gerçekleştirilen geri yükleme işlemi açıklanmaktadır. Kullanım **yedekleme kataloğu** geri yüklemek bir birimden yerel veya Bulut yedekleme dikey penceresinde. **Yedekleme kataloğu** dikey penceresinde el ile veya otomatik yedeklemeler alındığında, oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme kümesinden geri yükleme işlemi hemen veri arka planda yüklenirken birimi çevrimiçi duruma getirir.

Gitmek için geri yüklemeyi başlatmak için alternatif bir yöntem olan **cihazlar > [Cihazınızı] > birimleri**. İçinde **birimleri** dikey penceresinde bir birimi seçin, sağ tıklayın bağlam menüsünü açmak ve ardından **geri**.

## <a name="before-you-restore"></a>Geri yüklemeden önce

Bir geri yükleme başlamadan önce aşağıdakilere dikkat ederek gözden geçirin:

* **Birimi çevrimdışı duruma** – birimi hem konak hem de cihaz çevrimdışı geri yükleme işlemini başlatmadan önce yararlanın. Geri yükleme işlemi birimi çevrimiçi cihazda otomatik olarak getirir. ancak, el ile cihaz çevrimiçi konakta getirmeniz gerekir. Cihazda birim çevrimiçidir hemen sonra birimi çevrimiçi konakta getirebilirsiniz. (Geri yükleme işlemi tamamlanana kadar beklemenize gerek yok.) Yordamlar için Git [bir birimi çevrimdışı duruma](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline).

* **Birim türü geri yükleme sonrasında** : silinen birimler geri yüklenir anlık görüntüdeki türüne göre; diğer bir deyişle, yerel olarak sabitlenmiş birimler yerel olarak sabitlenmiş birimler geri yüklenir ve katmanlı birimler olarak katmanlı birimler geri yüklenir.

    Var olan birimler için geçerli kullanım türü birimin anlık görüntüde depolanan tür geçersiz kılar. Örneğin, bir birim birim türünü katmanlı ve birim türü artık yerel olarak (gerçekleştirilen bir dönüştürme işlemi nedeniyle) sabitlenir alınmış bir anlık görüntüden geri yüklerseniz, birimin yerel olarak sabitlenmiş bir birim kurulacaktır. Benzer şekilde, var olan bir yerel olarak sabitlenmiş birim genişletilmiş ve daha sonra birim daha küçük gerçekleştirilen eski bir anlık görüntüden geri, geri yüklenen birimin geçerli genişletilmiş boyutu korur.

    Toplu geri yüklenirken bir birim için yerel olarak sabitlenmiş bir birim katmanlı birim veya yerel olarak sabitlenmiş bir birim için katmanlı birim dönüştürülemiyor. Geri yükleme işlemi tamamlandı ve ardından birimin başka bir türüne dönüştürebilir kadar bekleyin. Bir birim dönüştürme hakkında daha fazla bilgi için Git [birim türünü değiştirmek](storsimple-8000-manage-volumes-u2.md#change-the-volume-type). 

* **Birim boyutu geri yüklenen birimin yansıtılır** – (yerel olarak sabitlenmiş birimler tam olarak sağlandığından dolayı), silinmiş olan bir yerel olarak sabitlenmiş birim geri yüklüyorsanız bu önemli bir noktadır. Daha önce silinmiş olan bir yerel olarak sabitlenmiş birim geri yükleme girişiminde bulunmadan önce yeterli alana sahip olduğunuzdan emin olun.

* **Bu geri yüklenirken bir birim genişletilemiyor** – Birim genişletme girişiminde bulunmadan önce geri yükleme işlemi tamamlanana kadar bekleyin. Bir birim genişletme hakkında daha fazla bilgi için Git [bir birimi değiştirme](storsimple-8000-manage-volumes-u2.md#modify-a-volume).

* **Yerel bir birime geri yükleme sırasında bir yedek gerçekleştirebileceğiniz** – yordamları için şu adrese gidin [yedekleme ilkelerini yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md).

* **Geri yükleme işlemi iptal edebilirsiniz** : birimi geri yükleme işlemini başlattığınız önce bunu durumu geri alınacak sonra geri yükleme işi iptal ederseniz. Yordamlar için Git [bir işi iptal](storsimple-8000-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Nasıl iş geri yükleme

Güncelleştirme 4 veya üstünü çalıştıran cihazlarda bir ısı Haritası tabanlı geri yükleme uygulanır. Ana bilgisayar ister gibi cihaz verilere erişmek, bu istekleri izlenir ve yoğunluk Haritası yoluyla oluşturulur. Alt istek hızı düşük ısı ile öbeklere çevirir ise yüksek istek hızı veri öbekleri daha yüksek ısı ile sonuçlanır. Olarak işaretlenmiş olması en az iki kere veri erişim _sık erişimli_. Değiştirilen bir dosya da olarak işaretlenmiş _sık erişimli_. Geri yüklemeyi başlatmak sonra veri proaktif hidrasyonu ısı Haritası göre gerçekleşir. Sürümleri için veri erişimini yalnızca geri yükleme işlemi sırasında güncelleştirme 4'den önceki indirilir.

Aşağıdaki uyarılar ısı Haritası tabanlı geri yüklemeler için geçerlidir:

* Isı Haritası izleme yalnızca katmanlı birimleri için etkindir ve yerel olarak sabitlenmiş birimler desteklenmez.

* Isı Haritası tabanlı geri yükleme, bir birim başka bir cihaz için kopyalama desteklenmiyor. 

* (Veri zaten yerel olarak kullanılabilir olduğu gibi) varsa bir yerinde geri yükle ve yerel anlık görüntü geri yüklenmesi birimi için cihazda mevcut ardından biz yeniden doldurma değil. 

* Geri yüklediğinizde, varsayılan olarak, yeniden doldurma işleri, ısı Haritası üzerinde göre verileri proaktif bir şekilde yeniden doldurma başlatılır. 

Güncelleştirme 4'te çalışan yeniden doldurma işleri sorgulamak, yeniden doldurma işi iptal ve yeniden doldurma işi durumunu almak için Windows PowerShell cmdlet'leri kullanılabilir.

* `Get-HcsRehydrationJob` -Bu cmdlet, yeniden doldurma işinin durumunu alır. Tek bir birim için bir tek yeniden doldurma işi tetiklenir.

* `Set-HcsRehydrationJob` -Bu cmdlet, duraklatmak, durdurmak, yeniden doldurma işlemi sürdüğünden yeniden doldurma işlemi sürdürme olanak tanır.

Yeniden doldurma cmdlet'leri hakkında daha fazla bilgi için Git [StorSimple için Windows PowerShell cmdlet başvurusu](https://technet.microsoft.com/library/dn688168.aspx).

Otomatik yeniden doldurma ile genellikle yüksek geçici okuma performansı bekleniyor. Gerçek geliştirmeler büyüklüğünü erişim düzeni, veri değişim sıklığı ve veri türü gibi çeşitli faktörlere bağlıdır. 

Yeniden doldurma işi iptal etmek için PowerShell cmdlet'ini kullanabilirsiniz. Tüm gelecek geri yükler, işleri yeniden doldurma kalıcı olarak devre dışı bırakmak isterseniz [Microsoft Support başvurun](storsimple-8000-contact-microsoft-support.md).

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğunu kullanma

**Yedekleme kataloğunu** dikey pencere, yedekleme daraltmak için yardımcı olan bir sorgu kümesi seçimi sağlar. Aşağıdaki parametreleri temel alarak alınır yedekleme kümelerini filtreleyebilirsiniz:

* **Zaman aralığı** – yedekleme kümesi oluşturulduğunda tarih ve saat aralığı.
* **Cihaz** – cihaz yedekleme kümesi oluşturuldu.
* **Yedekleme İlkesi** veya **birim** – Bu yedekleme kümesiyle ilişkili birim ve yedekleme ilkesi.

Filtrelenmiş yedek kümelerini, ardından aşağıdaki özniteliklere bağlı tabloda verilmiştir:

* **Adı** – yedekleme kümesiyle ilişkili birim ve yedekleme ilkesi adı.
* **Tür** – kümeleri yedekleme yerel anlık görüntüler olması veya Bulut anlık görüntüleri. Birim verilerinin bulutta bulunan bir bulut anlık görüntüsünü başvuruyor ancak bir yedeğini cihaz üzerinde yerel olarak depolanan tüm birim verilerinizi yerel anlık görüntüsüdür. Veri dayanıklılığı için bulut anlık görüntüleri seçilir ancak yerel anlık görüntüleri daha hızlı erişim sağlar.
* **Boyutu** – yedekleme kümesi gerçek boyutu.
* **Oluşturulma tarihi** – yedeklemelerin ne zaman oluşturulduğu tarih ve saat. 
* **Birimleri** -yedekleme kümesiyle ilişkili birim sayısı.
* **Başlatılan** – yedeklemeleri otomatik olarak bir zamanlamaya göre veya el ile bir kullanıcı tarafından başlatılabilir. (Bir yedekleme İlkesi yedeklemeleri zamanlamak için kullanabilirsiniz. Alternatif olarak, **yedek Al** seçeneği etkileşimli veya isteğe bağlı bir yedekleme yapın.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple toplu bir yedekten geri yükleme

Kullanabileceğiniz **yedekleme kataloğunu** StorSimple toplu belirli bir yedekten geri yükleme dikey penceresi. Ancak unutmayın, bir birim, geri yükleme, birimin alındığı duruma geri döner. Sonra yedekleme işlemi eklenmiş olan tüm veriler kaybolur.

> [!WARNING]
> Bir yedekten geri yükleme yedeği var olan birimler yerini alır. Bu, yedekleme alındıktan sonra yazılan veri kaybına neden olabilir.


### <a name="to-restore-your-volume"></a>Toplu geri yüklemek için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **yedekleme kataloğu**.

2. Bir yedekleme kümesi şu şekilde seçin:
   
   1. Zaman aralığını belirtin.
   2. Uygun bir cihaz seçin.
   3. Aşağı açılan listeden, birim veya yedekleme ilkesi seçmek için istediğiniz yedekleme için seçin.
   4. Tıklayın **Uygula** bu sorguyu yürütmek için.

      Yedeklemeleri seçili birimle ilişkilendirilen veya yedekleme İlkesi yedekleme ayarları listesinde görüntülenmelidir.
   
      ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Geri yüklemeden önce bu birimleri ana bilgisayar ve cihaz çevrimdışına alınması gerekir. Birimleri erişim **birimleri** dikey penceresinde, cihazınızın ve sonra adımları [bir birimi çevrimdışı duruma](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) çevrimdışı duruma getirmek için.
   
   > [!IMPORTANT]
   > Cihazda birimleri çevrimdışı duruma önce birimleri konakta çevrimdışı ilk olarak, gerçekleştirdiğinizden emin olun. Konakta birimleri çevrimdışı duruma değil, olası veri bozulmasına neden.
   
4. Geri gidin **yedekleme kataloğunu** sekme ve bir yedek kümesi seçin. Sağ tıklayın ve ardından bağlam menüsünden **geri**.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Onaylamanız istenir. Geri yükleme bilgileri gözden geçirin ve ardından onay onay kutusunu seçin.
   
    ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7. Tıklayın **geri**. Bu erişerek görüntüleyebileceğiniz bir geri yükleme işi başlatan **işleri** sayfası.

   ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Geri yükleme tamamlandıktan sonra birimlerinizi içeriğini yedekleme birimlerden tarafından değiştirilmiş olduğunu doğrulayın.


## <a name="if-the-restore-fails"></a>Geri yükleme başarısız olursa

Geri yükleme işlemi için herhangi bir nedenle başarısız olursa bir uyarı alırsınız. Bu meydana gelirse, yedeğin hala geçerli olduğunu doğrulamak için yedek listesini yenileyin. Ardından yedeğinin geçerli olduğundan ve buluttan geri yüklediğiniz, bağlantı sorunlarını soruna neden.

Geri yükleme işlemini tamamlamak için konakta birimi çevrimdışı olması ve geri yükleme işlemini yeniden deneyin. Herhangi bir değişiklik geri yükleme işlemi sırasında gerçekleştirilen toplu veri kaybı olmayacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [yönetme StorSimple birimlerini](storsimple-8000-manage-volumes-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

