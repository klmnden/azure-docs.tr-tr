---
title: StorSimple Snapshot Manager yedekleme kataloğu | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşenini görüntülemek ve yedekleme kataloğunu yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: dc24ebd59fd977ef35766c304aec5824e2c7bb4c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127199"
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Yedekleme kataloğunu yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager'ın birincil işlevi, anlık görüntüleri biçiminde StorSimple birimlerini uygulamayla tutarlı yedekleme kopyalarını oluşturmanızı izin vermektir. Anlık görüntüleri ardından adlı bir XML dosyasında listelenen bir *yedekleme kataloğunu*. Yedekleme kataloğunu anlık görüntüler birim grubu ve ardından yerel anlık görüntü veya Bulut anlık görüntüsü göre düzenler.

Bu öğreticide nasıl kullanabileceğiniz açıklanır **yedekleme kataloğu** düğümünde aşağıdaki görevleri tamamlamak için:

* Bir birime geri yükleme
* Bir birim veya birim grubu kopyalama
* Yedek Sil
* Dosyayı kurtarma
* Storsimple Snapshot Manager veritabanını geri yükleme

Yedekleme kataloğunu genişleterek görüntüleyebileceğiniz **yedekleme kataloğunu** düğümünde **kapsam** bölmesi ve ardından birim Grup genişletme.

