---
title: "StorSimple sanal dizi depolama hesabının kimlik bilgilerini yönetme | Microsoft Docs"
description: "Ekleme, düzenleme, silme veya depolama hesabı kimlik bilgileri ile StorSimple sanal dizinin ilişkili güvenlik tuşları döndürmek için StorSimple Aygıt Yöneticisi'ni yapılandırma sayfasında nasıl kullanabileceğiniz açıklanır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: a4ce2d329d0e1399cffaf886adf2b95e34b9cd7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-storsimple-device-manager-to-manage-storage-account-credentials-for-storsimple-virtual-array"></a>StorSimple sanal dizisi için depolama hesabının kimlik bilgilerini yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
**Yapılandırma** , StorSimple sanal dizinin StorSimple cihaz Yöneticisi hizmeti dikey bölümünü StorSimple Yöneticisi hizmeti oluşturulabilir genel hizmet parametreleri gösterir. Bu parametreler hizmete bağlı tüm aygıtlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Erişim denetimi kayıtları
  
  ![Aygıt Yöneticisi'ni hizmet Panosu](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Bu öğretici nasıl ekleyebilir, düzenleme veya StorSimple sanal dizisi için depolama hesabının kimlik bilgilerini sil açıklanmaktadır. Bu öğreticide bilgileri yalnızca StorSimple sanal dizinin geçerlidir. 8000 serisi depolama hesaplarında yönetme hakkında daha fazla bilgi için bkz: [depolama hesabınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manage-storage-accounts.md).

Depolama hesabının kimlik bilgilerini cihaz bulut hizmeti sağlayıcısı ile depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bu hesap adı ve birincil erişim anahtarını gibi kimlik bilgileridir.

Üzerinde **depolama hesabının kimlik bilgilerini** dikey penceresinde, fatura abonelik görüntülenir için aşağıdaki bilgileri içeren bir tablo biçiminde oluşturduğunuz tüm depolama hesabı kimlik bilgileri:

* **Ad** – oluşturulduğunda bu hesaba atanan benzersiz bir ad.
* **SSL özellikli** – olup olmadığını SSL etkinleştirilmişse ve cihaz bulut iletişimidir güvenli kanal üzerinden.
  
  ![Yapılandırma bölümü](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

En yaygın görevleri üzerinde gerçekleştirilen depolama hesabı kimlik bilgileri ile ilgili **depolama hesabının kimlik bilgilerini** dikey şunlardır:

* Depolama hesabı kimlik bilgisi ekleyin
* Bir depolama hesabı kimlik bilgilerini Düzenle
* Bir depolama hesabı kimlik bilgilerini sil

## <a name="types-of-storage-account-credentials"></a>Türleri depolama hesabı kimlik bilgileri
StorSimple cihazınızla kullanılabilir depolama hesabı kimlik bilgileri üç tür vardır.

* **Otomatik olarak oluşturulan depolama hesabının kimlik bilgilerini** – adı da anlaşılacağı gibi bu tür bir depolama hesabı kimlik bilgilerini hizmet ilk oluşturulduğunda otomatik olarak oluşturulur. Bu depolama hesabı kimlik bilgilerini nasıl oluşturulduğunu hakkında daha fazla bilgi için bkz: [yeni bir hizmet Oluştur](storsimple-virtual-array-manage-service.md#create-a-service).
* **Hizmet aboneliği depolama hesabı kimlik bilgilerini** –, hizmetin aynı abonelik ile ilişkili Azure depolama hesabı bilgilerini bunlar. Hakkında nasıl bu depolama hesabı kimlik bilgileri oluşturulan daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md).
* **Hizmet aboneliği dışında depolama hesabının kimlik bilgilerini** – bu değil hizmetiniz ile ilişkili ve büyük olasılıkla hizmet oluşturulmadan önce varolan Azure depolama hesabı kimlik bilgileridir.

## <a name="add-a-storage-account-credential"></a>Depolama hesabı kimlik bilgisi ekleyin
Benzersiz bir kolay ad ve depolama hesabına bağlı erişim kimlik bilgileri sağlayarak bir depolama hesabı kimlik bilgilerini StorSimple cihaz Yöneticisi hizmeti yapılandırmanızı ekleyebilirsiniz. Ayrıca, cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için Güvenli Yuva Katmanı (SSL) modunu etkinleştirme seçeneğiniz de vardır.

Verilen bulut hizmeti sağlayıcı için birden çok hesabı oluşturabilirsiniz. Depolama hesabı kimlik bilgilerini kaydedilirken, hizmet, bulut hizmeti sağlayıcısı ile iletişim kurmaya çalışır. Kimlik bilgileri ve sağladığınız erişim malzemeleri şu anda doğrulanır. Bir depolama hesabı kimlik bilgileri yalnızca kimlik doğrulaması başarılı olursa oluşturulur. Kimlik doğrulama başarısız olursa, ilgili hata iletisi görüntülenir.

Azure depolama hesabının kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Aygıt Yöneticisi hizmeti aynı Azure abonelik sahip bir depolama hesabı kimlik bilgilerini eklemek için
* Aygıt Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgilerini eklemek için

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Aygıt Yöneticisi hizmeti aynı Azure abonelik sahip bir depolama hesabı kimlik bilgilerini eklemek için

1. Aygıt Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü.
3. **Ekle**'ye tıklayın.
4. İçinde **depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **geçerli**.
    2. Azure depolama hesabınızın adını sağlayın.
    3. Seçin **etkinleştirmek** StorSimple cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için. Seçin **devre dışı** yalnızca özel bir bulutta işlem yapıyorsanız.
    4. **Ekle**'ye tıklayın. Depolama hesabı başarıyla oluşturulduktan sonra size bildirilir.<br></br>
   
        ![Varolan bir depolama hesabı kimlik bilgileri Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="to-add-an-azure-storage-account-credential-that-is-outside-of-the-device-manager-service-subscription"></a>Aygıt Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgilerini eklemek için

1. Aygıt Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü. StorSimple cihaz Yöneticisi hizmetiyle ilişkili tüm var olan depolama hesabının kimlik bilgilerini listeler.
3. **Ekle**'ye tıklayın.
4. İçinde **depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **diğer**.
   
    2. Azure depolama hesabı kimlik bilgilerinizi adını sağlayın.
   
    3. İçinde **depolama hesabının erişim anahtarı** metin kutusunda, Azure depolama hesabı kimlik bilgileriniz için birincil erişim anahtarını girin. Bu anahtarı almak için Azure depolama hizmeti için depolama hesabı kimlik bilgilerinizi seçin ve tıklayın Git **hesabı anahtarları Yönet**. Birincil erişim anahtarını şimdi kopyalayabilirsiniz.
   
    4. SSL etkinleştirmek için **etkinleştirmek** StorSimple cihaz Yöneticisi hizmetiniz ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için düğmesi. Tıklatın **devre dışı** yalnızca özel bir bulutta işlem yapıyorsanız düğmesine tıklayın.
   
    5. **Ekle**'ye tıklayın. Depolama hesabı kimlik bilgilerini başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgileri altında StorSimple yapılandırma Aygıt Yöneticisi'ni hizmet dikey penceresinde görüntülenir **depolama hesabının kimlik bilgilerini**.
   
    ![Aygıt Yöneticisi'ni hizmet aboneliği dışında bir depolama hesabı kimlik bilgileri Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini Düzenle
Cihazınız tarafından kullanılan depolama hesabı kimlik bilgilerini düzenleyebilirsiniz. Şu anda kullanılmakta olan bir depolama hesabı kimlik bilgilerini düzenlerseniz, değiştirmek kullanılabilir erişim anahtarı ve depolama hesabı kimlik bilgisi için SSL modu alanlardır. Yeni depolama erişim tuşunu sağlayın veya değiştirmek için **SSL'yi etkinleştir modu** seçimi ve güncelleştirilmiş ayarları kaydedin.

#### <a name="to-edit-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini düzenlemek için
1. Aygıt Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü. StorSimple cihaz Yöneticisi hizmetiyle ilişkili tüm var olan depolama hesabının kimlik bilgilerini listeler.
3. Depolama hesabı kimlik bilgileri Tablo listesinde seçin ve değiştirmek istediğiniz hesaba çift tıklayın.
4. Depolama hesabı kimlik bilgilerini de **özellikleri** dikey penceresinde aşağıdakileri yapın:
   
   1. Gerekirse, değiştirebileceğiniz varsa **SSL'yi etkinleştir** mod seçimi.
   2. Depolama hesabı kimlik bilgileri erişim anahtarlarını yeniden seçebilirsiniz. Daha fazla bilgi için bkz: [depolama hesabı anahtarlarını yeniden](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys). Yeni depolama hesabı kimlik bilgisi anahtarı sağlayın. Bir Azure depolama hesabı için birincil erişim anahtarını budur.
   3. Tıklatın **kaydetmek** en üstündeki **özellikleri** ayarları kaydetmek için dikey. Ayarları güncelleştirilir **depolama hesabının kimlik bilgilerini** dikey.
      
      ![Bir depolama hesabı kimlik bilgilerini Düzenle](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini sil
> [!IMPORTANT]
> Yalnızca kullanımda değilse, bir depolama hesabı kimlik bilgilerini silebilirsiniz. Bir depolama hesabı kimlik bilgisi kullanımda olduğunda size bildirilir.
> 
> 

#### <a name="to-delete-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini silmek için
1. Aygıt Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü. StorSimple cihaz Yöneticisi hizmetiyle ilişkili tüm var olan depolama hesabının kimlik bilgilerini listeler.
3. Depolama hesabı kimlik bilgileri Tablo listesinde seçin ve silmek istediğiniz hesaba çift tıklayın.
4. Depolama hesabı kimlik bilgilerini de **özellikleri** dikey penceresinde aşağıdakileri yapın:
   
   1. Tıklatın **silmek** kimlik bilgilerini silmek için.
   2. Onayınız istendiğinde tıklatın **Evet** silme işlemine devam etmek için. Sekmeli liste değişiklikleri yansıtacak şekilde güncelleştirilir.
      
      ![Bir depolama hesabı kimlik bilgilerini sil](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Depolama hesabı kimlik bilgileri anahtarları eşitleniyor
Güvenlik nedenleriyle, anahtar döndürme genellikle veri merkezlerinde gerekli değildir. Microsoft Azure yönetici yeniden oluşturmak veya doğrudan depolama hesabı kimlik bilgilerini (aracılığıyla Microsoft Azure depolama hizmeti) erişerek birincil veya ikincil anahtarı değiştirin. StorSimple cihaz Yöneticisi hizmeti bu değişikliği otomatik olarak görmez.

StorSimple cihaz Yöneticisi hizmeti değişikliği bildirmek için StorSimple cihaz Yöneticisi hizmetine erişim depolama hesabı kimlik bilgilerini erişmek ve (hangisinin bağlı olarak değiştirildi) birincil veya ikincil anahtarı Eşitle gerekir. Hizmet sonra en son anahtarın alır, anahtarları şifreler ve şifrelenmiş anahtar cihaza gönderir.

#### <a name="to-synchronize-keys-for-storage-account-credentials-in-the-same-subscription-as-the-service-azure-only"></a>(Yalnızca Azure) hizmet ile aynı abonelikte depolama hesabı kimlik bilgileri tuşları eşitlemek için
1. Hizmet giriş dikey penceresinde, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** 'yi tıklatın **depolama hesabının kimlik bilgilerini**.
2. Üzerinde **depolama hesabının kimlik bilgilerini** dikey penceresinde, depolama hesabı kimlik bilgileri eşitlemek istediğiniz, anahtarları depolama hesabının kimlik bilgilerini, select listesi.
3. İçinde **özellikleri** seçilen depolama hesabı kimlik bilgileri dikey penceresinde aşağıdakileri yapın:
   
    1. Tıklatın **daha fazla**ve ardından **eşitleme erişim tuşu**.
   
    2. Onayınız istendiğinde tıklatın **eşitleme anahtarı** eşitleme işlemini tamamlamak için.
    
4. StorSimple cihaz Yöneticisi hizmeti daha önce Microsoft Azure depolama hizmetinde değiştirildi anahtarı güncelleştirmeniz gerekir. İçinde **Eşitle depolama hesabı anahtarı** birincil erişim anahtarını (oluşturuldu) değiştirilmişse, dikey penceresinde, birincil tıklayın ve ardından **eşitleme anahtarı**. İkincil anahtar değiştirdiyseniz tıklatın **ikincil**ve ardından **eşitleme anahtarı**.
   
    ![Eşitleme erişim anahtarı](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple sanal dizinizi yönetmek](storsimple-ova-web-ui-admin.md).

