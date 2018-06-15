---
title: Azure kaynak durumu ile ilgili SSS | Microsoft Docs
description: Azure kaynak durumu genel bakış
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/27/2018
ms.author: BernardoAMunoz
ms.openlocfilehash: 2a0758f5db381f721120cbc1c018a491c018aa8d
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
ms.locfileid: "30264243"
---
# <a name="azure-resource-health-faq"></a>Azure kaynak durumu ile ilgili SSS
Azure kaynak durumu hakkında genel soruların yanıtlarını öğrenin.

## <a name="what-is-azure-resource-health"></a>Azure kaynak durumu nedir?
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.  

## <a name="what-is-the-resource-health-intended-for"></a>Hangi kaynak durumu yöneliktir?
Bir kaynağı ile ilgili bir sorun algıladı sonra kaynak durumu, temel nedeni tanılamanıza yardımcı olabilir. Azure hizmet sorunları hakkında daha fazla yardıma ihtiyacınız olduğunda sorunu ve teknik destek azaltmaya yardımcı sağlar.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Hangi sistem durumu denetler gerçekleştirilir kaynak durumu tarafından?
Kaynak durumu göre çeşitli denetimleri gerçekleştirir [kaynak türü](resource-health-checks-resource-types.md). Bu denetimler, sorunları üç tür uygulamak için tasarlanmıştır: 
- Planlanmamış olayları, örneğin bir beklenmeyen ana bilgisayar yeniden başlatma
- Planlanan olayları ister zamanlanmış konak işletim sistemi güncelleştirmeleri
- Bir sanal makine yeniden başlatılıyor kullanıcı örneğin kullanıcı eylemleri tarafından tetiklenen olayları

## <a name="what-does-each-of-the-health-status-mean"></a>Her sistem durumunu anlamı nedir?
Üç farklı sistem durum vardır:
- Kullanılabilir: Olmadığı herhangi bir bilinen sorun bu kaynak etkileyen Azure platformu
- Kullanılabilir: Kaynak durumu kaynak etkileyen sorunları algıladı.
- Bilinmeyen: hakkında bilgi alma durduğundan kaynak durumu kaynağının durumu belirlenemiyor. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Bilinmeyen Durum ne anlama geliyor? My kaynakla yanlış bir şey mi?
Kaynak durumu belirli bir kaynak hakkında bilgi alma durduğunda sistem durumunu bilinmeyen olarak ayarlanır. Bu durum burada sorunları yaşıyor durumlarda kaynak durumunun kesin bir gösterge olmamasına karşın Azure bir sorun olduğunu gösteriyor olabilir.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Kullanılabilir olmayan bir kaynak için nasıl Yardım alabilirim?
Kaynak durumu dikey penceresinden bir destek isteği gönderebilirsiniz. Kaynak kullanılamadığında bir istek açmak için Microsoft ile bir destek sözleşmesi gerekli değildir çünkü platform olaylar.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Kaynak durumu platform sorunları şey yaptım karşı ortası kullanılamazlık birbirinden mu?
Evet, bir kaynak kullanılabilir olmadığında kaynak durumu kök nedeni bu kategorilerden birini içinde tanımlar: 
-   Kullanıcı tarafından başlatılan eylemi
-   Olay 
-   Planlanmamış olay

