---
title: Avere vFXT - Azure desteğini etkinleştirme
description: Desteğini nasıl etkinleştireceğinizi Avere vFXT ' Azure'a yükler.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: fe096b2e2a75cc89e3ce5ef905d8e4c347cc153a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409859"
---
# <a name="enable-support-uploads"></a>Destek karşıya yüklemelerini etkinleştirme

Azure için Avere vFXT destek veri kümeniz hakkında otomatik olarak karşıya yükleyebilirsiniz. En iyi olası müşteri hizmetleri personeli destek sağlar. Bu yüklemeler sağlar.

## <a name="steps-to-enable-uploads"></a>Karşıya yükleme etkinleştirme adımları

Avere desteğini etkinleştirmek için Denetim Masası ' aşağıdaki adımları izleyin. (Okuma [vFXT kümeye erişmek](avere-vfxt-cluster-gui.md) Avere denetim masasını açmak hakkında bilgi edinmek için.)

1. Gidin **ayarları** en üstteki sekmedeki.
1. Tıklayın **Destek** bağlantı sol tarafta ve gizlilik ilkesini kabul edin.

   ![Gizlilik ilkesini kabul etmek için Avere Denetim Masası ve Onayla düğmesi açılır pencereyi gösteren ekran görüntüsü](media/avere-vfxt-privacy-policy.png)

1. Sol tarafındaki üçgeni **müşteri bilgileri** bölümü genişletin.
1. Tıklayın **Revalidate karşıya yükleme bilgileri** düğmesi.
1. Kümenin destek adı kümesinde **benzersiz küme adı** -benzersiz olarak tanımladığı personel desteklemek için kümenizin emin olun.
1. İçin onay kutularını işaretleyin **istatistikleri izleme**, **genel bilgilerini karşıya**, ve **kilitlenme bilgi karşıya**.
1. **Gönder**'e tıklayın.

   ![Müşteri bilgileri bölümü destek ayarları sayfasının ekran görüntüsü içeren tamamlandı](media/avere-vfxt-support-info.png)

1. Sol tarafındaki üçgeni **güvenli proaktif destek (SPS)** bölümü genişletin.
1. İçin kutuyu **etkinleştir SPS bağlantısını**.
1. **Gönder**'e tıklayın.

   ![Destek Ayarları sayfasında tamamlanmış güvenli proaktif destek bölüm içeren ekran görüntüsü](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi eklemeniz gerekiyorsa, var olan bulut depolama sistemi kümeye, yönergeleri izleyin veya [depolamayı yapılandırma](avere-vfxt-add-storage.md). 

İstemciler okuma kümeye ekleme başlatmaya hazırsanız [Avere vFXT küme bağlama](avere-vfxt-mount-clients.md).