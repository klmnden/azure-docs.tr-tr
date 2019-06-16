---
title: Azure kaynak durumu hakkında SSS | Microsoft Docs
description: Azure kaynak durumu genel bakış
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 01/29/2019
ms.workload: Supportability
ms.openlocfilehash: 03c5fb0f399e43af2584743655eeea8af4c5b2f5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64924438"
---
# <a name="azure-resource-health-faq"></a>Azure kaynak durumu hakkında SSS
Azure kaynak durumu hakkında genel soruların yanıtlarını öğrenin.

## <a name="what-is-azure-resource-health"></a>Azure kaynak durumu nedir?
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.  

## <a name="what-is-the-resource-health-intended-for"></a>Hangi kaynak durumu yöneliktir?
Bir kaynak ile ilgili bir sorun algıladığında kaynak durumu, kökenini tanılamaya yardımcı olabilir. Bu, daha fazla Azure hizmetleriyle ilgili sorunlarda yardıma ihtiyacınız olduğunda teknik destek ve sorun gidermek için Yardım sağlar.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Hangi sistem durumu denetimleri gerçekleştirilir tarafından kaynak durumu?
Kaynak durumu göre çeşitli denetimleri gerçekleştirir [kaynak türü](resource-health-checks-resource-types.md). Bu denetimler, sorunları üç tür uygulamak için tasarlanmıştır: 
- Planlanmamış olaylar, örneğin bir beklenmeyen ana bilgisayar yeniden başlatma
- Planlanan olayları ister zamanlanmış konak işletim sistemi güncelleştirmeleri
- Kullanıcı eylemleri tarafından örneğin bir sanal makine yeniden başlatma kullanıcı tetiklenen olayları

## <a name="what-does-each-of-the-health-status-mean"></a>Her sistem durumu ne demektir?
Üç farklı sistem durumlarını vardır:
- Kullanılabilir: Bu kaynak etkileyen Azure platformunda herhangi bir bilinen sorun yok
- Kullanılabilir: Kaynak durumu, kaynak etkileyen sorunları algıladı.
- Bilinmiyor: Kaynak durumu, hakkında bilgi alma durdurduğundan kaynak durumu belirlenemiyor. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Bilinmeyen Durum ne anlama geliyor? Kaynağım ile yanlış bir şey mi?
Kaynak durumu, belirli bir kaynak hakkında bilgi alma durduğunda sistem durumunu bilinmeyen olarak ayarlanır. Bu durum, kesin bir yere sorunlarla karşılaştığı durumlarda kaynak durumu göstergesi olmamasına karşın Azure bir sorun olduğunu gösteriyor olabilir.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Kullanılabilir olmayan bir kaynak için nasıl Yardım alabilirim?
Kaynak durumu dikey penceresinden bir destek isteği gönderebilirsiniz. Kaynak kullanılamadığında bir isteği açmak için Microsoft ile bir destek sözleşmesi gerekmez çünkü platform olayları.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Kaynak durumu platform sorunları sorun yaptım ve büyük/küçük harfleri kullanılamazlık birbirinden mu?
Evet, kaynak kullanılamadığında, kök nedeni bu kategorilerden birini içinde kaynak durumu tanımlar: 
-   Kullanıcı tarafından başlatılan eylemi
-   Olay 
-   Planlanmamış bir olay

