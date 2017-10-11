---
title: "StorSimple Snapshot Manager yedekleme kataloğu | Microsoft Docs"
description: "StorSimple Snapshot Manager MMC ek bileşenini görüntülemek ve yedekleme kataloğu yönetmek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: b97753e6f1b67e3c8d247281c5e5208033a56eca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Yedekleme kataloğu yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
Birincil işlev, StorSimple Snapshot Manager anlık görüntüleri biçiminde StorSimple birimlerini uygulama tutarlı yedek kopyalarını oluşturmanızı izin vermektir. Anlık görüntüler sonra adlı bir XML dosyasında listelenen bir *yedekleme kataloğu*. Yedekleme kataloğu anlık görüntüler birim grubu ve ardından yerel anlık görüntü veya Bulut anlık görüntüsü göre düzenler.

Bu öğretici nasıl kullanabileceğinizi açıklar **yedekleme kataloğu** düğümünde aşağıdaki görevleri tamamlayın:

* Bir birime geri yükleme
* Bir birim veya birim grubu kopyalama
* Yedek Sil
* Bir dosya kurtarma
* Storsimple Snapshot Manager veritabanını geri yükle

Yedekleme kataloğu genişleterek görüntüleyebileceğiniz **yedekleme kataloğu** düğümünde **kapsam** bölmesinde ve birim grubu genişletme.

