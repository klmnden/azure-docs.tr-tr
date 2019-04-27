---
title: Microsoft Azure StorSimple 8000 serisi cihazlar için StorSimple depolama hesabı kimlik bilgilerinizi yönetme | Microsoft Docs
description: Nasıl eklemek, düzenlemek, silmek için StorSimple cihaz Yöneticisi'ni yapılandırma sayfasında kullanın, veya bir depolama hesabı için güvenlik anahtarlarını döndürme açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 53aa442b86f5c82ded2f212a64f43852e6b3d2c5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60632778"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

**Yapılandırma** StorSimple cihaz Yöneticisi hizmet dikey bölümde StorSimple cihaz Yöneticisi hizmeti için oluşturulan tüm genel hizmet parametreleri gösterir. Bu parametreler hizmete bağlı tüm cihazlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Bant genişliği şablonları 
* Erişim denetimi kayıtları 

Bu öğreticide, eklemek, düzenlemek veya depolama hesabı kimlik bilgilerini silme veya bir depolama hesabı için güvenlik anahtarlarını döndürme açıklanmaktadır.

 ![Depolama hesabı kimlik bilgileri listesi](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Depolama hesapları StorSimple cihazı ile bulut hizmeti sağlayıcısı depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bu hesap adına ve birincil erişim anahtarı gibi kimlik bilgileridir. 

Üzerinde **depolama hesabı kimlik bilgileri** dikey penceresinde aşağıdaki bilgileri içeren bir tablo biçiminde faturalama aboneliği görüntülenir için oluşturduğunuz tüm depolama hesapları:

* **Adı** – oluşturulduğunda hesabına atanmış bir benzersiz adı.
* **SSL etkin** – olmadığını SSL etkin ve CİHAZDAN buluta iletişimidir güvenli kanal üzerinden.
* **Tarafından kullanılan** – depolama hesabı'nı kullanarak birim sayısı.

Gerçekleştirilen depolama hesaplarına ilgili en yaygın görevler şunlardır:

* Bir depolama hesabı ekleme 
* Bir depolama hesabı Düzenle 
* Bir depolama hesabını silme 
* Depolama hesabı anahtar rotasyonu 

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

StorSimple Cihazınızı kullanılabilir depolama hesapları üç tür vardır.

* **Otomatik olarak oluşturulan depolama hesapları** – adından da anlaşılacağı gibi bu depolama hesabı türü hizmet ilk oluşturulduğunda otomatik olarak oluşturulur. Bu depolama hesabı nasıl oluşturulur hakkında daha fazla bilgi için bkz: [1. adım: Yeni bir hizmet oluşturma](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md). 
* **Hizmet aboneliği depolama hesaplarında** – bunlar aynı abonelikte, hizmet ile ilişkili Azure depolama hesaplarıdır. Hakkında nasıl bu depolama hesapları oluşturulur. daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md). 
* **Hizmet aboneliği dışında depolama hesapları** – bunlar hizmet oluşturulmadan önce büyük olasılıkla var olmayan hizmetiniz ile ilişkili ve Azure depolama hesaplarıdır.

## <a name="add-a-storage-account"></a>Bir depolama hesabı ekleme

Bir depolama hesabı, benzersiz bir kolay ad ve (ile belirtilen bulut hizmeti sağlayıcısı) depolama hesabına bağlı erişim kimlik bilgilerini sağlayarak ekleyebilirsiniz. Ayrıca, cihazınız ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için Güvenli Yuva Katmanı (SSL) modunu etkinleştirme seçeneğiniz de vardır.

Belirli bir bulut hizmeti sağlayıcısı için birden çok hesap oluşturabilirsiniz. Ancak, bir depolama hesabı oluşturulduktan sonra bulut hizmeti sağlayıcısının değiştiremeyeceğinizi unutmayın.

Depolama hesabı kaydedilirken, hizmet, bulut hizmet sağlayıcınız ile iletişim kurmak çalışır. Şu anda, kimlik bilgilerini ve size sağlanan erişim malzemeleri doğrulanır. Yalnızca kimlik doğrulaması başarılı olursa, bir depolama hesabı oluşturulur. Kimlik doğrulaması başarısız olursa, uygun bir hata iletisi görüntülenir.

