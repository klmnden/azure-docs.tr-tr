---
title: "Toplamak ve günlük analizi içinde Azure etkinlik günlüklerini analiz edin | Microsoft Docs"
description: "Azure etkinlik günlükleri çözüm çözümlemek ve tüm Azure abonelikleri Azure etkinlik günlüğü aramak için kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: magoedte
ms.openlocfilehash: c13890862c058701268c07d032d6d990c659287a
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics"></a>Toplamak ve günlük analizi içinde Azure etkinlik günlüklerini analiz edin

![Azure etkinlik günlükleri simgesi](./media/log-analytics-activity/activity-log-analytics.png)

Etkinlik günlük analizi çözüm arayın ve çözümlemenize yardımcı olur [Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) tüm Azure abonelikleri arasında. Azure etkinlik günlüğü aboneliklerinizi kaynaklarında üzerinde gerçekleştirilen işlemler Öngörüler sunan bir günlüktür. Etkinlik günlüğü önceden olarak biliniyordu *denetim günlüklerini* veya *işlem günlüklerini* aboneliklerinizi olaylarını raporları beri.

Etkinlik günlüğü kullanarak, belirleyebilirsiniz *ne*, *kimin*, ve *zaman* herhangi yazma işlemleri için aboneliğinizi kaynaklarında yapılan (PUT, POST, DELETE) için. Ayrıca, işlemleri ve diğer ilgili özellikleri durumunu anlayabilirsiniz. Etkinlik günlüğü okuma (GET) işlemleri ya da Klasik dağıtım modeli kullanan kaynakları işlemlerinde içermez.

Azure etkinlik günlükleriniz için günlük analizi bağlandığınızda, şunları yapabilirsiniz:

- Önceden tanımlanmış görünümleri ile etkinlik günlüklerini analiz edin
- Analiz etmek ve birden çok Azure abonelikleri arama ve etkinlik günlükleri
- Etkinlik günlükleri 90 günden daha uzun süre tutmak<sup>1</sup>
- Etkinlik günlükleri diğer Azure ile ilişkilendirmek platform ve uygulama verileri
- Duruma göre toplanan işletimsel etkinlikleri bakın
- Her Azure hizmetlerinizi gerçekleştiği etkinliklerin eğilimleri görüntüleme
- Tüm Azure kaynaklarınızı yetkilendirme değişiklikleri hakkında rapor oluşturun.
- Kaynaklarınızın etkileyen kesintisinden veya hizmet sistem durumu sorunlarını belirle
- Kullanıcı etkinlikleri, otomatik ölçeklendirme işlemleri, yetkilendirme değişikliklerini ve diğer günlükler veya ölçümleri hizmet sistem durumu, ortamınızdan ilişkilendirmek için günlük Ara'yı kullanın

<sup>1</sup>ücretsiz katmanında olsa bile, varsayılan olarak, günlük analizi Azure etkinlik günlüklerinizi 90 gün boyunca tutar. Veya 90 günden çalışma saklama ayarı varsa. Çalışma alanınızı 90 günden daha uzun bekletme varsa, etkinlik günlükleri için çalışma alanınızı Bekletme dönemi tutulur.

Günlük analizi, etkinlik günlükleri ücretsiz toplar ve ücretsiz 90 gün için günlükleri depolar. 90 günden daha uzun süre günlüklerini saklıyorsanız 90 günden daha uzun depolanan veriler için veri bekletme ücret uygulanabilir.

Ücretsiz fiyatlandırma katmanı olduğunuzda, etkinlik günlükleri, günlük veri tüketimi için geçerli değildir.

## <a name="connected-sources"></a>Bağlı kaynaklar

Diğer çoğu günlük analizi çözümleri, etkinlik günlükleri için aracıları tarafından toplanan veriler değil. Çözüm tarafından kullanılan tüm verileri doğrudan Azure'dan gelir.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Çözüm Windows aracılardan bilgi toplamaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Çözüm, Linux aracılarını bilgi toplamaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | Çözüm bağlı SCOM yönetim grubunda aracılardan gelen bilgiler toplamaz. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Çözüm, Azure depolama biriminden bilgi toplamaz. |

## <a name="prerequisites"></a>Önkoşullar

- Azure etkinlik günlüğü bilgilerine erişmek için bir Azure aboneliğinizin olması gerekir.

## <a name="configuration"></a>Yapılandırma

