---
title: include dosyası
description: include dosyası
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: include
ms.date: 08/20/2018
ms.author: anshan
ms.custom: include file
ms.openlocfilehash: c9daa86bf36b260001d9969385b9e8a98a8ac0cf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61443251"
---
## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

4. Seçin **veri erişimi ilkeleri**, ardından **+ Ekle**.
    ![Zaman serisi görüşleri kaynağını yönetme - ortam](media/iot-tsi-data-access/getstarted-grant-data-access1.png)

5. Seçin **Kullanıcı Seç**.  Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklayın **seçin** Seçimi onaylamak için. 

    ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/iot-tsi-data-access/getstarted-grant-data-access2.png)

6. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:
   - Seçin **katkıda bulunan** başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmek kullanıcı izin vermek istiyorsanız. 
   - Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

     Seçin **Tamam** rolü seçiminizi onaylamak için.

     ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/iot-tsi-data-access/getstarted-grant-data-access3.png)

7. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

    ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/iot-tsi-data-access/getstarted-grant-data-access4.png)

8. **Veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

    ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/iot-tsi-data-access/getstarted-grant-data-access5.png)