---
title: "Hyper-V sanal dizisinde StorSimple sağlama | Microsoft Docs"
description: "StorSimple sanal dizinin dağıtım ikinci Bu öğreticide, Hyper-V sanal bir dizide sağlama içerir."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 4354963c-e09d-41ac-9c8b-f21abeae9913
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bad431c8958f7d381bb9c0410caa3a57c6e75c19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-hyper-v"></a>StorSimple sanal dizinin - sağlama Hyper-v dağıtma
![](./media/storsimple-virtual-array-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Genel Bakış
Bu öğretici, bir StorSimple sanal dizisi Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde sağlayacak açıklar. Bu makale, Azure portalı ve Microsoft Azure kamu bulut StorSimple sanal diziler dağıtımda için geçerlidir.

Sanal bir dizi yapılandırmak ve sağlamak için yönetici ayrıcalıkları gerekir. Sağlama ve ilk kurulumu tamamlamak için yaklaşık 10 dakika sürebilir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
Burada, Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 üzerinde Hyper-V çalıştıran bir konak sisteminde sanal bir dizi sağlamak için gereken önkoşullar bulacaksınız.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [StorSimple sanal dizisi için portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).
* Hyper-V için sanal dizinin görüntüyü Azure portalından yüklediniz. Daha fazla bilgi için bkz: **3. adım: sanal dizinin görüntüyü indirmeyi** , [StorSimple sanal dizinin kılavuzu için portal hazırlamak](storsimple-virtual-array-deploy1-portal-prep.md).

  > [!IMPORTANT]
  > StorSimple sanal dizi çalışan yazılımın yalnızca StorSimple cihaz Yöneticisi hizmeti ile kullanılabilir.
  >
  >

### <a name="for-the-storsimple-virtual-array"></a>StorSimple sanal dizi için
Sanal bir dizi dağıtmadan önce emin olun:

* Hyper-olabilecek V Windows Server 2008 R2 veya üstü çalıştıran bir ana bilgisayar sistemine erişimi sağlamak için kullanılan bir cihaz.
* Ana bilgisayar sistemi sanal dizinizi sağlamak için aşağıdaki kaynaklara ayrılması yapabiliyor:

  * 4 çekirdek en az.
  * En az 8 GB RAM. Dosya sunucusu olarak sanal dizinin yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyaları desteklemek için gerekir.
  * Bir ağ arabirimi.
  * Veri için 500 GB sanal disk.

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce StorSimple sanal dizinin dağıtmak ve veri merkezi ağ uygun şekilde yapılandırmak için ağ gereksinimlerini gözden geçirin. Daha fazla bilgi için bkz: [StorSimple sanal ağ gereksinimleri dizi](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Sağlamak ve sanal bir dizi bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ana bilgisayar sistemi minimum sanal dizinin gereksinimlerini karşılamak için yeterli kaynaklara sahip olduğundan emin olun.
2. Hiper yönetici sanal bir dizide sağlayın.
3. Sanal dizinin başlatın ve IP adresi alın.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements"></a>1. adım: ana bilgisayar sistemi minimum sanal dizinin gereksinimleri karşıladığından emin olun
Sanal bir dizi oluşturmak için gerekir:

* Windows Server 2012 R2, Windows Server 2012 veya Windows Server 2008 R2 SP1 yüklü Hyper-V rolü.
* Microsoft Windows İstemcisi üzerindeki Microsoft Hyper-V Yöneticisi ana bilgisayara bağlı.

Sanal dizinin oluşturmakta olduğunuz temel alınan donanım (ana bilgisayar sistemi) aşağıdaki kaynaklar sanal dizinizi ayrılması mümkün olduğundan emin olun:

* 4 çekirdek en az.
* En az 8 GB RAM. Dosya sunucusu olarak sanal dizinin yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyaları desteklemek için gerekir.
* Bir ağ arabirimi.
* Sistem verileri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-array-in-hypervisor"></a>2. adım: hiper yönetici sanal bir dizide sağlama
Bir aygıt, hiper yönetici sağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-provision-a-virtual-array"></a>Sanal bir dizi sağlamak için
1. Windows Server ana bilgisayarında sanal dizinin görüntüyü bir yerel sürücüye kopyalayın. Bu görüntü (VHD veya VHDX) Azure portalı üzerinden yüklenen. Sonraki yordamda bu görüntüyü kullanma gibi görüntü kopyaladığınız konumunu not edin.
2. Açık **Sunucu Yöneticisi'ni**. Sağ alt köşesinde üst tıklatın **Araçları** seçip **Hyper-V Yöneticisi'ni**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image1.png)  

   Windows Server 2008 R2 çalıştırıyorsanız, Hyper-V Yöneticisi'ni açın. Sunucu Yöneticisi'nde **rolleri > Hyper-V > Hyper-V Yöneticisi'ni**.
3. İçinde **Hyper-V Yöneticisi'ni**, kapsam bölmesinde bağlam menüsünü açmak için sistem düğümünü sağ tıklatın ve ardından **yeni** > **sanal makine**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image2.png)
4. Üzerinde **başlamadan önce** sayfasında yeni sanal makine Sihirbazı'nın, tıklatın **sonraki**.
5. Üzerinde **ad ve konum belirtin** sayfasında, sağlayan bir **adı** sanal dizini. **İleri**’ye tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image4.png)
6. Üzerinde **nesil** sayfasında, cihaz resim türünü seçin ve ardından **sonraki**. Windows Server 2008 R2 kullanıyorsanız, bu sayfa görünmez.

   * Seçin **2. nesil** Windows Server 2012 veya sonraki bir .vhdx görüntüsü yüklediyseniz.
   * Seçin **1. nesil** Windows Server 2008 R2 veya sonraki bir .vhd görüntüsü yüklediyseniz.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image5.png)
