---
title: Microsoft Azure StorSimple 8000 serisi cihazlar için StorSimple depolama hesabı kimlik bilgilerinizi yönetme | Microsoft Docs
description: Nasıl StorSimple Aygıt Yöneticisi'ni yapılandırma sayfasında kullanabilir eklemek için Düzenle, Sil, veya bir depolama hesabı için güvenlik anahtarları döndürme açıklanmaktadır.
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
ms.openlocfilehash: 36058ad69ea670998b50cf9038741c294a5b79ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23875133"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış

**Yapılandırma** StorSimple cihaz Yöneticisi hizmeti dikey bölümde StorSimple Aygıt Yöneticisi'ni hizmetinde oluşturulan tüm genel hizmet parametreleri gösterir. Bu parametreler hizmete bağlı tüm aygıtlara uygulanabilir ve şunları içerir:

* Depolama hesabı kimlik bilgileri
* Bant genişliği şablonları 
* Erişim denetimi kayıtları 

Bu öğretici, eklemek, düzenlemek veya depolama hesabının kimlik bilgilerini silmek veya bir depolama hesabının güvenlik anahtarlarını döndürmek açıklanmaktadır.

 ![Depolama hesabı kimlik bilgileri listesi](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Depolama hesapları StorSimple cihazı bulut hizmeti sağlayıcısı ile depolama hesabınıza erişmek için kullandığı kimlik bilgilerini içerir. Microsoft Azure depolama hesapları için bu hesap adı ve birincil erişim anahtarını gibi kimlik bilgileridir. 

Üzerinde **depolama hesabının kimlik bilgilerini** dikey penceresinde, fatura abonelik görüntülenir için aşağıdaki bilgileri içeren bir tablo biçiminde oluşturulan tüm depolama hesapları:

* **Ad** – oluşturulduğunda bu hesaba atanan benzersiz bir ad.
* **SSL özellikli** – olup olmadığını SSL etkinleştirilmişse ve cihaz bulut iletişimidir güvenli kanal üzerinden.
* **Tarafından kullanılan** – depolama hesabı kullanarak birimlerin sayısı.

Gerçekleştirilebilir depolama hesaplarına ilgili yaygın görevleri şunlardır:

* Depolama hesabı ekleme 
* Bir depolama hesabı Düzenle 
* Bir depolama hesabını silme 
* Depolama hesaplarının anahtar döndürme 

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

Üç tür StorSimple cihazınızla kullanılabilir depolama hesabı vardır.

* **Otomatik olarak oluşturulan depolama hesapları** – adı da anlaşılacağı gibi bu depolama hesap türü hizmet ilk oluşturulduğunda otomatik olarak oluşturulur. Bu depolama hesabının nasıl oluşturulacağını hakkında daha fazla bilgi için bkz: [1. adım: yeni bir hizmet Oluştur](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) içinde [şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md). 
* **Hizmet Abonelikteki depolama hesapları** – Bu, hizmetin aynı abonelik ile ilişkili Azure depolama hesaplarıdır. Hakkında nasıl bu depolama hesapları oluşturulur daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../storage/common/storage-create-storage-account.md). 
* **Hizmet aboneliği dışında depolama hesapları** – bu değil hizmetiniz ile ilişkili olan ve büyük olasılıkla hizmet oluşturulmadan önce varolan Azure depolama hesaplarıdır.

## <a name="add-a-storage-account"></a>Depolama hesabı ekleme

Benzersiz bir kolay ad ve depolama hesabı (ile belirtilen bulut hizmeti sağlayıcısı) bağlantılı erişim kimlik bilgileri sağlayarak bir depolama hesabı ekleyebilirsiniz. Ayrıca, cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için Güvenli Yuva Katmanı (SSL) modunu etkinleştirme seçeneğiniz de vardır.

Verilen bulut hizmeti sağlayıcı için birden çok hesabı oluşturabilirsiniz. Ancak, bir depolama hesabı oluşturulduktan sonra bulut hizmet sağlayıcısı değiştirilemiyor unutmayın.

Depolama hesabı kaydedilirken, hizmet, bulut hizmeti sağlayıcısı ile iletişim kurmaya çalışır. Kimlik bilgileri ve sağladığınız erişim malzemeleri şu anda doğrulanır. Yalnızca kimlik doğrulaması başarılı olursa bir depolama hesabı oluşturulur. Kimlik doğrulama başarısız olursa, ilgili hata iletisi görüntülenir.

Azure depolama hesabının kimlik bilgilerini eklemek için aşağıdaki yordamları kullanın:

* Aygıt Yöneticisi hizmeti aynı Azure abonelik sahip bir depolama hesabı kimlik bilgilerini eklemek için
* Aygıt Yöneticisi hizmeti abonelik dışında bir Azure depolama hesabı kimlik bilgilerini eklemek için

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="to-add-an-azure-storage-account-credential-outside-of-the-storsimple-device-manager-service-subscription"></a>StorSimple cihaz Yöneticisi hizmet aboneliği dışında bir Azure depolama hesabı kimlik bilgilerini eklemek için