Portalda, planlı ve plansız olayları kırmızı bir uyarı simgesi kullanarak gösterilmekte iken bir mavi bildirim simgesini kullanarak kullanıcı tarafından başlatılan Eylemler gösterilir. Daha fazla ayrıntı sağlanır [kaynak durumu genel bakış](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kaynak durumu my izleme araçları ile tümleştirebilir miyim?
Kaynak durumu sahip [Önizleme desteği](resource-health-alert-arm-template-guide.md) etkinlik günlüğü uyarıları tabanlı için. Etkinlik günlüğü uyarıları, kullanım [Eylem grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) uyarı tetiklendi kullanıcılara bildirmek için. Eylem grupları, bildirim kanalları e-posta, SMS, Web kancası ve ITSM eylemleri gibi çeşitli destekler.

## <a name="where-do-i-find-resource-health"></a>Kaynak durumu nerede bulabilirim?
Azure portalında oturum açtıktan sonra kaynak durumu erişebileceğiniz birden çok yolu vardır:
- Kaynağınıza gidin. Sol gezinti bölmesinde **kaynak durumu**
- Azure hizmet durumu dikey penceresine gidin.  Sol gezinti bölmesinde **kaynak durumu**.
- Açık **Yardım + Destek** portalın sağ üst köşesinde soru işaretini seçerek ve ardından dikey **Yardım + Destek**. Dikey penceresi açıldıktan sonra seçin **kaynak durumu**

Kaynaklarınızın sistem durumu hakkında bilgi edinmek için kaynak durumu API'si de kullanabilirsiniz.

## <a name="is-resource-health-available-for-all-resource-types"></a>Kaynak durumu, tüm kaynak türleri için kullanılabilir mi?
Sistem durumu denetimleri ve kaynak durumu desteklenen kaynak türleri listesi bulunabilir [burada](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Ne kaynağım kullanılabilir gösteriyor, ancak ben değil düşünüyorsanız yapmam?"
Kaynak durumunu denetleme, sistem durumunu altında sağ tıklayabilirsiniz **yanlış sistem durumu raporu**. Raporu göndermeden önce neden geçerli durumu yanlış olduğunu düşündüğünüz hakkında ek ayrıntılar sağlayan seçeneğiniz vardır.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Kaynak durumu için tüm Azure bölgelerinde kullanılabilir mi? 
Kaynak durumu, tüm Azure coğrafi bölgesinde kullanılabilir.

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>Kaynak durumu Azure durumu ya da hizmet durumu Panosu farklı mı?
Kaynak durumu tarafından sağlanan bilgileri ne Azure durumu veya hizmet durumu Panosu tarafından sağlanan daha özel hale geldi.

Oysa [Azure durumu](https://status.azure.com) ve hizmet durumu Panosu (örneğin bir Azure bölgesi) müşteriler çok geniş bir yelpazedeki etkileyen hizmeti sorunları hakkında bilgilendirmek, kaynak durumu, yalnızca ilgili daha ayrıntılı olayları gösterir belirli kaynak. Örneğin, bir ana bilgisayar beklenmedik şekilde yeniden başlatılırsa, kaynak durumu, yalnızca o ana bilgisayarda çalışan sanal makineleri müşteriler uyarır.

Kaynaklarınızı etkileyen olayların tam görünürlük sağlamak için dikkat edin önemlidir, kaynak durumu, hizmet durumu Panosu yayımlanan olaylar ayrıca ortaya çıkarır.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Kaynak durumu, her kaynak için etkinleştirme gerekiyor mu?
Hayır, sistem durumu bilgisi, kaynak durumu kullanılabilir olan tüm kaynak türleri kullanılabilir. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Kaynak durumu Kuruluşum için etkinleştirme gerekiyor mu?
Hayır.  Azure kaynak durumu, Azure portalında herhangi bir kurulum gereksinimi olmadan erişilebilir.

## <a name="is-resource-health-available-free-of-charge"></a>Kaynak durumu kullanılabilir ücretsiz mi?
Evet.  Azure kaynak durumu ücretsizdir.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Kaynak durumu sağlayan önerileri nelerdir?
Sistem durumuna bağlı olarak, kaynak durumu, sorun giderme için harcanan süreyi azaltmak amacıyla ile öneriler sağlar. Kullanılabilir kaynaklar için önerileri odaklanan nasıl çözeceğinizi en yaygın sorunları müşteriler karşılaşırsınız. Kaynak Azure planlanmamış bir olay nedeniyle kullanılamıyor, odağı, sırasında ve sonrasında kurtarma işlemi Yardım üzerinde olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi edinin:
-  [Azure kaynak durumu genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)
