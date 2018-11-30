---
title: Azure Güvenlik Merkezi'nde güvenlik kişi ayrıntılarını sağlama | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik kişi ayrıntılarını sağlama işlemini göstermektedir.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/20/2018
ms.author: terrylan
ms.openlocfilehash: 530bde33035d6e702c15d2f4efbe9c97a77bb855
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52306089"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik kişi ayrıntılarını sağlama
Azure Güvenlik Merkezi, henüz yapmadıysanız, Azure aboneliğiniz için güvenlik kişi ayrıntılarını sağlama önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. MSRC select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve Üçüncü taraflardan tehdit zekası ve kötüye kullanımı şikayetlerinin alır.

İlk günlük örneğinde bir uyarı ve yalnızca yüksek önem düzeyindeki uyarılar için e-posta bildirimi gönderilir. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. Bir abonelik içindeki kaynak grupları, bu ayarları devralır.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. Altında **önerileri**seçin **güvenlik kişi ayrıntılarını sağlama**.
   ![Güvenlik ilgili kişi sağlayın][1]
2. Şirket iletişim bilgilerini sağlamak için Azure aboneliği seçin.
3. Bu açılır **e-posta bildirimleri**.

   ![Güvenlik ilgili kişi bilgilerini belirtin][2]

   * Güvenlik ilgili kişi e-posta adresini veya adreslerini noktalı virgüllerle ayırarak girin. Girdiğiniz e-posta adresleri sayısına bir sınır yoktur.
   * Bir güvenlik ilgili kişi uluslararası telefon numarası girin.
   * Yüksek önem düzeyindeki uyarılar hakkında e-posta almak için seçeneğini açın **Gönder e-posta uyarıları hakkında**.
   * Gelecekte, abonelik sahiplerine e-posta bildirimleri göndermek için seçeneğine sahip olursunuz. Bu seçenek, gri renkte gösterilir.
   * Seçin **Kaydet** aboneliğiniz için güvenlik bilgilerini uygulamak için.

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-azure-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