Azure depolama hesabı kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Cihaz Yöneticisi hizmetiyle aynı Azure aboneliği olan bir depolama hesabı kimlik bilgisi eklemek için
* Cihaz Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="to-add-an-azure-storage-account-credential-outside-of-the-storsimple-device-manager-service-subscription"></a>StorSimple cihaz Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgisi eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize select gidin ve çift tıklayın. Bu açılır **genel bakış** dikey penceresi.
2. Seçin **depolama hesabı kimlik bilgileri** içinde **yapılandırma** bölümü. Bu StorSimple cihaz Yöneticisi hizmeti ile ilişkili herhangi bir mevcut depolama hesabı kimlik listeler.
3. **Ekle**'ye tıklayın.
4. İçinde **bir depolama hesabı kimlik bilgisi Ekle** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **diğer**.
   
    2. Azure depolama hesabı kimlik bilgilerinizi adını sağlayın.
   
    3. İçinde **depolama hesabı erişim anahtarı** metin kutusunda, birincil erişim anahtarı için Azure depolama hesabı kimlik bilgilerinizi sağlayın. Bu anahtarı almak için Azure depolama hizmetine, depolama hesabı kimlik bilgilerini seçin ve tıklayın Git **hesabı anahtarlarını yönetme**. Birincil erişim anahtarını artık kopyalayabilirsiniz.
   
    4. SSL etkinleştirmek için tıklayın **etkinleştirme** StorSimple cihaz Yöneticisi hizmetiniz ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak için. Tıklayın **devre dışı** yalnızca, bir özel bulutta işlem yapıyorsanız düğmesi.
   
    5. **Ekle**'ye tıklayın. Depolama hesabı kimlik bilgisi başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgisi altında yapılandırma StorSimple cihaz Yöneticisi hizmet dikey penceresinde görüntülenen **depolama hesabı kimlik bilgileri**.
   


## <a name="edit-a-storage-account"></a>Bir depolama hesabı Düzenle

Birim kapsayıcısı tarafından kullanılan depolama hesabı düzenleyebilirsiniz. Şu anda kullanılmakta olan bir depolama hesabı düzenlerseniz, yalnızca değiştirmek kullanılabilir depolama hesabı için erişim anahtarı bir alandır. Yeni depolama erişim anahtarı sağlayın ve güncelleştirilmiş ayarları kaydedin.

