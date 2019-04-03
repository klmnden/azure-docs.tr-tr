---
title: Azure Otomasyonu'nda az modüllerini kullanma
description: Bu makale Azure Automation'da Az modüllerini kullanarak bilgi sağlar
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 02/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a076c924d57aadfae477a5df0d128aad8e67af60
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862735"
---
# <a name="az-module-support-in-azure-automation"></a>Azure automation'da az Modülü Desteği

Azure Otomasyonu kullanma yeteneğini destekleyen [Az Azure Powershell Modülü](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) larınızdaki. Az modül tüm yeni veya var olan Otomasyon hesaplarını otomatik olarak alınmaz. Bu makalede, Azure Otomasyonu ile Az modülleri kullanma açıklanmaktadır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Az modül Azure Automation'da kullanırken dikkate gereken birçok şey vardır. Runbook'ları ve modüller Otomasyon hesabınızdaki daha yüksek düzeyli çözüm tarafından kullanılabilir. Olası sorunları runbook'ları düzenleme veya modülleri yükseltme ile runbook'larınızı neden olabilir. Tüm runbook'ları ve çözümler dikkatle ayrı bir Otomasyon hesabında yeni almadan önce test etmelisiniz `Az` modüller. Modüller için herhangi bir değişiklik, güncelleştirme yönetimi ve mesai saatleri dışında başlatma/durdurma Vm'leri gibi daha üst düzey çözüm olumsuz yönde etkileyebilir. Modüllerini ve runbook'ları içeren herhangi bir çözüm Automation hesaplarında değişiklik önerilir. Bu davranış Az modüllerine özgü değildir. Bu davranış, Otomasyon hesabınıza herhangi bir değişiklik uygulandığında dikkate edilmelidir.

İçeri aktarma bir `Az` modülü Otomasyon hesabınızdaki değil otomatik olarak içeri aktarma modülü PowerShell oturumunda runbook'ları kullanın. Modüller aşağıdaki durumlarda PowerShell oturumuna içeri aktarılır:

* Bir runbook'tan bir modülden bir cmdlet olduğunda çağrılır
* Ne zaman bir runbook'u içeri aktarır açıkça ile `Import-Module` cmdlet'i
* Başka bir modül modülü bağlı olarak bir PowerShell oturumuna aktarıldığında

> [!IMPORTANT]
> Bu runbook'ları bir Otomasyon hesabı ya da yalnızca içeri emin olmak önemli olan `Az` veya `AzureRM` modüllerine runbook'ları ve ikisini tarafından kullanılan PowerShell oturumları. Varsa `Az` önce içeri aktarılan `AzureRM` bir runbook'ta, runbook tamamlanır, ancak bir [get_SerializationSettings yöntemi başvuran hata](troubleshoot/runbooks.md#get-serializationsettings) cmdlet'leri değil kaybolmuş düzgün ve iş akışlarını gösterir yürütülür. İçe aktarıyorsanız, `AzureRM` ve ardından `Az` , runbook devam eder tamamlandı, ancak iş akışlarında belirten bir hata göreceğiniz hem `Az` ve `AzureRM` aynı oturumda alınamıyor veya aynı runbook'ta kullanılan.

## <a name="migrating-to-az-modules"></a>Az modüllerine geçirme

Otomasyon hesabı testinde AzureRM modülleri yerine Az modüllerini kullanarak geçişin test önerilir. Bu Otomasyon hesabı oluşturulduktan sonra geçişinizin sorunsuz gider emin olmak için aşağıdaki adımları kullanabilirsiniz:

### <a name="stop-and-unschedule-all-runbook-that-uses-azurerm-modules"></a>Durdur ve AzureRM modülleri kullanan tüm runbook'u zamanlamasını Kaldır

Kullanan mevcut runbook'ların çalıştırmayın emin olmak için `AzureRM` cmdlet'leri, durdurmak ve gerekir kullanan tüm runbook'ları zamanlamasını `AzureRM` modüller. Hangi zamanlamaları var ve hangi zamanlamaları, aşağıdaki örnekte çalıştırarak kaldırılmalıdır görebilirsiniz:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Ayrıca, gelecekte bu runbook'larınızı gerekirse zamanlayabilirsiniz emin olmak için her zamanlamayı gözden geçirmek önemlidir.

### <a name="import-the-az-modules"></a>Az modüllerini içeri aktarma

Yalnızca runbook'larınızı için gerekli olan Az modülleri içeri aktarın. Paketi içeri aktarmayın `Az` haliyle modülü içeren tüm `Az.*` modülleri içeri aktarılacak. Bu kılavuz, tüm modüller için aynıdır.

[Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) modüldür diğer bir bağımlılık `Az.*` modüller. Bu nedenle, bu modül diğer modüllerin içeri aktarmadan önce Otomasyon hesabınıza aktarılması gerekir.

Otomasyon hesabınızdan seçin **modülleri** altında **paylaşılan kaynakları**. Tıklayın **Galeriye Gözat** açmak için **Galeriye Gözat** sayfası.  Arama çubuğunda, modül adı girin (örneğin `Az.Accounts`). PowerShell modülü sayfasında tıklayın **alma** modülü Otomasyon hesabınızda içeri aktarın.

![Otomasyon hesabından modülleri Al](media/az-modules/import-module.png)

Bu içeri aktarma işlemi üzerinden de yapılabilir [PowerShell Galerisi](https://www.powershellgallery.com) modülü için arama yapın. Modül bulduktan sonra seçin ve altındaki **Azure Otomasyonu** sekmesini tıklatın, **Azure Otomasyonu Dağıt**.

![Modülleri doğrudan Galeriden İçeri Aktar](media/az-modules/import-gallery.png)

## <a name="test-your-runbooks"></a>Runbook'larınızı test edin

Bir kez `Az` Otomasyon hesabınızda modülünü içeri aktarmadınız, runbook'larınızı Az modül kullanmayı düzenleme şimdi başlayabilirsiniz. Cmdlet'lerin çoğu dışında aynı ada sahip `AzureRM` değiştirildi `Az`. Bu işlem izlemeyin modüller listesi için bkz. [özel durumların listesi](/powershell/azure/migrate-from-azurerm-to-az?view=azps-1.1.0#change-module-imports-and-cmdlet-names).

Yeni cmdlet'lerle kullanmak için runbook değiştirme kullanarak önce runbook'larınızı test etmek için tek yönlü `Enable-AzureRMAlias -Scope Process` runbook başına. Runbook'unuzda bu runbook'a eklediğinizde, değişikliğe gerek kalmadan çalıştırılabilir.

## <a name="after-migration-details"></a>Sonra geçiş ayrıntıları

Geçiş tamamlandıktan sonra runbook'ları kullanarak başlatma `AzureRM` hesabında artık modüller. Ayrıca önerilir alma veya güncelleştirme `AzureRM` Bu hesapta modüller. Bu andan itibaren başlamayı düşünün bu hesabı geçirildiği için `Az`ve `Az` yalnızca modüller. Yeni bir Otomasyon hesabı oluşturulduğunda mevcut `AzureRM` modülleri hala yüklenir ve öğretici runbook'ları hala ile yazılmış olarak görünür `AzureRM` cmdlet'leri. Bu runbook'ları çalıştırılmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Az modüllerini kullanma hakkında daha fazla bilgi için bkz. [Az modül ile çalışmaya başlama](/powershell/azure/get-started-azureps?view=azps-1.1.0).
