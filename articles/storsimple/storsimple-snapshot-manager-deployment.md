---
title: StorSimple Snapshot Manager'ı dağıtma | Microsoft Docs
description: StorSimple veri koruma ve yedekleme özellikleri yönetmek için MMC ek bileşenini, StorSimple Snapshot Manager'ı yükleyip öğrenin.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: f0128f57-519e-49ec-9187-23575809cdbe
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: ee17e4b69d1e6c9de465e4241ee2237361e320b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61077835"
---
# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple Snapshot Manager MMC ek bileşenini dağıtma

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, veri koruma ve Microsoft Azure StorSimple ortamında yedekleme yönetimini basitleştiren bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager ile Microsoft Azure StorSimple şirket içi yönetebilir ve tamamen tümleşik depolama sistemi değilmiş gibi yedekleme ve geri yükleme işlemleri basitleştirmek ve maliyetlerin düşürülmesi bulut depolama. 

Bu öğreticide, yapılandırma gereksinimlerinin yanı sıra, yükleme, kaldırma ve StorSimple Snapshot Manager yükseltme yordamları açıklanmaktadır.

> [!NOTE]
> * Microsoft Azure StorSimple sanal (diğer adıyla StorSimple sanal cihazları şirket) dizilerini yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.
> * StorSimple Cihazınızda StorSimple güncelleştirme 2 yüklemeyi planlıyorsanız, StorSimple Snapshot Manager'ın en son sürümünü indirip yüklemeniz mutlaka **StorSimple güncelleştirme 2'yi yüklemeden önce**. StorSimple Snapshot Manager'ın en son sürümü, geriye dönük uyumludur ve Microsoft Azure StorSimple yayımlanan tüm sürümleri ile çalışır. StorSimple Snapshot Manager'ın önceki sürümü kullanıyorsanız (, yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez) güncelleştirmeniz gerekir.


## <a name="storsimple-snapshot-manager-installation"></a>StorSimple Snapshot Manager yükleme
StorSimple Snapshot Manager, Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2 işletim sistemini çalıştıran bilgisayarlara yüklenebilir. Windows 2008 R2 çalıştıran sunuculara, Windows Server 2008 SP1 ve Windows Management Framework 3.0 da yüklemeniz gerekir.

Yüklediğinizde veya yükselttiğinizde StorSimple Snapshot Manager ek bileşenini Microsoft Yönetim Konsolu (MMC) için önce Microsoft Azure StorSimple cihaz ve ana bilgisayar sunucusu, düzgün bir şekilde yapılandırıldığından emin olun.

## <a name="configure-prerequisites"></a>Önkoşulları yapılandırma
Aşağıdaki adımlar, StorSimple Snapshot Manager'ı yüklemeden önce tamamlamanız gereken yapılandırma görevleri üst düzey bir genel bakış sağlar. Microsoft Azure StorSimple yapılandırmasını tamamlamak ve sistem gereksinimleri ve adım adım yönergeler dahil olmak üzere, kurulum bilgilerini görmek için [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

