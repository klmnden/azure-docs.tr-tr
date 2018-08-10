---
title: Erişim ve Azure Time Series Insights'ı yönetmek için güvenlik yapılandırma | Microsoft Docs
description: Bu makalede güvenlik ve izinler yönetim erişimi yapılandırma ilkeleri ve veri erişim ilkeleri Azure Time Series Insights güvenliğini sağlamak için.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 97c9480d6f2b75d83252bfb6410d7b5f946757ef
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630662"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamına veri erişimi verme

Zaman Serisi Görüşleri ortamlarının birbirinden bağımsız türde iki erişim ilkesi vardır:

* Yönetim erişimi ilkeleri
* Veri erişimi ilkeleri

Her iki ilke de Azure Active Directory asıl adlarına (kullanıcılar ve uygulamalar) belirli bir ortam üzerinde çeşitli izinler verir. Ortamı içeren abonelikle ilişkili (Azure kiracısı bilinir) active Directory asıl adlarına (kullanıcılar ve uygulamalar) ait olmalıdır.

Yönetim erişimi ilkeleri, ortamı, olay kaynaklarını, başvuru veri kümelerini oluşturma ve
*   Silme, veri erişimi ilkelerini yönetme gibi ortamın yapılandırmasıyla ilgili izinler
*   verir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

İki ilke türü de ortamın yönetimine erişim ile ortam içindeki verilere erişim arasında net bir ayrım yapmaya olanak tanır. Örneğin, ortamın sahibinin/oluşturucusunun veri erişiminden kaldırılır, bir ortamı ayarlamak mümkündür. Ayrıca, kullanıcılar ve ortamdan veri okumak için izin verilen hizmetler ortamının yapılandırması erişim verilebilir.

## <a name="grant-data-access"></a>Veri erişim izni verme
Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Time Series Insights ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.
   
4. Seçin **veri erişimi ilkeleri**, ardından **+ Ekle**.
  ![Zaman serisi görüşleri kaynağını yönetme - ortam](media/data-access/getstarted-grant-data-access1.png)

5. Seçin **Kullanıcı Seç**.  Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklayın **seçin** Seçimi onaylamak için. 

   ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/data-access/getstarted-grant-data-access2.png)

6. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:
   - Seçin **katkıda bulunan** başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmek kullanıcı izin vermek istiyorsanız. 
   - Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

   Seçin **Tamam** rolü seçiminizi onaylamak için.

   ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/data-access/getstarted-grant-data-access3.png)

8. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

   ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/data-access/getstarted-grant-data-access4.png)

9. **Veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

   ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi [Azure zaman serisi görüşleri ortamınıza bir Event Hub olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Olayları gönderme](time-series-insights-send-events.md) olay kaynağına.
* Ortamınızı görüntüleyebilirsiniz [Time Series Insights gezgininin](https://insights.timeseries.azure.com).
