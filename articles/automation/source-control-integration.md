---
title: Kaynak denetimi tümleştirmesi Azure automation'da
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/15/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 400cf53172fbd1ce5803cf3de298749afbf45cd4
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430203"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

Kaynak denetimi, runbook'larınızı tutmanızı sağlar, Otomasyon hesabı ile GitHub veya Azure DevOps kaynak denetim deposu betiğinizde güncel. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi, geliştirme, test veya üretim Otomasyon hesaplarınız için kaynak denetiminde farklı dallara eşitleme sağlar. Bu Otomasyon hesabı üretim geliştirme ortamınızda test kod yükseltmek kolaylaştırır.

Azure Otomasyonu, kaynak denetimi 3 türlerini destekler:

* GitHub
* Azure DevOps (Git)
* Azure DevOps (TFVC)

## <a name="pre-requisites"></a>Ön koşullar

* Bir kaynak denetim deposu (GitHub veya Azure DevOps)
* Doğru [izinleri](#personal-access-token-permissions) için kaynak denetim deposu
* A [Run-As hesabı ve bağlantı](manage-runas-account.md)

> [!NOTE]
> Kaynak denetimi eşitleme işi, Otomasyon hesabı kapsamındaki kullanıcıların çalıştırabilir ve diğer Otomasyon işleri aynı fiyat üzerinden ücretlendirilir.

## <a name="configure-source-control"></a>Kaynak denetimini yapılandırma

Otomasyon hesabınızdan seçin **kaynak denetimi (Önizleme)** tıklatıp **+ Ekle**

![Kaynak Denetimi Seç](./media/source-control-integration/select-source-control.png)

Seçin **kaynak denetimi türü**, tıklayın **doğrulaması**.

Uygulama isteği izinleri sayfasında gözden geçirin ve tıklayın **kabul**.

Üzerinde **kaynak denetimi özeti** sayfasında, bilgileri doldurun ve tıklayın **Kaydet**. Aşağıdaki tabloda kullanılabilir alanları açıklamasını gösterir.

|Özellik  |Açıklama  |
|---------|---------|
|Kaynak denetimi adı     | Kaynak denetimi için bir kolay ad        |
|Kaynak Denetim türü     | Kaynak denetimi türü. Kullanılabilen seçenekler:</br> GitHub</br>Azure DevOps (Git)</br> Azure DevOps (TFVC)        |
|Depo     | Depo veya projenin adı. Bu değer, kaynak denetimi deposundan alınır. Örnek: $/ ContosoFinanceTFVCExample         |
|Şube     | Kaynak dosyalarını çekmek için dal. Dal hedefleyen TFVC kaynak denetimi türü için kullanılamıyor.          |
|Klasör yolu     | Eşitleme için runbook'ları içeren klasör. Örnek: /Runbooks         |
|Auto Sync     | Açar veya kaynak denetim deposunda bir işleme yapıldığında otomatik eşitleme devre dışı         |
|Publish Runbook     | Varsa kümesine **üzerinde**, runbook'ları, bunlar otomatik olarak yayımlanacak kaynak denetiminden eşitlendiğinde.         |
|Açıklama     | Ek ayrıntılar sağlamak için bir metin alanı        |

![Kaynak denetimi özeti](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Kaynak denetimi yapılandırma sırasında doğru hesabıyla oturum emin olun. Bir şüpheli varsa, tarayıcınızda yeni bir sekme açın ve visualstudio.com veya github.com Oturumu Kapat ve bağlantı kaynak denetimine yeniden deneyin.

## <a name="syncing"></a>Eşitleniyor

Kaynak denetimi tümleştirmesini yapılandırırken autosync yapılandırmak, ilk eşitleme otomatik olarak başlar. Otomatik eşitleme ayarlı değil, kaynak tablosundan seçin **kaynak denetimi (Önizleme)** sayfası. Tıklayın **Eşitlemeyi Başlat** eşitleme işlemini başlatmak için.

Geçerli eşitleme işi veya öncekilerle tıklayarak durumu görüntüleyebilir **Eşitleme işleri** sekmesi. Üzerinde **kaynak denetimi** açılan listesinde, kaynak denetimi seçin.

![Eşitleme durumu](./media/source-control-integration/sync-status.png)

Bir işe tıklamak iş çıktısı görüntülemenize olanak sağlar. Aşağıdaki örnek bir kaynak denetimi eşitleme işi çıktısı bulunmaktadır.

```output
========================================================================================================

Azure Automation Source Control Public Preview.
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

## <a name="personal-access-token-permissions"></a>Kişisel erişim belirteci izinleri

Kaynak denetimi için kişisel erişim belirteçleri bazı minimum izinleri gerektirir. Aşağıdaki tablolarda, GitHub ve Azure DevOps için gerekli en düşük izinleri içerir.

### <a name="github"></a>GitHub

|Kapsam  |Açıklama  |
|---------|---------|
|**depo**     |         |
|Depo: durumu     | Erişim yürütme durumu         |
|repo_deployment      | Erişim dağıtım durumu         |
|public_repo     | Genel erişim depolar         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Depo kancaları yazma         |
|read:repo_hook|Okuma deposu kancaları|

### <a name="azure-devops"></a>Azure DevOps

|Kapsam  |
|---------|
|Kod (okuma)     |
|Proje ve takım (okuma)|
|Kimlik (okuma)      |
|Kullanıcı profili (okuma)     |
|İş öğeleri (okuma)    |
|Hizmet (okuma, sorgulama ve yönetme) bağlantıları<sup>1</sup>    |

<sup>1</sup>hizmet bağlantıları yalnızca izindir autosync etkinleştirdiyseniz gerekli.

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

Bir kaynak denetimi deposundan bağlantıyı kesmek için açık **kaynak denetimi (Önizleme)** altında **hesap ayarları** Otomasyon hesabınızdaki.

Kaldırmak istediğiniz kaynak denetimi seçin. Üzerinde **kaynak denetimi özeti** sayfasında **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)

