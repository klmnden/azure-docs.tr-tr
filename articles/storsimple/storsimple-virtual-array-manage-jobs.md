---
title: StorSimple sanal dizisi işleri görüntüleme ve yönetme | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti işleri sayfasında ve nasıl StorSimple sanal dizisi için yeni ve güncel işleri izlemek için kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: dbab2aaab2c12bef07748f54e5864d042f1c982a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302509"
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>StorSimple sanal dizisi için işleri görüntülemek için StorSimple cihaz Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
**İşleri** görüntüleme ve StorSimple cihaz Yöneticisi hizmetinize bağlı olan sanal diziler üzerinde çalışmaya işlerini yönetmek için tek bir merkezi portal dikey penceresinde sağlar. Birden çok sanal cihaz için çalışan, tamamlanan ve başarısız işleri görüntüleyebilirsiniz. Sonuçları bir tablo biçiminde gösterilir.

![İşleri dikey penceresi](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Alanlarını filtreleyerek, ilgilenen işleri hızlıca bulabilirsiniz:

* **Zaman aralığı** – işleri filtrelenebilir göre tarih ve saat aralığı.
* **Cihazları** – işleri başlatılan hizmetinize bağlı olarak belirli bir cihazdaki. Filtrelenen işler, ardından aşağıdaki özniteliklere bağlı tabloda verilmiştir:
  
  * **Adı** – iş ad **tüm**, **yedekleme**, **kopya**, **yük devretme**, **güncelleştirmeleriniindir**, veya **Güncelleştirmeleri Yükle**.
  * **Durum** – işleri olabilir **tüm**, **sürüyor**, **başarılı**, veya **başarısız**, veya **iptaledildi**.
  * **Varlık** – işleri bir birim, paylaşım veya cihaz ile ilişkili olabilir.
  * **Cihaz** – iş başlatıldı cihazın adı.
  * **Başlangıç zamanı** – işin başlama zamanı.
  * **Süre** – süresince işin çalıştırıldığı üzerinde.
* **Durum** – tüm, çalışan, tamamlandı veya başarısız işler için arama yapabilirsiniz.
* **İş türü** – iş türü all, yedekleme, geri yükleme, yük devretme olması, güncelleştirmeleri karşıdan yüklemek veya güncelleştirmeleri yükleyin.

İşlerin listesini her 30 saniyede bir yenilenir.

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işin ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. Üzerinde **işleri** dikey penceresinde, ilgilendiğiniz bir sorgu uygun filtreleri ile çalıştırarak işi görüntüle. Tamamlanmış veya çalışan işler için arama yapabilirsiniz.
2. Bir iş işleri Tablo listesinden seçin.
   
    ![İş dikey penceresi](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Sayfanın en altında tıklayın **ayrıntıları**.
4. İçinde **ayrıntıları** iletişim kutusu, durum, Ayrıntılar ve süresi istatistikleri görüntüleyebilirsiniz. Örnek olarak aşağıdaki çizimde **yedekleme işinin ayrıntılarını** iletişim kutusu.
   
    ![İş ayrıntıları](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Sanal makine hiper yönetici içine duraklatıldığında iş hataları
Bir işi olduğunda StorSimple Virtual Array'iniz ve cihaza (sanal makine hiper yöneticide sağlanır) ilerleme 15 dakikadan daha duraklatılır, işi başarısız oluyor. Bu StorSimple Virtual Array sürenizi nedeniyle Microsoft Azure saati ile eşitlenmemiş yapılıyor. 

Aşağıdaki hatayı görürsünüz: "Cihaz saatiniz tarafından 15 dakikadan daha fazla Microsoft Azure saati ile eşitlenmemiş. Hiper Yönetici ve süreleri bir NTP sunucusuyla eşitlenmiş ve cihaz emin olun. Hiçbir bağlantı sorunu olmadığını doğrulayın. Bağlantı sorunlarını gidermek için tanılama testleri yerel web kullanıcı arabiriminden sanal cihazınızın çalıştırın."

Bu hatalar, yedekleme, geri yükleme, güncelleştirme ve yük devretme işleri için geçerli. Sanal makinenizi Hyper-V sağladıysanız, makine zaman sonunda, hiper yönetici ile eşitler. Bu durumda, iş yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Virtual Array'iniz yönetmek için yerel web UI'ı kullanmayı öğrenin](storsimple-ova-web-ui-admin.md).

