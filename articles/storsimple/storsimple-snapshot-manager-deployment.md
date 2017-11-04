---
title: "StorSimple Snapshot Manager dağıtma | Microsoft Docs"
description: "StorSimple Snapshot Manager StorSimple veri koruma ve yedekleme özellikleri yönetmek için MMC ek bileşenini yükleyip öğrenin."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: cde355381b0d726a1ab340bc4230b2dc8f6e2c56
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple Snapshot Manager MMC ek bileşenini dağıtma

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamda yedekleme yönetimini basitleştirir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple şirket içi yönetme ve tam olarak tümleşik depolama sistemi değilmiş gibi depolama böylece yedekleme ve geri yükleme işlemleri basitleştirir ve maliyetlerini azaltma bulut. 

Bu öğretici, yükleme, kaldırma ve StorSimple Snapshot Manager yükseltme yordamları yanı sıra yapılandırma gereksinimlerini açıklar.

> [!NOTE]
> * StorSimple Snapshot Manager, Microsoft Azure StorSimple sanal (olarak da bilinen StorSimple sanal cihazlar şirket) diziler yönetmek için kullanamazsınız.
> * StorSimple Cihazınızda StorSimple güncelleştirme 2 yüklemeyi planlıyorsanız, StorSimple anlık görüntü Yöneticisi'nin en son sürümü karşıdan yükleyip emin olun **StorSimple güncelleştirme 2 yüklemeden önce**. StorSimple anlık görüntü Yöneticisi'nin en son sürümü, geriye dönük olarak uyumludur ve Microsoft Azure StorSimple serbest bırakılmış tüm sürümleri ile çalışır. StorSimple anlık görüntü Yöneticisi'nin önceki sürümü kullanıyorsanız, (, yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez) güncelleştirmeniz gerekir.


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple anlık görüntü Yöneticisi'ni yükleme
StorSimple Snapshot Manager Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2 işletim sistemini çalıştıran bilgisayarlara yüklenebilir. Windows 2008 R2 çalıştıran sunucularda, Windows Server 2008 SP1 ve Windows Management Framework 3.0 de yüklemeniz gerekir.

Yüklemeden veya StorSimple Snapshot Manager ek bileşenini Microsoft Yönetim Konsolu (MMC) için yükseltmeden önce Microsoft Azure StorSimple cihaz ve ana bilgisayar sunucusu olduğunu doğru yapılandırıldığından emin olun.

