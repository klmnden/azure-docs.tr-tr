---
title: Kaynak denetimi tümleştirmesi Azure automation'da
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/26/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9bbf3582da2664b6e6429677d47aad4d69a7c1bb
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785333"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

Kaynak denetimi, runbook'larınızı tutmanızı sağlar, Otomasyon hesabı ile GitHub veya Azure Dev Ops kaynak denetim deposu betiğinizde güncel. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi, geliştirme, test veya üretim Otomasyon hesaplarınız için kaynak denetiminde farklı dallara eşitleme sağlar. Bu Otomasyon hesabı üretim geliştirme ortamınızda test kod yükseltmek kolaylaştırır.

Azure Otomasyonu, kaynak denetimi 3 türlerini destekler:

* GitHub
* Visual Studio Team Services (Git)
* Visual Studio Team Services (TFVC)

## <a name="pre-requisites"></a>Ön koşullar

* Bir kaynak denetim deposu (GitHub veya Visual Studio Team Services)
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
|Kaynak Denetim türü     | Kaynak denetimi türü. Kullanılabilen seçenekler:</br> GitHub</br>Visual Studio Team Services (Git)</br> Visual Studio Team Services (TFVC)        |
|Havuz     | Depo veya projenin adı. Bu değer, kaynak denetimi deposundan alınır. Örnek: $/ ContosoFinanceTFVCExample         |
|Dal     | Kaynak dosyalarını çekmek için dal. Dal hedefleyen TFVC kaynak denetimi türü için kullanılamıyor.          |
|Klasör yolu     | Eşitleme için runbook'ları içeren klasör. Örnek: /Runbooks         |
|Otomatik eşitleme     | Açar veya kaynak denetim deposunda bir işleme yapıldığında otomatik eşitleme devre dışı         |
|Runbook yayımlama     | Varsa kümesine **üzerinde**, runbook'ları, bunlar otomatik olarak yayımlanacak kaynak denetiminden eşitlendiğinde.         |
|Açıklama     | Ek ayrıntılar sağlamak için bir metin alanı        |

![Kaynak denetimi özeti](./media/source-control-integration/source-control-summary.png)

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
|**Admin: repo_hook**     |         |
|yazma: repo_hook     | Depo kancaları yazma         |
|Okuma: repo_hook|Okuma deposu kancaları|

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
