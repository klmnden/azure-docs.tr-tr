---
title: Azure Stack için doğrulama raporu | Microsoft Docs
description: Doğrulama sonuçlarını gözden geçirmek için Azure Stack hazırlık denetleyicisi raporu kullanın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/23/2018
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 0eed7943d782bda134c2df2de6cf9eeeb58bfa3b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243874"
---
# <a name="azure-stack-validation-report"></a>Azure Stack doğrulama raporu
Dağıtım ve bir Azure Stack ortamı hizmet verme desteği doğrulamaları çalıştırmak için Azure Stack hazırlık Denetleyicisi aracını kullanın. Araç sonuçları bir .json rapor dosyasına yazar. Raporu, Azure Stack dağıtım önkoşulları durumuyla ilgili ayrıntılı ve Özet verileri görüntüler. Rapor, ayrıca var olan Azure Stack dağıtımları için gizli dizileri döndürme hakkında bilgi görüntüler.  

 ## <a name="where-to-find-the-report"></a>Rapor yeri
Araç çalıştığında sonuçlarını kaydeder. **AzsReadinessCheckerReport.json**. Aracı adlı bir günlük oluşturur **AzsReadinessChecker.log**. Doğrulama sonuçları PowerShell'de bu dosyalarının konumunu görüntüler.

![doğrulamayı Çalıştır](./media/azure-stack-validation-report/validation.png)

Her iki dosya sonraki doğrulama denetimlerini aynı bilgisayarda çalıştırıldığında sonuçlarını kalıcı hale getirin.  Örneğin, Aracı sertifikalarını doğrulamak için Azure kimlik doğrulamak için tekrar çalıştırın ve ardından kaydı doğrulamak için üçüncü kez çalıştırılabilir. Elde edilen .json raporda tüm üç doğrulama sonuçları kullanılabilir.  

Varsayılan olarak, her iki dosya için yazılan *C:\Users\<kullanıcıadı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Kullanım **- OutputPath** ***&lt;yolu&gt;*** sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.   
- Kullanım **- CleanReport** parametre bilgilerini temizlemek için bir run komutu sonunda *AzsReadinessCheckerReport.json*. Önceki hakkında aracını çalıştırır.

## <a name="view-the-report"></a>Raporu görüntüleyin
PowerShell'de raporunu görüntülemek için değeri olarak rapor için bir yol sağlamanız **- ReportPath**. Bu komut, raporun içeriğini görüntüler ve sonuçları henüz sahip olmayan doğrulamaları tanımlar.

Örneğin, raporun bulunduğu konuma açık olan bir PowerShell isteminden raporu görüntülemek için çalıştırın: 
   > `Read-AzsReadinessReport -ReportPath .\AzsReadinessReport.json` 

Çıktı aşağıdakine benzer:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation results not available.

############### Registration Validation Summary ###############

Azure Registration Validation results not available.

############### Azure Identity Results ###############

Test                          : ServiceAdministrator
Result                        : OK
AAD Service Admin             : admin@contoso.onmicrosoft.com
Azure Environment             : AzureCloud
Azure Active Directory Tenant : contoso.onmicrosoft.com
Error Details                 : 

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.

############### AzsReadiness Job Summary ###############

Index             : 0
Operations        : 
StartTime         : 2018/10/22 14:24:16
EndTime           : 2018/10/22 14:24:19
Duration          : 3
PSBoundParameters : 
````

## <a name="view-the-report-summary"></a>Özet raporunu görüntüle
Rapor özetini görüntülemek için ekleyebilirsiniz **-Özet** PowerShell komut satırının sonuna geçin. Örneğin: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -summary`  

Özet, sonuçları yoksa doğrulamaları gösterir ve başarılı veya başarısız tamamlandığında Doğrulamalar için gösterir. Çıktı aşağıdakine benzer:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

    Certificate Validation found no errors or warnings.
    
############### Registration Validation Summary ###############

    Registration Validation found no errors or warnings.

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.
````


## <a name="view-a-filtered-report"></a>Filtrelenmiş rapor görüntüle
Tek bir doğrulama türü üzerinde filtrelenmiş bir rapor görüntülemek için kullanın **- ReportSections** parametresiyle aşağıdaki değerlerden biri:
- Sertifika
- AzureRegistration
- AzureIdentity
- Graf
- ADFS
- İşler   
- Tümü  

Örneğin, raporu görüntülemek için sertifikaları Özeti yalnızca, aşağıdaki PowerShell komut satırını kullanın: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Ayrıca bkz.
