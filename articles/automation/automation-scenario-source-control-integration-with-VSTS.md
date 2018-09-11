---
title: Azure Otomasyonu, Azure DevOps Services kaynak denetimiyle tümleştirme
description: Senaryo, bir Azure Otomasyonu hesabını ve Azure DevOps Services kaynak denetimi tümleştirmesi kurma açıklanmaktadır.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: Azure powershell, Azure DevOps Hizmetleri, kaynak denetimi Otomasyonu
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 03/19/2017
ms.openlocfilehash: 022fca09b9e748c030df6b5fc944f7930942a6f7
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302417"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Azure otomasyonu senaryosu - Azure DevOps ile Otomasyon kaynak denetimi tümleştirmesi

Bu senaryoda, Azure Otomasyonu runbook'ları veya DSC yapılandırmaları kaynak denetimi altında yönetmek için kullandığınız bir Azure DevOps projesi sahip.
Bu makalede, böylece her iade için sürekli tümleştirme gerçekleşir, Azure DevOps Azure Otomasyonu ortamınızla tümleştirmek açıklar.

## <a name="getting-the-scenario"></a>Senaryoyu alma

Bu senaryo, doğrudan aktarabileceğiniz iki PowerShell runbook'ları oluşan [Runbook Galerisi](automation-runbook-gallery.md) Azure portalında veya indirilerek [PowerShell Galerisi](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbook'lar

Runbook | Açıklama| 
--------|------------|
VSTS eşitleme | Bir iade tamamlandığında Azure DevOps kaynak denetiminden runbook'ları veya yapılandırmalarını içeri aktarın. El ile çalıştırmak, alır ve tüm runbook'ları veya Otomasyon hesabının yapılandırmalarına yayımlar.| 
Eşitleme VSTSGit | Bir iade tamamlandığında Azure DevOps Git kaynak denetimi altında runbook'ları veya yapılandırmalarını içeri aktarın. El ile çalıştırmak, alır ve tüm runbook'ları veya Otomasyon hesabının yapılandırmalarına yayımlar.|

### <a name="variables"></a>Değişkenler

Değişken | Açıklama|
-----------|------------|
VSToken | Güvenli değişken varlığı içeren Azure DevOps kişisel erişim belirteci oluşturun. Azure DevOps kişisel erişim belirteci oluşturmak nasıl öğrenebilirsiniz [Azure DevOps kimlik doğrulaması sayfası](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Bu senaryoyu yükleme ve yapılandırma

Oluşturma bir [kişisel erişim belirteci](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) , Azure DevOps, runbook'ları veya bunları Otomasyon hesabınıza yapılandırmalarına eşitlemek için kullanın.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Oluşturma bir [güvenli değişkeni](automation-variables.md) Otomasyon hesabınızdaki runbook, Azure DevOps ile kimlik doğrulaması ve Otomasyon hesabı yapılandırmalarına ve runbook'ları Eşitle kişisel erişim belirteci tutacak. Bu VSToken ad verebilirsiniz.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Runbook'ları veya Otomasyon hesabının yapılandırmalarına eşitlenen runbook'u içeri aktarın. Kullanabileceğiniz [Azure DevOps örnek runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) veya [Git örnek runbook ile Azure DevOps](https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) PowerShellGallery.com if bağlı olarak, Azure DevOps kaynak denetimi veya Azure DevOps ile Git kullanmak ve Otomasyon hesabınıza dağıtın.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Artık [yayımlama](automation-creating-importing-runbook.md#publishing-a-runbook) bir Web kancası oluşturmak için bu runbook. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Oluşturma bir [Web kancası](automation-webhooks.md) bu Eşitleme VSTS runbook ve aşağıda gösterildiği gibi parametreleri doldurun. Bir hizmet kancası, Azure DevOps için gereken Web kancası URL'sini kopyaladığınızdan emin olun. VSAccessTokenVariableName (VSToken) kişisel erişim belirteci tutmak için daha önce oluşturduğunuz güvenli değişkeninin adıdır. 

Azure DevOps (VSTS.ps1 eşitleme) ile tümleştirme aşağıdaki parametreleri alır:
### <a name="sync-vsts-parameters"></a>Eşitleme VSTS parametreleri

Parametre | Açıklama| 
--------|------------|
WebhookData | Bu, Azure DevOps hizmeti kancasından gönderilen iade etme bilgiler içerir. Bu parametre boş bırakmanız gerekir.| 
ResourceGroup | Bu Otomasyon hesabının bulunduğu kaynak grubunun adıdır.|
AutomationAccountName | Azure DevOps ile eşitlenen bir Otomasyon hesabı adı.|
VSFolder | Azure DevOps runbook'larda ve yapılandırmalarda bulunduğu klasörün adı.|
VSAccount | Azure DevOps kuruluş adı.| 
VSAccessTokenVariableName | Azure DevOps kişisel erişim belirteci içeren güvenli değişkenini (VSToken) adı.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Azure DevOps GIT (VSTSGit.ps1 eşitleme) ile kullanıyorsanız, aşağıdaki parametreleri sürer.

Parametre | Açıklama|
--------|------------|
WebhookData | Bu Azure DevOps hizmeti kancasından gönderilen iade etme bilgiler içerir. Bu parametre boş bırakmanız gerekir.| 
ResourceGroup | Bu Otomasyon hesabının bulunduğu kaynak grubunun adı.|
AutomationAccountName | Azure DevOps ile eşitlenen bir Otomasyon hesabı adı.|
VSAccount | Azure DevOps kuruluş adı.|
VSProject | Runbook'larda ve yapılandırmalarda bulunduğu Azure DevOps projesinin adı.|
GitRepo | Git deposunun adı.|
GitBranch | Azure DevOps Git deposunda dal adı.|
Klasör | Azure DevOps Git dalı klasörün adı.|
VSAccessTokenVariableName | Azure DevOps kişisel erişim belirteci içeren güvenli değişkenini (VSToken) adı.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Bir hizmet kancası, kodu iade bu Web kancasını tetikler klasöre iade etme işlemleri için Azure DevOps oluşturun. Seçin **Web kancaları** yeni bir abonelik oluşturduğunuzda ile tümleştirmek için hizmet olarak. Hizmet kancalarında hakkında daha fazla bilgi edinebilirsiniz [Azure DevOps hizmet kancaları belgeleri](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Artık tüm iade etmelerin runbook'ları ve Azure DevOps yapılandırmalarına yapmak mümkün olması gerekir ve bunlar otomatik olarak Otomasyon hesabınıza eşitlenmiş olması.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Bu runbook el ile Azure DevOps tarafından tetiklenen olmadan çalıştırırsanız, webhookdata parametresi boş bırakabilirsiniz ve bunu belirtilen Azure DevOps klasöründen bir tam eşitleme yapar.

Senaryo kaldırmak istiyorsanız hizmet kancası Azure DevOps kaldırmak runbook ve VSToken değişkeni silin.
