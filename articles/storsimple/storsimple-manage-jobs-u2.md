---
title: "StorSimple işleri görüntüle ve Yönet | Microsoft Docs"
description: "StorSimple Yöneticisi hizmeti işleri sayfasında ve son, geçerli ve zamanlanmış yedekleme işlerini izleme açıklar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 6df1b27ce76de7a781ecc40af8430114d80b20d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Görüntülemek ve StorSimple işleri (güncelleştirme 2) yönetmek için StorSimple Yöneticisi hizmetini kullanma
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Genel Bakış
**İşleri** sayfasını görüntülemek için tek bir merkezi portal sağlar ve StorSimple Yöneticisi hizmetinize bağlı cihazlarda başlatılmış işlerini yönetme. Birden çok aygıt zamanlanmış, çalışan, tamamlandı, iptal edilmiş ve başarısız işleri görüntüleyebilirsiniz. Sonuçları bir tablo biçiminde sunulur. 

![İşler sayfası](./media/storsimple-manage-jobs-u2/jobs.png)

Hızlı bir şekilde, içinde alanları gibi filtreleyerek ilginizi çekiyor mu işleri bulabilirsiniz:

* **Durum** – işleri çalıştırıyor olabilir, tamamlandı, iptal edilmiş, başarısız, iptal etme veya hatalarla tamamlandı.
* **Başlangıç ve bitiş** – işleri filtre tarih ve saat aralıklarına göre.
* **Tür** – iş türü yedekleme, el ile yedekleme, geri yükleme, kopyalama, aygıt yük devretme, yerel olarak sabitlenmiş birim oluşturma, birim değiştirmek, Güncelleştir, paket veya sanal cihaz sağlamayı destekler.
* **Aygıtları** – işleri hizmetinize bağlanan bir aygıttaki belirli başlattı.
  Filtrelenen işler, ardından aşağıdaki öznitelikleri temel alınarak tabloda verilmiştir:
  
  * **Tür** – yedekleme, el ile yedekleme, geri yükleme, kopyalama, aygıt yük devretme, yerel olarak sabitlenmiş birim oluşturma, birim değiştirmek, Güncelleştir, paket veya sanal cihaz sağlamayı destekler.
  * **Durum** – çalışan, tamamlandı, iptal edilmiş, başarısız, iptal etme veya hatalarla tamamlandı.
  * **Varlık** – işleri bir birim, bir yedekleme İlkesi veya bir aygıt ile ilişkili olabilir. Örneğin, zamanlanmış bir yedekleme işi bir yedekleme ilkesiyle ilişkili iken bir kopya iş bir birimi ile ilişkilidir. Bir aygıt iş olağanüstü durum kurtarma (DR) veya geri yükleme işleminin sonucu olarak oluşturulur.
  * **Aygıt** – işi başlatıldı aygıt adı.
  * **Tarihinde başlayan** – zaman zaman işi başlatıldı.
  * **İlerleme** – çalışan işin tamamlanma. Tamamlanmış bir iş için bu her zaman % 100 olmalıdır.

İşlerin listesini her 30 saniyede yenilenir.

Bu sayfada iş ile ilgili aşağıdaki eylemleri gerçekleştirebilirsiniz:

* İş ayrıntılarını görüntüleme
* Bir işi iptal etme

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işi ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. Üzerinde **işleri** sayfasında, ilgilendiğiniz uygun filtrelerle bir sorgu çalıştırarak iş görüntüler. Çalışan veya işi iptal tamamlandı için arama yapabilirsiniz.
2. Bir iş seçin.
3. Sayfanın alt kısmındaki tıklatın **ayrıntıları**.
4. İçinde **yedekleme işi ayrıntılarını** iletişim kutusu, durumunu, ayrıntıları, süresi istatistikleri ve veri istatistiklerini görüntüleyebilirsiniz.
   
    ![İş Ayrıntıları sayfası](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Bir işi iptal etme
Çalışan bir işi iptal etmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Birim türünü değiştirmek için bir birim değiştirmekten ya da bir birim genişletme gibi bazı işleri iptal edilemez.
> 
> 

### <a name="to-cancel-a-job"></a>Bir işi iptal etmek için
1. Üzerinde **işleri** sayfasında, uygun filtrelerle bir sorgu çalıştırarak iptal etmek istediğiniz çalışan iş görüntüler.
2. İşi seçin.
3. Sayfanın alt kısmındaki tıklatın **iptal**.
4. Onayınız istendiğinde **Evet**’e tıklayın.

Bu işi iptal edildi.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [, StorSimple yedekleme ilkelerini yönetme](storsimple-manage-backup-policies.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manager-service-administration.md).

