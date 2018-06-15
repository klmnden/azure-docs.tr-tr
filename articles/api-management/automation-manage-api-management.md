---
title: Azure otomasyonu kullanarak Azure API Management'te yönetme
description: Nasıl Azure Otomasyon hizmetine Azure API Management'ı yönetmek için kullanılabilir hakkında bilgi edinin.
services: api-management, automation
documentationcenter: ''
author: vladvino
manager: eamono
editor: ''
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 10b483c70f7b5a3d767815306d8a690b1b9a5faf
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30161855"
---
# <a name="managing-azure-api-management-using-azure-automation"></a>Azure otomasyonu kullanarak Azure API Management'te yönetme
Bu kılavuz size Azure Otomasyon hizmetine ve nasıl Azure API Management yönetimini basitleştirmek için kullanılabilmesi için tanıtır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) işlem Otomasyonu aracılığıyla bulut yönetimi basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu, el ile kullanarak yinelenen, uzun süre çalışan ve hataya yatkın görevleri güvenilirlik, verimliliği ve kuruluşunuz için değer süre artırmak için otomatik olarak yapılabilir.

Azure Otomasyon gereksinimlerinizi karşılayacak şekilde ölçekler bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Tam olarak gerekli görevleri ortaya böylece Azure Otomasyonu'nda işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla artırılabilir devre dışı.

İşlem yükünü azaltmak ve boş BT ve iş değeri Azure Automation'ın otomatik olarak çalıştırılmak üzere bulut yönetim görevlerinizi taşıyarak ekler iş odaklanmak için DevOps personeli.

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a>Azure Otomasyonu Azure API Management yönetmenize nasıl yardımcı olabilir?
API Management yönetilebilir Azure Otomasyonu'nda kullanarak [Azure API Yönetimi API için Windows PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/?view=azurermps-5.5.0#api_management/). Azure Otomasyonu cmdlet'leri kullanarak API Management görevlerinizi çoğunu gerçekleştirmek için PowerShell iş akışı komut dosyalarını yazabilirsiniz. Ayrıca Azure Automation bu cmdlet'leri Azure hizmeti ve 3. taraf sistemi arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri ile eşleştirin.

API Management ile otomasyon kullanma bazı örnekleri şunlardır:

* [Azure API Management – yedekleme ve geri yükleme için PowerShell kullanma](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ve nasıl Azure API Management yönetmek için kullanılmadan öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Azure Otomasyonu bkz [başlama Öğreticisi](../automation/automation-first-runbook-graphical.md).

