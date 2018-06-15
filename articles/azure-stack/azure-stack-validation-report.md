---
title: Doğrulama raporu Azure yığınının | Microsoft Docs
description: Doğrulama sonuçlarını gözden geçirmek için Azure yığın hazırlık denetleyicisi raporu kullanın.
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
ms.openlocfilehash: a0ca0ae3ed615f6bc2774364f7a443023b911b5d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937823"
---
# <a name="azure-stack-validation-report"></a>Azure yığın doğrulama raporu
Dağıtım ve bir Azure yığın ortamını bakım desteği doğrulamaları Azure yığın hazırlık Denetleyicisi aracını çalıştırır. Aracı doğrulama sonuçlarını bir .json rapor dosyasına yazar. Rapor, mevcut Azure yığın dağıtımları için ayrıntılı ve Özet verileri Azure yığınının dağıtımı için Önkoşullar durumunu ve gizli anahtarları döndürme görüntüler.  

 ## <a name="where-to-find-the-report"></a>Raporu nerede bulacağını
Aracı çalıştırıldığında sonuçları günlüklerini **AzsReadinessCheckerReport.json**. Aracı adlı bir günlük oluşturur **AzsReadinessChecker.log**. Bu dosyaların konumunu PowerShell'de doğrulama sonuçlarını görüntüler.

![Çalışma doğrulama](./media/azure-stack-validation-report/validation.png)

Her iki dosya aynı bilgisayarda çalıştırdığınızda sonraki doğrulama denetimlerin sonuçlarını kalıcı olmasını sağlar.  Örneğin, aracı sertifikaları doğrulamak için Azure kimlik doğrulamak için yeniden çalıştırın ve kayıt doğrulamak için üçüncü kez çalıştırılabilir. Tüm üç doğrulama sonuçlarını elde edilen .json raporda kullanılabilir.  

Varsayılan olarak, her iki dosya yazılan *C:\Users\<kullanıcı adı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Kullanım **- OutputPath** ***&lt;yolu&gt;*** farklı rapor konumunu belirtmek için çalışma komut satırı sonunda parametresi.   
- Kullanım **- CleanReport** parametre bilgilerini temizlemek için run komutunun sonuna *AzsReadinessCheckerReport.json*. Önceki hakkında aracını çalıştırır.

## <a name="view-the-report"></a>Raporu görüntüleyin
PowerShell'de raporunu görüntülemek için değeri olarak rapor yolu tedarik **- ReportPath**. Bu komut raporun içeriğini görüntüler ve ayrıca sonuçları henüz yok doğrulamaları tanımlar.

Örneğin, raporun bulunduğu konuma açık bir PowerShell isteminden raporunu görüntülemek için çalıştırın: 
   > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json` 

Çıktı aşağıdaki görüntüde benzer:

![Raporu Görüntüle](./media/azure-stack-validation-report/view-report.png)

## <a name="view-the-report-summary"></a>Özet raporunu görüntüle
Bir özeti raporu görüntülemek için ekleyebilirsiniz **-Özet** PowerShell komut satırından sonunu geçin. Örneğin: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -summary`  

Özet sonuçları yok doğrulamaları gösterir ve başarılı veya başarısız tam doğrulamaların gösterir. Çıktı aşağıdaki görüntüde benzer:

![raporu-Özet](./media/azure-stack-validation-report/report-summary.png)


## <a name="view-a-filtered-report"></a>Filtrelenmiş raporunu görüntüle
Tek bir doğrulama türü üzerinde filtrelenmiş bir raporu görüntülemek için kullanın **- ReportSections** parametre ve görüntülemek istediğiniz doğrulama türü için karşılık gelen aşağıdaki değerlerden birini belirtin:
- Sertifika
- AzureRegistration
- AzureIdentity
- İşler   
- Tümü  

Örneğin, raporu görüntülemek için sertifikaları Özeti yalnızca, aşağıdaki PowerShell komut satırını kullanın: 
 > `Start-AzsReadinessChecker -ReportPath .\AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Ayrıca bkz.
[Start-AzsReadinessChecker cmdlet başvurusu](azure-stack-azsreadiness-cmdlet.md)
