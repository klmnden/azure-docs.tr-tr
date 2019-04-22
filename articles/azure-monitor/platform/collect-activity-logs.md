---
title: Toplama ve çözümleme Azure etkinlik günlüklerini Log Analytics çalışma alanında | Microsoft Docs
description: Azure etkinlik günlükleri çözümü, analiz etmek ve tüm Azure aboneliklerinizi arasında Azure etkinlik günlüğü aramak için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: magoedte
ms.openlocfilehash: 4476bb0a5a343fd43ce5ed70cf0e493d0ccae0e9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59505643"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Toplama ve Azure İzleyici'de Log Analytics çalışma alanında Azure etkinlik günlüklerini çözümleme

![Azure etkinlik günlüklerini simgesi](./media/collect-activity-logs/activity-log-analytics.png)

Etkinlik günlüğü analizi çözümü arayın ve çözümlemenize yardımcı olur. [Azure etkinlik günlüğü](activity-logs-overview.md) tüm Azure abonelikleri arasında. Azure etkinlik günlüğü aboneliklerinizdeki kaynakları üzerinde gerçekleştirilen işlemleri hakkında Öngörüler sunan bir günlüktür. Etkinlik günlüğü önceden olarak biliniyordu *denetim günlüklerini* veya *işlem günlüklerini* olayları Abonelikleriniz için raporları olduğundan.

Etkinlik günlüğü'nü kullanarak belirleyebilirsiniz *ne*, *kimin*, ve *olduğunda* işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar için yapılan herhangi bir yazma için. Ayrıca, işlemleri ve diğer ilgili özellikler durumunu anlayabilirsiniz. Etkinlik günlüğünü okuma (GET) işlemlerini ya da Klasik dağıtım modelini kullanan kaynakları işlemlerinde dahil değildir.

Azure etkinlik günlüklerini Log Analytics çalışma alanına bağlandığınızda, şunları yapabilirsiniz:

- Önceden tanımlanmış görünümleri ile etkinlik günlüklerini çözümleme
- Analiz edin ve birden çok Azure aboneliklerinden gelen etkinlik günlüklerinde arama yapma
- Etkinlik günlüklerini 90 günden daha uzun süre tutmak<sup>1</sup>
- Etkinlik günlükleri ile diğer Azure bağıntısını platform ve uygulama verileri
- Duruma göre toplanmış işletim etkinliklerini bakın
- Her Azure hizmetlerinizi geçekleşmiş etkinliklerin eğilimleri görüntüleme
- Tüm Azure kaynaklarınızın yetkilendirme değişikliklerini raporlama
- Kaynaklarınızı etkileyen kesinti veya hizmet sistem durumu sorunlarını tanımlama
- Kullanıcı etkinliklerini, otomatik ölçeklendirme işlemlerini, yetkilendirme değişikliklerini ve diğer günlükleri ve ölçümleri hizmet durumunu ortamınızdaki ilişkilendirmek için günlük araması'nı kullanın

<sup>1</sup>ücretsiz katmanında olsa bile varsayılan olarak, Azure İzleyici, Azure etkinlik günlüklerini Log Analytics çalışma alanında 90 gün boyunca tutar. Veya 90 günden daha az workspace saklama ayarı varsa. Etkinlik günlükleri 90 günden daha uzun bekletme çalışma alanınız varsa çalışma alanınızı sunucusundaki bekletme süresini göre tutulur.

Log Analytics çalışma alanı ücretsiz olarak etkinlik günlüklerini toplar ve günlükleri, 90 gün boyunca ücretsiz olarak depolar. Günlükleri, 90 günden daha uzun süre saklamak, veri saklama 90 günden daha uzun depolanan veriler için ücretlendirilirsiniz.

Ücretsiz fiyatlandırma katmanı olduğunuzda, etkinlik günlükleri, günlük veri tüketimi için geçerli değildir.

## <a name="connected-sources"></a>Bağlı kaynaklar

Diğer birçok Azure İzleyici çözümlerinden farklı olarak, etkinlik günlükleri için aracıları tarafından verileri toplanmaz. Çözüm tarafından kullanılan tüm verileri doğrudan Azure gelir.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Windows aracıları](agent-windows.md) | Hayır | Çözüm, Windows aracılarından bilgi toplamaz. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Çözüm, Linux aracılarından bilgi toplamaz. |
| [System Center Operations Manager yönetim grubu](om-agents.md) | Hayır | Bir Operations Manager yönetim grubuna raporlanan aracılardan çözüm herhangi bir bilgi toplamaz. |
| [Azure depolama hesabı](collect-azure-metrics-logs.md) | Hayır | Çözüm, Azure depolama biriminden bir bilgi toplamaz. |

## <a name="prerequisites"></a>Önkoşullar

Azure etkinlik günlüğü bilgilerine erişmek için bir Azure aboneliğinizin olması gerekir.

Çözüm ayrıca aşağıdaki iki kaynak sağlayıcıları, aboneliğinize kayıtlı olduğundan gerektirir:

