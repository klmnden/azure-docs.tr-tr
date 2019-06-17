---
title: Veri çubuğunda StorSimple 8000 serisi cihaz için 5000-7000 Serisi geçirme | Microsoft Docs
description: Bir genel bakış ve önkoşullar geçiş özelliği sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2018
ms.author: alkohli
ms.openlocfilehash: 967c03f3c4201bdcf1529fdda93717b6eb74e771
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60631683"
---
# <a name="migrate-data-from-storsimple-5000-7000-series-to-8000-series-device"></a>8000 serisi cihaz için StorSimple 5000-7000 serisinden veri geçirme

> [!IMPORTANT]
> - 31 Temmuz 2019, StorSimple 5000/7000 Serisi (EOS) destek durumu sonuna ulaşacak. StorSimple 5000/7000 Serisi müşteriler belgede açıklanan alternatifleri birine geçiş öneririz.
> - Geçişi şu anda destekli bir işlemdir. StorSimple 5000-7000 Serisi cihazınızın 8000 serisi cihazına veri geçirmeyi düşünüyorsanız, Microsoft Support geçiş zamanlamak gerekir. Microsoft Support sonra aboneliğiniz geçiş için etkinleştirir. Daha fazla bilgi için bkz. nasıl [bir destek bileti açın](storsimple-8000-contact-microsoft-support.md).
> - Hizmet isteği dosya sonra birkaç hafta Geçiş Planı yürütmek ve gerçekten geçişi başlatmak için beklemeniz gerekebilir.
> - Microsoft Support başvurmadan önce gözden geçirin ve tamamlamak mutlaka [geçiş önkoşulları](#migration-prerequisites) makalesinde belirtilen.

## <a name="overview"></a>Genel Bakış

Bu makalede sağlayan StorSimple 8000 serisi fiziksel cihazının verilerini veya bir 8010/8020 bulut Gereci geçirilecek StorSimple 5000-7000 Serisi müşterilere geçiş özelliği sunmaktadır. Bu makalede ayrıca bir 5000-7000 Serisi eski bir 8000 serisi fiziksel cihaza veya Bulut Gereci verileri geçirmek için gerekli adımları indirilebilir bir adım adım kılavuz bağlar.

Bu makalede, hem şirket içi 8000 serisi cihaz ek olarak StorSimple Cloud Appliance geçerlidir.


## <a name="migration-feature-versus-host-side-migration"></a>Geçiş özelliğini konak tarafı geçiş karşılaştırması

Geçiş özelliğini kullanarak verilerinizi taşıyabilir veya bir konak tarafı geçiş yaparak. Bu bölümde, her bir yöntemin Artıları ve eksileri dahil olmak üzere özelliklerini açıklar. Hangi yöntemini kullanmak, verileri geçirmek istediğiniz anlamak için bu bilgileri kullanın.

Geçiş özelliğini 8000 serisi için olağanüstü durum kurtarma (DR) işlemi 7000/5000 Series'den benzetimini yapar. Bu özellik 8000 serisi biçimine azure'da 5000/7000 Serisi biçimden veri geçirmenizi sağlar. Geçiş işlemi StorSimple geçiş aracı kullanılarak başlatılır. Araç, indirme ve yedekleme meta verilerinin dönüştürülmesi 8000 serisi Cihazınızı başlar ve sonra cihazdaki birimleri kullanıma sunmak için en son yedeklemesini kullanır.

|      | Uzmanları                                                                                                                                     |Simgeler                                                                                                                                                              |
|------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçiş işlemi 5000/7000 serisinin alınan yedeklemeler geçmişini korur.                                               | Bu geçiş kullanıcılar verilere erişmeye çalıştıklarında, verileri bu nedenle veri indirme maliyetler Azure'dan indirir.                                     |
| 2.   | Veri yok konak tarafta geçirilir.                                                                                                     | İşlemi, yedekleme ve en son yedekleme (Geçiş aracını kullanarak tahmin edilebilir) 8000 serisinin ortaya başlangıcı arasındaki kapalı kalma süresi gerekir. |
| 3.   | Bu işlem, tüm ilkeleri, bant genişliği şablonları, şifreleme ve diğer ayarları 8000 serisi cihazlarda korur.                      | Kullanıcı erişimini yalnızca kullanıcı tarafından erişilen verileri yeniden getirecek ve veri kümesinin tamamında yeniden doldurma değil.                                                  |
| 4.   | Bu işlem, zaman uyumsuz olarak üretim etkilemeden gerçekleştirilir azure'da tüm eski yedeklemeler dönüştürmek için ek zaman gerektirir. | Geçiş yalnızca bir bulut yapılandırma düzeyinde gerçekleştirilebilir.  Bir bulut yapılandırmasında tek birimleri ayrı olarak geçirilemez                       |

Bir konak tarafı geçiş 8000 serisi bağımsız olarak ayarlama ve veriler için 8000 serisi cihaz 5000/7000 Serisi CİHAZDAN kopyalama sağlar. Bunun eşdeğeri olan bir depolama cihazından başka bir geçirme veri. Çeşitli araçlar Diskboss, robocopy gibi verileri kopyalamak için kullanılır.

|      | Uzmanları                                                                                                                      |Simgeler                                                                                                                                                                                                      |
|------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.   | Geçiş aşamalı bir şekilde bir birim temelinde yaklaşıldığında.                                               | Önceki yedekler (5000/7000 serisinin taken) 8000 serisi Cihazınızı kullanılamaz.                                                                                                       |
| 2.   | Azure üzerinde bir depolama hesabına veri birleştirme olanak tanır.                                                       | İlk yedekleme 8000 serisinin bulutta Azure'a yedeklenmesi 8000 serisinin gereksinimlerini şirket tüm veriler daha uzun bir süre sürer.                                                                     |
| 3.   | Başarılı bir geçiş tüm verileri gerecinde yerel. Verilere erişirken hiçbir gecikmeleri vardır. | Azure depolama alanı tüketimi, veriler 5000/7000 CİHAZDAN silinene kadar artacaktır.                                                                                                        |
| 4.   |                                                                                                                           | Geçiş sırasında bu verilerin 7000/5000 Serisi cihaz büyük miktarda veri varsa, maliyetleri ve Azure'dan veri indirmeye ilgili gecikmeleri ödenmesini azure'dan indirilmesi gerekir. |

Bu makalede yalnızca 8000 serisi cihaz 5000/7000'den geçiş özelliğe odaklanır. Konak tarafı geçiş hakkında daha fazla bilgi için Git [geçiş diğer depolama cihazlarından](https://download.microsoft.com/download/9/4/A/94AB8165-CCC4-430B-801B-9FD40C8DA340/Migrating%20Data%20to%20StorSimple%20Volumes_09-02-15.pdf).

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

Eski 5000 veya 7000 Serisi Cihazınızı ve StorSimple 8000 serisi cihaz geçiş önkoşulları şunlardır.

> [!IMPORTANT]
> Gözden geçirin ve Microsoft Support olan hizmet isteği dosya önce geçiş önkoşulları tamamlayın.

### <a name="for-the-50007000-series-device-source"></a>5000/7000 Serisi cihaz (kaynak)

Geçişe başlamadan önce şunlardan emin olun:

* Bilgisayarınızda, 5000 veya cihaz 7000 Serisi kaynak; cihazın live veya aşağı.

    > [!IMPORTANT]
    > Geçiş süreci boyunca bu cihaz seri erişimi olmasını öneririz. Cihaz sorunları var olmalıdır, seri erişim gidermeye yardımcı olabilir.

* Yazılım sürümü v2.1.1.518 5000 veya 7000 Serisi kaynak cihazınız çalışıyor veya üzeri. Önceki sürümleri desteklenmez.
* 5000 veya 7000 Serisi çalıştığı sürümünü doğrulamak için Web kullanıcı Arabirimi sağ üst köşesinde arayın. Bu, cihazınızın çalışan yazılım sürümü görüntülemelidir. Geçiş için 5000 veya 7000 Serisi v2.1.1.518 çalıştırıyor olmalıdır.

    ![Eski cihaz üzerinde yazılım sürümü](media/storsimple-8000-migrate-from-5000-7000/check-version-legacy-device1.png)

    * Canlı Cihazınızı v2.1.1.518 çalışmıyor ya da daha sonra lütfen sisteminiz için gerekli en düşük sürümü yükseltin. Ayrıntılı yönergeler için başvurmak [v2.1.1.518 için sisteminizi yükseltin](http://onlinehelp.storsimple.com/111_Appliance/6_System_Upgrade_Guides/Current_(v2.1.1)/000_Software_Patch_Upgrade_Guide_v2.1.1.518).
    * V2.1.1.518 çalıştırıyorsanız, web kullanıcı Arabirimi için kayıt defteri geri yükleme hatalarının herhangi bir bildirim olup olmadığını görmek için gidin. Kayıt defteri geri yükleme işlemi başarısız oldu, kayıt defterini geri yüklemeyi çalıştırın. Nasıl daha fazla bilgi için Git [kayıt defterini geri yüklemeyi Çalıştır](http://onlinehelp.storsimple.com/111_Appliance/2_User_Guides/1_Current_(v2.1.1)/1_Web_UI_User_Guide_WIP/2_Configuration/4_Cloud_Accounts/1_Cloud_Credentials#Restoring_Backup_Registry).
    * V2.1.1.518 çalışmıyordu aşağı bir cihazınız varsa v2.1.1.518 çalıştıran yeni bir cihaza bir yük devretme gerçekleştirin. 5000/7000 serisinin StorSimple cihazınızın DR için ayrıntılı yönergeler için bkz.
    * Bir bulut anlık görüntüsünü alarak cihazınız için verileri yedekleyin.
    * Kaynak cihazda çalışmakta olan tüm diğer etkin yedekleme işlerini denetleyin. Bu StorSimple veri koruma konsol konağı işleri içerir. Geçerli işlerinin tamamlanmasını bekleyin.


### <a name="for-the-8000-series-physical-device-target"></a>8000 serisi fiziksel cihazının (hedef)

Geçişe başlamadan önce şunlardan emin olun:

* Hedef 8000 serisi Cihazınızı, kayıtlı ve çalışır durumdadır. Daha fazla bilgi için bkz. nasıl [StorSimple Yöneticisi hizmeti ile StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md).
* 8000 serisi Cihazınızı en son StorSimple 8000 serisi güncelleştirme 5'in yüklü olan ve 6.3.9600.17845 veya sonraki bir sürümü çalışıyor. Cihazınızı en son güncelleştirmelerin yüklü olmaması durumunda, yükseltmeye devam etmeden önce en son güncelleştirmeleri yüklemeniz gerekir. Daha fazla bilgi için bkz. nasıl [8000 serisi Cihazınızı en son güncelleştirmeyi yükleme](storsimple-8000-install-update-5.md).
* Azure aboneliğinizi geçiş için etkin. Aboneliğinizin etkin değilse, geçiş için aboneliğinizi etkinleştirmek için Microsoft Support başvurun.

### <a name="for-the-80108020-cloud-appliance-target"></a>8010/8020 bulut Gereci için (hedef)

Geçişe başlamadan önce emin olun:

* Hedef bulut gereciniz, kayıtlı ve çalışır durumdadır. Daha fazla bilgi için bkz. nasıl [Dağıt ve StorSimple bulut Gereci yönetme](storsimple-8000-cloud-appliance-u2.md).
* En son StorSimple 8000 serisi güncelleştirme 5 yazılım sürümü 6.3.9600.17845 cloud appliance'ınız çalışıyor. Bulut gereciniz güncelleştirme 5 çalışmıyorsa geçirme işlemine devam etmeden önce yeni güncelleştirme 5 bulut gerecini oluşturun. Daha fazla bilgi için bkz. nasıl [8010/8020 bulut Gereci oluşturma](storsimple-8000-cloud-appliance-u2.md).

### <a name="for-the-computer-running-storsimple-migration-tool"></a>StorSimple geçiş aracını çalıştıran bir bilgisayar için

StorSimple geçiş aracı, 5000-7000 Serisi için 8000 serisi bir cihaza bir Storsimple'dan verileri geçirmek için sağlayan bir kullanıcı Arabirimi tabanlı bir araçtır. StorSimple geçiş aracını yüklemek için aşağıdaki gereksinimleri karşılayan bir bilgisayar kullanın.

Bilgisayarın Internet bağlantısı olduğundan ve:

* Aşağıdaki işletim sistemi çalıştıran
    * Windows 10.
    * Windows Server 2012 R2 (veya üzeri) StorSimple geçiş aracını yüklemek için.
* .NET 4.5.2'nin yükledi.
* En az 5 GB boş alan aracını yükleme ve kullanma için vardır.

> [!TIP]
> StorSimple cihazınız için bir Windows Server konağının bağlıysa, Windows Server ana bilgisayarında geçiş aracı yükleyebilirsiniz.

#### <a name="to-install-storsimple-migration-tool"></a>StorSimple geçiş aracını yüklemek için

StorSimple geçiş aracı bilgisayarınıza yüklemek için aşağıdaki adımları gerçekleştirin.

1. Klasörü kopyalama _StorSimple8000SeriesMigrationTool_ Windows bilgisayarınıza. Yazılım kopyalandığı sürücüsünde yeterli alan olduğunu doğrulayın.

    Aracı yapılandırma dosyasını Aç _StorSimple8000SeriesMigrationTool.exe.config_ klasöründe. Dosyanın bir parçacığı aşağıda verilmiştir.
    
    ```xml
        <add key="UserName" value="username@xyz.com" />
        <add key="SubscriptionName" value="YourSubscriptionName" />
        <add key="SubscriptionId" value="YourSubscriptionId" />
        <add key="TenantId" value="YourTenantId" />
        <add key="ResourceName" value="YourResourceName" />
        <add key="ResourceGroupName" value="YourResourceGroupName" />

    ```
2. Anahtarlar için karşılık gelen değerleri düzenleyin ve değiştirin:

    * `UserName` – Azure portalında oturum açmak için kullanıcı adı.
    * `SubscriptionName and SubscriptionId` – Adı ve kimliği Azure aboneliğiniz için. StorSimple cihaz Yöneticisi hizmetinize giriş sayfası, altında **genel**, tıklayın **özellikleri**. Abonelik adı kopyalayın ve abonelik kimliği, hizmetiniz ile ilişkili.
    * `ResourceName` – Azure portalında StorSimple cihaz Yöneticisi hizmetinizin adını. Ayrıca hizmet özellikleri altında gösterilir.
    * `ResourceGroup` – Azure portalında StorSimple cihaz Yöneticisi hizmetiniz ile ilişkili kaynak grubunun adı. Ayrıca hizmet özellikleri altında gösterilir.
    ![Hedef cihaz için onay hizmet özellikleri](media/storsimple-8000-migrate-from-5000-7000/check-service-properties1.png)
    * `TenantId` – Azure Active Directory Kiracı Kimliğini Azure portalında. Microsoft Azure için bir yönetici olarak oturum açın. Microsoft Azure portalında **Azure Active Directory**. Altında **Yönet**, tıklayın **özellikleri**. Kiracı kimliği görüntülenir **dizin kimliği** kutusu.
    ![İçin Azure Active Directory Kiracı kimliği denetle](media/storsimple-8000-migrate-from-5000-7000/check-tenantid-aad.png)

3.  Yapılandırma dosyasına yapılan değişiklikleri kaydedin.
4.  Çalıştırma _StorSimple8000SeriesMigrationTool.exe_ aracını başlatmak için. Kimlik bilgileri istendiğinde, Azure portalında aboneliğinizle ilişkili kimlik bilgilerini sağlayın. 
5.  StorSimple Geçiş Aracı kullanıcı Arabirimi görüntülenir.
  

## <a name="next-steps"></a>Sonraki adımlar
Adım adım yönergeler için karşıdan yükleme [veri bir StorSimple 5000-7000 Serisi 8000 serisi bir cihaza geçirme](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b).
