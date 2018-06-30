---
title: Kullanıcı tarafından başlatılan Azure Otomasyonu Runbook günlük analizi eylemde | Microsoft Docs
description: Bu makalede, bir günlük analizi arama sonucu isteğe bağlı bir Otomasyon runbook'u çalıştırmak açıklar.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 48e184206d9f1cf5b14c2e18b4ca1ed260d70161
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130225"
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Günlük analizi günlük arama sonucu Otomasyon Runbook'tan eylemiyle alın

Azure günlük analizi'da bir günlük Arama sonuçlarından şimdi seçebileceğiniz **ele eylem** bir Otomasyon runbook'u çalıştırmak için.  Runbook gibi sorun giderme bilgilerini toplarken bir e-posta gönderebilir veya bir hizmet isteği oluşturduğunuzda bazı diğer eylemler veya sorunu düzeltmek için kullanılabilir. 

## <a name="components-and-features-used"></a>Kullanılan bileşenler ve özellikler
* [Azure Otomasyon hesabı](../automation/automation-offering-get-started.md)
* [Log Analytics çalışma alanı](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Günlük arama runbook'tan başlatmak için

Bir olayda eylemi gerçekleştirin ve günlük arama sonuçlarınızı runbook'tan başlatmak için bir günlük arama oluşturarak başlatın ve sonuçları, bir runbook isteğe bağlı çağırabilirsiniz.  Bu günlük arama özelliğini sağlanabilir [Azure portal](../log-analytics/log-analytics-log-search-new.md).  Bu örnekte, bu özellik temel Tanıtımı ile Azure portalından bir günlük arama yapın.

1. Azure portalında tıklatın **tüm hizmetleri** seçip **günlük analizi**.  
2. Günlük analizi çalışma alanınızı seçin.
3. Çalışma alanında, seçin **günlük arama**.  
4. Günlük arama sayfasında, bir günlük arama yapın.  
5. Bir alan ve açılan, select solundaki elips günlük Arama sonuçlarından'ı tıklatın **ele eylem**.<br><br> ![Arama sonuçlarından gerçekleştirmesi eylemi seçin](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
6. Seçin **bir Runbook'a** ve çalıştırmak için bir runbook seçin.  Herhangi bir runbook için günlük analizi çalışma alanına bağlı Otomasyon hesabı seçin.  Şunlara dikkat edin:

    * Runbook'ları etiketlere göre düzenlenir
    * Runbook giriş parametresi değerleri doğrudan arama sonucu alanlarından seçilebilir.  Aşağı açılan listesi kullanılabilir tüm alanlar seçmek için sonuç görüntüleme görünür.  
    * Runbook'u çalıştırmak seçebileceğiniz bir [karma runbook çalışanı](../automation/automation-hybrid-runbook-worker.md) bu makinenin bir üye olarak yalnızca içeren karşılık gelen bir karma Runbook çalışanı grubunuz varsa, sorunu olan makinede yüklü.  Karma çalışanı grubunun adı günlük arama sonucunda olduğu bilgisayarın adını eşleşirse, Grup otomatik olarak seçilir.    

6. Tıklattıktan sonra **çalıştırmak**, iş durumunu incelemek izin vermek için runbook işi sayfası açılır.   

Olacak şekilde yapılandırılan bir runbook seçeneğini belirlerseniz [adlı günlük analizi uyarıdan](../automation/automation-invoke-runbook-from-omsla-alert.md), adlı bir giriş parametresi var. **WebhookData** diğer bir deyişle **nesne** türü.  Giriş parametre zorunluysa, runbook etkinlikleri başvurur belirli öğeleri filtre olanak tanıyan bir nesne türü, JSON biçimli dize dönüştürebilir için arama sonuçlarını runbook'a geçmesi gerekir.  Seçerek bunu **arama sonucu (nesne)** aşağı açılan listeden.<br><br> ![Runbook parametresi için Web kancası veri nesnesi seçin](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [günlük analizi oturum Arama başvurusu](log-analytics-search-reference.md) tüm arama alanlarını ve günlük analizi kullanılabilir modelleri görüntülemek için.
* Otomatik olarak bir Otomasyon runbook'u Çağır öğrenmek için gözden [günlük analizi uyarıdan bir Azure Otomasyonu runbook çağırma](../automation/automation-invoke-runbook-from-omsla-alert.md).  