* Birim grubu adı tıklatırsanız **sonuçları** bölmesinde yerel anlık görüntüler ve bulut anlık görüntüleri birim grubu için kullanılabilir sayısını gösterir. 
* Tıklatırsanız **yerel anlık görüntü** veya **bulut anlık görüntüsü**, **sonuçları** bölmesinde her yedekleme anlık görüntüsünü hakkında aşağıdaki bilgileri gösterir (bağlı olarak,  **Görüntüleme** ayarları):
  
  * **Ad** – anlık görüntünün alındığı zaman.
  * **Tür** – bu yerel bir anlık görüntüsü veya bir bulut anlık görüntüsü olsun.
  * **Sahibi** – içerik sahibi. 
  * **Kullanılabilir** – anlık görüntüsü şu anda kullanılabilir durumda olup olmadığını. **Doğru** anlık görüntü kullanılabilir olduğunu ve geri yüklenebilir; gösterir. **False** anlık görüntü artık kullanılabilir olduğunu gösterir. 
  * **İçeri aktarılan** – yedekleme içeri aktarılmış olup olmadığını. **Doğru** cihaz StorSimple anlık görüntü Yöneticisi'nde; yapılandırıldığından zaman Yedekleme StorSimple Aygıt Yöneticisi'ni hizmetinden alınan gösterir **False** alınmadığından, ancak StorSimple Snapshot Manager tarafından oluşturulmuş olduğunu gösterir. (Bir sonek birim grubu içeri aktarılmış aygıtı tanımlayan eklendiği bir içe aktarılan birim grubu kolayca tanıyacak.)
    
    ![Yedekleme kataloğu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Genişletirseniz **yerel anlık görüntü** veya **bulut anlık görüntüsü**ve ardından bir tek tek anlık görüntü adı **sonuçları** bölmesinde anlık görüntü hakkında aşağıdaki bilgileri gösterir Seçtiğiniz:
  
  * **Ad** – birimin sürücü harfiyle tanımlanır. 
  * **Yerel ad** – (varsa) sürücüsünün yerel adı. 
  * **Aygıt** – birimin bulunduğu aygıt adı. 
  * **Kullanılabilir** – anlık görüntüsü şu anda kullanılabilir durumda olup olmadığını. **Doğru** anlık görüntü kullanılabilir olduğunu ve geri yüklenebilir; gösterir. **False** anlık görüntü artık kullanılabilir olduğunu gösterir. 

## <a name="restore-a-volume"></a>Bir birime geri yükleme
Bir birim yedekten geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="prerequisites"></a>Ön koşullar
Zaten yapmadıysanız, bir birim ve birim grubu oluşturun ve sonra birimi silin. Varsayılan olarak, StorSimple Snapshot Manager silinecek sorgulamasına önce bir birim yedekler. Bu önlem, birim yanlışlıkla silinirse veya verileri herhangi bir nedenle kurtarılması gereken veri kaybı engelleyebilir. 

StorSimple Snapshot Manager bu önlem yedekleme oluşturduğu aşağıdaki iletisi görüntüler.

![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Bir birim grubunun parçası olan bir birimi silemezsiniz. Sil seçeneği kullanılamaz. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Bir birime geri yüklemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubunu genişletin ve ardından **yerel anlık görüntüler** veya **bulut anlık görüntüleri**. Yedekleme anlık görüntülerinin listesi görünür **sonuçları** bölmesi.
3. Geri yüklemek istediğiniz yedekleme Bul sağ tıklatın ve ardından **geri**.
   
    ![Yedekleme kataloğunu geri yükle](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Onay sayfasında ayrıntıları gözden türü **Onayla**ve ardından **Tamam**. StorSimple Snapshot Manager yedekleme birimi geri yüklemek için kullanır.
   
    ![Geri yükle onay iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Çalışırken geri yükleme eylemini izleyebilirsiniz. İçinde **kapsam** bölmesini genişletin **işleri** düğümünü ve ardından **çalıştıran**. İş ayrıntılarını görünür **sonuçları** bölmesi. Geri yükleme işi tamamlandığında, iş ayrıntılarını aktarılır **son 24 saat** listesi.

## <a name="clone-a-volume-or-volume-group"></a>Bir birim veya birim grubu kopyalama
Bir kopyasını (kopya) bir birim veya birim grubu oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-clone-a-volume-or-volume-group"></a>Bir birim veya birim grubu kopyalamak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubunu genişletin ve ardından **bulut anlık görüntüleri**. Yedeklemeleri listesi görünür **sonuçları** bölmesi.
3. Birim veya kopyalama, birim veya birim grubu adını sağ tıklatın ve istediğiniz birim grubu bulma **kopya**. **Kopyalama bulut anlık görüntü** iletişim kutusu görüntülenir.
   
    ![Bir bulut anlık görüntüsü kopyalayın](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Tamamlamak **kopyalama bulut anlık görüntü** aşağıdaki gibi iletişim kutusunda: 
   
   1. İçinde **adı** metin kutusuna, kopyalanan birim için bir ad yazın. Bu ad görünür **birimleri** düğümü. 
   2. (İsteğe bağlı) seçin **sürücü**ve ardından açılır listeden bir sürücü harfi seçin.
   3. (İsteğe bağlı) seçin **klasörü (NTFS)**, bir klasör yolunu yazın veya Gözat'ı tıklatın ve klasörü için bir konum seçin. 
   4. **Oluştur**'a tıklayın.
5. Kopyalama işlemi tamamlandığında, kopyalanan birim başlatması gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı yönergeler için bkz: [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Başlatıldıktan sonra birimi listelenir **birimleri** düğümünde **kapsam** bölmesi. Listelenen birimleri görmüyorsanız birimlerin listesini yenilemek (sağ **birimleri** düğümünü ve ardından **yenileme**).

## <a name="delete-a-backup"></a>Yedek Sil
Yedekleme Kataloğu'ndan bir anlık görüntüyü silmek için aşağıdaki yordamı kullanın. 

> [!NOTE]
> Bir anlık görüntüyü sildiğinizde anlık görüntü ilişkili yedeklenmiş verileri siler. Ancak, bulut verilerini temizleme işlemi biraz zaman alabilir.<br>


#### <a name="to-delete-a-backup"></a>Bir yedekleme silmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubunu genişletin ve ardından **yerel anlık görüntüler** veya **bulut anlık görüntüleri**. Anlık görüntü listesi görünür **sonuçları** bölmesi.
3. Silin ve ardından anlık görüntüsünü sağ **silmek**.
4. Onay iletisi göründüğünde tıklayın **Tamam**.

## <a name="recover-a-file"></a>Bir dosya kurtarma
Bir dosyayı bir birimden yanlışlıkla silinirse silme önceden tarihleri bir anlık görüntü alma, birimin bir kopyasını oluşturmak için anlık görüntü kullanılarak ve sonra dosyayı özgün birimin kopyalanan birimden kopyalamayı tarafından dosya kurtarabilirsiniz.

#### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce geçerli bir birim grubu yedeği olduğundan emin olun. Ardından, bu birim grubundaki birimlerden biri üzerinde depolanan bir dosya silin. Son olarak, silinen dosyanın yedekten geri yüklemek için aşağıdaki adımları kullanın. 

#### <a name="to-recover-a-deleted-file"></a>Silinmiş bir dosyayı kurtarmak için
1. Masaüstünüzde StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager konsol penceresi görünür. 
2. İçinde **kapsam** bölmesinde genişletin **yedekleme kataloğu** düğümü ve Silinen dosyasını içeren bir anlık görüntü göz atın. Genellikle, yalnızca silme işleminden önce oluşturulmuş bir anlık görüntü seçmelisiniz.
3. Kopyalamak istediğiniz birimi bulamıyor sağ tıklayın ve ardından tıklatın **kopya**. **Kopyalama bulut anlık görüntü** iletişim kutusu görüntülenir.
   
    ![Bir bulut anlık görüntüsü kopyalayın](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Tamamlamak **kopyalama bulut anlık görüntü** aşağıdaki gibi iletişim kutusunda: 
   
   1. İçinde **adı** metin kutusuna, kopyalanan birim için bir ad yazın. Bu ad görünür **birimleri** düğümü. 
   2. (İsteğe bağlı) Seçin **sürücü**ve ardından açılır listeden bir sürücü harfi seçin. 
   3. (İsteğe bağlı) Seçin **klasörü (NTFS)**ve bir klasör yolunu yazın veya **Gözat** ve klasörü için bir konum seçin. 
   4. **Oluştur**'a tıklayın. 
5. Kopyalama işlemi tamamlandığında, kopyalanan birim başlatması gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı yönergeler için bkz: [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Başlatıldıktan sonra birimi listelenir **birimleri** düğümünde **kapsam** bölmesi. 
   
    Listelenen birimleri görmüyorsanız birimlerin listesini yenilemek (sağ **birimleri** düğümünü ve ardından **yenileme**).
6. Kopyalanan birimi içeren NTFS klasörünü açın, **birimleri** düğümü ve kopyalanan birim açın. Kurtarmak istediğiniz dosyayı bulun ve birincil birimin kopyalayın.
7. Dosyayı geri yükledikten sonra kopyalanan birimi içeren NTFS klasörüne silebilirsiniz.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple Snapshot Manager veritabanını geri yükle
Ana bilgisayara StorSimple Snapshot Manager veritabanını düzenli olarak yedeklemeniz gerekir. Bir olağanüstü durum oluştuğunda veya ana bilgisayar için herhangi bir nedenle başarısız olursa, bunu daha sonra yedekten geri yükleyebilirsiniz. Veritabanı yedeklemesi oluşturma elle yapılan bir işlemdir.

#### <a name="to-back-up-and-restore-the-database"></a>Yedeklemek ve veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   3. Üzerinde **Hizmetleri** penceresinde, seçin **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmetini durdurun**.
2. Ana bilgisayara C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını bulun, dosyasını kopyalayın ve güvenli bir konumda veya bulutta kopyayı depolar. Ana bilgisayar başarısız olursa, StorSimple Snapshot Manager'da oluşturulan yedekleme ilkelerini kurtarmanıza yardımcı olması için bu yedekleme dosyasını kullanabilirsiniz.
   
    ![Azure StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   2. Üzerinde **Hizmetleri** penceresinde, seçin **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmeti yeniden**.
5. Ana bilgisayara C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
6. Katalog XML dosyasını silin ve oluşturduğunuz yedek sürümle değiştirin. 
7. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü StorSimple Snapshot Manager simgesine tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple çözümünüzün yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanarak](storsimple-snapshot-manager-admin.md).
* Daha fazla bilgi edinmek [StorSimple Snapshot Manager görevleri ve iş akışları](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

