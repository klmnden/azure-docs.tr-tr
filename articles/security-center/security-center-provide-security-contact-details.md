---
title: "Azure Güvenlik Merkezi'nde güvenlik iletişim ayrıntılarını sağlamak | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi'nde güvenlik kişi ayrıntıları gösterilmiştir."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: terrylan
ms.openlocfilehash: 726b59c45e2eb18eebe28a180db23336ae141408
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik iletişim ayrıntılarını sağlayın
Azure Güvenlik Merkezi, henüz yapmadıysanız, Azure aboneliğiniz için güvenlik iletişim ayrıntılarını sağlayın önerir. Bu bilgiler, Microsoft Güvenlik Yanıt Merkezi (MSRC) müşteri verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. MSRC select güvenlik Azure ağ ve altyapı izleme gerçekleştirir ve Üçüncü taraflardan tehdit Intelligence ve kötüye şikayetlerinden alır.

İlk günlük oluşması bir uyarı ve yüksek öneme sahip uyarılar için yalnızca bir e-posta bildirimi gönderilir. E-posta tercihleri yalnızca abonelik ilkeleri için yapılandırılabilir. Bir abonelik içindeki kaynak grupları, bu ayarları devralır.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. Altında **önerileri**seçin **güvenlik iletişim ayrıntılarını sağlamak**.
   ![Güvenlik kişi sağlayın][1]
2. Kişi bilgileri sağlamak için Azure aboneliğini seçin.
3. Bu açılır **güvenlik ilkesi - e-posta bildirimleri**.

   ![Güvenlik ilgili kişi bilgilerini belirtin][2]

   * Güvenlik iletişim e-posta adresini veya adreslerini noktalı virgüllerle ayırarak girin. Girdiğiniz e-posta adresi sayısı için bir sınır değil.
   * Bir güvenlik kişi uluslararası telefon numarası girin.
   * Yüksek öneme sahip uyarılar hakkında e-postaları için seçeneğini açın **bana Gönder e-postalar uyarılar hakkında**.
   * Gelecekte, abonelik sahipleri için e-posta bildirimleri gönderme seçeneği gerekir. Bu seçenek, gri renkte görüntülenir.
   * Seçin **kaydetmek** aboneliğinize güvenlik bilgilerini uygulanacak.

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --nasıl önerilerin Azure kaynaklarınızı korumanıza yardımcı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) --Azure kaynaklarınızı sağlığını izlemek öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --en son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