7. Üzerinde **atamak bellek** sayfasında, belirtin bir **başlangıç belleği** , en az **8192 MB**olmayan dinamik belleği etkinleştirme ve ardından **sonraki**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image6.png)  
8. Üzerinde **ağ yapılandırma** sayfasında, Internet'e bağlı bir sanal anahtar belirtin ve ardından **sonraki**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image7.png)
9. Üzerinde **sanal sabit diski bağlayın** sayfasında, **varolan bir sanal sabit diski kullanmak**(.vhdx veya .vhd) sanal dizinin görüntü konumunu belirtin ve ardından **sonraki**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image8m.png)
10. Gözden geçirme **Özet** ve ardından **son** sanal makine oluşturulamıyor.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image9.png)
11. En düşük gereksinimleri karşılamak için 4 çekirdek gerekir. 4 sanal işlemci eklemek için ana bilgisayar sisteminizin seçin **Hyper-V Yöneticisi'ni** penceresi. Listesini bölümündeki sağ bölmede **sanal makineleri**, az önce oluşturduğunuz sanal makine bulun. Seçin ve makine adını sağ tıklatın ve seçin **ayarları**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image10.png)
12. Üzerinde **ayarları** sol bölmedeki sayfasını tıklatın **İşlemci**. Sağ bölmede, ayarlayın **sanal işlemcilerin sayısı** 4 (veya daha fazla). **Uygula**'ya tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image11.png)
13. En düşük gereksinimleri karşılamak için de bir 500 GB sanal veri diski eklemeniz gerekir. İçinde **ayarları** sayfa:

    1. Sol bölmede seçin **SCSI denetleyicisi**.
    2. Sağ bölmede seçin **sabit sürücü** tıklatıp **Ekle**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image12.png)
14. Üzerinde **sabit sürücü** sayfasında, **sanal sabit disk** seçeneğini ve tıklayın **yeni**. **Yeni Sanal Sabit Disk Sihirbazı** başlatır.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image13.png)
15. Üzerinde **başlamadan önce** sayfasında yeni sanal sabit Disk Sihirbazı'nın tıklatın **sonraki**.
16. Üzerinde **Disk biçimi seçin sayfasında**, varsayılan seçeneği kabul **VHDX** biçimi. **İleri**’ye tıklayın. Bu ekran, Windows Server 2008 R2 çalıştıran, sunulan değil.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image15.png)
17. Üzerinde **Disk türünü seçin sayfasında**, sanal sabit disk türü olarak ayarlamak **dinamik olarak genişletilen** (önerilen). **Boyutu sabit** disk çalışır ancak uzun bir süre beklemeniz gerekebilir. Değil kullanmanız önerilir **Differencing** seçeneği. **İleri**’ye tıklayın. Windows Server 2012 R2 ve Windows Server 2012'de, **dinamik olarak genişletilen** Windows Server 2008 R2'de varsayılan iken varsayılan seçenektir **boyutu sabit**.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image16.png)
18. Üzerinde **ad ve konum belirtin** sayfasında, sağlayan bir **adı** yanı **konumu** (birine gözatabilirsiniz) veri diski için. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image17.png)
19. Üzerinde **Disk yapılandırma** sayfasında, seçeneğini **bir yeni boş sanal sabit disk Oluştur** ve boyutu olarak belirtin **500 GB** (veya daha fazla). 500 GB en düşük gereksinim olsa da, her zaman daha büyük bir disk sağlayabilirsiniz. Olamaz genişletmek veya bir kez sağlanan diski küçültmeye unutmayın. Sağlamak için disk boyutu hakkında daha fazla bilgi için boyutlandırma bölümünde gözden [en iyi yöntemler belge](storsimple-ova-best-practices.md). **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image18.png)
20. Üzerinde **Özet** sayfasında, sanal veri diskinizi ayrıntılarını gözden geçirin ve memnun tıklatmak **son** disk oluşturmak için. Sihirbaz kapanır ve bir sanal sabit disk makinenize eklenir.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image19.png)
21. Geri dönüp **ayarları** sayfası. Tıklatın **Tamam** kapatmak için **ayarları** sayfasında ve Hyper-V Yöneticisi'ni penceresine dönün.

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-array-and-get-the-ip"></a>3. adım: sanal dizinin başlatın ve IP Al
Sanal dizinizi başlatmak ve buna bağlanmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-start-the-virtual-array"></a>Sanal dizinin başlatmak için
1. Sanal dizinin başlatın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image21.png)
2. Cihaz çalıştırdıktan sonra cihazı seçin, sağ tıklatın ve seçin **Bağlan**.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image22.png)
3. Cihazın hazır olması 5-10 dakika beklemeniz gerekebilir. Bir durum iletisi ilerlemeyi göstermek için konsolda görüntülenir. Cihaz hazır olduktan sonra Git **eylem**. Tuşuna `Ctrl + Alt + Delete` sanal diziye oturum açmak için. Varsayılan kullanıcı *StorSimpleAdmin* ve varsayılan parola *Parola1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image23.png)
4. Güvenlik nedenleriyle ilk oturum açmada cihaz Yönetici parolasının süresi dolar. Parolayı değiştirmek için istenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image24.png)

   En az 8 karakter içeren bir parola girin. Parola en az 3 dışında aşağıdaki 4 gereksinimleri karşılaması gerekir: büyük harf, küçük harfler, sayısal ve özel karakter. Onaylamak için parolayı yeniden girin. Parolanın değiştirilmesi bildirilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image25.png)
