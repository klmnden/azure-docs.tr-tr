---
title: Azure Güvenlik Merkezi'nde öneriler kapsayıcı | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerilerini kapsayıcılarınızı korunmasına nasıl yardımcı olacağını açıklar.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 2e76c7f7-a3dd-4d9f-add9-7e0e10e9324d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: 782c769bc7825dc9b6bd3ba3b8e36885bf150eaa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60705296"
---
# <a name="understand-azure-security-center-container-recommendations"></a>Azure Güvenlik Merkezi kapsayıcı önerilerini anlama

Kritik çalıştırmak için tek uygulamalarınızı geçiş yaparken, kapsayıcılı bulutta yerel uygulamaları üretim kapsayıcılar, kolay ve hızlı dağıtım ve güncelleştirme gibi özelliklerinden yararlanabilirsiniz. Dağıtılan kapsayıcıları sayısını artırmak devam ettikçe güvenlik çözümlerini güvenlik durumuna ilişkin kapsayıcılarınızı görünürlük sağlar ve bunları tehditlere karşı korumaya yardımcı olmak için yapılması gerekir.

Azure Güvenlik Merkezi, kapsayıcıları güvenli hale getirmeye yardımcı olacak aşağıdaki özellikleri sağlar:

- **Iaas Linux makinelerinde barındırılan kapsayıcılar görünürlük**<br>Azure Güvenlik Merkezi'nde, Docker ile dağıtılan tüm sanal makineleri kapsayıcıları sekmesini görüntüler. Güvenlik Merkezi, bir sanal makine üzerinde güvenlik sorunlarını keşfetmek, Docker sürümü gibi makine ve ana bilgisayarda çalışan görüntülerinin sayısını kapsayıcılarında ilgili ek bilgiler sağlar.

    ![kapsayıcı sekmesi](./media/security-center-container-recommendations/docker-recommendation.png)


- **Docker için CIS Kıyaslama dayalı güvenlik önerileri**<br>Güvenlik Merkezi, Docker yapılandırmaları tarar ve değerlendirilen tüm başarısız kuralların listesi sağlayarak, yanlış yapılandırmalarını görünürlük sağlar. Güvenlik Merkezi, zamandan tasarruf edin ve bu sorunların hızla çözülmesine yardımcı olacak yönergeler sağlar. Güvenlik Merkezi, sürekli olarak Docker yapılandırmaları değerlendirir ve son durumlarını sağlar.

    ![kapsayıcı sekmesi](./media/security-center-container-recommendations/container-cis-benchmark.png)

- **Gerçek zamanlı kapsayıcı tehdit algılama**<br> Güvenlik Merkezi, gerçek zamanlı algılama için kapsayıcılarınızı AuditD bileşeni ile Linux makinelerinde sağlar. Konak, bir Docker kapsayıcısı ya da şifreleme madencilerinin kullanımını içinde çalışan güvenli Kabuk (SSH) sunucusunun göstergesidir ayrıcalıklı bir kapsayıcı oluşturma gibi birkaç şüpheli Docker etkinlik uyarıları belirleyin. Hızlı güvenlik sorunlarını düzeltmesine ve kapsayıcılarınızı güvenliğini artırmak için bu bilgileri kullanabilirsiniz.

    ![kapsayıcı sekmesi](./media/security-center-container-recommendations/docker-threat-detection.png)

## <a name="recommendations"></a>Öneriler
Aşağıdaki tablolara, Iaas Linux makineleri ve Docker yapılandırmalarına güvenlik değerlendirmesini üzerinde barındırılan kullanılabilir kapsayıcıları anlamanıza yardımcı olması için bir başvuru olarak kullanın.

| Öneri | Açıklama | Düzeltme |
| --- | --- | --- |
|Kapsayıcı güvenlik yapılandırmalarındaki güvenlik açıklarını düzeltin |Kapsayıcı güvenlik yapılandırmalarını yapılandırma en iyi uygulamalarına göre güvenlik açıklarını düzeltin.| Kapsayıcı güvenlik yapılandırmalarını güvenlik açıklarını düzeltme için:<br>1. Başarısız kurallar listesini gözden geçirin.<br>2. Her bir kural belirtilen yönergelere göre düzeltin.|


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure Güvenlik Merkezi'nde Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.

