---
title: StorSimple sanal dizisi vmware'de sağlama | Microsoft Docs
description: StorSimple sanal dizisi dağıtım serisi ikinci Bu öğreticide, bir VMware sanal cihazı sağlama içerir.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c9fe597957057dc61da5c2b1cf6f9216711764a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61419307"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple sanal dizisi - Vmware'de sağlama dağıtma
![](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Genel Bakış
Bu öğreticide, sağlamak ve VMware ESXi 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir konak sisteminde bir StorSimple Virtual Array bağlanmak açıklar. Bu makale, Azure portalı ve Microsoft Azure kamu Bulutu StorSimple sanal dizilerine dağıtımda için geçerlidir.

Sanal cihaz sağlamak ve bağlantı kurmak için yönetici ayrıcalıklarına sahip olmanız gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
Bir konak sisteminde VMware ESXi 5.0, 5.5, 6.0 veya 6.5, çalışan bir sanal cihaz sağlama için gereken önkoşullar aşağıdaki gibidir.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* Tüm adımları tamamladınız [StorSimple Virtual Array için portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).
* VMware için Azure portalından sanal cihaz görüntüsü yüklediniz. Daha fazla bilgi için **3. adım: Sanal cihaz görüntüsü indirme** , [StorSimple Virtual Array kılavuzu için portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).

### <a name="for-the-storsimple-virtual-device"></a>StorSimple sanal cihaz için
Sanal cihazı dağıtmadan önce şunlardan emin olun:

* Hyper-V çalıştıran bir konak sistemi erişebilirsiniz (2008 R2 veya üzeri) olabilecek bir sağlamak için kullanılan bir cihaz.
* Ana bilgisayar sistemi sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabiliyor:

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal dizi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyalarını desteklemek için ihtiyacınız vardır.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-network-in-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce aşağıdakilerden emin olun:

* Bir StorSimple sanal cihaz dağıtmak için ağ gereksinimleri gözden geçirdikten ve gereksinimlerine uygun veri merkezi ağı yapılandırılır. 

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Sağlama ve bir sanal cihaza bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Ana bilgisayar sistemi en düşük sanal cihaz gereksinimlerini karşılamak için yeterli kaynak bulunduğundan emin olun.
2. İçinde hiper yönetici sanal cihaz sağlayın.
3. Sanal cihazı başlatın ve IP adresini alın.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>1. Adım: Ana bilgisayar sistemi en düşük sanal cihaz gereksinimlerini karşıladığından emin olun.
Sanal cihazı oluşturmak için ihtiyacınız:

* VMware ESXi Server 5.0, 5.5, 6.0 veya 6.5 çalıştıran bir konak sistemi erişim.
* ESXi ana bilgisayarını yönetmek için sisteminizde VMware vSphere istemcisi yüklü.

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal dizi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 2 milyondan az dosyalarını destekler. 16 GB RAM, 2-4 milyon dosyalarını desteklemek için ihtiyacınız vardır.
  * İnternet trafiği için ağa bağlı bir ağ arabirimi. En düşük Internet bant genişliği için en iyi çalışan cihazın izin vermek için 5 MB/sn olmalıdır.
  * Veri için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>2. Adım: Hiper yöneticide bir sanal cihaz sağlama
Hiper yöneticinizde sanal cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

1. Sanal cihaz görüntüsünü sisteminize kopyalayın. Bu sanal görüntü Azure portalından indirdiğiniz.

   1. En son görüntü dosyasını yüklediğiniz emin olun. Görüntü daha önce indirdiyseniz, sahip olduğunuz en son görüntü yeniden sağlamak için indirin. En son görüntü (yerine bir) iki dosya vardır.
   2. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.

2. vSphere istemcisini kullanarak ESXi sunucusunda oturum açın. Sanal makine oluşturmak için yönetici ayrıcalıklarınızın olması gerekir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. Sol bölmede, Envanter bölümünde vSphere Client ESXi Server'ı seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. VMDK dosyasını ESXi sunucusuna yükleyin. Gidin **yapılandırma** sağ bölmede sekme. Altında **donanım**seçin **depolama**.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Sağ taraftaki bölmede **Datastores** (Veri depoları) öğesini seçerek VMDK dosyasını yüklemek istediğiniz yeri belirleyin. Veri deposu işletim sistemi ve veri diskleri için yeterli boş alan olması gerekir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Sağ tıklayıp **Browse Datastore** (Veri Deposuna Göz At) öğesini seçin.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. **Datastore Browser** (Veri Deposu Tarayıcısı) penceresi açılır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Araç çubuğunda, ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png) simgesini yeni bir klasör oluşturun. Klasör adını belirtin ve not edin. Sanal makine oluştururken bu klasör adını kullanacaksınız (önerilen yöntemdir). **Tamam** düğmesine tıklayın.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. Yeni klasör **Datastore Browser** (Veri Deposu Tarayıcısı) penceresinin sol tarafında görünür.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Karşıya yükleme simgesini ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png) seçip **dosyasını karşıya yükle**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. İndirdiğiniz VMDK dosyalarını bulun. İki dosya vardır. Karşıya yüklemek için dosyalardan birini seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. **Aç**'a tıklayın. VMDK dosyası belirtilen veri deposuna yüklenmeye başlar. Dosyanın karşıya yüklenmesi birkaç dakika sürebilir.
13. Karşıya yükleme işlemi tamamlandıktan sonra dosyayı oluşturduğunuz veri deposunda görebilirsiniz.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Şimdi ikinci VMDK dosyasını da aynı ver deposuna yükleyin.
14. vSphere istemcisi penceresine dönün. Seçili ESXi sunucusu ile sağ tıklayıp **yeni sanal makine**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. A **yeni sanal makine oluştur** penceresi görünür. Üzerinde **yapılandırma** sayfasında **özel** seçeneği. **İleri**’ye tıklayın.
    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. Üzerinde **adını ve konumunu** sayfasında, sanal makinenizin adını belirtin. Bu adı daha önce adım 8'de belirttiğiniz klasör adı (önerilen en iyi uygulama) eşleşmelidir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. Üzerinde **depolama** sayfasında, VM'nize sağlamak için kullanmak istediğiniz bir veri deposu seçin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. Üzerinde **sanal makine sürümünü** sayfasında **sanal makine sürümünü: 8**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. Üzerinde **konuk işletim sistemi** sayfasında **konuk işletim sistemi** olarak **Windows**. İçin **sürüm**, açılır listeden seçin **Microsoft Windows Server 2012 (64-bit)**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. Üzerinde **CPU'lar** sayfasında, ayarlamak **sanal Yuva sayısını** ve **sanal yuva başına çekirdek sayısı** böylece **toplam çekirdek sayınız** 4 (veya daha fazla) olabilir. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. Üzerinde **bellek** sayfasında, 8 GB (veya daha fazla) RAM miktarını belirtin. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. Üzerinde **ağ** sayfasında, ağ arabirimlerinin sayısını belirtin. Bir ağ arabirimi en düşük gereksinimdir.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. Üzerinde **SCSI denetleyicisi** sayfasında, varsayılanı kabul **LSI Logic SAS Denetleyici**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. Üzerinde **bir Disk seçin** sayfasında **var olan bir sanal disk kullan**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. Üzerinde **var olan bir diski seçin** sayfasındaki **Disk dosya yolu**, tıklayın **Gözat**. Bu açılır bir **Gözat veri depoları** iletişim. VMDK yüklediğiniz konuma gidin. Başlangıçta karşıya iki dosya birleştirilmiş olarak yalnızca bir veri deposu dosyasında göreceksiniz. Dosyayı seçin ve tıklayın **Tamam**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. Üzerinde **Gelişmiş Seçenekler** sayfasında, varsayılanı kabul edin ve tıklayın **sonraki**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. **Ready to Complete** (Tamamlanmak İçin Hazır) sayfasında yeni sanal makineyle ilgili tüm ayarları gözden geçirin. Denetleme **tamamlanmadan önce sanal makine ayarlarını Düzenle**. **Devam**’a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. Üzerinde **sanal makinelerin özellikleri** sayfasında **donanım** sekmesinde, cihaz donanım bulun. Seçin **yeni Sabit Disk**. **Ekle**'ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. Gördüğünüz bir **Donanım Ekle** penceresi. Üzerinde **cihaz türü** sayfasındaki **eklemek istediğiniz cihaz türünü seçin**seçin **Sabit Disk**, tıklatıp **sonraki**.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. Üzerinde **bir Disk seçin** sayfasında **yeni bir sanal disk oluşturma**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. Üzerinde **Disk Oluştur** sayfasında, değişiklik **Disk boyutu** 500 GB (veya daha fazla). 500 GB en düşük gereksinim olsa da, her zaman daha büyük bir disk sağlayabilirsiniz. Genişletin veya bir kez sağlanmış bir diski küçültmeye olduğunu unutmayın. Sağlama disk boyutu hakkında daha fazla bilgi için boyutlandırma bölümünde gözden [en iyi yöntemler belge](storsimple-ova-best-practices.md). Altında **Disk sağlama**seçin **ölçülü kaynak sağlama**. **İleri**’ye tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. Üzerinde **Gelişmiş Seçenekler** sayfasında, varsayılanı kabul edin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. Üzerinde **tamamlamaya hazır** sayfasında, disk seçenekleri gözden geçirin. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Sanal makine özellikleri sayfasına geri dönün. Yeni bir sabit disk, sanal makinenize eklenir. **Son**'a tıklayın.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Gidin, sağ bölmede seçili ile sanal makine, **özeti** sekmesi. Sanal makineniz için ayarları gözden geçirin.

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Sanal makineniz sağlanır. Bir sonraki adım bu makineyi açmak ve IP adresini almaktır.

> [!NOTE]
> VMware araçları sanal diziniz (yukarıda sağlanan gibi) yüklememenizi öneririz. VMware araçlarının yüklenmesi desteklenmeyen bir yapılandırmaya neden olabilir.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>3. Adım: Sanal cihazı başlatma ve IP adresini alma
Sanal cihazınızı başlatmak ve bağlantı kurmak için aşağıdaki adımları izleyin.

#### <a name="to-start-the-virtual-device"></a>Sanal cihazı başlatmak için
1. Sanal cihazı başlatın. Sol bölmede, Configuration Manager, vSphere Cihazınızı seçin ve bağlam menüsünü açmak için sağ tıklayın. **Power** (Güç) ve ardından **Power on** (Aç) seçimini yapın. Bu işlemin ardından makinenizin açılması gerekir. Alt durum görüntüleyebilirsiniz **son kullanılan görevler** bölmesinde vSphere istemcisi.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Kurulum görevleri tamamlamak için birkaç dakika sürer. Cihaz çalışmaya başladıktan sonra gidin **konsol** sekmesi. Cihazında oturum açmak için Ctrl + Alt + Delete Gönder. Alternatif olarak, konsol penceresinde imleç gelin ve Ctrl + Alt + INSERT tuşuna basın. Varsayılan kullanıcı *StorSimpleAdmin* ve varsayılan parola *Password1*.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Güvenlik nedeniyle cihazın yönetici parolasının ilk oturum açma işleminin ardından değiştirilmesi gerekir. Parolayı değiştirmeniz istenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. En az 8 karakterden oluşan bir parola girin. Bu gereksinimlerin 3 / 4 parola içermelidir: büyük harf, küçük harfler, sayısal ve özel karakter. Onaylamak için parolayı yeniden girin. Parola değiştirildi bildirilir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Parola başarıyla değiştirildikten sonra sanal cihaz yeniden başlatılabilir. Tamamlamak için sistemin yeniden başlatılmasını bekleyin. Cihazın Windows PowerShell konsolu ile birlikte bir ilerleme çubuğu görüntülenir.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Adım 6-8 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 9. adımla devam edebilirsiniz. DHCP olmayan Ortam aygıtınızda'kurmak önyükleme aşağıdaki ekranı görürsünüz.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Ardından, ağ yapılandırın.
7. Kullanım `Get-HcsIpAddress` sanal Cihazınızda etkin ağ arabirimleri listelemek için komutu. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıda bir örnek gösterilmiştir:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Web kullanıcı Arabirimine sanal cihazınızın bağlanmak ve yerel kurulumu ve kaydı tamamlamak için bu IP adresi kullanır.

   ![](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. (İsteğe bağlı) Yalnızca kamu bulutunda Cihazınızı dağıtıyorsanız bu adımı gerçekleştirin. Bu gibi durumlarda, Amerika Birleşik Devletleri Federal Bilgi İşleme Standardı (FIPS) modunu artık Cihazınızda sağlayacaktır. FIPS 140 standardı, hassas verilerin korunması için ABD Federal hükümeti bilgisayar sistemleri tarafından kullanım için onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunu etkinleştirmek için aşağıdaki cmdlet'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. Şifreleme doğrulama etkili olacak şekilde FIPS modundayken etkinleştirdikten sonra Cihazınızı yeniden başlatın.

       > [!NOTE]
       > Etkinleştirebilir veya FIPS modundayken, Cihazınızda devre dışı bırakın. Cihaz FIPS ve FIPS olmayan modu arasında geçiş yapma desteklenmiyor.
       >
       >

Cihazınız minimum yapılandırma gereksinimlerini karşılamıyorsa başlık metninde hata iletisi görüntülenir (aşağıda gösterilmiştir). Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirmeniz gerekir. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. En düşük yapılandırma gereksinimlerini başvurmak [1. adım: Ana bilgisayar sistemi en düşük sanal cihaz gereksinimleri karşıladığından emin olun](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Yerel web kullanıcı arabirimini kullanarak ilk yapılandırma sırasında herhangi bir hata yüz tanıma, aşağıdaki iş akışları için başvurun:

* Tanılama testleri [web kullanıcı Arabirimi Kurulumu sorunlarını giderme](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).
* [Günlük paketini oluşturma ve günlük dosyalarını görüntülemek](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Virtual Array'iniz bir dosya sunucusu ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple Virtual Array'iniz bir iSCSI sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md)