1. Microsoft.OperationalInsights
2. Microsoft.OperationsManagement

Kaydolun veya kayıtlı oldukları doğrulamak hakkında bilgi edinmek için bkz: [Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md)

## <a name="configuration"></a>Yapılandırma

Etkinlik günlüğü analizi çözümü, çalışma alanları için yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

2. Etkinlik Günlüğü Analizi çözümünü [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview)'ten veya [Çözüm Galerisi'nden Log Analytics çözümleri ekleme](../insights/solutions.md) başlığı altında açıklanan işlemi kullanarak etkinleştirin.

2. Etkinlik günlüklerini Log Analytics çalışma alanınıza dönmek için yapılandırın.
    1. Azure portalında çalışma alanınızı seçin ve ardından **Azure etkinlik günlüğü**.
    2. Her abonelik için abonelik adına tıklayın.  
        
        ![Abonelik Ekle](./media/collect-activity-logs/add-subscription.png)
    
    3. İçinde *SubscriptionName* dikey penceresinde tıklayın **Connect**.  
    
        ![Abonelik'e bağlanma](./media/collect-activity-logs/subscription-connect.png)

## <a name="using-the-solution"></a>Çözümü kullanma

Etkinlik günlüğü analizi çözümü, çalışma alanınıza eklediğinizde **Azure etkinlik günlüklerini** kutucuk genel bakış panonuza eklenir. Bu kutucuk, Azure etkinlik kayıtlarını erişimi çözümü olan Azure aboneliklerinin sayısını görüntüler.

![Azure etkinlik günlüklerini kutucuğu](./media/collect-activity-logs/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Görünüm Azure etkinlik günlükleri

Tıklayın **Azure etkinlik günlüklerini** açmak için kutucuğa **Azure etkinlik günlüklerini** Pano. Pano aşağıdaki tabloda gösterilen dikey pencereleri içerir. Her dikey pencerede, dikey pencerenin belirtilen kapsam ve zaman aralığına yönelik ölçütleriyle eşleşen en fazla 10 öğe listelenir. Dikey pencerenin altındaki **Tümünü göster**’e tıklayarak veya dikey pencere başlığına tıklayarak tüm kayıtları döndüren bir günlük araması yapabilirsiniz.

Etkinlik günlüğü verileri bir yalnızca görünür *sonra* etkinliği günlüklerinizi daha önce veri görüntüleyemezsiniz çözüme konulunca yapılandırdınız.

| Dikey penceresi | Açıklama |
| --- | --- |
| Azure etkinlik günlüğü girdileri | Azure etkinlik günlüğü girdisi üst çubuk grafik, seçtiğiniz tarih aralığı için kayıt toplamları gösterir ve en iyi 10 etkinlik çağıranlar listesini gösterir. Günlük araması çalıştırmak için çubuk grafiğe tıklayın <code>AzureActivity</code>. Bu öğe tüm etkinlik günlüğü girdilerini döndüren bir günlük araması gerçekleştirmek için arayan bir öğeye tıklayın. |
| Duruma göre etkinlik günlükleri | Azure etkinlik günlüğü durumunun seçtiğiniz tarih aralığı için bir halka grafiği gösterir. Ayrıca üst on durum kayıtlarını içeren bir liste listesini gösterir. Günlük araması çalıştırmak için grafiği tıklatın <code>AzureActivity &#124; summarize AggregatedValue = count() by ActivityStatus</code>. Bu durum kaydını tüm etkinlik günlüğü girdilerini döndüren bir günlük araması gerçekleştirmek için bir durum öğesini tıklatın. |
| Kaynağa göre etkinlik günlükleri | Etkinlik günlükleri ile kaynakların toplam sayısını gösterir ve kayıt on kaynakları sayar her kaynak için üst listeler. Günlük araması çalıştırmak için toplam alanı <code>AzureActivity &#124; summarize AggregatedValue = count() by Resource</code>, çözüme kullanılabilir tüm Azure kaynaklarını gösterir. Bu kaynak için tüm etkinlik kayıtlarını döndüren günlük araması çalıştırmak için bir kaynağa tıklayın. |
| Kaynak sağlayıcısı tarafından etkinlik günlükleri | Etkinliği oluşturan kaynak sağlayıcıları toplam sayısını kaydeder ve ilk on listeler gösterir. Günlük araması çalıştırmak için toplam alanı <code>AzureActivity &#124; summarize AggregatedValue = count() by ResourceProvider</code>, tüm Azure kaynak sağlayıcılarını gösterir. Bir kaynak sağlayıcısı için sağlayıcı tüm etkinlik kayıtlarını döndüren günlük araması çalıştırmak için tıklayın. |

![Azure etkinlik günlüklerini Panosu](./media/collect-activity-logs/activity-log-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [uyarı](../platform/alerts-metric.md) belirli bir etkinlik olduğunda gerçekleşir.
- Kullanım [günlük araması](../log-query/log-query-overview.md) , etkinlik günlüklerinden daha ayrıntılı bilgi görüntülemek için.
