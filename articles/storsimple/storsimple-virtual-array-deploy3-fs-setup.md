---
title: StorSimple sanal dizisi, dosya sunucusu olarak ayarlama | Microsoft Docs
description: StorSimple Virtual Array dağıtım üçüncü Bu öğreticide bir sanal cihaz dosya sunucusu olarak ayarlamanıza olanak sağlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a931b303e40e41bc23e8b586e1d37e600625b1a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61415239"
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>StorSimple sanal dizisi - kümesi oluşturan Azure portal aracılığıyla dosya sunucusu dağıtma
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Giriş
Bu makalede, ilk kurulumu gerçekleştirmek, StorSimple dosya sunucunuzu kaydedin, cihaz kurulumunu tamamlayın ve oluşturmak ve bağlanmak için SMB paylaşımları açıklar. Serideki son makaleyi dağıtım öğretici tamamen sanal dizininiz bir dosya sunucusu veya bir iSCSI sunucusu olarak dağıtmak için gereken budur.

Kurulum ve yapılandırma işlemini tamamlamak için yaklaşık 10 dakika sürebilir. Bu makaledeki bilgiler, yalnızca StorSimple sanal dizisi dağıtımı için geçerlidir. StorSimple 8000 serisi cihazlar dağıtımı için şu adrese gidin: [Güncelleştirme 2 çalıştıran StorSimple 8000 serisi Cihazınızı dağıtma](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Kurulum Önkoşulları
Yapılandırmak ve StorSimple Virtual Array'iniz ayarlamak için önce emin olun:

* Sanal dizi hazırladıktan ve içinde ayrıntılı olarak bağlı [StorSimple Virtual Array Hyper-V'de sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [StorSimple Virtual Array vmware'de sağlama](storsimple-virtual-array-deploy2-provision-vmware.md).
* Hizmet kayıt anahtarı, StorSimple sanal dizilerini yönetmek için oluşturduğunuz StorSimple cihaz Yöneticisi hizmeti var. Daha fazla bilgi için [2. adım: Hizmet kayıt anahtarı alma](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple Virtual Array için.
* Mevcut bir StorSimple cihaz Yöneticisi hizmetiyle kaydettirmekte olduğunuz ikinci veya sonraki sanal dizi varsa, hizmet veri şifreleme anahtarı olmalıdır. Bu hizmet ile ilk cihaz başarıyla kaydedildiğinde bu anahtarı yeniden oluşturuldu. Bu anahtar kaybettiyseniz, bkz. [hizmet veri şifreleme anahtarı alma](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) StorSimple Virtual Array'iniz için.

## <a name="step-by-step-setup"></a>Adım adım Kurulum
Ayarlanmış ve StorSimple Virtual Array'iniz yapılandırmak için aşağıdaki adım adım yönergeleri kullanın.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>1. Adım: Yerel web kullanıcı Arabirimi Kurulumu tamamlayın ve Cihazınızı kaydetme
#### <a name="to-complete-the-setup-and-register-the-device"></a>Kurulumu tamamlayın ve cihaz kaydetmek için
1. Bir tarayıcı penceresi açın ve yerel web kullanıcı Arabirimine bağlanın. Şunu yazın:
   
   `https://<ip-address of network interface>`
   
   Önceki adımda not ettiğiniz bağlantı URL'sini kullanın. Web sitesinin güvenlik sertifikasında sorun olduğunu belirten bir hata görürsünüz. Tıklayın **bu Web sayfasına devam**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Web kullanıcı Arabiriminde sanal diziniz oturum olarak **StorSimpleAdmin**. Adım 3'te değiştirdiğiniz cihaz Yöneticisi parolasını girin: Sanal diziyi başlatın [StorSimple Virtual Array Hyper-V'de sağlama](storsimple-virtual-array-deploy2-provision-hyperv.md) veya [StorSimple Virtual Array vmware'de sağlama](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. Yönlendirilirsiniz **giriş** sayfası. Bu sayfa, çeşitli ayarları yapılandırmak ve sanal diziyi StorSimple cihaz Yöneticisi hizmetine kaydetmek için gereken açıklar. **Ağ ayarları**, **Web proxy ayarları**, ve **saat ayarlarını** isteğe bağlıdır. Yalnızca gerekli ayarları **cihaz ayarları** ve **Cloud ayarları**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. İçinde **ağ ayarları** altındaki **ağ arabirimleri**, DATA 0 otomatik olarak yapılandırılacaktır sizin için. Her ağ arabirimi IP adresini otomatik olarak almak için varsayılan olarak ayarlanır (DHCP). Bu nedenle, bir IP adresi, alt ağ ve ağ geçidi otomatik olarak (IPv4 ve IPv6 için) atanır.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Cihazın sağlama işlemi sırasında birden fazla ağ arabirimi eklediyseniz, bunları burada yapılandırabilirsiniz. Ağ Arabiriminizin IPv4 yalnızca veya IPv4 ve IPv6 yapılandırabileceğiniz unutmayın. Yalnızca IPv6 yapılandırmaları desteklenmez.
5. Cihazınız, bulut depolama hizmeti sağlayıcıları ile iletişim kurmak için veya Cihazınızı çözümlemeye çalıştığında, dosya sunucusu olarak yapılandırılmış ad tarafından kullanıldığı için DNS sunucusu gereklidir. İçinde **ağ ayarları** altındaki **DNS sunucuları**:
   
   1. Bir birincil ve ikincil DNS sunucusu otomatik olarak yapılandırılır. Statik IP adreslerini yapılandırmak isterseniz, DNS sunucuları belirtebilirsiniz. Yüksek kullanılabilirlik için birincil ve ikincil DNS sunucusu yapılandırmanızı öneririz.
   2. Tıklayın **Uygula** uygulamak ve ağ ayarlarını doğrulamak için.
6. İçinde **cihaz ayarları** sayfası:
   
   1. Benzersiz bir atama **adı** cihazınıza. Bu ad, 1-15 karakter uzunluğunda olabilir ve harf, rakam ve kısa çizgi içerebilir.
   2. Tıklayın **dosya sunucusu** simgesi ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) için **türü** oluşturmakta olduğunuz cihazın. Bir dosya sunucusu paylaşılan klasörler oluşturmanıza olanak sağlar.
   3. Cihazınızı bir dosya sunucusu olduğundan, cihaz bir etki alanına gerekecektir. Girin bir **etki alanı adı**.
   4. **Uygula**'ya tıklayın.
7. Bir iletişim kutusu görünür. Belirtilen biçimde etki alanı kimlik bilgilerinizi girin. Onay simgesine tıklayın. Etki alanı kimlik bilgilerinin doğrulanır. Kimlik bilgileri yanlışsa, bir hata iletisi görürsünüz.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. **Uygula**'ya tıklayın. Bu uygulama ve cihaz ayarlarını doğrulayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Sanal diziniz kendi kuruluş birimi (OU) için Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış veya devralınan emin olun. Grup İlkesi, StorSimple sanal dizisi üzerinde virüsten koruma yazılımı gibi uygulamalar yükleyebilir. Ek yazılım yüklemeniz, desteklenmez ve veri bozulmasına yol açabilir. 
   > 
   > 
9. (İsteğe bağlı olarak), web Ara sunucusunu yapılandırın. Web proxy yapılandırması isteğe bağlı olsa da, bir web proxy kullanıyorsanız, yalnızca, burada yapılandırabilirsiniz olduğunu unutmayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   İçinde **Web proxy** sayfası:
   
   1. Tedarik **Web proxy URL'si** şu biçimde: *http://&lt;konak IP adresi veya FQDN&gt;: bağlantı noktası numarası*. HTTPS URL'leri desteklenmediğine dikkat edin.
   2. Belirtin **kimlik doğrulaması** olarak **temel** veya **hiçbiri**.
   3. Kimlik doğrulama kullanılıyorsa, ayrıca sağlamanız gerekir bir **kullanıcıadı** ve **parola**.
   4. **Uygula**'ya tıklayın. Doğrulama ve yapılandırılmış web proxy ayarlarını uygulayın.
10. (İsteğe bağlı olarak) saat dilimi ve birincil ve ikincil NTP sunucuları gibi cihazınız için saat ayarlarını yapılandırın. Bu, bulut hizmeti sağlayıcılarıyla doğrulanabileceği şekilde Cihazınızı saati eşitlemesi gerekir çünkü NTP sunucuları gereklidir.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    İçinde **saat ayarlarını** sayfası:
    
    1. Açılır listeden seçin **saat dilimi** içinde cihaz dağıtıldığı coğrafi konum temelinde. Cihazınız için varsayılan saat dilimini PST ' dir. Cihazınız zamanlanan tüm işlemler için bu saat dilimini kullanır.
    2. Belirtin bir **birincil NTP sunucusu** cihazınız için veya time.windows.com varsayılan değerini kabul edin. Ağınızın, NTP trafiğini veri merkezinizden İnternete geçirilmesine izin vermesini sağlayın.
    3. İsteğe bağlı olarak belirtin bir **ikincil NTP sunucusu** cihazınız için.
    4. **Uygula**'ya tıklayın. Bu, doğrulamak ve yapılandırılan süre ayarlar uygulanır.
11. Cihazınız için bulut ayarlarını yapılandırın. Bu adımda, yerel cihaz yapılandırmasını tamamlayın ve ardından cihazı, StorSimple cihaz Yöneticisi hizmetiyle kaydedin.
    
    1. Girin **hizmet kayıt anahtarını** aldığınız [2. adım: Hizmet kayıt anahtarı alma](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple Virtual Array için.
    2. Bu ilk Cihazınızı bu hizmetle kaydetme ise, ile sunulacak **hizmet veri şifreleme anahtarı**. Bu anahtarı kopyalayın ve güvenli bir konuma kaydedin. StorSimple cihaz Yöneticisi hizmetiyle ek cihazlar kaydetmek için hizmet kayıt anahtarıyla birlikte bu anahtar gereklidir. 
       
       Bu hizmetle kaydettiriyor olduğunuz ilk cihaz bu değilse, hizmet veri şifreleme anahtarınızı sağlamanız gerekir. Daha fazla bilgi için Al başvurmak [hizmet veri şifreleme anahtarı](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) yerel web kullanıcı Arabirimi.
    3. Tıklayın **kaydetme**. Bu cihaz yeniden başlatılır. 2-3 cihaz başarıyla kaydedildikten önce dakika beklemeniz gerekebilir. Cihaz yeniden başlatıldıktan sonra oturum açma sayfasına ulaşabilirsiniz.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Azure portalına dönün. Git **tüm kaynakları**, StorSimple cihaz Yöneticisi hizmetinize arayın.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. Filtrelenen listede, StorSimple cihaz Yöneticisi hizmetinize seçin ve ardından gidin **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde, cihazın hizmete sorunsuz bağlandığını ve durumu doğrulayın **kuruluma hazır**.
    
    ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-the-device-as-file-server"></a>2. Adım: Dosya sunucusu olarak cihazı yapılandırma
Aşağıdaki adımlarda gerçekleştirmek [Azure portalında](https://portal.azure.com/) gerekli cihaz kurulumunu tamamlamak için.

#### <a name="to-configure-the-device-as-file-server"></a>Cihaz, dosya sunucusu olarak yapılandırmak için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından Git **Yönetim > cihazlar**. İçinde **cihazları** dikey penceresinde oluşturulan yeni bir cihaz seçin. Bu cihazı olarak gösterilmesini **kuruluma hazır**.
   
   ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Bir cihaza tıklayın ve cihaz için Kurulum hazır olduğunu belirten bir başlık iletisi görürsünüz.
   
    ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Tıklayın **yapılandırma** komut çubuğunda. Bu açılır **yapılandırma** dikey penceresi. İçinde **yapılandırma** dikey penceresinde aşağıdakileri yapın:
   
   1. Dosya sunucusu adı otomatik olarak doldurulur.
    
   2. Bulut depolama şifrelemesi emin olun kümesine **etkin**. Bu buluta gönderilen tüm verileri şifreler. 
    
   3. 256 bit AES anahtar şifreleme için kullanıcı tanımlı anahtar ile kullanılır. Bir 32 karakter anahtarı belirtin ve sonra da onaylamak için anahtarı girin. Gelecek başvurular için bir anahtar yönetimi uygulamasında kayıt anahtarı.
    
   4. Tıklayın **gerekli ayarları Yapılandır** cihazınızla kullanılacak depolama hesabı kimlik bilgilerini belirtmek için. Tıklayın **yeni Ekle** yapılandırılan depolama hesabı kimlik bilgilerine olup olmadığını. **Blok bloblarını destekler kullandığınız depolama hesabını emin olun. Sayfa blobları desteklenmez.** Hakkında daha fazla bilgi [engeller blobları ve sayfa blobları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
      ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. İçinde **bir depolama hesabı kimlik bilgileri ekleme** dikey penceresinde aşağıdakileri yapın: 

    1. Hizmet ile aynı abonelikte depolama hesabı ise, geçerli aboneliği seçin. Diğer depolama belirtmek dışında hizmet aboneliği hesabıdır. 
    
    2. Açılan listeden mevcut bir depolama hesabını seçin. 
    
    3. Konum, belirtilen depolama hesabına göre otomatik olarak doldurulur. 
    
    4. Bir cihaz ile bulut arasında güvenli ağ iletişim kanalı emin olmak SSL'yi etkinleştirin.
    
    5. Tıklayın **Ekle** bu depolama hesabı kimlik bilgisi eklemek için. 
   
        ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. Depolama hesabı kimlik bilgisi başarıyla oluşturulduktan sonra **yapılandırma** dikey penceresinde, belirtilen depolama hesabı kimlik bilgilerini görüntülemek için güncelleştirilir. **Yapılandır**'a tıklayın.
   
   ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Bir dosya görürsünüz sunucusu oluşturuluyor. Dosya sunucusu başarıyla oluşturulduğunda size bildirilir.
   
   ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   Cihaz durumu da değişir **çevrimiçi**.
   
   ![Dosya sunucusu yapılandırın](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   Bir paylaşımı ekleyebilirsiniz.

## <a name="step-3-add-a-share"></a>3. Adım: Paylaşım ekleme
Paylaşım oluşturmak için [Azure portalında](https://portal.azure.com/) aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-share"></a>Bir paylaşımı oluşturmak için
1. Önceki adımda yapılandırdığınız dosya sunucusu cihazı seçip tıklayın **...**  (veya sağ tıklayın). Bağlam menüsünde seçin **Ekle paylaşımı**. Alternatif olarak, tıklayabilirsiniz **+ paylaşım Ekle** cihaz komut çubuğunda.
   
   ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Aşağıdaki paylaşım ayarları belirtin:

   1. Paylaşımınız için benzersiz bir ad. Ad 3 ile 127 karakter içeren bir dize olmalıdır.
    
   2. İsteğe bağlı **açıklama** paylaşım için. Açıklama, paylaşım sahiplerini belirlemeye yardımcı olur.
    
   3. A **türü** paylaşım için. Türü olabilir **katmanlı** veya **yerel olarak sabitlenmiş**, varsayılan olan katmanlı. Yerel GARANTİLERİN, düşük gecikme süreleri ve daha yüksek performans gerektiren iş yükleri için seçin bir **yerel olarak sabitlenmiş** paylaşın. Diğer tüm veriler için seçin bir **katmanlı** paylaşın.
      Yerel olarak sabitlenmiş bir paylaşımı hazırlanmıştır ve birincil veri paylaşımında cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar. Öte yandan, katmanlı bir paylaşım sıkı hazırlanmıştır. Katmanlı bir paylaşım oluşturduğunuzda, %10 alan yerel katmanında sağlanır ve bulutta % 90 ' alanı sağlanır. Örneğin, 1 TB'lik bir birimi sağladıysanız, 100 GB yerel alanı bulunması ve 900 GB bulutta kullanılabilir olduğunda veri katmanları. Bu sırayla cihazda yerel alanın tümünü dışında çalıştırırsanız, katmanlı bir paylaşım sağlayamazsınız anlamına gelir.
   
   4. İçinde **ayarlanmış varsayılan tam izinleri** alanında, kullanıcının veya grubun bu paylaşıma erişim izinleri atayın. Kullanıcı veya kullanıcı grubunun adını *john\@contoso.com* biçimi. Bu paylaşımlara erişmek yönetici ayrıcalıkları izin vermek için bir kullanıcı grubuna (yerine tek bir kullanıcı) kullanmanızı öneririz. Burada izinleri atadıktan sonra, Dosya Gezgini'ni kullanarak bu izinlerde değişiklik yapabilirsiniz.
   
   5. Tıklayın **Ekle** paylaşımı oluşturmak için. 
    
       ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
       Paylaşım oluşturma işleminin devam ettiği size bildirilir.
   
       ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
      Belirtilen ayarlarla paylaşım oluşturulduktan sonra **paylaşımları** dikey penceresinde, yeni bir paylaşım yansıtacak şekilde güncelleştirilir. Varsayılan olarak, izleme ve yedekleme paylaşımı için etkinleştirilir.
   
      ![Paylaşım ekleme](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-to-the-share"></a>4. Adım: Paylaşıma bağlanma
Şimdi, önceki adımda oluşturduğunuz bir veya daha fazla paylaşımı bağlanmak gerekir. StorSimple Virtual Array'iniz için bağlı bir Windows Server ana bilgisayarınızda aşağıdaki adımları gerçekleştirin.

#### <a name="to-connect-to-the-share"></a>Paylaşımına bağlanmak için
1. Tuşuna ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + r Belirtin Çalıştır penceresinde *&#92; &#92; &lt;dosya sunucusu adı&gt;* yol olarak değiştirerek *dosya sunucusu adı* dosyanıza atanmış cihaz adı ile Sunucu. **Tamam** düğmesine tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Bu, dosya Gezgini'ni açar. Artık oluşturduğunuz paylaşımları klasörler olarak görebiliyor olmalısınız. İçeriğini görmek için paylaşımı (klasörü) seçin ve çift tıklayın.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Artık, dosyaları eklemek için bu paylaşımlar ve yedekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Yerel kullanmayı öğrenin web kullanıcı Arabirimi için [StorSimple Virtual Array'iniz yönetmek](storsimple-ova-web-ui-admin.md).

