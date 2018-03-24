---
title: Azure Otomasyonu ile Visual Stuido Team Services kaynak denetimini Tümleştir
description: Senaryo, bir Azure Otomasyonu hesabı ve Visual Stuido Team Services kaynak denetimi tümleştirmesi kurma açıklanmaktadır.
services: automation
documentationcenter: ''
author: eamonoreilly
ms.author: eamono
keywords: Azure powershell, VSTS, kaynak denetimi, Otomasyon
ms.service: automation
ms.topic: article
ms.date: 03/19/2017
ms.openlocfilehash: a60143db03e5f89685a25f26789003de30d91f4c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure otomasyonu senaryosu - Visual Studio Team Services ile Otomasyon kaynak denetimi tümleştirme

Bu senaryoda, Azure Otomasyon çalışma kitabı ya da kaynak denetimi altındaki DSC yapılandırmaları yönetmek için kullandığınız bir Visual Studio Team Services projesi vardır.
Bu makalede, böylece her giriş için sürekli tümleştirme gerçekleşir VSTS Azure Otomasyonu ortamınız ile tümleştirmek açıklar.

## <a name="getting-the-scenario"></a>Senaryoyu alma

Bu senaryo, doğrudan aktarabilirsiniz iki PowerShell runbook'ların oluşur [Runbook Galerisi](automation-runbook-gallery.md) Azure portalında veya Merkezi'nden [PowerShell Galerisi](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbook'lar

Runbook | Açıklama| 
--------|------------|
Sync-VSTS | Runbook'ları veya yapılandırmaları bir iade yapıldığında VSTS kaynak denetiminden içeri aktarın. El ile çalıştırırsanız, alır ve tüm runbook'ları veya Otomasyon hesabı yapılandırmaları yayımlar.| 
Sync-VSTSGit | Bir iade yapıldığında VSTS Git kaynak denetimi altında yok veya yapılandırmalarını içeri aktarın. El ile çalıştırırsanız, alır ve tüm runbook'ları veya Otomasyon hesabı yapılandırmaları yayımlar.|

### <a name="variables"></a>Değişkenler

Değişken | Açıklama|
-----------|------------|
VSToken | Güvenli değişken varlığı VSTS kişisel erişim belirteci içeren oluşturun. VSTS kişisel erişim belirteci oluşturmak nasıl öğrenebilirsiniz [VSTS kimlik doğrulaması sayfası](/vsts/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma

Oluşturma bir [kişisel erişim belirteci](/vsts/accounts/use-personal-access-tokens-to-authenticate) runbook'ları veya automation hesabınız yapılandırmaları eşitlemek için kullandığınız VSTS içinde.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Oluşturma bir [güvenli değişkeni](automation-variables.md) runbook VSTS kimliğini doğrulamak ve runbook'ları ya da Otomasyon hesabı yapılandırmaları eşitleme böylece kişisel erişim belirteci tutmak için Otomasyon hesabınızda. Bu VSToken adı verebilirsiniz. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Runbook'ları ya da Otomasyon hesabı yapılandırmaları eşitlenir runbook içeri aktarın. Kullanabileceğiniz [VSTS örnek runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) veya [VSTS] Git örnek runbook ile (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) PowerShellGallery.com IF bağlı olarak gelen VSTS kaynak denetimi veya VSTS Git ile kullanın ve automation hesabınız dağıtın.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Artık [yayımlama](automation-creating-importing-runbook.md#publishing-a-runbook) bir Web kancası oluşturabilmesi için bu runbook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Oluşturma bir [Web kancası](automation-webhooks.md) bu Eşitleme VSTS runbook ve aşağıda gösterildiği gibi parametrelerini doldurun. VSTS içinde bir hizmet kancası için gerek duyduğunuz Web kancası URL'si kopyaladığınızdan emin olun. VSAccessTokenVariableName (VSToken) kişisel erişim belirteci tutmak için daha önce oluşturduğunuz güvenli değişkeninin adıdır. 

VSTS (Eşitleme-VSTS.ps1) ile tümleştirme aşağıdaki parametreleri alır:
### <a name="sync-vsts-parameters"></a>Eşitleme VSTS parametreleri

Parametre | Açıklama| 
--------|------------|
WebhookData | VSTS hizmet kanca gönderilen iade bilgiler içerir. Bu parametre boş bırakmanız gerekir.| 
ResourceGroup | Otomasyon hesabının bulunduğu kaynak grubunun adıdır.|
AutomationAccountName | VSTS ile eşitlenen Otomasyon hesabının adı.|
VSFolder | Runbook'ları ve yapılandırmaları var olduğu VSTS klasöründe adı.|
VSAccount | Visual Studio Team Services hesabı adı.| 
VSAccessTokenVariableName | VSTS kişisel erişim belirtecine güvenli değişkeni (VSToken) adı.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

VSTS GIT (Sync-VSTSGit.ps1) ile kullanıyorsanız, aşağıdaki parametreleri sürer.

Parametre | Açıklama|
--------|------------|
WebhookData | Bu VSTS hizmet kanca gönderilen iade bilgiler içerir. Bu parametre boş bırakmanız gerekir.| ResourceGroup | Bu Otomasyon hesabı bulunduğu kaynak grubunun adı.|
AutomationAccountName | VSTS ile eşitlenen Otomasyon hesabının adı.|
VSAccount | Visual Studio Team Services hesabı adı.|
VSProject | Runbook'ları ve yapılandırmaları var olduğu VSTS projesinde adı.|
GitRepo | Git deposu adı.|
GitBranch | VSTS Git deposu dalında adı.|
Klasör | VSTS Git dal klasörün adı.|
VSAccessTokenVariableName | VSTS kişisel erişim belirtecine güvenli değişkeni (VSToken) adı.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Bir hizmet kancası VSTS içinde bu Web kancası kod girişinde tetikler klasörüne iadeler oluşturun. Seçin **Web Kancalarını** yeni bir abonelik oluşturduğunuzda ile tümleştirmek için hizmet olarak. Üzerindeki hizmet kancaları hakkında daha fazla bilgiyi [VSTS hizmet kancaları belgelerine](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Artık tüm onay bileşenler runbook'lardan ve VSTS yapılandırmaları yapın ve bu otomatik olarak synched Otomasyon hesabınızda sahip olması gerekir.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Bu runbook el ile VSTS tarafından tetiklenen olmadan çalıştırırsanız, webhookdata parametresi boş bırakabilirsiniz ve belirtilen VSTS klasöründen bir tam eşitleme yapar.

Senaryo kaldırmak istiyorsanız hizmet kanca VSTS Kaldır runbook ve VSToken değişkeni silin.
