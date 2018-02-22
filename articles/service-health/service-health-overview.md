---
title: "Azure hizmet durumu genel bakış | Microsoft Docs"
description: "Geçerli ve gelecekteki Azure hizmeti sorunları ve Bakım Azure uygulamalarınızı nasıl etkilenir kişiselleştirilmiş bilgi."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 5f2b68e800f484a64e79f965fc855aebe090186a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-health"></a>Azure Hizmet Durumu
Azure Hizmetleri sorunlarını hizmetlerinizi etkisi yüklediğinizde azure hizmet durumu zamanında ve kişiselleştirilmiş bilgileri sağlar.  Ayrıca, gelecek planlı bakım için hazırlanmanıza yardımcı olur.

## <a name="service-health-events"></a>Hizmet sistem durumu olayları
Hizmet durumu üç tür kaynaklarınızı etkileyebilir sistem durumu olayları izler:
1. **Hizmet sorunları** -şu anda etkileyen Azure hizmetlerinde sorunları. 
2. **Planlı Bakım** -gelecekte hizmetlerinizin kullanılabilirliğini etkileyebilecek yaklaşan Bakımı.  
3. **Sistem durumu danışma** -dikkat etmeniz gereken Azure Hizmetleri değişiklikleri. Azure özellikleri kullanım dışı bırakılmıştır olduğunda veya bir kullanım kotası aşarsa örnek olarak verilebilir.

    ![Hizmet sistem durumu olayları](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Hizmet durumu ile çalışmaya başlama
Hizmet durumu panonuz başlatmak için portal panosunda hizmet durumu kutucuğu seçin. 

![Hizmet durumu ile çalışmaya başlama](./media/service-health-overview/azure-service-health-overview-1.png)

Döşeme daha önce kaldırdığınız veya özel Pano kullanıyorsanız, hizmet sistem durumu hizmeti için arama "**tüm hizmetleri**" (üst sol Panonuzda).

![Hizmet durumu ile çalışmaya başlama](./media/service-health-overview/azure-service-health-overview-1a.png)

## <a name="see-current-issues-which-impact-your-services"></a>Hizmetlerinizin etkiler geçerli sorunları bakın
**Hizmet sorunları** görünüm kaynaklarınızı etkileyen Azure Hizmetleri'nde devam eden sorunları gösterir. Ne zaman sorun başladı ve hangi Hizmetleri ve bölgeleri etkilenen anlayabilirsiniz. Ayrıca, Azure sorunu çözmek için yaptıklarını anlamak için en son güncelleştirmeyi okuyabilirsiniz. 
![Hizmet sorunu yönetme](./media/service-health-overview/azure-service-health-overview-2.png)

Seçin **olası etkisini** bu sorundan etkilenebilirsiniz sahip olduğunuz kaynakların belirli listesini görmek için sekme. CSV listesini takımınızla paylaşmak için şu kaynaklara indirebilirsiniz.
![Hizmet sorunu - etkisi yönetme](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Get bağlantılar ve indirilebilir açıklamalar 
Sorun yönetimi sisteminizi kullanabilmek sorun için bir bağlantı elde edebilirsiniz. Azure portalına erişiminiz yoksa kişilerle paylaşmak için PDF ve bazı durumlarda da CSV dosyası indirebilirsiniz.   
![Hizmet sorunu - sorun yönetimi yönetme](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Microsoft'tan destek alın
Sorun çözümlendikten sonra kaynak hatalı bir duruma bırakılırsa desteğe başvurun.  Sayfanın sağ tarafta destek bağlantıları kullanın.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Kişiselleştirilmiş sistem durumu harita panonuza sabitleyin
Hizmet durumu, iş açısından kritik abonelikler, bölgeler ve kaynak türleri göstermek için filtre uygulayın. Filtre kaydedin ve kişiselleştirilmiş sistem durumu world haritaya portalı panonuza sabitleyin. 
![Filtre kişiselleştirilmiş sistem durumu harita](./media/service-health-overview/azure-service-health-overview-6a.png)
![kişiselleştirilmiş sistem durumu harita sabitleyin](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Hizmet durumu uyarıları yapılandırma
Azure hizmet durumu izleme, iş açısından kritik kaynaklarınızı etkilenen olduğunda e-posta, metin iletileri ve Web kancası bildirimleri sizi uyarmak için Azure tümleştirir. Uygun hizmet durumu olay için bir etkinlik günlüğü uyarı ayarlayın. Bu uyarı Eylem grupları kullanarak kuruluşunuzdaki uygun kişilerin yönlendirmek. Daha fazla bilgi için bkz: [hizmet sistem durumu için uyarıları Yapılandır](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

 
