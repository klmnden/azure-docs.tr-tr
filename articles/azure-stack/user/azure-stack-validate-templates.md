---
title: Şablonları Azure yığını denetlemek için bir şablon doğrulama aracını kullanma | Microsoft Docs
description: Azure yığın dağıtımına için onay şablonları
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 88fac41ce2c9fa0c5569beae02ab90a507c89a34
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Şablonlarınızı Azure yığını için şablon doğrulama aracıyla denetleyin.

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Şablon doğrulama aracı denetlemek için kullanabileceğiniz, Azure Resource Manager [şablonları](azure-stack-arm-templates.md) Azure yığınına dağıtmak için hazır olursunuz. Şablon doğrulama aracı Azure yığın araçları bir parçası olarak kullanılabilir. Açıklanan adımları kullanarak Azure yığın araçları karşıdan [araçları Github'dan indirdiğinizde](azure-stack-powershell-download.md) makalesi.

## <a name="overview"></a>Genel Bakış

Bir bulut yeteneklerini oluşturmak zorunda şablon doğrulamak için ilk dosya ve Doğrulama Aracı'nı çalıştırın. Aşağıdaki PowerShell modülleri Azure yığın araçları kullanın:

- İçinde **TemplateValidator** klasörü:<br>         AzureRM.CloudCapabilities.psm1 Azure yığın bulut sürümlerinde ve Hizmetleri temsil eden bir bulut özellikleri JSON dosyası oluşturur.
- İçinde **CloudCapabilities** klasörü:<br>
AzureRM.TemplateValidator.psm1 şablonlarını dağıtımı için Azure yığınında test etmek için bir bulut özellikleri JSON dosyasını kullanır.

## <a name="build-the-cloud-capabilities-file"></a>Bulut özellikleri dosyası oluşturma

Şablon Doğrulayıcı kullanmadan önce bir JSON dosyası oluşturmak için AzureRM.CloudCapabilities PowerShell modülü çalıştırın.

>[!NOTE]
>Tümleşik sisteminizi güncelleştirmeniz veya herhangi bir yeni hizmet veya sanal uzantıları eklerseniz, bu modül yeniden çalıştırmanız gerekir.

1. Azure yığın bağlantınız olduğundan emin olun. Bu adımları Azure yığın Geliştirme Seti ana bilgisayardan gerçekleştirilebilir veya kullanabileceğiniz bir [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) istasyonunuzdan bağlanmak için.
2. AzureRM.CloudCapabilities PowerShell modülünü içeri aktarın:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Hizmet sürümleri almak ve bir bulut özellikleri JSON dosyası oluşturmak için Get-CloudCapabilities cmdlet'ini kullanın. Belirtmediyseniz **- OutputPath**, dosyanın AzureCloudCapabilities.Json geçerli dizinde oluşturulur. Gerçek konumunuz kullanın:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Şablonları doğrula

AzureRM.TemplateValidator PowerShell modülünü kullanarak şablonları doğrulamak için aşağıdaki adımları kullanın. Kendi şablonlarınızı kullanın veya doğrulama [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates).

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

Şablon doğrulama uyarılar veya hatalar PowerShell konsolunu ve kaynak dizin bir HTML dosyasına kaydedilir. Aşağıdaki ekran görüntüsünde bir doğrulama raporu örneği gösterilmektedir:

![Şablon doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

Şablon Doğrulayıcı aşağıdaki parametreleri destekler.

| Parametre | Açıklama | Gerekli |
| ----- | -----| ----- |
| TemplatePath | Yinelemeli olarak yoluna Bul Azure Resource Manager şablonları belirtir | Evet | 
| TemplatePattern | Eşleştirilecek şablon dosyalarını adını belirtir. | Hayır |
| CapabilitiesPath | Bulut özellikleri JSON dosyası yolunu belirtir. | Evet | 
| IncludeComputeCapabilities | VM boyutları ve VM uzantıları gibi Iaas kaynaklarına değerlendirmesine içerir | Hayır |
| IncludeStorageCapabilities | Değerlendirme SKU türleri gibi depolama kaynakları içerir | Hayır |
| Rapor | Oluşturulan HTML raporun adını belirtir | Hayır |
| Ayrıntılı | Hatalar ve Uyarılar için konsolu günlükleri | Hayır|

### <a name="examples"></a>Örnekler

Bu örnekte tüm doğrulama [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) yerel depolama alanına indirilir. Örnek ayrıca sanal makine boyutları ve uzantıları Azure yığın Geliştirme Seti yetenekleri karşı doğrular.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınına şablonlarını dağıtma](azure-stack-arm-templates.md)
- [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)