Alanlarınızı etkinlik günlük analizi çözümü yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Etkinlik günlük analizi çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. Günlük analizi çalışma alanına gidin, etkinlik günlükleri yapılandırın.
    1. Azure portalında çalışma alanınızı seçin ve ardından **Azure etkinlik günlüğü**.
    2. Her abonelik için abonelik adını tıklatın.  
        ![Abonelik Ekle](./media/log-analytics-activity/add-subscription.png)
    3. İçinde *varlığıyla SubscriptionName* dikey penceresinde tıklatın **Bağlan**.  
        ![Abonelik Bağlan](./media/log-analytics-activity/subscription-connect.png)

OMS Portalı'nı kullanarak çözüm eklerseniz, aşağıdaki kutucuk görürsünüz. Bir Azure aboneliği, çalışma alanına bağlanmak için Azure portalında oturum açın.  
![değerlendirme gerçekleştirme](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a>Çözümü kullanma

Etkinlik günlüğü analiz çözümü, çalışma alanına eklediğinizde **Azure etkinlik günlükleri** kutucuğu, genel bakış Panosu'na eklenir. Bu kutucuğu erişimi çözümü olan Azure aboneliklerinin Azure etkinlik kayıtlarını sayısını görüntüler.

![Azure etkinlik günlükleri döşeme](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Görünüm Azure etkinliğini günlüğe kaydeder

Tıklatın **Azure etkinlik günlükleri** açmak için kutucuğa **Azure etkinlik günlükleri** Pano. Pano Kanatlar aşağıdaki tabloda içerir. Her dikey penceresinde belirtilen kapsam ve zaman aralığı için o dikey 's ölçütlerle eşleşen en fazla 10 öğeleri listeler. Tıklayarak tüm kayıtları döndüren bir günlük arama çalıştırabilirsiniz **tümünü görmek** alt dikey veya dikey başlığını tıklatarak.

Etkinlik günlüğü verileri yalnızca görünür *sonra* , daha önce verileri görüntüleyemezsiniz şekilde çözüme gitmek için etkinlik günlükleri yapılandırdığınız.

| Dikey penceresi | Açıklama |
| --- | --- |
| Azure etkinlik günlüğü girişleri | Çubuk grafik Azure etkinlik günlüğü girişi üst halinde seçtiğiniz tarih aralığını kayıt toplamlarını gösterir ve üst 10 etkinlik arayanlar listesini gösterir. Günlük aramasını çalıştırmak için çubuk grafiği tıklatın <code>AzureActivity</code>. Bu öğe için tüm etkinlik günlüğü girişleri döndüren bir günlük arama çalıştırmak için bir arayan öğesini tıklatın. |
| Duruma göre etkinlik günlükleri | Azure etkinlik günlüğü durumu seçtiğiniz tarih aralığı için bir halka grafik gösterir. Ayrıca bir liste üst on durum kayıtların listesini gösterir. Günlük aramasını çalıştırmak için grafiği tıklatın <code>AzureActivity &#124; summarize AggregatedValue = count() by ActivityStatus</code>. Bu durum kaydı tüm etkinlik günlüğü girişlerini döndüren bir günlük arama çalıştırmak için bir durum öğesini tıklatın. |
| Kaynak tarafından etkinlik günlükleri | Etkinlik günlükleri kaynaklarla toplam sayısını gösterir ve kaydıyla on kaynakları sayar her kaynak için üst listeler. Günlük aramasını çalıştırmak için toplam alanı <code>AzureActivity &#124; summarize AggregatedValue = count() by Resource</code>, çözüme kullanılabilir tüm Azure kaynakları gösterir. Bu kaynak için tüm etkinlik kayıtlarını döndüren bir günlük arama çalıştırmak için bir kaynak'ı tıklatın. |
| Kaynak sağlayıcısı tarafından etkinlik günlükleri | Etkinlik üretmek kaynak sağlayıcıları toplam sayısı günlüğe kaydeder ve ilk on listeler gösterir. Günlük aramasını çalıştırmak için toplam alanı <code>AzureActivity &#124; summarize AggregatedValue = count() by ResourceProvider</code>, tüm Azure kaynak sağlayıcıları gösterir. Sağlayıcı için tüm etkinlik kayıtlarını döndüren bir günlük arama çalıştırmak için bir kaynak Sağlayıcısı'ı tıklatın. |

![Azure etkinlik günlükleri Panosu](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Sonraki adımlar

- Oluşturma bir [uyarı](log-analytics-alerts-creating.md) zaman belirli bir etkinliğe olur.
- Kullanım [günlük arama](log-analytics-log-searches.md) , etkinlik günlükleri ayrıntılı bilgileri görüntülemek için.
