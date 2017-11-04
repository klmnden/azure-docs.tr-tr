---
title: "İş ortağı çözümleri Azure Güvenlik Merkezi'nde bağlı yönetme | Microsoft Docs"
description: "Bu belge, Azure Güvenlik Merkezi'nin Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta nasıl izleyeceğiniz konusunda size rehberlik sağlar."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: terrylan
ms.openlocfilehash: 181e1e00716987732ee809df6171c2f71087f3e1
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Azure Güvenlik Merkezi ile bağlı iş ortağı çözümlerini yönetme
Bu makalede, Azure Güvenlik Merkezi'nde bağlı güvenlik çözümleri izlemek ve yönetmek nasıl açıklanmaktadır.

## <a name="monitoring-partner-solutions"></a>İş ortağı çözümlerini izleme
Bağlı güvenlik çözümleri sistem durumunu ve temel yönetim gerçekleştirmek için:

1. Altında **Güvenlik Merkezi - genel bakış**seçin **güvenlik çözümleri**.

  ![Güvenlik çözümleri seçin][1]

  **Bağlı çözümleri** bölüm Güvenlik Merkezi ve her çözümün sistem durumu hakkında bilgi için bağlı güvenlik çözümleri içerir.

  ![İş ortağı çözümleri][2]

   Bir iş ortağı çözümünün durumunu olabilir:

   * Sağlıklı (yeşil) - sistem durumu sorun yoktur.
   * Sağlıksız (kırmızı) - sistem durumunda hemen ilgilenilmesi gereken bir sorun var.
   * Sistem durumu sorunları (turuncu) - çözüm, sistem durumunu raporlamayı durdurdu.
   * (Gri) - bildirmemiş çözümü herhangi bir şey bildirilmedi henüz çözümün durumu yakın zamanda bağlanmış ve hala ya da sistem durumu veri yok bildirilmeyebilir.

   > [!NOTE]
   > Sistem Durumu verileri kullanılabilir durumda değilse, Güvenlik Merkezi çözümü veya Raporlama belirtmek için alınan son olay saat ve tarihi gösterir. Sistem durumu veri yok ve hiçbir uyarı son 14 gün içinde alınan, Güvenlik Merkezi çözümü sağlıksız veya değil raporlama olduğunu gösterir.
   >
   >

2. Seçin **Görünüm** ek bilgileri veya seçenekleri için içerir:

  - **Çözüm konsolunu**. Bu çözüm için yönetim deneyimi açar.
  - **Bağlantı VM**. Bağlantı uygulamaları dikey pencere açılır. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
  - **Çözümü silmek**.
  - **Yapılandırma**.

   ![İş ortağı çözümü ayrıntısı][3]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yönetme ve bağlı güvenlik çözümleri Güvenlik Merkezi'ndeki izleme öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Güvenlik çözümleri genel bakış](security-center-partner-integration.md) — bağlanmak ve güvenlik çözümleri yönetmek öğrenin.
* [Microsoft Advanced Threat Analytics (ATA) bağlanma](security-center-ata-integration.md) — den ATA uyarıları bağlayacağınızı öğrenin.
* [Azure Active Directory (AD) kimlik koruması bağlanma ](security-center-aadip-integration.md) — Azure AD Identity Protection uyarılar bağlayacağınızı öğrenin.
* [İş ortakları ve çözümleri tümleştirme](security-center-partner-integration.md) -diğer güvenlik çözümleri tümleştirme genel bakış alın.
* [Yönetme ve güvenlik uyarılarını yanıt](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
