---
title: Uzaktan izleme veri erişim denetimi - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme çözüm Hızlandırıcısını Time Series Insights telemetri Gezgini'nde için erişim denetimleri nasıl yapılandıracağınız hakkında bilgi sağlar.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/06/2018
ms.topic: conceptual
ms.openlocfilehash: 9d5d572c3e32e3645e65ba8d6fc28b567b3c1e9a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827177"
---
# <a name="configure-access-controls-for-the-time-series-insights-telemetry-explorer"></a>Time Series Insights telemetri Gezgini erişim denetimlerini yapılandırın

Bu makale Uzaktan izleme çözüm Hızlandırıcısını Time Series Insights Gezgininde için erişim denetimleri yapılandırma hakkında bilgi sağlar. Çözüm Hızlandırıcısı, kullanıcıların Time Series Insights gezgininin erişmesine izin vermek için her kullanıcı veri erişimi vermeniz gerekir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

4. Seçin **veri erişimi ilkeleri**, ardından **+ Ekle**.
    ![Zaman serisi görüşleri kaynağını yönetme - ortam](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access1.png)

5. Seçin **Kullanıcı Seç**.  Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklayın **seçin** Seçimi onaylamak için. 

    ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access2.png)

6. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:
   - Seçin **katkıda bulunan** başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmek kullanıcı izin vermek istiyorsanız. 
   - Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

     Seçin **Tamam** rolü seçiminizi onaylamak için.

     ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access3.png)

7. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

    ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access4.png)

8. **Veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

    ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/iot-accelerators-remote-monitoring-rbac-tsi/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Uzaktan izleme çözüm Hızlandırıcısını Time Series Insights Gezgininde için erişim denetimleri nasıl verilir öğrendiniz.

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan izleme çözümü özelleştirme hakkında daha fazla bilgi için bkz. [özelleştirme ve yeniden dağıtma bir mikro hizmet](iot-accelerators-microservices-example.md)
<!-- Next tutorials in the sequence -->