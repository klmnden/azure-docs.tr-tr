---
title: StorSimple Snapshot Manager ve birimler | Microsoft Docs
description: "StorSimple Snapshot Manager MMC ek bileşenini görüntülemek ve birimleri yönetmek için ve yedeklemeler yapılandırmak için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 2c0b211bced99d272a73a7b018a22f99d8d58aa9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple Snapshot Manager görüntülemek ve birimleri yönetmek için kullanın
## <a name="overview"></a>Genel Bakış
StorSimple anlık görüntü Yöneticisi'ni kullanabilirsiniz **birimleri** düğümü (üzerinde **kapsam** bölmesi) birimleri seçin ve bunlarla ilgili bilgileri görüntülemek için. Birimleri, ana bilgisayar tarafından bağlanan birimler için karşılık gelen sürücüleri olarak sunulur. **Birimleri** düğümü yerel birimleri ve StorSimple, iSCSI ve bir cihaz kullanımı ile keşfedilen birimler dahil tarafından desteklenen birim türleri gösterir. 

Desteklenen birimleri hakkında daha fazla bilgi için Git [birden çok birim türleri için destek](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Sonuçlar bölmesinde birim listesi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

**Birimleri** düğümü de sağlar yeniden tarayın veya StorSimple Snapshot Manager bunları bulur sonra birimleri silin. 

Bu öğretici nasıl bağlamak, başlatmak ve birimleri biçimlendirme ve StorSimple anlık görüntü Yöneticisi'ni kullanarak açıklanmaktadır:

* Birimler hakkındaki bilgileri görüntüleme 
* Birimleri silin
* Birimleri yeniden tara 
* Temel birim yapılandırmak ve yedekleyin
* Dinamik yansıtılmış birim yapılandırmak ve yedekleyin

> [!NOTE]
> Tüm **birim** düğümü Eylemler bulunan de **Eylemler** bölmesi.
> 
> 

## <a name="mount-volumes"></a>Bağlama birimleri
Bağlamayı, başlatmayı ve StorSimple birimlerini biçimlendirmek için aşağıdaki yordamı kullanın. Bu yordam, Disk Yönetimi, sabit diskleri ve karşılık gelen birimlerini veya bölümlerini yönetmek için bir sistem hizmet kullanır. Disk Yönetimi hakkında daha fazla bilgi için Git [Disk Yönetimi](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet Web sitesinde.

#### <a name="to-mount-volumes"></a>Birimleri bağlamak için
1. Ana bilgisayarınızda Microsoft iSCSI başlatıcısını başlatın.
2. Bir hedef portal arabirimi IP adresleri veya bulma IP adresi sağlayın ve cihaza bağlanın. Cihaz bağlandıktan sonra Windows sisteminize birimleri erişilebilir. Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için bölümüne gidin "bir iSCSI hedef aygıta bağlanma" [yükleme ve yapılandırma Microsoft iSCSI başlatıcısı][1].
3. Disk Yönetimi'ni başlatmak için aşağıdaki seçeneklerden birini kullanın:
   
   * İçinde diskmgmt.msc yazın **çalıştırmak** kutusu.
   * Sunucu Yöneticisi'ni başlatın, genişletin **depolama** düğümünü ve ardından **Disk Yönetimi**.
   * Başlat **Yönetimsel Araçlar**, genişletin **Bilgisayar Yönetimi** düğümünü ve ardından **Disk Yönetimi**. 
     
     > [!NOTE]
     > Disk Yönetimi'ni çalıştırmak için yönetici ayrıcalıkları kullanmanız gerekir.
     > 
     > 
4. Birimlerin çevrimiçi yap:
   
   1. İşaretlenen herhangi bir birim Disk Yönetimi'nde sağ **çevrimdışı**.
   2. Tıklatın **yeniden Disk**. Disk işaretlenmelidir **çevrimiçi** disk yeniden etkinleştirildikten sonra.
5. Birimleri başlatın:
   
   1. Bulunan bir birimi sağ tıklatın.
   2. Menüsünde seçin **diski başlatma**.
   3. İçinde **diski başlatma** iletişim kutusunda, başlatmak istediğiniz diskleri seçin ve ardından **Tamam**.
6. Basit birimler Biçimlendir:
   
   1. Biçimlendirmek istediğiniz bir birime sağ tıklayın.
   2. Menüsünde seçin **yeni basit birim**.
   3. Birimi biçimlendirmek için Yeni Basit Birim Sihirbazı'nı kullanın:
      
      * Birimin boyutunu belirtin.
      * Bir sürücü harfi sağlayın.
      * NTFS dosya sistemi seçin.
      * 64 KB ayırma birimi boyutu belirtin.
      * Hızlı biçimlendirme gerçekleştirin.
7. Birden çok bölüm birimleri biçimlendirme. Yönergeler için "Bölüm ve birimleri" bölümüne Git [Disk Yönetimi uygulama](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Birimlerinizi hakkındaki bilgileri görüntüleme
Yerel ve Azure StorSimple birimler hakkındaki bilgileri görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-volume-information"></a>Birim bilgileri görüntülemek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın. 
2. İçinde **kapsam** bölmesinde tıklatın **birimleri** düğümü. Tüm Azure StorSimple birimleri de dahil olmak üzere, yerel ve bağlı birimlerin listesini görünür **sonuçları** bölmesi. Sütunları **sonuçları** bölmesinde yapılandırılabilir. (Sağ **birimleri** düğümü, select **Görünüm**ve ardından **Sütun Ekle/Kaldır**.)
   
    ![Sütunları Yapılandır](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Sonuçları sütun | Açıklama |
   |:--- |:--- |
   |  Ad |**Adı** sütunu bulunan her birime atanan sürücü harfini içerir. |
   |  Cihaz |**Aygıt** sütun ana bilgisayara bağlı aygıtın IP adresi içeriyor. |
   |  Cihaz birim adı |**Aygıt birim adı** sütun seçilen birime ait olduğu cihaz birim adını içerir. Bu, özel birim için Azure Portalı'nda tanımlanan birim adıdır. |
   |  Erişim yolları |**Erişim yolları** sütunu birime erişim yolunu görüntüler. Birim ana bilgisayara erişilebilir olduğu sürücü harfi veya bağlama noktası budur. |

## <a name="delete-a-volume"></a>Bir birim Sil
StorSimple anlık görüntü Yöneticisi'nden bir birimi silmek için aşağıdaki yordamı kullanın.

> [!NOTE]
> Herhangi bir birim grubu parçası ise, bir birim silemezsiniz. (Sil seçeneğini bir birim grubu üyesi olan birimleri için kullanılabilir değildir.) Birimi silmek için tüm birim grubunu silmeniz gerekir.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde tıklatın **birimleri** düğümü. 
3. İçinde **sonuçları** bölmesinde, silmek istediğiniz birime sağ tıklayın.
4. Menüsünde **silmek**. 
   
    ![Bir birim Sil](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. **Birim Sil** iletişim kutusu görüntülenir. Tür **Onayla** metin kutusuna ve ardından **Tamam**.
6. Varsayılan olarak, StorSimple Snapshot Manager silmeden önce bir birim yedekler. Silme işlemi istenmeden yapıldıysa, bu önlem veri kaybına karşı koruyabilirsiniz. StorSimple Snapshot Manager görüntüler bir **otomatik anlık görüntü** birimi yavaşça yedekler olsa ilerleme iletisi. 
   
    ![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Birimleri yeniden tara
StorSimple Snapshot Manager bağlı birimleri yeniden taramak için aşağıdaki yordamı kullanın.

#### <a name="to-rescan-the-volumes"></a>Birimleri yeniden taramak için
1. StorSimple anlık görüntü Yöneticisi'ni başlatmak için Masaüstü simgesine tıklayın.
2. İçinde **kapsam** bölmesinde sağ **birimleri**ve ardından **birimleri yeniden tara**.
   
    ![Birimleri yeniden tara](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Bu yordam birim listesi StorSimple Snapshot Manager ile eşitler. Yeni birimlere veya silinen birimler gibi herhangi bir değişiklik sonuçları yansıtılır.

## <a name="configure-and-back-up-a-basic-volume"></a>Yapılandırma ve temel bir birimi yedekleyin
Temel birim yedeğini yapılandırın ve ardından bir yedeklemeyi hemen başlatmak veya zamanlanmış yedeklemeler için bir ilke oluşturmak için aşağıdaki yordamı kullanın.

### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce:

* StorSimple cihaz ve ana bilgisayar, doğru yapılandırıldığından emin olun. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).
* Yükleyin ve StorSimple Snapshot Manager yapılandırın. Daha fazla bilgi için Git [dağıtmak StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Temel birim yedekleme yapılandırmak için
1. Temel birim StorSimple cihazında oluşturun.
2. Bağlamayı, başlatmayı ve açıklandığı gibi birimi biçimlendirmek [bağlama birimleri](#mount-volumes). 
3. Masaüstünüzde StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
4. İçinde **kapsam** bölmesinde sağ **birimleri** düğümünü ve ardından **birimleri yeniden tara**. Tarama tamamlandığında, birimlerin listesini görüntülenmelidir **sonuçları** bölmesi. 
5. İçinde **sonuçları** bölmesinde, birime sağ tıklayın ve ardından **birim grubu oluştur**. 
   
    ![Birim grubu oluştur](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. İçinde **birim grubu oluştur** iletişim kutusu, birim grubu için bir ad girin, birimleri atayın ve ardından **Tamam**.
7. İçinde **kapsam** bölmesini genişletin **birim grupları** düğümü. Yeni birim grubu altında görünmelidir **birim grupları** düğümü. 
8. Birim grubu adını sağ tıklatın.
   
   * (İsteğe bağlı) için etkileşimli bir yedekleme işini başlatmak için tıklatın **ele yedekleme**. 
   * Bir otomatik yedekleme işini zamanlamak için tıklayın **yedekleme İlkesi Oluştur**. Üzerinde **genel** sayfasında, listeden bir birim grubu seçin. Üzerinde **zamanlama** sayfasında, zamanlama ayrıntılarını girin. İşiniz bittiğinde, tıklatın **Tamam**. 
9. Yedekleme işi başlatıldığını doğrulamak için Genişlet **işleri** düğümünde **kapsam** bölmesi ve ardından **çalıştıran** düğümü. Şu anda çalışan işi listesi görünür **sonuçları** bölmesi. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Yapılandırma ve dinamik yansıtılmış birimi yedekleyin
Dinamik yansıtılmış birim yedeğini yapılandırmak için aşağıdaki adımları tamamlayın:

* 1. adım: Disk dinamik yansıtılmış birim oluşturmak için yönetimini kullanma. 
* 2. adım: StorSimple anlık görüntü yedekleme Yapılandırma Yöneticisi'ni kullanın.

### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce:

* StorSimple cihaz ve ana bilgisayar, doğru yapılandırıldığından emin olun. Daha fazla bilgi için Git [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
* Yükleyin ve StorSimple Snapshot Manager yapılandırın. Daha fazla bilgi için Git [dağıtmak StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
* İki birimleri StorSimple cihazında yapılandırın. (Örneklerde, kullanılabilir birimlerdir **Disk 1** ve **Disk 2**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>1. adım: Dinamik yansıtılmış birim oluşturmak kullanım Disk Yönetimi
Disk Yönetimi, sabit diskleri ve birimleri veya içerdikleri bölümleri yönetmek için bir sistem aracıdır. Disk Yönetimi hakkında daha fazla bilgi için Git [Disk Yönetimi](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet Web sitesinde.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dinamik yansıtılmış birim oluşturmak için
1. Disk Yönetimi'ni başlatmak için aşağıdaki seçeneklerden birini kullanın: 
   
   * Açık **çalıştırmak** kutusuna **Diskmgmt.msc**, ve Enter tuşuna basın.
   * Sunucu Yöneticisi'ni başlatın, genişletin **depolama** düğümünü ve ardından **Disk Yönetimi**. 
   * Başlat **Yönetimsel Araçlar**, genişletin **Bilgisayar Yönetimi** düğümünü ve ardından **Disk Yönetimi**. 
2. StorSimple cihazında iki birimler kullanılabilir olduğundan emin olun. (Örnekte, kullanılabilir birimlerdir **Disk 1** ve **Disk 2**.) 
3. Disk Yönetimi penceresinde alt bölmede sağındaki sütunu sağ **Disk 1** seçip **Yeni Yansıtılmış birim**. 
   
    ![Yeni Yansıtılmış birim](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Üzerinde **Yeni Yansıtılmış birim** sihirbaz sayfasında, tıklatın **sonraki**.
5. Üzerinde **diskleri Seç** sayfasında, seçin **Disk 2** içinde **seçili** bölmesinde tıklatın **Ekle**ve ardından **sonraki**. 
6. Üzerinde **sürücü harfi atama veya yol** sayfasında, Varsayılanları kabul edin ve ardından **sonraki**. 
7. Üzerinde **birim biçimlendirmeyi** sayfasında **ayırma birimi boyutu** kutusunda **64K**. Seçin **hızlı biçimlendirme gerçekleştirmek** onay kutusunu işaretleyin ve ardından **sonraki**. 
8. Üzerinde **Yeni Yansıtılmış birim Tamamlanıyor** sayfasında, ayarlarınızı gözden geçirin ve ardından **son**. 
9. Temel diski dinamik diske dönüştürülecek belirtmek için bir ileti görüntülenir. **Evet**'e tıklayın.
   
    ![Dinamik disk dönüştürme iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Disk Yönetimi'nde Disk 1 ve Disk 2 dinamik yansıtılmış birimler olarak gösterildiğinden emin olun. (**Dinamik** Durum sütununda görünür olmalıdır ve kapasite çubuğu rengini yansıtılmış birim belirten kırmızı için değiştirmeniz gerekir.) 
    
    ![Disk Yönetimi yansıtılmış dinamik diskler](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>2. adım: StorSimple anlık görüntü yedeklemeyi Yapılandırma Yöneticisi'ni
Dinamik yansıtılmış birim yapılandırın ve ardından bir yedeklemeyi hemen başlatmak veya zamanlanmış yedeklemeler için bir ilke oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dinamik yansıtılmış birim yedeğini yapılandırmak için
1. Masaüstünüzde StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
2. İçinde **kapsam** bölmesinde sağ **birimleri** düğümü ve seçin **birimleri yeniden tara**. Tarama tamamlandığında, birimlerin listesini görüntülenmelidir **sonuçları** bölmesi. Dinamik yansıtılmış birim tek bir birim listelenir. 
3. İçinde **sonuçları** bölmesinde, dinamik yansıtılmış birime sağ tıklayın ve ardından **birim grubu oluştur**. 
4. İçinde **birim grubu oluştur** iletişim kutusu, birim grubu için bir ad yazın, bu gruba dinamik yansıtılmış birim atayın ve ardından **Tamam**. 
5. İçinde **kapsam** bölmesini genişletin **birim grupları** düğümü. Yeni birim grubu altında görünmelidir **birim grupları** düğümü. 
6. Birim grubu adını sağ tıklatın. 
   
   * (İsteğe bağlı) için etkileşimli bir yedekleme işini başlatmak için tıklatın **ele yedekleme**. 
   * Bir otomatik yedekleme işini zamanlamak için tıklayın **yedekleme İlkesi Oluştur**. Üzerinde **genel** sayfasında, listeden birim grubu seçin. Üzerinde **zamanlama** sayfasında, zamanlama ayrıntılarını girin. İşiniz bittiğinde, tıklatın **Tamam**. 
7. Çalışan yedekleme işini izleyebilirsiniz. İçinde **kapsam** bölmesinde genişletin **işleri** düğümünü ve ardından **çalıştıran**, iş ayrıntılarını görünür **sonuçları** bölmesi. Yedekleme işi tamamlandığında, ayrıntıları aktarılır **son 24** saatleri proje listesi. 

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple çözümünüzün yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-admin.md).
* Bilgi edinmek için nasıl [birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager kullanmak](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
