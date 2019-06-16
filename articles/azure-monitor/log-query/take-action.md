---
title: Kullanıcı tarafından başlatılan Log analytics'te Azure Otomasyonu Runbook eylemini | Microsoft Docs
description: Bu makalede bir Log Analytics arama sonucu üzerine bir Otomasyon runbook'unu çalıştırmayı öğrenin.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/06/2019
ms.author: magoedte
ms.openlocfilehash: 9194d5fe6553607ac5a0bb4e133da97f53790984
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61424763"
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Log Analytics günlük araması sonuç Otomasyon Runbook'u ile bir eylem

> [!NOTE]
> Arama sonuçlarından bir runbook başlatma, 15 Şubat 2019 kullanım dışı bırakılacak Klasik günlük araması Portalı'nın bir özelliğidir. Diğer Eylemler yanı sıra bir runbook başlatabilirsiniz bir eylem grubu yapılandırabileceğiniz bir [uyarı kuralı](../platform/alerts-log.md) Azure İzleyici'de.

Azure Log Analytics'da bir günlük Arama sonuçlarından artık seçebilirsiniz **harekete** Otomasyon runbook'u çalıştırmak için.  Runbook, sorunu düzeltmek veya bu gibi sorun giderme bilgilerini toplarken bir e-posta gönderebilir veya bir hizmet isteği oluşturduğunuzda bazı diğer eylemler için kullanılabilir. 


## <a name="components-and-features-used"></a>Kullanılan bileşenler ve özellikler
* [Azure Otomasyon hesabı](../../automation/automation-quickstart-create-account.md)
* [Log Analytics çalışma alanı](../../azure-monitor/log-query/log-query-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Runbook günlük araması'nı başlatmak için

Bir olayı harekete ve günlük arama sonuçlarınızı runbook başlatmak için bir günlük araması oluşturma işlemiyle başlayın ve sonuçları bir runbook isteğe bağlı çağırabilirsiniz. Bu, Klasik günlük arama özelliğinden gerçekleştirilebilir [Azure portalında](../../azure-monitor/log-query/log-query-overview.md). Bu örnekte, bu özelliğin temel bir örnek ile Azure portalından bir günlük araması gerçekleştirin.

1. Azure portalında **tüm hizmetleri** seçip **Log Analytics**.  
2. Log Analytics çalışma alanınızı seçin.
3. Çalışma alanında, seçin **günlükleri (Klasik)** .  
4. Günlük araması sayfasında, bir günlük araması gerçekleştirin.  
5. Günlük araması sonuçlarından sol tarafındaki bir alanların ve açılan, select üç nokta işaretine **üzerinde eylem gerçekleştirmenize**.<br><br> ![Arama sonuçlarından Al eylemini seçin](./media/take-action/log-search-takeaction-menuoption.png) 
6. Seçin **bir runbook'u çalıştırma** ve çalıştırmak için bir runbook seçin.  Herhangi bir runbook Log Analytics çalışma alanına bağlı Otomasyon hesabı seçin.  Şunlara dikkat edin:

    * Runbook'ları etiketlere göre düzenlenir.
    * Runbook giriş parametresi değerlerini doğrudan arama sonucu alanlardan seçilebilir.  Sonuç seçmek için kullanılabilir tüm alanları görüntüleme açılır listede görüntülenir.  
    * Runbook'u çalıştırmak seçebileceğiniz bir [karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md) yalnızca bu makinenin bir üye olarak içeren karşılık gelen bir karma Runbook çalışanı grubu varsa, sorunu olan makinede yüklü.  Karma çalışanı grubu adını günlük arama sonucu olan bir bilgisayar adı eşleşiyorsa, Grup otomatik olarak seçilir.    

6. Tıkladıktan sonra **çalıştırma**, iş durumunu gözden geçirmek, izin vermek için runbook işi sayfası açılır.   

Olmak üzere yapılandırılmış bir runbook seçeneğini belirlerseniz [bir Log Analytics uyarısından adlı](../../automation/automation-create-alert-triggered-runbook.md), adlı bir giriş parametresine sahiptir **WebhookData** diğer bir deyişle **nesne** türü.  Giriş parametre zorunluysa, runbook etkinlikleri Bakacağınız belirli öğeleri üzerinde filtreleme yapmanızı sağlayan bir nesne türü, JSON ile biçimlendirilmiş dizeyi dönüştürebilirsiniz için arama sonuçlarını runbook'a geçirilecek gerekir.  Seçerek bunu **arama sonucu (nesne)** aşağı açılan listeden.<br><br> ![Runbook parametresi için Web kancası veri nesnesini seçin](media/take-action/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [Log Analytics günlük araması başvuru](../../azure-monitor/log-query/log-query-overview.md) tüm özellikleri Log Analytics'te kullanılabilir ve arama alanlarını görüntüleyin.
* Bir Otomasyon runbook'unu otomatik olarak çağırmak nasıl bilgi edinmek için [bir Log Analytics uyarısından Azure Otomasyonu runbook'u çağırma](../../automation/automation-create-alert-triggered-runbook.md).  
