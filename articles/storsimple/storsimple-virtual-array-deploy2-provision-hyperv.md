---
title: StorSimple sanal dizisi, Hyper-V'de sağlama | Microsoft Docs
description: StorSimple sanal dizisi dağıtım ikinci Bu öğreticide, bir sanal dizin Hyper-V'de sağlama içerir.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5104d630e2b4e97b80a6fedfb6d863061c2722fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61416335"
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>StorSimple sanal dizisi - Hyper-V'de sağlama dağıtma
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir StorSimple sanal dizisi Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde sağlama işlemi açıklanır. Bu makale, Azure portalı ve Microsoft Azure kamu Bulutu StorSimple sanal dizilerine dağıtımda için geçerlidir.

Sanal dizi yapılandırmak ve sağlamak için yönetici ayrıcalıkları gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
Burada, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde bir sanal diziyi sağlamak için önkoşulları bulacaksınız.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [StorSimple Virtual Array için portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).
* Azure portalından Hyper-V için sanal dizi görüntüsünü yüklediniz. Daha fazla bilgi için **3. adım: Sanal dizi görüntüsünü indir** , [StorSimple Virtual Array kılavuzu için portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > StorSimple sanal dizisi üzerinde çalışan yazılımı yalnızca StorSimple cihaz Yöneticisi hizmeti ile kullanılabilir.
  >
  >

### <a name="for-the-storsimple-virtual-array"></a>StorSimple sanal dizisi için
Sanal dizi dağıtmadan önce emin olun:

* Hyper-olabilecek V Windows Server 2008 R2 veya üzeri çalıştıran bir konak sistemi erişiminiz bir sağlamak için kullanılan bir cihaz.
* Konak sisteminin, sanal diziyi sağlamak için aşağıdaki kaynakları ayırmanız olanağına sahip:

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal dizi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyalarını desteklemek için ihtiyacınız vardır.
  * Bir ağ arabirimi.
  * Veri için 500 GB sanal disk.

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce StorSimple sanal Dizini'ni dağıtma ve veri merkezi ağı uygun şekilde yapılandırmak için ağ gereksinimlerini gözden geçirin. Daha fazla bilgi için [gereksinimlerinde StorSimple Virtual Array](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Sağlama ve bir sanal diziye bağlamak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ana bilgisayar sistemi en düşük sanal dizi gereksinimleri karşılamak için yeterli kaynak bulunduğundan emin olun.
2. Hiper yönetici olarak bir sanal dizin sağlayın.
3. Sanal diziyi başlatın ve IP adresini alın.

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a>1\. adım: Ana bilgisayar sistemi en düşük sanal dizi gereksinimleri karşıladığından emin olun
Bir sanal dizin oluşturmak için ihtiyacınız vardır:

* Hyper-V rolü yüklü Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 SP1.
* Ana bilgisayara bağlı Microsoft Windows istemcisine yüklenmiş Microsoft Hyper-V Yöneticisi.

Sanal dizi oluşturmakta olduğunuz temel donanımın (ana bilgisayar sistemi) sanal diziniz için aşağıdaki kaynakları ayırmanız mümkün olduğundan emin olun:

* En az 4 çekirdek.
* En az 8 GB RAM. Sanal dizi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyalarını desteklemek için ihtiyacınız vardır.
* Bir ağ arabirimi.
* Sistem verileri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>2\. adım: Hiper yönetici olarak bir sanal dizin sağlayın
Hiper yöneticinizde cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-provision-a-virtual-array"></a>Sanal diziyi sağlamak için
1. Windows Server konağınızda sanal dizi görüntüsünü yerel bir sürücüye kopyalayın. Bu görüntüyü, (VHD veya VHDX) Azure portalından indirdiğiniz. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.
2. **Sunucu Yöneticisi**'ni açın. Sağ üst köşede **Araçlar**'a tıklayın ve **Hyper-V Yöneticisi**'ni seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Windows Server 2008 R2 çalıştırıyorsanız, Hyper-V Yöneticisi'ni açın. Sunucu Yöneticisi'nde **rolleri > Hyper-V > Hyper-V Yöneticisi'ni**.
3. **Hyper-V Yöneticisi**'nin kapsam bölmesinde sistem düğümünüze sağ tıklayarak bağlam menüsünü açın ve **Yeni** > **Sanal Makine**'ye tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Yeni Sanal Makine Sihirbazı'nın **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
5. Üzerinde **adı ve konumu belirtin** sayfasında, sağlayan bir **adı** sanal diziniz için. **İleri**’ye tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. Üzerinde **nesli belirtmeniz** sayfasında, cihaz görüntü türünü seçin ve ardından **sonraki**. Windows Server 2008 R2 kullanıyorsanız, bu sayfa görünmez.

   * Seçin **2. nesil** Windows Server 2012 veya sonraki bir .vhdx görüntüsü yüklediyseniz.
   * Seçin **1. nesil** Windows Server 2008 R2 veya sonraki bir .vhd görüntüsü yüklediyseniz.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. **Bellek ata** sayfasında **Başlangıç belleği** değerini en az **8192 MB** yapın, dinamik bellek özelliğini etkinleştirmeyin ve **İleri**'ye tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. **Ağı yapılandır** sayfasında İnternete bağlı olan sanal anahtarı belirtin ve **İleri**'ye tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. Üzerinde **sanal sabit diski bağlayın** sayfasında **var olan bir sanal sabit disk kullan**(.vhdx veya .vhd) sanal dizi görüntüsünü konumunu belirtin ve ardından **sonraki**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. **Özet** sayfasını gözden geçirin ve **Son**'a tıklayarak sanal makineyi oluşturun.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. Minimum gereksinimleri karşılamak için 4 çekirdeğe ihtiyacınız vardır. 4 sanal işlemci eklemek için **Hyper-V Yöneticisi** penceresinde ana bilgisayar sisteminizi seçin. Sağ tarafta, **Sanal Makineler** listesinin altında bulunan bölmede az önce oluşturduğunuz sanal makineyi bulun. Makine adına sağ tıklayın ve **Ayarlar**'ı seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. **Ayarlar** sayfasında sol taraftaki bölmeden **İşlemci**'yi seçin. Sağ taraftaki bölmede **sanal işlemci sayısını** 4 (veya üzeri) olarak ayarlayın. **Uygula**'ya tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. En düşük gereksinimleri karşılamak için de 500 GB sanal veri diski eklemeniz gerekir. **Ayarlar** sayfasında:

    1. Sol taraftaki bölmede **SCSI Denetleyicisi**'ni seçin.
    2. Sağ taraftaki bölmede **Sabit Sürücü**'yü seçin ve **Ekle**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. **Sabit sürücü** sayfasında **Sanal sabit disk** seçeneğini belirleyin ve **Yeni**'ye tıklayın. **Yeni Sanal Sabit Disk Sihirbazı** açılır.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Yeni Sanal Sabit Disk Sihirbazı'nın **Başlamadan önce** sayfasında **İleri**'ye tıklayın.
16. **Disk Biçimini Seç** sayfasında varsayılan seçenek olan **VHDX** biçimini kabul edin. **İleri**’ye tıklayın. Bu ekran, Windows Server 2008 R2 çalıştırıyorsanız görüntülenmez.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. **Disk Türünü Seç** sayfasında sanal sabit disk türünü **Dinamik olarak genişletilen** (önerilen) olarak ayarlayın. **Sabit boyutlu** diski de seçebilirsiniz ancak daha uzun süre beklemeniz gerekebilir. **Fark kayıt** seçeneğini kullanmamanızı öneririz. **İleri**’ye tıklayın. Windows Server 2012 R2 ve Windows Server 2012'de, **dinamik olarak genişleyen** Windows Server 2008 R2'de varsayılan iken varsayılan seçenektir **boyutu sabit**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. **Ad ve Konum Belirtin** sayfasında veri diski için bir **ad** ve **konum** (göz atabilirsiniz) belirtin. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. Üzerinde **Disk yapılandırma** sayfasında, seçeneğini **bir yeni boş sanal sabit disk Oluştur** ve boyutu olarak **500 GB** (veya daha fazla). 500 GB en düşük gereksinim olsa da, her zaman daha büyük bir disk sağlayabilirsiniz. Genişletin veya bir kez sağlanmış bir diski küçültmeye olduğunu unutmayın. Sağlama disk boyutu hakkında daha fazla bilgi için boyutlandırma bölümünde gözden [en iyi yöntemler belge](storsimple-ova-best-practices.md). **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. **Özet** sayfasında sanal veri diskinizin ayrıntılarını gözden geçirin ve her şey yolunda görünüyorsa **Son**'a tıklayarak diski oluşturun. Sihirbaz kapanır ve makinenize bir sanal sabit disk eklenir.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. **Ayarlar** sayfasına geri dönün. **Tamam**'a tıklayarak **Ayarlar** sayfasını kapatın ve Hyper-V Yöneticisi penceresine dönün.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a>3\. adım: Sanal diziyi başlatın ve IP'yi alın
Sanal diziyi başlatın ve buna bağlanmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-start-the-virtual-array"></a>Sanal dizi başlatmak için
1. Sanal diziyi başlatın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Cihaz çalışmaya başladıktan sonra cihazı ve **Bağlan**'ı seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Cihazın hazır olması 5-10 dakika beklemeniz gerekebilir. Konsolda ilerleme durumunu gösteren bir durum iletisi görüntülenir. Cihaz hazır olduktan sonra **Eylem** bölümüne gidin. Tuşuna `Ctrl + Alt + Delete` sanal diziye oturum açmak için. Varsayılan kullanıcı *StorSimpleAdmin* ve varsayılan parola *Password1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Güvenlik nedeniyle cihazın yönetici parolasının ilk oturum açma işleminin ardından değiştirilmesi gerekir. Parolayı değiştirmeniz istenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   En az 8 karakterden oluşan bir parola girin. Parolanın şu 4 gereksinimden en az 3 tanesini karşılaması gerekir: büyük harf, küçük harf, rakam ve özel karakter. Onaylamak için parolayı yeniden girin. Parolanın değiştirildiği bildirilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Parola başarıyla değiştirildikten sonra sanal diziyi yeniden başlatılabilir. Cihazın başlatılmasını bekleyin.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    Cihazın Windows PowerShell konsolu ve ilerleme çubuğu gösterilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Adım 6-8 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 9. adımla devam edebilirsiniz. DHCP olmayan Ortam aygıtınızda'kurmak önyükleme aşağıdaki ekranı görürsünüz.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Ardından, ağ yapılandırın.
7. Kullanım `Get-HcsIpAddress` sanal diziniz etkin ağ arabirimleri listelemek için komutu. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıdaki örneğe bakın:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Web kullanıcı Arabiriminde sanal diziniz bağlanmak ve yerel kurulumu ve kaydı tamamlamak için bu IP adresini kullanın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (İsteğe bağlı) Yalnızca kamu bulutunda Cihazınızı dağıtıyorsanız bu adımı gerçekleştirin. Bu gibi durumlarda, Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) modunu artık Cihazınızda sağlayacaktır. FIPS 140 standardı, hassas verilerin korunması için ABD Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunu etkinleştirmek için aşağıdaki cmdlet'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. Şifreleme doğrulama etkili olacak şekilde FIPS modundayken etkinleştirdikten sonra Cihazınızı yeniden başlatın.

       > [!NOTE]
       > Etkinleştirebilir veya FIPS modundayken, Cihazınızda devre dışı bırakın. Cihaz FIPS ve FIPS olmayan modu arasında geçiş yapma desteklenmiyor.
       >
       >

Cihazınız en düşük yapılandırma gereksinimlerini karşılamaması durumunda (aşağıda gösterilen) başlık metnini aşağıdaki hatayı görürsünüz. Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirin. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. Adım 1'deki en düşük yapılandırma gereksinimlerini bakın: Ana bilgisayar sistemi en düşük sanal dizi gereksinimleri karşıladığından emin olun.

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Yerel web kullanıcı arabirimini kullanarak ilk yapılandırma sırasında herhangi bir hata yüz tanıma, aşağıdaki iş akışları için başvurun:

* Tanılama testleri [web kullanıcı Arabirimi Kurulumu sorunlarını giderme](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Günlük paketini oluşturma ve günlük dosyalarını görüntülemek](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Virtual Array'iniz bir dosya sunucusu ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple Virtual Array'iniz bir iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md)
