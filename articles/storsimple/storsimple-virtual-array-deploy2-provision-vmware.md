---
title: "VMware sanal dizisinde StorSimple sağlama | Microsoft Docs"
description: "StorSimple sanal dizinin dağıtım serisi ikinci Bu öğreticide, bir VMware sanal aygıt sağlama içerir."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/15/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 118521a127b2e4b765efabdbdde71605440d81c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple sanal dizinin - VMware sağlama dağıtma
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Genel Bakış
Bu öğretici, sağlamak ve bir StorSimple sanal dizinin VMware ESXi 5.5 çalıştıran bir konak sistemi ve üstü bağlanmak açıklar. Bu makale, Azure portalı ve Microsoft Azure kamu bulut StorSimple sanal diziler dağıtımda için geçerlidir.

Sanal cihaza bağlanmak ve sağlamak için yönetici ayrıcalıkları gerekir. Sağlama ve ilk kurulumu tamamlamak için yaklaşık 10 dakika sürebilir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
VMware ESXi 5.5 çalıştıran bir konak sistemi ve üstü, sanal cihazı sağlamak için gereken önkoşullar aşağıdaki gibidir.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [StorSimple sanal dizisi için portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).
* Azure portalından için VMware sanal cihaz görüntüsü yüklediniz. Daha fazla bilgi için bkz: **3. adım: sanal cihaz görüntüsünü karşıdan yüklemek** , [StorSimple sanal dizinin kılavuzu için portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>StorSimple sanal cihaz için
Sanal cihazı dağıtmadan önce emin olun:

* Hyper-V çalıştıran bir ana bilgisayar sistemine erişimi (2008 R2 veya sonrası), olabilir sağlamak için kullanılan bir cihaz.
* Ana bilgisayar sistemi sanal Cihazınızı sağlamak için aşağıdaki kaynaklara ayrılması yapabiliyor:

  * 4 çekirdek en az.
  * En az 8 GB RAM. Dosya sunucusu olarak sanal dizinin yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyaları desteklemek için gerekir.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-network-in-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce aşağıdakilerden emin olun:

* StorSimple sanal cihazı dağıtmak için ağ gereksinimleri gözden geçirdikten ve veri merkezi ağ gereksinimlerine uygun yapılandırılmış. 

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Sağlamak ve bir sanal cihaza bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ana bilgisayar sistemi minimum sanal cihaz gereksinimlerini karşılamak için yeterli kaynak bulunduğundan emin olun.
2. Sanal cihazı, hipervizörde sağlayın.
3. Sanal cihaz başlatın ve IP adresi alın.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>1. adım: ana bilgisayar sistemi minimum sanal cihaz gereksinimlerini karşıladığından emin olun.
Sanal cihazı oluşturmak için ihtiyacınız:

* VMware ESXi Server 5.5 çalıştıran bir konak sistemi ve üstü erişim.
* ESXi konağı yönetebilmesi için VMware vSphere istemci sisteminizdeki.

  * 4 çekirdek en az.
  * En az 8 GB RAM. Dosya sunucusu olarak sanal dizinin yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyaları desteklemek için gerekir.
  * Bir ağ arabirimi Internet trafiği yönlendirme yeteneğine ağa bağlı. En düşük Internet bant genişliği en iyi cihaz çalışma için izin vermek için 5 MB/sn olmalıdır.
  * Veri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>2. adım: hiper yönetici sanal bir aygıt sağlama
Sanal cihazı, hipervizörde sağlamak için aşağıdaki adımları gerçekleştirin.

1. Sisteminizde sanal cihaz görüntüsü kopyalayın. Azure Portalı aracılığıyla bu sanal görüntü indirilir.

   1. En son görüntü dosyasını karşıdan emin olun. Görüntünün daha önce yüklediyseniz, onu yeniden son görüntüsüne sahip olmak için indirin. En son görüntü (yerine bir) iki dosya vardır.
   2. Sonraki yordamda bu görüntüyü kullanma gibi görüntü kopyaladığınız konumunu not edin.

2. VSphere istemci kullanarak ESXi sunucusunda oturum açın. Bir sanal makine oluşturmak için Yönetici ayrıcalıklarınızın olması gerekir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Sol bölmede, stok bölümünde vSphere istemci ESXi sunucuyu seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. VMDK ESXi sunucuya yükleyin. Gidin **yapılandırma** sekmesini sağ bölmede. Altında **donanım**seçin **depolama**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Sağ bölmede altında **Datastores**, VMDK karşıya yüklemek istediğiniz veri deposu seçin. Veri deposu, işletim sistemi ve veri diskleri için yeterli boş alan olması gerekir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Sağ tıklatıp **Gözat veri deposu**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. A **veri deposu tarayıcı** penceresi görüntülenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Araç çubuğunda ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) simgesi yeni bir klasör oluşturun. Klasör adı belirtin ve bunu not edin. Bu klasör adı (en iyisi önerilir) bir sanal makine oluştururken daha sonra kullanacaksınız. **Tamam** düğmesine tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. Yeni klasör sol bölmesinde görünür **veri deposu tarayıcı**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Karşıya yükleme simgesini ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) seçip **dosyasını karşıya yükle**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. Gözat ve indirdiğiniz VMDK dosyaları gösterin. İki dosya vardır. Karşıya yüklenecek dosyayı seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. Tıklatın **açık**. Belirtilen veri deposu VMDK dosyasını karşıya yükleme başlar. Karşıya yüklenecek dosyayı birkaç dakika sürebilir.
13. Yükleme tamamlandıktan sonra veri deposunu oluşturduğunuz klasördeki dosyasında bakın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Şimdi ikinci VMDK dosyasını aynı veri deposuna yükleyin.
14. VSphere istemci penceresine dönün. Seçili ESXi sunucusuyla sağ tıklatıp **yeni bir sanal makine**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. A **yeni sanal makine oluşturma** penceresi görünür. Üzerinde **yapılandırma** sayfasında, **özel** seçeneği. **İleri**’ye tıklayın.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Üzerinde **ad ve konum** sayfasında, sanal makinenin adını belirtin. Bu adı daha önce adım 8'de belirtilen klasör adı (önerilen en iyi yöntem) eşleşmelidir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Üzerinde **depolama** sayfasında, VM'yi sağlamak için kullanmak istediğiniz bir veri deposu seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Üzerinde **sanal makine sürüm** sayfasında, **sanal makine sürüm: 8**. Sürümleri 8-11 tüm desteklenir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Üzerinde **konuk işletim sistemi** sayfasında, **konuk işletim sistemi** olarak **Windows**. İçin **sürüm**, açılır listeden seçin **Microsoft Windows Server 2012 (64 bit)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Üzerinde **CPU** sayfasında, ayarlamak **sanal Yuva sayısını** ve **çekirdek sanal yuva sayısı** böylece **çekirdek toplam sayısı** 4 (veya daha fazla). **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Üzerinde **bellek** sayfasında, 8 GB (veya daha fazla) RAM belirtin. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Üzerinde **ağ** sayfasında, ağ arabirimlerinin sayısını belirtin. Bir ağ arabirimi en düşük gereksinimdir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Üzerinde **SCSI denetleyicisi** sayfasında, varsayılanı kabul **LSI mantığı SAS Denetleyici**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Üzerinde **bir Disk seçin** sayfasında, **varolan bir sanal disk kullanmak**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Üzerinde **mevcut Disk seçin** sayfasında **Disk dosya yolu**, tıklatın **Gözat**. Bu açılır bir **Gözat Datastores** iletişim. Burada VMDK karşıya yüklediğiniz konuma gidin. Başlangıçta karşıya iki dosya birleştirilmiş olarak yalnızca bir veri deposu dosyasında şimdi bakın. Dosyayı seçin ve tıklatın **Tamam**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Üzerinde **Gelişmiş Seçenekler** sayfasında, varsayılanı kabul etmek ve tıklayın **sonraki**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. Üzerinde **tam hazır** sayfasında, yeni bir sanal makine ile ilişkili tüm ayarları gözden geçirin. Denetleme **tamamlanmadan önce sanal makine ayarlarını Düzenle**. Tıklatın **devam**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Üzerinde **sanal makineleri özellikleri** sayfasında **donanım** sekmesinde, aygıt donanım bulun. Seçin **yeni Sabit Disk**. **Ekle**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Gördüğünüz bir **Donanım Ekle** penceresi. Üzerinde **aygıt türü** sayfasında **eklemek istediğiniz aygıt türü seçin**seçin **Sabit Disk**, tıklatıp **sonraki**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Üzerinde **bir Disk seçin** sayfasında, **yeni bir sanal disk oluşturma**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Üzerinde **bir Disk oluşturmak** sayfasında, değişiklik **Disk boyutu** 500 GB (veya daha fazla). 500 GB en düşük gereksinim olsa da, her zaman daha büyük bir disk sağlayabilirsiniz. Olamaz genişletmek veya bir kez sağlanan diski küçültmeye unutmayın. Sağlamak için disk boyutu hakkında daha fazla bilgi için boyutlandırma bölümünde gözden [en iyi yöntemler belge](storsimple-ova-best-practices.md). Altında **Disk sağlama**seçin **ince provizyon**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Üzerinde **Gelişmiş Seçenekler** sayfasında, varsayılanı kabul edin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Üzerinde **tam hazır** sayfasında, disk seçeneklerini gözden geçirin. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Sanal makine özellikleri sayfasına geri dönün. Yeni bir sabit disk, sanal makineye eklenir. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Gidin, sağ bölmede seçili ile sanal makine, **Özet** sekmesi. Sanal makineniz için ayarları gözden geçirin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Sanal makineniz şimdi sağlanır. Bu makinede güç ve IP adresi almak için sonraki adımdır bakın.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>3. adım: sanal cihaz başlatın ve IP Al
Sanal cihazınız başlatmak ve buna bağlanmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-start-the-virtual-device"></a>Sanal cihaz başlatmak için
1. Sanal cihaz başlatın. Sol bölmede, Configuration Manager vSphere Cihazınızı seçin ve bağlam menüsünü açmak için sağ tıklatın. Seçin **güç** ve ardından **üzerinde güç**. Bu, sanal makinenizde güç. Alt durumunu görüntüleyebilirsiniz **son kullanılan görevler** vSphere istemci bölmesi.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Kurulum görevleri tamamlamak için birkaç dakika sürer. Cihaz çalışmaya başladıktan sonra gidin **konsol** sekmesi. Cihaza oturum açmak için Ctrl + Alt + Delete gönderin. Alternatif olarak, konsol penceresi imleci noktası ve Ctrl + Alt + INSERT tuşuna basın. Varsayılan kullanıcı *StorSimpleAdmin* ve varsayılan parola *Parola1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Güvenlik nedenleriyle ilk oturum açmada cihaz Yönetici parolasının süresi dolar. Parolayı değiştirmek için istenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. En az 8 karakter içeren bir parola girin. Parola 3 çıkışı 4'ün bu gereksinimlerden birini içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter. Onaylamak için parolayı yeniden girin. Parolanın değiştirilmesi bildirilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Parola başarıyla değiştirildikten sonra sanal cihaz yeniden başlatabilirsiniz. Tamamlamak için yeniden başlatma için bekleyin. Cihazın Windows PowerShell konsolu ile birlikte bir ilerleme çubuğu görüntülenebilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Adım 6-8 yalnızca yukarı DHCP olmayan ortamda önyüklemesi sırasında uygulanır. Bir DHCP ortamında varsa, bu adımları atlayın ve 9. adıma gidin. Cihazınızı DHCP olmayan ortamda yukarı önyüklendiğinde aşağıdaki ekranı görürsünüz.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Ardından, ağ yapılandırın.
7. Kullanım `Get-HcsIpAddress` sanal Cihazınızda etkin ağ arabirimleri listelemek için komutu. Cihazınızı etkin tek bir ağ arabirimi varsa, bu arabirime atanmış varsayılan addır `Ethernet`.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Kullanım `Set-HcsIpAddress` ağı yapılandırmak için cmdlet. Bir örnek aşağıda verilmiştir:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. İlk kurulumu tamamlandıktan ve cihaz yukarı önyüklendikten sonra aygıt başlık metni görürsünüz. IP adresi ve cihaz yönetmek için başlık metni görüntülenen URL not edin. Web kullanıcı Arabirimine sanal cihazınızın bağlanmak ve yerel kurulumunu ve kaydını tamamlamak için bu IP adresini kullanacak.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (İsteğe bağlı) Yalnızca Cihazınızı kamu bulutta dağıtıyorsanız bu adımı gerçekleştirin. Şimdi Cihazınızda Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) mod olanak sağlar. FIPS 140 standardı, önemli verilerin korunması için BİZE Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunda etkinleştirmek için aşağıdaki cmdlet'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. Böylece şifreleme doğrulama etkili FIPS modunda etkinleştirdikten sonra Cihazınızı yeniden başlatın.

       > [!NOTE]
       > Etkinleştirmek veya FIPS modundayken, Cihazınızda devre dışı. Cihaz FIPS ve FIPS olmayan mod arasında geçiş yapma desteklenmiyor.
       >
       >

Cihazınız en düşük yapılandırma gereksinimlerini karşılamıyor (aşağıda gösterilen) başlık metni bir hata görürsünüz. Böylece en düşük gereksinimleri karşılamak için yeterli kaynaklara sahip aygıt yapılandırmasını değiştirmeniz gerekir. Ardından, yeniden başlatın ve cihaza bağlanın. Minimum yapılandırma gereksinimlerini başvurmak [1. adım: ana bilgisayar sistemi minimum sanal cihaz gereksinimlerini karşıladığından emin olmak](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Yerel web kullanıcı Arabirimi kullanılarak yapılan başlangıç yapılandırması sırasında başka bir hata yüz, aşağıdaki iş akışları için başvurun:

* Tanılama testleri [web kullanıcı Arabirimi Kurulumu sorunlarını giderme](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Günlük paketi oluşturmak ve günlük dosyalarını görüntülemek](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [Dosya sunucusu olarak StorSimple sanal dizinizi ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple sanal dizinizi bir iSCSI sunucusu olarak ayarla](storsimple-virtual-array-deploy3-iscsi-setup.md)
