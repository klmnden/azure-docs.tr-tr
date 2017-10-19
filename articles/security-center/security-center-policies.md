---
title: "Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama | Microsoft Belgeleri"
description: "Bu belge, Azure Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: 67564e930310433bf4d51f7642bdd7ebf7e8e600
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama
Bu belge, bu görevi gerçekleştirmeye ilişkin gerekli adımlarda size kılavuzluk ederek Güvenlik Merkezi'nde güvenlik ilkelerini yapılandırmanıza yardımcı olur.


## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. İlkeyi Güvenlik Merkezi'nde düzenleyebilir veya [Azure İlkesi](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)'ni kullanarak yeni tanım oluşturabilir, ek ilke tanımlayabilir, ilkeleri farklı Yönetim Gruplarına (kuruluşun tamamı, tek bir iş birimi vs. olabilir) atayabilir ve bu kapsamlarda bu ilkelere uyum sağlanıp sağlanmadığını izleyebilirsiniz.

> [!NOTE]
> Azure İlkesi sınırlı önizleme aşamasındadır. Katılmak için [buraya](https://aka.ms/getpolicy) tıklayın. Azure İlkeleri hakkında daha fazla bilgi için bkz. [Uyumluluğu zorlamak için ilke oluşturma ve yönetme](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy).

## <a name="how-to-change-security-policies-in-security-center"></a>Güvenlik Merkezi'ndeki güvenlik ilkeleri nasıl değiştirilir?
Güvenlik Merkezi'nde tüm Azure aboneliklerinizin varsayılan güvenlik ilkesini düzenleyebilirsiniz. Bir güvenlik ilkesini değiştirmek için abonelikte veya ilkeyi içeren Yönetim Grubunda sahip, katkıda bulunan veya Güvenlik Yöneticisi rolüne sahip olmanız gerekir. Azure portalında oturum açın ve aşağıdaki adımları izleyerek Güvenlik Merkezi'nde güvenlik ilkelerini görüntüleyin:

1. **Güvenlik Merkezi** panosunun **Genel** bölümünde **Güvenlik İlkesi**'ne tıklayın.
2. Güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin.

    ![İlke Yönetimi](./media/security-center-policies/security-center-policies-fig10.png)

3. **İLKE BİLEŞENLERİ** bölümünde **Güvenlik ilkesi**'ne tıklayın.

    ![İlke bileşenleri](./media/security-center-policies/security-center-policies-fig12.png)

4. Bu, Azure İlkesi aracılığıyla Güvenlik Merkezi'ne atanmış olan varsayılan ilkedir. **İLKELER VE PARAMETRELER** altındaki öğeleri silebilir veya **KULLANILABİLİR SEÇENEKLER** altına başka ilke tanımları ekleyebilirsiniz. Bunun için tanım adının yanındaki artı işaretine tıklamanız yeterlidir.

    ![İlke tanımları](./media/security-center-policies/security-center-policies-fig11.png)

5. İlke hakkında daha ayrıntılı bir açıklamaya ihtiyacınız varsa üzerine tıklayarak ayrıntıları ve [ilke tanımını(https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy#policy-definition-structure) structure: içeren JSON kodunun bulunduğu sayfanın açılmasını sağlayabilirsiniz:

    ![Json](./media/security-center-policies/security-center-policies-fig13.png)

6. Düzenlemeyi tamamladığınızda **Kaydet**'e tıklayın.

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md). Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](security-center-managing-and-responding-alerts.md). Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md). İş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](http://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
