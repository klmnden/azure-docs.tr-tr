---
title: "StorSimple sanal diziye bağlı ana bilgisayar MPIO'yu yapılandırmanızı | Microsoft Docs"
description: "StorSimple sanal Windows Server 2012 R2 çalıştıran bir ana bilgisayara bağlı Diziniz için çok yollu g/ç (MPIO) yapılandırılacağını açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: c75c6ed40754aee964e2b68f4f569dc1422507f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Windows Server konağında StorSimple sanal dizisi için çok yollu g/ç yapılandırın
## <a name="overview"></a>Genel Bakış
Bu makalede, Windows Server ana bilgisayarında çok yollu g/ç özelliğini (MPIO) yükleme, yalnızca StorSimple birimler için belirli yapılandırma ayarları uygulamak ve ardından MPIO StorSimple birimlerini doğrulayın. Yordamı StorSimple 1200 sanal dizinizi iki ağ arabirimi ile iki ağ arabirimi ile bir Windows Server ana bilgisayara bağlı olduğu varsayılır. Bu makalede yer alan bilgileri yalnızca sanal dizinin için geçerlidir. StorSimple 8000 serisi cihazlar hakkında daha fazla bilgi için Git [StorSimple ana bilgisayar için MPIO yapılandırma](storsimple-configure-mpio-windows-server.md). 

