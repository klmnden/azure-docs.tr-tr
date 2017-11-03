---
title: "StorSimple cihazınız için MPIO yapılandırma | Microsoft Docs"
description: "Windows Server 2012 R2 çalıştıran bir ana bilgisayara bağlı StorSimple cihazınız için çok yollu g/ç (MPIO) yapılandırılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 9fe3fa3a2df63d111de742ecb48b1469aad543cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multipath-io-for-your-storsimple-device"></a>Çok yollu g/ç StorSimple cihazınız için yapılandırma

Bu öğretici yüklemek ve Windows Server 2012 R2 çalıştıran bir konağa çok yollu g/ç (MPIO) özelliğini kullanmak için izlemeniz gereken ve StorSimple fiziksel cihaza bağlı adımlar açıklanmaktadır. Bu makaledeki Kılavuzu yalnızca fiziksel cihazları StorSimple 8000 serisi için geçerlidir. MPIO, StorSimple bulut uygulaması üzerinde şu anda desteklenmiyor.

Microsoft Windows Server Yardım derleme yüksek oranda kullanılabilir, hataya dayanıklı SAN yapılandırmaları için çok yollu g/ç (MPIO) özelliği için destek yerleşiktir. MPIO yedek fiziksel yol bileşenlerini kullanır — bağdaştırıcılar, kablolar ve anahtarlar — sunucu ile depolama cihazı arasında mantıksal yollar oluşturmak için. Bir mantıksal yol başarısız olmasına neden olan bir bileşen hata olduğunda çoklu yol oluşturma mantığı g/ç için alternatif bir yol böylece uygulamaların verilerine erişmeye devam kullanır. Ayrıca yapılandırmanıza bağlı olarak, MPIO de bu yollar genelinde yük dengelenmesi olarak performansı artırabilir. Daha fazla bilgi için bkz: [MPIO genel bakış](https://technet.microsoft.com/library/cc725907.aspx "MPIO genel bakış ve özellikleri").

Yüksek kullanılabilirlik için StorSimple çözümünüzün, MPIO, StorSimple Cihazınızda yapılandırılması gerekir. Windows Server 2012 R2 çalıştıran, ana bilgisayar sunucuları üzerinde MPIO yüklendiğinde sunucuları bağlantı, ağ veya arabirim hatalarına sonra dayanabilir.

## <a name="mpio-configuration-summary"></a>MPIO Yapılandırma Özeti

MPIO Windows Server'da isteğe bağlı bir özelliktir ve varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir.

MPIO, StorSimple Cihazınızda yapılandırmak için aşağıdaki adımları izleyin:

* 1. adım: Install Windows Server konağında MPIO
* 2. adım: StorSimple birimler için MPIO yapılandırma
* 3. adım: Bağlama StorSimple birimlerini ana bilgisayarda
* 4. adım: Yüksek kullanılabilirlik için MPIO yapılandırma ve Yük Dengelemesi

Yukarıdaki adımların her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>1. adım: Install Windows Server konağında MPIO

Bu özellik, Windows Server ana bilgisayarında yüklemek için aşağıdaki yordamı tamamlayın.

#### <a name="to-install-mpio-on-the-host"></a>Konakta MPIO'yu yüklemek için

1. Windows Server ana bilgisayarında Sunucu Yöneticisi'ni açın. Windows Server 2012 R2 veya Windows Server 2012 çalıştıran bir bilgisayardan Yöneticiler grubu açtığında üyesi olduğunda, varsayılan olarak, Sunucu Yöneticisi'ni başlatır. Sunucu Yöneticisi'ni zaten açık değilse tıklatın **Başlat > Sunucu Yöneticisi'ni**.
   
   ![Sunucu Yöneticisi](./media/storsimple-configure-mpio-windows-server/IC740997.png)

2. Tıklatın **Sunucu Yöneticisi'ni > Pano > Rol ve Özellik Ekle**. Bu başlatır **rol ve Özellik Ekle** Sihirbazı.
   
   ![Roller ve Özellikler Sihirbazı 1 ekleme](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. İçinde **rol ve Özellik Ekle** Sihirbazı, aşağıdaki adımları gerçekleştirin:
   
   1. Üzerinde **başlamadan önce** sayfasında, **sonraki**.
   2. Üzerinde **yükleme türünü seçin** sayfasında, varsayılan ayarı kabul **rol tabanlı veya özellik tabanlı** yükleme. **İleri**’ye tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 2 Ekle](./media/storsimple-configure-mpio-windows-server/IC740999.png)
   3. Üzerinde **Select hedef sunucu** sayfasında, **sunucu havuzundan bir sunucu seçin**. Ana bilgisayar sunucunuz otomatik olarak bulunması gerekir. **İleri**’ye tıklayın.
   4. Üzerinde **sunucu rollerini seçin** sayfasında, **sonraki**.
   5. Üzerinde **seçin özellikleri** sayfasında **çok yollu g/ç**, tıklatıp **sonraki**.
   
       ![Rol ve Özellik Ekleme Sihirbazı 5 Ekle](./media/storsimple-configure-mpio-windows-server/IC741000.png)
   6. Üzerinde **Yükleme Seçimlerini Onayla** sayfasında seçimini onaylayın ve ardından **gerekirse hedef sunucuyu otomatik olarak yeniden**, aşağıda gösterildiği gibi. **Yükle**'ye tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 8 Ekle](./media/storsimple-configure-mpio-windows-server/IC741001.png)
   7. Yükleme tamamlandığında, size bildirilir. Sihirbazı kapatmak için **Kapat**'a tıklayın.
   
       ![Rol ve Özellik Ekleme Sihirbazı 9 Ekle](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. adım: StorSimple birimler için MPIO yapılandırma

MPIO, StorSimple birimlerini tanımlamak için yapılandırılmalıdır. StorSimple birimlerini tanımak için MPIO yapılandırmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>StorSimple birimleri için MPIO yapılandırma

1. Açık **MPIO Yapılandırması**. Tıklatın **Sunucu Yöneticisi'ni > Pano > Araçlar > MPIO**.
2. İçinde **MPIO Özellikleri** iletişim kutusunda **çoklu yolları Bul** sekmesi.
3. Seçin **iSCSI aygıtları için destek eklemek**ve ardından **Ekle**.  
   ![MPIO özellikleri çoklu yolları Bul](./media/storsimple-configure-mpio-windows-server/IC741003.png)
4. İstendiğinde sunucusunu yeniden başlatın.
5. İçinde **MPIO Özellikleri** iletişim kutusu, tıklatın **MPIO cihazları** sekmesi. **Ekle**'ye tıklayın.
    </br>![MPIO özellikleri MPIO cihazları](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. İçinde **MPIO desteği Ekle** iletişim kutusunda **aygıt donanım kimliği**, cihaz seri numarasını girin. Cihaz seri numarasını almak için StorSimple cihaz Yöneticisi hizmetinize erişmek. Gidin **cihazlar > Pano**. Cihaz seri numarasını sağa görüntülenir **Hızlı Bakış** cihaz Pano bölmesi.
    </br>
    ![MPIO desteği Ekle](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. İstendiğinde sunucusunu yeniden başlatın.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>3. adım: Bağlama StorSimple birimlerini ana bilgisayarda

Windows Server'da MPIO yapılandırıldıktan sonra StorSimple cihazında oluşturulan birimlerin bağlanabilir ve ardından MPIO artıklığı avantajından yararlanabilirsiniz. Bir birim için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-volumes-on-the-host"></a>Birimleri konakta bağlamak için

1. Açık **iSCSI başlatıcısı özellikleri** Windows Server ana penceresinde. Tıklatın **Sunucu Yöneticisi'ni > Pano > Araçlar > iSCSI başlatıcısı**.
2. İçinde **iSCSI başlatıcısı özellikleri** iletişim kutusu, bulma sekmesini tıklatın ve ardından **Hedef Portal Bul**.
3. İçinde **Hedef Portal Bul** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   1. VERİ bağlantı noktası StorSimple cihazınızın IP adresini girin (örneğin, veri 0 girin).
   2. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
     
     > [!IMPORTANT]
     > **İSCSI bağlantıları için bir özel ağ kullanıyorsanız, özel ağa bağlı veri bağlantı noktası IP adresini girin.**
    
4. Cihazınızda ikinci bir ağ arabirimi (örneğin, veri 1) için 2-3 adımları yineleyin. Bu arabirimleri için iSCSI etkin olması gerektiğini aklınızda bulundurun. Daha fazla bilgi için bkz: [ağ arabirimleri değiştirme](storsimple-8000-modify-device-config.md#modify-network-interfaces).
5. Seçin **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** iletişim kutusu. StorSimple cihazı görmelisiniz IQN altında hedef **bulunan hedefler**.

   ![iSCSI Başlatıcı özellikleri hedefler sekmesi](./media/storsimple-configure-mpio-windows-server/IC741007.png)
   
6. Tıklatın **Bağlan** StorSimple cihazınızla bir iSCSI oturumu. A **hedef Bağlan** iletişim kutusu görüntülenir.
7. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusu. Tıklatın **Gelişmiş**.
8. İçinde **Gelişmiş ayarları** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:
   
   1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinden, **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ana bilgisayarın IP adresi seçin.
   3. Üzerinde **hedef portalı** IP aşağı açılan listesinde, aygıt arabiriminin bir IP adresi seçin.
   4. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
9. **Özellikler**'e tıklayın. İçinde **özellikleri** iletişim kutusu, tıklatın **oturum Ekle**.
10. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusu. Tıklatın **Gelişmiş**.
11. İçinde **Gelişmiş ayarları** iletişim kutusunda:

    1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinde, select Microsoft iSCSI başlatıcısı.
    2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ana bilgisayara karşılık gelen IP adresi seçin. Bu durumda, ana bilgisayardaki tek bir ağ arabirimi için cihazda iki ağ arabirimi bağlanan. Bu nedenle, bu arabirim ilk oturum için sağlanan aynı değil.
    3. Üzerinde **Hedef Portal IP** IP adresi cihazda etkin ikinci veri arabirimi için aşağı açılan listesinde seçin.
    4. Tıklatın **Tamam** iSCSI başlatıcısı Özellikleri iletişim kutusuna geri dönmek için. İkinci bir oturum hedef eklediniz.
12. Açık **Bilgisayar Yönetimi** giderek **Sunucu Yöneticisi'ni > Pano > Bilgisayar Yönetimi**. Sol bölmede **depolama > Disk Yönetimi**. Bu konak için görünür olan StorSimple cihazında oluşturulan birimi altında görünür **Disk Yönetimi** yeni diskler olarak.
13. Diski başlatın ve yeni bir birim oluşturun. Biçimlendirme işlemi sırasında 64 KB blok boyutunu seçin.
    
    ![Disk Yönetimi](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Altında **Disk Yönetimi**, sağ **Disk** seçip **özellikleri**.
15. StorSimple modelinde ### **çok yollu Disk cihaz özellikleri** iletişim kutusu, tıklatın **MPIO** sekmesi.
    
    ![StorSimple 8100 çok yollu Disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)
16. İçinde **DSM adı** 'yi tıklatın **ayrıntıları** ve parametreler için varsayılan parametreleri ayarlandığını doğrulayın. Varsayılan parametreler şunlardır:
    
    * Yolunu doğrulayın süresi = 30
    * Yeniden deneme sayısı = 3
    * PDO kaldırma süresi 20 =
    * Yeniden deneme aralığı = 1
    * Yolunu doğrulayın etkin = işaretli.

> [!NOTE]
> **Varsayılan parametreleri değiştirmeyin.**


## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>4. adım: Yüksek kullanılabilirlik için MPIO yapılandırma ve Yük Dengelemesi

Çok yollu tabanlı yüksek kullanılabilirlik ve Yük Dengeleme için kullanılabilir farklı yollar bildirmek için birden çok oturumu el ile eklenmesi gerekir. Örneğin, ana bilgisayar varsa, iki arabirim SAN'a bağlı ve SAN'a bağlı iki arabirim aygıtın ardından (her bir veri arabirimi ve ana bilgisayar arabirimi farklı bir IP alt ağda ise ve yönlendirilebilir değil yalnızca iki oturumları gerekli olacaktır) uygun yolu permütasyon ile yapılandırılmış dört oturumları gerekir.

**Cihaz ve uygulama ana bilgisayarı arasındaki en az 8 etkin paralel oturumları sahip olmasını öneririz.** Bu, Windows Server sisteminizdeki 4 ağ arabirimleri etkinleştirerek elde edilebilir. Donanım veya işletim sistemi düzeyi, Windows Server ana bilgisayardaki fiziksel ağ arabirimlerine veya sanal arabirimleri aracılığıyla ağ sanallaştırma teknolojileri kullanın. Cihazda iki ağ arabirimi ile bu yapılandırmayı 8 etkin oturumlarını neden olur. Bu yapılandırma, aygıt ve bulut verimliliği en iyi duruma getirme yardımcı olur.

> [!IMPORTANT]
> **1 GbE ve 10 GbE ağ arabirimleri karıştırmamanızı öneririz. İki ağ arabirimi kullanırsanız, her iki arabirimde aynı türde olmalıdır.**

Aşağıdaki yordam, StorSimple cihazı iki ağ arabirimi ile iki ağ arabirimi ile bir ana bilgisayara bağlandığında oturumları eklemeyi açıklar. Bu, yalnızca 4 oturumları sağlar. Bu yordamı StorSimple cihazı ile dört ağ arabirimlerine sahip bir ana bilgisayara bağlı iki ağ arabirimi ile kullanın. Burada açıklanan 4 oturumları yerine 8 yapılandırmanız gerekir.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Yüksek kullanılabilirlik için MPIO yapılandırma ve Yük Dengeleme

1. Hedef bir bulma gerçekleştir: içinde **iSCSI başlatıcısı özellikleri** iletişim kutusundaki **bulma** sekmesini tıklatın, **Portal Bul**.
2. İçinde **hedef Bağlan** iletişim kutusunda, aygıt ağ arabirimlerinden biri, IP adresini girin.
3. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
4. İçinde **iSCSI başlatıcısı özellikleri** iletişim kutusunda **hedefleri** sekmesinde bulunan hedef vurgulayın ve ardından **Bağlan**. **Hedef Bağlan** iletişim kutusu görüntülenir.
5. İçinde **hedef Bağlan** iletişim kutusunda:
   
   1. Varsayılan olarak bırakın seçili hedefi ayarları **Bu bağlantı Ekle** sık kullanılan hedefler için. Bu, otomatik olarak bu bilgisayarı yeniden başlatır her zaman bağlantı yeniden başlatmaya aygıt hale getirir.
   2. Seçin **etkinleştir çok yollu** onay kutusu.
   3. Tıklatın **Gelişmiş**.
6. İçinde **Gelişmiş ayarları** iletişim kutusunda:
   
   1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinden, **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ana bilgisayarın IP adresi seçin.
   3. Üzerinde **Hedef Portal IP** veri arabiriminin IP adresini etkin cihazda aşağı açılan listesinde, seçin.
   4. Tıklatın **Tamam** iSCSI başlatıcısı Özellikleri iletişim kutusuna geri dönmek için.
7. ' I tıklatın **özellikleri**hem de **özellikleri** iletişim kutusu, tıklatın **oturum Ekle**.
8. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusunu işaretleyin ve ardından **Gelişmiş**.
9. İçinde **Gelişmiş ayarları** iletişim kutusunda:
   
   1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinden, **Microsoft iSCSI başlatıcısı**.
   2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ikinci arabirim ana bilgisayarda karşılık gelen IP adresi seçin.
   3. Üzerinde **Hedef Portal IP** IP adresi cihazda etkin ikinci veri arabirimi için aşağı açılan listesinde seçin.
   4. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu. Şimdi, ikinci bir oturum hedef eklediniz.
10. Hedef ek oturumlar (yol) eklemek için 8-10 adımları yineleyin. Ana bilgisayarda iki arabirim ve cihazda iki, dört oturumları toplam ekleyebilirsiniz.
11. İstenen oturumları (yol) içinde ekledikten sonra **iSCSI başlatıcısı özellikleri** iletişim kutusunda, hedef seçin ve tıklatın **özellikleri**. Oturumları sekmesinde **özellikleri** iletişim kutusu, Not dört oturum olası yolu permütasyon karşılık tanımlayıcıları. Bir oturum iptal etmek, bir oturum tanımlayıcısı yanındaki onay kutusunu seçin ve ardından **Bağlantıyı Kes**.
12. Oturumlarla sunulan cihazları görüntülemek için seçin **aygıtları** sekmesi. Seçili cihaz için MPIO İlkesi yapılandırmak için tıklatın **MPIO**. **Cihaz ayrıntıları** iletişim kutusu görüntülenir. Üzerinde **MPIO** sekmesinde, seçebileceğiniz uygun **Yük Dengeleme İlkesi** ayarlar. Ayrıca görüntüleyebilirsiniz **etkin** veya **bekleme** yol türü.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [StorSimple cihaz yapılandırmasını değiştirmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-modify-device-config.md).

