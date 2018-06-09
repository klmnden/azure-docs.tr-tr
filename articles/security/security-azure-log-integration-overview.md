---
title: Azure kaynaklarını günlükleri, SIEM sistemleriyle tümleştirme | Microsoft Docs
description: Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
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
ms.date: 06/07/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 3c875060a7abdf4431026e79ce966efdc89e4e77
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236187"
---
# <a name="introduction-to-azure-log-integration"></a>Azure günlük tümleştirme giriş

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 01/06/2019 tarafından kullanım dışı kalacaktır. AzLog yüklemeleri 27 Haz 2018 tarafından devre dışı bırakılacak. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında yönergeler için [SIEM araçları ile tümleştirmek için kullanım Azure İzleyicisi](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirme Azure tümleştirme görevini kolaylaştırmak kullanılabilir yapıldı günlükleri sisteminizle şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM).

 Tümleştirme Azure günlükleri için önerilen yöntem, SIEM satıcınızın bağlayıcılar kullanmaktır. Azure izleme günlükleri Olay hub'lara akış olanağı sağlar ve daha fazla olay hub'ı günlüklerinden SIEM tümleştirmek için bağlayıcıları SIEM satıcısını yazabilirsiniz.  Bunun nasıl çalıştığı bir açıklaması için ' ndaki yönergeleri izleyin [olay hub'ları için veriler izleme monitör akış](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). Makale ayrıca doğrudan Azure bağlayıcılar zaten mevcut olduğu Sıem'lerden listeler.  

> [!IMPORTANT]
> Birincil ilgi sanal makine günlüklerinin toplanması, çoğu SIEM satıcısını kendi çözümde bu seçenek içerir. SIEM satıcının bağlayıcı her zaman tercih edilen alternatif kullanmaktır.

Bu özellik kullanım dışı kadar Azure günlük tümleştirme özelliği belgelerine hala sağlanıyor.

Daha fazla Azure günlük tümleştirme özelliği hakkında daha fazla bilgi edinmek için şunu okuyun:

Azure günlük tümleştirme toplar Windows olaylarını Windows Olay Görüntüleyicisi'ni günlüklerinden [Azure etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Güvenlik Merkezi uyarılarını](../security-center/security-center-intro.md), ve [Azure tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) gelen Azure kaynakları. Tümleştirme yardımcı olan SIEM çözümünüzü birleştirilmiş bir pano, tüm varlıklar için mi sağlamak şirket içi veya bulutta. Alma, toplama, bağıntılı ve güvenlik olayları için uyarıları çözümlemek için panoyu kullanabilirsiniz.

> [!NOTE]
> Şu anda yalnızca Azure ticari ve Azure kamu Bulutlar Azure günlük tümleştirmeyi destekler. Diğer bulut desteklenmez.

![Azure günlük tümleştirme işlemi][1]

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri ı tümleştirebilir miyim?

Azure Azure her hizmet için ayrıntılı günlük kaydını üretir. Günlükleri üç günlük türleri temsil eder:

* **Denetim/Yönetim günlüklerini**: Görünürlük sağlayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma, güncelleştirme ve silme işlemleri. Bir Azure etkinlik günlüğü, günlük bu tür bir örnektir.
* **Veri günlüklerini düzlemine**: bir Azure kaynağı kullanırken oluşan olayları görünürlük sağlar. Bu günlük türü, Windows Olay Görüntüleyicisi'nin örneğidir **sistem**, **güvenlik**, ve **uygulama** Windows sanal makine kanallarında. Azure İzleyicisi aracılığıyla yapılandırma Azure tanılama günlükleri başka bir örnektir.
* **İşlenen olayların**: çözümlenen olay ve işlenen uyarı bilgileri sağlar. Bu olay türü Azure Güvenlik Merkezi uyarılarını örnektir. Azure Güvenlik Merkezi, işler ve geçerli güvenlikle ilgili tutumunuzu için uygun olan uyarılar sağlamak için aboneliğinizi analiz eder.

Azure günlük tümleştirme ArcSight, QRadar ve Splunk destekler. Satıcı yerel bir bağlayıcı olup olmadığını değerlendirmek için SIEM satıcınızla birlikte denetleyin. Yerel bir bağlayıcı varsa Azure günlük tümleştirme kullanmayın.

Başka hiçbir seçenek varsa, Azure günlük tümleştirme kullanmayı düşünün. Aşağıdaki tabloda bizim öneriler içerir:

|SIEM | Müşteri Azure günlük Tümleştirici zaten kullanıyor | Müşteri SIEM tümleştirme seçeneklerini araştırmaktadır.|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Geçiş başlamadan [Splunk için Azure İzleyici eklentisi](https://splunkbase.splunk.com/app/3534/). | Kullanım [Splunk bağlayıcı](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Geçiş veya son bölümünde belgelenen QRadar connector'ı kullanmaya başlamak [akış izleme verileri tüketim için bir olay hub'ına bir dış aracı tarafından Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). | Son bölümünde belgelenen QRadar bağlayıcıyı kullanmak [akış izleme verileri tüketim için bir olay hub'ına bir dış aracı tarafından Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük Tümleştirici kullanmaya devam etmek ve ardından bağlayıcı tabanlı bir çözüme geçirin.  | Alternatif olarak Azure günlük analizi kullanmayı düşünün. Bağlayıcı kullanılabilir hale geldiğinde geçiş sürecinde gitmek hazır olduğunuz sürece yerleşik Azure günlük tümleştirme yoktur. |

> [!NOTE]
> Azure günlük tümleştirme boş bir çözüm olsa da, günlük dosyası bilgilerini depolama ile ilişkili Azure storage maliyetleri vardır.

Yardıma gereksinim duyarsanız oluşturabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Hizmeti için seçin **günlük tümleştirme**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure günlük tümleştirme kullanıma sunuldu. Azure günlük tümleştirme ve desteklenen günlükleri türleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure günlük tümleştirme ile çalışmaya başlama](security-azure-log-integration-get-started.md). Bu öğreticide Azure günlük tümleştirme yükleme açıklanmaktadır. Ayrıca, Windows Azure tanılama (WAD) depolama, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure Active Directory denetim günlüklerini günlüklerinden tümleştirmek nasıl açıklanır.
* [Azure günlük tümleştirme sık sorulan sorular (SSS)](security-azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan soruları yanıtlar.
* Nasıl yapılır hakkında daha fazla bilgi [bir dış aracı tarafından izleme verileri tüketim için bir olay hub'ına Azure akış](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
