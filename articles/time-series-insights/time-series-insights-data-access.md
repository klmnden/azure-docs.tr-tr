---
title: "Erişim ve Azure zaman serisi Öngörüler yönetmek için güvenliği yapılandırma | Microsoft Docs"
description: "Bu makalede güvenlik ve izinler yönetim erişimi nasıl yapılandırılacağı ilkeler ve veri erişim ilkeleri Azure zaman serisi Öngörüler güvenli hale getirmek için."
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: c7d4079c9106226e0d07aa97c4a52c16ddb257c3
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamına veri erişimi verme

Zaman Serisi Görüşleri ortamlarının birbirinden bağımsız türde iki erişim ilkesi vardır:

* Yönetim erişimi ilkeleri
* Veri erişimi ilkeleri

Her iki ilke de Azure Active Directory asıl adlarına (kullanıcılar ve uygulamalar) belirli bir ortam üzerinde çeşitli izinler verir. Ortam içeren abonelikle ilişkili (Azure Kiracı olarak da bilinir) active Directory ilkelerini (kullanıcılar ve uygulamalar) ait olması gerekir.

Yönetim erişimi ilkeleri, ortamı, olay kaynaklarını, başvuru veri kümelerini oluşturma ve
*   Silme, veri erişimi ilkelerini yönetme gibi ortamın yapılandırmasıyla ilgili izinler
*   verir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

İki ilke türü de ortamın yönetimine erişim ile ortam içindeki verilere erişim arasında net bir ayrım yapmaya olanak tanır. Örneğin, sahibi/Oluşturucusu ortamının veri erişimden kaldırılır, bir ortamı ayarlamak mümkündür. Ayrıca, kullanıcılar ve ortamdan verileri okumak için izin verilen hizmetler ortamının yapılandırması erişimi verilebilir.

## <a name="grant-data-access"></a>Veri erişim izni verme
Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Zaman serisi Öngörüler ortamınızı bulun. Tür **zaman serisi** içinde **arama** kutusu. Seçin **zaman serisi ortam** arama sonuçlarında. 

3. Listeden Zaman Serisi Görüşleri ortamınızı seçin.
   
4. Seçin **veri erişimi ilkelerini**seçeneğini belirleyip **+ Ekle**.
  ![Zaman serisi Öngörüler kaynak - yönetmek ortamı](media/data-access/getstarted-grant-data-access1.png)

5. Seçin **kullanıcı**.  Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklatın **seçin** Seçimi onaylamak için. 

   ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/data-access/getstarted-grant-data-access2.png)

6. Seçin **Select rol**. Kullanıcı için uygun erişim rolünü seçin:
   - Seçin **katkıda bulunan** perspektif Ortamı'nın diğer kullanıcılarla başvuru verileri ve kaydedilmiş paylaşımı sorgular ve değiştirmek kullanıcı izin vermek istiyorsanız. 
   - Aksi takdirde seçin **okuyucu** kullanıcı sorgu veri ortamda izin ver ve kişisel (paylaşılmayan) sorguları ortamda kaydetmek için.

   Seçin **Tamam** rolü Seçimi onaylamak için.

   ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/data-access/getstarted-grant-data-access3.png)

8. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

   ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/data-access/getstarted-grant-data-access4.png)

9. **Veri erişimi ilkelerini** sayfası kullanıcılar ve her kullanıcı için rolleri listeler.

   ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi [Azure zaman serisi Öngörüler ortamınız için bir olay hub'ı olay kaynağı ekleme](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Olayları göndermek](time-series-insights-send-events.md) olay kaynağı.
* Ortamınızdaki görüntülemek [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com).
