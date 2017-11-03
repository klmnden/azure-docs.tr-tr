---
title: "Kullanıcı tarafından başlatılan Azure Otomasyonu Runbook günlük analizi eylemde | Microsoft Docs"
description: "Bu makalede, bir günlük analizi arama sonucu isteğe bağlı bir Otomasyon runbook'u çalıştırmak açıklar."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: ff938697add98f3d21b4971175432335ee2e39ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Günlük analizi günlük arama sonucu Otomasyon Runbook'tan eylemiyle alın

Azure günlük analizi'da bir günlük Arama sonuçlarından şimdi seçebileceğiniz **ele eylem** bir Otomasyon runbook'u çalıştırmak için.  Runbook gibi sorun giderme bilgilerini toplarken bir e-posta gönderebilir veya bir hizmet isteği oluşturduğunuzda bazı diğer eylemler veya sorunu düzeltmek için kullanılabilir. 

## <a name="components-and-features-used"></a>Kullanılan bileşenler ve özellikler
* [Azure Otomasyon hesabı](../automation/automation-offering-get-started.md)
* [Günlük analizi çalışma alanı](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Günlük arama runbook'tan başlatmak için

Bir olayda eylemi gerçekleştirin ve günlük arama sonuçlarınızı runbook'tan başlatmak için bir günlük arama oluşturarak başlatın ve sonuçları, bir runbook isteğe bağlı çağırabilirsiniz.  Bu Azure günlük arama özelliğini elde veya [OMS portalı](../log-analytics/log-analytics-log-searches.md).  Bu örnekte, bu özellik temel Tanıtımı ile Azure portalından bir günlük arama yapın.

1. Azure portalında Hub menüsünde **daha fazla hizmet** seçip **günlük analizi**.  
2. Günlük analizi dikey penceresinde, günlük analizi çalışma alanınız seçip çalışma dikey penceresinde **günlük arama**.  
3. Günlük arama dikey penceresinde, günlük arama yapın.  
4. Bir alan ve açılan, select solundaki elips günlük Arama sonuçlarından'ı tıklatın **ele eylem**.<br><br> ![Arama sonuçlarından gerçekleştirmesi eylemi seçin](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. Alma eylemi dikey penceresinden seçin **bir Runbook'a**ve **bir runbook'u çalıştırmak** dikey bir runbook çalıştırma seçebilirsiniz.  Herhangi bir runbook için günlük analizi çalışma alanına bağlı Otomasyon hesabı seçin.  Şunlara dikkat edin:

    * Runbook'ları etiketlere göre düzenlenir
    * Runbook giriş parametresi değerleri doğrudan arama sonucu alanlarından seçilebilir.  Aşağı açılan listesi kullanılabilir tüm alanlar seçmek için sonuç görüntüleme görünür.  
    * Runbook'u çalıştırmak seçebileceğiniz bir [karma runbook çalışanı](../automation/automation-hybrid-runbook-worker.md) bu makinenin bir üye olarak yalnızca içeren karşılık gelen bir karma Runbook çalışanı grubunuz varsa, sorunu olan makinede yüklü.  Karma çalışanı grubunun adı günlük arama sonucunda olduğu bilgisayarın adını eşleşirse, Grup otomatik olarak seçilir.    

6. Tıklattıktan sonra **çalıştırmak**, iş durumunu incelemek izin vermek için runbook iş dikey penceresi açılır.   

Olacak şekilde yapılandırılan bir runbook seçeneğini belirlerseniz [adlı günlük analizi uyarıdan](../automation/automation-invoke-runbook-from-omsla-alert.md), adlı bir giriş parametresi var. **WebhookData** diğer bir deyişle **nesne** türü.  Giriş parametre zorunluysa, runbook etkinlikleri başvurur belirli öğeleri filtre olanak tanıyan bir nesne türü, JSON biçimli dize dönüştürebilir için arama sonuçlarını runbook'a geçmesi gerekir.  Seçerek bunu **arama sonucu (nesne)** aşağı açılan listeden.<br><br> ![Runbook parametresi için Web kancası veri nesnesi seçin](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [günlük analizi oturum Arama başvurusu](log-analytics-search-reference.md) tüm arama alanlarını ve günlük analizi kullanılabilir modelleri görüntülemek için.
* Otomatik olarak bir Otomasyon runbook'u Çağır öğrenmek için gözden [bir OMS günlük analizi uyarıdan bir Azure Otomasyonu runbook çağırma](../automation/automation-invoke-runbook-from-omsla-alert.md).  