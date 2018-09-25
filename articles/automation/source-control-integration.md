---
title: Kaynak denetimi tümleştirmesi Azure automation'da
description: Bu makalede, Azure automation'da GitHub ile kaynak denetimi tümleştirmesi açıklanır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c2d13a409d095bca64da781e5c5ca58553f9710c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048343"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

Kaynak denetimi, runbook'larınızı, Otomasyon hesabı olan GitHub veya Azure Dev Ops kaynak denetimi deponuza betiğinizde ile güncel tutmanızı sağlar. Kaynak denetimi kolayca takımınızla işbirliği yapmanıza, değişiklikleri izlemek ve runbook'larınızı önceki sürümleri geri alma sağlar. Örneğin, kaynak denetimi Otomasyonu üretim geliştirme ortamınızda test kod yükseltmek kolaylaştıran geliştirme, test veya üretim Otomasyon hesaplarınız, kaynak denetimine farklı dalları eşitleme sağlar hesabı.

Azure Otomasyonu, kaynak denetimi 3 türlerini destekler:

* GitHub
* Visual Studio Team Services (Git)
* Visual Studio Team Services (TFVC)

## <a name="pre-requisites"></a>Ön koşullar

* Bir kaynak denetim deposu (GitHub veya Visual Studio Team Services)
* A [Run-As hesabı ve bağlantı](manage-runas-account.md)

> [!NOTE]
> Kaynak denetimi eşitleme işi, Otomasyon hesabı kapsamındaki kullanıcıların çalıştırabilir ve diğer Otomasyon işleri aynı fiyat üzerinden ücretlendirilir.

## <a name="configure-source-control"></a>Kaynak denetimini yapılandırma

Otomasyon hesabınızdan seçin **kaynak denetimi (Önizleme)** tıklatıp **+ Ekle**

![Kaynak Denetimi Seç](./media/source-control-integration/select-source-control.png)

Seçin **kaynak denetimi türü** , tıklayın **doğrulaması**.

Uygulama isteği izinleri sayfasında gözden geçirin ve tıklayın **kabul**.

Üzerinde **kaynak denetimi özeti** sayfasında, bilgileri doldurun ve tıklayın **Kaydet**. Aşağıdaki tabloda kullanılabilir alanları açıklamasını gösterir.

|Özellik  |Açıklama  |
|---------|---------|
|Kaynak denetimi adı     | Kaynak denetimi için bir kolay ad        |
|Kaynak Denetim türü     | Kaynak denetimi türü. Kullanılabilen seçenekler:</br> GitHub</br>Visual Studio Team Services (Git)</br>Visual Studio Team Services (TFVC)        |
|Havuz     | Depo veya projenin adı. Bu, kaynak denetimi deposundan alınır. Örnek: $/ ContosoFinanceTFVCExample         |
|Dal     | Kaynak dosyalarını çekmek için dal. Dal hedefleyen TFVC kaynak denetimi türü için kullanılamıyor.          |
|Klasör yolu     | Eşitleme için runbook'ları içeren klasör. Örnek: /Runbooks         |
|Otomatik eşitleme     | Açar veya kaynak denetim deposunda bir işleme yapıldığında otomatik eşitleme devre dışı         |
|Runbook yayımlama     | Varsa kümesine **üzerinde**, runbook'ları, bunlar otomatik olarak yayımlanacak kaynak denetiminden eşitlendiğinde.         |
|Açıklama     | Ek ayrıntılar sağlamak için bir metin alanı        |

![Kaynak denetimi özeti](./media/source-control-integration/source-control-summary.png)

## <a name="syncing"></a>Eşitleniyor

Otomatik eşitleme, kaynak denetimi tümleştirmesini yapılandırırken ayarlandıysa, ilk eşitleme otomatik olarak başlar. Otomatik eşitleme ayarlı değil, kaynak tablosundan seçin **kaynak denetimi (Önizleme)** sayfası. Tıklayın **Eşitlemeyi Başlat** eşitleme işlemini başlatmak için.  

Geçerli eşitleme işi veya öncekilerle tıklayarak durumu görüntüleyebilir **Eşitleme işleri** sekmesi. Üzerinde **kaynak denetimi** açılan listesinde, kaynak denetimi seçin.

![Eşitleme durumu](./media/source-control-integration/sync-status.png)

Bir işe tıklamak iş çıktısı görüntülemenize olanak sağlar. Kaynak denetimini eşitleme işi çıktının bir örnek verilmiştir.

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

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

Bir kaynak denetimi deposundan bağlantıyı kesmek için açık **kaynak denetimi (Önizleme)** altında **hesap ayarları** Otomasyon hesabınızdaki.

Kaldırmak istediğiniz kaynak denetimi seçin. Üzerinde **kaynak denetimi özeti** sayfasında **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Runbook türleri, avantajları ve sınırlamaları hakkında daha fazla bilgi için, bkz. [Azure Automation runbook türleri](automation-runbook-types.md)
