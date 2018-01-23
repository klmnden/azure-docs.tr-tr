---
title: "Şablonları Azure yığını denetlemek için şablon Doğrulayıcı kullanın | Microsoft Docs"
description: "Azure yığın dağıtımına için onay şablonları"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: c30b0a78cf3421554cf8f7c887c7973c7b9f4b9c
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Azure şablonu Doğrulayıcı yığınla şablonlarınızı denetle

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Şablon doğrulama aracı denetlemek için kullanabileceğiniz, Azure Resource Manager [şablonları](azure-stack-arm-templates.md) Azure yığını için hazırsınız. Şablon doğrulama aracı Azure yığın araçları bir parçası olarak kullanılabilir. Açıklanan adımları kullanarak Azure yığın araçları karşıdan [araçları Github'dan indirdiğinizde](azure-stack-powershell-download.md) makalesi. 

Şablonları doğrulamak için aşağıdaki PowerShell modülleri kullanın ve JSON dosyası bulunan **TemplateValidator** ve **CloudCapabilities** klasörler: 

 - AzureRM.CloudCapabilities.psm1 sürümleri Azure yığın gibi bir bulutta ve Hizmetleri temsil eden bir bulut özellikleri JSON dosyası oluşturur.
 - AzureRM.TemplateValidator.psm1 şablonlarını dağıtımı için Azure yığınında test etmek için bir bulut özellikleri JSON dosyasını kullanır.
 - AzureStackCloudCapabilities_with_AddOns_20170627.json is a default cloud capabilities file.  Kendinizinkini oluşturun veya başlamak için bu dosyayı kullanın. 

Bu konuda, şablonlarınızı karşı doğrulama çalıştırın ve isteğe bağlı olarak bir bulut özellikleri dosyası oluşturun.

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

Şablon doğrulama uyarı veya hata PowerShell konsoluna kaydedilir ve ayrıca kaynak dizin bir HTML dosyasına kaydedilir. Bir doğrulama raporu çıkış şöyle görünür:

![Örnek doğrulama raporu](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametreler

| Parametre | Açıklama | Gerekli |
| ----- | -----| ----- |
| TemplatePath | Yinelemeli olarak yoluna Resource Manager şablonları bulma belirtir | Evet | 
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

## <a name="build-cloud-capabilities-file"></a>Bulut özellikleri dosyası oluşturma
Karşıdan yüklenen dosyalar varsayılan dahil *AzureStackCloudCapabilities_with_AddOns_20170627.json* PaaS Hizmetleri yüklendiğinde Azure yığın geliştirme Seti'nde kullanılabilir hizmet sürümlerini açıklar dosya.  Ek kaynak sağlayıcıları yüklerseniz, yeni hizmetler de dahil olmak üzere bir JSON dosyası oluşturmak için AzureRM.CloudCapabilities PowerShell modülü kullanabilirsiniz.  

1.  Azure yığın bağlantınız olduğundan emin olun.  Kullanabilirsiniz veya bu adımları Azure yığın Geliştirme Seti ana bilgisayardan gerçekleştirilebilir [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) istasyonunuzdan bağlanmak için. 
2.  AzureRM.CloudCapabilities PowerShell modülünü içeri aktarın:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Hizmet sürümleri almak ve bir bulut özellikleri JSON dosyası oluşturmak için Get-CloudCapabilities cmdlet'ini kullanın:

    ```PowerShell
    Get-AzureRMCloudCapability -Location 'local' -Verbose
    ```             


## <a name="next-steps"></a>Sonraki adımlar
 - [Azure yığınına şablonlarını dağıtma](azure-stack-arm-templates.md)
 - [Şablonları geliştirmek için Azure yığını](azure-stack-develop-templates.md)

