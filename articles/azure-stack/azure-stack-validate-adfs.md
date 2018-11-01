---
title: Azure Stack için AD FS tümleştirme doğrula
description: Azure Stack için AD FS tümleştirme doğrulamak için Azure Stack hazırlık Denetleyicisi'ni kullanın.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: 87e3f03ce5d4c65d5c4b1754300f5d57feca2a49
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416520"
---
# <a name="validate-ad-fs-integration-for-azure-stack"></a>Azure Stack için AD FS tümleştirme doğrula

Ortamınızı Azure Stack ile Active Directory Federasyon Hizmetleri (AD FS) tümleştirme için hazır olduğunu doğrulamak için Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) kullanın. AD FS tümleştirme, veri merkezi tümleştirmesi başlamadan önce veya bir Azure Stack dağıtımdan önce doğrulayın.

Hazırlık denetleyicisi doğrular:

* *Federasyon meta verileri* Federasyon için geçerli XML öğelerini içerir.
* *AD FS SSL sertifikası* can alınması ve yerleşik can güven zinciri. Damga üzerinde AD FS SSL sertifika zincirine güvenmelidir. Sertifika aynı tarafından imzalanmalıdır *sertifika yetkilisi* Azure Stack dağıtım sertifikalar olarak veya bir güvenilir kök yetkilisi iş ortağı. Güvenilir kök yetkilisi iş ortakları tam listesi için bkz. [TechNet](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
* *İmzalama sertifikası AD FS* güvenilir ve değil tamamlanmak üzere zaman aşımı.

Azure Stack veri merkezi tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi - kimlik](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümünü indirin [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılanması gerekir.

**Aracın çalıştığı bilgisayarda:**

* Windows 10 veya Windows Server 2016, etki alanı bağlantısı.
* PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell komutunu çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  
   > `$PSVersionTable.PSVersion`
* En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Active Directory Federasyon Hizmetleri ortamı:**

Aşağıdaki biçimlerden meta verilerin en az biri gerekir:

* AD FS federasyon meta verileri için URL. `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml` bunun bir örneğidir.
* Federasyon meta verileri XML dosyası. FederationMetadata.xml buna bir örnektir.

## <a name="validate-ad-fs-integration"></a>AD FS tümleştirme doğrula

1. Önkoşulları karşılayan bir bilgisayarda, yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. PowerShell isteminden doğrulamayı başlatmak için aşağıdaki komutu çalıştırın. Değer için **- CustomADFSFederationMetadataEndpointUri** olarak Federasyon meta verileri için URI.

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. AD FS tümleştirme gereksinimleri durumunun Tamam olduğunu doğrulayın. Başarılı bir doğrulama, aşağıdaki örneğe benzer:

    ```
    Invoke-AzsADFSValidation v1.1809.1001.1 started.

    Testing ADFS Endpoint https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

            Read Metadata:                         OK
            Test Metadata Elements:                OK
            Test SSL ADFS Certificate:             OK
            Test Certificate Chain:                OK
            Test Certificate Expiry:               OK

    Details:
    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.
    Additional help URL: https://aka.ms/AzsADFSIntegration

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsADFSValidation Completed
    ```

Üretim ortamlarında sertifika zincirleri bir işlecin iş istasyonundan güven test tam olarak Azure Stack altyapısının içinde PKI güveni duruşunu göstergesi değildir. Azure Stack damganın genel VIP ağları, bağlantı için CRL'yi PKI altyapısı gerekir.

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Bu dosyaların konumunu PowerShell doğrulama sonuçları görüntülenir.

Doğrulama dosyaları yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya için yazılan `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Kullanım:

* **-OutputPath**: *yolu* sonunda farklı rapor konumunu belirtmek için bir run komutu, parametre.
* **-CleanReport**: önceki rapor bilgilerin AzsReadinessCheckerReport.json temizlemek için çalıştırma komutunun sonuna parametre. Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları

Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görünür. Araç ayrıca bilgileri günlüğe kaydeder *AzsReadinessChecker.log*.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar.

### <a name="command-not-found"></a>Komut bulunamadı

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Neden**: PowerShell Sorsorgu hazırlık denetleyicisi modülü düzgün şekilde yüklenemedi.

**Çözüm**: açıkça hazırlık denetleyicisi modülü içeri aktarın. PowerShell ve güncelleştirme aşağıdaki kodu yapıştırın \<sürüm\> şu anda yüklü sürüm numarasıyla.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Sonraki adımlar

[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  
