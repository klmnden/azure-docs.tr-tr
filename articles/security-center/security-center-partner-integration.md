---
title: Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs
description: Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: d94567800a9fd020784c9cb07b2c6824cd032509
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064283"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

> [!NOTE]
> Güvenlik çözümlerinin bir alt kümesi, 31 Temmuz 2019 üzerinde kullanımdan kaldırılacaktır. Daha fazla bilgi ve diğer hizmetler için bkz. [devre dışı bırakılması, Güvenlik Merkezi özelliklerini (Temmuz 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş Dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay sağlama sunar. Güvenlik Merkezi, kötü amaçlı yazılımdan koruma ve güvenlik açığı değerlendirmesi gibi çözümler için gerekli aracıyı sanal makinelerinize sağlayabilir. Güvenlik duvarı cihazları için ise Güvenlik Merkezi gerekli ağ yapılandırmasının çoğunluğunu gerçekleştirebilir.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve Yönetim**: Müşteriler, tümleşik sistem durumu olaylarını kullanarak bir bakışta tüm iş ortağı çözümlerini izleyebilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Şu anda, tümleşik güvenlik çözümlerini güvenlik açığı değerlendirmesi tarafından dahil [Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) ve [Rapid7](https://www.rapid7.com/products/insightvm/) ve Microsoft Application Gateway Web uygulaması güvenlik duvarı.

> [!NOTE]
> Güvenlik cihazı satıcılarının çoğu dış aracıların cihazlarında çalışmasını engellediğinden Güvenlik Merkezi, Microsoft Monitoring Agent uygulamasını iş ortağı sanal cihazlarına yüklemez.
>
>

## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. Şirket içi çalıştıran bilgisayarlar da dahil olmak üzere diğer güvenlik verisi kaynaklarına da bağlanabilir veya diğer bulutlarda.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

1. [Azure Portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

3. Güvenlik Merkezi menüsünde **Güvenlik çözümleri**’ni seçin.

   ![Güvenlik Merkezine Genel Bakış](./media/security-center-partner-integration/overview.png)

**Güvenlik çözümleri** altında tümleşik Azure güvenlik çözümlerinin sistem durumu hakkındaki bilgileri görüntüleyebilir ve temel yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca Common Event Format (CEF) biçimindeki Azure Active Directory Kimlik Koruması uyarılarını ve güvenlik duvarı günlükleri gibi diğer güvenlik veri kaynağı türlerini de bağlayabilirsiniz.

### <a name="connected-solutions"></a>Bağlantılı çözümler

**Bağlantılı çözümler** bölümünde Güvenlik Merkezi'ne bağlı olan güvenlik çözümleri ve her çözümün sistem durumu hakkında bilgiler yer alır.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Bir iş ortağı çözümünün durumunu olabilir:

* Sağlıklı (yeşil) - hiçbir sistem durumu sorunu yok.
* Sağlıksız (kırmızı) - sistem durumunda hemen ilgilenilmesi gereken bir sorun var.
* Sistem durumu sorunları (turuncu) - çözüm, sistem durumunu raporlamayı durdurdu.
* Bildirilmedi (gri) - çözüm herhangi bir şey bildirilmedi henüz yakın zamanda bağlanmış ve hala veya sistem veri yok, çözümün durumu bildirilmeyen olabilir.

> [!NOTE]
> Güvenlik Merkezi, sistem durumu verilerini kullanılabilir durumda değilse, çözüm veya rapor bildirmek üzere alınan son olayın saat ve tarihi gösterir. Son 14 gün içinde alınan hiçbir uyarı ve sistem durumu veri yok, Güvenlik Merkezi, sağlıksız veya değil raporlama çözümüdür gösterir.
>
>

1. Seçin **görünümü** ek bilgileri veya seçenekleri için içerir:

   - **Çözüm Konsolu**. Bu çözüm için yönetim deneyimi açar.
   - **Bağlantı VM**. Bağlantı uygulamalar dikey penceresi açılır. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   - **Çözümü Sil**.
   - **Yapılandırma**.

   ![İş ortağı çözümü ayrıntısı](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Bulunan çözümler

Güvenlik Merkezi, Azure’da çalışmasına karşın Güvenlik Merkezi’ne bağlı olmayan güvenlik çözümlerini otomatik olarak bulur ve çözümleri **Bulunan çözümler** bölümünde gösterir. Buna [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) gibi Azure çözümlerinin yanı sıra iş ortağı çözümleri dahildir.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. Güvenlik fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
>
>

Güvenlik Merkezi ile tümleştirmek ve güvenlik uyarılarını bildirim olarak almak üzere bir çözümün altında **BAĞLAN** öğesini seçin.

![Bulunan çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

Güvenlik Merkezi ayrıca Ortak Olay Biçimi (CEF) günlüklerini iletebilen, abonelikte dağıtılmış çözümleri bulur. CEF günlükleri kullanan [bir güvenlik çözümünü](quick-security-solutions.md) Güvenlik Merkezi'ne bağlama hakkında bilgi edinin.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>SIEM'ye aktarma verileri dışarı aktarma

Azure Güvenlik Merkezi tarafından üretilen işlenen olaylar, Azure'a yayımlanır [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), bir günlük türlerini Azure İzleyici aracılığıyla kullanılabilir. Azure İzleyici, tüm izleme verilerinizi bir SIEM aracına yönlendirme için birleştirilmiş bir işlem hattı sunar. Bu, bir iş ortağı aracına bu verileri Burada, ardından çekilebilir olay Hub'ına akış tarafından gerçekleştirilir.

Bu kanal kullanan [Azure izleme tek bir işlem hattı](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) erişim, Azure ortamınızdan izleme verilerini alma. Bu, kolayca veri tüketmek için Sıem'lerden ve izleme araçlarını ayarlama sağlar.

Sonraki bölümlerde, verileri olay hub'ına akışla nasıl yapılandırılacağını açıklar. Adımlar, Azure Güvenlik Merkezi, Azure aboneliğinizde yapılandırılmış zaten sahip olduğunuzu varsaymaktadır.

Yüksek düzey genel bakış

![Üst düzey genel bakış](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>SIEM için kullanıma sunulan Azure güvenlik veri nedir?

Bu sürümde kullanıma sunuyoruz [güvenlik uyarıları.](../security-center/security-center-managing-and-responding-alerts.md) Gelecek sürümlerde biz güvenlik önerilerini veri kümesiyle zenginleştirin.

### <a name="how-to-setup-the-pipeline"></a>İşlem hattı ayarlama

#### <a name="create-an-event-hub"></a>Event Hub'ı oluşturma

Başlamadan önce yapmanız [bir Event Hubs ad alanı oluşturma](../event-hubs/event-hubs-create.md). Bu ad alanı ve olay hub'ı olduğu için tüm izleme verilerinizi hedef.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Azure etkinlik günlüğünün Event Hubs'a Stream

Lütfen aşağıdaki makaleye başvurun [etkinlik günlüğünün Event Hubs'a akış](../azure-monitor/platform/activity-logs-stream-event-hubs.md)

#### <a name="install-a-partner-siem-connector"></a>Bir iş ortağı SIEM bağlayıcısı yükleyin 

Bir olay hub'ına Azure İzleyici ile izleme verilerinizi yönlendirme, iş ortağı SIEM ve izleme araçları ile kolayca tümleştirmenize olanak sağlar.

Listesini görmek için aşağıdaki bağlantıya bakın [desteklenen Sıem'lerin](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

### <a name="example-for-querying-data"></a>Örneğin, verileri Sorgulama 

Birkaç uyarı veri çekmek için kullanabileceğiniz Splunk sorgu aşağıda verilmiştir:

| **Sorgu açıklaması** | **Sorgu** |
|----|----|
| Tüm uyarılar| Dizin ana Microsoft.Security/locations/alerts =|
| İşlem sayısı adlarına göre özetleyin.| index=main sourcetype="amal:security" \| table operationName \| stats count by operationName|
| Uyarıları bilgi alın: Saat, adı, abonelik, durumu ve kimliği | Dizin ana Microsoft.Security/locations/alerts = \| tablo \_zaman, properties.eventName, durumu, properties.operationId am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
