---
title: Toplama ve çözümleme Azure etkinlik günlüklerini Log Analytics çalışma alanında | Microsoft Docs
description: Azure İzleyici günlüklerine Azure etkinlik günlüğü toplayın ve analiz edin ve tüm Azure aboneliklerinizi arasında Azure etkinlik günlüğü aramak için izleme çözümü kullanın.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 5839fd40a128097e400f13acbe4fb6ef90c656b7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248137"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Toplama ve Azure İzleyici'de Log Analytics çalışma alanında Azure etkinlik günlüklerini çözümleme
[Azure etkinlik günlüğü](activity-logs-overview.md) Azure aboneliğinizde gerçekleşen abonelik düzeyindeki olayların hakkında Öngörüler sağlar. Bu makalede bir Log Analytics çalışma alanınıza etkinlik günlüğü toplamak nasıl ve Activity Log Analytics kullanma [izleme çözümü](../insights/solutions.md), sağlayan günlük sorguları ve görünümleri bu verileri çözümlemek için. 

Etkinlik günlüğü bir Log Analytics çalışma alanına bağlanma aşağıdaki avantajları sağlar:

- Birden çok Azure aboneliği analiz için tek bir konumda etkinlik günlüğünde birleştirin.
- Etkinlik günlüğü girdileri 90 günden uzun Store.
- Azure İzleyici tarafından toplanan diğer izleme verilerinin ile etkinlik günlüğü verileri ilişkilendirin.
- Kullanım [oturum sorguları](../log-query/log-query-overview.md) karmaşık bir analiz gerçekleştirin ve etkinlik günlüğü girdileri dayalı derin Öngörüler elde etmek için.

