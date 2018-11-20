---
title: Avere vFXT - Azure desteğini etkinleştirme
description: Desteğini nasıl etkinleştireceğinizi Avere vFXT ' Azure'a yükler.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 0f5eee20b0487fb5fce82047f40d137effb87ead
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164439"
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
1. Tıklayın **gönderme**.

   ![Müşteri bilgileri bölümü destek ayarları sayfasının ekran görüntüsü içeren tamamlandı](media/avere-vfxt-support-info.png)

1. Sol tarafındaki üçgeni **güvenli proaktif destek (SPS)** bölümü genişletin.
1. İçin kutuyu **etkinleştir SPS bağlantısını**.
1. Tıklayın **gönderme**.

   ![Destek Ayarları sayfasında tamamlanmış güvenli proaktif destek bölüm içeren ekran görüntüsü](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir şirket içi depolama sistemi kümeye eklemeniz gerekiyorsa, yönergeleri izleyin [depolamayı yapılandırma](avere-vfxt-add-storage.md). 

İstemciler okuma kümeye ekleme başlatmaya hazırsanız [Avere vFXT küme bağlama](avere-vfxt-mount-clients.md).