---
title: StorSimple cihazınız için MPIO yapılandırma | Microsoft Docs
description: Windows Server 2012 R2 çalıştıran bir konağa bağlı StorSimple cihazınız için çok yollu g/ç (MPIO) yapılandırılması açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/26/2018
ms.author: alkohli
ms.openlocfilehash: eda134257edb851eea076459b44e02fc59028f46
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60363402"
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Çok yollu g/ç StorSimple cihazınız için yapılandırma

Bu öğreticide, yüklemek ve Windows Server 2012 R2 çalıştıran bir konağa çok yollu g/ç (MPIO) özelliğini kullanmak için uymalı ve bağlı bir StorSimple fiziksel cihaz için gereken adımlar açıklanmaktadır. Bu makaledeki Kılavuzu, StorSimple 8000 serisi fiziksel cihazlar yalnızca geçerlidir. MPIO, StorSimple Cloud Appliance'ta şu anda desteklenmiyor.

Microsoft Windows Server'ın yüksek oranda kullanılabilir, hataya dayanıklı bir iSCSI ağ yapılandırmaları oluşturmanıza yardımcı olmak için çok yollu g/ç (MPIO) özelliği için destek yerleşik. MPIO yedek fiziksel yol bileşenlerini kullanır; bağdaştırıcılar, kablolar ve anahtarlar — sunucu ile depolama cihazı arasında mantıksal yollar oluşturmak için. Bir mantıksal yol başarısız olmasına neden olan bir bileşeni hatası ise uygulamaların verilerine erişmeye devam edebilir, böylece çoklu yol oluşturma mantığı g/ç için alternatif bir yol kullanır. Buna ek olarak yapılandırmanıza bağlı olarak, MPIO de bu yollar genelinde yükü yeniden Dengeleme performansını geliştirebilirsiniz. Daha fazla bilgi için [MPIO genel bakış](https://technet.microsoft.com/library/cc725907.aspx "MPIO genel bakış ve özellikleri").

Yüksek kullanılabilirlik için StorSimple çözümünün, MPIO, StorSimple Cihazınızda yapılandırılmalıdır. Windows Server 2012 R2 çalıştıran ana bilgisayar sunucuları üzerinde MPIO yüklendiğinde, sunucuların bağlantı, ağ ve arabirim hatalarından sonra dayanabilir.

## <a name="mpio-configuration-summary"></a>MPIO yapılandırması özeti

MPIO, Windows Server'da isteğe bağlı bir özelliktir ve varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir.

MPIO, StorSimple Cihazınızı yapılandırmak için aşağıdaki adımları izleyin:

* 1\. adım: Windows Server ana bilgisayarında MPIO yükleme
* 2\. adım: StorSimple birimlerini için MPIO yapılandırma
* 3\. adım: Konaktaki bağlama StorSimple birimlerini
* 4\. Adım: Yük Dengeleme ve yüksek kullanılabilirlik için MPIO yapılandırma

Yukarıdaki adımların her biri, aşağıdaki bölümlerde ele alınmıştır.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>1\. adım: Windows Server ana bilgisayarında MPIO yükleme

Bu özellik Windows Server ana bilgisayarınıza yüklemek için aşağıdaki yordamı tamamlayın.

#### <a name="to-install-mpio-on-the-host"></a>Konakta MPIO yüklemek için