5. Parola başarıyla değiştirildikten sonra sanal dizinin yeniden başlatılabilir. Cihazın başlatmak için bekleyin.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image26.png)

    Cihazın Windows PowerShell konsolu ile birlikte bir ilerleme çubuğu görüntülenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image27.png)
6. Adım 6-8 yalnızca yukarı DHCP olmayan ortamda önyüklemesi sırasında uygulanır. Bir DHCP ortamında varsa, bu adımları atlayın ve 9. adıma gidin. Cihazınızı DHCP olmayan ortamda yukarı önyüklendiğinde aşağıdaki ekranı görürsünüz.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image28m.png)

    Ardından, ağ yapılandırın.
7. Kullanım `Get-HcsIpAddress` sanal dizinizi etkin ağ arabirimleri listelemek için komutu. Cihazınızı etkin tek bir ağ arabirimi varsa, bu arabirime atanmış varsayılan addır `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image29m.png)
8. Kullanım `Set-HcsIpAddress` ağı yapılandırmak için cmdlet. Aşağıdaki örneğe bakın:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image30.png)
9. İlk kurulumu tamamlandıktan ve cihaz yukarı önyüklendikten sonra aygıt başlık metni görürsünüz. IP adresi ve cihaz yönetmek için başlık metni görüntülenen URL not edin. Web kullanıcı Arabirimine sanal dizinin bağlanmak ve yerel kurulumunu ve kaydını tamamlamak için bu IP adresi kullanın.

   ![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image31m.png)
10. (İsteğe bağlı) Yalnızca Cihazınızı kamu bulutta dağıtıyorsanız bu adımı gerçekleştirin. Şimdi Cihazınızda Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) mod olanak sağlar. FIPS 140 standardı, önemli verilerin korunması için BİZE Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunda etkinleştirmek için aşağıdaki cmdlet'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. Böylece şifreleme doğrulama etkili FIPS modunda etkinleştirdikten sonra Cihazınızı yeniden başlatın.

       > [!NOTE]
       > Etkinleştirmek veya FIPS modundayken, Cihazınızda devre dışı. Cihaz FIPS ve FIPS olmayan mod arasında geçiş yapma desteklenmiyor.
       >
       >

Cihazınızı minimum yapılandırma gereksinimlerini karşılamıyorsa, (aşağıda gösterilen) başlık metni aşağıdaki hatayı görürsünüz. Böylece makine en düşük gereksinimleri karşılamak için yeterli kaynaklara sahip aygıt yapılandırmasını değiştirin. Ardından, yeniden başlatın ve cihaza bağlanın. Minimum yapılandırma gereksinimlerini başvurmak [1. adım: ana bilgisayar sistemi minimum sanal dizinin gereksinimlerini karşıladığından emin olmak](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-hyperv/image32.png)

Yerel web kullanıcı Arabirimi kullanılarak yapılan başlangıç yapılandırması sırasında başka bir hata yüz, aşağıdaki iş akışları için başvurun:

* Tanılama testleri [web kullanıcı Arabirimi Kurulumu sorunlarını giderme](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Günlük paketi oluşturmak ve günlük dosyalarını görüntülemek](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [Dosya sunucusu olarak StorSimple sanal dizinizi ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple sanal dizinizi bir iSCSI sunucusu olarak ayarla](storsimple-virtual-array-deploy3-iscsi-setup.md)
