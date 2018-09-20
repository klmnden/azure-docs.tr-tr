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
ms.openlocfilehash: fb45ea02f365cf4e7b394e249f9b91a784e5469f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368864"
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