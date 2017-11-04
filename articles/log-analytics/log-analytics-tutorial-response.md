---
title: "Azure günlük analizi uyarılarla olaylara yanıt | Microsoft Docs"
description: "Bu öğretici, önemli bilgiler OMS deponuzun tanımlamak proaktif olarak sorunları size bildiren veya düzeltmenize girişiminde eylemleri çağırmak için günlük analizi uyarılarını anlamanıza yardımcı olur."
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
ms.openlocfilehash: 3ab8d32eb4b3f2748249f40139de76c8e7f4d971
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Günlük analizi uyarılarla olaylara yanıt
Günlük analizi uyarılarını günlük analizi deponuzun önemli bilgiler tanımlayın.  Günlük aramaları düzenli aralıklarla otomatik olarak çalışacak uyarı kuralları tarafından oluşturulan ve günlük arama sonuçlarını belirli ölçütlere uyan ise bir uyarı kaydı oluşturulur ve otomatik yanıt gerçekleştirmek için yapılandırılabilir.  Bu öğretici devamıdır [oluşturma ve paylaşımı panolar günlük analizi veri](log-analytics-tutorial-dashboards.md) Öğreticisi.   

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir uyarı kuralı oluştur
> * Bir e-posta bildirimi göndermek için bir uyarı kuralı yapılandırma

Bu öğreticide örnek tamamlamak için var olan bir sanal makine olması [için günlük analizi çalışma alanına bağlı](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Uyarı oluşturma

Uyarılar, günlük aramaları düzenli aralıklarla otomatik olarak çalışacak uyarı kuralları tarafından oluşturulur.  Özel performans ölçümleri temelinde uyarılar oluşturabilir veya belirli olaylar oluşturduğunuzda, bir olay ya da olayların sayısı yokluğu oluşturulur belirli zaman penceresi içinde.  Örneğin, uyarılar, ortalama CPU kullanımı belirli bir eşiği aşarsa, bir olay belirli bir Windows hizmeti oluşturulur veya Linux arka plan programı çalışmıyor bildirmek için kullanılabilir.   Günlük arama sonuçlarını belirli ölçütlere uyan varsa bir uyarı kaydı oluşturulur. Kural, ileriye dönük olarak uyarı bildiren veya başka bir işlem çağırmak için bir veya daha fazla eylemleri otomatik olarak çalıştırabilirsiniz. 

Aşağıdaki örnekte, her bilgisayar nesnesi için bir uyarı % 90 eşiğini aştığında bir değerle sorgu oluşturacak olan bir ölçüm ölçüm uyarı kuralı oluşturun.

1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**...
2. OMS portalı seçerek ve üzerinde OMS Portalı'nı başlatma **genel bakış** sayfasında, **günlük arama**.  
3. Seçin **Sık Kullanılanlar** portalı hem de üstten **kayıtlı aramaları** sağ bölmede seçme sorgusu *Azure Vm'leri - işlemci kullanımı*.  
4. Tıklatın **uyarı** açmak için sayfanın üstündeki **uyarı kuralı Ekle** ekran.  
5. Uyarı kuralı aşağıdaki bilgilerle yapılandırın:  
   a. Sağlayan bir **adı** , uyarının gibi *aşıldı VM işlemci kullanımı > 90*  
   b. İçin **zaman penceresi**, sorgu için bir zaman aralığı belirtin *30*.  Sorgu, geçerli zaman aralığında oluşturulan kayıtları döndürür.  
   c. **Uyarı sıklığı** sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir.  Bu örnekte, belirtin *5* belirtilen bizim zaman penceresi içinde gerçekleşecek dakika.  
   d. Seçin **ölçüm ölçüm** ve girin *90* için **toplanan değeri** ve girin *3* için **tetikleyici uyarı temel alarak**   
   e. Altında **Eylemler**, e-posta bildirimini devre dışı bırakın.
6. Tıklatın **kaydetmek** uyarı kuralı tamamlamak için. Çalıştırdıktan hemen başlar.<br><br> ![Uyarı kuralı örneği](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Günlük analizi uyarı kuralları tarafından oluşturulan uyarı kayıtlarına sahip bir tür **uyarı** ve SourceSystem, **OMS**.<br><br> ![Oluşturulan uyarı olayları örneği](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Uyarı eylemleri
Böyle bir e-posta bildirimi oluştururken başlatma uyarılarla Gelişmiş eylemleri gerçekleştirebilirsiniz bir [Otomasyon runbook'u](../automation/automation-runbook-types.md), ITSM Olay yönetimi sisteminizdeki veya birlikte bir olay kaydı oluşturmak için bir Web kancası kullanın [BT Hizmet Yönetimi Bağlayıcısı çözüm](log-analytics-itsmc-overview.md) uyarı ölçütler karşılandığında yanıt olarak.   

E-posta Eylemler bir veya daha fazla alıcıya uyarı ayrıntılarını içeren bir e-posta gönderin. Posta konusunu belirtebilirsiniz, ancak buna ait günlük analizi tarafından oluşturulan standart bir biçim içeriktir.  Şimdi uyarı kuralı daha önce oluşturduğunuz ve e-posta yapılandırma güncelleştirmesini bildir, etkin bir günlük arama uyarı kaydıyla izleme yerine.     

1. OMS portalında üst menüsünde seçin **ayarları** ve ardından **uyarıları**.
2. Uyarı kuralları listesinden daha önce oluşturulan uyarı yanındaki kalem simgesine tıklayın.
3. Altında **Eylemler** bölümünde, e-posta bildirimlerini etkinleştirin.
4. Sağlayan bir **konu** e gibi *işlemci aşıldı kullanım eşiği > 90*.
5. Bir veya daha fazla e-posta alıcıları adreslerini eklemek **alıcılar** alan.  Birden fazla adres belirtirseniz, adreslerini noktalı virgül (;) ayırın.
6. Tıklatın **kaydetmek** uyarı kuralı tamamlamak için. Çalıştırdıktan hemen başlar.<br><br> ![E-posta bildirim uyarı kuralı](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide nasıl kuralları, ileriye dönük tanımlama ve günlük aramaları adresindeki çalıştırdığınızda soruna yanıt uyarı aralıklarla zamanlanmış öğrenilen ve belirli ölçütlere uyan.  

Önceden derlenmiş günlük analizi kod örnekleri görmek için bu bağlantıyı izleyin.  

> [!div class="nextstepaction"]
> [Günlük analizi kod örnekleri](powershell-samples.md)