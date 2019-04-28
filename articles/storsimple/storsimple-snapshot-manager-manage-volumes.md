---
title: StorSimple Snapshot Manager ve birimler | Microsoft Docs
description: StorSimple Snapshot Manager MMC ek bileşenini görüntülemek ve birimleri yönetme ve yedeklemeleri yapılandırmak için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 260dfdd4b8fe7c277358fa5773029ea9a532740a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078361"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple Snapshot Manager görüntülemek ve birimler yönetmek için kullanın
## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager kullanabilirsiniz **birimleri** düğümü (üzerinde **kapsam** bölmesinde) birimleri seçin ve bunlarla ilgili bilgileri görüntülemek için. Birimleri, karşılık gelen birimlere ana bilgisayar tarafından bağlanan sürücüler olarak sunulur. **Birimleri** düğüm, yerel birimler ve kullanarak, iSCSI ve bir cihaz üzerinde birimler dahil olmak üzere, StorSimple tarafından desteklenen birim türleri gösterir. 

Desteklenen birimleri hakkında daha fazla bilgi için Git [birden çok birim türleri için destek](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Sonuçlar bölmesinde birim listesi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

**Birimleri** düğüm ayrıca sayesinde yeniden tarayın veya bunları StorSimple Snapshot Manager bulduktan sonra birimleri silin. 

Bu öğreticide nasıl bağlayın, başlatın ve birimleri biçimlendirmek ve StorSimple anlık görüntü Yöneticisi'ni kullanarak açıklanmaktadır:

* Birimler hakkındaki bilgileri görüntüleme 
* Birimleri silin
* Birimleri yeniden tara 
* Temel birim yapılandırın ve yedekleyin
* Dinamik yansıtılmış birim yapılandırın ve yedekleyin

> [!NOTE]
> Tüm **birim** düğüm eylemleri kullanılabilir ayrıca **eylemleri** bölmesi.
> 
> 

## <a name="mount-volumes"></a>Bağlama birimleri
Bağlamayı, başlatmayı ve StorSimple birimleri biçimlendirmek için aşağıdaki yordamı kullanın. Bu yordam, Disk Yönetimi, sabit diskler ve karşılık gelen birimi veya bölüm yönetmek için bir sistem yardımcı programını kullanır. Disk Yönetimi hakkında daha fazla bilgi için Git [Disk Yönetimi](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet Web sitesinde.

#### <a name="to-mount-volumes"></a>Bağlama birimleri için
1. Ana bilgisayarınızda Microsoft iSCSI başlatıcısını başlatın.
2. Bir hedef portal arabirimi IP adresleri veya bulma IP adresi girin ve cihaza bağlayın. Cihaz bağlandıktan sonra Windows sisteminize birimlerin erişilebilir. Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için bölümüne gidin "bir iSCSI hedef cihaza bağlanma" [iSCSI başlatıcısını yükleme ve yapılandırma Microsoft][1].
3. Disk Yönetimi'ni başlatmak için aşağıdaki seçeneklerden birini kullanın:
   
   * İçinde diskmgmt.msc yazın **çalıştırma** kutusu.
   * Sunucu Yöneticisi'ni başlatın, genişletme **depolama** düğümüne tıklayın ve ardından **Disk Yönetimi**.
   * Başlangıç **Yönetimsel Araçlar**, genişletme **Bilgisayar Yönetimi** düğümüne tıklayın ve ardından **Disk Yönetimi**. 
     
     > [!NOTE]
     > Disk Yönetimi'ni çalıştırmak için yönetici ayrıcalıkları kullanmanız gerekir.
     > 
     > 
4. Birimlerin çevrimiçi yap:
   
   1. Herhangi bir birim olarak işaretlenmiş Disk Yönetimi'nde sağ **çevrimdışı**.
   2. Tıklayın **yeniden Disk**. Disk işaretlenmelidir **çevrimiçi** disk yeniden etkinleştirildikten sonra.
5. Birimleri başlatır:
   
   1. Keşfedilen birimleri sağ tıklayın.
   2. Menüsünde **diski başlatma**.
   3. İçinde **diski başlatma** iletişim kutusu, başlatmak istediğiniz diskleri seçin ve ardından **Tamam**.
6. Basit birimleri biçimlendirin:
   
   1. Biçimlendirmek istediğiniz bir birime sağ tıklayın.
   2. Menüsünde **yeni basit birim**.
   3. Birimi biçimlendirmek için Yeni Basit Birim Sihirbazı'nı kullanın:
      
      * Birim boyutu belirtin.
      * Bir sürücü harfi sağlayın.
      * NTFS dosya sistemi seçin.
      * 64 KB ayırma birimi boyutu belirtin.
      * Hızlı biçimlendirme gerçekleştirin.
7. Birden çok bölüm birimleri biçimlendirin. Yönergeler için "Bölümler ve birimleri" bölümüne gidin [Disk Yönetimi'ni uygulama](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Birimlerinizi hakkındaki bilgileri görüntüleme
Yerel ve Azure StorSimple birimlerini hakkındaki bilgileri görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-volume-information"></a>Birim bilgilerini görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesinde tıklayın **birimleri** düğümü. Tüm Azure StorSimple birimlerini dahil olmak üzere, yerel ve bağlı birimlerin listesini görünür **sonuçları** bölmesi. Sütunları **sonuçları** bölmesinde yapılandırılabilir. (Sağ **birimleri** düğümünü **görünümü**ve ardından **sütunları Ekle/Kaldır**.)
   
    ![Sütunları yapılandırma](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Sonuçları sütun | Açıklama |
   |:--- |:--- |
   |  Ad |**Adı** sütunu bulunan her birime atanan sürücü harfini içerir. |
   |  Cihaz |**Cihaz** sütun konak bilgisayara bağlanan aygıtın IP adresini içerir. |
   |  Cihaz birim adı |**Cihaz birim adı** sütun, seçilen birim ait olduğu cihaz birim adını içerir. Bu, Azure portalında, belirli bir birim için tanımlanan birim addır. |
   |  Erişim yolları |**Erişim yolları** sütunu birimine erişim yolunu görüntüler. Birim ana bilgisayarda erişilebilir olduğu sürücü harfi veya bağlama noktası budur. |

## <a name="delete-a-volume"></a>Birim Sil
StorSimple anlık görüntü Yöneticisi'nden bir birimi silmek için aşağıdaki yordamı kullanın.

> [!NOTE]
> Herhangi bir birim grubu bir parçası ise, bir birim silemezsiniz. (Sil seçeneği bir birim grubu üyesi olan birimleri için kullanılabilir değildir.) Birim silinecek tüm birim grubunu silmeniz gerekir.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklayın **birimleri** düğümü. 
3. İçinde **sonuçları** bölmesinde, silmek istediğiniz birime sağ tıklayın.
4. Menüsünde **Sil**. 
   
    ![Birim Sil](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. **Birim Sil** iletişim kutusu görüntülenir. Tür **Onayla** metin kutusuna ve ardından **Tamam**.
6. StorSimple Snapshot Manager, varsayılan olarak, bir birimin silmeden önce yedekler. Silme işlemi istenmeden yapıldıysa Bu önlem, veri kaybına karşı koruyabilirsiniz. StorSimple Snapshot Manager görüntüler bir **otomatik anlık görüntü** birimin yedekler sırada ilerleme iletisi. 
   
    ![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Birimleri yeniden tara
StorSimple Snapshot Manager için bağlı birimleri yeniden taramak için aşağıdaki yordamı kullanın.

#### <a name="to-rescan-the-volumes"></a>Birimleri yeniden taramak için
1. StorSimple Snapshot Manager'ı başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **birimleri**ve ardından **birimleri yeniden tara**.
   
    ![Birimleri yeniden tara](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Bu yordam, StorSimple Snapshot Manager ile birim listesini eşitler. Sonuçlarda yeni birimlere veya silinen birimler gibi tüm değişiklikler yansıtılır.

## <a name="configure-and-back-up-a-basic-volume"></a>Yapılandırma ve temel bir birimi yedekleyin
Temel birim, bir yedekleme yapılandırmak ve ardından bir yedeklemeyi hemen başlatmak veya zamanlanmış yedeklemeler için bir ilke oluşturmak için aşağıdaki yordamı kullanın.

### <a name="prerequisites"></a>Önkoşullar
Başlamadan önce:

* StorSimple cihaz ve ana bilgisayar, düzgün yapılandırıldığından emin olun. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).
* Yükleme ve StorSimple Snapshot Manager'ı yapılandırma. Daha fazla bilgi için Git [StorSimple Snapshot Manager'ı Dağıtma](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Temel birim yedeklemeyi yapılandırmak için
1. StorSimple cihazında bir temel birim oluşturun.
2. Bağlamayı, başlatmayı ve açıklandığı gibi birimde [bağlama birimleri](#mount-volumes). 
3. Masaüstü StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
4. İçinde **kapsam** bölmesinde sağ **birimleri** düğümüne tıklayın ve ardından **birimleri yeniden tara**. Tarama bittiğinde, birimlerin bir listesini gözükeceğini **sonuçları** bölmesi. 
5. İçinde **sonuçları** bölmesinde, birime sağ tıklayın ve ardından **birim grubu oluştur**. 
   
    ![Birim grubu oluştur](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. İçinde **birim grubu oluştur** iletişim kutusu, birim grubu için bir ad yazın, birimleri atayın ve ardından **Tamam**.
7. İçinde **kapsam** bölmesini genişletin **birim gruplarını** düğümü. Yeni birim grubu altında görünmelidir **birim gruplarını** düğümü. 
8. Birim grubu adına sağ tıklayın.
   
   * (İsteğe bağlı) için etkileşimli bir yedekleme işi başlatmak için tıklatın **ele yedekleme**. 
   * Otomatik bir yedekleme işini zamanlamak için tıklayın **yedekleme İlkesi Oluştur**. Üzerinde **genel** sayfasında, listeden bir birim grubu seçin. Üzerinde **zamanlama** sayfasında, zamanlama ayrıntılarını girin. İşlemi tamamladığınızda, tıklayın **Tamam**. 
9. Yedekleme işi başlatıldığını onaylamak için genişletme **işleri** düğümünde **kapsam** bölmesi ve ardından **çalıştıran** düğümü. Şu anda çalışan iş listesi görünür **sonuçları** bölmesi. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Yapılandırma ve dinamik yansıtılmış birimi yedekleyin
Dinamik yansıtılmış birim yedeklemeyi yapılandırmak için aşağıdaki adımları tamamlayın:

* 1. Adım: Disk Yönetimi yansıtılmış dinamik birim oluşturmak için kullanın. 
* 2. Adım: StorSimple anlık görüntü yedekleme Yapılandırma Yöneticisi'ni kullanın.

### <a name="prerequisites"></a>Önkoşullar
Başlamadan önce:

* StorSimple cihaz ve ana bilgisayar, düzgün yapılandırıldığından emin olun. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
* Yükleme ve StorSimple Snapshot Manager'ı yapılandırma. Daha fazla bilgi için Git [StorSimple Snapshot Manager'ı Dağıtma](storsimple-snapshot-manager-deployment.md).
* İki birim StorSimple cihazında yapılandırın. (Örneklerde, kullanılabilir birimlerdir **Disk 1** ve **Disk 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>1. Adım: Disk Yönetimi yansıtılmış dinamik birim oluşturmak için kullanın
Disk Yönetimi, sabit diskleri ve birimleri veya içerdikleri bölümleri yönetmek için bir sistem aracıdır. Disk Yönetimi hakkında daha fazla bilgi için Git [Disk Yönetimi](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet Web sitesinde.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dinamik yansıtılmış birim oluşturmak için
1. Disk Yönetimi'ni başlatmak için aşağıdaki seçeneklerden birini kullanın: 
   
   * Açık **çalıştırma** kutusuna **Diskmgmt.msc**, ve Enter tuşuna basın.
   * Sunucu Yöneticisi'ni başlatın, genişletme **depolama** düğümüne tıklayın ve ardından **Disk Yönetimi**. 
   * Başlangıç **Yönetimsel Araçlar**, genişletme **Bilgisayar Yönetimi** düğümüne tıklayın ve ardından **Disk Yönetimi**. 
2. StorSimple cihazında iki birim kullanılabilir olduğundan emin olun. (Örnekte, kullanılabilir birimlerdir **Disk 1** ve **Disk 2**.) 
3. Disk Yönetimi penceresinde, alt bölme, sağ sütuna sağ **Disk 1** seçip **Yeni Yansıtılmış birim**. 
   
    ![Yeni Yansıtılmış birim](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Üzerinde **Yeni Yansıtılmış birim** sihirbaz sayfasını tıklatın **sonraki**.
5. Üzerinde **diskleri seçin** sayfasında **Disk 2** içinde **seçili** bölmesinde tıklayın **Ekle**ve ardından **sonraki**. 
6. Üzerinde **sürücü harfi atama veya yolu** sayfasında, Varsayılanları kabul edin ve ardından **sonraki**. 
7. Üzerinde **birim biçimlendirmeyi** sayfasında **ayırma birimi boyutu** kutusunda **64K**. Seçin **hızlı biçimlendirme gerçekleştirmek** onay kutusunu işaretleyin ve ardından **sonraki**. 
8. Üzerinde **Yeni Yansıtılmış birim Tamamlanıyor** sayfasında, ayarlarınızı gözden geçirin ve ardından **son**. 
9. Temel diski dinamik diske dönüştürülecek belirten bir ileti görüntülenir. **Evet**'e tıklayın.
   
    ![Dinamik disk dönüştürme iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Disk Yönetimi'nde Disk 1 ve 2 Disk yansıtılmış dinamik birimler olarak gösterildiğinden emin olun. (**Dinamik** Durum sütununda görünür olmalıdır ve kapasite çubuğu rengi yansıtılmış birim gösteren kırmızı değiştirmeniz gerekir.) 
    
    ![Disk Yönetimi yansıtılmış dinamik diskler](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>2. Adım: Yedeklemeyi yapılandırmak için StorSimple anlık görüntü Yöneticisi'ni kullanın
Bir dinamik yansıtılmış birim yapılandırın ve ardından bir yedeklemeyi hemen başlatmak veya zamanlanmış yedeklemeler için bir ilke oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dinamik yansıtılmış birim yedeklemeyi yapılandırmak için
1. Masaüstü StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
2. İçinde **kapsam** bölmesinde sağ tıklayın **birimleri** düğümünü seçip alt **birimleri yeniden tara**. Tarama bittiğinde, birimlerin bir listesini gözükeceğini **sonuçları** bölmesi. Dinamik yansıtılmış birim tek bir birim listelenir. 
3. İçinde **sonuçları** bölmesinde dinamik yansıtılmış birime sağ tıklayın ve ardından **birim grubu oluştur**. 
4. İçinde **birim grubu oluştur** iletişim kutusu, birim grubu için bir ad yazın, dinamik yansıtılmış birim bu grubuna atayın ve ardından **Tamam**. 
5. İçinde **kapsam** bölmesini genişletin **birim gruplarını** düğümü. Yeni birim grubu altında görünmelidir **birim gruplarını** düğümü. 
6. Birim grubu adına sağ tıklayın. 
   
   * (İsteğe bağlı) için etkileşimli bir yedekleme işi başlatmak için tıklatın **ele yedekleme**. 
   * Otomatik bir yedekleme işini zamanlamak için tıklayın **yedekleme İlkesi Oluştur**. Üzerinde **genel** sayfasında, listeden bir birim grubu seçin. Üzerinde **zamanlama** sayfasında, zamanlama ayrıntılarını girin. İşlemi tamamladığınızda, tıklayın **Tamam**. 
7. Çalışan yedekleme işini izleyebilirsiniz. İçinde **kapsam** bölmesinde genişletin **işleri** düğümünü ve ardından **çalıştıran**, iş ayrıntılarını görünür **sonuçları** bölmesi. Yedekleme işi tamamlandığında, ayrıntıları aktarılan **son 24** saat proje listesi. 

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
