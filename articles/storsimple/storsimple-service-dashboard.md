---
title: "StorSimple Yöneticisi hizmet panosunu | Microsoft Docs"
description: "StorSimple Yöneticisi hizmet panosunu tanımlar ve bunu StorSimple çözümünüzün sağlığını izlemek için nasıl kullanılacağını açıklar."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: fb0f131d-d60b-45d7-ace2-56d0502e6627
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 596431b7279b753ca4da838eb028cdde2022ce02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-dashboard"></a>StorSimple Yöneticisi hizmet panosunu kullanma
## <a name="overview"></a>Genel Bakış
StorSimple Yöneticisi hizmeti Pano sayfası, sistem yöneticisinin dikkat etmeniz gereken vurgulama StorSimple Yöneticisi hizmetine bağlı olan tüm aygıtları Özet görünümünü sağlar. Bu öğretici, Pano sayfası tanıtır, Pano içeriği ve işlevi açıklanmaktadır ve bu sayfadan gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet panosu](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

StorSimple Yöneticisi hizmet Panosu aşağıdaki bilgileri görüntüler:

* İçinde **grafik** alanı aygıtlarınız için ilgili ölçümleri grafik görebilirsiniz. (Yerel olarak sabitlenmiş ve katmanlı) birincil depolama görüntüleyebileceğiniz bir süre boyunca aygıtlarının kullandığı bulut depolama yanı sıra tüm cihazlar arasında kullanılır. Denetimleri grafiğin sağ üst köşesinde 1 hafta, ay 1, 3 ay ya da 1 yıllık ölçek belirtmek için kullanın.
* **Kullanıma genel bakış** sağlandı ve kullanılabilir toplam depolama alanı göre tüm aygıtları farklı cihazlara kullandığını birincil depolamayı gösterir. **Sağlanan** hazırlanmış ve kullanım için ayrılan while depolama miktarını başvuruyor **kullanılan** birimlerin kullanımı aygıtlara bağlı başlatıcıları tarafından görüntülenen şekilde başvuruyor.
* **Uyarıları** alanı uyarı önem derecesine göre gruplandırılmış tüm aygıtlar arasında anlık görüntüsünü tüm etkin uyarıları sağlar. Önem düzeyi tıklatıldığında açılır **uyarıları** sayfasında, bu uyarıları göstermek için kapsamlı. Üzerinde **uyarıları** sayfasında, önerilen eylemleri de dahil olmak üzere, bu uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı tıklatabilirsiniz. Sorunu çözerseniz uyarı işaretini kaldırabilirsiniz.
* **İşleri** alanı hizmetinize bağlanan tüm cihazlar arasında anlık görüntüsü en son işlerin sağlar. İlerleme durumu, son 24 saat içinde başarısız olan veya sonraki 24 saat içinde çalışacak şekilde zamanlanmış bulunan işleri bakmak için kullanabileceğiniz bağlantılar bulunur.
* **Hızlı Bakış** alanı yararlı bilgiler sağlar, hizmet durumu gibi cihaz sayısını bağlı hizmeti, hizmet konumu ve hizmeti ile ilişkilendirilmiş Abonelik Ayrıntıları. İşlem günlüğü bağlantısı yoktur. Tüm tamamlanan StorSimple Yöneticisi hizmet işlemleri listesini görmek için bağlantıya tıklayın.

StorSimple Yöneticisi hizmeti Pano sayfası, aşağıdaki görevleri başlatmak için kullanabilirsiniz:

* Hizmet kayıt anahtarını yeniden oluşturmak veya görüntüleyin.
* Hizmet verileri şifreleme anahtarı değiştirin.
* İşlem günlüklerine bakın.

## <a name="view-or-regenerate-the-service-registration-key"></a>Hizmet kayıt anahtarını yeniden oluşturmak veya görüntülemek
Daha fazla yönetim işlemleri için Klasik Azure portalındaki cihaz görünmesi hizmet kayıt anahtarı StorSimple Yöneticisi hizmetiyle bir Microsoft Azure StorSimple cihazı kaydetmek için kullanılır. Anahtar ilk cihazda oluşturulur ve aygıtlarınızın geri kalanı ile paylaşılan.

Tıklatarak **kayıt anahtarı** (sayfasının en altında) açar **hizmet kayıt anahtarını** iletişim kutusu, burada geçerli hizmet kayıt anahtarını panoya kopyalayın veya hizmet kayıt anahtarını yeniden oluşturma.

Anahtar yeniden önceden kayıtlı cihazları etkilemez: anahtar yeniden oluşturulacak sonra hizmete kayıtlı aygıtları etkiler.

Görüntüleme ve hizmet kayıt anahtarı oluşturma hakkında daha fazla bilgi için Git [hizmet kayıt anahtarını alın](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-the-service-data-encryption-key"></a>Değişiklik hizmeti veri şifreleme anahtarı
Hizmet veri şifreleme anahtarları, StorSimple Yöneticisi hizmetiniz StorSimple cihaza gönderilen depolama hesabının kimlik bilgilerini gibi gizli müşteri verilerini şifrelemek için kullanılır. BT kuruluşunuzun depolama cihazlarında bir anahtar döndürme ilke varsa bu anahtarlar düzenli olarak değiştirmeniz gerekir. Anahtar değişim işlemini bağlı olarak biraz farklı olabilir bir tek veya birden çok cihazı StorSimple Yöneticisi hizmeti tarafından yönetilen yoktur.

Hizmet verileri şifreleme anahtarı değiştirme 3 adımlı bir işlemdir:

1. Klasik Azure portalını kullanarak, hizmet verileri şifreleme anahtarı değiştirmek için bir aygıt yetkilendirin.
2. StorSimple için Windows PowerShell kullanarak hizmet verileri şifreleme anahtarı değişikliği başlatır.
3. Birden çok StorSimple cihazınız varsa, diğer cihazlarda hizmet verileri şifreleme anahtarı güncelleştirin.

Aşağıdaki adımlar, hizmet verileri şifreleme anahtarı geçiş işlemini açıklamaktadır.

[!INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]

## <a name="view-the-operations-logs"></a>View operations logs
İşlem günlükleri bulunan işlem günlükleri bağlantısını tıklatarak görüntüleyebileceğiniz **Hızlı Bakış** panonun bölmesi. Bu sizi, filtre ve StorSimple Yöneticisi hizmetinize belirli günlüklerine bakın Yönetim Hizmetleri sayfasına götürür.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [StorSimple cihazını sorun giderme](storsimple-troubleshoot-operational-device.md).
* Nasıl yapılır hakkında daha fazla bilgi [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

