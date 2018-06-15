---
title: Azure otomasyonu kullanarak Azure Cloud Services yönetme | Microsoft Docs
description: Nasıl Azure Otomasyon hizmetine ölçekte Azure bulut hizmetlerini yönetmek için kullanılabilir hakkında bilgi edinin.
services: cloud-services, automation
documentationcenter: ''
author: jodoglevy
manager: timlt
editor: ''
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 6b5acac1b8647c324988c316cd5602b3dba98a1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23842940"
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure otomasyonu kullanarak Azure Cloud Services yönetme
Bu kılavuz Azure Otomasyon hizmetine ve nasıl Azure bulut hizmetlerinizi yönetimini basitleştirmek için kullanılabilmesi için tanıtılacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) işlem Otomasyonu aracılığıyla bulut yönetimi basitleştirmek için bir Azure hizmetidir. Azure otomasyonu kullanarak, uzun süre çalışan, el ile hatasız ve sık tekrarlanan görevleri güvenilirlik, verimliliği ve kuruluşunuz için değer süre artırmak için otomatik olarak yapılabilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçekler bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Tam olarak gerekli görevleri ortaya böylece Azure Otomasyonu'nda işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla artırılabilir devre dışı.

İşletimsel ek yükü azaltmak ve boşaltmak BT / iş ekler çalışmaları odaklanmaya DevOps personel değer Azure Automation'ın otomatik olarak çalıştırılmak üzere bulut yönetim görevlerinizi taşıyarak.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Azure Otomasyonu Azure bulut hizmetlerini yönetmenize nasıl yardımcı olabilir?
Azure bulut hizmetlerine yönetilebilir Azure Otomasyonu'nda kullanılabilir PowerShell cmdlet'lerini kullanarak [Azure PowerShell Araçları](https://msdn.microsoft.com/library/azure/jj156055.aspx). Böylece tüm hizmet içinde bulut Hizmeti Yönetim görevlerinizi gerçekleştirmek azure Otomasyonu kutudan çıktığında, bu bulut hizmeti PowerShell cmdlet'leri kullanılabilir olur. Ayrıca Azure Automation bu cmdlet'leri Azure hizmeti ve 3. taraf sistemi arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri ile eşleştirin.

Azure Otomasyonu, Azure bulut hizmetlerini yönetmek için bazı örnek kullanımları şunları içerir:

* [Bir bulut cscfg veya cspkg Azure Blob depolama alanına güncelleştirildiğinde hizmetin sürekli dağıtımı](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Bulut hizmeti örnekleri paralel, bir seferde bir yükseltme etki alanı yeniden başlatılıyor](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ve nasıl Azure bulut hizmetlerini yönetmek için kullanılmadan öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Azure Otomasyonu genel bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
* [Azure Otomasyonu öğrenme Haritası](https://azure.microsoft.com/documentation/learning-paths/automation/)