> [!IMPORTANT]
> Başlamadan önce gözden [dağıtım yapılandırma denetim listesi](storsimple-8000-deployment-walkthrough-u2.md#deployment-configuration-checklist) ve ve [dağıtım önkoşulları](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites) içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
> <br>
> 
> 

### <a name="before-you-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yüklemeden önce
1. Cihazınızı kutusundan çıkarma, bağlamak ve açıklandığı gibi Microsoft Azure StorSimple cihazı bağlayın [StorSimple 8100 cihazınızın yükleme](storsimple-8100-hardware-installation.md) veya [StorSimple 8600 cihazınızın yükleme](storsimple-8600-hardware-installation.md).
2. Ana bilgisayarınızda aşağıdaki işletim sistemlerinden birini çalıştırdığından emin olun:
   
   * Windows Server 2008 R2 (Windows 2008 R2 çalıştıran sunucular üzerinde ayrıca Windows Server 2008 SP1 ve Windows Management Framework 3.0 yüklemeniz gerekir)
   * Windows Server 2012
   * Windows Server 2012 R2
     
     StorSimple sanal cihaz için bir Microsoft Azure sanal makine konağı olmalıdır.
3. Tüm Microsoft Azure StorSimple yapılandırma gereksinimlerini karşıladığından emin olun. Ayrıntılar için Git [dağıtım önkoşulları](storsimple-8000-deployment-walkthrough-u2.md#deployment-prerequisites).
4. Aygıt ana bilgisayarına bağlanmak ve ilk yapılandırmasını gerçekleştirin. Ayrıntılar için Git [dağıtım adımları](storsimple-8000-deployment-walkthrough-u2.md#deployment-steps).
5. Cihaz üzerinde birimler oluşturun, bunları konağa atayın ve konak bağlayabilir ve bunları doğrulayın. StorSimple Snapshot Manager birimler aşağıdaki türlerini destekler:
   
   * Temel birimler
   * Basit birimler
   * Dinamik birimler
   * Yansıtılmış dinamik birimler (RAID 1)
   * Küme Paylaşılan Birimleri
     
     Birimler StorSimple cihazı veya StorSimple sanal cihazı oluşturma hakkında daha fazla bilgi için Git [adım 6: Birim oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume), [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Yeni bir StorSimple Snapshot Manager'ı yükleme
StorSimple Snapshot Manager'ı yüklemeden önce StorSimple sanal cihazını ve StorSimple cihaz üzerinde oluşturulan birimlere bağlı, başlatılır ve açıklanan şekilde biçimlendirilmiş olduğunu emin olun [önkoşulları yapılandırma](#configure-prerequisites).

> [!IMPORTANT]
> * StorSimple sanal cihaz için bir Microsoft Azure sanal makine konağı olmalıdır. 
> * Ana bilgisayar, Windows 2008 R2, Windows Server 2012 veya Windows Server 2012 R2 çalıştırmalıdır. Sunucunuz Windows Server 2008 R2 çalıştırıyorsa, Windows Server 2008 SP1 ve Windows Management Framework 3.0 yüklemeniz gerekir.
> * Cihaz için StorSimple Snapshot Manager bağlanabilmesi için önce bir StorSimple cihazı konağa iSCSI bağlantısı yapılandırmanız gerekir.

StorSimple Snapshot Manager'ın yeni yüklemesini tamamlamak için aşağıdaki adımları izleyin. Yükseltme yüklüyorsanız, Git [yükseltmek veya yeniden StorSimple Snapshot Manager](#upgrade-or-reinstall-storsimple-snapshot-manager).

* 1\. adım: StorSimple anlık görüntü Yöneticisi'ni yükleyin 
* 2\. adım: StorSimple Snapshot Manager bir cihaza bağlanma 
* 3\. adım: Cihaz bağlantısını doğrulayın 

### <a name="step-1-install-storsimple-snapshot-manager"></a>1\. adım: StorSimple anlık görüntü Yöneticisi'ni yükleyin
StorSimple Snapshot Manager'ı yüklemek için aşağıdaki adımları kullanın.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yüklemek için
1. StorSimple Snapshot Manager yazılımı indirin (Git [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220) Microsoft Download Center'daki) ve konakta yerel olarak kaydedin.
2. Dosya Gezgini'nde, sıkıştırılmış klasöre sağ tıklayın ve ardından **tümünü Ayıkla**.
3. İçinde **sıkıştırılmış ayıklamak (Zipped) klasörleri** penceresi içinde **bir hedef seçmeniz ve dosyaları ayıklayın** kutusuna yazın veya istediğiniz ayıklanacak dosya yoluna göz atın.
   
    > [!IMPORTANT]
    > StorSimple Snapshot Manager C: sürücüsüne yüklemesi gerekir.
    
4. Seçin **Show ayıklanan dosyaları tamamlandığında** onay kutusunu işaretleyin ve ardından **ayıklamak**.
   
    ![Dosyaları ayıkla iletişim kutusu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 
5. Ayıklama işlemi tamamlandıktan sonra hedef klasörünü açar. Hedef klasörde görünen uygulama Kurulum simgesini çift tıklatın.
6. Zaman **Kurulum başarılı** iletisi görüntülendikten sonra **Kapat**. StorSimple Snapshot Manager simgesi masaüstünüzde görmeniz gerekir.
   
    ![Masaüstü simgesi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>2\. adım: StorSimple Snapshot Manager bir cihaza bağlanma
StorSimple Snapshot Manager bir StorSimple cihazına bağlanmak için aşağıdaki adımları kullanın.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple Snapshot Manager bir cihaza bağlanmak için
1. Masaüstü StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. Pencereyi içeren bir **kapsam** bölmesinde, bir **sonuçları** bölmesinde ve bir **eylemleri** bölmesinde. 
   
    ![StorSimple Snapshot Manager kullanıcı arabirimi](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png)
   
   * **Kapsam** bölmesinde (sol bölme), düğüm bir ağaç yapısında düzenlenmiş bir listesini içerir. Bir görünüm veya o düğümle ilgili belirli verileri seçmek için bazı düğümleri genişletebilirsiniz. Genişletmek veya daraltmak için bir düğüm için ok simgesine tıklayın. Bir öğeyi sağ **kapsam** bölmesinde bu öğeye ilişkin kullanılabilir eylemler listesini görmek için.
   * **Sonuçları** bölmesinde (Orta bölme) düğümü, görünümü veya seçtiğiniz verileri hakkında ayrıntılı durum bilgisi içeren **kapsam** bölmesi.
   * **Eylemleri** bölmesi düğümü, görünümü veya seçtiğiniz veri gerçekleştirebileceğiniz işlemleri listeler **kapsam** bölmesi.
     
     StorSimple Snapshot Manager kullanıcı arabirimini eksiksiz bir açıklaması için bkz: [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).
2. İçinde **kapsam** bölmesinde sağ **cihazları** düğümünü ve ardından **bir cihaz yapılandırma**. **Bir cihaz yapılandırma** iletişim kutusu görüntülenir.
   
    ![Bir cihaz yapılandırma](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 
3. İçinde **cihaz** liste kutusunda, sanal cihaz ve Microsoft Azure StorSimple cihazı IP adresi seçin. İçinde **parola** metin kutusuna, Azure portalında cihaz için oluşturduğunuz StorSimple Snapshot Manager parolasını yazın. **Tamam** düğmesine tıklayın.
4. Tanımladığınız cihaz için StorSimple Snapshot Manager arar. Kullanılabilir cihazsa, StorSimple Snapshot Manager bağlantı ekler. Yapabilecekleriniz [cihaz bağlantısını doğrulayın](#to-verify-the-connection) bağlantı başarıyla eklendiğini doğrulamak için.
   
    Cihaz için herhangi bir nedenle kullanılamıyorsa, StorSimple Snapshot Manager bir hata iletisi döndürür. Tıklayın **Tamam** hata iletiyi kapatın ve ardından **iptal** kapatmak için **bir cihaz yapılandırma** iletişim kutusu.
5. Birim grubu yedeklemeleri ilişkili şartıyla, StorSimple Snapshot Manager bir aygıta bağlandığında, bu cihaz için yapılandırılmış her bir birim grubu içeri aktarır. İlişkili yedekleme olmayan birim gruplarını içeri aktarılmaz. Ayrıca, bir birim grubu için oluşturulan yedekleme ilkelerini içeri aktarılmaz. İçeri aktarılan gruplar görmek için en üstteki sağ **birim gruplarını** düğümünde **kapsam** bölmesinde seçeneğine tıklayıp **içeri aktarılan gruplar geçiş**.

### <a name="step-3-verify-the-connection-to-the-device"></a>3\. adım: Cihaz bağlantısını doğrulayın
StorSimple Snapshot Manager StorSimple cihazına bağlı olduğunu doğrulamak için aşağıdaki adımları kullanın.

#### <a name="to-verify-the-connection"></a>Bağlantıyı doğrulamak için
1. İçinde **kapsam** bölmesinde tıklayın **cihazları** düğümü.
   
    ![StorSimple Snapshot Manager cihaz durumu](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 
2. Denetleme **sonuçları** bölmesi: 
   
   * Cihaz simgesinde yeşil bir göstergesi görünüyorsa ve **kullanılabilir** görünür **durumu** sütun, ardından cihaz bağlı. 
   * Cihaz simgesi üzerinde kırmızı bir göstergesi görünür ve kullanılamıyor görünür **durumu** sütun, ardından cihaz bağlı değil. 
   * Varsa **yenilemek** görünür **durumu** sütun ve StorSimple Snapshot Manager alınırken birim gruplarını ve bağlı bir cihaz için ilişkili yedekler.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı yeniden yükleyin veya yükseltin
Yükseltme veya yazılımı yeniden önce StorSimple Snapshot Manager tamamen kaldırmanız gerekir. 

StorSimple Snapshot Manager yeniden yüklemeden önce ana bilgisayarda mevcut StorSimple Snapshot Manager veritabanını yedekleyin. Bu yedekleme ilkeleri ve yapılandırma bilgilerini kaydeder, böylece bu verileri kolayca yedekten geri yükleyebilirsiniz.

Yükseltme veya yeniden yüklemeyi StorSimple Snapshot Manager aşağıdaki adımları izleyin:

* 1\. adım: StorSimple Snapshot Manager'ı kaldırın 
* 2\. adım: StorSimple Snapshot Manager veritabanını yedekleme 
* 3\. adım: StorSimple Snapshot Manager'ı yeniden yükleyin ve veritabanını geri yükleyin 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>1\. adım: StorSimple Snapshot Manager'ı kaldırın
StorSimple Snapshot Manager'ı kaldırmak için aşağıdaki adımları kullanın.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'ı kaldırmak için
1. Ana bilgisayarda açın **Denetim Masası**, tıklayın **programlar**ve ardından **programlar ve Özellikler**.
2. Sol bölmede **Kaldır veya Değiştir bir program**.
3. Sağ **StorSimple Snapshot Manager**ve ardından **kaldırma**.
4. Bu, StorSimple Snapshot Manager Kurulum programını başlatır. Tıklayın **Kurulumu Değiştir**ve ardından **kaldırma**.
   
   > [!NOTE]
   > Arka planda çalışan hiçbir MMC işlem varsa, StorSimple Snapshot Manager veya Disk Yönetimi gibi kaldırma başarısız olur ve program Kaldırmaya çalışmadan önce MMC tüm örneklerini kapatın isteyen bir ileti alırsınız. Seçin **otomatik olarak uygulamaları kapatın ve Kurulum tamamlandıktan sonra yeniden başlatmayı dene**ve ardından **Tamam**.
   > 
   > 
5. Kaldırma işlemi tamamlandığında bir **Kurulum başarılı** iletisi görüntülenir. **Kapat**’a tıklayın.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>2\. adım: StorSimple Snapshot Manager veritabanını yedekleme
Oluşturmak ve StorSimple Snapshot Manager veritabanının bir kopyasını kaydetmek için aşağıdaki adımları kullanın.

#### <a name="to-back-up-the-database"></a>Veritabanını yedeklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   3. Üzerinde **Hizmetleri** sayfasında **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **Hizmeti Durdur**.
      
        ![StorSimple cihaz Yöneticisi hizmetini durdurun](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)
2. C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
  
3. Katalog XML dosyasını bulun, dosyasını kopyalayın ve kopyayı güvenli bir konumda veya bulutta depolamak.
   
    ![StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   2. Üzerinde **Hizmetleri** sayfasında **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **hizmetini yeniden**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>3\. adım: StorSimple Snapshot Manager'ı yeniden yükleyin ve veritabanını geri yükleyin
StorSimple Snapshot Manager'ı yeniden yüklemek için adımları izleyin. [yeni bir StorSimple Snapshot Manager'ı yükleme](#install-a-new-storsimple-snapshot-manager). Ardından, StorSimple Snapshot Manager veritabanını geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="to-restore-the-database"></a>Veritabanını geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   3. Üzerinde **Hizmetleri** sayfasında **Microsoft StorSimple Yöneticisi hizmeti**.
   4. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **Hizmeti Durdur**.
2. C:\ProgramData\Microsoft\StorSimple\BACatalog için göz atın.
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını silin ve daha önce kaydettiğiniz bir sürümle değiştirin.
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda üzerinde **Araçları** menüsünde **Hizmetleri**.
   2. Üzerinde **Hizmetleri** sayfasında **Microsoft StorSimple Yöneticisi hizmeti**.
   3. Sağ bölmede altında **Microsoft StorSimple Yöneticisi hizmeti**, tıklayın **hizmetini yeniden**.

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple Snapshot Manager hakkında daha fazla bilgi için Git [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md).
* StorSimple Snapshot Manager kullanıcı arabirimi hakkında daha fazla bilgi için şuraya gidin [StorSimple Snapshot Manager kullanıcı arabirimini](storsimple-use-snapshot-manager.md).
* StorSimple Snapshot Manager'ı kullanma hakkında daha fazla bilgi edinmek için Git [StorSimple çözümünüzü yönetmek için StorSimple anlık görüntü Yöneticisi'ni kullanın](storsimple-snapshot-manager-admin.md).

