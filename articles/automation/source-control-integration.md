---
title: Kaynak denetimi tümleştirmesi Azure automation'da
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/26/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ce41ae73a0c55a2b5c27cbdce4d4c16853acf59e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273197"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

Kaynak denetimi, runbook'larınızı Otomasyon hesabınızda, GitHub veya Azure depoları kaynak denetim deposu betiğinizde ile güncel tutmanızı sağlar. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi, geliştirme, test veya üretim Otomasyon hesaplarınız için kaynak denetiminde farklı dallara eşitleme sağlar. Bu Otomasyon hesabı üretim geliştirme ortamınızda test kod yükseltmek kolaylaştırır. Otomasyon ile kaynak denetimi tümleştirmesi, tek yönlü kaynak denetimi deponuzun eşitlemeyi destekler.

Azure Otomasyonu, üç kaynak denetim türlerini destekler:

* GitHub
* Azure depoları (Git)
* Azure depoları (TFVC)

## <a name="pre-requisites"></a>Ön koşullar

* Bir kaynak denetim deposu (GitHub veya Azure depoları)
* A [Çalıştır hesabı](manage-runas-account.md)
* Olduğundan emin olun [en son Azure modüllerine](automation-update-azure-modules.md) Otomasyon hesabınızdaki

> [!NOTE]
> Kaynak denetimi eşitleme işi, Otomasyon hesabı kapsamındaki kullanıcıların çalıştırabilir ve diğer Otomasyon işleri aynı fiyat üzerinden ücretlendirilir.

## <a name="configure-source-control---azure-portal"></a>Kaynak denetimi - Azure portal'ı yapılandırma

Otomasyon hesabınızdan seçin **kaynak denetimi** tıklatıp **+ Ekle**

![Kaynak Denetimi Seç](./media/source-control-integration/select-source-control.png)

Seçin **kaynak denetimi türü**, tıklayın **doğrulaması**. Bir tarayıcı penceresi açılır ve oturum açın, kimlik doğrulamasını tamamlamak için istemleri takip edin isteyip istemediğinizi sorar.

Üzerinde **kaynak denetimi özeti** sayfasında, bilgileri doldurun ve tıklayın **Kaydet**. Aşağıdaki tabloda kullanılabilir alanları açıklamasını gösterir.

|Özellik  |Açıklama  |
|---------|---------|
|Kaynak denetimi adı     | Kaynak denetimi için bir kolay ad. *Bu ad yalnızca harf ve rakam içermelidir.*        |
|Kaynak Denetim türü     | Kaynak denetimi türü. Kullanılabilen seçenekler:</br> GitHub</br>Azure depoları (Git)</br> Azure depoları (TFVC)        |
|Havuz     | Depo veya projenin adı. İlk 200 depoları döndürülür. Bir depo için arama yapmak için ad alanını yazın ve **arama github'da**.|
|Şube     | Kaynak dosyalarını çekmek için dal. Dal hedefleyen TFVC kaynak denetimi türü için kullanılamaz.          |
|Klasör yolu     | Eşitleme için runbook'ları içeren klasör. Örnek: /Runbooks </br>*Belirtilen klasör yalnızca runbook'lar eşitlenir. Özyineleme desteklenmez.*        |
|Auto Sync<sup>1</sup>     | Açar veya kaynak denetim deposunda bir işleme yapıldığında otomatik eşitleme devre dışı         |
|Publish Runbook     | Varsa kümesine **üzerinde**sonra bunlar otomatik olarak yayımlanacağı kaynak denetiminden runbook'lar eşitlenir.         |
|Açıklama     | Ek ayrıntılar sağlamak için bir metin alanı        |

<sup>1</sup> Azure depoları ile kaynak denetimi tümleştirmesini yapılandırırken Otomatik eşitlemeyi etkinleştirmek için bir proje yöneticisi olmanız gerekir.

