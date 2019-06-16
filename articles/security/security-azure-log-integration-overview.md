---
title: Azure kaynaklarından günlüklerini SIEM Sistemlerinizle tümleştirin | Microsoft Docs
description: Azure günlük tümleştirmesi, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/28/2019
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 65e256b476c1e459ae937d9f6cbb43e0020fd9fe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66298127"
---
# <a name="introduction-to-azure-log-integration"></a>Azure günlük tümleştirmesine giriş

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanımdan kaldırılacaktır. AzLog yüklemeleri, 27 Haziran 2018'de devre dışı bırakıldı. Taşıma iletme gözden geçirme sonrası yapmanız gerekenler hakkında rehberlik için [SIEM araçlarla tümleştirmek için kullanım Azure İzleyici](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirmesi, Azure tümleştirme görevlerini basitleştirmek kullanılabilir yapıldı ile şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemine günlükleri.

 Azure tümleştirme günlükleri için önerilen yöntem, SIEM satıcınızın bağlayıcılar kullanmaktır. Azure İzleyici günlükler event hubs'a akış olanağı sağlar ve daha fazla olay hub'ından günlüklerini SIEM ile tümleştirme için bağlayıcılar SIEM satıcısını yazabilirsiniz.  Bunun nasıl çalıştığını bir açıklaması için yönergeleri izleyin [İzleyici akışı izleme verileri olay hub'ları](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Makale ayrıca doğrudan Azure bağlayıcıları zaten mevcut olduğu Sıem'lerden listelenir.  

> [!IMPORTANT]
> Birincil ilgi, sanal makine günlüklerinin toplanması, çoğu SIEM satıcısını bu seçenek, çözüme ekleyin. SIEM Bağlayıcısı satıcısının her zaman tercih edilen alternatif kullanmaktır.

Özelliği kullanım dışı kadar Azure günlük tümleştirme özelliği belgelerine yine sağlanıyor.

Daha fazla Azure günlük tümleştirme özelliği hakkında daha fazla bilgi edinmek için okuma:

Azure günlük tümleştirmesi, Windows Olay Görüntüleyicisi'ni günlüklerinden Windows olaylarını toplar [Azure etkinlik günlüklerini](../azure-monitor/platform/activity-logs-overview.md), [Azure Güvenlik Merkezi uyarılarını](../security-center/security-center-intro.md), ve [Azure tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) gelen Azure kaynakları. Tümleştirme yardımcı olur, birleştirilmiş bir pano, tüm varlıklar için mi sağlamak SIEM çözümünüze şirket içinde veya bulutta. Bir pano, alma, toplama, ilişkilendirmenize ve güvenlik olayları için uyarıları çözümlemek için kullanabilirsiniz.

> [!NOTE]
> Şu anda yalnızca Azure ticari ve Azure kamu bulutlarında Azure günlük tümleştirmesi destekler. Diğer bulutlarda desteklenmez.

![Azure günlük tümleştirme işlemi][1]

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri tümleştirebilirim?

Azure, her bir Azure hizmeti için ayrıntılı günlük kaydını üretir. Günlükleri üç günlük türlerini temsil eder:

* **Denetim/Yönetim günlükleri**: Görünürlük sağlayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma, güncelleştirme ve silme işlemleri. Azure etkinlik günlüğü, bu tür bir günlük örneğidir.
* **Veri düzlemi günlükleri**: Bir Azure kaynağı kullandığınızda başlatılan olaylara yönelik görünürlük sağlar. Bu tür bir günlük, Windows Olay Görüntüleyicisi'nin örneğidir **sistem**, **güvenlik**, ve **uygulama** kanalları bir Windows sanal makine. Azure İzleyici yapılandırdığınız Azure tanılama günlükleri, başka bir örnektir.
* **İşlenen olaylar**: Çözümlenen olay ve işlenen uyarı bilgileri sizin için sağlayın. Azure Güvenlik Merkezi uyarılarını bu tür bir olayda örneğidir. Azure Güvenlik Merkezi, işler ve geçerli güvenlik duruşunuzu ilgili uyarılar sağlamak için aboneliğinizi analiz eder.

Azure günlük tümleştirmesi ArcSight QRadar ve Splunk destekler. Yerel bir bağlayıcı satıcı sahip olup olmadığını değerlendirmek için SIEM satıcınızla denetleyin. Azure günlük tümleştirmesi, yerel bir bağlayıcı varsa kullanmayın.

Diğer bir seçenek kullanılabilir Azure günlük tümleştirmesi kullanmayı düşünün. Aşağıdaki tablo, önerilerimizle içerir:

|SIEM | Müşteri Azure günlük Tümleştiricisini zaten kullanıyor. | Müşteri SIEM tümleştirme seçenekleri araştırmaktadır.|
|---------|--------------------------|-------------------------------------------|
|**Splunk** | Geçiş başlamak [Splunk için Azure İzleyici eklentisi](https://splunkbase.splunk.com/app/3534/). | Kullanım [Splunk bağlayıcı](https://splunkbase.splunk.com/app/3534/). |
|**QRadar** | Geçiş veya son kısmında belgelenen QRadar bağlayıcıyı kullanmaya başlamak [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). | Son bölümde belgelenen QRadar bağlayıcıyı kullanmak [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). |
|**ArcSight** | Azure günlük Tümleştiricisini bağlayıcı kullanılabilir hale gelene kadar kullanmaya devam edin ve ardından bağlayıcı tabanlı bir çözüm geçirin.  | Alternatif olarak Azure İzleyici günlüklerine kullanmayı düşünün. Yerleşik bağlayıcı kullanıma sunulduğunda geçiş sürecinde Git iradeye sahip değilseniz Azure günlük tümleştirmesi için kullanmayın. |

> [!NOTE]
> Azure günlük tümleştirmesi ücretsiz bir çözüm olsa da, günlük dosya bilgileri depolama ile ilişkili Azure depolama maliyeti yoktur.

Yardıma ihtiyacınız varsa, oluşturabileceğiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Hizmet için seçin **günlük tümleştirmesi**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure günlük tümleştirmesi için kullanıma sunuldu. Azure günlük tümleştirmesi ve desteklenen günlük türlerini hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure günlük tümleştirmesini kullanmaya başlama](security-azure-log-integration-get-started.md). Bu öğreticide, Azure günlük tümleştirmesi yükleme işlemine açıklanmaktadır. Ayrıca, günlükleri Windows Azure tanılama (WAD) depolama, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure Active Directory denetim günlüklerini tümleştirme işlemini açıklamaktadır.
* [Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS)](security-azure-log-integration-faq.md). Bu SSS, Azure günlük tümleştirmesi hakkında sık sorulan sorular yanıtlanmaktadır.
* Kullanma hakkında daha fazla bilgi edinin [Azure harici bir aracı tarafından izleme verilerini tüketim için olay hub'ına akış](../azure-monitor/platform/stream-monitoring-data-event-hubs.md).

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
