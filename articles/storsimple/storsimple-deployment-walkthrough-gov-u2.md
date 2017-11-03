---
title: "Kamu portalında StorSimple Cihazınızı dağıtma | Microsoft Docs"
description: "StorSimple güncelleştirme 2 cihazını ve hizmetini Azure kamu portalında dağıtmak için en iyi yöntemler ve adımları açıklar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 5277538c-797e-4e8e-b613-31b5c10cf5a9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/16/2016
ms.author: v-sharos
ms.openlocfilehash: 0b22dcdfc0432533b286e70d130bfe2ee2db92b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal-update-2"></a>Kamu portalında (güncelleştirme 2) şirket içi StorSimple Cihazınızı dağıtma
[!INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple cihaz dağıtımına hoş geldiniz. Bu dağıtım öğreticileri StorSimple 8000 serisi güncelleştirme 2 yazılım Azure kamu Portalı'nda çalışan uygulanır. Bu dizi öğreticiler bir yapılandırma denetim listesi, yapılandırma önkoşulları ve StorSimple cihazınız için ayrıntılı yapılandırma adımlarını listesini içerir.

Bu öğreticilerdeki bilgiler, güvenlik önlemlerini gözden geçirdiğinizi ve StorSimple cihazınızı kutusundan çıkardığınızı, yerleştirdiğinizi ve kablolarını taktığınızı varsayar. Bu görevleri henüz gerçekleştirmediyseniz, [güvenlik önlemlerini](storsimple-safety.md) inceleyerek başlayın. Cihazınızı açmak, rafa monte etmek ve kablo bağlantısını yapmak için cihaza özel yönergeleri izleyin.

* [8100 model cihazınızı kutusundan çıkarma, rafa takma ve kablolarını bağlama](storsimple-8100-hardware-installation.md)
* [8600 model cihazınızı kutusundan çıkarma, rafa takma ve kablolarını bağlama](storsimple-8600-hardware-installation.md)

Kurulum ve yapılandırma işlemini tamamlamak için yönetici ayrıcalıkları gerekir. Başlamadan önce yapılandırma denetim listesini gözden geçirmenizi öneririz. Dağıtım ve yapılandırma işleminin tamamlanması biraz zaman alabilir.

> [!NOTE]
> Microsoft Azure Web sitesinde yayınlanan StorSimple dağıtım bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. 7000 serisi cihazlar hakkında tam bilgi için şu adresi ziyaret edin: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 serisi dağıtım bilgileri için bkz. [StorSimple Sistemi Hızlı Başlangıç Kılavuzu](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>Dağıtım adımları
StorSimple cihazınızı yapılandırmak ve StorSimple Yöneticisi hizmetine bağlamak için gerekli adımları gerçekleştirin. Gerekli adımlara ek olarak, isteğe bağlı adımlar ve dağıtım sırasında tamamlamak için gereken yordamları vardır. Bu adım adım dağıtım yönergelerinde isteğe bağlı adımların her birini ne zaman gerçekleştirmeniz gerektiği belirtilmiştir.

| Adım | Açıklama |
| --- | --- |
| **ÖNKOŞULLAR** |Dağıtım için bu önkoşulların tamamlanması gerekir. |
| [Dağıtım yapılandırma denetim listesi](#deployment-configuration-checklist) |Dağıtımdan önce ve dağıtım sırasında bilgi toplamak ve bilgileri kaydetmek için bu denetim listesini kullanın. |
| [Dağıtım önkoşulları](#deployment-prerequisites) |Bunlar, ortamın dağıtım için hazır olduğunu doğrulayın. |
|  | |
| **ADIM ADIM DAĞITIM** |Bu adımlar, StorSimple cihazınızı üretim ortamına dağıtmak için gereklidir. |
| [1. Adım: Yeni bir hizmet oluşturun](#step-1-create-a-new-service) |StorSimple cihazınız için bulut yönetimi ve depolamayı ayarlayın. *Başka StorSimple cihazlar için bir hizmetiniz varsa, bu adımı atlayın*. |
| [2. Adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key) |Kaydolun ve StorSimple Cihazınızı yönetim hizmetine bağlamak için bu anahtarı kullanın. |
| [3. Adım: StorSimple için Windows PowerShell üzerinden cihazı yapılandırın ve kaydedin](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |Yönetim hizmetini kullanarak kurulumu tamamlamak için cihazı ağınıza bağlayın ve Azure ile kaydedin. |
| [4. adım: minimum cihaz kurulumunu tamamlayın](#step-4-complete-minimum-device-setup) </br>İsteğe bağlı: StorSimple cihazınızı güncelleştirin. |Depolama alanı sağlamak için, yönetim hizmetini kullanarak cihaz kurulumunu tamamlayın ve etkinleştirin. |
| [5. Adım: Birim kapsayıcısı oluşturun](#step-5-create-a-volume-container) |Birimleri sağlamak için bir kapsayıcı oluşturun. Birim kapsayıcısı, kapsadığı tüm birimler için depolama hesabı, bant genişliği ve şifreleme ayarlarını içerir. |
| [6. Adım: Birim oluşturun](#step-6-create-a-volume) |Sunucularınız için StorSimple cihazında depolama birimleri sağlayın. |
| [7. Adım: Bir birimi bağlayın, başlatın ve biçimlendirin](#step-7-mount-initialize-and-format-a-volume) </br>İsteğe bağlı: MPIO’yu yapılandırın. |Sunucularınızı cihaz tarafından sağlanan iSCSI depolama alanına bağlayın. İsteğe bağlı olarak, sunucularınızın bağlantı, ağ ve arabirim hatalarından etkilenmemesini sağlamak için MPIO'yu yapılandırın. |
| [8. Adım: Yedekleyin](#step-8-take-a-backup) |Verilerinizi korumak için yedekleme ilkenizi ayarlayın |
|  | |
| **DİĞER YORDAMLAR** |Çözümünüzü dağıtırken bu yordamlara başvurmanız gerekebilir. |
| [Hizmet için yeni bir depolama hesabı yapılandırın](#configure-a-new-storage-account-for-the-service) | |
| [Cihaz seri konsoluna bağlanmak için PuTTY kullanın](#use-putty-to-connect-to-the-device-serial-console) | |
| [İçin tarama ve güncelleştirmeleri uygulama](#scan-for-and-apply-updates) | |
| [Bir Windows Server konağının IQN’ini alın](#get-the-iqn-of-a-windows-server-host) | |
| [El ile yedekleme oluşturun](#create-a-manual-backup) | |
| [MPIO yapılandırma](#configure-mpio) | |

## <a name="deployment-configuration-checklist"></a>Dağıtım yapılandırma denetim listesi
StorSimple Cihazınızı dağıtmadan önce Cihazınızda yazılımı yapılandırmak amacıyla bilgi toplamanız gerekecektir. Bu bilgilerin bir bölümünü önceden hazırlamak, ortamınızda StorSimple cihazını dağıtma işlemini kolaylaştırmaya yardımcı olur. Karşıdan yükleyip Cihazınızı dağıtırken yapılandırma ayrıntılarını not etmek için bu denetim listesini kullanın.  

[StorSimple dağıtım yapılandırma denetim listesini indirme](http://www.microsoft.com/download/details.aspx?id=49159)  

## <a name="deployment-prerequisites"></a>Dağıtım önkoşulları
Aşağıdaki bölümlerde, StorSimple Yöneticisi hizmetiniz ve StorSimple cihazınız için yapılandırma önkoşulları açıklanmaktadır.

### <a name="for-the-storsimple-manager-service"></a>StorSimple Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* Erişim kimlik bilgilerine sahip bir Microsoft hesabınız var.
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.
* Microsoft Azure aboneliğiniz StorSimple Yöneticisi hizmeti için etkinleştirildi. Aboneliğinizin [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/) aracılığıyla satın alınmış olması gerekir.
* PuTTY gibi bir terminal öykünme yazılımına erişiminiz var.

### <a name="for-the-device-in-the-datacenter"></a>Veri merkezindeki cihaz için
Cihazı yapılandırmadan önce aşağıdakilerden emin olun:

* Cihazınız tam olarak açılmış, bir rafa monte edilmiş ve güç, ağ ve seri erişim için kablolar aşağıdaki şekilde tam olarak bağlanmış:
  
  * [8100 model cihazınızı kutusundan çıkarma, rafa takma ve kablolarını bağlama](storsimple-8100-hardware-installation.md)
  * [8600 model cihazınızı kutusundan çıkarma, rafa takma ve kablolarını bağlama](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce aşağıdakilerden emin olun:

* Veri merkezi güvenlik duvarınızdaki bağlantı noktaları iSCSI ve bulut trafiğine izin vermek için [StorSimple cihazınız için ağ gereksinimleri](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device) bölümünde belirtildiği şekilde açık.

## <a name="step-by-step-deployment"></a>Adım adım dağıtım
StorSimple cihazınızı veri merkezinde dağıtmak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-create-a-new-service"></a>1. Adım: Yeni bir hizmet oluşturun
Bir StorSimple Yöneticisi hizmeti birden çok StorSimple cihazını yönetebilir. StorSimple Yöneticisi hizmetinin yeni bir örneğini oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [!IMPORTANT]
> Hizmetinizle birlikte bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz, bir hizmeti başarıyla oluşturduktan sonra en az bir depolama hesabı oluşturmanız gerekir. Bir birim kapsayıcısı oluşturduğunuzda, bu depolama hesabı kullanılacaktır.
> 
> * Otomatik olarak bir depolama hesabı oluşturmadıysanız, ayrıntılı yönergeler için [Hizmet için yeni bir depolama hesabı yapılandırma](#configure-a-new-storage-account-for-the-service) bölümüne gidin.
> * Bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirdiyseniz, [2. Adım: Hizmet kayıt anahtarını alın](#step-2-get-the-service-registration-key) bölümüne gidin.
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>2. Adım: Hizmet kayıt anahtarını alın
StorSimple Yöneticisi hizmeti çalışır duruma geldikten sonra, hizmet kayıt anahtarını almanız gerekir. Bu anahtar kaydetmek ve StorSimple Cihazınızı hizmete bağlanmak için kullanılır.

Kamu portalında aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>3. Adım: StorSimple için Windows PowerShell üzerinden cihazı yapılandırın ve kaydedin
Aşağıdaki yordamda açıklandığı gibi StorSimple cihazınızın ilk kurulumu tamamlamak üzere StorSimple için Windows PowerShell kullanın. Bu adımı tamamlamak için bir terminal öykünme yazılımı kullanmanız gerekir. Daha fazla bilgi için bkz. [Cihaz seri konsoluna bağlanmak için PuTTY kullanma](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>4. Adım: Minimum cihaz kurulumunu tamamlayın
StorSimple cihazınız için en düşük cihaz yapılandırması için aşağıdakileri yapmanız gerekir:

* İkincil DNS sunucusunu ayarlayın.
* En az bir ağ arabiriminde iSCSI’ı etkinleştirin.
* İki denetleyiciye de sabit IP adresleri atayın.

Minimum cihaz kurulumunu tamamlamak için kamu Portalı'nda aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>5. Adım: Birim kapsayıcısı oluşturun
Birim kapsayıcısı, kapsadığı tüm birimler için depolama hesabı, bant genişliği ve şifreleme ayarlarını içerir. StorSimple cihazınızda birimleri sağlamaya başlamadan önce bir birim kapsayıcısı oluşturmanız gerekir.

Birim kapsayıcısı oluşturmak için kamu Portalı'nda aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>6. Adım: Birim oluşturun
Bir birim kapsayıcısı oluşturduktan sonra, sunucularınız için StorSimple cihazında bir depolama birimi sağlayabilirsiniz. Birim oluşturmak için kamu Portalı'nda aşağıdaki adımları gerçekleştirin.

> [!IMPORTANT]
> Azure StorSimple yalnızca ölçülü kaynak kullanan birimler oluşturabilir.  Tamamen sağlanan veya kısmen sağlanan oluşturulamıyor bir Azure StorSimple sistemi birimlerde.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>7. Adım: Bir birimi bağlayın, başlatın ve biçimlendirin
Windows Server ana bilgisayarında aşağıdaki adımları gerçekleştirin.

> [!IMPORTANT]
> * StorSimple çözümünüzün yüksek oranda kullanılabilirliğini sağlamak için, iSCSI’ı yapılandırmadan önce konak sunucularınızda (isteğe bağlı) MPIO’yu yapılandırmanızı öneririz. Konak sunucuları üzerinde MPIO yapılandırması sunucuların bağlantı, ağ veya arabirim hatalarına dayanabileceğinden emin olunmasını sağlar.
> * Windows Server konağında MPIO ve iSCSI yükleme ve yapılandırma yönergeleri için [StorSimple cihazınız için MPIO yapılandırma](storsimple-configure-mpio-windows-server.md) bölümüne gidin. Bu adımlar StorSimple birimlerini bağlamayı, başlatmayı ve biçimlendirmeyi de içerir.
> * Bir Linux konağında MPIO ve iSCSI yükleme ve yapılandırma yönergeleri için [StorSimple Linux konağınız için MPIO yapılandırma](storsimple-configure-mpio-on-linux.md) bölümüne gidin.
> 
> 

MPIO yapılandırmamaya karar verirseniz, bir Windows Server konağında StorSimple birimlerinizi bağlamak, başlatmak ve biçimlendirmek için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>8. Adım: Yedekleyin
Yedeklemeler, birimlerin zaman noktası korumasını sağlar ve geri yükleme sürelerini azaltırken kurtarılabilirliği iyileştirir. StorSimple cihazınızda iki tür yedekleme oluşturabilirsiniz: yerel anlık görüntüler ve bulut anlık görüntüleri. Bu yedekleme türlerinin her biri **Zamanlanmış** veya **El ile** olabilir.

Zamanlanmış yedekleme oluşturmak için kamu Portalı'nda aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

İstediğiniz zaman el ile yedekleme oluşturabilirsiniz. Yordamlar için, [El ile yedekleme oluşturun](#create-a-manual-backup) bölümüne gidin.

## <a name="configure-a-new-storage-account-for-the-service"></a>Hizmet için yeni bir depolama hesabı yapılandırın
Bu yalnızca hizmetinizle bir depolama hesabının otomatik olarak oluşturulmasını etkinleştirmediyseniz gerçekleştirmeniz gereken isteğe bağlı bir adımdır. StorSimple birim kapsayıcısı oluşturmak için bir Microsoft Azure Storage hesabı gereklidir.

Farklı bir bölgede bir Azure Storage hesabı oluşturmanız gerekiyorsa, adım adım yönergeler için bkz. [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md).

Kamu portalında aşağıdaki adımları gerçekleştirin **StorSimple Yöneticisi hizmeti** sayfası.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Cihaz seri konsoluna bağlanmak için PuTTY kullanın
StorSimple için Windows PowerShell’e bağlanmak için PuTTY gibi bir terminal öykünme yazılımı kullanmanız gerekir. Cihaza seri konsolu aracılığıyla doğrudan veya uzak bir bilgisayardan telnet oturumu açarak eriştiğinizde PuTTY kullanabilirsiniz.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Güncelleştirmeleri tarama ve güncelleştirmeleri uygulama
Cihazınızın güncelleştirilmesi birkaç saat sürebilir. Cihazınızda güncelleştirmeleri taramak ve güncelleştirmeleri uygulamak için aşağıdaki adımları gerçekleştirin.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Cihazınızı güncelleştirmek için
1. Cihaz **Hızlı Başlangıç** sayfasında, **Cihazlar**’a tıklayın. Fiziksel cihazı seçin, **Bakım**’a tıklayın ve ardından **Güncelleştirmeleri Tara**’ya tıklayın.  
2. Kullanılabilir güncelleştirmeleri taramak için bir iş oluşturulur. Güncelleştirmeler varsa, **Güncelleştirmeleri Tara**, **Güncelleştirmeleri Yükle** olarak değişir. **Güncelleştirmeleri Yükle**’ye tıklayın.
3. Bir güncelleştirme işi oluşturulur. Güncelleştirme durumunu izlemek için **İşler**’e gidin.
   
   > [!NOTE]
   > Güncelleştirme işi başladığında, durum anında yüzde 50 olarak görüntülenir. Durum yalnızca güncelleştirme işi tamamlandıktan sonra yüzde 100 olarak değişir. Güncelleştirme süreci için gerçek zamanlı durum sağlanmaz.
   > 
   > 
4. Cihaz başarıyla güncelleştirildikten sonra, Veri 2 ve Veri 3 ağ arabirimleri devre dışı bırakılmışsa bunları etkinleştirin.

## <a name="get-the-iqn-of-a-windows-server-host"></a>Bir Windows Server konağının IQN’ini alın
Windows Server® 2012 çalıştıran bir Windows konağının iSCSI Tam Adını (IQN) almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
StorSimple Cihazınızda tek bir birim için bir isteğe bağlı el ile yedekleme oluşturmak için kamu Portalı'nda aşağıdaki adımları gerçekleştirin.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>MPIO yapılandırma
Çok yollu G/Ç (MPIO) isteğe bağlı bir özelliktir ve Windows Server'da varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir. MPIO yükleme yönergeleri için [StorSimple cihazınız için MPIO yapılandırma](storsimple-configure-mpio-windows-server.md) bölümüne gidin.

Bir Linux konağına bağlı bir StorSimple cihazına yönelik MPIO yükleme yönergeleri için [Linux konağınız için MPIO yapılandırma](storsimple-configure-mpio-on-linux.md) bölümüne gidin.

> [!NOTE]
> MPIO, StorSimple sanal cihazı Azure üzerinde desteklenmiyor.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sanal cihaz](storsimple-virtual-device-u2.md) yapılandırın.
* StorSimple cihazınızı yönetmek için [StorSimple Yöneticisi hizmetini](storsimple-manager-service-administration.md) kullanın.

