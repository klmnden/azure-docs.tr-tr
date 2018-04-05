---
title: Azure Hizmetleri için-uyarı oluşturma Azure portalı | Microsoft Docs
description: Belirttiğiniz koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın.
author: rboucher
manager: carmonm
editor: ''
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
ms.openlocfilehash: b0d938112aaea4d86dd539b53a1749cc800607a7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-classic-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Klasik ölçüm uyarılar için Azure services - Azure İzleyicisi'nde oluşturma Azure portalı
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Genel Bakış

> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md). 
>
>

Bu makalede Azure portalını kullanarak Klasik Azure ölçüm uyarıları ayarlamak gösterilmiştir. 

İzleme ölçümlerini ya da olayları, Azure hizmetlerinizi göre bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler koşul ilk ve ardından daha sonra ne zaman, koşul artık karşılanıp zaman.    
* **Etkinlik günlüğü olaylarını** -bir uyarıyı tetiklemek *her* olay veya yalnızca belirli bir olayı oluşur. Daha fazla bilgi edinmek [etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md).

Tetikler, aşağıdakileri yapmak için Klasik bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* bir Web kancası çağırın
* (yalnızca Azure portalından) Azure bir runbook'un yürütülmesi Başlat

Yapılandırma ve klasik ölçüm uyarı kuralları kullanma hakkında bilgi edinin

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal ile bir ölçüm bir uyarı kuralı oluşturma
1. İçinde [portal](https://portal.azure.com/), izleme ilgilenen kaynak bulup seçin.

2. Seçin **uyarıları (Klasik)** izleme bölümünde. Metin ve simge farklı kaynaklar için biraz değişebilir. Bulamadı, **uyarıları (Klasik)**, bunları altında bulabilirsiniz **uyarıları** veya **uyarı kuralları**

    ![İzleme](./media/insights-alerts-portal/AlertRulesButton.png)

3. Seçin **ölçüm uyarı Ekle (Klasik)** komut ve alanları doldurun.

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
* Daha fazla bilgi edinmek [yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlükleri'ne genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak ve.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
