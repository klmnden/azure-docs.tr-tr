---
title: Birleştirilmiş uyarılar Azure İzleyicisi'nde | Microsoft Docs
description: Azure yönetmenize olanak birleştirilmiş uyarılar açıklaması uyarıları ve Azure Hizmetleri genelinde kuralları uyarır.
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: mamit,bwren
ms.custom: ''
ms.openlocfilehash: 699dff42846ee1f9d42980feca55d8a79e2514e3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839852"
---
# <a name="unified-alerts-in-azure-monitor"></a>Birleştirilmiş uyarılar Azure İzleyicisi

## <a name="overview"></a>Genel Bakış

> [!NOTE]
>  Yönetmenize olanak sağlayan yeni bir birleşik uyarı deneyim birden çok aboneliklerden uyarıları ve uyarı durumları ve akıllı grupları genel önizlemede şu anda kullanılabilir olduğu tanıtır. Bkz: [bu makalenin en son](#enhanced-unified-alerts-experience-public-preview) bu açıklamasını Gelişmiş deneyimi ve işlem için etkin.


Bu makalede Azure İzleyicisi'nde birleşik uyarı deneyimi açıklanır. [Önceki uyarı deneyimi](monitoring-overview-alerts.md) kullanılabilir **uyarıları (Klasik)** Azure İzleyici menü seçeneği. 

## <a name="features-of-the-unified-alert-experience"></a>Birleşik uyarı deneyim özellikleri

Birleşik deneyim Klasik deneyimi aşağıdaki faydaları vardır:

-   **Daha iyi bildirim sistemi**: uyarıları kullanım birleşik [Eylem grupları]( https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups), bildirimler ve birden çok uyarıları yeniden kullanılabilir eylemler grupları denir. 
- **Yazma deneyimini birleşik** - uyarıları yönetebilir ve ölçümleri, uyarı kuralları günlüğe kaydeder ve etkinlik oturum Azure monitör, günlük analizi ve tek bir yerde Application Insights arasında. 
- **Görünüm harekete Azure Portal'da günlük analizi uyarılarını** -Azure portalında günlük analizi uyarılarla diğer kaynaklardan diğer uyarıları görüntüleyin. Daha önce bu ayrı bir portal yoktu.
- **Fired uyarıları ve uyarı kuralları ayrılması** -kuralları şimdi uyarılardan ayırt edilen uyar. Bir uyarı kuralı, bir uyarı tetikleyen bir koşul tanımlarını ' dir. Bir uyarı, bir uyarı kuralı tetikleme örneğidir.
- **Daha iyi iş akışı** -yazma deneyimini birleşik uyarı bir uyarı kuralı yapılandırma işleminde size rehberlik eder.
 
Ölçüm uyarıların Klasik ölçüm uyarıları aşağıdaki geliştirmeleri vardır:

-   **Geliştirilmiş gecikme**: ölçüm uyarıları dakikada bir kadar sık çalıştırılabilir. Klasik ölçüm uyarıları olan 5 dakikada bir sıklığında her zaman çalışır. Günlük uyarıların hala süre nedeniyle bir dakika sürer günlüklerini alma daha uzun bir gecikme vardır. 
-   **Çok boyutlu ölçümleri desteği**: ölçüm belirli bir örneği izlemenizi sağlayan boyutlu ölçülerine uyarabilir.
-   **Ölçüm koşullar hakkında daha fazla denetime**: ölçümleri maksimum, minimum, ortalama ve toplam değerler izleme desteği daha zengin uyarı kurallarını tanımlayabilirsiniz.
-   **Birden çok ölçümlerini izleme birleştirilmiş**: tek bir kural ile en fazla iki ölçümleri izleyebilirsiniz. Her iki ölçümleri, belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal varsa bir uyarı tetiklenir.
-   **Ölçümleri günlüklerinden** (sınırlı genel Önizleme): günlük analizi giderek veri için şimdi bazı günlük ayıklanır ve Azure İzleyici ölçümleri dönüştürülür ve ardından yalnızca diğer ölçümleri gibi uyarı. 


## <a name="alert-rules"></a>Uyarı kuralları
Birleştirilmiş uyarılar deneyimi aşağıdaki kavramlar geliştirme deneyimi farklı uyarı türleri arasında birleştirin sırasında uyarı kuralları uyarılardan ayırmak için kullanır.

| Öğe | Tanım |
|:---|:---|
| Uyarı kuralı | Bir uyarı oluşturmak için koşul tanımı. Oluşan bir _hedef kaynak_, _sinyal_, _ölçütleri_, ve _mantığı_. Bir uyarı kuralı yalnızca içinde durumunda etkin bir _etkin_ durumu.
| Hedef Kaynak | Belirli kaynakları tanımlar ve uyarmak için kullanılabilir işaret eder. Bir hedef herhangi bir Azure kaynağı olabilir.<br>Örnekler: sanal makine, depolama hesabı, sanal makine ölçek kümesi, günlük analizi çalışma alanı, Application Insights kaynağı |
| Sinyal | Hedef kaynak tarafından yayılan veriler kaynağı. Desteklenen sinyal türleri *ölçüm*, *etkinlik günlüğü*, *Application Insights*, ve *günlük*. |
| Ölçütler | Birleşimi _sinyal_ ve _mantığı_ hedef kaynak üzerindeki uygulanır.<br>Örnekler: Yüzdesi % CPU > 70, sunucu yanıt süresi > 4 ms, sonuç sayısı günlük sorgu > 100 vs. |
| Mantığı | Kullanıcı tanımlı mantık sinyal içinde olup olmadığını denetlemek için aralığı/değer bekleniyor. |
| Eylem | Uyarı başlatıldığında gerçekleştirilecek eylem. Bir uyarı oluşturulduğunda birden çok eylem ortaya çıkabilir. Bu uyarılar eylem gruplarını destekler.<br>Örnekler: bir Web kancası URL'si çağrılırken bir e-posta adresi, e-postayla gönderme. |
| İzleme Koşulu | Ölçüm uyarı oluşturulan koşul sonradan giderilmiş olup olmadığını gösterir. Ölçüm uyarı kuralları belirli bir ölçüm düzenli aralıklarla örnek. Uyarı kuralı ölçütler karşılanırsa, yeni bir uyarı Fired bir koşul ile oluşturulur.  Zaman ölçütleri hala karşılanırsa hiçbir şey olmaz sonra ölçüm yeniden örneklenen.  Ölçütleri karşılanmadığında rağmen uyarının koşulu çözümlendi olarak değiştirilir. Ölçütler karşılanıyorsa, sonra başka bir uyarı Fired bir koşul ile oluşturulan sonraki süre. |


## <a name="alert-pages"></a>Uyarı sayfaları
Birleştirilmiş uyarılar tüm Azure uyarıları görüntülemek ve yönetmek için tek bir konum sağlar. Aşağıdaki bölümlerde birleşik deneyim tek tek her sayfanın işlevleri açıklanmaktadır.

### <a name="alerts-overview-page"></a>Uyarılar genel bakış sayfası
**Uyarıları** genel bakış sayfasında bir toplanmış tüm Mazotlu uyarıların özetini gösterir ve uyarı kuralları toplam etkin. Abonelikler ve filtre parametrelerini değiştirme toplamalar güncelleştirir ve liste uyarıları tetiklenir.

 ![Uyarılar genel bakış](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Uyarı kuralları Yönetimi
**Kuralları** tüm uyarı kuralları, Azure Aboneliklerini yönetmek için tek bir sayfadır. Tüm Uyarı kurallarını listeler ve hedef kaynak, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları da etkin veya devre dışı bu sayfadan düzenlenen nd olabilir.

 ![Uyarı kuralları](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="creating-an-alert-rule"></a>Bir uyarı kuralı oluşturma
Uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya türü sinyal. Tüm uyarıları tetiklenir ve ilgili ayrıntıları tek sayfa olarak kullanılabilir.
 
Aşağıdaki üç adımı ile yeni bir uyarı kuralı oluşturun:
1. Çekme _hedef_ uyarı.
1. Seçin _sinyal_ hedefi için kullanılabilir sinyalleri gelen.
1. Belirtin _mantığı_ sinyalden verilere uygulanacak.
 
Bu Basitleştirilmiş yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri öğrenmek kullanıcının gerektirir. Kullanılabilir sinyal listesi otomatik olarak seçilen hedef kaynak üzerindeki göre filtrelenir ve uyarı kuralı mantığını tanımlayan size rehberlik eder.

Uyarı kuralları oluşturma hakkında daha fazla bilgi edinebilirsiniz [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](monitor-alerts-unified-usage.md).

Uyarıları hizmetlerini izleme birkaç Azure kullanılabilir. Hakkında bilgi ve ne zaman bu hizmetlerin her birini kullanmak için bkz: [izleme Azure uygulamaları ve kaynakları](./monitoring-overview.md). Aşağıdaki tabloda, Azure ve birleşik uyarı deneyim tarafından şu anda desteklenen üzerinden uyarı kuralları kullanılabilir türleri listesini sağlar.

| **Kaynağı izle** | **Sinyal türü**  | **Açıklama** | 
|-------------|----------------|-------------|
| Azure İzleyicisi | Ölçüm  | Olarak da bilinir [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md), ölçüm koşulları 1 dakika olarak sık değerlendiriliyor destekler ve birden çok ölçüm ve çok boyutlu ölçüm kuralları için izin. Desteklenen kaynak türleri listesi kullanılabilir [Azure portalında Azure Hizmetleri için yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Klasik ölçüm uyarıları](monitoring-overview-alerts.md) yeni uyarılar deneyimi desteklenmez. Bunları Azure portalında uyarılar (Klasik) altında bulabilirsiniz. Klasik uyarıları henüz yeni uyarılar taşınmamış olan bazı ölçümleri türleri desteklemez. Tam listesi için bkz: [ölçümleri desteklenen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Günlükler  | Bildirimleri almak veya bir günlük arama sorgusu belirli ölçütleri karşıladığında otomatik eylemler çalıştırın. Günlük analizi uyarıların [yeni deneyim halinde kopyalanmasını](monitoring-alerts-extend.md). A [, Önizleme *günlük analizi günlüklerini ölçümleri* ](monitoring-alerts-extend-tool.md) kullanılabilir. Önizleme günlükleri bazı türleri almak ve bunları burada, ardından yeni bir uyarı deneyim kullanarak üzerlerinde uyarabilir ölçümlere dönüştürmek sağlar. Önizleme yanı sıra yerel Azure İzleyici ölçümleri almak istediğiniz Azure olmayan günlükleri varsa faydalıdır. |
| Etkinlik Günlükleri | Etkinlik Günlüğü | Seçili hedef tarafından oluşturulan tüm oluşturma, güncelleştirme ve silme eylemlerini kayıtları içerir. |
| Hizmet Durumu | Etkinlik Günlüğü  | Birleştirilmiş uyarılar desteklenmiyor. Bkz: [etkinlik günlüğü uyarı hizmeti bildirimlerinin oluşturma](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Günlükler  | Uygulamanızın performansı ayrıntılarla günlükleri içerir. Analytics sorgu kullanarak uygulama verilerini tabanlı gerçekleştirilecek eylemler için koşullar tanımlayabilirsiniz. |
| Application Insights | Ölçüm | Birleştirilmiş uyarılar desteklenmiyor. [Ölçüm uyarılar] bakın. (.. /Application-insights/App-insights-Alerts.MD) |
| Application Insights | Web kullanılabilirlik testleri | Birleştirilmiş uyarılar desteklenmiyor.  Bkz: [Web testi uyarıları](../application-insights/app-insights-monitor-web-app-availability.md). Application Insights'a veri göndermek için izleme eklenmiş tüm Web sitesi için kullanılabilir. Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentilerini altında olduğunda bir bildirim alırsınız. |

## <a name="enhanced-unified-alerts-public-preview"></a>Gelişmiş birleşik uyarıları (genel Önizleme)
> [!NOTE]
>  Bu bölümdeki işlevselliği yakında çıkıyor. Her portal sürümünüzde henüz görünmeyebilir. 

Gelişmiş birleşik uyarıları deneyimi 1 Haziran 2018 üzerinde Azure izleyicisinin genel önizlemede yayımlanmıştır. Bu deneyim yararları derlemeler [uyarıları birleşik](#overview) Mart 2018 serbest ve yönetmek ve uyarı durumu değiştirme ek olarak uyarıları ayrı ayrı bir araya olanağı sağlar. Bu bölümde, yeni özellikleri ve Azure portalında yeni uyarı sayfalarında gezinmek nasıl açıklanmaktadır.

### <a name="features-enhanced-unified-alerts"></a>Gelişmiş Özellikler uyarıları birleşik

Yeni deneyimi Klasik birleşik deneyimi bulunmayan aşağıdaki özellikleri sağlar:

- **Abonelikler arasında uyarıları görüntülemek** -artık görüntülemek ve tek bir görünümde birden çok Aboneliklerdeki uyarıları tek tek örneklerini yönetebilirsiniz.
- **Uyarıları durumunu yöneten** -uyarıları şimdi belirten bir duruma sahip olup olmadığını kendi edilmiş kapalı için onaylandı.
- **Akıllı grupları uyarılarla düzenlemek** -akıllı grupları otomatik olarak Grup birlikte ilgili uyarılar yerine tek tek bir küme olarak yönetilebilir.

### <a name="enable-enhanced-unified-alerts"></a>Gelişmiş birleşik uyarılarını etkinleştir
Sayfanın üst kısmındaki uyarıları başlıktaki tıklatarak yeni birleşik uyarı deneyim etkinleştirin. Bu işlem Mazotlu uyarıların son 30 gün desteklenen hizmetleri içeren bir uyarı deposu oluşturur. Yeni deneyimi etkinleştirildikten sonra geri ve İleri başlığında tıklatarak yeni ve eski bir deneyim arasında geçiş yapabilirsiniz.

> [!NOTE]
>  Başlangıçta etkinleştirilmesi için yeni deneyim için birkaç dakika sürebilir.

![Başlık](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Yeni deneyimi etkinleştirdiğinizde, erişiminiz olan tüm abonelikler kaydedeceksiniz. Tüm abonelik etkin olsa da, yalnızca yeni deneyimi Seçili kullanıcıları görüntüleyin kuramaz. Abonelik erişimi olan diğer kullanıcıların deneyimini tek tek etkinleştirmelisiniz.

Yeni uyarı deneyimi etkinleştirme Eylem grupları veya uyarı kurallarınızı bildirimleri yapılandırması etkilemez. Yalnızca, görüntülemek ve Azure portalında uyarılar Mazotlu örneklerini yönetmek şeklini değiştirir.

### <a name="smart-groups"></a>Akıllı grupları
Akıllı grupları, ilgili uyarıları yönetme bireysel uyarıları yerine tek bir birim olarak yönetmenize olanak tanıyarak gürültü azaltma. Akıllı grupları ayrıntılarını görüntülemek ve durumunu benzer bir uyarı şekilde ayarlayın. Her uyarı, tek bir akıllı grubunun üyesidir.

Akıllı grupları tek bir sorunu temsil eden ilgili uyarıları birleştirmek öğrenme makine kullanarak otomatik olarak oluşturulur. Bir uyarı oluşturulduğunda, algoritma yeni bir akıllı grubu veya geçmiş desenleri, benzerlik özelliklerinin ve benzerlik yapısının gibi bilgileri temel alan var olan bir akıllı grup ekler. Şu anda algoritması yalnızca bir abonelik içindeki aynı İzleyici hizmeti uyarıları dikkate alır. Akıllı grupları aralığını belirterek uyarı sesini bu birleştirme aracılığıyla % 99'ye kadar azaltabilirsiniz. Uyarıları akıllı grubu ayrıntı sayfası grubunda bulunan neden görüntüleyebilirsiniz.

Akıllı grubunun adı, ilk uyarı adıdır. Oluşturamaz veya akıllı grubunu yeniden adlandırın.


### <a name="alert-states"></a>Uyarı durumları
Gelişmiş birleştirilmiş uyarılar uyarı durumu kavramını tanıtır. Çözümleme işlemine olduğu belirtmek için bir uyarı durumu ayarlayabilirsiniz.  Bir uyarı oluşturulduğunda, durumuna sahip *yeni*. Bir uyarı ve ne zaman kapattığınız Onaylandı zaman durumunu değiştirebilirsiniz. Durum değişiklikleri uyarı geçmişinde depolanır.

Aşağıdaki uyarı durumları desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yalnızca bırakıldı algıladı ve henüz gözden geçirilmedi. |
| Onaylanan | Bir yönetici, uyarıyı gözden ve üzerinde çalışmaya başladı. |
| Kapatıldı | Sorunu Çözümlendi. Bir uyarı kapatıldıktan sonra my başka bir duruma değiştirmeden yeniden açabilirsiniz. |

Bir uyarının durumunu izleme koşulu farklıdır. Ölçüm uyarı kuralları bir koşulu için uyarı ayarlayabilirsiniz _çözülmüş_ olduğunda hata koşulu artık karşılanmıyor. Uyarı durumu kullanıcı tarafından ayarlanan ve izleme koşulu bağımsızdır. Sistem izleme koşulu çözülmüş ayarlayabilir olsa bile, kullanıcı bunu olana kadar uyarı durumu değiştirilmez.

#### <a name="changing-the-state-of-an-alert-or-smart-group"></a>Bir uyarı veya akıllı grubu durumunu değiştirme
Bir bireysel uyarı durumunu değiştirme veya akıllı bir grubun durumunu ayarlayarak birden çok uyarı birlikte yönetebilirsiniz.

Tıklayarak bir uyarı durumunu değiştirme **değişiklik uyarı durumu** ayrıntı görüntülemek için uyarı ya da değişiklik tıklayarak akıllı bir grup için durumu **değişiklik akıllı Grup durumu** Ayrıntılar görünümünde. Bir seferde birden çok öğe durumunu bir liste görünümü seçerek ve tıklatarak değiştirebilirsiniz **durum değiştirme** sayfanın üst kısmındaki. Her iki durumda da yeni bir durum aşağı açılır listeden seçin ve isteğe bağlı olarak bir açıklama sağlayın. Ardından tek bir öğe değiştiriyorsanız, ayrıca akıllı grubundaki tüm uyarılar aynı değişiklikleri uygulamak için bir seçeneğiniz vardır.

![Durumu değiştir](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Uyarılar sayfası
Varsayılan uyarılar sayfasında belirli zaman penceresi içinde oluşturulan uyarıların bir özetini sağlar. Her önem derecesi için her durumda uyarıların toplam sayısı tanımlayan sütunlarla her önem toplam uyarılarını görüntüler. Açmak için önem derecelerinin birini tıklatın [tüm uyarıları](#all-alerts-page) sayfa o önem derecesine göre filtrelenir.

![Uyarılar sayfası](media/monitoring-overview-unified-alerts/alerts-page.png)

Bu görünümde, sayfanın üst kısmındaki bırakmalar değerleri seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | En fazla 5 Azure aboneliklerini seçin. Yalnızca Seçili abonelikleri uyarılar görünümünde dahil edilir. |
| Kaynak Grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunda hedefler uyarıları görünümünde dahil edilir. |
| Zaman Aralığı | Yalnızca seçilen zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Saati aşan, son 24 saat, son 7 gün ve son 30 gün değerleri desteklenir. |

Aşağıdaki değerleri sayfanın üst kısmındaki başka bir sayfasını açmak için Uyarılar'ı tıklatın.

| Değer | Açıklama |
|:---|:---|
| Toplam Uyarı Sayısı | Seçilen ölçütlerle eşleşen uyarılar toplam sayısı. Bu değer sahip hiç filtre tüm uyarılar görünümü açmak için tıklatın. |
| Akıllı grupları | Seçilen ölçütlerle eşleşen uyarılar oluşturulan akıllı gruplarını toplam sayısı. Bu değer tüm uyarılar görünümünde akıllı grupları listesini açmak için tıklatın.
| Toplam Uyarı Kuralı | Uyarı kurallarını seçili abonelik ve kaynak grubunda toplam sayısı. Bu değer seçilen abonelikleri ve kaynak grubu üzerinde filtre kuralları görünümü açmak için tıklatın.


### <a name="all-alerts-page"></a>Tüm uyarılar sayfası 
Tüm uyarılar sayfasında, seçilen zaman penceresi içinde oluşturulan uyarıların bir listesi görüntülemenizi sağlar. Tek tek uyarıların bir listesi veya uyarıları içeren Akıllı gruplarının bir listesini görüntüleyebilirsiniz. Görünümleri arasında geçiş yapmak için sayfanın üstündeki başlığını tıklatın.

![Tüm uyarılar sayfası](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Sayfanın üstündeki bırakmalar aşağıdaki değerleri seçerek görünümünü filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | En fazla 5 Azure aboneliklerini seçin. Yalnızca Seçili abonelikleri uyarılar görünümünde dahil edilir. |
| Kaynak Grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunda hedefler uyarıları görünümünde dahil edilir. |
| Kaynak Türü | Bir veya daha fazla kaynak türü seçin. Yalnızca seçilen tür hedefler uyarıları görünümünde dahil edilir. Bu sütun, yalnızca bir kaynak grubu belirtilen bir kez kullanılabilir. |
| Kaynak | Bir kaynak seçin. Hedef olarak bu kaynaklarla yalnızca uyarılar görünümünde dahil edilir. Bu sütun, yalnızca bir kaynak türü belirtilen bir kez kullanılabilir. |
| Severity | Uyarı önem veya seçin *tüm* tüm önem dereceleridir uyarıları eklenecek. |
| İzleme Koşulu | Bir izleme koşulu veya seçin *tüm* koşulları uyarılar eklemek için. |
| Uyarı Durumu | Durum veya select bir uyarı seçin *tüm* durumlardan uyarıları eklenecek. |
| Hizmeti İzle | Bir hizmet veya seçin *tüm* tüm hizmetleri dahil etmek için. Bu hizmetin hedef olarak kullanan kurallar tarafından oluşturulan uyarıların dahil edilir. |
| Zaman Aralığı | Yalnızca seçilen zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Saati aşan, son 24 saat, son 7 gün ve son 30 gün değerleri desteklenir. |

Tıklatın **sütunları** sayfanın üst kısmındaki hangi sütunların görüntüleneceğini seçin. Dışında herhangi bir sütunu kaldırmak için 

### <a name="alert-detail-page"></a>Uyarı Ayrıntı Sayfası
Bir uyarıya tıklayın uyarı ayrıntı sayfası görüntülenir. Uyarı ayrıntılarını sağlar ve durumuna değiştirmenize izin verir.

![Uyarı ayrıntısı](media/monitoring-overview-unified-alerts/alert-detail.png)

Uyarı ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Temel Bileşenler | Özelliklerini ve diğer uyarı hakkında önemli bilgileri görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylem ve uyarıya yapılan değişiklikler listelenmektedir. Bu, şu anda durumu değişiklikleri sınırlıdır. |
| Akıllı grubu | Akıllı grubu uyarı hakkındaki bilgiler dahil edilir. **Uyarıların sayısını** akıllı gruba dahil uyarıların sayısını ifade eder. Bu, son 30 gün içinde oluşturulan aynı aynı akıllı grubunda yer alan diğer uyarıları içerir.  Zaman filtresi uyarıları listesi sayfasındaki bakılmaksızın budur. Kendi ayrıntı görüntülemek için bir uyarıyı tıklatın. |
| Daha fazla ayrıntı | Daha fazla uyarı oluşturulan kaynak türü için genellikle belirli olan uyarı bağlamsal bilgi görüntüler. |


### <a name="smart-group-detail-page"></a>Akıllı grubu Ayrıntı Sayfası
Akıllı bir gruba tıkladığınızda akıllı grubu ayrıntı sayfası görüntülenir. Grubu oluşturmak için kullanılan mantığı dahil olmak üzere akıllı grubunun ayrıntılarını sağlar ve durumuna değiştirmenize izin verir.
 
![Akıllı grubu ayrıntısı](media/monitoring-overview-unified-alerts/smart-group-detail.png)


Akıllı grubu ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Uyarılar | Akıllı gruba dahil bireysel uyarıları listeler. Uyarı ayrıntı sayfasını açmak için bir uyarıyı tıklatın. |
| Geçmiş | Akıllı grubu tarafından gerçekleştirilen her eylem ve üzerinde yapılan değişiklikleri listeler. Durum değişikliklerini ve uyarı üyelik değişiklikleri şu anda sınırlı budur. |

## <a name="next-steps"></a>Sonraki adımlar
- [Yeni uyarılar deneyimi oluşturun, görüntüleyin ve Uyarıları yönetmek için nasıl kullanılacağını öğrenin](monitor-alerts-unified-usage.md)
- [Uyarıları deneyiminde günlük uyarılar hakkında bilgi edinin](monitor-alerts-unified-log.md)
- [Uyarıları deneyiminde ölçüm uyarılar hakkında bilgi edinin](monitoring-near-real-time-metric-alerts.md)
- [Etkinlik günlüğü uyarıları uyarılar deneyiminde hakkında bilgi edinin](monitoring-activity-log-alerts-new-experience.md)
