---
title: StorSimple cihaz Yöneticisi hizmetini dağıtma | Microsoft Docs
description: Oluşturma ve Azure portalında StorSimple cihaz Yöneticisi hizmetini silme açıklar ve hizmet kayıt anahtarını yönetme işlemi açıklanır.
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
ms.openlocfilehash: 9f6e5b606caa661429a3c4d4a53e2021d57730aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62116967"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-virtual-array"></a>StorSimple Virtual Array için StorSimple cihaz Yöneticisi hizmetini dağıtma
## <a name="overview"></a>Genel Bakış

StorSimple cihaz Yöneticisi hizmeti, Microsoft Azure'da çalışan ve birden çok StorSimple aygıtına bağlanır. Hizmeti oluşturduktan sonra bir tarayıcıda çalışan Microsoft Azure portalından cihazları yönetmek için kullanabilirsiniz. Bu, böylece yönetici yükünü en aza bir tek, merkezi konumdan, StorSimple cihaz Yöneticisi hizmetine bağlanan tüm cihazlar izlemenize olanak sağlar.

StorSimple cihaz Yöneticisi hizmeti ile ilgili genel görevleri şunlardır:

* Bir hizmet oluşturma
* Bir hizmeti Sil
* Hizmet kayıt anahtarı alma
* Hizmet kayıt anahtarını yeniden oluştur

Bu öğreticide, görevlerin her biri önceki yerine getirilmesi anlatılmaktadır. Bu makalede yer alan bilgileri yalnızca StorSimple sanal diziler için geçerlidir. StorSimple 8000 serisi hakkında daha fazla bilgi için Git [bir StorSimple Yöneticisi hizmetini dağıtma](storsimple-manage-service.md).

## <a name="create-a-service"></a>Bir hizmet oluşturma

Bir hizmet oluşturmak için sahip olmanız gerekir:

* Bir kurumsal anlaşma kapsamında olan bir abonelik
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan faturalandırma bilgileri

Hizmet oluşturduğunuzda, bir depolama hesabı oluşturmak seçebilirsiniz.

Tek bir hizmet birden çok cihazları yönetebilirsiniz. Ancak, bir cihaz birden fazla hizmet yayılamaz. Büyük bir kuruluş, farklı Aboneliklerde, kuruluşlar veya bile Dağıtım konumları ile çalışmak için birden çok hizmeti örneği olabilir.

> [!NOTE]
> StorSimple 8000 serisi cihazlar ve StorSimple sanal dizilerini yönetmek için StorSimple cihaz Yöneticisi hizmeti ayrı örneklerini ihtiyacınız vardır.


Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

## <a name="delete-a-service"></a>Bir hizmeti Sil

Bir hizmet silmeden önce bağlı cihaz yok, kullandığınızdan emin olun. Hizmet kullanılıyorsa, bağlı cihazları devre dışı bırakın. Devre dışı bırakma işlemi cihaz ile hizmet arasında bağlantı sever, ancak bulutta cihaz verilerini koruma.

> [!IMPORTANT]
> Hizmet silindikten sonra işlemi geri alınamaz. Hizmeti kullanan herhangi bir CİHAZDAN başka bir hizmetle kullanılmadan önce fabrika ayarlarına olması gerekir. Bu senaryoda, yapılandırmanın yanı sıra, cihaz üzerinde yerel veriler kaybolacak.
 

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-service"></a>Bir hizmeti silmek için

1. **Tüm kaynaklar**'a gidin. StorSimple cihaz Yöneticisi hizmetiniz için arama yapın. Silmek istediğiniz hizmeti seçin.
   
    ![Hizmeti silmek için seçin](./media/storsimple-virtual-array-manage-service/deleteservice2.png)
2. Hizmete bağlı cihaz olmadığında olmadığından emin olmak için hizmeti panonuza gidin. Bu hizmete kayıtlı cihaz yok ise, etkili olması için bir başlık iletisi görürsünüz. Tıklayın **Sil**.
   
    ![Hizmeti Sil](./media/storsimple-virtual-array-manage-service/deleteservice3.png)

