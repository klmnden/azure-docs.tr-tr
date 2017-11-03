---
title: "Azure Otomasyonu Visual Stuido Team Services kaynak denetimi ile Birleşen | Microsoft Docs"
description: "Senaryo, bir Azure Otomasyonu hesabı ve Visual Stuido Team Services kaynak denetimi tümleştirmesi kurma açıklanmaktadır."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: Azure powershell, VSTS, kaynak denetimi, Otomasyon
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure otomasyonu senaryosu - Visual Studio Team Services ile Otomasyon kaynak denetimi tümleştirme

Bu senaryoda, Azure Otomasyon çalışma kitabı ya da kaynak denetimi altındaki DSC yapılandırmaları yönetmek için kullandığınız bir Visual Studio Team Services projesi vardır.
Bu makalede, böylece her giriş için sürekli tümleştirme gerçekleşir VSTS Azure Otomasyonu ortamınız ile tümleştirmek açıklar.

## <a name="getting-the-scenario"></a>Senaryoyu alma

Bu senaryo, doğrudan aktarabilirsiniz iki PowerShell runbook'ların oluşur [Runbook Galerisi](automation-runbook-gallery.md) Azure portalında veya Merkezi'nden [PowerShell Galerisi](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbook'lar

Runbook | Açıklama| 
--------|------------|
Eşitleme VSTS | Runbook'ları veya yapılandırmaları bir iade yapıldığında VSTS kaynak denetiminden içeri aktarın. El ile çalıştırırsanız, içeri aktarma ve tüm runbook'ları veya Otomasyon hesabı yapılandırmaları yayımlayın.| 
Eşitleme VSTSGit | Bir iade yapıldığında VSTS Git kaynak denetimi altında yok veya yapılandırmalarını içeri aktarın. El ile çalıştırırsanız, içeri aktarma ve tüm runbook'ları veya Otomasyon hesabı yapılandırmaları yayımlayın.|

### <a name="variables"></a>Değişkenler

Değişken | Açıklama|
-----------|------------|
VSToken | Güvenli değişken varlığı VSTS kişisel erişim belirteci içeren oluşturur. VSTS kişisel erişim belirteci oluşturmak nasıl öğrenebilirsiniz [VSTS kimlik doğrulaması sayfası](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma

Oluşturma bir [kişisel erişim belirteci](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) runbook'ları veya automation hesabınız yapılandırmaları eşitlemek için kullanacağınız VSTS içinde.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Oluşturma bir [güvenli değişkeni](automation-variables.md) runbook VSTS kimliğini doğrulamak ve runbook'ları ya da Otomasyon hesabı yapılandırmaları eşitleme böylece kişisel erişim belirteci tutmak için Otomasyon hesabınızda. Bu VSToken adı verebilirsiniz. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Runbook'ları ya da Otomasyon hesabı yapılandırmaları eşitlenecek runbook içeri aktarın. Kullanabileceğiniz [VSTS örnek runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) veya [VSTS] Git örnek runbook ile (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) PowerShellGallery.com IF bağlı olarak gelen VSTS kaynak denetimi veya VSTS Git ile kullanın ve automation hesabınız dağıtın.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Artık [yayımlama](automation-creating-importing-runbook.md#publishing-a-runbook) bir Web kancası oluşturabilmesi için bu runbook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Oluşturma bir [Web kancası](automation-webhooks.md) bu Eşitleme VSTS runbook ve aşağıda gösterildiği gibi parametrelerini doldurun. VSTS içinde bir hizmet kancası için ihtiyacınız şekilde Web kancası URL'si kopyaladığınızdan emin olun. VSAccessTokenVariableName (VSToken) kişisel erişim belirteci tutmak için daha önce oluşturduğunuz güvenli değişkeninin adıdır. 

VSTS (Eşitleme-VSTS.ps1) ile tümleştirme aşağıdaki parametreleri olur.
### <a name="sync-vsts-parameters"></a>Eşitleme VSTS parametreleri

Parametre | Açıklama| 
--------|------------|
WebhookData | Bu VSTS hizmet kanca gönderilen iade bilgiler içerir. Bu parametre boş bırakmanız gerekir.| 
ResourceGroup | Otomasyon hesabının bulunduğu kaynak grubunun adıdır.|
AutomationAccountName | VSTS ile eşitlenecek Otomasyon hesabının adı.|
VSFolder | Runbook'ları ve yapılandırmaları var olduğu VSTS klasöründe adı.|
VSAccount | Visual Studio Team Services hesabı adı.| 
VSAccessTokenVariableName | VSTS kişisel erişim belirtecine güvenli değişkeni (VSToken) adı.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

VSTS GIT (Sync-VSTSGit.ps1) ile kullanıyorsanız, aşağıdaki parametreleri sürer.

Parametre | Açıklama|
--------|------------|
WebhookData | Bu VSTS hizmet kanca gönderilen iade bilgiler içerir. Bu parametre boş bırakmanız gerekir.| ResourceGroup | Bu Otomasyon hesabı bulunduğu kaynak grubunun adı.|
AutomationAccountName | VSTS ile eşitlenecek Otomasyon hesabının adı.|
VSAccount | Visual Studio Team Services hesabı adı.|
VSProject | Runbook'ları ve yapılandırmaları var olduğu VSTS projesinde adı.|
GitRepo | Git deposu adı.|
GitBranch | VSTS Git deposu dalında adı.|
Klasör | VSTS Git dal klasörün adı.|
VSAccessTokenVariableName | VSTS kişisel erişim belirtecine güvenli değişkeni (VSToken) adı.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Bir hizmet kancası VSTS içinde bu Web kancası kod girişinde tetikler klasörüne iadeler oluşturun. Yeni bir abonelik oluşturduğunuzda ile tümleştirmek için hizmet olarak Web Kancalarını'ı seçin. Üzerindeki hizmet kancaları hakkında daha fazla bilgiyi [VSTS hizmet kancaları belgelerine](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Şimdi tüm onay bileşenler runbook'lardan ve VSTS yapılandırmaları yapın ve bu otomatik olarak sahip olmanız gerekir eşitleme 'd Otomasyon hesabınızda.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Bu runbook el ile VSTS tarafından tetiklenen olmadan çalıştırırsanız, webhookdata parametresi boş bırakabilirsiniz ve belirtilen VSTS klasöründen bir tam eşitleme yapar.

Senaryo kaldırmak istiyorsanız hizmet kanca VSTS Kaldır runbook ve VSToken değişkeni silin.