![Kaynak denetimi özeti](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Kaynak denetimi deponuza için oturum açma bilgilerinizi Azure portalına yönelik oturum açma bilgilerinizi farklı olabilir. Kaynak denetimi yapılandırırken, kaynak denetim deposu için doğru hesapla oturum kaydedilir emin olun. Bir şüpheli varsa, tarayıcınızda yeni bir sekme açın ve visualstudio.com veya github.com Oturumu Kapat ve bağlantı kaynak denetimine yeniden deneyin.

## <a name="configure-source-control---powershell"></a>Kaynak denetimi - PowerShell yapılandırma

Azure Otomasyonu'nda kaynak denetimi yapılandırmak için PowerShell de kullanabilirsiniz. PowerShell cmdlet'leri ile kaynak denetimi yapılandırmak için bir kişisel erişim belirteci (PAT) gereklidir. Kullandığınız [yeni AzureRmAutomationSourceControl](/powershell/module/AzureRM.Automation/New-AzureRmAutomationSourceControl) kaynak denetimi bağlantısı oluşturmak için. Cmdlet, kişisel erişim güvenli bir dize oluşturma hakkında bilgi edinmek için belirteci, güvenli bir dize gerektirir [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

### <a name="azure-repos-git"></a>Azure depoları (Git)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposGit -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="azure-repos-tfvc"></a>Azure depoları (TFVC)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposTFVC -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_versionControl -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="github"></a>GitHub

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="personal-access-token-permissions"></a>Kişisel erişim belirteci izinleri

Kaynak denetimi için kişisel erişim belirteçleri bazı minimum izinleri gerektirir. Aşağıdaki tablolarda, GitHub ve Azure deposu için gerekli en düşük izinleri içerir.

#### <a name="github"></a>GitHub

GitHub kişisel erişim belirteci oluşturma hakkında daha fazla bilgi için ziyaret [komut satırı için bir kişisel erişim belirteci oluşturma](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|`Scope`  |Açıklama  |
|---------|---------|
|**depo**     |         |
|Depo: durumu     | Erişim yürütme durumu         |
|repo_deployment      | Erişim dağıtım durumu         |
|public_repo     | Genel erişim depolar         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Depo kancaları yazma         |
|read:repo_hook|Okuma deposu kancaları|

#### <a name="azure-repos"></a>Azure Repos

Azure depolarda kişisel erişim belirteci oluşturma hakkında daha fazla bilgi için ziyaret [erişim kişisel erişim belirteçleri ile kimlik doğrulaması](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

|`Scope`  |
|---------|
|Kod (okuma)     |
|Proje ve takım (okuma)|
|Kimlik (okuma)      |
|Kullanıcı profili (okuma)     |
|İş öğeleri (okuma)    |
|Hizmet (okuma, sorgulama ve yönetme) bağlantıları<sup>1</sup>    |

<sup>1</sup> hizmet bağlantıları izindir yalnızca autosync etkinleştirdiyseniz gerekli.

## <a name="syncing"></a>Eşitleniyor

Kaynak tablo seçin **kaynak denetimi** sayfası. Tıklayın **Eşitlemeyi Başlat** eşitleme işlemini başlatmak için.

Geçerli eşitleme işi veya öncekilerle tıklayarak durumu görüntüleyebilir **Eşitleme işleri** sekmesi. Üzerinde **kaynak denetimi** açılan listesinde, kaynak denetimi seçin.

![Eşitleme durumu](./media/source-control-integration/sync-status.png)

Bir işe tıklamak iş çıktısı görüntülemenize olanak sağlar. Aşağıdaki örnek bir kaynak denetimi eşitleme işi çıktısı bulunmaktadır.

```output
========================================================================================================

Azure Automation Source Control.
Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

Setting AzureRmEnvironment.

Getting AzureRunAsConnection.

Logging in to Azure...

Source control information for syncing:

[Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

Connecting to VSTS...


Source Control Sync Summary:


2 files synced:
 - ExampleRunbook1.ps1
 - ExampleRunbook2.ps1



========================================================================================================
```

Ek günlükler kullanılabilir seçerek **tüm günlükler** üzerinde **kaynak denetimi eşitleme işi özeti** sayfası. Bu ek günlük girişleri kaynak denetimi kullanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olabilir.

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

Bir kaynak denetimi deposundan bağlantıyı kesmek için açık **kaynak denetimi** altında **hesap ayarları** Otomasyon hesabınızdaki.

Kaldırmak istediğiniz kaynak denetimi seçin. Üzerinde **kaynak denetimi özeti** sayfasında **Sil**.

## <a name="encoding"></a>Encoding

Birden çok kişinin farklı bir düzenleyici ile runbook'ları kaynak denetimi deponuzda düzenliyorsanız, kodlama sorunlarına çalıştırma olanağı yoktur. Bu durum hatalı karakterler runbook'unuzda neden olabilir. Bunun hakkında daha fazla bilgi edinmek için [yaygın nedenleri kodlama sorunlarına](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)

## <a name="next-steps"></a>Sonraki adımlar

Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
