---
title: "StorSimple depolama hesabınızı yönetme | Microsoft Docs"
description: "Nasıl StorSimple Yöneticisi yapılandırma sayfasında kullanabilir eklemek için Düzenle, Sil, veya bir depolama hesabı için güvenlik anahtarları döndürme açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 4bf8b397d81e12bc48fe3d0ce16d5fff705cedbc
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Depolama hesabınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [depolama hesabınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-8000-manage-storage-accounts.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Genel Bakış
**Yapılandırma** StorSimple Yöneticisi hizmeti oluşturulan tüm genel hizmet parametreleri sayfası gösterir. Bu parametreler hizmete bağlı tüm aygıtlara uygulanabilir ve şunları içerir:

* Depolama hesapları 
* Bant genişliği şablonları 
* Erişim denetimi kayıtları 

Bu öğretici nasıl kullanabileceğinizi açıklar **yapılandırma** eklemek, düzenlemek veya silmek depolama hesapları için sayfasında veya bir depolama hesabının güvenlik anahtarlarını döndürün.

 ![Yapılandırma sayfası](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Depolama hesapları, cihaz bulut hizmeti sağlayıcısı ile depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bu hesap adı ve birincil erişim anahtarını gibi kimlik bilgileridir. 

Üzerinde **yapılandırma** sayfası, fatura abonelik görüntülenir için aşağıdaki bilgileri içeren bir tablo biçiminde oluşturulan tüm depolama hesapları:

* **Ad** – oluşturulduğunda bu hesaba atanan benzersiz bir ad.
* **SSL özellikli** – olup olmadığını SSL etkinleştirilmişse ve cihaz bulut iletişimidir güvenli kanal üzerinden.
* **Tarafından kullanılan** – depolama hesabı kullanarak birimlerin sayısı.

En yaygın görevleri ilgili üzerinde gerçekleştirilen depolama hesaplarına **yapılandırma** sayfa şunlardır:

* Depolama hesabı ekleme 
* Bir depolama hesabı Düzenle 
* Bir depolama hesabını silme 
* Depolama hesaplarının anahtar döndürme 

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri
Üç tür StorSimple cihazınızla kullanılabilir depolama hesabı vardır.

* **Otomatik olarak oluşturulan depolama hesapları** – adı da anlaşılacağı gibi bu depolama hesap türü hizmet ilk oluşturulduğunda otomatik olarak oluşturulur. Bu depolama hesabının nasıl oluşturulacağını hakkında daha fazla bilgi için bkz: [1. adım: yeni bir hizmet Oluştur](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-deployment-walkthrough.md). 
* **Hizmet Abonelikteki depolama hesapları** – Bu, hizmetin aynı abonelik ile ilişkili Azure depolama hesaplarıdır. Hakkında nasıl bu depolama hesapları oluşturulur daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md). 
* **Hizmet aboneliği dışında depolama hesapları** – bu değil hizmetiniz ile ilişkili olan ve büyük olasılıkla hizmet oluşturulmadan önce varolan Azure depolama hesaplarıdır.

## <a name="add-a-storage-account"></a>Depolama hesabı ekleme
Benzersiz bir kolay ad ve depolama hesabı (ile belirtilen bulut hizmeti sağlayıcısı) bağlantılı erişim kimlik bilgileri sağlayarak bir depolama hesabı ekleyebilirsiniz. Ayrıca, cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için Güvenli Yuva Katmanı (SSL) modunu etkinleştirme seçeneğiniz de vardır.

Verilen bulut hizmeti sağlayıcı için birden çok hesabı oluşturabilirsiniz. Ancak, bir depolama hesabı oluşturulduktan sonra bulut hizmet sağlayıcısı değiştirilemiyor unutmayın.

Depolama hesabı kaydedilirken, hizmet, bulut hizmeti sağlayıcısı ile iletişim kurmaya çalışır. Kimlik bilgileri ve sağladığınız erişim malzemeleri şu anda doğrulanır. Yalnızca kimlik doğrulaması başarılı olursa bir depolama hesabı oluşturulur. Kimlik doğrulama başarısız olursa, ilgili hata iletisi görüntülenir.

Azure portalında oluşturulan resource Manager depolama hesapları StorSimple ile de desteklenir. Birim kapsayıcısı, yalnızca Azure Klasik Portalı'nda oluşturulan depolama hesapları oluşturmaya görüntülendiği zaman Resource Manager depolama hesaplarıyla seçimi için aşağı açılan listesinde gösterilmez. Resource Manager depolama hesaplarıyla, aşağıda açıklandığı bir depolama hesabı eklemek için yordamı kullanarak eklenmesi gerekir.

> [!NOTE]
> Bir depolama hesabı ekleme yordamı kullanmakta olduğunuz StorSimple yazılım sürümü göre farklılık gösterir. StorSimple sürümünüz için doğru yordamı takip ettiğinizden emin olun.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Bir depolama hesabı Düzenle
Bir birim kapsayıcısı tarafından kullanılan bir depolama hesabı düzenleyebilirsiniz. Şu anda kullanımda olduğundan bir depolama hesabı düzenlerseniz, yalnızca değiştirmek kullanılabilir depolama hesabı için erişim anahtarı alanıdır. Yeni depolama erişim tuşunu sağlayın ve güncelleştirilmiş ayarları kaydedin.

#### <a name="to-edit-a-storage-account"></a>Bir depolama hesabı düzenlemek için
1. Hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma**.
2. Tıklatın **depolama hesapları Ekle/Düzenle**.
3. İçinde **Ekle/Düzenle depolama hesapları** iletişim kutusunda:
   
   1. Aşağı açılan listesinde **depolama hesapları**, değiştirmek istediğiniz var olan bir hesabı seçin. Bu hizmetin ilk oluşturulduğunda otomatik olarak oluşturulan depolama hesapları de içerebilir.
   2. Gerekirse, değiştirebileceğiniz varsa **SSL modunu etkinleştir** seçim.
   3. Depolama hesap erişim tuşlarınızı döndürmek seçebilirsiniz. Bkz: [anahtar depolama hesapları dönüşünü](#key-rotation-of-storage-accounts) anahtar döndürme gerçekleştirme hakkında daha fazla bilgi için.
   4. Onay simgesine ![onay simgesi](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) ayarları kaydetmek için. Ayarlar üzerinde güncelleştirildi **yapılandırma** sayfası. Tıklatın **kaydetmek** yeni güncelleştirilmiş ayarları kaydetmek için.
      
      ![Bir depolama hesabı Düzenle](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme
> [!IMPORTANT]
> Yalnızca bir birim kapsayıcısı tarafından kullanılmıyorsa, bir depolama hesabı silebilirsiniz. Bir depolama hesabı bir birim kapsayıcısı tarafından kullanılıyorsa, birim kapsayıcısı silmeniz ve ilişkili depolama hesabı silin.
> 
> 

#### <a name="to-delete-a-storage-account"></a>Bir depolama hesabını silmek için
1. StorSimple Yöneticisi hizmet giriş sayfasında hizmetinizi seçin, hizmet adına çift tıklayın ve ardından **yapılandırma**.
2. Depolama hesapları Tablo listesinde, silmek istediğiniz hesap gelin.
3. Sil simgesini (**x**) bu depolama hesabı için aşırı sağ sütunda görüntülenir. Tıklatın **x** kimlik bilgilerini silmek için simge.
4. Onayınız istendiğinde tıklatın **Evet** silme işlemine devam etmek için. Sekmeli liste değişiklikleri yansıtacak şekilde güncelleştirilir.

## <a name="key-rotation-of-storage-accounts"></a>Depolama hesaplarının anahtar döndürme
Güvenlik nedenleriyle, anahtar döndürme genellikle veri merkezlerinde gerekli değildir. 

> [!NOTE]
> Aşağıdaki anahtar döndürme bilgileri ve döndürme prosedürünün yalnızca Microsoft Azure depolama hesapları için geçerlidir. Başka bir bulut hizmet sağlayıcısı kullanıyorsanız, depolama hesabı anahtarlarını sağlayıcının Pano üzerinden yönetebilirsiniz.
> 
> 

Her bir Microsoft Azure aboneliği bir veya daha fazla ilişkili depolama hesapları olabilir. Bu hesaplara her depolama hesabı için abonelik ve erişim anahtarları tarafından denetlenir. 

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure depolama hesabı erişildiğinde, kimlik doğrulaması için kullanılan iki 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sahip depolama hizmetiniz için kesinti olmadan veya bu hizmete erişim anahtarlarını yeniden olanak sağlar. Şu anda kullanımda anahtar *birincil* yedekleme anahtarı olarak adlandırılır *ikincil* anahtarı. Microsoft Azure StorSimple Cihazınızı bulut depolama hizmeti sağlayıcısı eriştiğinde, bu iki anahtarlarından birini sağlanmalıdır.

## <a name="what-is-key-rotation"></a>Anahtar döndürme nedir?
Genellikle, uygulamaları yalnızca birini anahtarları verilerinize erişmek için kullanın. Belirli bir süre süre sonra ikinci anahtar kullanmaya geçiş uygulamalarınız olabilir. Uygulamalarınıza ikincil anahtarı geçirildikten sonra ilk anahtar devre dışı bırakmak ve ardından yeni bir anahtar oluşturun. Bu şekilde iki tuşlarını kullanarak, uygulamaları erişiminizi veri yansıtılmasını kapalı kalma süresi olmadan sağlar.

Depolama hesabı anahtarlarını her zaman hizmeti şifrelenmiş biçimde depolanır. Ancak, bunlar StorSimple Yöneticisi hizmeti ile sıfırlanabilir. Hizmet birincil anahtar ve tüm depolama hesapları için ikincil anahtar StorSimple Yöneticisi hizmeti ilk kez yüklendiğinde oluşturulan varsayılan depolama hesaplarının yanı sıra depolama hizmetinde oluşturulan hesapların dahil olmak üzere aynı abonelikte alabilirsiniz oluşturulur. StorSimple Yöneticisi hizmeti her zaman bu anahtarları Klasik Azure portalından almak ve bunları bir şifrelenmiş biçimde depolamak.

## <a name="rotation-workflow"></a>Döndürme iş akışı
Microsoft Azure yönetici yeniden oluşturmak veya doğrudan depolama hesabı (aracılığıyla Microsoft Azure depolama hizmeti) erişerek birincil veya ikincil anahtarı değiştirin. StorSimple Yöneticisi hizmeti bu değişikliği otomatik olarak görmez.

Değişikliği StorSimple Yöneticisi hizmetine bildirmek için StorSimple Yöneticisi hizmetine erişim için depolama hesabına erişim ve (hangisinin bağlı olarak değiştirildi) birincil veya ikincil anahtarı Eşitle. Hizmet sonra en son anahtarın alır, anahtarları şifreler ve şifrelenmiş anahtar cihaza gönderir.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>(Yalnızca Azure) hizmet ile aynı Abonelikteki depolama hesapları için anahtarları eşitlemek için
1. Üzerinde **Hizmetleri** sayfasında, **yapılandırma** sekmesi.
2. Tıklatın **depolama hesapları Ekle/Düzenle**.
3. İletişim kutusunda aşağıdakileri yapın:
   
   1. Depolama hesabı ile eşitlemek için istediğiniz anahtarı seçin. Depolama hesabı anahtarları, bunlar görüntülendiğinde şifrelenir.
   2. StorSimple Yöneticisi hizmeti daha önce Microsoft Azure depolama hizmetinde değiştirildi anahtarı güncelleştirmeniz gerekir. Birincil erişim anahtarını (oluşturuldu) değiştirilmişse, tıklatın **birincil anahtarı Eşitle**. İkincil anahtar değiştirdiyseniz tıklatın **ikincil anahtarı Eşitle**.
      
      ![anahtarları Eşitle](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Hizmet aboneliği dışında depolama hesapları için anahtarları eşitlemek için
1. Üzerinde **Hizmetleri** sayfasında, **yapılandırma** sekmesi.
2. Tıklatın **depolama hesapları Ekle/Düzenle**.
3. İletişim kutusunda aşağıdakileri yapın:
   
   1. Güncelleştirmek istediğiniz erişim anahtarıyla depolama hesabını seçin.
   2. StorSimple Yöneticisi hizmeti depolama erişim tuşu güncelleştirmeniz gerekir. Bu durumda, depolama erişim tuşu görebilirsiniz. Yeni anahtarı girin **depolama hesabının erişim anahtarı**y kutusu. 
   3. Yaptığınız değişiklikleri kaydedin. Depolama hesabı erişim anahtarınızı şimdi güncelleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-security.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

