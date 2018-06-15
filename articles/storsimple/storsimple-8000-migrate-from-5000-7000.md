---
title: Verileri StorSimple 8000 serisi cihaza 5000-7000 Serisi geçirmek | Microsoft Docs
description: Genel bir bakış ve geçiş özelliğini Önkoşullar sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/11/2017
ms.author: alkohli
ms.openlocfilehash: 36df62c4b01c623702707d39c6af59f4752ee6e0
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
ms.locfileid: "26639876"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>8000 serisi aygıta StorSimple 5000-7000 Serisi veri geçirme

> [!IMPORTANT]
> - Geçiş şu anda Yardımlı bir işlemdir. StorSimple 5000-7000 Serisi aygıtınızdan 8000 serisi cihazına veri geçirmek istiyorsanız, Microsoft Support ile geçiş zamanlama gerekir. Microsoft Support sonra aboneliğinizi geçiş için olanak sağlar. Daha fazla bilgi için bkz: nasıl yapılır [bir destek bileti açmadan](storsimple-8000-contact-microsoft-support.md).
> - Hizmet isteği dosya sonra birkaç geçiş planı çalıştırıp gerçekte geçişi başlatmak için hafta sürebilir.
> - Microsoft Support başvurmadan önce aws'de emin ve eksiksiz olması [geçiş Önkoşullar](#migration-prerequisites) makalesinde belirtilen.

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple 8000 serisi fiziksel aygıt verilerini veya bir 8010/8020 bulut uygulaması geçirmek StorSimple 5000-7000 Serisi müşterilerin sağlayan Geçiş özelliği sunmaktadır. Bu makalede ayrıca bir 5000-7000 Serisi eski 8000 serisi bir fiziksel cihaza veya veri bulut uygulaması geçirmek için gerekli adımlar indirilebilir bir adım adım kılavuz bağlar.

Bu makalede, hem şirket içi 8000 serisi aygıt yanı sıra StorSimple bulut uygulaması için geçerlidir.


## <a name="migration-feature-versus-host-side-migration"></a>Geçiş özelliğini konak tarafı geçiş karşılaştırması

Geçiş özelliğini kullanarak verilerinizi taşıyabilirsiniz veya bir ana bilgisayar tarafı geçiş yaparak. Bu bölümde Artıları ve eksileri dahil olmak üzere her bir yöntemin özelliklerini açıklar. Hangi yöntemi kullanmak, verilerini geçirmek istediğiniz anlamak için bu bilgileri kullanın.

Geçiş özelliğini 8000 serisi için olağanüstü durum kurtarma (DR) işlemi 7000/5000 serisinden benzetimini yapar. Bu özellik, Azure üzerinde 8000 serisi biçimine 5000/7000 Serisi biçiminden veri geçirmenize olanak sağlar. Geçiş işlemi StorSimple geçiş aracı kullanılarak başlatılır. Aracı karşıdan yüklenmesini ve yedekleme meta verilerinin dönüştürülmesi 8000 serisi cihazda başlar ve ardından cihaz birimlerde kullanıma sunmak için en son yedekleme kullanır.

|      | Uzmanları                                                                                                                                     |Simgeler                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçiş işlemi 5000/7000 Serisi üzerinde gerçekleştirilen yedeklemeler geçmişini korur.                                               | Kullanıcılar verilere erişmeye çalıştığında, bu geçiş veri böylece veri indirme maliyetleri doğurmasını Azure'dan indirir.                                     |
| 2.   | Veri yok ana bilgisayar tarafında geçirilir.                                                                                                     | İşlemi, yedekleme ve son yedekleme (Geçiş aracını kullanarak tahmin edilebilir) üzerinde 8000 series ortaya başlangıcı arasındaki kapalı kalma süresi gerekir. |
| 3.   | Bu işlem, tüm ilkelerin, bant genişliği şablonları, şifreleme ve diğer ayarları 8000 serisi cihazlar üzerinde korur.                      | Kullanıcı erişimi yalnızca kullanıcılar tarafından erişilen verileri yeniden getirecek ve tüm veri kümesinin rehydrate değil.                                                  |
| 4.   | Bu işlem, zaman uyumsuz olarak üretim etkilemeden yapılır Azure tüm eski yedeklemeler dönüştürmek için ek süre gerektirir. | Geçiş yalnızca bir bulut yapılandırma düzeyinde yapılabilir.  Bir bulut yapılandırmasında ayrı birimleri ayrı olarak geçirilemez                       |

Bir ana bilgisayar tarafı geçiş 8000 serisi bağımsız olarak ayarlama ve veri 5000/7000 Serisi aygıttan 8000 serisi cihaza kopyalama sağlar. Bu eşdeğer olan başka bir depolama aygıtından verileri geçirme. Çeşitli araçları Diskboss, robocopy gibi verileri kopyalamak için kullanılır.