1. Windows Server ana bilgisayarında Sunucu Yöneticisi'ni açın. Varsayılan olarak, Sunucu Yöneticisi, Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir bilgisayara Yöneticiler grubu açtığında üyesi olduğunda başlatır. Sunucu Yöneticisi'ni zaten açık değilse **Başlat > Sunucu Yöneticisi**.
   
   ![Sunucu Yöneticisi](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Tıklayın **Sunucu Yöneticisi > Panosu > Rol ve Özellik Ekle**. Bu başlatır **rol ve Özellik Ekle** Sihirbazı.
   
   ![Rol ve özellikleri Sihirbazı 1 Ekle](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. İçinde **rol ve Özellik Ekle** Sihirbazı, aşağıdaki adımları gerçekleştirin:
   
   1. Üzerinde **başlamadan önce** sayfasında **sonraki**.
   2. Üzerinde **yükleme türünü seçin** sayfasında, varsayılan ayarı olan **rol tabanlı veya özellik tabanlı** yükleme. **İleri**’ye tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 2 Ekle](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Üzerinde **hedef sunucuyu seçin** sayfasında **sunucu havuzundan bir sunucu seçin**. Ana bilgisayar sunucunuz otomatik olarak bulunması gerekir. **İleri**’ye tıklayın.
   4. Üzerinde **Sunucu Rollerini Seç** sayfasında **sonraki**.
   5. Üzerinde **özellikleri seçin** sayfasında **çok yollu g/ç**, tıklatıp **sonraki**.
   
       ![Rol ve Özellik Ekleme Sihirbazı 5 Ekle](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Üzerinde **yükleme seçimlerini onaylayın** sayfasında seçimi onaylayın ve ardından **gerekirse hedef sunucuyu otomatik olarak yeniden**, aşağıda gösterildiği gibi. **Yükle**'ye tıklatın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 8 Ekle](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Yükleme tamamlandığında size bildirilir. Sihirbazı kapatmak için **Kapat**'a tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 9 Ekle](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2\. adım: StorSimple birimlerini için MPIO yapılandırma

MPIO, StorSimple birimlerini tanımlamak için yapılandırılmalıdır. MPIO, StorSimple birimlerini tanıyacak şekilde yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>StorSimple birimlerini için MPIO yapılandırma

1. Açık **MPIO Yapılandırması**. Tıklayın **Sunucu Yöneticisi > Panosu > Araçlar > MPIO**.
2. İçinde **MPIO Özellikleri** iletişim kutusunda **çoklu yolları Bul** sekmesi.
3. Seçin **iSCSI cihazlar için destek eklenecek**ve ardından **Ekle**.  
   ![MPIO özellikleri çoklu yolları Bul](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. İstendiğinde sunucuyu yeniden başlatın.
5. İçinde **MPIO Özellikleri** iletişim kutusu, tıklayın **MPIO cihazları** sekmesi. **Ekle**'yi tıklatın.
    </br>![MPIO özellikleri MPIO cihazları](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. İçinde **MPIO desteği Ekle** iletişim kutusunun **cihaz donanım kimliği**, cihaz seri numaranızı girin. Cihaz seri numarasını almak için StorSimple cihaz Yöneticisi hizmetine erişin. Gidin **cihazlar > Pano**. Cihaz seri numarasını sağa görüntülenen **Hızlı Bakış** cihaz panonun bölmesi.
    </br>
    ![MPIO desteği Ekle](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. İstendiğinde sunucuyu yeniden başlatın.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>3\. adım: Konaktaki bağlama StorSimple birimlerini

Windows Server'da MPIO yapılandırdıktan sonra oluşturulan StorSimple cihazında birim bağlanabilir ve ardından MPIO artıklık yararlanabilirsiniz. Bir birimi bağlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-volumes-on-the-host"></a>Birimleri konakta bağlamak için

1. Açık **iSCSI başlatıcısı özellikleri** Windows Server konağında penceresi. Tıklayın **Sunucu Yöneticisi > Panosu > Araçlar > iSCSI başlatıcısı**.
2. İçinde **iSCSI başlatıcısı özellikleri** iletişim kutusu, bulma sekmesine tıklayın ve ardından **Hedef Portal Bul**.
3. İçinde **Hedef Portal Bul** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   1. VERİ bağlantı noktası StorSimple cihazınızın IP adresini girin (örneğin, DATA 0 girin).
   2. Tıklayın **Tamam** dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
     
      > [!IMPORTANT]
      > **Özel bir ağ bağlantıları için iSCSI kullanıyorsanız, özel ağa bağlı veri bağlantı noktası IP adresini girin.**
    
4. Cihazınızda ikinci bir ağ arabirimi (örneğin, verileri 1) için 2-3 adımları yineleyin. Bu arabirimler için iSCSI etkin olması gerektiğini aklınızda bulundurun. Daha fazla bilgi için [ağ arabirimleri değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Seçin **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** iletişim kutusu. StorSimple cihazı görmeniz gerekir IQN altında hedef **bulunan hedefler**.

   ![iSCSI Başlatıcı özellikleri hedefler sekmesi](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Tıklayın **Connect** StorSimple Cihazınızı bir iSCSI oturumu oluşturmak için. A **hedef Bağlan** iletişim kutusu görüntülenir.
7. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusu. **Gelişmiş**'e tıklayın.
8. İçinde **Gelişmiş ayarlar** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   1. Üzerinde **yerel bağdaştırıcısı** aşağı açılan listesinden **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, konak IP adresi seçin.
   3. Üzerinde **hedef portalı** IP aşağı açılan listesinde, cihaz arabiriminin IP adresini seçin.
   4. Tıklayın **Tamam** dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
9. **Özellikler**'e tıklayın. İçinde **özellikleri** iletişim kutusu, tıklayın **oturum Ekle**.
10. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusu. **Gelişmiş**'e tıklayın.
11. İçinde **Gelişmiş ayarlar** iletişim kutusunda:

    1. Üzerinde **yerel bağdaştırıcısı** aşağı açılan listesinde, Microsoft iSCSI Başlatıcısı'nı seçin.
    2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, konağa karşılık gelen IP adresi seçin. Bu durumda, cihazda iki ağ arabirimi konak üzerindeki bir tek bir ağ arabirimi bağlandığınız. Bu nedenle, bu arabirim ilk oturum için sağlanan aynıdır.
    3. Üzerinde **hedef portalı IP** etkinleştirilmiş ikinci veri arabirimi için IP adresi aşağı açılan listesinde seçin.
    4. Tıklayın **Tamam** iSCSI başlatıcısı Özellikleri iletişim kutusuna dönmek için. İkinci bir oturumu hedef ekledik.
12. Açık **Bilgisayar Yönetimi** giderek **Sunucu Yöneticisi > Panosu > Bilgisayar Yönetimi**. Sol bölmede **depolama > Disk Yönetimi**. Bu konak için görünür olan StorSimple cihazında oluşturulan birimi altında görünür **Disk Yönetimi** yeni diskler olarak.
13. Diski başlatın ve yeni bir birim oluşturun. Biçimlendirme işlemi sırasında bir blok boyutu 64 KB'ı seçin.
    
    ![Disk Yönetimi](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Altında **Disk Yönetimi**, sağ **Disk** seçip **özellikleri**.
15. StorSimple modelinde ### **çok yollu Disk cihaz özelliklerini** iletişim kutusu, tıklayın **MPIO** sekmesi.
    
    ![StorSimple 8100 çok yollu Disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. İçinde **DSM adı** bölümünde **ayrıntıları** ve parametreler için varsayılan parametre olarak ayarlandığını doğrulayın. Varsayılan parametreler şunlardır:
    
    * Yolunu doğrulayın süresi = 30
    * Yeniden deneme sayısı 3 =
    * PDO kaldırma süresi = 20
    * Yeniden deneme aralığı = 1
    * Yolunu doğrulayın etkin = seçeneği işaretli değil.

> [!NOTE]
> **Varsayılan parametreleri değiştirmeyin.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>4\. Adım: Yük Dengeleme ve yüksek kullanılabilirlik için MPIO yapılandırma

Yüksek kullanılabilirlik ve Yük Dengeleme çok yollu tabanlı için kullanılabilen farklı yolları bildirmek için birden çok oturumu el ile eklenmesi gerekir. Örneğin, konak iki arabirim iSCSI ağa bağlı ve cihazın iSCSI ağa bağlı iki arabirim sahip ve ardından uygun yolu permütasyon ile yapılandırılmış dört oturumları gerekir (her, yalnızca iki oturumları gerekli olacak veri arabirimi ve konak farklı bir IP alt ağda bulunan ve arabirimi yönlendirilemez).

**Cihaz ve uygulama ana bilgisayarınız arasında en az 8 etkin paralel oturumları sahip olmasını öneririz.** Bu, Windows Server sisteminizin 4 ağ arabirimlerindeki etkinleştirerek gerçekleştirilebilir. Windows Server ana bilgisayarınızda donanım veya işletim sistemi düzeyinde fiziksel ağ arabirimlerine veya sanal arabirim aracılığıyla ağ sanallaştırma teknolojilerini kullanın. Cihazda iki ağ arabirimi ile bu yapılandırma 8 etkin oturumları neden olur. Bu yapılandırma, cihaz ve bulut aktarım hızını iyileştirme yardımcı olur.

> [!IMPORTANT]
> **1 GbE ve 10 GbE ağ arabirimleri karıştırmamanızı öneririz. İki ağ arabirimi kullanırsanız, her iki arabirimde bir aynı türden olmalıdır.**

Aşağıdaki yordamda, bir ana bilgisayara iki ağ arabirimi ile iki ağ arabirimi ile bir StorSimple cihazı bağlandığında oturumları eklemeyi açıklar. Bu, yalnızca 4 oturumları sağlar. Dört ağ arabirimine sahip bir ana bilgisayara bağlı iki ağ arabirimi ile bir StorSimple cihazı ile aynı prosedürü kullanın. Burada açıklanan 4 oturumları yerine 8 yapılandırmanız gerekecektir.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Yüksek kullanılabilirlik için MPIO'yu yapılandırın ve Yük Dengeleme

1. Hedef bir bulma gerçekleştir: içinde **iSCSI başlatıcısı özellikleri** iletişim kutusundaki **bulma** sekmesini tıklatın, **Portal Bul**.
2. İçinde **hedef Bağlan** iletişim kutusunda, cihaz ağ arabirimlerinden biri, IP adresini girin.
3. Tıklayın **Tamam** dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
4. İçinde **iSCSI başlatıcısı özellikleri** iletişim kutusunda **hedefleri** sekmesinde bulunan hedef vurgulayın ve ardından **Connect**. **Hedef Bağlan** iletişim kutusu görüntülenir.
5. İçinde **hedef Bağlan** iletişim kutusunda:
   
   1. Seçili hedef ayarı varsayılan olarak bırakın **Bu bağlantı Ekle** sık kullanılan hedefler listesine. Bu cihaz otomatik olarak bu bilgisayarı yeniden başlatır her zaman bağlantı yeniden başlatmaya sağlar.
   2. Seçin **etkinleştir çok yollu** onay kutusu.
   3. **Gelişmiş**'e tıklayın.
6. İçinde **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. Üzerinde **yerel bağdaştırıcısı** aşağı açılan listesinden **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ilk arabirimi (iSCSI arabirimi) ana bilgisayarda karşılık gelen IP adresi seçin.
   3. Üzerinde **hedef portalı IP** etkinleştirilmiş ilk veri arabirimi için IP adresi aşağı açılan listesinde seçin.
   4. Tıklayın **Tamam** iSCSI başlatıcısı Özellikleri iletişim kutusuna dönmek için.
7. Tıklayın **özellikleri**hem de **özellikleri** iletişim kutusu, tıklayın **oturum Ekle**.
8. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusunu işaretleyin ve ardından **Gelişmiş**.
9. İçinde **Gelişmiş ayarlar** iletişim kutusunda:
   
   1. Üzerinde **yerel bağdaştırıcısı** aşağı açılan listesinden **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ikinci bir iSCSI arabirimi ana bilgisayarda karşılık gelen IP adresi seçin.
   3. Üzerinde **hedef portalı IP** etkinleştirilmiş ikinci veri arabirimi için IP adresi aşağı açılan listesinde seçin.
   4. Tıklayın **Tamam** dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu. Şimdi, ikinci bir oturumu hedef ekledik.
10. Hedef ek oturumlar (yol) eklemek için 8-10 adımları yineleyin. Konaktaki iki arabirim ve cihazda iki, dört oturumların toplam ekleyebilirsiniz.
11. De (yol), istenen oturumları ekledikten sonra **iSCSI başlatıcısı özellikleri** iletişim kutusunda, hedef seçin ve tıklayın **özellikleri**. Oturumlarının sekmesinde **özellikleri** iletişim kutusunda, Not dört oturumu olası yolu permütasyon için karşılık gelen tanımlayıcıları. Bir oturumu iptal, oturum tanımlayıcıyı yanındaki onay kutusunu işaretleyin ve ardından **Bağlantıyı Kes**.
12. Oturumlarının içinde sunulan cihazları görüntülemek için seçin **cihazları** sekmesi. Seçili cihaz için MPIO İlkesi yapılandırmak için tıklayın **MPIO**. **Cihaz ayrıntıları** iletişim kutusu görüntülenir. Üzerinde **MPIO** sekmesinde, seçebileceğiniz uygun **Yük Dengeleme İlkesi** ayarları. Ayrıca görüntüleyebilirsiniz **etkin** veya **bekleme** yol türü.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [, StorSimple cihaz yapılandırmasını değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-modify-device-config.md).

