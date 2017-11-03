---
title: "Azure anahtar Kasası'nın Azure otomasyonu kullanarak yönetme | Microsoft Docs"
description: "Nasıl Azure Otomasyon hizmetine Azure anahtar kasası yönetmek için kullanılabilir hakkında bilgi edinin."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Azure anahtar Kasası'nın Azure otomasyonu kullanarak yönetme
Bu kılavuz Azure Otomasyon hizmetine ve nasıl Bu anahtarları ve gizli anahtarları Azure anahtar Kasası'nda yönetimini kolaylaştırmak için kullanılabilir tanıtılacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](../automation/automation-intro.md) işlem Otomasyonu ve istenen durum yapılandırması aracılığıyla bulut yönetimi basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu, el ile kullanarak yinelenen, uzun süre çalışan ve hataya yatkın görevleri güvenilirlik, verimliliği ve kuruluşunuz için değer süre artırmak için otomatik olarak yapılabilir.

Azure Otomasyon gereksinimlerinizi karşılayacak şekilde ölçekler bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Tam olarak gerekli görevleri ortaya böylece Azure Otomasyonu'nda işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla artırılabilir devre dışı.

İşlem yükünü azaltmak ve boş BT ve iş değeri Azure Automation'ın otomatik olarak çalıştırılmak üzere bulut yönetim görevlerinizi taşıyarak ekler iş odaklanmak için DevOps personeli.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Azure Otomasyonu Azure anahtar kasası yönetmenize nasıl yardımcı olabilir?
Anahtar kasası yönetilebilir Azure Otomasyonu'nda kullanarak [AzureRM anahtar kasası cmdlet'leri](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) ve [Azure Klasik anahtar kasası cmdlet'leri](https://msdn.microsoft.com/library/azure/dn868052.aspx). Klasik anahtar kasası yönetmek için Azure modülü otomatik olarak Azure Otomasyonu'nda kullanılabilir ve içe aktarabilirsiniz [AzureRM KeyVault Modülü](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) içine Azure Otomasyonu, böylece anahtar kasası yönetim görevlerinizi hizmet içinde çoğunu gerçekleştirebilirsiniz. Ayrıca Azure Automation bu cmdlet'leri Azure hizmeti ve 3. taraf sistemi arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri ile eşleştirin.

Azure anahtar kasası cmdlet ile birlikte, diğerlerinin yanı sıra bu görevleri gerçekleştirebilirsiniz: 

* Oluşturma ve bir anahtar kasası yapılandırma
* Oluşturun veya bir anahtarı içeri aktarın
* Bir parolayı güncelle
* Bir anahtarı güncelleştirme öznitelikleri
* Bir anahtar veya gizli Al
* Bir anahtar veya gizli Sil

Anahtar kasası yönetmek için PowerShell kullanma ilişkin bazı örnekler şunlardır:  

* [Azure anahtar kasası - adım adım](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Ayarlama ve Azure anahtar kasası yapılandırma](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu ve nasıl Azure anahtar kasası yönetmek için kullanılmadan öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Azure Otomasyonu bkz [başlama Öğreticisi](../automation/automation-first-runbook-graphical.md).
* Bkz: [Azure anahtar kasası PowerShell komut dosyalarını](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

