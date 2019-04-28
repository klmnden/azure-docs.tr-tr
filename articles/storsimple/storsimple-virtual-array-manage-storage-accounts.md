---
title: StorSimple sanal dizisi depolama hesabı kimlik bilgilerini yönetme | Microsoft Docs
description: Eklemek, düzenlemek, silmek veya StorSimple sanal dizisi ile ilişkili depolama hesabı kimlik bilgileri için güvenlik anahtarlarını döndürmek için StorSimple cihaz Yöneticisi'ni yapılandırma sayfasında nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: a992851deda0659509c0ee4ea5de76b19734f017
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128847"
---
# <a name="use-storsimple-device-manager-to-manage-storage-account-credentials-for-storsimple-virtual-array"></a>StorSimple sanal dizisi için depolama hesabı kimlik bilgilerini yönetmek için StorSimple cihaz Yöneticisi'ni kullanın

## <a name="overview"></a>Genel Bakış
**Yapılandırma** StorSimple Virtual Array, StorSimple cihaz Yöneticisi hizmet dikey bölümünü StorSimple Yöneticisi hizmeti için oluşturulan genel hizmet parametreleri gösterir. Bu parametreler hizmete bağlı tüm cihazlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Erişim denetimi kayıtları
  
  ![Cihaz Yöneticisi hizmet Panosu](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Bu öğreticide nasıl ekleyebilir, düzenleme veya StorSimple Virtual Array'iniz için depolama hesabı kimlik bilgilerini silme açıklar. Bu öğreticide bilgileri yalnızca StorSimple Virtual Array için geçerlidir. 8000 serisi depolama hesaplarında yönetme hakkında daha fazla bilgi için bkz: [depolama hesabınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manage-storage-accounts.md).

Depolama hesabı kimlik bilgileri, cihaz, bulut hizmet sağlayıcınız ile depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bu hesap adına ve birincil erişim anahtarı gibi kimlik bilgileridir.

Üzerinde **depolama hesabı kimlik bilgileri** dikey penceresinde aşağıdaki bilgileri içeren bir tablo biçiminde faturalama aboneliği görüntülenir için oluşturduğunuz tüm depolama hesabı kimlik bilgileri:

* **Adı** – oluşturulduğunda hesabına atanmış bir benzersiz adı.
* **SSL etkin** – olmadığını SSL etkin ve CİHAZDAN buluta iletişimidir güvenli kanal üzerinden.
  
  ![Yapılandırma bölümü](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

En yaygın görevleri gerçekleştirilebilir depolama hesabı kimlik bilgileri ile ilgili **depolama hesabı kimlik bilgileri** dikey şunlardır:

* Depolama hesabı kimlik bilgisi ekleyin
* Bir depolama hesabı kimlik bilgilerini Düzenle
* Bir depolama hesabı kimlik bilgilerini silme

## <a name="types-of-storage-account-credentials"></a>Depolama hesabı kimlik bilgileri türü
StorSimple Cihazınızı kullanılabilir depolama hesabı kimlik bilgilerinin üç türü vardır.

* **Otomatik olarak oluşturulan depolama hesabı kimlik bilgileri** – adından da anlaşılacağı gibi bu tür bir depolama hesabı kimlik bilgisi hizmet ilk oluşturulduğunda otomatik olarak oluşturulur. Bu depolama hesabı kimlik bilgilerini nasıl oluşturulduğunu hakkında daha fazla bilgi için bkz: [yeni bir hizmet oluşturma](storsimple-virtual-array-manage-service.md#create-a-service).
* **Depolama hesabı kimlik bilgileri hizmet aboneliği** – bunlar aynı abonelikte, hizmet ile ilişkili Azure depolama hesabı kimlik bilgileri. Hakkında nasıl bu depolama hesabı kimlik bilgileri oluşturulur daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md).
* **Depolama hesabı kimlik bilgileri hizmet aboneliği dışında** – bunlar hizmet oluşturulmadan önce büyük olasılıkla var olmayan hizmetiniz ile ilişkili ve Azure depolama hesabı kimlik bilgileri.

## <a name="add-a-storage-account-credential"></a>Depolama hesabı kimlik bilgisi ekleyin
Depolama hesabı kimlik bilgisi benzersiz bir kolay ad ve depolama hesabına bağlı erişim kimlik bilgilerini sağlayarak, StorSimple cihaz Yöneticisi hizmet yapılandırmanıza ekleyebilirsiniz. Ayrıca, cihazınız ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için Güvenli Yuva Katmanı (SSL) modunu etkinleştirme seçeneğiniz de vardır.

Belirli bir bulut hizmeti sağlayıcısı için birden çok hesap oluşturabilirsiniz. Depolama hesabı kimlik bilgileri kaydedilirken, hizmet, bulut hizmet sağlayıcınız ile iletişim kurmak çalışır. Şu anda kimliği doğrulanmış kimlik bilgileri ve size sağlanan erişim malzemeleri. Yalnızca kimlik doğrulaması başarılı olursa, bir depolama hesabı kimlik bilgisi oluşturulur. Kimlik doğrulaması başarısız olursa, ardından uygun bir hata iletisi görüntülenir.

Azure depolama hesabı kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Cihaz Yöneticisi hizmetiyle aynı Azure aboneliği olan bir depolama hesabı kimlik bilgisi eklemek için
* Cihaz Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Cihaz Yöneticisi hizmetiyle aynı Azure aboneliği olan bir depolama hesabı kimlik bilgisi eklemek için

1. Cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü.
3. **Ekle**'ye tıklayın.
4. İçinde **bir depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **geçerli**.
    2. Azure depolama hesabınızın adını sağlayın.
    3. Seçin **etkinleştirme** StorSimple cihazınız ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için. Seçin **devre dışı** yalnızca, bir özel bulutta işlem yapıyorsanız temizleyin.
    4. **Ekle**'ye tıklayın. Depolama hesabı başarıyla oluşturulduktan sonra size bildirilir.<br></br>
   
        ![Mevcut bir depolama hesabı kimlik bilgisi Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="to-add-an-azure-storage-account-credential-that-is-outside-of-the-device-manager-service-subscription"></a>Cihaz Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

1. Cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü. Bu StorSimple cihaz Yöneticisi hizmeti ile ilişkili herhangi bir mevcut depolama hesabı kimlik listeler.
3. **Ekle**'ye tıklayın.
4. İçinde **bir depolama hesabı ekleme** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **diğer**.
   
    2. Azure depolama hesabı kimlik bilgilerinizi adını sağlayın.
   
    3. İçinde **depolama hesabı erişim anahtarı** metin kutusunda, birincil erişim anahtarı için Azure depolama hesabı kimlik bilgilerinizi sağlayın. Bu anahtarı almak için Azure depolama hizmetine, depolama hesabı kimlik bilgilerini seçin ve tıklayın Git **hesabı anahtarlarını yönetme**. Birincil erişim anahtarını artık kopyalayabilirsiniz.
   
    4. SSL etkinleştirmek için tıklayın **etkinleştirme** StorSimple cihaz Yöneticisi hizmetiniz ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için. Tıklayın **devre dışı** yalnızca, bir özel bulutta işlem yapıyorsanız düğmesi.
   
    5. **Ekle**'ye tıklayın. Depolama hesabı kimlik bilgisi başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgisi altında yapılandırma StorSimple cihaz Yöneticisi hizmet dikey penceresinde görüntülenen **depolama hesabı kimlik bilgileri**.
   
    ![Cihaz Yöneticisi hizmeti abonelik dışında depolama hesabı kimlik bilgisi Ekle](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini Düzenle
Cihazınız tarafından kullanılan depolama hesabı kimlik bilgisi düzenleyebilirsiniz. Şu anda kullanılmakta olan bir depolama hesabı kimlik bilgilerini Düzenle, erişim anahtarı ve depolama hesabı kimlik bilgisi için SSL modu değiştirmek kullanılabilir alanları demektir. Yeni depolama erişim anahtarı sağlamanız ya da değiştirme **SSL'yi etkinleştir modu** seçimi ve güncelleştirilmiş ayarları kaydedin.

#### <a name="to-edit-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini düzenlemek için
1. Cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü. Bu StorSimple cihaz Yöneticisi hizmeti ile ilişkili herhangi bir mevcut depolama hesabı kimlik listeler.
3. Depolama hesabı kimlik bilgilerinin tablosal listesinde seçin ve değiştirmek istediğiniz hesaba çift tıklayın.
4. Depolama hesabı kimlik **özellikleri** dikey penceresinde aşağıdakileri yapın:
   
   1. Gerekirse, değiştirebileceğiniz, **SSL'yi etkinleştir** kipinde seçim.
   2. Depolama hesabı kimlik bilgisi erişim anahtarlarınızı yeniden seçebilirsiniz. Daha fazla bilgi için [depolama hesabı anahtarlarını yeniden](../storage/common/storage-account-manage.md#access-keys). Yeni depolama hesabı kimlik bilgileri anahtarını sağlayın. Azure depolama hesabınız için birincil erişim anahtarını budur.
   3. Tıklayın **Kaydet** en üstündeki **özellikleri** ayarları kaydetmek için dikey pencere. Şirket ayarları güncellendi **depolama hesabı kimlik bilgileri** dikey penceresi.
      
      ![Bir depolama hesabı kimlik bilgilerini Düzenle](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgilerini silme
> [!IMPORTANT]
> Yalnızca kullanımda olmayan bir depolama hesabı kimlik bilgilerini silebilirsiniz. Depolama hesabı kimlik bilgisi kullanılıyorsa, size bildirilir.
> 
> 

#### <a name="to-delete-a-storage-account-credential"></a>Bir depolama hesabı kimlik bilgisini silmek için
1. Cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü. Bu StorSimple cihaz Yöneticisi hizmeti ile ilişkili herhangi bir mevcut depolama hesabı kimlik listeler.
3. Depolama hesabı kimlik bilgilerinin tablosal listesinde seçin ve silmek istediğiniz hesaba çift tıklayın.
4. Depolama hesabı kimlik **özellikleri** dikey penceresinde aşağıdakileri yapın:
   
   1. Tıklayın **Sil** kimlik bilgileri silinemedi.
   2. Onayınız istendiğinde tıklayın **Evet** silme işlemine devam etmek için. Tablosal değişiklikleri yansıtacak şekilde güncelleştirilir.
      
      ![Bir depolama hesabı kimlik bilgilerini silme](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Depolama hesabı kimlik bilgisi anahtarları eşitleniyor
Güvenlik nedenleriyle, anahtar döndürme genellikle bir veri merkezlerinde gereksinimdir. Microsoft Azure yönetici, yeniden oluşturulmuş veya depolama hesabı kimlik bilgisi (Microsoft Azure depolama hizmeti) aracılığıyla doğrudan erişerek birincil veya ikincil anahtarını değiştirin. StorSimple cihaz Yöneticisi hizmeti bu değişikliği otomatik olarak görmez.

StorSimple cihaz Yöneticisi hizmeti değişikliği bildirmek için StorSimple cihaz Yöneticisi hizmetine erişim depolama hesabı kimlik ve sonra (hangisi bağlı olarak değiştirildi) birincil veya ikincil anahtarı Eşitle gerekir. Hizmet daha sonra en son anahtarı alır, anahtarları şifreler ve şifrelenmiş anahtarı cihaza gönderir.

#### <a name="to-synchronize-keys-for-storage-account-credentials-in-the-same-subscription-as-the-service-azure-only"></a>Anahtarlar (yalnızca Azure) hizmet ile aynı abonelikte depolama hesabı kimlik için eşitleme
1. Hizmeti giriş dikey penceresinde, hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma** bölümünde **depolama hesabı kimlik bilgileri**.
2. Üzerinde **depolama hesabı kimlik bilgileri** dikey penceresinde depolama hesabı kimlik bilgisi, eşitlemek istediğiniz anahtarları depolama hesabı kimlik bilgileri, select listesinde.
3. İçinde **özellikleri** dikey penceresinde seçilen depolama hesabı kimlik bilgileri için şunları yapın:
   
    1. Tıklayın **daha fazla**ve ardından **erişim anahtarını Eşitle**.
   
    2. Onayınız istendiğinde tıklayın **eşitleme anahtarı** eşitlemenin tamamlanması.
    
4. StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure depolama hizmetinde önceden değiştirildiği anahtarı güncelleştirmeniz gerekir. İçinde **Eşitle depolama hesabı anahtarı** birincil erişim anahtarını (yeniden oluşturuldu) değiştirildiyse dikey penceresinde, birincil tıklayın ve ardından **eşitleme anahtarı**. İkincil anahtar değiştirildiyse tıklayın **ikincil**ve ardından **eşitleme anahtarı**.
   
    ![Erişim anahtarını Eşitle](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-access-key.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple Virtual Array'iniz yönetmek](storsimple-ova-web-ui-admin.md).

