---
title: Azure Stack tümleşik sistemleri dağıtımı için Azure Stack ortak anahtar altyapısı sertifikalarını oluşturmak | Microsoft Docs
description: Azure Stack tümleşik sistemleri için Azure Stack PKI sertifika dağıtım işlemini açıklar.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: b5adc1bb5a5aae96f37cc312588aa71e57d8342e
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42916360"
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure Stack sertifika imzalama isteği oluşturma

Bu makalede açıklanan Azure Stack hazırlık Denetleyicisi aracı kullanılabilir [PowerShell Galerisi'ndeki](https://aka.ms/AzsReadinessChecker). Aracı, sertifika imzalama isteği (CSR) Azure Stack dağıtımı için uygun olarak oluşturur. Sertifikaları istenen, oluşturulan ve dağıtımdan önce test etmek için yeterli zaman ile doğrulanması gerekir.

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker), aşağıdaki sertifika isteklerini gerçekleştirir:

 - **Standart sertifika istekleri**  
    Şunlara göre istek [Azure Stack dağıtımı için PKI sertifikaları oluşturmak](azure-stack-get-pki-certs.md).
 - **Hizmet olarak Platform**  
    İsteğe bağlı olarak hizmet olarak platform (PaaS) adları belirtilen sertifika isteği [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri - isteğe bağlı PaaS sertifikaları](azure-stack-pki-certs.md#optional-paas-certificates).



## <a name="prerequisites"></a>Önkoşullar

Sisteminizde Azure Stack dağıtımı için PKI sertifikaları için CSR(s) oluşturmadan önce aşağıdaki önkoşulları karşılamalıdır:

 - Microsoft Azure Stack hazırlık denetleyicisi
 - Sertifika öznitelikleri:
    - Bölge adı
    - Dış tam etki alanı adı (FQDN)
    - Konu
 - Windows 10 veya Windows Server 2016
 
  > [!NOTE]
  > Sertifikalarınızı aldığınızda adımlarda Sertifika yetkilinizden geri [hazırlama Azure Stack PKI sertifikaları](azure-stack-prepare-pki-certs.md) aynı sistemde tamamlanması gerekir!

## <a name="generate-certificate-signing-requests"></a>Sertifika imzalama istekleri oluştur

Hazırlama ve Azure Stack PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

1.  AzsReadinessChecker bir PowerShell İstemi'nden (5.1 veya üzeri), aşağıdaki cmdlet'i çalıştırarak yükleyin:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Bildirme **konu** sıralı bir sözlük olarak. Örneğin: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Ortak ad (CN) sağlandıysa Bu sertifika isteğini ilk DNS adı tarafından üzerine yazılır.

3.  Zaten bir çıktı dizini bildirin. Örneğin:

    ````PowerShell  
    $outputDirectory = "$ENV:USERPROFILE\Documents\AzureStackCSR"
    ````
4.  Bildirme sistem tanımlayın

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Active Directory Federasyon Hizmetleri

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Bildirme **bölge adı** ve **dış FQDN** Azure Stack dağıtım için hedeflenen.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` temelini, tüm dış DNS adlarını Azure Stack'te oluşturulur, bu örnekte, portalı olacaktır `portal.east.azurestack.contoso.com`.  

6. Sertifika imzalama istekleri için her bir DNS adı oluşturmak için:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    PaaS Hizmetleri dahil etmek için anahtar belirtin ```-IncludePaaS```

7. Alternatif olarak, geliştirme ve Test ortamları için. Birden çok konu diğer adları ile tek bir sertifika isteği oluşturmak için Ekle **- RequestType SingleCSR** parametresi ve değeri (**değil** üretim ortamları için önerilen):

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    PaaS Hizmetleri dahil etmek için anahtar belirtin ```-IncludePaaS```
    
8. Çıktıyı gözden geçirin:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

9.  Gönderme **. İstek** (dahili veya genel) CA için oluşturulan dosya.  Çıktı dizinine **başlangıç AzsReadinessChecker** sertifika yetkilisine göndermek gerekli CSR(s) içerir.  Ayrıca, bir başvuru olarak sertifika isteği oluşturma sırasında kullanılan INF dosyaları içeren bir alt dizin içerir. CA'nız karşılayan oluşturulan isteğiniz kullanarak sertifikaları oluşturur mutlaka [Azure Stack PKI gereksinimleri](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack PKI sertifikaları hazırlama](azure-stack-prepare-pki-certs.md)
