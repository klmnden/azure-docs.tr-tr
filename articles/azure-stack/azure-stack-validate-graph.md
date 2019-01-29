---
title: Azure Graph entegrasyonuna yönelik Azure Stack doğrulama
description: Azure Stack için graph entegrasyonuna doğrulamak için Azure Stack hazırlık denetleyicisi kullanın.
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
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: fbc6e7586e116df1fa19c6c9e0c5a5b17f00d646
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55096781"
---
# <a name="validate-graph-integration-for-azure-stack"></a>Azure Stack için Graph entegrasyonuna doğrula

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) ortamınızın graph entegrasyonuna Azure Stack için hazır olduğunu doğrulamak için kullanın. Graph entegrasyonuna veri merkezi tümleştirmesi başlamadan önce veya bir Azure Stack dağıtımdan önce doğrulayın.

Hazırlık denetleyicisi doğrular:

* Graph entegrasyonuna yönelik oluşturulan hizmet hesabı için kimlik bilgilerini Active Directory sorgusu için uygun haklara sahip.
* *Genel katalog* contactable ve çözülebilir.
* KDC çözülebilir ve contactable.
* Gerekli ağ bağlantısı yerdir.

Azure Stack veri merkezi tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi - kimlik](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümünü indirin [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılanması gerekir.

**Aracın çalıştığı bilgisayarda:**

* Windows 10 veya Windows Server 2016, etki alanı bağlantısı.
* PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell komutunu çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  
   > `$PSVersionTable.PSVersion`
* Active Directory PowerShell modülü.
* En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Active Directory ortamında:**

* Kullanıcı adı ve parola mevcut Active Directory örneğinde graph hizmeti için bir hesap için bu seçeneği belirleyin.
* Active Directory orman kök FQDN belirleyin.

## <a name="validate-the-graph-service"></a>Graf hizmeti doğrula

1. Önkoşulları karşılayan bir bilgisayarda, yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın *$graphCredential* graf hesabına değişken. Değiştirin `contoso\graphservice` kullanarak hesabınızla `domain\username` biçimi.

    `$graphCredential = Get-Credential contoso\graphservice -Message "Enter Credentials for the Graph Service Account"`

1. PowerShell komut isteminde, graf hizmetinin doğrulamayı başlatmak için aşağıdaki komutu çalıştırın. Değer için **- ForestFQDN** FQDN için orman kökü olarak.

     `Invoke-AzsGraphValidation -ForestFQDN contoso.com -Credential $graphCredential`

1. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Graf tümleştirme gereksinimleri durumunun Tamam olduğunu doğrulayın. Başarılı bir doğrulama, aşağıdaki örneğe benzer:

    ```
    Testing Graph Integration (v1.0)
            Test Forest Root:            OK
            Test Graph Credential:       OK
            Test Global Catalog:         OK
            Test KDC:                    OK
            Test LDAP Search:            OK
            Test Network Connectivity:   OK

    Details:

    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.

    Additional help URL: https://aka.ms/AzsGraphIntegration

    AzsReadinessChecker Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log

    AzsReadinessChecker Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsGraphValidation Completed
    ```

Üretim ortamlarında bir işlecin iş istasyonu arasında ağ bağlantısı sınaması tam olarak Azure Stack için kullanılabilir bağlantı göstergesi değildir. Azure Stack damganın genel VIP ağları kimlik tümleştirme gerçekleştirmek LDAP trafik için bağlantı gerekir.

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Bu dosyaların konumunu PowerShell doğrulama sonuçları görüntülenir.

Doğrulama dosyaları yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya için yazılan `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Kullanım:

* **-OutputPath**: *Yolu* sonunda farklı rapor konumunu belirtmek için bir run komutu, parametre.
* **-CleanReport**: Sonunda temizlemek için bir run komutu, parametre *AzsReadinessCheckerReport.json* önceki rapor bilgileri. Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları

Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görünür. Araç ayrıca bilgileri günlüğe kaydeder *AzsGraphIntegration.log*.

## <a name="next-steps"></a>Sonraki adımlar

[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  
