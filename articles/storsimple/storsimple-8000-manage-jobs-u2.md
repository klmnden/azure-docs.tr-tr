---
title: StorSimple 8000 serisi için işleri görüntüleme ve yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti işler dikey penceresinde ve bunu son, geçerli ve zamanlanmış yedekleme işleri izlemek için nasıl kullanılacağını açıklar.
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
ms.openlocfilehash: 462f8dafdffa7ee01e6ccf7945a1abfdff90db42
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60506223"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a>(Güncelleştirme 3 ve üzeri) işleri görüntüleme ve yönetme için StorSimple cihaz Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış
**İşleri** dikey penceresini görüntülemek için tek bir Yönetim Portalı sağlar ve StorSimple cihaz Yöneticisi hizmetinize bağlı cihazlarda başlatılan işleri yönetme. Birden çok cihaza yönelik zamanlanmış, çalışan, tamamlandı, iptal edilen ve başarısız işleri görüntüleyebilirsiniz. Sonuçları bir tablo biçiminde gösterilir.

![İşleri dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Alanlarını filtreleyerek, ilgilenen işleri hızlıca bulabilirsiniz:

* **Durum** – işleri başarılı oldu, devam ediyor, iptal edildi, başarısız, iptal etme veya başarılı olan hataları olabilir.
* **Zaman aralığı** – işleri filtrelenebilir göre tarih ve saat aralığı. Son 1 saat, son son 30 gün, geçtiğimiz yıl veya özel tarih son 7 gün 24 saat aralıktır.
* **Tür** – iş türü olabilir zamanlanmış yedekleme, el ile yedekleme, geri yükleme yedeği toplu kopyalama, birim kapsayıcılarını Devret, yerel olarak sabitlenmiş birim oluşturma, birimi değiştirme, güncelleştirmeleri yüklemek, destek günlükleri toplayın ve bulut Gereci oluşturma.
* **Cihazları** – işleri başlatılan hizmetinize bağlanan bir cihazdaki belirli.
  
Filtrelenen işler, ardından aşağıdaki öznitelikleri temel alarak tabloda verilmiştir:
  
* **Adı** – zamanlanmış yedekleme, yedekleme, yedekleme geri yükleme, kopyalama toplu başarısız birim kapsayıcılarının yükü el ile yerel olarak sabitlenmiş birim oluştur, birimi değiştirme, güncelleştirmeleri yükleyin, destek günlüklerini Topla veya Bulut Gereci oluşturma.
* **Durum** – çalışan, tamamlandı, iptal edildi, başarısız, iptal etme veya hatalarla tamamlandı.
* **Varlık** – işleri bir birim, bir yedekleme İlkesi veya bir cihaz ile ilişkili olabilir. Örneğin, zamanlanmış bir yedekleme işi bir yedekleme ilkesiyle ilişkili iken bir kopyalama işi bir birim ile ilişkilidir. Bir cihaz iş olağanüstü durum kurtarma (DR) veya geri yükleme işleminin sonucu olarak oluşturulur.
* **Cihaz** – iş başlatıldı cihazın adı.
* **Başlangıç zamanı** – işin başlama zamanı.
* **Süre** – işi tamamlamak için gereken süre.

İşlerin listesini her 30 saniyede bir yenilenir.

Bu sayfada, iş ile ilgili aşağıdaki eylemleri gerçekleştirebilirsiniz:

* İş ayrıntılarını görüntüleme
* Bir işi iptal et

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işin ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından **işleri**.

2. İçinde **işleri** dikey penceresinde, ilgilendiğiniz bir sorgu uygun filtreleri ile çalıştırarak işi görüntüle. Çalışan veya işleri iptal tamamlandı için arama yapabilirsiniz.

    ![İş dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Seçin ve bir işi tıklayın.

    ![İş dikey penceresi](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. İş ayrıntıları dikey penceresinde, durum, Ayrıntılar, süresi istatistikleri ve veri istatistiklerini görüntüleyebilirsiniz.
   
    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Bir işi iptal et
Bir çalışan işi iptal etmek için aşağıdaki adımları gerçekleştirin.

> [!NOTE]
> Birim türünü değiştirmek için bir birim değiştirme ya da bir birim, genişletme gibi bazı işleri iptal edilemez.


### <a name="to-cancel-a-job"></a>Bir işi iptal etmek
1. Üzerinde **işleri** sayfasında, uygun filtreleri ile çalışan bir sorgu iptal etmek istediğiniz çalışan işler görüntüleyebilirsiniz. İşi seçin.

2. Sağ tıklayın ve bağlam menüsünü açmak için seçilen projede **iptal**.

    ![İş ayrıntıları](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Onayınız istendiğinde **Evet**’e tıklayın. Bu işi iptal edildi.

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [, StorSimple yedekleme ilkelerini yönetme](storsimple-8000-manage-backup-policies-u2.md).
* Bilgi edinmek için nasıl [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