## <a name="connect-to-log-analytics-workspace"></a>Log Analytics çalışma alanına bağlayın
Bir etkinlik günlüğü yalnızca bir çalışma alanına bağlanabilir, ancak tek bir çalışma alanı aynı Azure kiracısı içinde birden fazla aboneliğiniz için etkinlik günlüğüne bağlanabilir. Birden fazla Kiracı genelinde koleksiyon için bkz [bir Log Analytics çalışma alanına Azure Active Directory'de farklı abonelikler arasında Azure etkinlik günlüklerini toplamak Kiracı](activity-log-collect-tenants.md).

Etkinlik günlüğü, Log Analytics çalışma alanına bağlamak için aşağıdaki yordamı kullanın:

1. Gelen **Log Analytics çalışma alanları** menüsünde Azure portalında etkinlik günlükleri toplamak için çalışma alanı seçin.
1. İçinde **çalışma alanı veri kaynakları** çalışma alanınızın menüsünde, select bölümünü **Azure etkinlik günlüğü**.
1. Bağlanmak istediğiniz aboneliğe tıklayın.

    ![Çalışma Alanları](media/activity-log-export/workspaces.png)

1. Tıklayın **Connect** abonelik etkinlik günlüğünde seçilen çalışma alanına bağlamak için. Abonelik zaten başka bir çalışma alanına bağlıysa, tıklayın **Bağlantıyı Kes** ilk bağlantısı kesilemedi.

    ![Çalışma alanları bağlanma](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Log Analytics çalışma alanında analiz edin
Log Analytics çalışma alanına bir etkinlik günlüğü bağlandığınızda girişleri çalışma alanına adlı bir tabloya yazılır **AzureActivity** ile alabileceğiniz bir [günlük sorgusu](../log-query/log-query-overview.md). Bu tablonun yapısını bağlı olarak değişir [günlük girişi kategorisi](activity-logs-overview.md#categories-in-the-activity-log). Bkz: [Azure etkinlik günlüğü olay şeması](activity-log-schema.md) her kategori açıklaması.

## <a name="activity-logs-analytics-monitoring-solution"></a>Etkinlik günlüklerini analiz izleme çözümü
Azure Log Analytics izleme çözümü, birden fazla günlük sorguları ve Log Analytics çalışma alanınızda etkinlik günlük kayıtları çözümlemek için görünümler içerir.

### <a name="install-the-solution"></a>Çözüm yükleme
Yordamı kullanın [bir izleme çözümü yükleme](../insights/solutions.md#install-a-monitoring-solution) yüklemek için **Activity Log Analytics** çözüm. Ek yapılandırma yoktur.

### <a name="use-the-solution"></a>Çözüm kullanın
İzleme çözümleri erişilebilir **İzleyici** Azure portalındaki menü. Seçin **daha fazla** içinde **Insights** açmak için bölüm **genel bakış** çözüm kutucuklarındaki sayfası. **Azure etkinlik günlüklerini** kutucuk sayısını sayısını görüntüler **AzureActivity** çalışma alanınızdaki kaydeder.

![Azure etkinlik günlüklerini kutucuğu](media/collect-activity-logs/azure-activity-logs-tile.png)


Tıklayın **Azure etkinlik günlüklerini** açmak için kutucuğa **Azure etkinlik günlüklerini** görünümü. Görünümü aşağıdaki tabloda görselleştirme bölümü içerir. Her bölüm en fazla 10 öğe belirtilen zaman aralığı için bu bölümleri 's ölçütlerle eşleşen listeler. Tıklayarak tüm eşleşen kayıtlar döndüren bir günlük sorgusu çalıştırabilirsiniz **tümünü gör** bölümünün altındaki.

![Azure etkinlik günlüklerini Panosu](media/collect-activity-logs/activity-log-dash.png)

| Görselleştirme bölümü | Açıklama |
| --- | --- |
| Azure etkinlik günlüğü girdileri | Azure etkinlik günlüğü girdisi üst çubuk grafik, seçtiğiniz tarih aralığı için kayıt toplamları gösterir ve en iyi 10 etkinlik çağıranlar listesini gösterir. Günlük araması çalıştırmak için çubuk grafiğe tıklayın `AzureActivity`. Bu öğe için tüm etkinlik günlüğü girdileri döndüren bir günlük araması gerçekleştirmek için arayan bir öğeye tıklayın. |
| Duruma göre etkinlik günlükleri | Azure etkinlik günlüğü durumunun seçili tarih aralığı ve en iyi on durum kayıtlarını içeren bir liste için bir halka grafik gösterir. Günlük sorgusu çalıştırmak için grafiği tıklatın `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Bu durum kaydını tüm etkinlik günlüğü girişlerini döndüren bir günlük araması çalıştırmak için bir durum öğesini tıklatın. |
| Kaynağa göre etkinlik günlükleri | Etkinlik günlükleri ile kaynakların toplam sayısını gösterir ve kayıt on kaynakları sayar her kaynak için üst listeler. Günlük araması çalıştırmak için toplam alanı `AzureActivity | summarize AggregatedValue = count() by Resource`, çözüme kullanılabilir tüm Azure kaynaklarını gösterir. Bu kaynak için tüm etkinlik kayıtlarını döndüren günlük sorgu çalıştırmak için bir kaynağa tıklayın. |
| Kaynak sağlayıcısı tarafından etkinlik günlükleri | Etkinlik günlüğü oluşturan işler de kaynak sağlayıcıları toplam sayısını gösterir ve ilk on listeler. Toplam alanı için günlük sorgusu `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, tüm Azure kaynak sağlayıcılarını gösterir. Bir kaynak sağlayıcısı için sağlayıcı tüm etkinlik kayıtlarını döndüren günlük sorgu çalıştırmak için tıklayın. |

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [etkinlik günlüğü](activity-logs-overview.md).
- Daha fazla bilgi edinin [Azure İzleyici, veri platformu](data-platform.md).
- Kullanım [oturum sorguları](../log-query/log-query-overview.md) etkinlik günlüğünüzü ayrıntılı bilgileri görüntülemek için.
