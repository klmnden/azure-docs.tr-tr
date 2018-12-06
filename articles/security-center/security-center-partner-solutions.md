---
title: İş ortağı çözümlerini Azure Güvenlik Merkezi'ne bağlı yönetme | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nin Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta nasıl izleyeceğiniz konusunda size rehberlik sağlar.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/20/2018
ms.author: rkarlin
ms.openlocfilehash: 27a8abe0008c0b9c3854ea663e1c0fb3b55cfc30
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964552"
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Azure Güvenlik Merkezi ile bağlı iş ortağı çözümlerini yönetme
Bu makalede, yönetme ve bağlı güvenlik çözümlerini Azure Güvenlik Merkezi'nde izleme konusunda yol göstermektedir.

## <a name="monitoring-partner-solutions"></a>İş ortağı çözümlerini izleme
Bağlı güvenlik çözümlerinin sistem durumunu izleyin ve temel yönetim gerçekleştirmek için:

1. Altında **Güvenlik Merkezi - genel bakış**seçin **güvenlik çözümlerini**.

  ![Güvenlik çözümlerini seçin][1]

  **Bağlantılı çözümler** bölümünde Güvenlik Merkezi ve her çözümün sistem durumu hakkında bilgi için bağlı olan güvenlik çözümlerini içerir.

  ![İş ortağı çözümleri][2]

   Bir iş ortağı çözümünün durumunu olabilir:

   * Sağlıklı (yeşil) - hiçbir sistem durumu sorunu yok.
   * Sağlıksız (kırmızı) - sistem durumunda hemen ilgilenilmesi gereken bir sorun var.
   * Sistem durumu sorunları (turuncu) - çözüm, sistem durumunu raporlamayı durdurdu.
   * Bildirilmedi (gri) - çözüm herhangi bir şey bildirilmedi henüz yakın zamanda bağlanmış ve hala veya sistem veri yok, çözümün durumu bildirilmeyen olabilir.

   > [!NOTE]
   > Güvenlik Merkezi, sistem durumu verilerini kullanılabilir durumda değilse, çözüm veya rapor bildirmek üzere alınan son olayın saat ve tarihi gösterir. Son 14 gün içinde alınan hiçbir uyarı ve sistem durumu veri yok, Güvenlik Merkezi, sağlıksız veya değil raporlama çözümüdür gösterir.
   >
   >

2. Seçin **görünümü** ek bilgileri veya seçenekleri için içerir:

  - **Çözüm Konsolu**. Bu çözüm için yönetim deneyimi açar.
  - **Bağlantı VM**. Bağlantı uygulamalar dikey penceresi açılır. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
  - **Çözümü Sil**.
  - **Yapılandırma**.

   ![İş ortağı çözümü ayrıntısı][3]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, yönetme ve izleme Güvenlik Merkezi'nde bağlı güvenlik çözümlerini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Güvenlik çözümlerine genel bakış](security-center-partner-integration.md) — bağlanmak ve güvenlik çözümlerini yönetme hakkında bilgi edinin.
* [Microsoft Advanced Threat Analytics (ATA) bağlanma](security-center-ata-integration.md) — uyarıları ATA'dan bağlanmayı öğreneceksiniz.
* [Azure Active Directory (AD) kimlik koruması bağlantısını ](security-center-aadip-integration.md) — Azure AD kimlik koruması uyarıları bağlanmayı öğreneceksiniz.
* [İş ortağı ve çözüm tümleştirmesi](security-center-partner-integration.md) -diğer güvenlik çözümlerini tümleştirme genel bakış edinin.
* [Yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) — yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
