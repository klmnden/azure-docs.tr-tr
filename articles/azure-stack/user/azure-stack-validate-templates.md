---
title: "Şablonları Azure yığını denetlemek için şablon Doğrulayıcı kullanın | Microsoft Docs"
description: "Azure yığın dağıtımına için onay şablonları"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6a77efb3ef4236048ff08b14346175b592493982
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Azure şablonu Doğrulayıcı yığınla şablonlarınızı denetle

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Şablon doğrulama aracı denetlemek için kullanabileceğiniz, Azure Resource Manager [şablonları](azure-stack-arm-templates.md) Azure yığını için hazırsınız. Şablon doğrulama aracı Azure yığın araçları bir parçası olarak kullanılabilir. Açıklanan adımları kullanarak Azure yığın araçları karşıdan [araçları Github'dan indirdiğinizde](azure-stack-powershell-download.md) makalesi. 

Şablonları doğrulamak için aşağıdaki PowerShell modülleri kullanma **TemplateValidator** ve **CloudCapabilities** klasörler: 

 - AzureRM.CloudCapabilities.psm1 sürümleri Azure yığın gibi bir bulutta ve Hizmetleri temsil eden bir bulut özellikleri JSON dosyası oluşturur.
 - AzureRM.TemplateValidator.psm1 şablonlarını dağıtımı için Azure yığınında test etmek için bir bulut özellikleri JSON dosyasını kullanır.
 
Bu makalede, bir bulut özellikleri dosyasını oluşturup Doğrulayıcı Aracı'nı çalıştırın.

## <a name="build-cloud-capabilities-file"></a>Bulut özellikleri dosyası oluşturma
Şablon Doğrulayıcı kullanmadan önce bir JSON dosyası oluşturmak için AzureRM.CloudCapabilities PowerShell modülü çalıştırın. Tümleşik sisteminizi güncelleştirmeniz veya herhangi bir yeni hizmet veya VM uzantıları eklerseniz, bu modül yeniden da çalıştırmanız gerekir.

1.  Azure yığın bağlantınız olduğundan emin olun. Bu adımları Azure yığın Geliştirme Seti ana bilgisayardan gerçekleştirilebilir veya kullanabileceğiniz bir [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) istasyonunuzdan bağlanmak için. 
2.  AzureRM.CloudCapabilities PowerShell modülünü içeri aktarın:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Hizmet sürümleri almak ve bir bulut özellikleri JSON dosyası oluşturmak için Get-CloudCapabilities cmdlet'ini kullanın. -OutputPath, belirtmezseniz dosyası geçerli dizinde AzureCloudCapabilities.Json oluşturulur. Gerçek konumunuz kullanın:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```             

## <a name="validate-templates"></a>Şablonları doğrula
Bu adımlarda, AzureRM.TemplateValidator PowerShell modülünü kullanarak şablonları doğrulayın. Kendi şablonlarınızı kullanın veya doğrulama [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates).

1.  AzureRM.TemplateValidator.psm1 PowerShell modülünü içeri aktarın:
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  Şablon Doğrulayıcı çalıştırın:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Şablon doğrulama uyarı veya hata PowerShell konsolunu ve kaynak dizin bir HTML dosyasına kaydedilir. Doğrulama raporunu bir örneği burada verilmiştir:

![Örnek doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

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
Bu örnekte tüm doğrulama [Azure yığın hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) yerel olarak indirilir ve ayrıca VM boyutları ve uzantıları Azure yığın Geliştirme Seti yetenekleri karşı doğrular.

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

