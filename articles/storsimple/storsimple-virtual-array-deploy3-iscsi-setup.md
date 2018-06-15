---
title: Microsoft Azure StorSimple sanal dizinin iSCSI sunucusu Kurulumu | Microsoft Docs
description: Cihaz kurulumunu tamamlayın, StorSimple iSCSI sunucuyu kaydetmek ve ilk kurulumu gerçekleştirmek açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 076df176d7cd40c009aea27004fe0f4415999c80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23877310"
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>StorSimple sanal dizinin – kümesi yukarı Azure Portalı aracılığıyla iSCSI sunucusu olarak dağıtma

![iSCSI Kurulum işlem akışı](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Genel Bakış

Bu dağıtım öğretici Microsoft Azure StorSimple sanal dizinin için geçerlidir. Bu öğretici, ilk kurulumu gerçekleştirmek, StorSimple iSCSI sunucunuza kaydetmek, cihaz kurulumunu tamamlayın ve ardından, bağlamak, başlatmak, oluşturup, StorSimple sanal bir iSCSI sunucusu olarak yapılandırılmış dizisindeki birimleri biçimlendirme açıklar. 

Açıklanan yordamları, yaklaşık 30 dakika tamamlamak için 1 saat buraya yazın. Bu makalede yayımlanmış bilgiler yalnızca StorSimple sanal diziler için geçerlidir.

## <a name="setup-prerequisites"></a>Kurulum Önkoşulları

Yapılandırmak ve StorSimple sanal dizinizi ayarlama önce emin olun:

* Sanal bir dizi sağlanan ve açıklandığı şekilde ona bağlı [dağıtmak StorSimple sanal dizinin - sağlama Hyper-V sanal bir dizide](storsimple-ova-deploy2-provision-hyperv.md) veya [dağıtmak StorSimple sanal dizinin - VMware sanal bir dizide sağlama ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hizmet kayıt anahtarı, StorSimple sanal dizileriniz yönetmek için oluşturduğunuz StorSimple Aygıt Yöneticisi'ni hizmetinden var. Daha fazla bilgi için bkz: **2. adım: Hizmet kayıt anahtarını alın** içinde [StorSimple sanal dizinin dağıtma - portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Mevcut bir StorSimple cihaz Yöneticisi hizmeti ile kaydetme ikinci veya sonraki sanal dizinin gerekiyorsa hizmet verileri şifreleme anahtarı olması gerekir. İlk aygıt hizmeti ile başarıyla kaydettirildi olduğunda bu anahtar oluşturuldu. Bu anahtar kaybettiyseniz, bkz: **hizmet verileri şifreleme anahtarı alma** içinde [StorSimple sanal dizinizi yönetmek için Web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Adım adım Kurulumu

Ayarlamak ve StorSimple sanal dizinizi yapılandırmak için aşağıdaki adım adım yönergeleri kullanın:

* [1. adım: yerel web kullanıcı Arabirimi Kurulumu tamamlamak ve Cihazınızı kaydedin](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* [2. adım: gerekli cihaz kurulumunu tamamlayın](#step-2-complete-the-required-device-setup)
* [3. adım: Birim Ekle](#step-3-add-a-volume)
* [4. adım: Bağlamak, başlatmak ve bir birimi biçimlendirme](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>1. adım: yerel web kullanıcı Arabirimi Kurulumu tamamlamak ve Cihazınızı kaydedin

#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlamak ve cihaz kaydetmek için

1. Bir tarayıcı penceresi açın. UI türü Web'e bağlanmak için:
   
    `https://<ip-address of network interface>`
   
    Önceki adımda not ettiğiniz bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında bir sorun olduğunu bildiren bir hata görürsünüz. Tıklatın **bu web sayfası için devam**.
   
    ![güvenlik sertifikası hatası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Web kullanıcı Arabirimine sanal cihazınızın oturum olarak **StorSimpleAdmin**. Adım 3'te değiştirdiğiniz cihaz Yöneticisi parolasını girin: sanal cihaz başlangıç [dağıtmak StorSimple sanal dizinin - sağlama Hyper-V'de sanal cihazı](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [dağıtmak StorSimple sanal dizinin - sağlama bir VMware sanal cihazı](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Oturum açma sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. İçin gerçekleştirilecek **giriş** sayfası. Bu sayfayı yapılandırmak ve sanal cihaz StorSimple cihaz Yöneticisi hizmeti ile kaydetmek için gereken çeşitli ayarlar açıklanır. Unutmayın **ağ ayarlarını**, **Web proxy ayarlarını**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **aygıt ayarları** ve **bulut ayarları**.
   
    ![Giriş sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. Üzerinde **ağ ayarlarını** altında sayfa **ağ arabirimleri**, DATA 0 otomatik olarak yapılandırılacaktır sizin için. Her ağ arabiriminin IP adresini otomatik olarak almak için varsayılan olarak ayarlanır (DHCP). Bu nedenle, bir IP adresi, alt ağ ve ağ geçidi otomatik olarak (IPv4 ve IPv6 için) atanır.
   
    Bir iSCSI sunucusuna (blok depolama alanı Hazırla) olarak Cihazınızı dağıtmayı planlarken, devre dışı bırakmanızı öneririz **IP adresini otomatik olarak almak** seçeneği ve statik IP adreslerini yapılandırın.
   
    ![Ağ Ayarları sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Aygıt sağlama işlemi sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Ağ arabirimi yalnızca veya IPv4 ve IPv6 IPv4 olarak yapılandırabilirsiniz unutmayın. IPv6 yalnızca yapılandırmaları desteklenmez.
5. DNS sunucuları, dosya sunucusu olarak yapılandırılmışsa, Cihazınızı adıyla çözmek için veya Bulut depolama hizmeti sağlayıcılarınızın ile iletişim kurmak için Cihazınızı denediğinde, bunlar kullanıldığından gereklidir. Üzerinde **ağ ayarlarını** altında sayfa **DNS sunucuları**:
   
   1. Bir birincil ve ikincil DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmak isterseniz, DNS sunucuları belirtebilirsiniz. Yüksek kullanılabilirlik için birincil ve ikincil bir DNS sunucusuna yapılandırmanızı öneririz.
   2. **Uygula**'ya tıklayın. Bu uygulama ve ağ ayarlarını doğrulayın.
6. Üzerinde **aygıt ayarları** sayfa:
   
   1. Benzersiz bir Ata **adı** aygıtınıza. Bu ad, 1-15 karakter olabilir ve harf, rakam ve tire içerebilir.
   2. Tıklatın **iSCSI sunucusu** simgesi ![iSCSI sunucu simgesi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) için **türü** oluşturmakta olduğunuz cihazın. Bir iSCSI sunucusunun sağlama blok depolama birimine izin verir.
   3. Bu cihaz etki alanına katılmış olmasını isteyip istemediğinizi belirtin. Cihazınızı bir iSCSI sunucusu ise, etki alanına katılmak isteğe bağlıdır. İSCSI sunucunuz bir etki alanına katılma değil karar verirseniz, tıklatın **Uygula**, ayarların uygulanması ve sonraki adıma atlayın bekleyin.
      
       Cihaz bir etki alanına katılmak istiyorsanız. Girin bir **etki alanı adı**ve ardından **Uygula**.
      
      > [!NOTE]
      > İSCSI sunucunuz bir etki alanına katılma, sanal dizinizi kendi kuruluş biriminde (OU) için Microsoft Azure Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) için uygulanan emin olun.
      > 
      > 
   4. Bir iletişim kutusu görünür. Belirtilen biçimde etki alanı kimlik bilgilerinizi girin. Onay simgesine tıklayarak ![onay simgesi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). Etki alanı kimlik bilgileri doğrulanır. Kimlik bilgileri yanlışsa, bir hata iletisi görürsünüz.
      
       ![kimlik bilgileri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. **Uygula**'ya tıklayın. Bu uygulama ve aygıt ayarlarını doğrulayın.
7. (İsteğe bağlı) web proxy sunucunuzu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca burada yapılandırabilirsiniz olduğunu unutmayın.
   
    ![Web Proxy'yi Yapılandır](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Üzerinde **Web proxy** sayfa:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http://host-IP adresi* veya *FDQN:Port numarası*. HTTPS URL'leri desteklenmez unutmayın.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulaması kullanıyorsanız, siz de sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılmış web proxy ayarlarını uygulayın.
8. (İsteğe bağlı) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. NTP sunucuları gerekli olduğu bulut hizmeti sağlayıcılarınızın ile doğrulanabilmesi Cihazınızı zaman eşitlemeniz gerekir.
   
    ![Saat ayarları](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Üzerinde **saat ayarlarını** sayfa:
   
   1. Aşağı açılan listesinden **saat dilimi** hangi cihaz dağıtıldığından coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
   2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
   3. İsteğe bağlı olarak belirtmek bir **ikincil NTP sunucusu** cihazınız için.
   4. **Uygula**'ya tıklayın. Bu doğrulamak ve yapılandırılmış zaman ayarlarını uygulayın.
9. Cihazınız için bulut ayarlarını yapılandırın. Bu adımda, yerel aygıt yapılandırmasını tamamlayın ve ardından StorSimple cihaz Yöneticisi hizmetiniz ile cihazı kaydedin.
   
   1. Girin **hizmet kayıt anahtarını** aldığınız **2. adım: Hizmet kayıt anahtarını alın** içinde [StorSimple sanal dizinin dağıtma - Portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Bu hizmeti ile kaydetme ilk aygıtı değilse, sağlamanız gerekir **hizmet verileri şifreleme anahtarı**. Bu anahtar StorSimple cihaz Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarı gereklidir. Daha fazla bilgi için bkz [hizmet verileri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) yerel web kullanıcı Arabirimi.
   3. Tıklatın **kaydetmek**. Bu, aygıt yeniden başlatılır. 2-3 cihaz sorunsuz kaydedildikten önce dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına yönlendirilirsiniz.
      
      ![Cihaz kaydetme](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Azure portalına geri dönün.
11. Gidin **aygıtları** hizmetinizin dikey. Çok fazla kaynak varsa, tıklatın **tüm kaynakları**, hizmet adınızı (Ara gerekiyorsa) tıklatın ve ardından **aygıtları**.
12. Üzerinde **aygıtları** dikey penceresinde, aygıtın başarıyla hizmete durumu arayarak bağlandığını doğrulayın. Cihazın durumu **Kuruluma hazır** olmalıdır.
    
    ![Cihaz kaydetme](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-the-device-as-iscsi-server"></a>2. adım: cihaz iSCSI sunucusu olarak yapılandırın.

Gerekli cihaz kurulumunu tamamlamak için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-the-device-as-iscsi-server"></a>Cihaz iSCSI sunucusu olarak yapılandırmak için

1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından Git **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde, seçin yeni cihaz oluşturulmuş. Bu cihazı olarak görüntülenebilir **ayarlamak hazır**.
   
    ![Aygıt iSCSI sunucusu olarak yapılandırın](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Cihaz tıklayın ve cihaz için Kurulum hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Aygıt iSCSI sunucusu olarak yapılandırın](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Tıklatın **yapılandırma** cihaz komut çubuğunda. Bu açılır **yapılandırma** dikey. İçinde **yapılandırma** dikey penceresinde aşağıdakileri yapın:
   
   * İSCSI sunucu adını otomatik olarak doldurulur.
   * Emin olun bulut depolama şifrelemesini ayarlanmış **etkin**. Bu, CİHAZDAN buluta gönderilen verilerin şifrelenmesini sağlar.
   * Bir 32 karakterlik şifreleme anahtarı belirtin ve daha sonra başvurmak için bir anahtar yönetimi uygulamasında kaydedin.
   * Cihazınızı ile kullanılmak üzere bir depolama hesabı seçin. Bu abonelik, mevcut bir depolama hesabını seçebilir veya tıklayabilirsiniz **Ekle** farklı bir abonelikten bir hesap seçin.
     
     ![Aygıt iSCSI sunucusu olarak yapılandırın](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Tıklatın **yapılandırma** iSCSI sunucusunun kurulumunu tamamlamak için.
   
    ![Aygıt iSCSI sunucusu olarak yapılandırın](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. İSCSI sunucu oluşturma devam ediyor bildirilir. İSCSI sunucusu başarıyla oluşturulduktan sonra **aygıtları** dikey güncelleştirilir ve karşılık gelen cihaz durumu **çevrimiçi**.
   
    ![Aygıt iSCSI sunucusu olarak yapılandırın](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>3. adım: Birim Ekle

1. İçinde **aygıtları** eklediğiniz aygıt bir iSCSI sunucusu yapılandırılmış dikey penceresinde, seçin. Tıklatın **...**  (Alternatif olarak bu satırı sağ) ve bağlam menüsünden seçin **birim ekleme**. Tıklatarak **+ Add birim** komut çubuğundan. Bu açılır **birim ekleme** dikey.
   
    ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. İçinde **birim ekleme** dikey penceresinde aşağıdakileri yapın:
   
   * İçinde **birim adı** alanında, biriminiz için benzersiz bir ad girin. Adı 3 ile 127 karakter içeren bir dize olmalıdır.
   * İçinde **türü** açılır listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** birim. Yerel GARANTİLERİN, düşük gecikme ve yüksek performansın gerektiği iş yükleri için seçin **yerel olarak sabitlenmiş** **birim**. Diğer tüm veriler için seçin **katmanlı** **birim**.
   * İçinde **kapasite** alanında, birimin boyutunu belirtin. Katmanlı birim 500 GB ile 5 TB arasında olmalıdır ve yerel olarak sabitlenmiş bir birim 50 GB ve 500 GB arasında olması gerekir.
     
     Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil verilerin cihazda kalır ve buluta dağıtılmamasını sağlar.
     
     Öte yandan, katmanlı birim sıkı hazırlanmıştır. Katmanlı birim oluşturduğunuzda, alanın % 10'yaklaşık yerel katmanında sağlanır ve alan % 90'ını bulutta sağlanır. Örneğin, 1 TB birim sağlanan, 100 GB yerel alanında bulunması ve 900 GB bulutta kullanılabilir zaman veri katmanları. Bu sırayla cihazda yerel alana çalıştırırsanız olan anlamına gelir (% 10 kullanılamaz çünkü) bir katmanlı paylaşımı sağlayamazsınız.
     
     ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Tıklatın **bağlı Konaklar**, bu birime bağlanmak ve ardından istediğiniz iSCSI başlatıcısı karşılık gelen bir erişim denetimi kaydı (ACR) seçin **seçin**. <br><br> 
3. Yeni bir bağlı ana bilgisayar eklemek için tıklatın **yeni Ekle**, bir ana bilgisayar ve onun iSCSI adı tam adını (IQN) ve ardından **Ekle**. IQN'niz yoksa Git [ek A: bir Windows Server konağının IQN'ini alın](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Biriminiz yapılandırma bittiğinde, tıklatın **Tamam**. Belirtilen ayarlarla bir birim oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, izleme ve yedekleme birimi için etkinleştirilecek.
   
     ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. Birim başarıyla oluşturulduğunu doğrulamak için Git **birimleri** dikey. Listelenen birimleri görmeniz gerekir.
   
   ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>4. adım: Bağlamak, başlatmak ve bir birimi biçimlendirme

Bağlamak, başlatmak ve Windows Server konağında StorSimple birimlerinizi biçimlendirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Birimi bağlamak, başlatmak ve biçimlendirmek için

1. Açık **iSCSI başlatıcısı** uygun sunucusundaki uygulama.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Keşif** sekmesinde, **Portal Bul**’a tıklayın.
   
    ![Portal Bul](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. **Hedef Portal Bul** iletişim kutusuna iSCSI etkin ağ arabiriminin IP adresini girin ve ardından **Tamam**’a tıklayın.
   
    ![IP adresi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. **iSCSI Başlatıcısı Özellikleri** penceresinin **Hedefler** sekmesinde **Bulunan hedefler**’i bulun. (Her birimi bulunan bir hedef olacaktır.) Cihaz durumu **Etkin Olmayan** olarak görünmelidir.
   
    ![bulunan hedefler](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Hedef cihazı seçin ve ardından **Bağlan**. Cihaz bağlandıktan sonra durum **Bağlandı** olarak değişmelidir. (Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için bkz: [yükleme ve yapılandırma Microsoft iSCSI başlatıcısı][1].
   
    ![Hedef cihazı seçin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Windows konağında Windows Logo + X tuşlarına basıp **Çalıştır**’a tıklayın.
7. **Çalıştır** iletişim kutusuna **Diskmgmt.msc** yazın. **Tamam**’a tıklayın, **Disk Yönetimi** iletişim kutusu görüntülenir. Sağ bölmede, konaktaki birimler gösterilir.
8. **Disk Yönetimi** penceresinde bağlanan birimler aşağıdaki çizimde gösterildiği gibi görünür. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.
   
    ![Disk Yönetimi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Sağ tıklatıp **diski başlatma**.
   
    ![Disk 1 başlatma](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. İletişim kutusunda başlatın ve ardından disk veya diskler **Tamam**.
    
    ![Disk 2 başlatılamıyor](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. Yeni Basit Birim Sihirbazı'nı başlatır. Bir disk boyutu seçin ve ardından **sonraki**.
    
    ![Yeni Birim Sihirbazı 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Birime bir sürücü harfi atamak ve ardından **sonraki**.
    
    ![Yeni Birim Sihirbazı'nı 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Birimi biçimlendirmek için parametreleri girin. **Windows Server'da yalnızca NTFS desteklenir.** Ayırma birimi boyutu 64 K olarak ayarlayın. Biriminiz için bir etiket sağlar. Bu ad, StorSimple sanal dizisinde sağlanan birim adıyla aynı olması önerilen en iyi yöntem değil. **İleri**’ye tıklayın.
    
    ![Yeni Birim Sihirbazı'nı 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Biriminiz için değerleri kontrol edin ve ardından **son**.
    
    ![Yeni Birim Sihirbazı'nı 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    Birimleri olarak görünür **çevrimiçi** üzerinde **Disk Yönetimi** sayfası.
    
    ![birimlerin çevrimiçi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Sonraki adımlar

Yerel kullanmayı öğrenin web kullanıcı Arabirimi için [StorSimple sanal dizinizi yönetmek](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Ek A: bir Windows Server konağının IQN'ini alın

Windows Server 2012 çalıştıran bir Windows konağının iSCSI Tam Adını (IQN) almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Windows konağının IQN’sini almak için

1. Microsoft iSCSI başlatıcısını Windows konağında başlatın.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
   
    ![iSCSI başlatıcısı özellikleri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Bu dizeyi kaydedin.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



