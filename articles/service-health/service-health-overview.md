---
title: Hizmet durumu genel bakış | Microsoft Docs
description: Geçerli ve gelecekteki Azure hizmeti sorunları ve Bakım Azure uygulamalarınızı nasıl etkilenir kişiselleştirilmiş bilgi.
services: Resource health
documentationcenter: ''
author: rboucher
manager: ''
editor: ''
ms.assetid: ''
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/27/2018
ms.author: robb
ms.openlocfilehash: f7fd27dc9be161059dc62071fee33374106e18ef
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061290"
---
# <a name="service-health"></a>Hizmet Durumu
Hizmet durumu kullandığınız burada bölgelerdeki Azure hizmetlerinizi durumunu izleyen özelleştirilebilir bir Pano sağlar. Bu Panoda, devam eden hizmet sorunları, gelecek planlı Bakım veya ilgili sistem durumu danışma gibi etkin olayları izleyebilirsiniz. Olayları etkin olmayan duruma geldiğinde sistem durumu geçmişinizde 90 gün boyunca yerleştirilen. Son olarak, hizmet sorunlarını etkileyen bağlandığınızda proaktif olarak sizi uyarır hizmet durumu uyarıları oluşturma ve yönetme için hizmet sistem durumu Panosu'nu kullanabilirsiniz.

## <a name="service-health-events"></a>Hizmet sistem durumu olayları
Hizmet durumu üç tür kaynaklarınızı etkileyebilir sistem durumu olayları izler:
1. **Hizmet sorunları** -şu anda etkileyen Azure hizmetlerinde sorunları. 
2. **Planlı Bakım** -gelecekte hizmetlerinizin kullanılabilirliğini etkileyebilecek yaklaşan Bakımı.  
3. **Sistem durumu danışma** -dikkat etmeniz gereken Azure Hizmetleri değişiklikleri. Azure özellikleri kullanım dışı bırakılmıştır olduğunda veya bir kullanım kotası aşarsa örnek olarak verilebilir.

## <a name="get-started-with-service-health"></a>Hizmet durumu ile çalışmaya başlama
Hizmet durumu panonuz başlatmak için portal panosunda hizmet durumu kutucuğu seçin. Döşeme daha önce kaldırdığınız veya özel Pano kullanıyorsanız, "Daha fazla Hizmetleri" hizmet sistem durumu hizmeti için arama (alt, sol Panonuzda).

![Hizmet durumu ile çalışmaya başlama](./media/service-health-overview/azure-service-health-overview-1.png)

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

![Filtre kişiselleştirilmiş sistem durumu eşleme](./media/service-health-overview/azure-service-health-overview-6a.png)

![PIN kişiselleştirilmiş sistem durumu eşleme](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Hizmet durumu uyarıları yapılandırma
Hizmet durumu izleme, iş açısından kritik kaynaklarınızı etkilenen olduğunda e-posta, metin iletileri ve Web kancası bildirimleri sizi uyarmak için Azure tümleştirir. Uygun hizmet sistem durumu olayı için bir etkinlik günlüğü uyarı ayarlayın. Bu uyarı Eylem grupları kullanarak kuruluşunuzdaki uygun kişilerin yönlendirmek. Daha fazla bilgi için bkz: [hizmet sistem durumu için uyarıları Yapılandır](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

# <a name="next-steps"></a>Sonraki Adımlar
Sistem durumu sorunları, bildirim almak için uyarıları ayarlayın. Daha fazla bilgi için bkz: [hizmet durumu uyarıları yapılandırmak](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
