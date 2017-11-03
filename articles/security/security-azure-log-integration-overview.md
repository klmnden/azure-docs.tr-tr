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
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 6c3a2ac18fdb7a7a722447af720b9dee28adef08
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Microsoft Azure günlük tümleştirme giriş
Azure günlük tümleştirme, önemli işlevleri ve nasıl çalıştığı hakkında bilgi edinin.

## <a name="overview"></a>Genel Bakış

Azure günlük tümleştirme, Azure kaynaklarınızı ham günlüklerinden şirket içi güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlere tümleştirmenize olanak sağlayan ücretsiz bir çözümdür.

Azure günlük tümleştirme toplar Windows olaylarını Windows Olay Görüntüleyicisi'ni günlüklerinden [Azure etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [Azure Güvenlik Merkezi uyarılarını](../security-center/security-center-intro.md), ve [Azure tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) gelen Azure kaynakları. Bu tümleştirme tüm varlıklar için birleştirilmiş bir Pano sağlar, SIEM çözümünüzün yardımcı olur, şirket içi veya bulutta araya getirebilirsiniz böylece ilişkilendirmek, çözümlemek ve güvenlik olayları için uyarı.

>[!NOTE]
Şu anda yalnızca desteklenen Bulutlar Azure ticari ve Azure kamu var. Diğer bulut desteklenmez.

![Azure günlük tümleştirmesi][1]

## <a name="what-logs-can-i-integrate"></a>Hangi günlükleri ı tümleştirebilir miyim?
Azure Azure her hizmet için ayrıntılı günlük kaydını üretir. Bu günlükler günlükleri üç tür temsil eder:

* **Denetim/Yönetim günlüklerini** görünürlük sağlayan [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) oluşturma, güncelleştirme ve silme işlemleri. Azure etkinlik günlükleri, günlük bu tür bir örnektir.
* **Veri günlüklerini düzlemine** bir Azure kaynak kullanımını bir parçası olarak oluşturulan olaylara görünürlük sağlar. Bu günlük türü, Windows Olay Görüntüleyicisi'nin örneğidir **sistem**, **güvenlik**, ve **uygulama** Windows sanal makine kanallarında. Azure İzleyicisi aracılığıyla yapılandırılmış Tanılama Günlüğü başka bir örnek verilmiştir
* **İşlenen olayların** çözümlenen olay ve sizin adınıza işlenen uyarı bilgileri sağlar. Azure Güvenlik Merkezi burada ve işlenen geçerli güvenlikle ilgili tutumunuzu için ilgili uyarıları sağlamak için aboneliğinizi analiz Azure Güvenlik Merkezi uyarılarını, bu olay türü örnektir.

Azure günlük tümleştirme ArcSight, QRadar ve Splunk destekler. Tüm durumlarda, yerel bir bağlayıcı sahip olup olmadığını değerlendirmek için SIEM satıcınıza danışın. Bazı durumlarda, yerel bağlayıcılar mevcut olmadığında Azure günlük tümleştirme kullanması gerekmez. Ek bilgi günlük Lütfen ziyaret SSS desteklenen türleri için.

>[!NOTE]
Azure günlük tümleştirme boş bir çözüm olsa da, günlük dosyası bilgilerini depolama alanından kaynaklanan Azure storage maliyetleri vardır.

Topluluk Yardım ile de kullanılabilir [Azure günlük tümleştirme MSDN Forumu](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Forum AzLog topluluk birbirine sorular, yanıtlar, ipuçları ve püf noktaları Azure günlük tümleştirme yararlanmak nasıl destek olanağı sağlar. Ayrıca, Azure günlük tümleştirme takım Bu forumda izler ve geçebiliriz her yardımcı olur.

Ayrıca açabilirsiniz bir [destek isteği](../azure-supportability/how-to-create-azure-support-request.md). Bunu yapmak için seçin **günlük tümleştirme** destek isteyen hizmet olarak.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure günlük tümleştirme tanıtıldı. Azure günlük tümleştirme ve desteklenen günlükleri türleri hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Microsoft Azure günlük tümleştirme](https://www.microsoft.com/download/details.aspx?id=53324) – merkezi ayrıntılı bilgi için sistem gereksinimleri, yükleyip yönergeler Azure günlük tümleştirme.
* [Azure günlük tümleştirme ile çalışmaya başlama](security-azure-log-integration-get-started.md) - Bu öğretici, Azure günlük tümleştirme yüklenmesinde size yol gösterir ve Azure WAD depolama, Azure etkinlik günlükleri günlüklerinden tümleştirme Azure Güvenlik Merkezi uyarılarını ve Azure Active Directory denetim günlükleri.
* [Ortak yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – bu blog gönderisine Splunk, HP ArcSight ve IBM QRadar iş ortağı çözümleri ile çalışmak için Azure günlük tümleştirmesini yapılandırma gösterilmektedir. Bu Web günlüğü, iş ortağı çözümleri yapılandırma bizim geçerli konumu temsil eder. Her durumda, Lütfen ilk iş ortağı çözümü belgelerine başvurun.
* [Etkinlik ve ASC uyarıları QRadar syslog üzerinden](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/) – bu blog gönderisine etkinliği ve Azure Güvenlik Merkezi uyarıları için QRadar syslog göndermek için gereken adımları anlatır
* [Azure günlük sık sorulan sorular (SSS) tümleştirme](security-azure-log-integration-faq.md) -bu SSS Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
* [Güvenlik Merkezi tümleştirme uyarıları Azure ile tümleştirme oturum](../security-center/security-center-integrating-alerts-with-log-integration.md) – bu belgede Azure Güvenlik Merkezi uyarılarını Azure günlük Tümleştirmesi ile eşitlemek gösterilmektedir.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