* Birim grup adı tıklarsanız **sonuçları** bölmesinde yerel anlık görüntüler ve bulut anlık görüntüleri için birim grubu kullanılabilir sayısını gösterir. 
* Tıklarsanız **yerel anlık görüntü** veya **bulut anlık görüntüsü**, **sonuçları** bölmesinde her bir yedek anlık görüntüsü hakkında aşağıdaki bilgileri gösterir (bağlı olarak,  **Görüntüleme** ayarları):
  
  * **Adı** – anlık görüntünün alındığı zamanı.
  * **Tür** : Yerel anlık görüntü veya bir bulut anlık görüntüsünü budur.
  * **Sahibi** – içerik sahibi. 
  * **Kullanılabilir** : anlık görüntü şu anda kullanılabilir olup olmadığı. **Doğru** anlık görüntü kullanılabilir ve geri yüklenebilir; gösterir **False** anlık görüntü artık kullanılabilir olduğunu gösterir. 
  * **İçeri aktarılan** : yedeklemeyi içeri aktarıldı. **Doğru** yedekleme StorSimple cihaz Yöneticisi hizmetinden cihaz, StorSimple Snapshot Manager'da; yapılandırılmışsa çalışma zamanında alınan gösterir **False** bunu değil içeri aktarıldı ancak StorSimple Snapshot Manager tarafından oluşturulan gösterir. (İçinden birim grubu içeri aktarılan cihaz tanımlayan bir sonek eklendiğinde kolayca bir içeri aktarılan birim Grup belirleyebilirsiniz.)
    
    ![Yedekleme kataloğu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Genişletirseniz **yerel anlık görüntü** veya **bulut anlık görüntüsü**ve ardından bir tek bir anlık görüntü adına tıklayın **sonuçları** bölmesi anlık görüntü hakkında aşağıdaki bilgileri gösterir Seçtiğiniz:
  
  * **Adı** – birimin sürücü harfiyle tanımlanır. 
  * **Yerel ad** – (varsa) sürücüsünün yerel adı. 
  * **Cihaz** – birimin yer aldığı cihazın adı. 
  * **Kullanılabilir** : anlık görüntü şu anda kullanılabilir olup olmadığı. **Doğru** anlık görüntü kullanılabilir ve geri yüklenebilir; gösterir **False** anlık görüntü artık kullanılabilir olduğunu gösterir. 

## <a name="restore-a-volume"></a>Bir birime geri yükleme
Bir birim yedeklemeden geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="prerequisites"></a>Önkoşullar
Zaten yapmadıysanız, bir birim ve birim grubu oluşturun ve sonra birimi silin. StorSimple Snapshot Manager, varsayılan olarak, bir birimin silinmesini sorgulamasına önce yedekler. Bu önlem, birim yanlışlıkla silinirse veya veri herhangi bir nedenle kurtarılması gereken veri kayıplarını önleyebilirsiniz. 

StorSimple Snapshot Manager bu önlem yedekleme oluşturduğu aşağıdaki ileti görüntülenir.

![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Bir birim grubu parçası olan bir birim silinemiyor. Sil seçeneği kullanılamaz. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Bir birime geri yüklemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubu genişletin ve ardından **yerel anlık görüntüleri** veya **bulut anlık görüntüleri**. Yedekleme anlık görüntülerinin listesi görünür **sonuçları** bölmesi.
3. Geri yüklemek istediğiniz bir yedek bulmak sağ tıklatın ve ardından **geri**.
   
    ![Yedekleme kataloğunu geri yükle](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Onay sayfasında, ayrıntıları gözden geçirin türü **Onayla**ve ardından **Tamam**. StorSimple Snapshot Manager yedekleme birimi geri yüklemek için kullanır.
   
    ![Geri yükle onay iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Çalışırken geri yükleme eylemini izleyebilirsiniz. İçinde **kapsam** bölmesini genişletin **işleri** düğümünü ve ardından **çalıştıran**. İş ayrıntılarını görünür **sonuçları** bölmesi. Geri yükleme işi tamamlandığında, iş ayrıntılarını aktarılan **son 24 saat** listesi.

## <a name="clone-a-volume-or-volume-group"></a>Bir birim veya birim grubu kopyalama
Yinelenen (kopya) bir birim veya birim grubu oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-clone-a-volume-or-volume-group"></a>Bir birim veya birim grubu kopyalamak için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubu genişletin ve ardından **bulut anlık görüntüleri**. Yedekleme listesini görünür **sonuçları** bölmesi.
3. Birim veya birim grubu, kopyalama, birim veya birim grup adı sağ tıklayın ve istediğiniz bulma **kopya**. **Kopyalama bulut anlık görüntü** iletişim kutusu görüntülenir.
   
    ![Bir bulut anlık görüntüsü kopyalayın](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Tamamlamak **kopyalama bulut anlık görüntü** aşağıdaki gibi iletişim kutusunda: 
   
   1. İçinde **adı** metin kutusuna kopyalanan birim için bir ad yazın. Bu ad görünür **birimleri** düğümü. 
   2. (İsteğe bağlı) **sürücü**ve ardından bir sürücü harfi açılır listeden seçin.
   3. (İsteğe bağlı) **klasörü (NTFS)** , bir klasör yolunu yazın veya Gözat'a tıklayın ve klasör için bir konum seçin. 
   4. **Oluştur**’a tıklayın.
5. Kopyalama işlemi tamamlandıktan sonra kopyalanan birim başlatmanız gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı yönergeler için bkz. [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Başlatıldıktan sonra birimi altında listelenen **birimleri** düğümünde **kapsam** bölmesi. Listelenen birimleri görmüyorsanız, birimlerin listesini yenile (sağ **birimleri** düğümünü ve ardından **Yenile**).

## <a name="delete-a-backup"></a>Yedek Sil
Yedekleme Kataloğu'ndan bir anlık görüntüsünü silmek için aşağıdaki yordamı kullanın. 

> [!NOTE]
> Bir anlık görüntüsünü silme anlık görüntü ile ilişkili yedeklenen verileri siler. Ancak, buluttaki verileri temizleme işlemi biraz zaman alabilir.<br>


#### <a name="to-delete-a-backup"></a>Bir yedekleme silmek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü, bir birim grubu genişletin ve ardından **yerel anlık görüntüleri** veya **bulut anlık görüntüleri**. Anlık görüntülerin listesini görünür **sonuçları** bölmesi.
3. Silin ve ardından istediğiniz anlık görüntüsüne sağ tıklayın **Sil**.
4. Onay iletisi göründüğünde tıklayın **Tamam**.

## <a name="recover-a-file"></a>Dosyayı kurtarma
Bir dosya bir birimden yanlışlıkla silinirse, silme önceden tarihleri bir anlık görüntüsüne ulaşmanızın, birimin bir kopyasını oluşturmak için anlık görüntü kullanılarak ve sonra dosyanın özgün birimin kopyalanan birimden kopyalayarak dosyanın kurtarabilirsiniz.

#### <a name="prerequisites"></a>Önkoşullar
Başlamadan önce geçerli bir birim grubu yedeği olduğundan emin olun. Ardından, birim gruptaki birimlerden biri üzerinde depolanan bir dosyayı silin. Son olarak, silinen dosyanın yedekten geri yüklemek için aşağıdaki adımları kullanın. 

#### <a name="to-recover-a-deleted-file"></a>Silinen bir dosyayı kurtarmak için
1. Masaüstü StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager konsol penceresinde görünür. 
2. İçinde **kapsam** bölmesini genişletin **yedekleme kataloğu** düğümü ile silinen dosyanın bulunduğu bir anlık görüntüye göz atın. Genellikle, yalnızca silme işleminden önce oluşturulmuş bir anlık görüntü seçmelisiniz.
3. Kopyalamak istediğiniz bir birimi bulamıyor sağ tıklatın seçeneğine tıklayıp **kopya**. **Kopyalama bulut anlık görüntü** iletişim kutusu görüntülenir.
   
    ![Bir bulut anlık görüntüsü kopyalayın](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Tamamlamak **kopyalama bulut anlık görüntü** aşağıdaki gibi iletişim kutusunda: 
   
   1. İçinde **adı** metin kutusuna kopyalanan birim için bir ad yazın. Bu ad görünür **birimleri** düğümü. 
   2. (İsteğe bağlı) Seçin **sürücü**ve ardından bir sürücü harfi açılır listeden seçin. 
   3. (İsteğe bağlı) Seçin **klasörü (NTFS)** ve bir klasör yolunu yazın veya **Gözat** ve klasör için bir konum seçin. 
   4. **Oluştur**’a tıklayın. 
5. Kopyalama işlemi tamamlandıktan sonra kopyalanan birim başlatmanız gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı yönergeler için bkz. [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Başlatıldıktan sonra birimi altında listelenen **birimleri** düğümünde **kapsam** bölmesi. 
   
    Listelenen birimleri görmüyorsanız, birimlerin listesini yenile (sağ **birimleri** düğümünü ve ardından **Yenile**).
6. Kopyalanan birim içeren NTFS klasörünü açın, **birimleri** düğüm ve kopyalanan birim açın. Kurtarmak istediğiniz dosyayı bulun ve birincil birimin kopyalayın.
7. Dosyayı geri yükledikten sonra kopyalanan birim içeren NTFS klasörüne silebilirsiniz.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple Snapshot Manager veritabanını geri yükleme
Ana bilgisayarda StorSimple Snapshot Manager veritabanını düzenli olarak yedeklemelisiniz. Bir olağanüstü durum oluştuğunda veya ana bilgisayar için herhangi bir nedenle başarısız olursa, ardından yedekten geri yükleyebilirsiniz. Veritabanı yedeklemesi oluşturma elle yapılan bir işlemdir.

#### <a name="to-back-up-and-restore-the-database"></a>Yedeklemek ve veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   3. Üzerinde **Hizmetleri** penceresinde **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **Hizmeti Durdur**.
2. Ana bilgisayarda C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını bulun, dosyasını kopyalayın ve kopyayı güvenli bir konumda veya bulutta depolamak. Ana bilgisayar arıza yaparsa, yedekleme ilkelerini StorSimple Snapshot Manager'da oluşturduğunuz kurtarmanıza yardımcı olması için bu yedekleme dosyasını kullanabilirsiniz.
   
    ![Azure StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   2. Üzerinde **Hizmetleri** penceresinde **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **hizmetini yeniden**.
5. Ana bilgisayarda C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
6. Katalog XML dosyasını silin ve oluşturduğunuz yedeğin sürümünü ile değiştirin. 
7. StorSimple Snapshot Manager'ı başlatmak için Masaüstü StorSimple Snapshot Manager simgesine tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple çözümünüzü yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanarak](storsimple-snapshot-manager-admin.md).
* Daha fazla bilgi edinin [StorSimple Snapshot Manager görevleri ve iş akışları](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

