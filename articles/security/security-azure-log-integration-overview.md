---
title: "Azure kaynaklarını günlüklerinden SIEM sistemlerinizi tümleştirme | Microsoft Docs"
description: "Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Microsoft Azure günlük tümleştirme giriş

Azure günlük tümleştirme sağlar, Azure kaynaklarınızı ham günlüklerinden şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri ile tümleştirmek durumunda bir bağlayıcı [Azure İzleyici](../monitoring-and-diagnostics/monitoring-get-started.md) henüz kullanılabilir değil SIEM satıcınıza.

SIEM satıcınızın Azure İzleyici Bağlayıcısı'nı kullanarak ve bunlar aşağıdaki Azure günlükleri tümleştirmek için tercih edilen yöntem olduğu [yönergeleri](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). SIEM satıcınıza Azure İzleyici için bir bağlayıcı sağlamıyorsa (SIEM'iniz Azure günlük tümleştirme tarafından destekleniyorsa) bu tür bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük tümleştirme geçici bir çözüm olarak kullanmak mümkün olabilir.

>[!IMPORTANT]
>Birincil ilgi sanal makine günlükleri toplamayı ise, çoğu SIEM satıcısını bu kendi çözümde içerir. SIEM kullanarak satıcının bağlayıcı her zaman tercih edilen alternatif olması gerekir.

Azure günlük tümleştirme toplar Windows olaylarını Windows Olay Görüntüleyicisi'ni günlüklerinden [Azure etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Güvenlik Merkezi uyarılarını](../security-center/security-center-intro.md), ve [Azure tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) gelen Azure kaynakları. Bu tümleştirme tüm varlıklar için birleştirilmiş bir Pano sağlar, SIEM çözümünüzün yardımcı olur, şirket içi veya bulutta araya getirebilirsiniz böylece ilişkilendirmek, çözümlemek ve güvenlik olayları için uyarı.

>[!NOTE]
Şu anda yalnızca desteklenen Bulutlar Azure ticari ve Azure kamu var. Diğer bulut desteklenmez.

![Azure günlük tümleştirmesi][1]

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri ı tümleştirebilir miyim?

Azure Azure her hizmet için ayrıntılı günlük kaydını üretir. Bu günlükler günlükleri üç tür temsil eder:

* **Denetim/Yönetim günlüklerini** görünürlük sağlayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma, güncelleştirme ve silme işlemleri. Azure etkinlik günlükleri, günlük bu tür bir örnektir.
* **Veri günlüklerini düzlemine** bir Azure kaynak kullanımını bir parçası olarak oluşturulan olaylara görünürlük sağlar. Bu günlük türü, Windows Olay Görüntüleyicisi'nin örneğidir **sistem**, **güvenlik**, ve **uygulama** Windows sanal makine kanallarında. Azure İzleyicisi aracılığıyla yapılandırılmış Tanılama Günlüğü başka bir örnek verilmiştir
* **İşlenen olayların** çözümlenen olay ve sizin adınıza işlenen uyarı bilgileri sağlar. Azure Güvenlik Merkezi burada ve işlenen geçerli güvenlikle ilgili tutumunuzu için ilgili uyarıları sağlamak için aboneliğinizi analiz Azure Güvenlik Merkezi uyarılarını, bu olay türü örnektir.

Azure günlük tümleştirme ArcSight, QRadar ve Splunk destekler. Tüm durumlarda, yerel bir bağlayıcı yüklü olup olmadığını değerlendirmek için SIEM satıcınızla birlikte denetleyin. Yerel bağlayıcılar kullanılabilir olduğunda Azure günlük tümleştirme kullanmamanız gerekir.

Başka hiçbir seçenek yoksa Azure günlük tümleştirme kabul. Aşağıdaki tabloda bizim öneriler içerir.

|**SIEM** | **Müşteri zaten günlük Tümleştirici kullanma** | **Müşteri SIEM tümleştirme seçeneklerini araştırma**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Geçiş başlamadan [Splunk için Azure İzleyici eklentisi](https://splunkbase.splunk.com/app/3534/) | Kullanım [SPLUNK bağlayıcı](https://splunkbase.splunk.com/app/3534/) |
|**IBM QRADAR** | Geçiş veya http://aka.ms/azmoneventhub sonunda belgelenen QRadar connector'ı kullanmaya başlamak | Http://aka.ms/azmoneventhub sonunda belgelenen QRadar Bağlayıcısı'nı kullanın  |
|**ARCSIGHT** | Bir bağlayıcı kullanılabilir hale gelene kadar günlük Tümleştirici kullanmaya devam ardından bağlayıcı tabanlı bir çözüme geçirme.  | Azure günlük analizi alternatif olarak göz önünde bulundurun. Yapmak değil yerleşik Azure günlük tümleştirme için bağlayıcı kullanılabilir hale geldiğinde geçiş sürecinde gitmek hazır olduğunuz sürece. |

>[!NOTE]
>Azure günlük tümleştirme boş bir çözüm olsa da, günlük dosyası bilgilerini depolama alanından kaynaklanan Azure storage maliyetleri vardır.

Yardıma ihtiyacınız varsa, açabilirsiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Bunu yapmak için seçin **günlük tümleştirme** destek isteyen hizmet olarak.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure günlük tümleştirme tanıtıldı. Azure günlük tümleştirme ve desteklenen günlükleri türleri hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure günlük tümleştirme ile çalışmaya başlama](security-azure-log-integration-get-started.md) - Bu öğretici, Azure günlük tümleştirme yüklenmesinde size yol gösterir ve Azure WAD depolama, Azure etkinlik günlükleri günlüklerinden tümleştirme Azure Güvenlik Merkezi uyarılarını ve Azure Active Directory denetim günlükleri.
* [Azure günlük sık sorulan sorular (SSS) tümleştirme](security-azure-log-integration-faq.md) -bu SSS Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Bir dış aracı tarafından izleme verileri tüketim için bir olay hub'ına akış Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