1. StorSimple cihaz Yöneticisi hizmetinize select gidin ve çift tıklatın. Bu açılır **genel bakış** dikey.
2. Seçin **depolama hesabının kimlik bilgilerini** içinde **yapılandırma** bölümü. StorSimple cihaz Yöneticisi hizmetiyle ilişkili tüm var olan depolama hesabının kimlik bilgilerini listeler.
3. **Ekle**'ye tıklayın.
4. İçinde **bir depolama hesabı kimlik bilgileri Ekle** dikey penceresinde aşağıdakileri yapın:
   
    1. İçin **abonelik**seçin **diğer**.
   
    2. Azure depolama hesabı kimlik bilgilerinizi adını sağlayın.
   
    3. İçinde **depolama hesabının erişim anahtarı** metin kutusunda, Azure depolama hesabı kimlik bilgileriniz için birincil erişim anahtarını girin. Bu anahtarı almak için Azure depolama hizmeti için depolama hesabı kimlik bilgilerinizi seçin ve tıklayın Git **hesabı anahtarları Yönet**. Birincil erişim anahtarını şimdi kopyalayabilirsiniz.
   
    4. SSL etkinleştirmek için **etkinleştirmek** StorSimple cihaz Yöneticisi hizmetiniz ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak için düğmesi. Tıklatın **devre dışı** yalnızca özel bir bulutta işlem yapıyorsanız düğmesine tıklayın.
   
    5. **Ekle**'ye tıklayın. Depolama hesabı kimlik bilgilerini başarıyla oluşturulduktan sonra size bildirilir.

5. Yeni oluşturulan depolama hesabı kimlik bilgileri altında StorSimple yapılandırma Aygıt Yöneticisi'ni hizmet dikey penceresinde görüntülenir **depolama hesabının kimlik bilgilerini**.
   


## <a name="edit-a-storage-account"></a>Bir depolama hesabı Düzenle

Bir birim kapsayıcısı tarafından kullanılan bir depolama hesabı düzenleyebilirsiniz. Şu anda kullanımda olduğundan bir depolama hesabı düzenlerseniz, yalnızca değiştirmek kullanılabilir depolama hesabı için erişim anahtarı alanıdır. Yeni depolama erişim tuşunu sağlayın ve güncelleştirilmiş ayarları kaydedin.