3. Onayınız istendiğinde tıklayın **Evet** onay bildirimi içinde. 
   
    ![Hizmet silme işlemini onaylayın](./media/storsimple-virtual-array-manage-service/deleteservice4.png)
4. Bu hizmetin silinmesi birkaç dakika sürebilir. Hizmet başarıyla silindikten sonra size bildirilir.
   
    ![Başarılı hizmet silme](./media/storsimple-virtual-array-manage-service/deleteservice6.png)

Hizmetler listesinden yenilenir.

 ![Güncelleştirilmiş hizmetlerin listesi](./media/storsimple-virtual-array-manage-service/deleteservice7.png)

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma
Bir hizmeti başarıyla oluşturduktan sonra StorSimple Cihazınızı hizmetine kaydetmeniz gerekir. İlk StorSimple Cihazınızı kaydetmek için hizmet kayıt anahtarı gerekir. Mevcut bir StorSimple hizmetiyle ek cihazlar kaydetmek için kayıt anahtarını hem de (Bu cihaz üzerinde ilk kayıt sırasında oluşturulan) hizmet veri şifreleme anahtarı gerekir. Hizmet veri şifreleme anahtarı hakkında daha fazla bilgi için bkz: [StorSimple güvenlik](storsimple-security.md). Kayıt anahtarını erişerek alabileceğiniz **anahtarları** hizmetinizin dikey.

Hizmet kayıt anahtarı almak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-get-the-service-registration-key"></a>Hizmet kayıt anahtarı almak için
1. İçinde **StorSimple cihaz Yöneticisi** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. İçinde **anahtarları** hizmet kayıt anahtarı dikey penceresinde görünür. Kopyala simgesini kullanarak kayıt anahtarını kopyalayın. 

Hizmet kayıt anahtarı güvenli bir yerde saklayın. Bu hizmetiyle ek cihazlar kaydetmek için hizmet veri şifreleme anahtarı, yanı sıra, bu anahtar gerekir. Hizmet kayıt anahtarını aldıktan sonra StorSimple arabirimi için Windows PowerShell aracılığıyla cihazınızın yapılandırmanız gerekecektir.

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluştur
Anahtar döndürme gerçekleştirmek için gerekli olduğunda veya hizmet yöneticilerinin listesini değişmişse bir hizmet kayıt anahtarını yeniden oluşturmak gerekir. Anahtarı yeniden ürettiğinizde, yeni anahtar yalnızca sonraki cihazları kaydetmek için kullanılır. Bu işlem tarafından zaten kayıtlı cihazlar etkilenmez.

Bir hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak için
1. İçinde **StorSimple cihaz Yöneticisi** dikey penceresinde, Git **Yönetim &gt;**  **anahtarları**.
   
   ![Anahtarlar dikey penceresi](./media/storsimple-virtual-array-manage-service/getregkey2.png)
2. İçinde **anahtarları** dikey penceresinde tıklayın **yeniden**.
   
   ![Regenerate tıklayın](./media/storsimple-virtual-array-manage-service/getregkey5.png)
3. İçinde **Regenerate hizmet kayıt anahtarını** dikey penceresinde, gözden geçirme eylem gerekli olduğunda anahtarları yeniden oluşturulur. Bu hizmete kayıtlı olan tüm sonraki cihazlara, yeni bir kayıt anahtarı kullanır. Tıklayın **yeniden** onaylamak için. Kayıt tamamlandığında size bildirilir.
   
   ![Yeniden oluşturma anahtarını doğrulayın](./media/storsimple-virtual-array-manage-service/getregkey3.png)
4. Yeni bir hizmet kayıt anahtarı görüntülenir.
   
    ![Yeniden oluşturma anahtarını doğrulayın](./media/storsimple-virtual-array-manage-service/getregkey4.png)
   
   Bu anahtarı kopyalayın ve bu hizmetle yeni cihazları kaydetmek için kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [başlama](storsimple-virtual-array-deploy1-portal-prep.md) StorSimple Virtual Array ile.
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek](storsimple-ova-web-ui-admin.md).

