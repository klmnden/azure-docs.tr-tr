---
title: StorSimple sanal dizisi dosya sunucusu olarak ayarlama | Microsoft Docs
description: "StorSimple sanal dizinin dağıtım üçüncü Bu öğreticide, bir sanal cihaz dosya sunucusu olarak ayarlamak için bildirir."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bf507fb21b314a6811db1c1e45a4356381caada1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>StorSimple sanal dizinin - kümesi yukarı Azure portal aracılığıyla dosya sunucusu olarak dağıtma
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Giriş
Bu makalede, ilk kurulumu gerçekleştirmek, StorSimple dosya sunucunuzu kaydetme, cihaz kurulumunu tamamlayın ve oluşturmak ve SMB paylaşımlara açıklar. Serideki son makaleyi tamamen sanal dizinizi bir dosya sunucusu veya bir iSCSI sunucu olarak dağıtmak için gereken dağıtım öğreticileri, budur.

Kurulum ve yapılandırma işleminin tamamlanması için yaklaşık 10 dakika sürebilir. Bu makaledeki bilgileri yalnızca StorSimple sanal dizinin dağıtımı için geçerlidir. StorSimple 8000 serisi cihazlar dağıtımı için gidin: [güncelleştirme 2 çalıştıran StorSimple 8000 serisi Cihazınızı dağıtmak](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Kurulum Önkoşulları
Yapılandırmak ve StorSimple sanal dizinizi ayarlama önce emin olun:

* Sanal bir dizi sağlanan ve içinde ayrıntılı olarak bağlı [Hyper-V StorSimple sanal dizisinde sağlamak](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [VMware StorSimple sanal dizisinde sağlamak](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hizmet kayıt anahtarı, StorSimple sanal diziler yönetmek için oluşturduğunuz StorSimple Aygıt Yöneticisi'ni hizmetinden var. Daha fazla bilgi için bkz: [2. adım: Hizmet kayıt anahtarını alın](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple sanal dizini.
* Mevcut bir StorSimple cihaz Yöneticisi hizmeti ile kaydetme ikinci veya sonraki sanal dizinin gerekiyorsa hizmet verileri şifreleme anahtarı olması gerekir. İlk aygıt hizmeti ile başarıyla kaydettirildi olduğunda bu anahtar oluşturuldu. Bu anahtar kaybettiyseniz, bkz: [hizmet verileri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) , StorSimple sanal dizini.

## <a name="step-by-step-setup"></a>Adım adım Kurulumu
Kurmak ve StorSimple sanal dizinizi yapılandırmak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>1. adım: yerel web kullanıcı Arabirimi Kurulumu tamamlamak ve Cihazınızı kaydedin
#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlamak ve cihaz kaydetmek için
1. Bir tarayıcı penceresi açın ve yerel web kullanıcı Arabirimi bağlanın. Şunu yazın:
   
   `https://<ip-address of network interface>`
   
   Önceki adımda not ettiğiniz bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında bir sorun olduğunu belirten bir hata görürsünüz. Tıklatın **bu Web sayfası için devam**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Web kullanıcı Arabirimine sanal dizinin oturum olarak **StorSimpleAdmin**. Adım 3'te değiştirdiğiniz cihaz Yöneticisi parolasını girin: sanal dizinin başlangıç [Hyper-V StorSimple sanal dizisinde sağlamak](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [VMware StorSimple sanal dizisinde sağlamak](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. Gittiğiniz **giriş** sayfası. Bu sayfayı yapılandırmak ve sanal dizinin StorSimple cihaz Yöneticisi hizmeti ile kaydetmek için gereken çeşitli ayarlar açıklanır. **Ağ ayarlarını**, **Web proxy ayarlarını**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **aygıt ayarları** ve **bulut ayarları**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. İçinde **ağ ayarlarını** altında sayfa **ağ arabirimleri**, DATA 0 otomatik olarak yapılandırılacaktır sizin için. Her ağ arabiriminin IP adresini otomatik olarak almak için varsayılan olarak ayarlanır (DHCP). Bu nedenle, bir IP adresi, alt ağ ve ağ geçidi otomatik olarak (IPv4 ve IPv6 için) atanır.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Aygıt sağlama işlemi sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Ağ arabirimi yalnızca veya IPv4 ve IPv6 IPv4 olarak yapılandırabilirsiniz unutmayın. IPv6 yalnızca yapılandırmaları desteklenmez.
5. DNS sunucuları, Cihazınızı çözmek için veya Bulut depolama hizmeti sağlayıcılarınızın ile iletişim kurmak için Cihazınızı denediğinde, dosya sunucusu olarak yapılandırılmış ad tarafından kullanıldığı için gereklidir. İçinde **ağ ayarlarını** altında sayfa **DNS sunucuları**:
   
   1. Bir birincil ve ikincil DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmak isterseniz, DNS sunucuları belirtebilirsiniz. Yüksek kullanılabilirlik için birincil ve ikincil bir DNS sunucusuna yapılandırmanızı öneririz.
   2. Tıklatın **Uygula** uygulamak ve ağ ayarlarını doğrulayın.
6. İçinde **aygıt ayarları** sayfa:
   
   1. Benzersiz bir Ata **adı** aygıtınıza. Bu ad, 1-15 karakter olabilir ve harf, rakam ve tire içerebilir.
   2. Tıklatın **dosya sunucusu** simgesi ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) için **türü** oluşturmakta olduğunuz cihazın. Bir dosya sunucusunda paylaşılan klasörleri oluşturmanıza izin verir.
   3. Cihazınızı bir dosya sunucusu olduğundan, cihaz bir etki alanına katılmak gerekir. Girin bir **etki alanı adı**.
   4. **Uygula**'ya tıklayın.
7. Bir iletişim kutusu görünür. Belirtilen biçimde etki alanı kimlik bilgilerinizi girin. Onay simgesine tıklayın. Etki alanı kimlik bilgileri doğrulanır. Kimlik bilgileri yanlışsa, bir hata iletisi görürsünüz.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. **Uygula**'ya tıklayın. Bu uygulama ve aygıt ayarlarını doğrulayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Sanal dizinizi kendi kuruluş biriminde (OU) için Active Directory ve hiçbir Grup İlkesi nesneleri (GPO) uygulanan veya devralınan emin olun. Grup İlkesi, StorSimple sanal dizi virüsten koruma yazılımı gibi uygulamaları yükleyebilir. Ek yazılım yükleme desteklenmez ve veri bozulmasına yol açabilir. 
   > 
   > 
9. (İsteğe bağlı) web proxy sunucunuzu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca burada yapılandırabilirsiniz olduğunu unutmayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   İçinde **Web proxy** sayfa:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http://&lt;ana bilgisayar IP adresi veya FDQN&gt;: bağlantı noktası numarası*. HTTPS URL'leri desteklenmez unutmayın.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulamasını kullanıyorsanız, siz de sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılmış web proxy ayarlarını uygulayın.
10. (İsteğe bağlı) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. NTP sunucuları gerekli olduğu bulut hizmeti sağlayıcılarınızın ile doğrulanabilmesi Cihazınızı zaman eşitlemeniz gerekir.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    İçinde **saat ayarlarını** sayfa:
    
    1. Açılır listeden seçin **saat dilimi** hangi cihaz dağıtıldığından coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak belirtmek bir **ikincil NTP sunucusu** cihazınız için.
    4. **Uygula**'ya tıklayın. Bu doğrulamak ve yapılandırılmış zaman ayarlarını uygulayın.
11. Cihazınız için bulut ayarlarını yapılandırın. Bu adımda, yerel aygıt yapılandırmasını tamamlayın ve ardından StorSimple cihaz Yöneticisi hizmetiniz ile cihazı kaydedin.
    
    1. Girin **hizmet kayıt anahtarını** aldığınız [2. adım: Hizmet kayıt anahtarını alın](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple sanal dizini.
    2. Bu hizmeti ile kaydetme ilk cihazınız varsa ile sunulur **hizmet verileri şifreleme anahtarı**. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. Bu anahtar StorSimple cihaz Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarı gereklidir. 
       
       Bu hizmeti ile kaydetme ilk aygıtı değilse, hizmet verileri şifreleme anahtarı sağlamanız gerekir. Daha fazla bilgi için alma başvurmak [hizmet verileri şifreleme anahtarı](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) yerel web kullanıcı Arabirimi.
    3. Tıklatın **kaydetmek**. Bu, aygıt yeniden başlatılır. 2-3 cihaz sorunsuz kaydedildikten önce dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına yönlendirilirsiniz.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Azure portalına geri dönün. Git **tüm kaynakları**, StorSimple Aygıt Yöneticisi'ni hizmetinizi arayın.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. Filtrelenmiş listesinde, StorSimple Aygıt Yöneticisi'ni hizmetinizi seçin ve ardından gidin **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde, cihaz hizmete başarıyla bağlandı ve durum sahip olduğunu doğrulayın **ayarlamak hazır**.
    
    ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-the-device-as-file-server"></a>2. adım: cihaz dosya sunucusu olarak yapılandırın.
Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://portal.azure.com/) gerekli cihaz kurulumunu tamamlamak için.

#### <a name="to-configure-the-device-as-file-server"></a>Cihaz dosya sunucusu olarak yapılandırmak için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından Git **Yönetim > aygıtları**. İçinde **aygıtları** dikey penceresinde, seçin yeni cihaz oluşturulmuş. Bu cihazı olarak görüntülenebilir **ayarlamak hazır**.
   
   ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Cihaz tıklayın ve cihaz için Kurulum hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Tıklatın **yapılandırma** komut çubuğunda. Bu açılır **yapılandırma** dikey. İçinde **yapılandırma** dikey penceresinde aşağıdakileri yapın:
   
    1. Dosya sunucusu adı otomatik olarak doldurulur.
    
    2. Emin olun bulut depolama şifrelemesini ayarlanmış **etkin**. Bu buluta gönderilen tüm verileri şifreler. 
    
    3. 256 bit AES anahtar şifrelemesi için kullanıcı tanımlı anahtarla kullanılır. Bir 32 karakter anahtarı belirtin ve sonra onaylamak için anahtarı girin. Gelecekte başvurmak için bir anahtar yönetimi uygulaması olarak kayıt anahtarı.
    
    4. Tıklatın **gerekli ayarları Yapılandır** cihazınızla kullanılmak üzere depolama hesabının kimlik bilgilerini belirtmek için. Tıklatın **yeni Ekle** yapılandırılmış depolama hesap kimlik bilgisi olup olmadığını. **Destekler blok blobları kullandığınız depolama hesabı emin olun. Sayfa bloblarını desteklenmez.** Hakkında daha fazla bilgi [engeller blobları ve sayfa bloblarını](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
    ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. İçinde **bir depolama hesabının kimlik bilgilerini eklemek** dikey penceresinde aşağıdakileri yapın: 

    1. Hizmet ile aynı abonelikte depolama hesabı ise, geçerli bir abonelik seçin. Diğer depolama belirtin hizmet aboneliği dışında bir hesaptır. 
    
    2. Açılır listeden mevcut bir depolama hesabını seçin. 
    
    3. Konum belirtilen depolama hesabına göre otomatik olarak doldurulur. 
    
    4. Bir cihaz ve bulut arasındaki güvenli ağ iletişim kanalını emin olmak SSL'yi etkinleştirin.
    
    5. Tıklatın **Ekle** bu depolama hesabı kimlik bilgilerini eklemek için. 
   
        ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Depolama hesabı kimlik bilgilerini başarıyla oluşturulduktan sonra **yapılandırma** belirtilen depolama hesabı bilgilerini görüntülemek için dikey pencereyi güncelleştirilmeyecek. **Yapılandır**'a tıklayın.
   
   ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Bir dosya görürsünüz sunucusu oluşturuluyor. Dosya sunucusu başarıyla oluşturulduktan sonra size bildirilecek.
   
   ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Cihaz durumu da değiştirir **çevrimiçi**.
   
   ![Bir dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   Bir paylaşım eklemeye devam edebilirsiniz.

## <a name="step-3-add-a-share"></a>3. adım: bir paylaşımı ekleme
Aşağıdaki adımlarda gerçekleştirmek [Azure portal](https://portal.azure.com/) bir paylaşımı oluşturmak için.

#### <a name="to-create-a-share"></a>Bir paylaşımı oluşturmak için
1. Önceki adımda yapılandırdığınız dosya sunucusu aygıtı seçin ve tıklatın **...**  (veya sağ tıklayın). Bağlam menüsünü **Ekle paylaşımı**. Alternatif olarak, tıklayabilirsiniz **+ paylaşımı eklemek** cihaz komut çubuğunda.
   
   ![Paylaşım Ekle](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Aşağıdaki paylaşım ayarları belirtin:

    1. Paylaşımınıza için benzersiz bir ad. Adı 3 ile 127 karakter içeren bir dize olmalıdır.
    
    2. İsteğe bağlı bir **açıklama** paylaşımı için. Açıklama, paylaşım sahiplerini tanımlamaya yardımcı olur.
    
    3. A **türü** paylaşımı için. Türü olabilir **katmanlı** veya **yerel olarak sabitlenmiş**, varsayılan değer olan katmanlı. Yerel GARANTİLERİN, düşük gecikme ve yüksek performansın gerektiği iş yükleri için seçin bir **yerel olarak sabitlenmiş** paylaşın. Diğer tüm veriler için seçin bir **katmanlı** paylaşın.
    Yerel olarak sabitlenmiş bir paylaşım sıkı hazırlanmıştır ve paylaşımdaki birincil verilerin cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar. Katmanlı bir paylaşımı diğer yandan sıkı hazırlanmıştır. Katmanlı bir paylaşımı oluşturduğunuzda, alanın % 10 yerel katmanında sağlanır ve alanı % 90'ını bulutta sağlanır. Örneğin, 1 TB birim sağlanan, 100 GB yerel alanında bulunması ve 900 GB bulutta kullanılabilir olduğunda veri katmanları. Bu sırayla cihazda yerel alana çalıştırırsanız, bir katmanlı paylaşımı sağlayamazsınız anlamına gelir.
   
    4. İçinde **kümesine varsayılan tam izinleri** alanında, kullanıcı ya da bu paylaşıma erişim grubu için izinleri atayın. Kullanıcı veya kullanıcı grubunun adını belirtin  *john@contoso.com*  biçimi. Bu paylaşımlar erişmek yönetici ayrıcalıkları izin vermek için bir kullanıcı grubu (yerine tek bir kullanıcı) kullanmanızı öneririz. İzinlerini burada atadıktan sonra bu izinleri değiştirmek için dosya Gezgini'ni kullanabilirsiniz.
   
    5. Tıklatın **Ekle** paylaşımı oluşturmak için. 
    
        ![Paylaşım Ekle](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
        Paylaşım oluşturma sürüyor bildirilir.
   
        ![Paylaşım Ekle](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
    Belirtilen ayarlarla paylaşımı oluşturulduktan sonra **paylaşımları** dikey penceresinde, yeni paylaşım yansıtacak şekilde güncelleştirilecektir. Varsayılan olarak, izleme ve yedekleme paylaşımı için etkinleştirilir.
   
    ![Paylaşım Ekle](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-to-the-share"></a>4. adım: paylaşımına bağlanma
Şimdi önceki adımda oluşturduğunuz bir veya daha fazla paylaşımlarına bağlanmak gerekir. StorSimple sanal dizisine bağlı bir Windows Server ana bilgisayarınızda aşağıdaki adımları gerçekleştirin.

#### <a name="to-connect-to-the-share"></a>Paylaşımına bağlanmak için
1. Tuşuna ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + r Çalıştır penceresine belirtin *&#92; &#92;&lt; dosya sunucusu adı&gt;*  yolu olarak değiştirerek *dosya sunucusu adı* dosya sunucunuzu atanan aygıt adı. **Tamam** düğmesine tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Bu dosya Gezgini'ni açar. Şimdi klasörler olarak oluşturduğunuz paylaşımları görüyor olmalısınız. Seçin ve içeriği görüntülemek için bir paylaşım (klasör) çift tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Şimdi bu paylaşımlara dosyaları ekleyin ve yedekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Yerel kullanmayı öğrenin web kullanıcı Arabirimi için [StorSimple sanal dizinizi yönetmek](storsimple-ova-web-ui-admin.md).

