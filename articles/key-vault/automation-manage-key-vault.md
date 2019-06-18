---
title: Azure Otomasyonu - Azure Key Vault kullanarak Azure Key Vault yönetme | Microsoft Docs
description: Nasıl Azure Otomasyonu Azure anahtar Kasası'nı yönetmek için kullanılabilir hakkında bilgi edinin.
services: Key-Vault, automation
author: mgoedtel
manager: jwhit
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: magoedte
ms.openlocfilehash: 87fdb565f47e7d88342c82bd5940c1ddb9c137e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712105"
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Azure anahtar Kasası'nın Azure Otomasyonu ile yönetme

Bu kılavuzda, Azure Otomasyonu ve nasıl anahtarları ve gizli anahtarları Azure Key vault'ta yönetimini basitleştirmek için kullanılabilmesi için başlatacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?

[Azure Otomasyonu](../automation/automation-intro.md) süreç otomasyonu ve istenen durum yapılandırması ile bulut yönetimini basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu, el ile kullanarak tekrarlanan, uzun süre çalışan ve hata yapmaya açık görevleri güvenilirlik, verimlilik ve kuruluşunuz için değer süresini artırmak için otomatik olarak yapılabilir.

Azure Otomasyonu ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilen bir yüksek oranda güvenilir, yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Görevleri tam olarak gerekli olduğunda gerçekleşir. böylece Azure Automation'da işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla çalıştırmasının.

İşlemsel yükünü azaltır ve boş BT ve DevOps personelinin Azure Otomasyonu tarafından otomatik olarak çalıştırılmak için bulut Yönetimi görevlerinizi taşıyarak iş değer kazandıran işlere odaklanın.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Azure Otomasyonu Azure anahtar kasası yönetmenize nasıl yardımcı olabilir?

Key Vault yönetilen Azure Automation'da kullanarak [AzureRM Key Vault cmdlet'leri](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) ve [Klasik Azure anahtar kasası cmdlet'leri](https://docs.microsoft.com/powershell/module/servicemanagement/azure). Klasik anahtar Kasası'nı yönetmek için Azure modülü, Azure Automation'da otomatik olarak kullanılabilir ve içeri aktarabilirsiniz [KeyVault AzureRM Modülü](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) içine Azure Otomasyonu, böylece, anahtar kasası yönetim görevlerin çoğunu gerçekleştirebilirsiniz içinde hizmet. Modülü Azure Automation'a içeri aktarma hakkında daha fazla bilgi için bkz. [modül Azure automation'da Yönetimi](../automation/shared-resources/modules.md) cmdlet'ler arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri ile Azure Otomasyonu'nda bu cmdlet'leri ayrıca eşleşebileceğini denetleyebilmesi Azure Hizmetleri ve 3. taraf sistemleri.

Azure Key Vault cmdlet'leri ile diğerlerinin yanı sıra bu görevleri gerçekleştirebilirsiniz: 

* Oluşturma ve bir anahtar Kasası'nı yapılandırma
* Oluşturma veya bir anahtarı içeri aktarma
* Oluşturma veya gizli anahtarı güncelleştirme
* Bir anahtarı güncelleştirme öznitelikleri
* Bir anahtar veya gizli anahtarı alma
* Bir anahtar veya gizli anahtarı silme

Anahtar Kasası'nı yönetmek için PowerShell kullanmanın bazı örnekleri aşağıda verilmiştir:  

* [Azure anahtar kasası - adım adım](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Ayarlama ve bir Azure anahtar Kasası'nı yapılandırma](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu ve nasıl Azure anahtar Kasası'nı yönetmek için kullanılmadan ilişkin temel bilgileri öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi için bu bağlantıları izleyin.

* Azure Otomasyonu bkz [çalışmaya başlama Öğreticisi](../automation/automation-first-runbook-graphical.md).
* Bkz: [Azure anahtar kasası PowerShell betikleri](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