Windows Server yardımcı derleme yüksek oranda kullanılabilir, hataya dayanıklı bir depolama yapılandırmaları için MPIO özelliği. MPIO yedek fiziksel yol bileşenlerini kullanır — bağdaştırıcılar, kablolar ve anahtarlar — sunucu ile depolama cihazı arasında mantıksal yollar oluşturmak için. Bir mantıksal yol başarısız olmasına neden olan bir bileşen hata olduğunda çoklu yol oluşturma mantığı g/ç için alternatif bir yol böylece uygulamaların verilerine erişmeye devam kullanır. Ayrıca yapılandırmanıza bağlı olarak, MPIO de bu yollar genelinde yeniden yük dengeleyerek performansı artırabilir. Daha fazla bilgi için bkz: [MPIO genel bakış](https://technet.microsoft.com/library/cc725907.aspx "MPIO genel bakış ve özellikleri").

Yüksek kullanılabilirlik için StorSimple çözümünüzün, MPIO, StorSimple sanal dizinin (modeli 1200) bağlı Windows Server konakları yapılandırın. Ana bilgisayar sunucuları, ardından bağlantı, ağ veya arabirim hatalarına dayanabilir. 

MPIO yapılandırmak için şu adımları izlemesi gerekir: 

* Yapılandırma önkoşulları
* 1. adım: Install Windows Server konağında MPIO
* 2. adım: StorSimple birimler için MPIO yapılandırma
* 3. adım: Bağlama StorSimple birimlerini ana bilgisayarda

Yukarıdaki adımların her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="prerequisites"></a>Ön koşullar
Bu bölümde Windows Server konak ve sanal dizinizi için yapılandırma önkoşulları ayrıntılarını verir.

### <a name="on-windows-server-host"></a>Windows Server ana bilgisayarında
* Windows Server konağınızda 2 ağ arabirimleri etkin olduğundan emin olun.

### <a name="on-storsimple-virtual-array"></a>StorSimple sanal dizisinde
* Sanal dizinin bir iSCSI sunucusu olarak yapılandırılması gerekir. Daha fazla bilgi için bkz: [sanal bir dizi bir iSCSI sunucu olarak ayarlamak](storsimple-virtual-array-deploy3-iscsi-setup.md). Bir veya daha fazla ağ arabirimine dizi etkinleştirilmiş olmalıdır.   
* Sanal dizinizi ağ arabirimlerindeki Windows Server konaktan erişilebilir olmalıdır.
* Bir veya daha fazla birim, StorSimple sanal dizisinde oluşturulmalıdır. Daha fazla bilgi için bkz: [birim Ekle](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) , StorSimple sanal dizi. Bu yordamda, sanal dizi 3 birimleri (1 yerel olarak sabitlenmiş ve 2 katmanlı gösterildiği gibi aşağıdaki) oluşturduk.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>StorSimple sanal dizinin için donanım yapılandırması
Aşağıdaki şekilde, yüksek kullanılabilirlik için donanım yapılandırması ve StorSimple sanal bu yordamda kullanılan dizisi ve Windows Server ana bilgisayar için çoklu yol oluşturma Yük Dengeleme gösterilmektedir.

![MPIO donanım yapılandırması](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Yukarıdaki şekilde gösterildiği gibi:

* StorSimple sanal Hyper-V üzerinde sağlanan dizinizi iSCSI sunucusu olarak yapılandırılmış bir tek düğümlü etkin aygıttır.
* İki sanal ağ arabirimi, dizi etkinleştirilir. Yerel web kullanıcı Arabirimi, sanal dizinin, iki ağ arabirimi giderek etkinleştirildiğini doğrulayın **ağ ayarlarını** aşağıda gösterildiği gibi:
  
    ![1200 üzerinde etkin ağ arabirimleri](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Etkin ağ arabirimlerinin (Ethernet, Ethernet 2 varsayılan olarak) IPv4 adreslerini not alın ve ana bilgisayarda daha sonra kullanmak için kaydedin.
* İki ağ arabirimi, Windows Server ana bilgisayarında etkinleştirilir. Ana bilgisayar ve dizi için bağlı arabirimleri aynı alt ağda varsa, ardından olacaktır 4 yolları kullanılabilir. Bu yordamı durumda, bu. Her bir ağ arabirimine dizi ve ana bilgisayar arabirimde farklı bir IP alt ağ (ve değil yönlendirilebilir) varsa, ancak sonra yalnızca 2 yolları kullanılabilir.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>1. adım: Install Windows Server konağında MPIO
MPIO Windows Server'da isteğe bağlı bir özelliktir ve varsayılan olarak yüklenmez. Sunucu Yöneticisi aracılığıyla bir özellik olarak yüklenmesi gerekir. Bu özellik, Windows Server ana bilgisayarında yüklemek için aşağıdaki yordamı tamamlayın.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>2. adım: StorSimple birimler için MPIO yapılandırma
MPIO, StorSimple birimlerini tanımlamak için yapılandırılması gerekir. StorSimple birimlerini tanımak için MPIO yapılandırmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>3. adım: Bağlama StorSimple birimlerini ana bilgisayarda
Windows Server'da MPIO yapılandırıldıktan sonra StorSimple dizisinde oluşturulan birimlerin bağlanabilir ve ardından MPIO artıklığı avantajından yararlanabilirsiniz. Bir birim için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-volumes-on-the-host"></a>Birimleri konakta bağlamak için
1. Açık **iSCSI başlatıcısı özellikleri** Windows Server ana penceresinde. Git **Sunucu Yöneticisi'ni > Pano > Araçlar > iSCSI başlatıcısı**.
2. İçinde **iSCSI başlatıcısı özellikleri** iletişim kutusu, tıklatın **bulma**ve ardından **Hedef Portal Bul**.
3. İçinde **Hedef Portal Bul** iletişim kutusunda, aşağıdakileri yapın:
   
    1. İlk etkin ağ arabiriminin IP adresini, StorSimple sanal dizisindeki girin. Varsayılan olarak, bu olacaktır **Ethernet**. 
    2. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
     
    > [!IMPORTANT]
    > İSCSI bağlantıları için bir özel ağ kullanıyorsanız, özel ağa bağlı veri bağlantı noktası IP adresini girin.
     
4. Dizinizi üzerinde ikinci bir ağ arabirimi (örneğin, Ethernet 2) için 2-3 adımları yineleyin. 
5. Seçin **hedefleri** sekmesinde **iSCSI başlatıcısı özellikleri** iletişim kutusu. Sanal dizinizi için altında hedef olarak her birim yüzeyini görmelisiniz **bulunan hedefler**. Bu durumda, üç hedefleri (üç birimlere karşılık gelen) bulunması.
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Tıklatın **Bağlan** StorSimple sanal dizinizi ile iSCSI oturumu. A **hedef Bağlan** iletişim kutusu görüntülenir. Seçin **etkinleştir çok yollu** onay kutusu. Tıklatın **Gelişmiş**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. İçinde **Gelişmiş ayarları** iletişim kutusunda, aşağıdakileri yapın:                                        
   
    1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinden, **Microsoft iSCSI başlatıcısı**.
    2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ana bilgisayarın IP adresi seçin.
    3. Üzerinde **hedef portalı** IP aşağı açılan listesinde, dizi arabiriminin bir IP adresi seçin.
    4. Tıklatın **Tamam** geri dönmek için **iSCSI başlatıcısı özellikleri** iletişim kutusu.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. **Özellikler**'e tıklayın. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. İçinde **özellikleri** iletişim kutusu, tıklatın **oturum Ekle**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. İçinde **hedef Bağlan** iletişim kutusunda **etkinleştir çok yollu** onay kutusu. Tıklatın **Gelişmiş**.
11. İçinde **Gelişmiş ayarları** iletişim kutusunda:                                        
    
    1. Üzerinde **yerel bağdaştırıcı** aşağı açılan listesinde, select Microsoft iSCSI başlatıcısı.

    2. Üzerinde **Başlatıcı IP** aşağı açılan listesinde, ana bilgisayara karşılık gelen IP adresi seçin. Bu durumda, tek bir ağ arabirimi ana bilgisayarda iki ağ arabirimi dizisindeki bağlandığınız. Bu nedenle, bu arabirim ilk oturum için sağlanan aynı değil.

    3. Üzerinde **Hedef Portal IP** IP adresi dizi etkin ikinci veri arabirimi için aşağı açılan listesinde seçin.

    4. Tıklatın **Tamam** iSCSI başlatıcısı Özellikleri iletişim kutusuna geri dönmek için. İkinci bir oturum hedef eklediniz.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. İstenen oturumları (yol) içinde ekledikten sonra **iSCSI başlatıcısı özellikleri** iletişim kutusunda, hedef seçin ve tıklatın **özellikleri**. Oturumları sekmesinde **özellikleri** iletişim kutusu, Not dört oturum olası yolu permütasyon karşılık tanımlayıcıları. Bir oturum iptal etmek, bir oturum tanımlayıcısı yanındaki onay kutusunu seçin ve ardından **Bağlantıyı Kes**.

    6. Oturumlarla sunulan cihazları görüntülemek için seçin **aygıtları** sekmesi. Seçili cihaz için MPIO İlkesi yapılandırmak için tıklatın **MPIO**.

    7. **Ayrıntıları** iletişim kutusu görüntülenir. Üzerinde **MPIO** sekmesinde, seçebileceğiniz uygun **Yük Dengeleme İlkesi** ayarlar. Ayrıca görüntüleyebilirsiniz **etkin** veya **bekleme** yol türü.
12. Hedef ek oturumlar (yol) eklemek için 8-11 adımları yineleyin. Ana bilgisayarda iki arabirim ve iki sanal dizisindeki, her hedef için dört oturumları toplam ekleyebilirsiniz. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Her birim (yüzeyleri hedefi olarak) için bu adımları yinelemeniz gerekecek.
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Açık **Bilgisayar Yönetimi** giderek **Sunucu Yöneticisi'ni > Pano > Bilgisayar Yönetimi**. Sol bölmede **depolama > Disk Yönetimi**. Bu konak için görünür olan StorSimple sanal dizisinde oluşturulan birimleri altında görünür **Disk Yönetimi** yeni diskler olarak.
15. Diski başlatın ve yeni bir birim oluşturun. Biçimlendirme işlemi sırasında 64 KB ayırma birimi boyutu (Avustralya) seçin. Kullanılabilir tüm birimleri için bu işlemi yineleyin.
    
    ![Disk Yönetimi](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. Altında **Disk Yönetimi**, sağ **Disk** seçip **özellikleri**.
17. İçinde **çok yollu Disk cihaz özellikleri** iletişim kutusu, tıklatın **MPIO** sekmesi.
    
    ![Disk özellikleri](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. İçinde **DSM adı** 'yi tıklatın **ayrıntıları** ve parametreler için varsayılan parametreleri ayarlandığını doğrulayın. Varsayılan parametreler şunlardır:
    
    * Yolunu doğrulayın süresi = 30
    * Yeniden deneme sayısı = 3
    * PDO kaldırma süresi 20 =
    * Yeniden deneme aralığı = 1
    * Yolunu doğrulayın etkin = işaretli.
    
    > [!NOTE]
    > **Varsayılan parametreleri değiştirmeyin.**
   
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [StorSimple sanal dizinizi yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-virtual-array-manager-service-administration.md).

