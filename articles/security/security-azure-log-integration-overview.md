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
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6d91692a64a4d3def80990a439fe0a0898bf2f09
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-azure-log-integration"></a>Azure günlük tümleştirme giriş

Azure günlük tümleştirme ham günlükleri Azure kaynaklarınızdan, şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlerle tümleştirmek için kullanabilirsiniz. Azure günlük tümleştirme yalnızca bir bağlayıcı kullanırsanız [Azure İzleyici](../monitoring-and-diagnostics/monitoring-get-started.md) SIEM satıcınızdan kullanılamaz.

Tümleştirme Azure günlükleri için tercih edilen yöntem SIEM satıcınızın Azure İzleyici bağlayıcı kullanmaktır. Bağlayıcıyı kullanmak üzere'ndaki yönergeleri izleyin [olay hub'ları için veriler izleme monitör akış](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). 

SIEM satıcınıza Azure İzleyici için bir bağlayıcı sağlamıyorsa bir bağlayıcı kullanılabilir hale gelene kadar geçici bir çözüm olarak Azure günlük tümleştirme kullanmanız mümkün olabilir. Yalnızca Azure günlük tümleştirme SIEM'iniz destekliyorsa azure günlük tümleştirme bir seçenektir.

> [!IMPORTANT]
> Birincil ilgi sanal makine günlüklerinin toplanması, çoğu SIEM satıcısını kendi çözümde bu seçenek içerir. SIEM satıcının bağlayıcı her zaman tercih edilen alternatif kullanmaktır.

Azure günlük tümleştirme toplar Windows olaylarını Windows Olay Görüntüleyicisi'ni günlüklerinden [Azure etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Güvenlik Merkezi uyarılarını](../security-center/security-center-intro.md), ve [Azure tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) gelen Azure kaynakları. Tümleştirme yardımcı olan SIEM çözümünüzü birleştirilmiş bir pano, tüm varlıklar için mi sağlamak şirket içi veya bulutta. Alma, toplama, bağıntılı ve güvenlik olayları için uyarıları çözümlemek için panoyu kullanabilirsiniz.

> [!NOTE]
> Şu anda yalnızca Azure ticari ve Azure kamu Bulutlar Azure günlük tümleştirmeyi destekler. Diğer bulut desteklenmez.

![Azure günlük tümleştirme işlemi diyagramı][1]

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri ı tümleştirebilir miyim

Azure Azure her hizmet için ayrıntılı günlük kaydını üretir. Günlükleri üç günlük türleri temsil eder:

* **Denetim/Yönetim günlüklerini**: Görünürlük sağlayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma, güncelleştirme ve silme işlemleri. Bir Azure etkinlik günlüğü, günlük bu tür bir örnektir.
* **Veri günlüklerini düzlemine**: bir Azure kaynağı kullanırken oluşan olayları görünürlük sağlar. Bu günlük türü, Windows Olay Görüntüleyicisi'nin örneğidir **sistem**, **güvenlik**, ve **uygulama** Windows sanal makine kanallarında. Azure tanılama günlük Azure İzleyicisi aracılığıyla yapılandırmak başka bir örnektir.
* **İşlenen olayların**: çözümlenen olay ve işlenen uyarı bilgileri sağlar. Bu olay türü Azure Güvenlik Merkezi uyarılarını örnektir. Azure Güvenlik Merkezi, işler ve geçerli güvenlikle ilgili tutumunuzu için uygun olan uyarılar sağlamak için aboneliğinizi analiz eder.

Azure günlük tümleştirme ArcSight, QRadar ve Splunk destekler. Satıcı yerel bir bağlayıcı olup olmadığını değerlendirmek için SIEM satıcınızla birlikte denetleyin. Yerel bir bağlayıcı varsa Azure günlük tümleştirme kullanmayın.

Başka hiçbir seçenek varsa, Azure günlük tümleştirme kullanmayı düşünün. Aşağıdaki tabloda bizim öneriler içerir:

|SIEM | Müşteri Azure günlük Tümleştirici zaten kullanıyor | Müşteri SIEM tümleştirme seçeneklerini araştırmaktadır.|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Geçiş başlamadan [Splunk için Azure İzleyici eklentisi](https://splunkbase.splunk.com/app/3534/). | Kullanım [Splunk bağlayıcı](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Geçiş veya son bölümünde belgelenen QRadar connector'ı kullanmaya başlamak [akış izleme verileri tüketim için bir olay hub'ına bir dış aracı tarafından Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). | Son bölümünde belgelenen QRadar Bağlayıcısı'nı kullanmak [akış izleme verileri tüketim için bir olay hub'ına bir dış aracı tarafından Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Bir bağlayıcı kullanılabilir hale gelene kadar Azure günlük Tümleştirici kullanmaya devam edin. Ardından, bağlayıcı tabanlı bir çözüme geçirin.  | Alternatif olarak Azure günlük analizi kullanmayı düşünün. Bağlayıcı kullanılabilir hale geldiğinde geçiş sürecinde gitmek hazır olduğunuz sürece yerleşik Azure günlük tümleştirme yoktur. |

> [!NOTE]
> Azure günlük tümleştirme boş bir çözüm olsa da, günlük dosyası bilgilerini depolama ile ilişkili Azure storage maliyetleri vardır.

Yardıma gereksinim duyarsanız oluşturabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Hizmeti için seçin **günlük tümleştirme**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure günlük tümleştirme tanıtılır. Azure günlük tümleştirme ve desteklenen günlükleri türleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure günlük tümleştirme ile çalışmaya başlama](security-azure-log-integration-get-started.md). Bu öğreticide Azure günlük tümleştirme yükleme açıklanmaktadır. Ayrıca, Windows Azure tanılama (WAD) depolama, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarılarını ve Azure Active Directory denetim günlüklerini günlüklerinden tümleştirmek nasıl açıklanır.
* [Azure günlük tümleştirme sık sorulan sorular (SSS)](security-azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan soruları yanıtlar.
* Nasıl yapılır hakkında daha fazla bilgi [bir dış aracı tarafından izleme verileri tüketim için bir olay hub'ına Azure akış](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
