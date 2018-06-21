---
title: Azure Hizmetleri için Klasik uyarıları oluşturmak için Azure portalını kullanma | Microsoft Docs
description: E-postalar veya bildirimleri tetiklemek veya belirttiğiniz koşullar karşılandığında Web sitesine URL'lerini (Web kancaları) ya da Otomasyon çağırın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/23/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: d0e5512eb3f963898ded6d7155f8b75cfb6ef911
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287438"
---
# <a name="use-the-azure-portal-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Klasik ölçüm uyarılar için Azure Hizmetleri Azure İzleyicisi'nde oluşturmak için Azure portalını kullanın 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>

> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md). 


Bu makalede Azure portalını kullanarak Klasik Azure ölçüm uyarıları ayarlamak gösterilmiştir. 

Azure hizmetlerinizi ölçümlerini göre bir uyarı alabilirsiniz veya Azure'da meydana gelen olayları için uyarı alabilirsiniz.

* **Ölçüm değerleri**: herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler zaman ilk koşul ve ardından zaman bu koşulu artık karşılanmıyor.    

* **Etkinlik günlüğü olaylarını**: bir uyarıyı tetiklemek *her* olay veya belirli olaylar olduğunda. Daha fazla bilgi edinmek [etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md).

Tetikler, aşağıdakileri yapmak için Klasik bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönderin.
* E-posta, belirttiğiniz ek e-posta adreslerine gönder.
* Bir Web kancası çağırın.
* (Yalnızca Azure portalından) Azure bir runbook'un yürütülmesi başlatın.

Yapılandırın ve aşağıdaki konumlardan Klasik ölçüm uyarı kuralları hakkında bilgi alın: 

* [Azure portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure CLI](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Azure portal ile bir ölçüm bir uyarı kuralı oluşturma
1. İçinde [portal](https://portal.azure.com/)izlemek istediğiniz kaynağı bulun ve seçin.

2. İçinde **izleme** bölümünde, select **uyarıları (Klasik)**. Metin ve simge farklı kaynaklar için biraz değişebilir. Aradığınız bulamazsanız **uyarıları (Klasik)** içinde burada bulabilirsiniz **uyarıları** veya **uyarı kuralları**.

    ![İzleme](./media/insights-alerts-portal/AlertRulesButton.png)

3. Seçin **ölçüm uyarı Ekle (Klasik)** komut ve alanları doldurun.

    ![Uyarı Ekle](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Ad** uyarı kuralı. Ardından bir **açıklama**, bildirim e-postalarda de görünür.

5. Seçin **ölçüm** izlemek istediğiniz. Ardından bir **koşulu** ve **eşik** ölçüm için değer. Ayrıca tercih **süresi** ölçüm kuralı uyarı Tetikleyicileri önce karşılanması gereken süre. Örneğin, CPU % 80 5 dakika boyunca sürekli olarak yukarıdaki kaldığında "son 5 dakika boyunca" dönem kullanırsanız ve bir CPU % 80 yukarıda Uyarınız arar, uyarıyı tetikleyen. Tetik tarafından oluşur sonra ilk 5 dakika boyunca % 80 CPU kaldığında oluşan yeniden tetikler. CPU ölçüm ölçüm dakikada olur.

6. Seçin **e-posta sahipleri...**  yöneticileri ve ortak Yöneticiler uyarı oluşturulduğunda e-posta bildirimleri almaya istiyorsanız.

7. Uyarı oluşturulduğunda ek e-posta adreslerine bildirim gönder istiyorsanız, bunları eklemeniz **ek yönetici email(s)** alan. Birden çok e-postaları aşağıdaki biçimde bir noktalı virgül ile ayırın:  *email@contoso.com; email2@contoso.com*

8. Geçerli bir URI koymak **Web kancası** uyarı oluşturulduğunda çağrılacak istiyorsanız alan.

9. Azure Otomasyonu kullanırsanız, uyarı oluşturulduğunda çalıştırılacak bir runbook seçebilirsiniz.

10. Seçin **Tamam** uyarı oluşturmak için.   

Birkaç dakika içinde uyarı etkindir ve daha önce açıklandığı şekilde tetikler.

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme
Bir uyarı oluşturduktan sonra onu seçin ve aşağıdaki görevlerden birini yapın:

* Ölçüm eşiği ve önceki gün gerçek değerleri gösteren bir grafik görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirmek** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md), toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Daha fazla bilgi edinmek [yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md).
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Otomasyon çalışma kitabı](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlükleri'ne genel bakış](monitoring-overview-of-diagnostic-logs.md), hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak ve.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
