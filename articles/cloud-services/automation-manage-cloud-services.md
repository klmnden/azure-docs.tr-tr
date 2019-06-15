---
title: Azure Cloud Services, Azure otomasyonu kullanarak yönetme | Microsoft Docs
description: Nasıl Azure Otomasyonu Azure bulut hizmetlerini uygun ölçekte yönetmek için kullanılabilir hakkında bilgi edinin.
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
ms.openlocfilehash: b3660901c86dd644369e6d1913e825cbd5ea316b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60623213"
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure Cloud Services, Azure Otomasyonu ile yönetme
Bu kılavuzda Azure Otomasyonu ve nasıl bunu Azure bulut hizmetlerinizde yönetimini basitleştirmek için kullanılabileceğini başlatacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) bulut yönetimini işlem Otomasyonu ile basitleştirmeyi için bir Azure hizmetidir. Azure otomasyonu kullanarak, güvenilirlik, verimlilik ve kuruluşunuz için değer süresini artırmak için uzun süre çalışan, el ile hata yapmaya açık ve sık sık tekrarlanan görevleri otomatikleştirilebilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe, gereksinimlerinizi karşılayacak şekilde ölçeklendirilen bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Görevleri tam olarak gerekli olduğunda gerçekleşir. böylece Azure Automation'da işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla çalıştırmasının.

Alt işlemsel yük ve boş BT / iş kazandıran işe odaklanmak için DevOps personeli, Azure Otomasyonu tarafından otomatik olarak çalıştırılmak için bulut Yönetimi görevlerinizi taşıyarak değer.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Azure Otomasyonu Azure bulut hizmetlerini yönetmenize nasıl yardımcı olabilir?
Azure bulut Hizmetleri yönetilen Azure Automation'da kullanılabilen PowerShell cmdlet'lerini kullanarak [Azure PowerShell Araçları](/powershell/). Azure Otomasyonu bu bulut hizmeti PowerShell cmdlet'leri kullanılabilir yepyeni sahiptir; böylece tüm hizmet içinde bulut hizmeti yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca, Azure automation'da bu cmdlet'ler Azure Hizmetleri ve 3. taraf sistemleri arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri eşleşebileceğini denetleyebilmesi.

Azure Otomasyonu, Azure bulut hizmetlerini yönetmek için bazı örnek kullanımları şunlardır:

* [Azure Blob Depolama alanında cscfg veya cspkg güncelleştirildiğinde bir bulut hizmetinin sürekli dağıtım](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Bulut hizmeti örnekleri paralel olarak bir yükseltme etki alanı aynı anda yeniden başlatılıyor](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ve nasıl Azure bulut hizmetlerini yönetmek için kullanılmadan ilişkin temel bilgileri öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi için bu bağlantıları izleyin.

* [Azure Otomasyonu'na genel bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
* [Azure Otomasyonu öğrenme Haritası](https://azure.microsoft.com/documentation/learning-paths/automation/)
