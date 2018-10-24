---
title: Azure Stack için ADFS tümleştirme doğrula
description: Azure Stack için ADFS tümleştirme doğrulamak için Azure Stack hazırlık Denetleyicisi'ni kullanın.
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
ms.openlocfilehash: 0aa13f2fced9122163d5278b5bb50cc1e11a0379
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947346"
---
# <a name="validate-adfs-integration-for-azure-stack"></a>Azure Stack için ADFS tümleştirme doğrula

Ortamınızı Azure Stack ile ADFS tümleştirme için hazır olduğunu doğrulamak için Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) kullanın. ADFS tümleştirme, veri merkezi tümleştirmesi başlamadan önce veya bir Azure Stack dağıtımdan önce doğrulamalıdır.

Hazırlık denetleyicisi doğrular:

* *Federasyon meta verileri* Federasyon için geçerli XML öğelerini içerir.
* *AD FS SSL sertifikası* alınabilir ve güven zinciri oluşturulabilir. Damgada ADFS SSL sertifikaları zincirine güvenmelidir. Sertifika aynı tarafından imzalanmalıdır *sertifika yetkilisi* Azure Stack dağıtım sertifikalar olarak veya bir güvenilir kök yetkilisi iş ortağı. Güvenilir kök yetkilisi iş ortakları tam listesi için bkz: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca
* *ADFS imzalama sertifikası* güvenilir ve değil tamamlanmak üzere süre sonu olduğu.

Azure Stack veri merkezi tümleştirmesi hakkında daha fazla bilgi için bkz: [Azure Stack'i veri merkezi tümleştirmesi - kimlik](azure-stack-integrate-identity.md)

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümünü indirin [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılanması gerekir.

**Aracın çalıştığı bilgisayarda:**

* Windows 10 veya Windows Server 2016, etki alanı bağlantısı.
* PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd'yi çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  
   > `$PSVersionTable.PSVersion`
* En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Active Directory Federasyon Hizmetleri ortamı:**

Aşağıdaki biçimlerden meta verilerin en az biri gerekir:

* AD FS federasyon meta verileri için URL. Örneğin, `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
* Federasyon meta verileri XML dosyası. Örneğin, FederationMetadata.xml

## <a name="validate-adfs-integration"></a>ADFS tümleştirme doğrula

1. Önkoşulları karşılayan bir bilgisayar, bir yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. PowerShell isteminden doğrulamayı başlatmak için aşağıdaki komutu çalıştırın. Değer için **- CustomADFSFederationMetadataEndpointUri** Federasyon meta veri URI'si olarak:

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Durum, ADFS tümleştirme gereksinimleri Tamam olduğunu onaylayın. Başarılı bir doğrulama aşağıdakine benzer görünür.

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

Üretim ortamlarında sertifika zincirleri bir iş istasyonundan işleçleri güven test Azure Stack altyapısının içinde PKI güveni duruşunu simulatorda tam olarak kabul edilmez. Azure Stack damganın genel VIP ağı bağlantı için CRL'yi PKI altyapısı gerekir.

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Doğrulama sonuçları PowerShell'de bu dosyalarının konumunu görüntüler.

Doğrulama dosyaları yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya için yazılan `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Kullanım:

* **-OutputPath** *yolu* sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.
* **-CleanReport** sonunda temizlemek için bir run komutu, parametre *AzsReadinessCheckerReport.json* önceki rapor bilgileri. Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları

Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca bilgileri günlüğe kaydeder *AzsReadinessChecker.log*.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar.

### <a name="command-not-found"></a>Komut bulunamadı

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Neden** -PowerShell Sorsorgu hazırlık denetleyicisi modülü düzgün şekilde yüklenemedi.

**Çözüm** – içeri aktarma hazırlık denetleyicisi modülü açıkça. PowerShell ve güncelleştirme aşağıdaki kodu yapıştırın \<sürüm\> şu anda yüklü olan sürümü için sürüm numarasına sahip.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Sonraki Adımlar

[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  
