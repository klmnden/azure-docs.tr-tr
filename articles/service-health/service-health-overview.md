---
title: Hizmet durumu genel bakış | Microsoft Docs
description: Azure uygulamalarınızı mevcut ve gelecekteki bir Azure hizmet sorunlarına ve Bakım nasıl etkilenir hakkında kişiselleştirilmiş bilgiler sağlar.
services: service-health
author: stephbaron
ms.author: stbaron
documentationcenter: service-health
ms.service: service-health
ms.topic: article
ms.workload: Supportability
ms.date: 03/27/2018
ms.openlocfilehash: d5385777134b911aa08b751978c44597f79acbc7
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442407"
---
# <a name="service-health"></a>Hizmet Durumu
Hizmet durumu, bunları kullandığınız bölgelerde Azure hizmetlerinizin durumunu izleyen özelleştirilebilir bir Pano sağlar. Bu Panoda, sürmekte olan bir hizmet sorunları, yaklaşan planlı Bakım veya ilgili bir sistem durumu danışmanları gibi etkin olayları izleyebilirsiniz. Olayları etkin olmayan duruma geldiğinde, 90 güne kadar sistem durumu geçmişinizde verdiklerine. Son olarak, hizmet durumu panosu oluşturmak ve hizmet sorunları, etkileşimimiz bağlandığınızda proaktif olarak size bildirim hizmeti sistem durumu Uyarıları yönetmek için kullanabilirsiniz.

## <a name="service-health-events"></a>Hizmet durumu olayları
Hizmet durumu, kaynaklarınızı etkileyebilecek sistem durumu olaylarını üç tür izler:
1. **Hizmet sorunları** -şu anda etkileyen Azure hizmetlerindeki sorunların. 
2. **Planlı Bakım** -hizmetlerinizin kullanılabilirliğini gelecekte etkileyebilir yaklaşan Bakımı.  
3. **Sistem durumu danışmanları** -Azure hizmetlerindeki dikkat etmeniz gereken değişiklikleri. Azure özellikleri kullanım dışı olduğunda veya bir kullanım kotası aşarsanız örneklerindendir.

> [!NOTE]
> Hizmet durumu olayları görüntülemek için kullanıcıların bir abonelikte okuyucu rolü olmalıdır.

## <a name="get-started-with-service-health"></a>Hizmet durumu ile çalışmaya başlama
Hizmet durumu panonuzu başlatmak için hizmet durumu kutucuğunu portal panonuza seçin. "Diğer hizmetler" hizmeti sistem sağlığı Hizmeti'nde kutucuğu daha önce kaldırdığınız veya özel bir Pano kullanıyorsanız, arama (alt, sol Panonuzda).

![Hizmet durumu ile çalışmaya başlama](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Hizmetlerinizi etkileyen geçerli sorunlara bakın
**Hizmet sorunları** görünümü, kaynaklarınızı etkileyen Azure hizmetlerini devam eden herhangi bir sorunu gösterir. Sorunun nerede başladığını ve hangi hizmet ve bölgelerin etkilendiğini anlayabilir. Ayrıca, sorunu çözmek için ne Azure yaptığını anlamak için en son güncelleştirmeyi okuyabilirsiniz. 

![Hizmet sorunu yönetme](./media/service-health-overview/azure-service-health-overview-2.png)

Seçin **olası etkisini** sorundan etkilenmiş olabilecek sahip olduğunuz kaynakların belirli listesini görmek için sekmesinde. Takımınızla paylaşmak için bu kaynakları CSV listesini indirebilirsiniz.

![Hizmet sorunu - etkisi yönetme](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Bağlantılar ve indirilebilir açıklamaları Al 
Sorun yönetim sisteminizde kullanmak için sorun için bir bağlantı elde edebilirsiniz. Azure portalına erişimi olmayan kişilerle paylaşmak için PDF ve bazen CSV dosyası indirebilirsiniz.   

![Hizmet sorunu - sorun yönetimi yönetme](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Microsoft'tan destek alın
Sorun çözümlendikten sonra kaynak hatalı bir durumda bırakılır desteğe başvurun.  Sayfanın sağ tarafındaki destek bağlantıları kullanın.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Kişiselleştirilmiş sağlık eşlemde panonuza sabitleyin
Hizmet durumu'nın, iş açısından önemli abonelik, bölge ve kaynak türlerini göstermek için filtreleyin. Filtreyi kaydedin ve kişiselleştirilmiş sistem durumu dünya Haritası portal panonuza sabitleyin. 

![Kişiselleştirilmiş sağlık harita Filtrele](./media/service-health-overview/azure-service-health-overview-6a.png)

![PIN kişiselleştirilmiş sağlık eşleme](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Hizmet sistem durumu uyarılarını yapılandırma
Hizmet durumu Azure İzleyici ile iş açısından kritik kaynaklarınız etkilendiğinde e-posta, kısa mesaj ve Web kancası bildirimleri uyarır tümleştirir. Uygun hizmet sistem durumu olayı için bir etkinlik günlüğü uyarısı ayarlayın. Bu uyarı, Eylem grupları kullanarak kuruluşunuzdaki uygun kişilerle rota. Daha fazla bilgi için [hizmet durumu için uyarıları yapılandırma](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="next-steps"></a>Sonraki adımlar
Sistem durumu sorunları bildirim almak için uyarılar ayarlayın. Daha fazla bilgi için [Azure hizmet durumu uyarıları ayarlama için en iyi yöntemler](https://www.youtube.com/watch?v=k5d5ca8K6tc&list=PLLasX02E8BPBBSqygdRvlTnHfp1POwE8K&index=6&t=0s). 
