---
title: "Azure Log Analytics Uyarılarıyla olaylara yanıt verme | Microsoft Docs"
description: "Bu öğretici, OMS deponuzdaki önemli bilgileri belirlemek ve proaktif olarak sorunları bildirip bu sorunları düzeltme girişiminde bulunmak amacıyla eylemler çağırmak için Log Analytics’te uyarıları anlamanıza yardımcı olur."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: fcfaa849f67ffcfa69672d116837e96d318c2124
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Log Analytics Uyarılarıyla olaylara yanıt verme
Log Analytics’teki uyarılar, Log Analytics deponuzdaki önemli bilgileri belirler. Bunlar düzenli aralıklarla otomatik olarak günlük aramaları çalıştıran uyarı kuralları tarafından oluşturulur. Günlük aramasının sonuçları belirli ölçütlerle eşleşirse bir uyarı kaydı oluşturulur ve otomatik yanıt gerçekleştirmek için yapılandırılabilir.  Bu öğretici, [Log Analytics verilerinin panolarını oluşturma ve paylaşma](log-analytics-tutorial-dashboards.md) öğreticisinin devamı niteliğindedir.   

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uyarı kuralı oluşturma
> * E-posta bildirimi göndermek için bir uyarı kuralını yapılandırma

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin [Log Analytics çalışma alanına bağlanmış](log-analytics-quick-collect-azurevm.md) olması gerekir.  

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinde Azure portalında oturum açın. 

## <a name="create-alerts"></a>Uyarı oluşturma

Düzenli aralıklarla otomatik olarak günlük aramaları çalıştıran uyarı kuralları tarafından uyarılar oluşturulur.  Belirli performans ölçümleri temelinde veya belirli bir zaman aralığında bir olay sayısı oluşturulduğunda, bir olay olmadığında ya da belirli olaylar oluşturulduğunda uyarılar oluşturabilirsiniz.  Örneğin, ortalama CPU kullanımı belirli bir eşiği aştığında veya belirli bir Windows hizmeti ya da Linux programı çalışmıyorken bir olay oluşturulduğunda size bildirim göndermek için uyarılar kullanılabilir.   Günlük aramasının sonuçları belirli ölçütlerle eşleşiyorsa bir uyarı kaydı oluşturulur. Daha sonra kural, uyarı hakkında proaktif olarak size bildirim göndermek veya başka bir işlem çağırmak için otomatik olarak bir ya da daha fazla eylem çalıştırabilir. 

Aşağıdaki örnekte, %90 eşiğini aşan değere sahip sorgudaki her bir bilgisayar nesnesi için uyarı oluşturan bir ölçüm ölçüsü uyarı kuralı oluşturursunuz.

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. OMS Portalını seçerek OMS portalını başlatın ve **Genel Bakış** sayfasında **Günlük Araması**’nı seçin.  
3. Portalın üst kısmından **Sık Kullanılanlar**’ı seçin ve sağdaki **Kayıtlı Aramalar** bölmesinde *Azure VM’leri - İşlemci Kullanımı* sorgusunu seçin.  
4. Sayfanın üst kısmındaki **Uyarı**’ya tıklayarak **Uyarı Kuralı Ekle** ekranını açın.  
5. Aşağıdaki bilgilerle uyarı kuralını yapılandırın:  
   a. Uyarınız için *VM işlemci kullanımı aşıldı >90* gibi bir **Ad** belirtin  
   b. **Zaman Aralığı** için, sorguya ilişkin *30* gibi bir zaman aralığı belirtin.  Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür.  
   c. **Uyarı Sıklığı**, sorgunun çalıştırılması gereken sıklığı belirtir.  Bu örnek için, belirtilen zaman aralığımız içinde gerçekleşen *5* dakika değerini belirtin.  
   d. **Ölçüm Ölçüsü**’nü seçin ve **Toplam Değer** için *90* değerini ve **Şuna bağlı olarak uyarıyı tetikle:**  için *3* değerini girin  
   e. **Eylemler** bölümünde e-posta bildirimini devre dışı bırakın.
6. **Kaydet**’e tıklayarak uyarı kuralını tamamlayın. Hemen çalıştırılmaya başlar.<br><br> ![Uyarı kuralı örneği](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Log Analytics’teki uyarı kuralları tarafından oluşturulan uyarı kayıtlarının Türü **Uyarı** ve Kaynak Sistemi **OMS**’dir.<br><br> ![Oluşturulan Uyarı olayları örneği](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Uyarı eylemleri
Uyarı ölçütleri karşılandığında yanıt olarak [BT Hizmet Yönetimi Bağlayıcısı çözümü](log-analytics-itsmc-overview.md) ile veya ITSM olay yönetimi sisteminizde olay kaydı oluşturmak amacıyla bir web kancası kullanma, [Otomasyon runbook'u](../automation/automation-runbook-types.md) başlatma, e-posta bildirimi oluşturma gibi uyarılarla gelişmiş eylemler gerçekleştirebilirsiniz.   

E-posta eylemleri, bir veya daha fazla alıcıya uyarının ayrıntılarını içeren bir e-posta gönderir. Postanın konusunu belirtebilirsiniz ancak içeriği, Log Analytics tarafından oluşturulan standart bir biçimdedir.  Şimdi daha önceden oluşturulan uyarı kuralını güncelleştirelim ve günlük araması ile uyarı kaydını etkin şekilde izlemek yerine size e-posta bildirimi gönderilecek şekilde kuralı yapılandıralım.     

1. OMS portalında üst menüden **Ayarlar**’ı ve sonra **Uyarılar**’ı seçin.
2. Uyarı kuralları listesinden, daha önceden oluşturulan uyarının yanındaki kalem simgesine tıklayın.
3. **Eylemler** bölümünde e-posta bildirimlerini etkinleştirin.
4. E-posta için *İşlemci kullanımı eşiği aştı >90* gibi bir **Konu** belirtin.
5. **Alıcılar** alanına bir veya daha fazla e-posta alıcısının adresini ekleyin.  Birden fazla adres belirtirseniz, adresleri noktalı virgül (;) ile ayırın.
6. **Kaydet**’e tıklayarak uyarı kuralını tamamlayın. Hemen çalıştırılmaya başlar.<br><br> ![E-posta bildirimi ile uyarı kuralı](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, uyarı kurallarının zamanlanan aralıklarda günlük aramaları çalıştırdığında ve belirli bir ölçütle eşleştiğinde proaktif şekilde bir sorunu nasıl belirleyebildiğini ve yanıtlayabildiğini öğrendiniz.  

Önceden oluşturulmuş Log Analytics betik örneklerini görmek için bu bağlantıyı izleyin.  

> [!div class="nextstepaction"]
> [Log Analytics betik örnekleri](powershell-samples.md)