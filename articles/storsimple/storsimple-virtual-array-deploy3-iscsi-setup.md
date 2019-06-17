---
title: Microsoft Azure StorSimple Virtual Array iSCSI sunucusu Kurulumu | Microsoft Docs
description: İlk kurulum gerçekleştirmek, StorSimple iSCSI sunucunuzu kaydedin ve cihaz kurulumunu tamamlamak açıklar.
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
ms.openlocfilehash: 5d3525952ec09474d60618c4f99138cef1fce57a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61417316"
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>StorSimple sanal dizisi – kümesini oluşturan Azure Portalı aracılığıyla iSCSI sunucusu olarak dağıtma

![iSCSI Kurulum işlem akışı](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Genel Bakış

Bu dağıtım öğretici, Microsoft Azure StorSimple Virtual Array için geçerlidir. Bu öğretici, ilk kurulumu gerçekleştirmek, StorSimple iSCSI sunucunuzu kaydedin, cihaz kurulumunu tamamlayın ve ardından oluşturmak, bağlamak, başlatmak ve StorSimple sanal bir iSCSI sunucusu olarak yapılandırılmış dizininiz birimleri biçimlendirin açıklar. 

Açıklanan yordamları, yaklaşık 30 dakika tamamlanması 1 saate buraya yazın. Bu makalede yayımlanmış bilgiler yalnızca StorSimple sanal diziler için geçerlidir.

## <a name="setup-prerequisites"></a>Kurulum Önkoşulları

Yapılandırmak ve StorSimple Virtual Array'iniz ayarlamak için önce emin olun:

* Sanal dizi hazırladıktan ve açıklandığı gibi bağlı [dağıtma StorSimple Virtual Array - sağlama Hyper-V'de bir sanal dizin](storsimple-ova-deploy2-provision-hyperv.md) veya [dağıtma StorSimple Virtual Array - bir sanal dizin vmware'de sağlama ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hizmet kayıt anahtarı, StorSimple sanal dizilerini yönetmek için oluşturulan StorSimple cihaz Yöneticisi hizmeti var. Daha fazla bilgi için **2. adım: Hizmet kayıt anahtarı alma** içinde [StorSimple sanal Dizini'ni dağıtma - portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Mevcut bir StorSimple cihaz Yöneticisi hizmetiyle kaydettirmekte olduğunuz ikinci veya sonraki sanal dizi varsa, hizmet veri şifreleme anahtarı olmalıdır. Bu hizmet ile ilk cihaz başarıyla kaydedildiğinde bu anahtarı yeniden oluşturuldu. Bu anahtar kaybettiyseniz, bkz. **hizmet veri şifreleme anahtarı alma** içinde [StorSimple Virtual Array'iniz yönetmek için Web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Adım adım Kurulum

Ayarlanmış ve StorSimple Virtual Array'iniz yapılandırmak için aşağıdaki adım adım yönergeleri kullanın:

* [1. adım: Yerel web kullanıcı Arabirimi Kurulumu tamamlayın ve Cihazınızı kaydetme](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* 2\. adım: Gerekli cihaz kurulumunu tamamlayın
* [3. adım: Birim Ekle](#step-3-add-a-volume)
* [4. adım: Bağlayın, başlatın ve bir birimde](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>1\. adım: Yerel web kullanıcı Arabirimi Kurulumu tamamlayın ve Cihazınızı kaydetme

#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlayın ve cihaz kaydetmek için

1. Tarayıcı penceresini açın. Web kullanıcı Arabirimi tür bağlanmak için:
   
    `https://<ip-address of network interface>`
   
    Önceki adımda not ettiğiniz bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında sorun olduğunu bildiren bir hata görürsünüz. Tıklayın **bu web sayfasına devam**.
   
    ![Güvenlik sertifika hatası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Web kullanıcı Arabirimine sanal cihazınızın oturum olarak **StorSimpleAdmin**. Adım 3'te değiştirdiğiniz cihaz Yöneticisi parolasını girin: Sanal cihazı başlatma [dağıtma StorSimple Virtual Array - Hyper-V'deki sanal cihazı sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [dağıtma StorSimple Virtual Array - bir VMware sanal cihazı sağlama](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Oturum açma sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. İçin gerçekleştirilecek **giriş** sayfası. Bu sayfa, yapılandırmak ve StorSimple cihaz Yöneticisi hizmeti ile sanal cihaz kaydetmek için gereken çeşitli ayarları açıklar. Unutmayın **ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **cihaz ayarları** ve **Cloud ayarları**.
   
    ![Giriş sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. Üzerinde **ağ ayarları** altındaki **ağ arabirimleri**, DATA 0 otomatik olarak yapılandırılacaktır sizin için. Her ağ arabirimi bir IP adresini otomatik olarak almak için varsayılan olarak ayarlanır (DHCP). Bu nedenle, bir IP adresi, alt ağ ve ağ geçidi otomatik olarak (IPv4 ve IPv6 için) atanır.
   
    Cihazınızı bir iSCSI sunucusu olarak (blok depolama alanı Hazırla) dağıtmayı planlarken, devre dışı bırakmanızı öneririz **IP adresini otomatik olarak almak** seçenek ve statik IP adreslerini yapılandırın.
   
    ![Ağ Ayarları sayfası](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Cihazın sağlama işlemi sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Ağ Arabiriminizin IPv4 yalnızca veya IPv4 ve IPv6 yapılandırabileceğiniz unutmayın. Yalnızca IPv6 yapılandırmaları desteklenmez.
5. DNS sunucuları gerekli olduğu Cihazınızı, bulut depolama hizmeti sağlayıcıları ile iletişim kurmak için veya bir dosya sunucusu olarak yapılandırılmışsa, Cihazınızı adına göre çözümlemeye çalıştığında kullanılır. Üzerinde **ağ ayarları** altındaki **DNS sunucuları**:
   
   1. Bir birincil ve ikincil DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmak isterseniz, DNS sunucuları belirtebilirsiniz. Yüksek kullanılabilirlik için birincil ve ikincil DNS sunucusu yapılandırmanızı öneririz.
   2. **Uygula**'ya tıklayın. Bu uygulama ve ağ ayarlarını doğrulayın.
6. Üzerinde **cihaz ayarları** sayfası:
   
   1. Benzersiz bir atama **adı** cihazınıza. Bu ad, 1-15 karakter uzunluğunda olabilir ve harf, rakam ve kısa çizgi içerebilir.
   2. Tıklayın **iSCSI sunucusu** simgesi ![iSCSI sunucusu simgesi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) için **türü** oluşturmakta olduğunuz cihazın. İSCSI sunucusu sağlama blok depolama sağlar.
   3. Bu cihaz etki alanına katılmış olmasını isteyip istemediğinizi belirtin. Cihazınızı bir iSCSI sunucusu ise, sonra etki alanına katılmak isteğe bağlıdır. İSCSI sunucunuz bir etki alanına değil karar verirseniz, tıklayın **Uygula**, ayarlarının uygulanması ve ardından sonraki adıma atlamak bekleyin.
      
       Cihaz bir etki alanına eklemek istiyorsanız. Girin bir **etki alanı adı**ve ardından **Uygula**.
      
      > [!NOTE]
      > İSCSI sunucunuz bir etki alanına katmak, sanal dizininiz için Microsoft Azure Active Directory kendi kuruluş birimi (OU) olduğu ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış emin olun.
      > 
      > 
   4. Bir iletişim kutusu görünür. Belirtilen biçimde etki alanı kimlik bilgilerinizi girin. Onay simgesine tıklayın ![onay simgesi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). Etki alanı kimlik bilgilerinin doğrulanır. Kimlik bilgileri yanlışsa, bir hata iletisi görürsünüz.
      
       ![Kimlik bilgileri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. **Uygula**'ya tıklayın. Bu uygulama ve cihaz ayarlarını doğrulayın.
7. (İsteğe bağlı olarak), web Ara sunucusunu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca, burada yapılandırabilirsiniz olduğunu unutmayın.
   
    ![Web Proxy'yi Yapılandır](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    Üzerinde **Web proxy** sayfası:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http:\//host-IP adresi* veya *FQDN: port numarası*. HTTPS URL'leri desteklenmediğine dikkat edin.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulaması kullanıyorsanız, aynı zamanda sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Doğrulama ve yapılandırılmış web proxy ayarlarını uygulayın.
8. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
   
    ![Saat ayarları](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    Üzerinde **saat ayarlarını** sayfası:
   
   1. Aşağı açılan listesinden **saat dilimi** içinde cihaz dağıtıldığı coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
   2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
   3. İsteğe bağlı olarak belirtin bir **ikincil NTP sunucusu** cihazınız için.
   4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılan süre ayarlar uygulanır.
9. Cihazınız için bulut ayarlarını yapılandırın. Bu adımda, yerel cihaz yapılandırmasını tamamlayın ve ardından cihazı, StorSimple cihaz Yöneticisi hizmetiyle kaydedin.
   
   1. Girin **hizmet kayıt anahtarını** aldığınız **2. adım: Hizmet kayıt anahtarı alma** içinde [StorSimple sanal Dizini'ni dağıtma - Portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Bu hizmetle kaydettiriyor olduğunuz ilk cihaz bu değilse, sağlamanız gerekir **hizmet veri şifreleme anahtarı**. StorSimple cihaz Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla birlikte bu anahtar gereklidir. Daha fazla bilgi için [hizmet veri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) yerel web kullanıcı Arabirimi.
   3. Tıklayın **kaydetme**. Bu cihaz yeniden başlatılır. 2-3 cihaz başarıyla kaydedildikten önce dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına ulaşabilirsiniz.
      
      ![Cihazı kaydedin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Azure portalına dönün.
11. Gidin **cihazları** hizmetinizin dikey penceresi. Çok fazla kaynak varsa tıklayın **tüm kaynakları**hizmet adınızı (Ara gerekiyorsa) tıklayın ve ardından **cihazları**.
12. Üzerinde **cihazları** dikey penceresinde, cihaz başarıyla hizmete durumunu arayarak bağlandığını doğrulayın. Cihazın durumu **Kuruluma hazır** olmalıdır.
    
    ![Cihazı kaydedin](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-the-device-as-iscsi-server"></a>2\. adım: Cihazın iSCSI sunucusu olarak yapılandırma

Gerekli cihaz kurulumunu tamamlamak için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-configure-the-device-as-iscsi-server"></a>Cihazın iSCSI sunucusu olarak yapılandırmak için

1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından Git **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde oluşturulan yeni bir cihaz seçin. Bu cihazı olarak gösterilmesini **kuruluma hazır**.
   
    ![Cihaz iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Bir cihaza tıklayın ve cihaz için Kurulum hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Cihaz iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Tıklayın **yapılandırma** cihaz komut çubuğunda. Bu açılır **yapılandırma** dikey penceresi. İçinde **yapılandırma** dikey penceresinde aşağıdakileri yapın:
   
   * İSCSI sunucusu adı otomatik olarak doldurulur.
   * Bulut depolama şifrelemesi emin olun kümesine **etkin**. Bu, CİHAZDAN buluta gönderilen verilerin şifrelenmesini sağlar.
   * 32 karakterlik şifreleme anahtarı belirtin ve sonra başvurabilmeniz için bir anahtar yönetimi uygulamasına kaydedin.
   * Cihazınız ile kullanılacak bir depolama hesabı seçin. Bu abonelikte mevcut bir depolama hesabını seçin veya tıklayabilirsiniz **Ekle** için farklı bir abonelikten bir hesap seçin.
     
     ![Cihaz iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Tıklayın **yapılandırma** iSCSI sunucusu kurulumunu tamamlamak için.
   
    ![Cihaz iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. İSCSI sunucusu oluşturma sürüyor bildirilir. İSCSI sunucusu başarıyla oluşturulduktan sonra **cihazları** dikey penceresi güncelleştirilerek ve karşılık gelen cihaz durumu **çevrimiçi**.
   
    ![Cihaz iSCSI sunucusu olarak yapılandırma](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>3\. adım: Birim Ekle

1. İçinde **cihazları** yeni cihaz bir iSCSI sunucusu olarak yapılandırılmış dikey penceresinde seçin. Tıklayın **...**  (Alternatif olarak bu satıra sağ tıklayın) ve bağlam menüsünden **birim ekleme**. Ayrıca **+ birim Ekle** komut çubuğundan. Bu açılır **birim ekleme** dikey penceresi.
   
    ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. İçinde **birim ekleme** dikey penceresinde aşağıdakileri yapın:
   
   * İçinde **birim adı** biriminiz için benzersiz bir ad girin. Ad 3 ile 127 karakter içeren bir dize olmalıdır.
   * İçinde **türü** açılan listesinde, oluşturulup oluşturulmayacağını belirtin bir **katmanlı** veya **yerel olarak sabitlenmiş** birim. Yerel GARANTİLERİN, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için seçin **yerel olarak sabitlenmiş** **birim**. Diğer tüm veriler için seçin **katmanlı** **birim**.
   * İçinde **kapasite** alanında, birimin boyutunu belirtin. Katmanlı birim 5 TB ve 500 GB arasında olmalıdır ve yerel olarak sabitlenmiş bir birim, 50 GB ile 500 GB arasında olmalıdır.
     
     Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil veri cihazda kalır ve buluta dağıtılmamasını sağlar.
     
     Öte yandan, bir katmanlı birim sıkı hazırlanmıştır. Katmanlı birim oluşturduğunuzda, yerel katmanında yaklaşık %10 alanı sağlandığında ve bulutta % 90 ' alanı sağlandığında. Örneğin, 1 TB'lik bir birimi sağladıysanız, 100 GB yerel alanı bulunması ve 900 GB bulutta kullanılabilir olduğunda veri katmanları. Bu cihaz üzerinde yerel alanın tümünü dışında çalıştırırsanız olan karşılık gelir (% 10 kullanılamaz çünkü) katmanlı bir paylaşım sağlayamazsınız.
     
     ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Tıklayın **bağlı Konaklar**, bu birime bağlanmak ve ardından istediğiniz iSCSI başlatıcısı için karşılık gelen bir erişim denetimi kaydı (ACR) seçin **seçin**. <br><br> 
3. Yeni bağlı bir konak eklemek için tıklatın **yeni Ekle**, konak ve kendi iSCSI için bir ad girin tam adı (IQN) ve ardından **Ekle**. IQN'niz yoksa, Git [ek A: Bir Windows Server konağının IQN'ini alın](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. Toplu yapılandırmayı bitirdiğinizde, tıklayın **Tamam**. Belirtilen ayarlarla bir birim oluşturulur ve bir bildirim görürsünüz. Varsayılan olarak, birim için izleme ve yedekleme etkinleştirilecektir.
   
     ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. Birimi başarıyla oluşturulduğunu doğrulamak için şu adrese gidin **birimleri** dikey penceresi. Listelenen birimleri görmeniz gerekir.
   
   ![Birim Ekle](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>4\. Adım: Bir birimi bağlayın, başlatın ve biçimlendirin

Bağlayın, başlatın ve bir Windows Server konağında StorSimple birimlerinizi biçimlendirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Birimi bağlamak, başlatmak ve biçimlendirmek için

1. Açık **iSCSI başlatıcısı** uygun sunucuda uygulama.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Keşif** sekmesinde, **Portal Bul**’a tıklayın.
   
    ![Portal'ı keşfedin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. **Hedef Portal Bul** iletişim kutusuna iSCSI etkin ağ arabiriminin IP adresini girin ve ardından **Tamam**’a tıklayın.
   
    ![IP adresi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. **iSCSI Başlatıcısı Özellikleri** penceresinin **Hedefler** sekmesinde **Bulunan hedefler**’i bulun. (Her birim bulunan bir hedef olacaktır.) Cihaz durumu **Etkin Olmayan** olarak görünmelidir.
   
    ![bulunan hedefler](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Bir hedef cihaz seçin ve ardından **Connect**. Cihaz bağlandıktan sonra durum **Bağlandı** olarak değişmelidir. (Microsoft iSCSI başlatıcısını kullanma hakkında daha fazla bilgi için bkz. [iSCSI başlatıcısını yükleme ve yapılandırma Microsoft][1].
   
    ![Hedef cihaz seçin](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Windows konağında Windows Logo + X tuşlarına basıp **Çalıştır**’a tıklayın.
7. **Çalıştır** iletişim kutusuna **Diskmgmt.msc** yazın. **Tamam**’a tıklayın, **Disk Yönetimi** iletişim kutusu görüntülenir. Sağ bölmede, konaktaki birimler gösterilir.
8. **Disk Yönetimi** penceresinde bağlanan birimler aşağıdaki çizimde gösterildiği gibi görünür. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.
   
    ![Disk Yönetimi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Sağ tıklayıp **diski başlatma**.
   
    ![diski 1 başlatın](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. İletişim kutusunda, diskleri başlatır ve ardından seçin **Tamam**.
    
    ![diski 2 başlatın](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. Yeni Basit Birim Sihirbazı'nı başlatır. Disk boyutu seçin ve ardından **sonraki**.
    
    ![Yeni Birim Sihirbazı 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Birime bir sürücü harfi atamak ve ardından **sonraki**.
    
    ![Yeni Birim Sihirbazı'nı 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Birimi biçimlendirmek için parametreler girin. **Windows Server üzerinde yalnızca NTFS desteklenir.** 64 K ayırma birimi boyutunu ayarlayın. Biriminiz için bir etiket belirtin. StorSimple sanal dizisi üzerinde sağlanan birim adıyla aynı olması için bu adı için önerilen bir en iyi yöntem var. **İleri**’ye tıklayın.
    
    ![Yeni Birim Sihirbazı'nı 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Biriminiz için değerleri kontrol edin ve ardından **son**.
    
    ![Yeni Birim Sihirbazı'nı 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    Birimleri olarak görünür **çevrimiçi** üzerinde **Disk Yönetimi** sayfası.
    
    ![birimler çevrimiçi](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Sonraki adımlar

Yerel kullanmayı öğrenin web kullanıcı Arabirimi için [StorSimple Virtual Array'iniz yönetmek](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Ek A: Bir Windows Server konağının IQN’ini alın

Windows Server 2012 çalıştıran bir Windows konağının iSCSI Tam Adını (IQN) almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Windows konağının IQN’sini almak için

1. Microsoft iSCSI başlatıcısını Windows konağında başlatın.
2. **iSCSI Başlatıcısı Özellikleri** penceresinin **Yapılandırma** sekmesinde, **Başlatıcı Adı** alanından dizeyi seçip kopyalayın.
   
    ![iSCSI başlatıcısı özellikleri](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Bu dizeyi kaydedin.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



