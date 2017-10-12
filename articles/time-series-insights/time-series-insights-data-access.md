---
title: "Azure Zaman Serisi Görüşleri’nde veri erişimi ilkeleri | Microsoft Docs"
description: "Bu öğreticide, Zaman Serisi Görüşleri’nde veri erişimi ilkelerini yönetmeyi öğreneceksiniz"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: 5258bf5de6f7aa1ea246f1235e7d362b1b7d0181
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamına veri erişimi verme

Zaman Serisi Görüşleri ortamlarının birbirinden bağımsız türde iki erişim ilkesi vardır:

* Yönetim erişimi ilkeleri
* Veri erişimi ilkeleri

Her iki ilke de Azure Active Directory asıl adlarına (kullanıcılar ve uygulamalar) belirli bir ortam üzerinde çeşitli izinler verir. Asıl adların (kullanıcılar ve uygulamalar), ortamı içeren abonelikle ilişkilendirilmiş Active Directory (veya “Azure kiracısı”) içinde yer almaları gerekir.

Yönetim erişimi ilkeleri, ortamı, olay kaynaklarını, başvuru veri kümelerini oluşturma ve
*   Silme, veri erişimi ilkelerini yönetme gibi ortamın yapılandırmasıyla ilgili izinler
*   verir.

Veri erişimi ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme, ortamla ilişkilendirilmiş kaydedilen sorguları ve perspektifleri paylaşma izinleri verir.

İki ilke türü de ortamın yönetimine erişim ile ortam içindeki verilere erişim arasında net bir ayrım yapmaya olanak tanır. Örneğin, ortamın sahibinin/oluşturucusunun veri erişiminden kaldırıldığı bir ortam kurmak mümkündür. Bunun yanı sıra, ortamdaki verileri okumasına izin verilen kullanıcılar ve hizmetlere, ortamın yapılandırması üzerinde erişim verilmeyebilir.

## <a name="grant-data-access"></a>Veri erişim izni verme
Aşağıdaki adımlar, bir kullanıcı asıl adına veri erişimi verme işlemini gösterir:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Azure Portal'ın sol tarafındaki menüde "Tüm kaynaklar"a tıklayın.
3.  Zaman Serisi Görüşleri ortamınızı seçin.

  ![Zaman Serisi Görüşleri kaynağını yönetme - ortam](media/data-access/getstarted-grant-data-access1.png)

4.  “Veri Düzlemi Erişimi” öğesini seçin, “Ekle” düğmesine tıklayın

  ![Zaman Serisi Görüşleri kaynağını yönetme - ekleme](media/data-access/getstarted-grant-data-access2.png)

5.  "Kullanıcı seç" öğesine tıklayın.
6.  Kullanıcıyı e-postaya göre arayın ve seçin.
7.  “Kullanıcı Seç” dikey penceresinde “Seç” düğmesine tıklayın.

  ![Zaman Serisi Görüşleri kaynağını yönetme - kullanıcı seçme](media/data-access/getstarted-grant-data-access3.png)

8.  “Rol seç” öğesine tıklayın.
9.  Kullanıcıya başvuru verilerini değiştirme ve kaydedilmiş sorgularla perspektifleri ortamdaki diğer kullanıcılarla paylaşma izni vermek istiyorsanız, “Katılımcı” öğesini seçin. Aksi takdirde, kullanıcının ortamdaki verileri sorgulamasına ve kişisel (paylaşılmayan) sorguları ortama kaydetmesine izin vermek için “Okuyucu” öğesini seçin.
10. “Rol Seç” dikey penceresinde “Tamam” düğmesine tıklayın.

  ![Zaman Serisi Görüşleri kaynağını yönetme - rol seçme](media/data-access/getstarted-grant-data-access4.png)

11. “Kullanıcı Rolü Seç” dikey penceresinde “Tamam” düğmesine tıklayın.
12. Şunu görmeniz gerekir:

  ![Zaman Serisi Görüşleri kaynağını yönetme - sonuçlar](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Olay kaynağı oluşturma](time-series-insights-add-event-source.md)
* Olay kaynağına [olayları gönderme](time-series-insights-send-events.md)
* [Zaman Serisi Görüşleri Portalı](https://insights.timeseries.azure.com)’nda ortamınızı görüntüleme