#### <a name="to-edit-a-storage-account"></a>Bir depolama hesabı düzenlemek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

    ![Depolama hesabı kimlik bilgileri](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. İçinde **depolama hesabının kimlik bilgilerini** dikey penceresinde, depolama hesabı kimlik bilgileri, listeden seçin ve düzenlemek istediğiniz öğeyi tıklatın. 

3. Değiştirebileceğiniz **SSL'yi etkinleştir** seçim. Tıklatarak **daha...**  ve ardından **döndürmek için eşitleme erişim anahtarı** depolama hesap erişim tuşlarınızı. Git [anahtar depolama hesapları dönüşünü](#key-rotation-of-storage-accounts) anahtar döndürme gerçekleştirme hakkında daha fazla bilgi için. Ayarları değiştirdikten sonra tıklatın **kaydetmek**. 

    ![Düzenlenen depolama hesabının kimlik bilgilerini Kaydet](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Onayınız istendiğinde **Evet**’e tıklayın. 

    ![Değişiklikleri onaylayın](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

Ayarları güncelleştirildi ve depolama hesabınız için kaydedilir. 

## <a name="delete-a-storage-account"></a>Bir depolama hesabını silme

> [!IMPORTANT]
> Yalnızca bir birim kapsayıcısı tarafından kullanılmıyorsa, bir depolama hesabı silebilirsiniz. Bir depolama hesabı bir birim kapsayıcısı tarafından kullanılıyorsa, birim kapsayıcısı silmeniz ve ilişkili depolama hesabı silin.

#### <a name="to-delete-a-storage-account"></a>Bir depolama hesabını silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.

2. Depolama hesapları Tablo listesinde, silmek istediğiniz hesap gelin. Sağ tıklatıp bağlam menüsü çağırma **silmek**.

    ![Depolama hesabının kimlik bilgilerini sil](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Onayınız istendiğinde tıklatın **Evet** silme işlemine devam etmek için. Sekmeli liste değişiklikleri yansıtacak şekilde güncelleştirilir.

    ![Silmeyi onayla](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Depolama hesaplarının anahtar döndürme

Güvenlik nedenleriyle, anahtar döndürme genellikle veri merkezlerinde gerekli değildir. Her bir Microsoft Azure aboneliği bir veya daha fazla ilişkili depolama hesapları olabilir. Bu hesaplara her depolama hesabı için abonelik ve erişim anahtarları tarafından denetlenir. 

Bir depolama hesabı oluşturduğunuzda, Microsoft Azure depolama hesabı erişildiğinde, kimlik doğrulaması için kullanılan iki 512 bit depolama erişim tuşu oluşturur. İki depolama erişim tuşu sahip depolama hizmetiniz için kesinti olmadan veya bu hizmete erişim anahtarlarını yeniden olanak sağlar. Şu anda kullanımda anahtar *birincil* yedekleme anahtarı olarak adlandırılır *ikincil* anahtarı. Microsoft Azure StorSimple Cihazınızı bulut depolama hizmeti sağlayıcısı eriştiğinde, bu iki anahtarlarından birini sağlanmalıdır.

## <a name="what-is-key-rotation"></a>Anahtar döndürme nedir?

Genellikle, uygulamaları yalnızca birini anahtarları verilerinize erişmek için kullanın. Belirli bir süre süre sonra ikinci anahtar kullanmaya geçiş uygulamalarınız olabilir. Uygulamalarınıza ikincil anahtarı geçirildikten sonra ilk anahtar devre dışı bırakmak ve ardından yeni bir anahtar oluşturun. Bu şekilde iki tuşlarını kullanarak, uygulamaları erişiminizi veri yansıtılmasını kapalı kalma süresi olmadan sağlar.

Depolama hesabı anahtarlarını her zaman hizmeti şifrelenmiş biçimde depolanır. Ancak, bunlar StorSimple cihaz Yöneticisi hizmeti ile sıfırlanabilir. Hizmet, birincil anahtar ve tüm depolama hesapları için ikincil anahtar StorSimple cihaz Yöneticisi hizmeti ilk kez yüklendiğinde oluşturulan hesaplarına oluşturulan varsayılan depolama yanı sıra depolama hizmeti oluşturulan hesapların dahil olmak üzere aynı abonelikte alabilirsiniz. StorSimple cihaz Yöneticisi hizmeti her zaman bu anahtarları Klasik Azure portalından almak ve bunları bir şifrelenmiş biçimde depolamak.

## <a name="rotation-workflow"></a>Döndürme iş akışı

Microsoft Azure yönetici yeniden oluşturmak veya doğrudan depolama hesabı (aracılığıyla Microsoft Azure depolama hizmeti) erişerek birincil veya ikincil anahtarı değiştirin. StorSimple cihaz Yöneticisi hizmeti bu değişikliği otomatik olarak görmez.

Değişikliği StorSimple cihaz Yöneticisi hizmetine bildirmek için StorSimple cihaz Yöneticisi hizmetine erişmek için depolama hesabına erişim ve (hangisinin bağlı olarak değiştirildi) birincil veya ikincil anahtarı Eşitle. Hizmet sonra en son anahtarın alır, anahtarları şifreler ve şifrelenmiş anahtar cihaza gönderir.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service"></a>Hizmet olarak aynı Abonelikteki depolama hesapları için anahtarları eşitlemek için 
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Yapılandırma** bölümünde **Depolama hesabı kimlik bilgileri**’ne tıklayın.
2. Gelen tablo depolama hesaplarının listesi, değiştirmek istediğiniz birini tıklatın. 

    ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Tıklatın **... Daha fazla** ve ardından **döndürmek için eşitleme erişim anahtarı**.   

    ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. StorSimple cihaz Yöneticisi hizmeti daha önce Microsoft Azure depolama hizmetinde değiştirildi anahtarı güncelleştirmeniz gerekir. Birincil erişim anahtarını (oluşturuldu) değiştirilmişse, seçin **birincil** anahtarı. İkincil anahtar değiştirdiyseniz seçin **ikincil** anahtarı. Tıklatın **eşitleme anahtarı**.
      
      ![anahtarları Eşitle](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Anahtar sycnhronized başarıyla tamamlandıktan sonra size bildirilecek.

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Hizmet aboneliği dışında depolama hesapları için anahtarları eşitlemek için
1. Üzerinde **Hizmetleri** sayfasında, **yapılandırma** sekmesi.
2. Tıklatın **depolama hesapları Ekle/Düzenle**.
3. İletişim kutusunda aşağıdakileri yapın:
   
   1. Güncelleştirmek istediğiniz erişim anahtarıyla depolama hesabını seçin.
   2. StorSimple cihaz Yöneticisi hizmeti depolama erişim tuşu güncelleştirmeniz gerekir. Bu durumda, depolama erişim tuşu görebilirsiniz. Yeni anahtarı girin **depolama hesabının erişim anahtarı** kutusu. 
   3. Yaptığınız değişiklikleri kaydedin. Depolama hesabı erişim anahtarınızı şimdi güncelleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple güvenlik](storsimple-8000-security.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

