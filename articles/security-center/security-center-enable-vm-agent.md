---
title: Azure Güvenlik Merkezi'nde VM aracısını etkinleştirin | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **VM aracısını etkinleştirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5b431c25-4241-45b7-9556-cf2a1956f3da
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 12934ad53050d16b89dd5b4175ca19a24d1ec4d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60916302"
---
# <a name="enable-vm-agent-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde VM aracısını etkinleştirin
VM Aracısı sanal makinelerinde (VM'ler) için yüklenmelidir [veri toplama özelliğini etkinleştirmeyi](security-center-enable-data-collection.md).  Azure Güvenlik Merkezi, hangi VM'lerin VM Aracısı gerektirir ve bu vm'lerdeki VM Aracısı etkinleştirmenizi önerir görmenize olanak sağlar.

VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. [VM Aracısı ve Uzantılar – 2. Kısım](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) makalesinde VM Aracısının nasıl yüklendiğine ilişkin bilgiler verilmektedir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **öneriler dikey**seçin **VM aracısını etkinleştirme**.
   ![VM Aracısını etkinleştirin][1]
2. Bu dikey pencereyi açar **VM Aracısı eksik veya yanıt vermiyor**. Bu dikey pencere, VM Aracısı gerektiren Vm'leri listeler. Dikey penceresinde VM Aracısı'nı yüklemek için yönergeleri izleyin.
   ![VM Aracısı eksik][2]

## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
