---
title: StorSimple sanal dizisi hizmeti Özet dikey | Microsoft Docs
description: Hizmet özeti dikey için StorSimple cihaz Yöneticisi ve bunu StorSimple Virtual Array'iniz durumunu izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9c05bddaeb3c34400db1ec75c624ef00a85d9444
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60337936"
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>StorSimple Virtual Array için kullanım hizmeti Özet dikey için StorSimple cihaz Yöneticisi bağlı
## <a name="overview"></a>Genel Bakış
Hizmet özeti dikey için StorSimple cihaz Yöneticisi, StorSimple sanal (diğer adıyla StorSimple sanal cihazları veya sanal cihazlar şirket içinde) hizmetinize bağlı olan bir sistem ihtiyacınız olanlar vurgulama dizilerin Özet görünümünü sağlar Yöneticinin dikkat. Bu öğreticide hizmet özeti dikey sunar, içerik ve işlevi açıklanmaktadır ve bu dikey pencereden gerçekleştirebileceğiniz görevler açıklanmaktadır.

![Hizmet panosu](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Yönetim komutları ve ilgili temel bilgiler
StorSimple Özet dikey penceresinde, StorSimple cihaz Yöneticisi hizmetiniz ve bunun yanı sıra bu hizmete kayıtlı sanal diziler yönetmeye yönelik seçenekler görürsünüz. Dikey pencerenin ve sol taraftaki üstteki yönetimi komutları bakın.

Paylaşımları veya birimler eklemek gibi veya İzleyici çeşitli işleri çalıştıran sanal diziler üzerinde çeşitli işlemler gerçekleştirmek için bu seçenekleri kullanın.

Temel alan, kaynak grubu, konum ve StorSimple cihaz Yöneticisi oluşturulduğu abonelik gibi önemli özelliklerin bazılarını yakalar.

## <a name="storsimple-device-manager-service-summary"></a>StorSimple cihaz Yöneticisi hizmet özeti
* **Uyarılar** kutucuğu uyarı önem derecesine göre gruplandırılmış olarak tüm sanal cihazlar arasında tüm etkin uyarıları anlık görüntüsünü sağlar. Kutucuğa tıklandığında açılır **uyarılar** dikey penceresinde, uyarı hakkında ek ayrıntıları görüntülemek için bir bireysel uyarı tıklayabilirsiniz burada dahil önerilen eylemler. Uyarı, sorunu çözerseniz de temizleyebilirsiniz.
* **Kapasite** kutucuk görüntüler, kullanılabilir toplam depolama alanı göreli sanal tüm cihazlardaki tüm sanal cihazlar arasında sağlanan ve kalan birincil depolama gösterir. **Sağlanan** hazır ve kullanılmak için ayrılan depolama alanı miktarının başvurduğu **kalan** tüm sanal cihazlar arasında sağlanabilir kalan kapasiteyi ifade eder. **Kalan katmanlı** bulut dahil olmak üzere sağlanabilir kullanılabilir kapasiteyi kapasitesidir sırada **kalan yerel** sanal diziler için bağlı diskler kalan kapasite.
* İçinde **kullanım** grafiği, ilgili ölçümleri sanal cihazlarınızı görebilirsiniz. Son 7 gün içindeki, varsayılan süre sanal cihazlar tarafından kullanılan bulut depolama alanına yanı sıra, tüm sanal cihazlar arasında kullanılan birincil depolama görüntüleyebilirsiniz. Kullanım **Düzenle** farklı zaman ölçeği seçmek için sağ üst köşedeki grafiğin seçeneği.
* **Cihazları** kutucuğu sanal dizisi, StorSimple cihaz cihaz durumlarına göre gruplandırılmış Yöneticisi'nde sayısının bir özeti sağlar. Açmak için bu kutucuğa tıklayarak **cihazları** listesi dikey penceresinde ve sonra cihaza özgü cihaz özeti incelemek için tek bir cihaza tıklayın. Ayrıca, belirli bir cihaz özeti dikey penceresinden belirli cihaz eylemleri gerçekleştirebilirsiniz. Cihaz özeti dikey penceresi hakkında daha fazla bilgi için Git [Özet dikey penceresinde cihaz](storsimple-virtual-array-device-summary.md).

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme
StorSimple cihaz Yöneticisi içinde gerçekleştirilen çeşitli işlemler görüntülemek için tıklayın **etkinlik günlüklerini** StorSimple hizmeti Özet dikey penceresinin sol tarafındaki bağlantıyı. Sayfasına yönlendirileceksiniz **etkinlik günlüklerini** dikey penceresinde gerçekleştirilen son işlem özetini görebileceğiniz.

![Etkinlik Günlükleri](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [StorSimple Virtual Array'iniz yönetmek için yerel web kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md).