Portalda, kullanıcı tarafından başlatılan Eylemler kırmızı bir uyarı simgesi kullanarak planlanmış ve planlanmamış olayları gösterilen sırada bir mavi bildirim simgesini kullanarak gösterilmektedir. Daha fazla ayrıntı sağlanan [kaynak durumu genel bakış](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kaynak durumu my izleme araçları ile tümleştirebilir miyim?
Kaynak durumu tanılamak ve kaynaklarınızı etkisi Azure hizmet sorunlarını gidermek yardımcı olmak üzere tasarlanmış bir hizmettir. Program aracılığıyla sistem durumunu almak için kaynak durumu API'si kullanabilmenize karşın, kaynaklarınızın izlemek için ölçümleri kullanmanızı öneririz. Bir sorun algılandığında, kaynak durumu kök nedenini belirlemenize yardımcı olur ve bunları ele almak için Eylemler size yol gösterir. Ziyaret [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kaynaklarınızı denetlemek için ölçümleri nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için.

## <a name="where-do-i-find-resource-health"></a>Kaynak durumu nereden bulabilirim?
Azure portalında oturum açtıktan sonra kaynak durumu erişebilmeniz için birden çok yolu vardır:
- Kaynağınıza gidin. Sol gezinti bölmesinde seçin **kaynak durumu**
- Azure İzleyici dikey penceresine gidin.  Sol gezinti bölmesinde seçin **kaynak durumu**.
- Açık **Yardım + Destek** portalının sağ üst köşedeki soru işareti seçerek ve ardından seçerek dikey **Yardım + Destek**. Dikey pencere açılır sonra seçeneğini **kaynak durumu**

Kaynaklarınızın sistem durumu hakkında bilgi edinmek için kaynak sistem durumu API de kullanabilirsiniz.

## <a name="is-resource-health-available-for-all-resource-types"></a>Kaynak durumu, tüm kaynak türleri için kullanılabilir mi?
Sistem durumu denetimlerinin ve kaynak durumu desteklenen kaynak türleri listesi bulunabilir [burada](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Ne my kaynak kullanılabilir gösteren ancak ı değil düşünüyorsanız yapmalıyım?"
Bir kaynak sistem durumu denetlenirken tıklayabilirsiniz sağ sistem durumunu altında **yanlış sistem durumu raporu**. Raporu göndermeden önce neden geçerli sistem durumu yanlış olduğunu düşündüğünüz hakkında ek ayrıntıları sağlayan seçeneğiniz vardır.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Kaynak durumu için tüm Azure bölgeleri var mı? 
Kaynak durumu aşağıdaki bölgeler dışında tüm Azure geos üzerinden kullanılabilir:
- ABD Devleti Virginia
- ABD Devleti Iowa
- US DoD Doğu
- US DoD Orta
- Almanya Orta
- Almanya Kuzeydoğu

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>Kaynak durumu Azure durum ya da hizmet sistem durumu Panosu farklı mı?
Kaynak durumu tarafından sağlanan bilgileri ne Azure durumu veya hizmet sistem durumu Panosu tarafından sağlanan daha fazla özeldir.

Oysa [Azure durum](https://status.azure.com) ve hizmet sistem durumu Panosu müşteriler (örneğin bir Azure bölgesi) geniş bir dizi etkileyen hizmeti sorunları hakkında bilgilendirmek, kaynak durumu yalnızca ilgili daha ayrıntılı olayları gösterir belirli kaynak. Örneğin, bir ana bilgisayar beklenmedik şekilde yeniden başlatılırsa, kaynak durumu, sanal makineler Bu konakta çalışmakta olan müşteriler uyarır.

Kaynaklarınızın etkileyen olayların tam görünürlük sağlamak için dikkat önemlidir, kaynak durumu, hizmet durumu panosunda yayımlanan olayları de ortaya çıkarır.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Her kaynak için kaynak durumu etkinleştirmeniz gerekiyor mu?
Hayır, sistem durumu bilgisi, kaynak durumu kullanılabilir tüm kaynak türleri kullanılabilir. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Kaynak durumu Kuruluşum için etkinleştirmek ihtiyacımız var?
Hayır.  Azure kaynak durumu, herhangi bir kurulum gereksinimi olmadan Azure portalındaki erişilebilir.

## <a name="is-resource-health-available-free-of-charge"></a>Kaynak durumu kullanılabilir ücretsiz?
Evet.  Azure kaynak durumu ücretsizdir.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Kaynak durumu sağlar önerileri nelerdir?
Sistem sağlığı durumlarına bağlı olarak, kaynak durumu sorun giderme için harcanan süreyi azaltarak amacı ile ile önerileri sağlar. Kullanılabilir kaynaklar için en yaygın sorunları müşteriler çözmek nasıl önerileri odaklanmak karşılaşırsınız. Kaynak Azure planlanmamış bir olay nedeniyle kullanılamıyorsa, odağı, sırasında ve sonrasında kurtarma işlemi yardımcı üzerinde olacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak durumu hakkında daha fazla bilgi edinin:
-  [Azure kaynak durumu genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)
