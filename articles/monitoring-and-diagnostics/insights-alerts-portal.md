---
title: "Azure Hizmetleri için-uyarı oluşturma Azure portalı | Microsoft Docs"
description: "Belirttiğiniz koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: 3e09c145d35665ec1c2467b60f06191ac51a5c16
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Ölçüm uyarılar için Azure services - Azure İzleyicisi'nde oluşturma Azure portalı
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Genel Bakış
Bu makalede Azure portalını kullanarak Azure ölçüm uyarılarını ayarlama gösterilmiştir. 

İzleme ölçümlerini ya da olayları, Azure hizmetlerinizi göre bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler koşul ilk ve ardından daha sonra ne zaman, koşul artık karşılanıp zaman.    
* **Etkinlik günlüğü olaylarını** -bir uyarıyı tetiklemek *her* olay veya yalnızca belirli olaylar meydana gelir. Daha fazla bilgi edinmek [etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md).

Tetikler, aşağıdakileri yapmak için bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* bir Web kancası çağırın
* (yalnızca Azure portalından) Azure bir runbook'un yürütülmesi Başlat

> [!NOTE]
> Azure İzleyici artık genel önizlemede yakın gerçek zamanlı ölçüm uyarıları destekler. Bu eylem gruplarını kullanın. Daha fazla bilgi edinmek [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md).
>
>

Yapılandırma ve kullanma ölçüm uyarı kuralları hakkında bilgi alın

* [Azure portalı](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal ile bir ölçüm bir uyarı kuralı oluşturma
1. İçinde [portal](https://portal.azure.com/), izleme ilgilenen kaynak bulup seçin.

2. Seçin **uyarıları** veya **uyarı kuralları** izleme bölümünde. Metin ve simge farklı kaynaklar için biraz değişebilir.  

    ![İzleme](./media/insights-alerts-portal/AlertRulesButton.png)

3. Seçin **uyarı Ekle** komut ve alanları doldurun.

    ![Uyarı Ekle](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Ad** , uyarı kuralı ve seçin bir **açıklama**, bildirim e-postalarda da gösterir.

5. Seçin **ölçüm** izlemek ve ardından istediğiniz bir **koşulu** ve **eşik** ölçüm için değer. Ayrıca tercih **süresi** ölçüm kuralı uyarı Tetikleyicileri önce karşılanması gereken süre. Dolayısıyla örneğin CPU % 80 5 dakika boyunca sürekli olarak yukarıdaki kaldığında "son 5 dakika boyunca" dönem kullanıyorsanız ve Uyarınız % 80 CPU görünüyor, uyarıyı tetikleyen. İlk tetikleyici oluşur sonra 5 dakika boyunca % 80 CPU kaldığında oluşan yeniden tetikler. CPU ölçüm ölçüm dakikada bir gerçekleşir.

6. Denetleme **e-posta sahipleri...**  yöneticileri ve ortak Yöneticiler uyarı oluşturulduğunda e-posta gönderilip istiyorsanız.

7. Uyarı oluşturulduğunda bir bildirim almak için ek e-postaları istiyorsanız, bunları ekleyin **ek yönetici email(s)** alan. Birden çok e-postaları - noktalı virgülle ayırın  *email@contoso.com;email2@contoso.com*

8. Geçerli bir URI koymak **Web kancası** adlı uyarı oluşturulduğunda istiyorsanız alan.

9. Azure Otomasyonu kullanırsanız, uyarı oluşturulduğunda çalıştırılacak bir Runbook seçebilirsiniz.

10. Seçin **Tamam** uyarı oluşturmak için yapıldığında.   

Birkaç dakika içinde uyarı etkindir ve daha önce açıklandığı şekilde tetikler.

## <a name="managing-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra bunu seçebilirsiniz ve:

* Ölçüm eşiği ve önceki gün gerçek değerleri gösteren bir grafik görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirmek** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md) toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Yeni hakkında daha fazla bilgi [yakın gerçek zamanlı ölçüm uyarıları (Önizleme)](monitoring-near-real-time-metric-alerts.md)
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlükleri'ne genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak ve.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
