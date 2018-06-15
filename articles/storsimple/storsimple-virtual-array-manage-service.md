---
title: StorSimple cihaz Yöneticisi Hizmeti'ni dağıtma | Microsoft Docs
description: Oluşturma ve Azure portalında StorSimple cihaz Yöneticisi hizmetini silmek açıklar ve hizmet kayıt anahtarını yönetme açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 28499494-8c4d-4a7f-9d44-94b2b8a93c93
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: alkohli
ms.openlocfilehash: 1881a0625b107ae1a90e5b772f5296a4d728973d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876106"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a>StorSimple sanal dizinin StorSimple cihaz Yöneticisi hizmetini dağıtma
## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Hizmeti oluşturduktan sonra tarayıcıda çalışan Microsoft Azure portalından cihazları yönetmek için kullanabilirsiniz. StorSimple cihaz Yöneticisi hizmetine böylece yönetim yükünü en aza bir tek, merkezi konumdan, bağlı olan tüm aygıtları izlemenize olanak tanır.

StorSimple cihaz Yöneticisi hizmeti ile ilgili ortak görevleri şunlardır:

* Hizmet oluşturma
* Bir hizmeti silin
* Hizmet kayıt anahtarı alma
* Hizmet kayıt anahtarını yeniden oluşturma

Bu öğretici, her bir önceki görev gerçekleştirmek açıklar. Bu makalede yer alan bilgileri yalnızca StorSimple sanal diziler için geçerlidir. StorSimple 8000 serisi hakkında daha fazla bilgi için Git [StorSimple Yöneticisi hizmet dağıtma](storsimple-manage-service.md).

## <a name="create-a-service"></a>Hizmet oluşturma

Bir hizmet oluşturmak için sahip olmanız gerekir:

* Bir aboneliği bir kurumsal anlaşma ile
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan faturalama bilgileri

Hizmet oluşturduğunuzda, bir depolama hesabı oluşturmak seçebilirsiniz.

Tek bir hizmet birden çok cihazları yönetebilirsiniz. Ancak, bir aygıt birden fazla hizmet yayılamaz. Büyük bir kuruluş farklı Aboneliklerde, kuruluşlar veya bile Dağıtım konumları ile çalışmak için birden fazla hizmet örneği olabilir.

> [!NOTE]
> StorSimple 8000 serisi cihazlar ve StorSimple sanal dizileri yönetmek için StorSimple cihaz Yöneticisi hizmeti ayrı ayrı örnekleri gerekir.


Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Bir hizmeti silin

Bir hizmet silmeden önce hiçbir bağlı aygıtlar, kullandığınızdan emin olun. Hizmet kullanımdaysa bağlı aygıtları devre dışı bırakın. Devre dışı bırakma işlemi cihaz ile hizmet arasında bağlantı sever ancak bulutta cihaz verileri koruma.

> [!IMPORTANT]
> Hizmet silindikten sonra işlemi geri alınamaz. Hizmet tarafından kullanılan herhangi bir aygıtı Fabrika başka bir hizmetle kullanılabilmesi için önce olması gerekir. Bu senaryoda, cihaz olarak yapılandırma üzerinde yerel veriler kaybolacak.
 

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. Git **tüm kaynakları**. StorSimple cihaz Yöneticisi hizmetiniz için arama yapın. Silmek istediğiniz hizmeti seçin.
   
    ![Hizmeti silmek için seçin](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Hizmete bağlı cihaz yok emin olmak için hizmet panosunu gidin. Bu hizmetle kaydedilen hiçbir cihaz varsa, ayrıca etkili olması için bir başlık iletisi görürsünüz. **Sil**'e tıklayın.
   
    ![Hizmet silme](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Onayınız istendiğinde tıklatın **Evet** onay bildirim. 
   
    ![Hizmet Silmeyi Onayla](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Hizmetin silinmesi birkaç dakika sürebilir. Hizmet başarıyla silindikten sonra size bildirilecek.
   
    ![Başarılı hizmet silme](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

Hizmetlerin listesi yenilenecektir.

 ![Güncelleştirilmiş hizmetlerin listesi](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma
Bir hizmeti başarıyla oluşturduktan sonra StorSimple Cihazınızı hizmete kaydolmak gerekir. İlk StorSimple Cihazınızı kaydetmek için hizmet kayıt anahtarı gerekir. Mevcut bir StorSimple hizmetiyle ek cihazlar kaydetmek için kayıt anahtarını ve (ilk cihazda kayıt sırasında oluşturulan) hizmeti veri şifreleme anahtarı gerekir. Hizmet verileri şifreleme anahtarı hakkında daha fazla bilgi için bkz: [StorSimple güvenlik](storsimple-security.md). Kayıt anahtarını erişerek alabileceğiniz **anahtarları** dikey hizmetiniz için.

Hizmet kayıt anahtarını almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-service-registration-key"></a>Hizmet kayıt anahtarını almak için
1. İçinde **StorSimple Aygıt Yöneticisi'ni** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. İçinde **anahtarları** hizmet kayıt anahtarını dikey penceresinde görüntülenir. Kopya simgesini kullanarak kayıt anahtarını kopyalayın. 

Hizmet kayıt anahtarını güvenli bir yerde tutun. Bu hizmetiyle ek cihazlar kaydetmek için hizmet verileri şifreleme anahtarı, yanı sıra, bu anahtar gerekir. Hizmet kayıt anahtarı aldıktan sonra Windows PowerShell aracılığıyla cihazınızın StorSimple arabirimi için yapılandırmanız gerekir.

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturma
Hizmet kayıt anahtarını anahtar döndürmeyi gerçekleştirmek için gerekli olduğunu veya hizmet yöneticilerinin listesini değiştirilmişse yeniden oluşturulması gerekir. Anahtarı yeniden oluşturmak, yeni anahtar yalnızca sonraki cihazları kaydetmek için kullanılır. Bu işlem tarafından zaten kayıtlı olan cihazları etkilenmez.

Hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak için
1. İçinde **StorSimple Aygıt Yöneticisi'ni** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. İçinde **anahtarları** dikey penceresinde tıklatın **yeniden**.
   
   ![Yeniden tıklatın](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. İçinde **üretme hizmet kayıt anahtarını** dikey penceresinde, gözden geçirme eylem gerekli olduğunda anahtarları yeniden oluşturulur. Bu hizmetle kaydedilen tüm sonraki cihazlar yeni kayıt anahtarını kullanır. Tıklatın **yeniden** onaylamak için. Kayıt tamamlandıktan sonra size bildirilecek.
   
   ![Yeniden oluşturma anahtarını doğrulayın](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Yeni bir hizmet kayıt anahtarı görüntülenir.
   
    ![Yeniden oluşturma anahtarını doğrulayın](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Bu anahtarı kopyalayın ve bu hizmeti ile yeni aygıtları kaydetme için kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [başlamak](storsimple-virtual-array-deploy1-portal-prep.md) bir StorSimple sanal diziye sahip.
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek](storsimple-ova-web-ui-admin.md).