## <a name="configure-prerequisites"></a>Önkoşulları yapılandırma
Aşağıdaki adımlar, StorSimple Snapshot Manager yüklemeden önce tamamlanması gereken yapılandırma görevleri üst düzey bir genel bakış sağlar. Microsoft Azure StorSimple yapılandırmasını tamamlamak ve sistem gereksinimleri ve adım adım yönergeler de dahil olmak üzere, kurulum bilgileri görmek için [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Başlamadan önce gözden [dağıtım yapılandırma denetim listesi](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) ve ve [dağıtımının önkoşulları](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager yüklemeden önce
1. Kutusundan çıkarma, bağlama ve açıklandığı gibi Microsoft Azure StorSimple cihazı bağlayın [StorSimple 8100 cihazınız yüklemek](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 model Cihazınızı yüklemek](storsimple-8600-hardware-installation.md).
2. Ana bilgisayarınız aşağıdaki işletim sistemlerinden birini çalıştığından emin olun:
   
   * Windows Server 2008 R2 (Windows 2008 R2 çalıştıran üzerinde sunucular da Windows Server 2008 SP1 ve Windows Management Framework 3.0 yüklemeniz gerekir)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     StorSimple sanal cihaz için bir Microsoft Azure sanal makine konak olması gerekir.
3. Tüm Microsoft Azure StorSimple yapılandırma gereksinimlerini karşıladığından emin olun. Ayrıntılar için Git [dağıtımının önkoşulları](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Cihaza ana bilgisayara bağlanın ve ilk yapılandırmasını gerçekleştirin. Ayrıntılar için Git [dağıtım adımları](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Cihazda birimler oluşturabilir, konağa atayın ve konak bağlama ve bunları doğrulayın. StorSimple Snapshot Manager birimler aşağıdaki türlerini destekler:
   
   * Temel birim
   * Basit birimler
   * Dinamik birimler
   * Yansıtılmış dinamik birimler (RAID 1)
   * Küme Paylaşılan Birimleri
     
     Birimleri StorSimple cihazı veya StorSimple sanal cihaz oluşturma hakkında daha fazla bilgi için Git [6. adım: birim oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume), [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Yeni bir StorSimple Snapshot Manager yükleyin
StorSimple Snapshot Manager'ı yüklemeden önce StorSimple cihazı veya StorSimple sanal cihazı üzerinde oluşturulan birimleri bağlanır, başlatılmış ve açıklandığı şekilde biçimlendirilmiş olduğunu emin olun [önkoşulları yapılandırma](#configure-prerequisites).

> [!IMPORTANT]
> * StorSimple sanal cihaz için bir Microsoft Azure sanal makine konak olması gerekir. 
> * Ana bilgisayar Windows 2008 R2, Windows Server 2012 veya Windows Server 2012 R2 çalıştırması gerekir. Sunucunuz Windows Server 2008 R2 çalıştırıyorsa, Windows Server 2008 SP1 ve Windows Management Framework 3.0 yüklemeniz gerekir.
> * StorSimple anlık görüntü Yöneticisi cihaz bağlanabilmesi için önce bir iSCSI bağlantısı StorSimple cihazı ana bilgisayardan yapılandırmanız gerekir.

StorSimple anlık görüntü Yöneticisi'nin yeni bir yüklemeyi tamamlamak için aşağıdaki adımları izleyin. Bir yükseltme yüklüyorsanız, Git [yükseltme veya yeniden StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* 1. adım: StorSimple anlık görüntü Yöneticisi'ni yükleyin 
* 2. adım: StorSimple Snapshot Manager bir cihaza bağlanma 
* Adım 3: cihaz bağlantısı doğrulama 

### <a name="step-1-install-storsimple-snapshot-manager"></a>1. adım: StorSimple anlık görüntü Yöneticisi'ni yükleyin
StorSimple anlık görüntü Yöneticisi'ni yüklemek için aşağıdaki adımları kullanın.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple anlık görüntü Yöneticisi'ni yüklemek için
1. StorSimple Snapshot Manager yazılımı yükle (Git [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) Microsoft Download Center'da) ve ana bilgisayarda yerel olarak kaydedin.
2. Dosya Gezgini'nde, sıkıştırılmış klasörü sağ tıklatın ve ardından **tümünü Ayıkla**.
3. İçinde **sıkıştırılmış ayıklamak (Zipped) klasörleri** penceresi, **bir hedef seçin ve dosyaları ayıklayın** kutusuna yazın veya burada istediğinizi ayıklanacak dosya yoluna göz atın.
   
    > [!IMPORTANT]
    > StorSimple Snapshot Manager C: sürücüsünde yüklemeniz gerekir.
    
4. Seçin **Göster ayıklanan dosyaları tamamlandığında** onay kutusunu işaretleyin ve ardından **ayıklamak**.
   
    ![Dosyaları ayıkla iletişim kutusu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Ayıklama işlemi tamamlandığında, hedef klasör açılır. Hedef klasörde görüntülenir uygulama Kurulum simgesini çift tıklatın.
6. Zaman **Kurulumu başarılı** iletisi görüntülendikten sonra **Kapat**. StorSimple Snapshot Manager simgesi masaüstünüzde görmeniz gerekir.
   
    ![Masaüstü simgesi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>2. adım: StorSimple Snapshot Manager bir cihaza bağlanma
StorSimple Snapshot Manager bir StorSimple cihazına bağlanmak için aşağıdaki adımları kullanın.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple Snapshot Manager bir aygıta bağlanmayı
1. Masaüstünüzde StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. Pencere içeren bir **kapsam** bölmesinde, bir **sonuçları** bölmesinde ve bir **Eylemler** bölmesi. 
   
    ![StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * **Kapsam** bölmesinde (sol bölme) içeren bir ağaç yapısında düzenlenmiş düğüm listesi. Bir görünüm veya bu düğüme ilgili belirli verileri seçmek için bazı düğümler genişletebilirsiniz. Genişletmek veya bir düğüm daraltmak için ok simgesine tıklayın. Bir öğeyi sağ **kapsam** bu öğe için kullanılabilir eylemlerin listesini görmek için bölmesi.
   * **Sonuçları** bölmesinde (Orta bölme) düğüm, görünümü veya seçtiğiniz veri hakkındaki ayrıntılı durum bilgilerini içeren **kapsam** bölmesi.
   * **Eylemler** bölmesi listeler düğümü, görünümü veya seçtiğiniz veri gerçekleştirebileceğiniz işlemler **kapsam** bölmesi.
     
     StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi tam bir açıklaması için bkz: [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).
2. İçinde **kapsam** bölmesinde sağ **aygıtları** düğümünü ve ardından **bir aygıt yapılandırma**. **Bir aygıt yapılandırma** iletişim kutusu görüntülenir.
   
    ![Bir aygıt yapılandırma](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. İçinde **aygıt** liste kutusunda, Microsoft Azure StorSimple cihazı veya sanal aygıt IP adresi seçin. İçinde **parola** metin kutusuna, Azure portalında cihaz için oluşturduğunuz StorSimple Snapshot Manager parolasını yazın. **Tamam** düğmesine tıklayın.
4. StorSimple Snapshot Manager tanımladığınız cihaz için arar. Cihaz kullanılabilir değilse, StorSimple Snapshot Manager bağlantı ekler. Yapabilecekleriniz [aygıt bağlantıyı doğrulama](#to-verify-the-connection) bağlantı başarıyla eklendiğini onaylamak için.
   
    Aygıt için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Tıklatın **Tamam** hata iletisini kapatın ve ardından **iptal** kapatmak için **bir aygıt yapılandırma** iletişim kutusu.
5. Birim grubu yedeklemeleri ilişkili koşuluyla StorSimple Snapshot Manager bir aygıta bağlandığında, o aygıtı için yapılandırılmış her birim grubu içeri aktarır. İlişkili yedeklemelerinizi olmayan birim grupları içeri aktarılmadı. Ayrıca, bir birim grubu için oluşturulan yedekleme ilkeleri içeri aktarılmadı. Alınan grupları görmek için en üstteki sağ tıklatın **birim grupları** düğümünde **kapsam** bölmesinde ve tıklatın **alınan grupları geçiş**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Adım 3: cihaz bağlantısı doğrulama
StorSimple Snapshot Manager StorSimple cihazına bağlı olduğunu doğrulamak için aşağıdaki adımları kullanın.

#### <a name="to-verify-the-connection"></a>Bağlantıyı doğrulamak için
1. İçinde **kapsam** bölmesinde tıklatın **aygıtları** düğümü.
   
    ![StorSimple Snapshot Manager cihaz durumu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Denetleme **sonuçları** bölmesi: 
   
   * Cihaz simgesinde yeşil bir göstergesi görünür değilse ve **kullanılabilir** görünür **durum** sütun sonra cihaz bağlı. 
   * Cihaz simgesinde kırmızı bir göstergesi görünür ve kullanılamaz görünür **durum** sütun sonra aygıt bağlı değil. 
   * Varsa **yenilemek** görünür **durum** sütun sonra StorSimple Snapshot Manager alma birim grupları ve ilişkili yedeklemeler bağlı bir aygıt için.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Yükseltme veya StorSimple Snapshot Manager yeniden yükleme
Yükseltme veya yazılımı yeniden yüklemek için önce StorSimple Snapshot Manager tamamen kaldırmanız gerekir. 

StorSimple Snapshot Manager yeniden yüklemeden önce ana bilgisayarda varolan StorSimple Snapshot Manager veritabanını yedekleyin. Bu yedekleme ilkeleri ve yapılandırma bilgileri kaydeder, böylece bu verileri kolayca yedekten geri yükleyebilirsiniz.

Yükseltme veya yeniden yüklemeyi StorSimple Snapshot Manager, şu adımları izleyin:

* 1. adım: StorSimple anlık görüntü Yöneticisi'ni kaldırma 
* 2. adım: StorSimple Snapshot Manager veritabanını yedekle 
* 3. adım: StorSimple anlık görüntü Yöneticisi'ni yeniden yükleyin ve veritabanını geri yükleyin 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>1. adım: StorSimple anlık görüntü Yöneticisi'ni kaldırma
StorSimple anlık görüntü Yöneticisi'ni kaldırmak için aşağıdaki adımları kullanın.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple anlık görüntü Yöneticisi'ni kaldırmak için
1. Ana bilgisayarda açmak **Denetim Masası**, tıklatın **programları**ve ardından **programlar ve Özellikler**.
2. Sol bölmede **Kaldır veya Değiştir bir program**.
3. Sağ **StorSimple Snapshot Manager**ve ardından **kaldırma**.
4. StorSimple Snapshot Manager Kurulum programını başlatır. Tıklatın **değiştirme kurulum**ve ardından **kaldırma**.
   
   > [!NOTE]
   > Arka planda çalışan tüm MMC işlemler varsa, StorSimple Snapshot Manager veya Disk Yönetimi gibi kaldırma işlemi başarısız olur ve program kaldırmayı denemeden önce tüm örneklerini MMC kapatmak için bir ileti alırsınız. Seçin **otomatik olarak uygulamaları kapatın ve Kurulum tamamlandıktan sonra yeniden başlatmayı dene**ve ardından **Tamam**.
   > 
   > 
5. Kaldırma işlemi tamamlandığında, bir **Kurulumu başarılı** ileti görüntülenir. **Kapat**’a tıklayın.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>2. adım: StorSimple Snapshot Manager veritabanını yedekle
Oluşturmak ve StorSimple Snapshot Manager veritabanının bir kopyasını kaydetmek için aşağıdaki adımları kullanın.

#### <a name="to-back-up-the-database"></a>Veritabanını yedeklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   3. Üzerinde **Hizmetleri** sayfasında, **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmetini durdurun**.
      
        ![StorSimple cihaz Yöneticisi hizmetini durdurun](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
  
3. Katalog XML dosyasını bulun, dosyasını kopyalayın ve güvenli bir konumda veya bulutta kopyayı depolar.
   
    ![StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   2. Üzerinde **Hizmetleri** sayfasında, **Microsoft StorSimple Yönetim bildirimleri**e.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmeti yeniden**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>3. adım: StorSimple anlık görüntü Yöneticisi'ni yeniden yükleyin ve veritabanını geri yükleyin
StorSimple Snapshot Manager yeniden yüklemek için adımları [yeni bir StorSimple Snapshot Manager yükleme](#install-a-new-storsimple-snapshot-manager). Ardından, StorSimple Snapshot Manager veritabanını geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-restore-the-database"></a>Veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   3. Üzerinde **Hizmetleri** sayfasında, **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmetini durdurun**.
2. C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın.
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını silin ve daha önce kaydettiğiniz sürümle değiştirin.
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde, select **Hizmetleri**.
   2. Üzerinde **Hizmetleri** sayfasında, **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklatın **hizmeti yeniden**.

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple anlık görüntü Yöneticisi hakkında daha fazla bilgi için şuraya gidin [StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md).
* StorSimple anlık görüntü Yöneticisi kullanıcı arabirimi hakkında daha fazla bilgi için şuraya gidin [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).
* StorSimple anlık görüntü Yöneticisi'ni kullanma hakkında daha fazla bilgi için şuraya gidin [StorSimple çözümünüzün yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-admin.md).

