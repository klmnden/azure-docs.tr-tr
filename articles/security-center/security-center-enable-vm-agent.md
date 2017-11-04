---
title: "Azure Güvenlik Merkezi'nde VM Aracısı etkinleştirme | Microsoft Docs"
description: "Bu belgede Azure Güvenlik Merkezi öneriyi uygulamayı gösterilmiştir ** etkinleştirmek VM Aracısı **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 337a7adfd93c76882a749685702bea6d1524c96a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde VM aracısını etkinleştir
Aşağıdakileri yapmak için sanal makineleri (VM'ler) üzerinde VM Aracısı yüklenmelidir [veri toplamayı etkinleştirmek](security-center-enable-data-collection.md).  Azure Güvenlik Merkezi, hangi VM'ler VM Aracısı gerektirir ve bu vm'lerde VM Aracısı etkinleştirmenizi öneririz görmenize olanak sağlar.

VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulamayı
1. İçinde **öneriler dikey**seçin **etkinleştirmek VM Aracısı**.
   ![VM Aracısını etkinleştirin][1]
2. Bu dikey pencere açılır **VM Aracısı eksik veya yanıt vermiyor**. Bu dikey VM Aracısı gerektiren sanal makineleri listeler. Dikey VM Aracısı'nı yüklemek için yönergeleri izleyin.
   ![VM Aracısı eksik.][2]

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
