---
title: "StorSimple Yöneticisi Hizmeti'ni dağıtma | Microsoft Docs"
description: "Oluşturma ve Azure Klasik portalında StorSimple Yöneticisi hizmetini silmek açıklar ve hizmet kayıt anahtarını yönetme açıklar."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee22a31e9c0ec23d9b042dc894cafe0fc346e742
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="deploy-the-storsimple-manager-service-in-the-azure-classic-portal"></a>Klasik Azure portalındaki StorSimple Yöneticisi hizmetini dağıtma
> [!NOTE]
> StorSimple için Klasik portalı kullanım dışıdır. StorSimple cihaz yöneticileri yeni Azure portalına kullanımdan zamanlamaya göre otomatik olarak taşır. Bir e-posta ve bu taşıma için portal bir bildirim alırsınız. Bu belgede ayrıca yakında kullanımdan kaldırılacaktır. Bu makalede yeni Azure portalına için sürümünü görüntülemek için şu adrese gidin [StorSimple Yöneticisi hizmeti klasik Azure portalındaki dağıtmak](storsimple-8000-manage-service.md). Taşıma hakkında herhangi bir sorunuz için bkz: [SSS: Azure portalına taşıma](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi hizmeti Microsoft Azure üzerinde çalışır ve birden çok StorSimple cihazını bağlanır. Hizmeti oluşturduktan sonra tarayıcıda çalışan Microsoft Azure Klasik Portalı'ndan cihazları yönetmek için kullanabilirsiniz. Böylece yönetim yükünü en aza bir tek, merkezi konumdan, StorSimple Yöneticisi hizmetine bağlanan tüm cihazlar izlemenize olanak tanır.

StorSimple Yöneticisi giriş sayfası, StorSimple depolama cihazlarını yönetmek için kullanabileceğiniz tüm StorSimple Yöneticisi hizmetleri listeler. Her StorSimple Yöneticisi hizmeti için StorSimple Yöneticisi sayfasında aşağıdaki bilgileri sunulur:

* **Ad** – oluşturulduğunda, StorSimple Yöneticisi hizmetinize atanmış adı. **Hizmet adı, hizmet oluşturulduktan sonra değiştirilemez. Bu, aynı zamanda cihazları, birimler, birim kapsayıcıları ve klasik Azure portalında yeniden adlandırılamaz yedekleme ilkeleri gibi diğer varlıklar için de geçerlidir.**
* **Durum** – olabilir hizmetinin durumunu **etkin**, **oluşturma**, veya **çevrimiçi**.
* **Konum** –, StorSimple cihazı dağıtılacak coğrafi konum.
* **Abonelik** – hizmetiniz ile ilişkili fatura abonelik.

StorSimple Yöneticisi sayfadan gerçekleştirilen ortak görevleri şunlardır:

* Hizmet oluşturma
* Bir hizmeti silin
* Hizmet kayıt anahtarı alma
* Hizmet kayıt anahtarını yeniden oluşturma

Bu öğretici, bu görevleri gerçekleştirmek açıklar.

## <a name="create-a-service"></a>Hizmet oluşturma
Kullanım **hızlı Oluştur** StorSimple Cihazınızı dağıtmak istiyorsanız, StorSimple Yöneticisi hizmeti oluşturmak için seçeneği. Bir hizmet oluşturmak için sahip olmanız gerekir:

* Bir aboneliği bir kurumsal anlaşma ile
* Etkin bir Microsoft Azure depolama hesabı
* Erişim yönetimi için kullanılan faturalama bilgileri

Hizmet oluşturduğunuzda, varsayılan depolama hesabı oluşturmak seçebilirsiniz.

Tek bir hizmet birden çok cihazları yönetebilirsiniz. Ancak, bir aygıt birden fazla hizmet yayılamaz. Büyük bir kuruluş farklı Aboneliklerde, kuruluşlar veya bile Dağıtım konumları ile çalışmak için birden fazla hizmet örneği olabilir. StorSimple 8000 serisi cihazlar ve StorSimple sanal dizileri yönetmek için StorSimple Yöneticisi hizmeti örneğini ayırmak unutmayın.

> [!IMPORTANT] 
> Ağustos 2016 önce oluşturulan kullanılmayan hizmete (aygıt gerçekleştirilen işlemler bu kaynakta) varsa, Azure portalından veya Klasik Azure portalı yönetilemez. Azure portalında yeni bir hizmet oluşturmanızı öneririz.

Bir hizmet oluşturmak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Bir hizmeti silin
Bir hizmet silmeden önce hiçbir bağlı aygıtlar, kullandığınızdan emin olun. Hizmet kullanımdaysa bağlı aygıtları devre dışı bırakın. Devre dışı bırakma işlemi cihaz ile hizmet arasında bağlantı sever ancak bulutta cihaz verileri koruma.

> [!IMPORTANT] 
> Hizmet silindikten sonra işlemi geri alınamaz. Hizmet tarafından kullanılan herhangi bir aygıtı Fabrika başka bir hizmetle kullanılabilmesi için önce olması gerekir. Bu senaryoda, cihaz olarak yapılandırma üzerinde yerel veriler kaybolacak.

Bir hizmeti silmek için aşağıdaki adımları gerçekleştirin.

### <a name="to-delete-a-service"></a>Bir hizmeti silmek için
1. Üzerinde **StorSimple Yöneticisi hizmeti** sayfasında, silmek istediğiniz hizmeti seçin.
2. Tıklatın **silmek** sayfanın sonundaki.
3. Tıklatın **Evet** onay bildirim. Hizmetin silinmesi birkaç dakika sürebilir.

## <a name="get-the-service-registration-key"></a>Hizmet kayıt anahtarı alma
Bir hizmeti başarıyla oluşturduktan sonra StorSimple Cihazınızı hizmete kaydolmak gerekir. İlk StorSimple Cihazınızı kaydetmek için hizmet kayıt anahtarı gerekir. Mevcut bir StorSimple hizmetiyle ek cihazlar kaydetmek için kayıt anahtarını ve (ilk cihazda kayıt sırasında oluşturulan) hizmeti veri şifreleme anahtarı gerekir. Hizmet verileri şifreleme anahtarı hakkında daha fazla bilgi için bkz: [StorSimple güvenlik](storsimple-security.md). Kayıt anahtarını erişerek alabileceğiniz **kayıt anahtarı** üzerinde **Hizmetleri** sayfası.

Hizmet kayıt anahtarını almak için aşağıdaki adımları gerçekleştirin.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Hizmet kayıt anahtarını güvenli bir yerde tutun. Bu hizmetiyle ek cihazlar kaydetmek için hizmet verileri şifreleme anahtarı, yanı sıra, bu anahtar gerekir. Hizmet kayıt anahtarı aldıktan sonra Windows PowerShell aracılığıyla cihazınızın StorSimple arabirimi için yapılandırmanız gerekir.

Bu kayıt anahtarı kullanma hakkında daha fazla bilgi için bkz: [adım 3: yapılandırmak ve storsimple için Windows PowerShell üzerinden cihazı](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturma
Hizmet kayıt anahtarını anahtar döndürmeyi gerçekleştirmek için gerekli olduğunu veya hizmet yöneticilerinin listesini değiştirilmişse yeniden oluşturulması gerekir. Anahtarı yeniden oluşturmak, yeni anahtar yalnızca sonraki cihazları kaydetmek için kullanılır. Bu işlem tarafından zaten kayıtlı olan cihazları etkilenmez.

Hizmet kayıt anahtarını yeniden oluşturmak için aşağıdaki adımları gerçekleştirin.

### <a name="to-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak için
1. Üzerinde **StorSimple Yöneticisi hizmeti** sayfasında, **kayıt anahtarı**.
2. İçinde **hizmet kayıt anahtarını** iletişim kutusu, tıklatın **yeniden**.
3. Bir onay iletisi görürsünüz. Tıklatın **Tamam** yeniden üretme işlemi ile devam etmek için.
4. Yeni bir hizmet kayıt anahtarı görüntülenir.
5. Bu anahtarı kopyalayın ve bu hizmeti ile yeni aygıtları kaydetme için kaydedin.
6. Onay simgesine tıklayarak ![Onay simgesi](./media/storsimple-manage-service/HCS_CheckIcon.png) Bu iletişim kutusunu kapatmak için.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [StorSimple dağıtım işlemi](storsimple-deployment-walkthrough-u2.md).
* Daha fazla bilgi edinmek [StorSimple depolama hesabınızı yönetme](storsimple-manage-storage-accounts.md).
* Nasıl yapılır hakkında daha fazla bilgi [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).
