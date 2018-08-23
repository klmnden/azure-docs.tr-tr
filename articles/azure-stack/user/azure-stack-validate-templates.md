---
title: Azure Stack için şablonları denetlemek için bir şablon doğrulama aracını kullanın | Microsoft Docs
description: Azure Stack dağıtım şablonları denetleme
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 73a0766baee8da782f0192fbc17fb2898a8360ac
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056573"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Şablonlarınızı Azure Stack için şablon doğrulama aracı ile denetleyin.

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Şablon doğrulama aracını olmadığını denetlemek için kullanabileceğiniz, Azure Resource Manager [şablonları](azure-stack-arm-templates.md) Azure Stack'e dağıtmak için hazır olursunuz. Şablon doğrulama aracı, Azure Stack Araçları'nın bir parçası kullanılabilir. İçinde açıklanan adımları kullanarak Azure Stack araçları indirin [araçları Github'dan indirin](azure-stack-powershell-download.md) makalesi.

## <a name="overview"></a>Genel Bakış

Bir şablon doğrulamak için bulut yapı özelliklerine sahip dosyası birinci ve doğrulama aracını çalıştırın. Aşağıdaki PowerShell modülleri Azure Stack araçları kullanabilirsiniz:

- İçinde **CloudCapabilities** klasörü:<br>         AzureRM.CloudCapabilities.psm1 Hizmetleri ve Azure Stack bulut sürümlerinde temsil eden bir bulut özellikleri JSON dosyası oluşturur.
- İçinde **TemplateValidator** klasörü:<br>
AzureRM.TemplateValidator.psm1 dağıtım için şablonlar Azure Stack'te test etmek için bir bulut özellikleri JSON dosyası kullanır.

## <a name="build-the-cloud-capabilities-file"></a>Bulut özellikleri dosyası oluşturma

Şablon Doğrulayıcı kullanmadan önce bir JSON dosyası oluşturmak için AzureRM.CloudCapabilities PowerShell modülü çalıştırın.

>[!NOTE]
>Tümleşik sisteminizi güncelleştirin veya herhangi bir yeni hizmetleri veya sanal uzantıları ekleyin, bu modül yeniden çalıştırmanız gerekir.

1. Azure Stack bağlantısı olduğundan emin olun. Bu adımlar, Azure Stack Geliştirme Seti konaktan gerçekleştirilebilir veya kullanabileceğiniz bir [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) istasyonunuzdan bağlanmak için.
2. AzureRM.CloudCapabilities PowerShell modülünü içeri aktarın:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Bir bulut özellikleri JSON dosyası oluşturun ve hizmet sürümlerini almak için Get-CloudCapabilities cmdlet'ini kullanın. Belirtmezseniz **- OutputPath**, AzureCloudCapabilities.Json geçerli dizinde oluşturulan dosya. Gerçek konumunuz kullanın:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Şablonları doğrula

Şablonları AzureRM.TemplateValidator PowerShell modülünü kullanarak doğrulamak için aşağıdaki adımları kullanın. Kendi şablonlarınızı kullanın veya doğrulama [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. AzureRM.TemplateValidator.psm1 PowerShell modülünü içeri aktarın:

    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Şablon Doğrulayıcı çalıştırın:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Şablon doğrulama uyarıları veya hataları, PowerShell konsolunu ve kaynak dizinde bir HTML dosyası kaydedilir. Aşağıdaki ekran görüntüsü yakalamayı, doğrulama raporuna örnek gösterilmektedir:

![Şablon doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

Şablon Doğrulayıcı aşağıdaki parametreleri destekler.

| Parametre | Açıklama | Gerekli |
| ----- | -----| ----- |
| TemplatePath | Yinelemeli olarak yolu Azure Resource Manager şablonları bulma belirtir | Evet | 
| TemplatePattern | Eşleştirilecek şablon dosyalarının adını belirtir. | Hayır |
| CapabilitiesPath | Bulut özellikleri JSON dosyasının yolunu belirtir. | Evet | 
| IncludeComputeCapabilities | Iaas kaynaklarının VM boyutları ve VM uzantıları gibi değerlendirme içerir | Hayır |
| IncludeStorageCapabilities | SKU türü gibi depolama kaynaklarını değerlendirmesini içerir | Hayır |
| Rapor | Oluşturulan HTML raporu adını belirtir | Hayır |
| Ayrıntılı | Konsola hataları ve uyarıları günlüğe kaydeder | Hayır|

### <a name="examples"></a>Örnekler

Bu örnekte, tüm doğrulama [Azure Stack hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) yerel depolama birimine yüklenen. Örnek aynı zamanda sanal makine boyutları ve uzantıları Azure Stack geliştirme Seti'ni özellikleri karşı doğrular.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure Stack'e dağıtma](azure-stack-arm-templates.md)
- [Şablonları Azure Stack için geliştirme](azure-stack-develop-templates.md)
