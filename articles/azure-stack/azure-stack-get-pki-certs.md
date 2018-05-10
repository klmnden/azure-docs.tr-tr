---
title: Azure tümleşik yığını systems dağıtımı Azure yığın ortak anahtar altyapısı sertifikalarını oluşturmak | Microsoft Docs
description: Azure tümleşik yığını sistemler için Azure yığın PKI sertifika dağıtım işlemini açıklar.
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
ms.date: 04/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 17737c2b272f2a123df3d58c62c471b3da5bebe1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Azure yığın sertifika imzalama isteği oluşturma

Bu makalede açıklanan Azure yığın hazırlık Denetleyicisi aracı kullanılabilir [PowerShell Galerisi'nden](https://aka.ms/AzsReadinessChecker). Araç sertifika imzalama isteği (CSR) bir Azure yığın dağıtımına uygun olarak oluşturur. Sertifikaları istenen, oluşturulan ve dağıtımından önce test etmek için yeterli süre ile doğrulanması gerekir.

Azure yığın hazırlık Denetleyicisi aracını (AzsReadinessChecker) aşağıdaki sertifika isteklerini gerçekleştirir:

 - **Standart sertifika istekleri**  
    Göre isteği [Azure yığın dağıtımı için PKI sertifikaları oluşturmak](azure-stack-get-pki-certs.md).
 - **İstek türü**  
    Sertifika imzalama isteği tek bir istekte ya da birden çok istek olup olmadığını belirtir.
 - **Hizmet olarak Platform**  
    İsteğe bağlı olarak hizmet olarak platform (PaaS) adlarını belirtildiği şekilde sertifikalarla isteği [Azure yığın ortak anahtar altyapısı sertifika gereksinimleri - isteğe bağlı PaaS sertifikaları](azure-stack-pki-certs.md#optional-paas-certificates).



## <a name="prerequisites"></a>Önkoşullar

Sisteminizi Azure yığın dağıtımı için PKI sertifikaları için CSR(s) oluşturmadan önce aşağıdaki önkoşulları yerine getirmeniz:

 - Microsoft Azure yığın hazırlık denetleyicisi
 - Sertifika öznitelikleri:
    - Bölge adı
    - Dış tam etki alanı adı (FQDN)
    - Konu
 - Windows 10 veya Windows Server 2016
 
  > [!NOTE]
  > Aldığınızda sertifikalarınızı adımlarda, sertifika yetkilisi tarafından geri [hazırlama Azure yığın PKI sertifikaları](azure-stack-prepare-pki-certs.md) aynı sistemde tamamlanması gerekir!

## <a name="generate-certificate-signing-requests"></a>İsteklere imzalama sertifikası oluştur

Hazırlama ve Azure yığın PKI sertifikalarını doğrulamak için aşağıdaki adımları kullanın: 

1.  AzsReadinessChecker bir PowerShell isteminde (5.1 veya üstü), aşağıdaki cmdlet'i çalıştırarak yükleyin:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Bildirme **konu** sıralı bir sözlük olarak. Örneğin: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Ortak ad (CN) belirtilirse bu sertifika isteğini ilk DNS adı tarafından üzerine yazılır.

3.  Zaten bir çıkış dizini bildirin:

    ````PowerShell  
    $outputDirectory = "$ENV:USERNAME\Documents\AzureStackCSR" 
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

5. Bildirme **bölge adı** ve bir **dış FQDN** Azure yığın dağıtım için hedeflenen.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` üzerinde Azure yığınında tüm dış DNS adlarını oluşturulur, bu örnekte temelini oluşturur, portal olacaktır `portal.east.azurestack.contoso.com`.

6. Birden fazla konu alternatif adı ile tek bir sertifika isteği oluşturmak için:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Eklenecek anahtar PaaS Hizmetleri belirtin ```-IncludePaaS```

7. İmzalama istekleri her bir DNS adı için tek tek sertifikasını oluşturmak için:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Eklenecek anahtar PaaS Hizmetleri belirtin ```-IncludePaaS```

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

9.  Gönderme **. İSTEĞİ** (dahili veya genel) CA için oluşturulan dosya.  Çıktı dizini **başlangıç AzsReadinessChecker** bir sertifika yetkilisine göndermek için gereken CSR(s) içerir.  Ayrıca, bir başvuru olarak sertifika isteği oluşturma sırasında kullanılan INF dosyaları içeren bir alt dizini içerir. CA'nız karşılayan oluşturulan isteğiniz kullanarak sertifikaları oluşturur mutlaka [Azure yığın PKI gereksinimleri](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığın PKI sertifikaları hazırlama](azure-stack-prepare-pki-certs.md)
