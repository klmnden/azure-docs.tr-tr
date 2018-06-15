---
title: StorSimple 8000 serisi işleri görüntüle ve Yönet | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti işler dikey penceresinde ve son, geçerli ve zamanlanmış yedekleme işlerini izleme açıklar.
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
ms.openlocfilehash: bf8038b7171053b75eeb9aed88bff4246e65a8a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23874818"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a>Görüntülemek ve işleri (güncelleştirme 3 ve üzeri) yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma

## <a name="overview"></a>Genel Bakış
**İşleri** dikey penceresini görüntülemek için tek bir merkezi portal sağlar ve StorSimple cihaz Yöneticisi hizmetinize bağlı cihazlarda başlatılmış işlerini yönetme. Birden çok aygıt zamanlanmış, çalışan, tamamlandı, iptal edilmiş ve başarısız işleri görüntüleyebilirsiniz. Sonuçları bir tablo biçiminde sunulur.

![İşlerini dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Hızlı bir şekilde, içinde alanları gibi filtreleyerek ilginizi çekiyor mu işleri bulabilirsiniz:

* **Durum** – işleri, başarılı, sürüyor, iptal edilmiş, başarısız, iptal etme veya ile başarılı oldu, hata olabilir.
* **Zaman aralığı** – işleri filtre tarih ve saat aralıklarına göre. 1 saat, son 30 gün, önceki yıl veya özel tarihi geçmiş 7 gün 24 saat geçmiş geçmiş aralıktır.
* **Tür** – zamanlanmış yedekleme, el ile yedekleme, geri yükleme yedekleme iş türü olabilir kopyalama birimi, birim kapsayıcıları başarısız, yerel olarak sabitlenmiş birim oluşturma, birim değiştirmek, güncelleştirmeleri yüklemek, destek günlükleri toplamak ve bulut uygulaması oluşturma.
* **Aygıtları** – işleri hizmetinize bağlanan bir aygıttaki belirli başlattı.
  
Filtrelenen işler, ardından aşağıdaki öznitelikleri temel alınarak tabloda verilmiştir:
  
* **Ad** – zamanlanmış yedekleme, yedekleme, geri yükleme yedekleme, kopya birim, başarısız birim kapsayıcıları el ile yerel olarak sabitlenmiş birim oluşturma, birim değiştirmek, güncelleştirmeleri yüklemek, destek günlükleri toplamak veya Bulut uygulaması oluşturun.
* **Durum** – çalışan, tamamlandı, iptal edilmiş, başarısız, iptal etme veya hatalarla tamamlandı.
* **Varlık** – işleri bir birim, bir yedekleme İlkesi veya bir aygıt ile ilişkili olabilir. Örneğin, zamanlanmış bir yedekleme işi bir yedekleme ilkesiyle ilişkili iken bir kopya iş bir birimi ile ilişkilidir. Bir aygıt iş olağanüstü durum kurtarma (DR) veya geri yükleme işleminin sonucu olarak oluşturulur.
* **Aygıt** – işi başlatıldı aygıt adı.
* **Tarihinde başlayan** – zaman zaman işi başlatıldı.
* **Süre** – işi tamamlamak için gereken süre.

İşlerin listesini her 30 saniyede yenilenir.

Bu sayfada iş ile ilgili aşağıdaki eylemleri gerçekleştirebilirsiniz:

* İş ayrıntılarını görüntüleme
* Bir işi iptal etme

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işi ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **işleri**.

2. İçinde **işleri** dikey penceresinde, ilgilendiğiniz uygun filtrelerle bir sorgu çalıştırarak iş görüntüler. Çalışan veya işi iptal tamamlandı için arama yapabilirsiniz.

    ![İş dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Seçin ve bir iş'i tıklatın.

    ![İş dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. İş ayrıntıları dikey penceresinde, durumunu, ayrıntıları, süresi istatistikleri ve veri istatistiklerini görüntüleyebilirsiniz.
   
    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Bir işi iptal etme
Çalışan bir işi iptal etmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Birim türünü değiştirmek için bir birim değiştirmekten ya da bir birim genişletme gibi bazı işleri iptal edilemez.


### <a name="to-cancel-a-job"></a>Bir işi iptal etmek için
1. Üzerinde **işleri** sayfasında, uygun filtrelerle bir sorgu çalıştırarak iptal etmek istediğiniz çalışan iş görüntüler. İşi seçin.

2. Bağlam menüsü çağırma tıklatıp seçili iş üzerinde sağ **iptal**.

    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın. Bu işi iptal edildi.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [, StorSimple yedekleme ilkelerini yönetme](storsimple-8000-manage-backup-policies-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

