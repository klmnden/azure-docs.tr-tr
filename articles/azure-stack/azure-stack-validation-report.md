---
title: Azure Stack için doğrulama raporu | Microsoft Docs
description: Doğrulama sonuçlarını gözden geçirmek için Azure Stack hazırlık denetleyicisi raporu kullanın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 06b5660a9428e98d2e99b5d447a05700968ec884
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2018
ms.locfileid: "43381922"
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
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

Çıktı aşağıdaki görüntüye benzer:

![Raporu Görüntüle](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Özet raporunu görüntüle
Rapor özetini görüntülemek için ekleyebilirsiniz **-Özet** PowerShell komut satırının sonuna geçin. Örneğin: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

Özet, sonuçları yoksa doğrulamaları gösterir ve başarılı veya başarısız tamamlandığında Doğrulamalar için gösterir. Çıktı aşağıdaki görüntüye benzer:

![Rapor Özeti](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Filtrelenmiş rapor görüntüle
Tek bir doğrulama türü üzerinde filtrelenmiş bir rapor görüntülemek için kullanın **- ReportSections** parametresiyle aşağıdaki değerlerden biri:
- Sertifika
- AzureRegistration
- AzureIdentity
- İşler   
- Tümü  

Örneğin, raporu görüntülemek için sertifikaları Özeti yalnızca, aşağıdaki PowerShell komut satırını kullanın: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Ayrıca bkz.
[Başlangıç AzsReadinessChecker cmdlet başvurusu](azure-stack-azsreadiness-cmdlet.md)
