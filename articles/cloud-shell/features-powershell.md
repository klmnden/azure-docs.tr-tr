---
title: "Azure bulut Kabuk (Önizleme) özellikleri PowerShell'de | Microsoft Docs"
description: "Azure bulut kabuğunda PowerShell özelliklerine genel bakış"
services: Azure
documentationcenter: 
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: damaerte
ms.openlocfilehash: ffe5298678de6cc16af0ea29c7d887330f77589d
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="features--tools-for-powershell-in-azure-cloud-shell"></a>Özellikler ve PowerShell Azure bulut kabuğu için Araçlar

[!include [features-introblock](../../includes/cloud-shell-features-introblock.md)]

> [!TIP]
> Özellikleri & araçlarını [Bash](features.md) da kullanılabilir.

PowerShell bulut Kabuk içinde çalıştığı `Windows Server 2016`.

## <a name="features"></a>Özellikler

### <a name="secure-automatic-authentication"></a>Güvenli otomatik kimlik doğrulaması

PowerShell bulut Kabuğu güvenli bir şekilde ve otomatik olarak hesap erişim için Azure PowerShell kimliğini doğrular.

### <a name="files-persistence-across-sessions"></a>Kalıcılık oturumlarında dosyaları

Dosyaları oturumlarında kalıcı hale getirmek için bulut Kabuk, ilk kez başlatıldığında Azure dosya paylaşımında ekleme aracılığıyla açıklanmaktadır.
Tamamlandığında, bulut Kabuk otomatik olarak depolama ihtiyaçlarınızı ekleme (olarak takılı `$home\clouddrive`) gelecekteki tüm oturumları için.
Bulut Kabuk geçici bir makine ayırma için her istek dışında dosyaları bu yana, `$home\clouddrive` ve makine durumunu oturumlar arasında sürdürülmez.

[Azure dosya paylaşımları için bulut Kabuk ekleme hakkında daha fazla bilgi edinin.](persisting-shell-storage-powershell.md)

### <a name="azure-drive-azure"></a>Azure sürücüsü (Azure:)

Bulut Kabuk PowerShell'de Azure sürücüde başlatır (`Azure:`).
Azure sürücüsü kolay bulma ve işlem, ağ, depolama vb. için dosya sistemi Gezinti benzer gibi Azure kaynakları gezinti sağlar.
Bilinen kullanmaya devam edebilirsiniz [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/en-us/powershell/azure) bu kaynakları yönetmek için.
Azure kaynaklarına doğrudan Azure portalında veya Azure PowerShell cmdlet'leri aracılığıyla yapılan ya da yapılan tüm değişiklikler anında Azure sürücüde yansıtılır.

![](media/features-powershell/azure-drive.png)

#### <a name="contextual-awareness"></a>Bağlamsal tanıma

- **Kaynak grubu kapsamı**: zaman Azure sürücü kaynak grubu yolunda bağlamında (`Azure:`), kaynak grubu adı için Azure PowerShell cmdlet'leri otomatik olarak geçirilir.

    ![](media/features-powershell/resource-group-autocomplete.png)

- **Get-AzureRmCommand**: Bu cmdlet komutların listesini Azure sürücüsü altındaki konum bağlamında geçerli döndürür (`Azure:`). Örneğin, bunlar, kullanıcı altında olduğunda yalnızca depolama ile ilgili komutları gösterir`Azure:\<subscription name>\StorageAccounts`

    ![](media/features-powershell/get-azurermcommand.png)

### <a name="rich-powershell-script-editing"></a>Zengin PowerShell komut dosyası düzenleme

PowerShell dosyalarını düzenlemek için VIM kullandığınızda (`.ps1,.psm1,.psd1`), sözdizimi vurgulama ve IntelliSense desteği otomatik olarak alın.
IntelliSense desteği, yerel bir örneği ile etkileşime giren bir VIM-eklentisi aracılığıyla gerçekleştirilir [PowerShell Düzenleyici Hizmetleri](https://github.com/PowerShell/PowerShellEditorServices).

> [!TIP]
> Kullanım `TAB` (uygunsa) tamamlama (IntelliSense) komut adları, parametre adları ve parametre değerlerini almak için.

![](media/features-powershell/powershell-editing-vim.png)

### <a name="extensible-model"></a>Genişletilebilir modeli

Kullanarak [PowerShellGet](https://docs.microsoft.com/powershell/module/powershellget), kolayca yükleyin (güncelleştirme özel modüller gelen komut dosyalarını ve) [PowerShell Galerisi](https://www.powershellgallery.com).
Yükleme sonrasında, modüllerinizi otomatik olarak bulut Kabuk oturumlarında kalıcıdır.

> [!TIP]
> Kullanıcılar tarafından yüklü modülleri kaydedilir `$Home\CloudDrive\.pscloudshell\WindowsPowerShell` klasör. Bu klasör için sembolik bağlantı kullanıcının Belgeler klasöründe oluşturulur (`$home\Documents\WindowsPowerShell`).

![](media/features-powershell/powershellget-module.png)

### <a name="management-of-guest-vms"></a>Konuk VM'ler Yönetimi

İki yerleşik komutları - kullanarak `Enter-AzureRmVM` ve `Invoke-AzureRmVMCommand`, Azure Vm'leriniz uzaktan yönetebilirsiniz.
Bu komutlar PowerShell uzaktan iletişimi üstünde oluşturulmuş ve Azure sanal makinelerini PowerShell bağlantısı gerektirir.

## <a name="tools"></a>Araçlar

|**Kategori**    |**Ad**                                 |
|----------------|-----------------------------------------|
|Azure Araçları     |[Azure PowerShell (4.4.1)](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.4.1)<br> [Azure CLI (2.0.20)](https://docs.microsoft.com/en-us/cli/azure/overview)|
|Metin düzenleyiciler    |VIM<br> nano                             |
|Paket Yöneticisi |PowerShellGet<br> PackageManagement<br> npm<br> PIP |
|Kaynak denetimi  |Git                                      |
|Veritabanları       |[SqlServer Modülü](https://www.powershellgallery.com/packages/SqlServer)<br> [SQLCMD yardımcı programı](https://docs.microsoft.com/sql/tools/sqlcmd-utility)      |
|Test Araçları      |Pester                                   |

## <a name="language-support"></a>Dil desteği

|**Dil**|**Sürüm**|
|------------|-----------|
|.NET        |4.6        |
|Node.js     |6.10       |
|PowerShell  |5.1 ve [6.0 (beta)](https://github.com/PowerShell/powershell/releases)       |
|Python      |2.7        |

## <a name="next-steps"></a>Sonraki adımlar

[Bulut Kabuk de PowerShell ile hızlı başlangıç](quickstart-powershell.md)

[Azure PowerShell hakkında bilgi edinin](https://docs.microsoft.com/powershell/azure/)