|      | Uzmanları                                                                                                                      |Simgeler                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçiş aşamalı bir şekilde bir birim birim temelinde yaklaşıldığında.                                               | Önceki yedeklemelerin (5000/7000 Serisi taken) 8000 serisi cihazda kullanılabilir olmaz.                                                                                                       |
| 2.   | Veri birleştirme Azure üzerinde bir depolama hesabı uygulamasına olanak sağlar.                                                       | 8000 serisi bulutta ilk yedekleme Azure'a yedeklenecek 8000 serisi gereksinimlerinize tüm veriler daha uzun sürede sürer.                                                                     |
| 3.   | Başarılı bir geçiş tüm verileri aygıtındaki yerel. Verilere erişirken hiçbir gecikmeleri vardır. | Azure depolama alanı tüketimi veri 5000/7000 aygıttan silinene kadar artacaktır.                                                                                                        |
| 4.   |                                                                                                                           | 7000/5000 Serisi aygıtı büyük miktarda veri varsa, geçiş sırasında bu veriler, maliyetleri ve Azure'dan veri indirme için ilgili gecikmeleri neden Azure indirilmesi gerekir |

Bu makalede yalnızca 5000/7000'den geçiş özelliğini 8000 serisi aygıta odaklanır. Konak tarafındaki geçiş hakkında daha fazla bilgi için Git [diğer depolama aygıtlarından geçiş](http://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating Data to StorSimple Volumes_09-02-15.pdf).

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

Burada, eski 5000 veya 7000 Serisi Cihazınızı ve 8000 serisi StorSimple cihaz geçiş önkoşulları bulunmaktadır.

> [!IMPORTANT]
> Gözden geçirin ve Microsoft Support olan hizmet isteği dosya yapmadan önce geçiş önkoşulları tamamlamalısınız.

### <a name="for-the-50007000-series-device-source"></a>5000/7000 Serisi aygıt (kaynak)

Geçişe başlamadan önce emin olun:

* 5000 sahip veya aygıt 7000 Serisi kaynak; cihaz olabilir Canlı veya aşağı.

    > [!IMPORTANT]
    > Geçiş işlemi boyunca bu cihaz seri erişimi olmasını öneririz. Cihaz sorunları var olmalıdır, seri erişim sorun gidermenize yardımcı olabilir.