#### <a name="to-edit-a-storage-account"></a>Bir depolama hesabı düzenleme

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

    ![Depolama hesabı kimlik bilgileri](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. İçinde **depolama hesabı kimlik bilgileri** dikey penceresinde depolama hesabı kimlik bilgileri, listesinden seçin ve düzenlemek istediğiniz öğeyi tıklatın. 

3. Değiştirebileceğiniz **SSL'yi etkinleştir** seçimi. Ayrıca **daha...**  seçip **döndürmek için erişim anahtarını Eşitle** depolama hesabı erişim anahtarlarınızı. Git [anahtar döndürme depolama hesaplarının](#key-rotation-of-storage-accounts) anahtar döndürme gerçekleştirme hakkında daha fazla bilgi için. Ayarları değiştirdikten sonra tıklayın **Kaydet**. 

    ![Düzenlenen depolama hesabı kimlik bilgilerini Kaydet](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Değişiklikleri onaylayın](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

Ayarlar güncelleştirildi ve depolama hesabınız için kaydedildi. 

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

> [!IMPORTANT]
> Yalnızca bir birim kapsayıcısı tarafından kullanılmıyorsa, bir depolama hesabı silebilirsiniz. Birim kapsayıcısı tarafından kullanılan bir depolama hesabı, birim kapsayıcısını silin ve ilişkili depolama hesabını silin.

#### <a name="to-delete-a-storage-account"></a>Bir depolama hesabını silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

2. Tablo depolama hesapları listesinde, silmek istediğiniz hesabın gelin. Sağ tıklayın ve bağlam menüsünü açmak için **Sil**.

    ![Depolama hesabı kimlik bilgilerini sil](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Onayınız istendiğinde tıklayın **Evet** silme işlemine devam etmek için. Tablosal değişiklikleri yansıtacak şekilde güncelleştirilir.

    ![Silmeyi onayla](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Depolama hesabı anahtar rotasyonu

Güvenlik nedenleriyle, anahtar döndürme genellikle bir veri merkezlerinde gereksinimdir. Her bir Microsoft Azure aboneliği bir veya daha fazla ilişkili depolama hesabı olabilir. Bu hesaplara erişim, her depolama hesabı için abonelik ve erişim anahtarları tarafından denetlenir. 

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure depolama hesabına erişim sağlandığında kimlik doğrulaması için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sahip aazure storage izmetinizde herhangi bir kesinti olmaz veya bu hizmete erişim anahtarlarını yeniden oluşturmak üzere sağlar. Şu anda kullanılmakta olan anahtar *birincil* yedekleme anahtarı olarak adlandırılır *ikincil* anahtarı. Microsoft Azure StorSimple Cihazınızı bulut depolama hizmeti sağlayıcınızla eriştiğinde, bu iki anahtarlarından biri sağlanmalıdır.

## <a name="what-is-key-rotation"></a>Anahtar döndürme nedir?

Genellikle, uygulamaları yalnızca birini anahtarları verilerinize erişmek için kullanın. Belirli bir süre saat sonra ikinci anahtarınızı kullanmaya geçiş yapın, uygulamalarınız olabilir. Uygulamalarınız için ikincil anahtar geçirildikten sonra ilk anahtarı devre dışı bırakma ve ardından yeni bir anahtar oluşturun. Bu şekilde iki anahtar kullanarak veri uygulamaları erişiminizi herhangi kapalı kalma süresi olmaksızın sağlar.

Depolama hesabı anahtarları, her zaman şifrelenmiş biçimde hizmetinde depolanır. Ancak, bu StorSimple cihaz Yöneticisi hizmeti ile sıfırlanabilir. Birincil anahtar ve ikincil anahtar tüm depolama hesapları için bir StorSimple cihaz Yöneticisi hizmetine ilk kez yüklendiğinde oluşturulan varsayılan depolama hesaplarının yanı sıra depolama hizmetinde oluşturulan hesapları dahil olmak üzere aynı abonelikte, hizmet alabilirsiniz oluşturuldu. StorSimple cihaz Yöneticisi hizmeti, her zaman bu anahtarlar Azure Klasik Portalı'ndan almak ve bunları şifrelenmiş olarak depolayın.

## <a name="rotation-workflow"></a>Döndürme iş akışı

Microsoft Azure yönetici, yeniden oluşturulmuş veya depolama hesabı (Microsoft Azure depolama hizmeti) aracılığıyla doğrudan erişerek birincil veya ikincil anahtarını değiştirin. StorSimple cihaz Yöneticisi hizmeti bu değişikliği otomatik olarak görmez.

StorSimple cihaz Yöneticisi hizmeti değişikliği bildirmek için StorSimple cihaz Yöneticisi hizmetine erişmek için depolama hesabına erişmek ve sonra (hangisi bağlı olarak değiştirildi) birincil veya ikincil anahtarı Eşitle. Hizmet daha sonra en son anahtarı alır, anahtarları şifreler ve şifrelenmiş anahtarı cihaza gönderir.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service"></a>Hizmet ile aynı abonelikte depolama hesaplarının anahtarlarını eşitleme 
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.
2. Tablo depolama hesapları listeleme yapmaya, değiştirmek istediğiniz bir tıklayın. 

    ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Tıklayın **... Daha fazla** seçip **döndürmek için erişim anahtarını Eşitle**.   

    ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure depolama hizmetinde önceden değiştirildiği anahtarı güncelleştirmeniz gerekir. Birincil erişim anahtarını yeniden (oluşturuldu) değiştirildiyse seçin **birincil** anahtarı. İkincil anahtar değiştirildiyse seçin **ikincil** anahtarı. Tıklayın **eşitleme anahtarı**.
      
      ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Anahtar başarıyla eşitlendikten sonra size bildirilir.

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Hizmet aboneliği dışında depolama hesaplarının anahtarlarını eşitleme
1. Üzerinde **Hizmetleri** sayfasında **yapılandırma** sekmesi.
2. Tıklayın **depolama hesapları ekleme/düzenleme**.
3. İletişim kutusunda, aşağıdakileri yapın:
   
   1. Erişim anahtarı ile güncelleştirmek istediğiniz depolama hesabını seçin.
   2. StorSimple cihaz Yöneticisi hizmetine depolama erişim tuşu güncelleştirmeniz gerekir. Bu durumda, depolama erişim anahtarı görebilirsiniz. Yeni anahtar girin **depolama hesabı erişim anahtarı** kutusu. 
   3. Yaptığınız değişiklikleri kaydedin. Depolama hesabı erişim anahtarınız artık güncelleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

