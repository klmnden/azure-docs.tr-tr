---
title: StorSimple sanal dizinin işleri görüntüle ve Yönet | Microsoft Docs
description: StorSimple cihaz Yöneticisi hizmeti işleri sayfasında ve, StorSimple sanal dizi için yeni ve geçerli işleri izlemek için nasıl kullanılacağını açıklar.
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
ms.openlocfilehash: 3fd1c262a8ce94d8e98f2b066a8028d974b15b1d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "23927722"
---
# <a name="use-the-storsimple-device-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>StorSimple sanal dizi için işleri görüntülemek için StorSimple cihaz Yöneticisi hizmetini kullanma
## <a name="overview"></a>Genel Bakış
**İşleri** dikey görüntüleme ve StorSimple cihaz Yöneticisi hizmetinize bağlı sanal diziler tarihinde başlayan işlerini yönetmek için tek bir merkezi portal sağlar. Birden çok sanal cihazda çalışan, tamamlanmış ve başarısız işleri görüntüleyebilirsiniz. Sonuçları bir tablo biçiminde sunulur.

![İşlerini dikey penceresi](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Hızlı bir şekilde, içinde alanları gibi filtreleyerek ilginizi çekiyor mu işleri bulabilirsiniz:

* **Zaman aralığı** – işleri filtre tarih ve saat aralıklarına göre.
* **Aygıtları** – işleri başlatılan hizmetinize bağlı olarak belirli bir aygıtta. Filtrelenen işler, ardından aşağıdaki özniteliklerini temel alarak tabloda verilmiştir:
  
  * **Ad** – iş adı olabilir **tüm**, **yedekleme**, **kopya**, **yük devri**, **indirmegüncelleştirir**, veya **güncelleştirmelerini yükleme**.
  * **Durum** – işleri olabilir **tüm**, **devam eden**, **başarılı**, veya **başarısız**, veya **iptaledildi**.
  * **Varlık** – işleri bir birim, paylaşım veya cihaz ile ilişkili olabilir.
  * **Aygıt** – işi başlatıldı aygıt adı.
  * **Tarihinde başlayan** – zaman zaman işi başlatıldı.
  * **Süre** – süresince, işin çalıştırıldığı üzerinde.
* **Durum** – tüm, çalışan, tamamlandı ya da başarısız işleri için arama yapabilirsiniz.
* **İş türünü** – iş türü tümü, yedekleme, geri yükleme, yük devretme olması, güncelleştirmeleri karşıdan veya güncelleştirmeleri yükleyin.

İşlerin listesini her 30 saniyede yenilenir.

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
Herhangi bir işi ayrıntılarını görüntülemek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-job-details"></a>İş ayrıntılarını görüntülemek için
1. Üzerinde **işleri** dikey penceresinde, ilgilendiğiniz uygun filtrelerle bir sorgu çalıştırarak iş görüntüler. Tamamlanan veya çalışan işleri arama yapabilirsiniz.
2. Bir iş işleri tablo listeden seçin.
   
    ![İş dikey penceresi](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Sayfanın alt kısmındaki tıklatın **ayrıntıları**.
4. İçinde **ayrıntıları** iletişim kutusu, durumu, Ayrıntılar ve süresi istatistikleri görüntüleyebilirsiniz. Aşağıdaki çizimde bir örneği gösterilmektedir **yedekleme işi ayrıntılarını** iletişim kutusu.
   
    ![İş ayrıntıları](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Sanal makine hiper yöneticide duraklatıldığında işi hataları
Bir iş olduğunda, StorSimple sanal dizinin ve (hiper yöneticide sağlanan sanal makine) cihaz ilerleme 15 dakikadan daha duraklatılır, iş başarısız. Bu, StorSimple sanal dizinin zaman nedeniyle Microsoft Azure saatiyle eşitlenmiş yapılıyor. 

Aşağıdaki hatayı görürsünüz: "cihaz zamanınızı 15 dakikadan fazla Microsoft Azure zaman ile eşitlenmemiş. Hiper Yönetici ve bir NTP sunucusu ile zamanlarının eşitlendiğini cihaz emin olun. Bağlantı sorunu olmadığından olduğunu doğrulayın. Bağlantı sorunlarını gidermek için tanılama testleri yerel web kullanıcı Arabirimi sanal cihazınızın çalıştırın."

Bu hatalar yedekleme, geri yükleme, güncelleştirme ve yük devretme işleri için geçerlidir. Hyper-V, sanal makine sağlandıktan, makine sonunda zaman, hiper yönetici ile eşitler. Bu gerçekleştikten sonra işi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
[Yerel web kullanıcı Arabirimi, StorSimple sanal dizinin yönetmek için nasıl kullanılacağını öğrenin](storsimple-ova-web-ui-admin.md).

