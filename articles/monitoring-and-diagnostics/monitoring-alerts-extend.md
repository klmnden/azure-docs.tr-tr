---
title: Azure uyarıları - genel bakış (kopya) günlük analizi uyarılar genişletme
description: Uyarıları günlük analizi OMS portalında Azure Uyarıları ' ile kopyalama işlemine genel bakış adresleme ortak müşteri sorunları ayrıntılarını verir.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6484142eafa8388117c1e96ab31eefeab188e488
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750281"
---
# <a name="extend-log-analytics-alerts-to-azure-alerts"></a>Azure Uyarıları'için günlük analizi uyarılar genişletme
Yakın zamanda kadar Azure günlük analizi, günlük analizi verilerine dayalı koşulları proaktif olarak bildirebilirsiniz kendi uyarı işlevselliği dahil. Microsoft Operations Management Suite portalına uyarı kurallarında yönetilen. Yeni uyarılar deneyimi artık Microsoft Azure çeşitli hizmetlerde uyarı tümleşiktir. Bu olarak kullanılabilir **uyarıları** Azure portalında Azure İzleyicisi altında etkinlik günlükleri, ölçümleri, uyarı destekler ve hem günlük analizi hem de Azure Application Insights günlüğe kaydeder. 

## <a name="benefits-of-extending-your-alerts"></a>Uyarılarınızı kullanmanın yararları
Oluşturma ve Azure Portal'da uyarılarını yönetme gibi çeşitli avantajları vardır:

- Aksine burada yalnızca 250 uyarıları görüntülenebilir ve oluşturulmalıdır, Operations Management Suite portalda Azure uyarıları böyle bir kısıtlama var.
- Azure Uyarıları ' yönetmek, numaralandırır ve tüm uyarı türlerini görüntülemek. Daha önce yalnızca günlük analizi uyarılar için bunu.
- Yalnızca izleme ve uyarı, kullanarak kullanıcıların erişimi sınırlandırabilirsiniz [Azure İzleyici rol](monitoring-roles-permissions-security.md).
- Azure Uyarıları'nda kullanabileceğiniz [Eylem grupları](monitoring-action-groups.md). Her uyarı için birden fazla eylem sahip olmanızı sağlar. SMS edebilir sesli arama göndermek, bir Azure Otomasyonu runbook'u Çağır, bir Web kancasını çağırma ve BT Hizmet Yönetimi (ITSM) bağlayıcısını yapılandırın. 

## <a name="process-of-extending-your-alerts"></a>Uyarılarınızı genişletme işlemi
Uyarıları günlük Analytics'ten Azure Uyarıları ' taşıma işlemi uyarı tanımı, sorgu veya herhangi bir şekilde yapılandırmasını değiştirmeyi gerektirmez. Gerekli yalnızca Azure içinde tüm eylemler bir eylem grubu kullanarak gerçekleştirmenizi değişikliktir. Eylem grupları zaten Uyarınız ile ilişkili ise, bunlar Azure genişletilmiş dahil edilir.

> [!NOTE]
> 14 Mayıs 2018 üzerinde tamamlanana kadar yinelenen serisinde başlangıç Microsoft otomatik olarak Azure uyarılar için günlük analizi oluşturulan uyarıların genişletir. Oluşturma sorunları varsa [Eylem grupları](monitoring-action-groups.md), kullanın [düzeltme adımları](monitoring-alerts-extend-tool.md#troubleshooting) otomatik olarak oluşturulan eylem grupları alınamadı. 5 Temmuz 2018 kadar bu adımları kullanabilirsiniz. 
> 

Azure için genişletilmesi için günlük analizi çalışma alanındaki uyarılar zamanladığınızda, iş ve buna herhangi bir şekilde yapılandırmanızı tehlikeye olmadığı devam eder. Zamanlanan uyarılarınızı geçici olarak değiştirilmesi için kullanılamıyor olabilir, ancak bu süre boyunca yeni Azure uyarıları oluşturmaya devam edebilirsiniz. Düzenleme veya uyarıları Operations Management Suite Portalı'ndan çalışırsanız, günlük analizi çalışma alanından oluşturmaya devam etmek için seçeneğiniz vardır. Bunları Azure portalında Azure uyarıları oluşturmak seçebilirsiniz.

 ![Günlük analizi veya Azure uyarıları uyarılar oluştur seçeneğinin ekran görüntüsü](./media/monitor-alerts-extend/ScheduledDirection.png)

> [!NOTE]
> Uyarılar için Azure günlük analizi genişletme hesabınıza ücretlendirme değil. Günlük analizi uyarılar sorgu tabanlı için Azure uyarıları kullanarak değil faturalandırılır sınırlarda kullanıldığında ve koşullar belirtildiği [Azure fiyatlandırma ilkesi İzleyici](https://azure.microsoft.com/pricing/details/monitor/).  


### <a name="how-to-extend-your-alerts-voluntarily"></a>Uyarılarınızı gönüllü genişletme
Azure uyarıları uyarılarınızı genişletmek için Operations Management Suite Portalı'nda kullanılabilir Sihirbazı kullanabilirsiniz veya bir API kullanarak bunu programlı yapabilirsiniz. Daha fazla bilgi için bkz: [uyarıları Operations Management Suite portal ve API kullanarak Azure genişletme](monitoring-alerts-extend-tool.md).

## <a name="experience-after-extending-your-alerts"></a>Uyarılarınızı genişlettikten sonra deneyimi
Uyarılarınızı Azure Uyarıları'için genişletilmiş sonra Hayır daha önce farklı yönetim Operations Management Suite portalında kullanılabilir olmaya devam.

![Operations Management Suite ekran portalıyla listelenen uyarıları](./media/monitor-alerts-extend/PostExtendList.png)

Var olan bir uyarı düzenleyin veya yeni bir uyarı Operations Management Suite Portalı'nda oluşturma girişiminde bulunduğunuzda Azure uyarıları otomatik olarak yeniden yönlendiriliyor.  

> [!NOTE]
> Uyarıları eklemek veya düzenlemek için gereken kişilere atanan izinlere Azure'da düzgün atanmış olmadığından emin olun. Vermek için gereken izinleri anlamak için bkz: [Azure İzleyicisi ve Uyarıları kullanarak izinlerini](monitoring-roles-permissions-security.md).  
> 

Uyarılar oluşturmak devam edebilirsiniz [günlük analizi API](../log-analytics/log-analytics-api-alerts.md) ve [günlük analizi kaynak şablonu](../monitoring/monitoring-solutions-resources-searches-alerts.md). Bunu yaptığınızda, eylem gruplarını eklemelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Araçlar hakkında bilgi edinin [başlatma uyarıları Azure günlük analizi genişletme](monitoring-alerts-extend-tool.md).
* Daha fazla bilgi edinmek [Azure uyarıları deneyimi](monitoring-overview-unified-alerts.md).
* Oluşturmayı öğrenin [uyarıları Azure Uyarıları'nda oturum](monitor-alerts-unified-log.md).