* 5000 veya 7000 Serisi kaynak Cihazınızı yazılım sürüm v2.1.1.518 çalışıyor veya sonraki bir sürümü. Önceki sürümlerinde desteklenmez.
* 5000 veya 7000 Serisi çalıştığı sürüm doğrulamak için Web kullanıcı arabirimini sağ üst köşesinde bakın. Bu, Cihazınızı çalışan yazılım sürümü görüntülemesi gerekir. Geçiş için 5000 veya 7000 Serisi v2.1.1.518 çalıştırıyor olması gerekir.

    ![Eski aygıt yazılım sürümünü denetleyin](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Canlı Cihazınızı v2.1.1.518 çalışmıyor ya da daha sonra sisteminiz için gerekli en düşük sürümü Lütfen yükseltin. Ayrıntılı yönergeler için bkz [v2.1.1.518 için sisteminizi yükseltin](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
    * V2.1.1.518 çalıştırıyorsanız, web kayıt defterini geri yükleme hataları için herhangi bir bildirim olup olmadığını görmek için kullanıcı Arabirimi gidin. Kayıt defteri geri yükleme başarısız oldu, kayıt defterini geri yükleme çalıştırın. Nasıl daha fazla bilgi için Git [kayıt defterini geri yüklemeyi Çalıştır](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry).
    * V2.1.1.518 çalıştırmayan bir aşağı cihazınız varsa, v2.1.1.518 çalıştıran bir değiştirme cihaza bir yük devretme gerçekleştirin. 5000/7000 Serisi StorSimple Cihazınızı DR için ayrıntılı yönergeler için bkz.
    * Bir bulut anlık cihazınız için verileri yedekleyin.
    * Kaynak cihazda çalışan tüm diğer etkin yedekleme işlerini denetleyin. Bu, StorSimple veri koruma Konsolu konak işlere içerir. Geçerli işlerinin tamamlanmasını bekleyin.


### <a name="for-the-8000-series-physical-device-target"></a>8000 serisi fiziksel aygıt (hedef)

Geçişe başlamadan önce emin olun:

* Hedef 8000 serisi cihazınız kayıtlı ve çalışır durumdadır. Daha fazla bilgi için bkz: nasıl yapılır [StorSimple Cihazınızı StorSimple Yöneticisi hizmetiyle dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
* 8000 serisi Cihazınızı son StorSimple 8000 serisi güncelleştirme 5 yüklüyse sahiptir ve 6.3.9600.17845 veya sonraki bir sürümünü çalıştıran. Cihazınızı son güncelleştirmelerin yüklü yoksa geçirme işlemine devam etmeden önce en son güncelleştirmeleri yüklemeniz gerekir. Daha fazla bilgi için bkz: nasıl yapılır [8000 serisi aygıtınızda en son güncelleştirmesini yükleyin](storsimple-8000-install-update-5.md).
* Azure aboneliğiniz için geçiş etkinleştirilir. Aboneliğinizi etkin değilse, geçiş için aboneliğinizi etkinleştirmek için Microsoft Support başvurun.

### <a name="for-the-80108020-cloud-appliance-target"></a>8010/8020 bulut uygulaması (hedef)

Geçişe başlamadan önce emin olun:

* Hedef bulut uygulaması kayıtlı ve çalışır durumdadır. Daha fazla bilgi için bkz: nasıl yapılır [dağıtma ve StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md).
* Bulut uygulaması en son StorSimple 8000 serisi güncelleştirme 5 yazılım sürüm 6.3.9600.17845 çalışıyor. Bulut uygulaması güncelleştirme 5 çalışmıyorsa geçirme işlemine devam etmeden önce yeni bir güncelleştirme 5 bulut uygulaması oluşturun. Daha fazla bilgi için bkz: nasıl yapılır [8010/8020 bulut uygulaması oluşturma](storsimple-8000-cloud-appliance-u2.md).

### <a name="for-the-computer-running-storsimple-migration-tool"></a>StorSimple geçiş aracını çalıştıran bilgisayar için

StorSimple geçiş aracı, 5000-7000 Serisi 8000 serisi aygıtına StorSimple verileri geçirmek için sağlayan bir UI tabanlı bir araçtır. StorSimple Geçiş Aracı yüklemek için aşağıdaki gereksinimleri karşılayan bir bilgisayar kullanın.

Bilgisayarın Internet bağlantısı olduğunu ve:

* Aşağıdaki işletim sistemi çalıştıran
    * Windows 10.
    * Windows Server 2012 R2 (veya sonrası) StorSimple Geçiş Aracı yüklemek için.
* .NET 4.5.2 yükledi.
* Aracı yükleme ve kullanma için 5 GB boş disk alanı en az sahiptir.

> [!TIP]
> StorSimple Cihazınızı bir Windows Server ana bilgisayara bağlı değilse, Geçiş Aracı Windows Server ana bilgisayara yükleyebilirsiniz.

#### <a name="to-install-storsimple-migration-tool"></a>StorSimple Geçiş Aracı yüklemek için

StorSimple geçiş aracı bilgisayarınıza yüklemek için aşağıdaki adımları gerçekleştirin.

1. Klasörü kopyalamak _StorSimple8000SeriesMigrationTool_ Windows bilgisayarınıza. Yazılım burada kopyalanır sürücü yeterli alana sahip olduğundan emin olun.

    Aracı yapılandırma dosyasını açın _StorSimple8000SeriesMigrationTool.exe.config_ klasöründe. Dosya parçacığı aşağıda verilmiştir.
    
    ```
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Anahtarlara karşılık gelen değerleri düzenleyin ve ile değiştirin:

    * `UserName`– Azure portalında oturum açmak için kullanıcı adı.
    * `SubscriptionName and SubscriptionId`– Adı ve kimliği Azure aboneliğiniz için. StorSimple Aygıt Yöneticisi'ni hizmetinizi giriş sayfasında, altında **genel**, tıklatın **özellikleri**. Abonelik adı kopyalayın ve abonelik kimliği hizmetiniz ile ilişkili.
    * `ResourceName`– StorSimple Aygıt Yöneticisi'ni hizmetinizi Azure portalındaki adı. Ayrıca hizmet özellikleri altında gösterilir.
    * `ResourceGroup`– Azure portalında, StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili kaynak grubunun adı. Ayrıca hizmet özellikleri altında gösterilir.
    ![Hedef cihaz için onay hizmet özellikleri](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId`– Azure Active Directory Kiracı kimliği Azure portalında. Microsoft Azure yönetici olarak oturum açın. Microsoft Azure Portalı'nda tıklatın **Azure Active Directory**. Altında **Yönet**, tıklatın **özellikleri**. Kimliği görüntülenir Kiracı **dizin kimliği** kutusu.
    ![Kiracı kimliği Azure Active Directory için denetleyin](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png)

3.  Yapılandırma dosyasına yapılan değişiklikleri kaydedin.
4.  Çalıştırma _StorSimple8000SeriesMigrationTool.exe_ araç başlatılamıyor. Kimlik bilgileri istendiğinde, Azure portalında aboneliğinizle ilişkili kimlik bilgilerini sağlayın. 
5.  StorSimple Geçiş Aracı kullanıcı Arabirimi görüntülenir.
  

## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeler için karşıdan [veri bir StorSimple 8000 serisi cihazına 5000-7000 Serisi geçirmek](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b).
