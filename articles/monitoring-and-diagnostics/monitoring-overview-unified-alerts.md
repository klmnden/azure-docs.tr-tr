---
title: Birleştirilmiş uyarılar Azure İzleyici'de
description: Azure'da yönetmenize olanak tanıyan birleşik uyarı açıklaması, uyarılar ve Azure Hizmetleri genelinde kuralları uyarır.
author: manishsm-msft
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: mamit
ms.component: alerts
ms.openlocfilehash: c4c8279a1d4638a1c5d889b53e2d9e89e458cc37
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117619"
---
# <a name="unified-alerts-in-azure-monitor"></a>Birleştirilmiş uyarılar Azure İzleyici'de

## <a name="overview"></a>Genel Bakış

> [!NOTE]
>  Yönetmenize olanak sağlayan yeni bir birleşik uyarı deneyimi, birden çok aboneliklerden uyarılar ve uyarı durumları ve akıllı gruplar şu anda genel önizlemede kullanıma sunar. Bu bir açıklaması için bu makalenin son bölümüne bakın, deneyimi ve bunu etkinleştirmek için işlem iyileştirdik.


Bu makalede, Azure İzleyici uyarı birleşik deneyim açıklanmaktadır. [Önceki uyarı deneyimi](monitoring-overview-alerts.md) kullanılabilir **uyarılar (Klasik)** Azure İzleyici'menü seçeneği. 

## <a name="features-of-the-unified-alert-experience"></a>Birleştirilmiş uyarı deneyimi özellikleri

Birleşik bir deneyim sahip Klasik deneyim aşağıdaki faydaları vardır:

-   **Daha iyi bir bildirim sistemi**: [Eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) bildirimleri ve Uyarıları birden çok yeniden kullanılabilir eylemler grupları olarak adlandırılır. 
- **Birleşik yazma deneyimini**: uyarılar ve ölçümler, günlükler ve etkinlik günlüklerini Azure İzleyici, Log Analytics ve Application Insights uyarı kuralları, tek bir yerde yönetilebilir. 
- **Görünüm, Azure portalında Log Analytics uyarılarını harekete**: Log Analytics uyarılarını artık görüntülenebilir uyarılar Azure portalında diğer kaynaklardan. Daha önce diğer kaynaklardan alınan uyarıları ayrı bir portalda bulunuyordu.
- **Tetiklenen uyarılar ve uyarı kuralları ayrımı**: uyarı kuralları artık uyarılardan ayırt edici. Bir uyarı kuralı uyarıyı tetikleyen bir koşul tanımıdır. Bir uyarı, bir uyarı kuralı Açmadığınızda örneğidir.
- **Daha iyi iş akışı**: yazma deneyimini birleşik uyarı bir uyarı kuralı yapılandırma işleminde size kılavuzluk eder.
 
Ölçüm uyarıları Klasik ölçüm uyarılarını aşağıdaki geliştirmeleri vardır:

-   **Geliştirilmiş gecikme**: ölçüm uyarıları dakikada olabildiğince sık çalıştırılabilir. Klasik ölçüm uyarıları, her 5 dakikada bir sıklığında her zaman çalışır. Günlük uyarıları hala günlükleri alma süresini nedeniyle bir dakikadan uzun bir gecikme vardır. 
-   **Çok boyutlu ölçümler için destek**: belirli bir ölçüm örneğini izleyebilirsiniz anlamına gelir boyutlu ölçümler sizi uyarabilir.
-   **Ölçüm koşullar hakkında daha fazla denetime**: ölçüm maksimum, minimum, ortalama ve toplam değerler izlemeyi desteklemek daha zengin bir uyarı kuralları tanımlayabilirsiniz.
-   **Birden çok ölçümlerini izleme birleştirilmiş**: en fazla iki ölçümleri tek bir kural ile izleyebilirsiniz. Her iki ölçüm, belirtilen zaman aralığı için ilgili kendi eşiklerini ihlal etmeniz durumunda bir uyarı tetiklenir.
-   **Ölçümleri günlüklerinden** (sınırlı genel Önizleme): bazı günlük verileri Log Analytics'e gidip artık ayıklanır ve Azure İzleyici ölçümleri dönüştürülür ve ardından diğer ölçümler gibi uyarı. 


## <a name="alert-rules"></a>Uyarı kuralları
Birleştirilmiş uyarılar deneyiminin aşağıdaki kavramları yazma deneyiminde farklı uyarı türleri arasında birleştirme sırasında uyarılardan uyarı kuralları ayırmak için kullanır.

| Öğe | Tanım |
|:---|:---|
| Uyarı kuralı | Bir uyarı oluşturmak için koşul tanımı. Bir uyarı kuralı oluşan bir _hedef kaynak_, _sinyal_, _ölçütleri_, ve _mantıksal_. Bir uyarı kuralı yalnızca içinde etkin bir _etkin_ durumu.
| Hedef kaynak | Belirli kaynaklar ve uyarı verme için kullanılabilir sinyaller tanımlar. Bir hedef herhangi bir Azure kaynak olabilir.<br><br>Örnekler: sanal makine, depolama hesabı, sanal makine ölçek kümesi, Log Analytics çalışma alanı, Application Insights kaynağı |
| Sinyal | Hedef kaynak tarafından yayılan veri kaynağı. Desteklenen sinyal türleri *ölçüm*, *etkinlik günlüğü*, *Application Insights*, ve *günlük*. |
| Ölçütler | Birleşimi _sinyal_ ve _mantıksal_ bir hedef kaynak üzerindeki uygulanır.<br><br>Örnekler: Yüzde 70 > % CPU, sunucu yanıt süresi > 4 ms, günlük sonuç sayısı > 100 sorgu vb. |
| Mantığı | Sinyal içinde olduğunu doğrulamak için kullanıcı tanımlı mantık aralığı/değerler bekleniyor. |
| Eylem | Uyarı tetiklendiğinde gerçekleştirilecek eylem. Bir uyarı tetiklendiğinde birden fazla eylem ortaya çıkabilir. Bu uyarılar eylem gruplarını destekler.<br><br>Örnekler: e-posta adresi, bir webhook URL'sine çağrı e-postayla gönderme |
| İzleme koşulu | Ölçüm uyarısı oluşturulan koşul çözümlenmiş olup olmadığını gösterir. Ölçüm uyarı kuralları belirli bir ölçüm düzenli aralıklarla örnek. Ardından uyarı kuralını ölçütü karşılanırsa "gönderildi." ile ilgili bir koşul yeni bir uyarı oluşturulur  Ölçütler karşılanıyorsa yine de, ölçüm yeniden örneklenir, hiçbir şey olmaz.  Ölçütleri karşılanmadığında, ardından uyarının koşulu olarak "Çözüldü" değiştirilir Ölçütler karşılanıyorsa, sonraki açışınızda "gönderildi." ile ilgili bir koşul başka bir uyarı oluşturulur |


## <a name="alert-pages"></a>Uyarı sayfaları
Birleştirilmiş uyarılar, tüm Azure uyarıları görüntülemek ve yönetmek için tek bir yer sağlar. Aşağıdaki bölümlerde, her bir sayfaya birleşik deneyim işlevlerini açıklanmaktadır.

### <a name="alerts-overview-page"></a>Uyarılar genel bakış sayfası
**Uyarılar** genel bakış sayfasında tüm tetiklenen uyarılar toplu bir özeti gösterilir ve toplam etkin uyarı kuralları. Toplamlar abonelikleri veya filtre parametrelerini değiştirme güncelleştirir ve uyarıların listesi tetiklendi.

 ![uyarılara genel bakış](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Uyarı kuralları Yönetimi
**Kuralları** Azure aboneliklerinizde uyarı kurallarının tümünü yönetmek için tek bir sayfadır. Bu, uyarı kurallarının tümünü listeler ve hedef kaynaklar, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları ayrıca düzenlendi, etkin veya bu sayfadan devre dışı.

 ![Uyarı kuralları](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya tür sinyal. Tüm uyarıları harekete ve ilgili ayrıntıları tek sayfasında kullanılabilir.
 
Aşağıdaki üç adımı ile yeni bir uyarı kuralı oluşturun:
1. Çekme _hedef_ uyarı.
1. Seçin _sinyal_ öğesinden hedef için kullanılabilir sinyaller.
1. Belirtin _mantıksal_ verilere sinyalden uygulanacak.
 
Bu basitleştirilmiş bir yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri bilmesini gerektirmez. Kullanılabilir sinyaller listesi otomatik olarak seçtiğiniz hedef kaynak göre filtrelenir ve uyarı kuralı mantığını tanımlama aracılığıyla yol gösterir.

Uyarı kuralları oluşturma hakkında daha fazla bilgi [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](monitor-alerts-unified-usage.md).

Uyarılar, çeşitli Azure izleme hizmetleri arasında kullanılabilir. Hakkında bilgi ve bu hizmetlerin her biri kullanıldığı durumlar için bkz: [izleme Azure uygulamalarını ve kaynaklarını](./monitoring-overview.md). Aşağıdaki tabloda, Azure genelinde kullanılabilir olan uyarı kuralları türlerinin bir listesini sağlar. Ayrıca, birleşik uyarı deneyimi tarafından desteklenen özellikler listelenir.

| **Kaynak İzleyicisi** | **Sinyal türü**  | **Açıklama** | 
|-------------|----------------|-------------|
| Azure İzleyici | Ölçüm  | Olarak da adlandırılan [neredeyse gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md), ölçüm koşulları genellikle bir dakika olarak değerlendiriliyor destekler ve birden çok ölçüm ve çok boyutlu ölçüm kuralları için izin verir. Desteklenen kaynak türleri listesi kullanılabilir [Azure portalında Azure Hizmetleri için yeni ölçüm uyarılarının](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>[Klasik ölçüm uyarıları](monitoring-overview-alerts.md) yeni uyarılar deneyiminde desteklenmez. Bunları Azure portalında uyarılar (Klasik) bulabilirsiniz. Klasik uyarılar henüz yeni uyarılar için taşınmamış olan bazı ölçüm türlerini destekler. Tam bir listesi için bkz [ölçümleri desteklenen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Günlükler  | Bildirimleri almak veya otomatik eylemleri günlük arama sorgusu belirli ölçütleri karşıladığında Çalıştır. Log analytics'teki uyarılar [yeni deneyime kopyalanan](monitoring-alerts-extend.md). A [önizlemesi *Log Analytics günlükleri ölçümler olarak* ](monitoring-alerts-extend-tool.md) kullanılabilir. Önizleme, günlükleri belirli türden olması ve onları burada, ardından bunlar üzerinde yeni uyarı deneyimi kullanarak uyarabilir ölçümlere dönüştürmek sağlar. Önizleme ile birlikte yerel Azure İzleyici ölçümleri almak istediğiniz Azure dışı günlükleri varsa yararlı olur. |
| Etkinlik günlükleri | Etkinlik günlüğü | İçeren tüm kayıtları oluşturmak, güncelleştirmek ve seçilen hedef tarafından oluşturulan eylemler silin. |
| Hizmet durumu | Etkinlik günlüğü  | Birleştirilmiş uyarılar desteklenmiyor. Bkz: [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Günlükler  | Uygulamanızın performans ayrıntıları ile günlükleri içerir. Analytics sorgusunu kullanarak uygulama verilerini göre gerçekleştirilecek eylemler için koşullar tanımlayabilirsiniz. |
| Application Insights | Ölçüm | Birleştirilmiş uyarılar desteklenmiyor. Bkz: [ölçüm uyarıları](../application-insights/app-insights-alerts.md). |
| Application Insights | Web kullanılabilirlik testleri | Birleştirilmiş uyarılar desteklenmiyor.  Bkz: [Web test Uyarısı](../application-insights/app-insights-monitor-web-app-availability.md). Application Insights'a veri göndermek için izleme eklenmiş olan tüm Web sitelerinin kullanılabilir. Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentileri altında olduğunda bir bildirim alırsınız. |

## <a name="enhanced-unified-alerts-public-preview"></a>Gelişmiş birleşik uyarıları (genel Önizleme)

1 Haziran 2018'de genel önizlemede Azure İzleyici için bir birleşik gelişmiş uyarı deneyimi yayınlanmıştır. Bu deneyim avantajlarını yapılar [uyarılar birleşik](#overview)Mart 2018'de sunulan ve yönetmek ve belirli uyarıları toplamak ve uyarı durumunu değiştirmek için olanağı sağlar. Bu bölümde, yeni özellikler ve Azure portalında yeni uyarı sayfalarında gezinmek nasıl açıklanmaktadır.

### <a name="enhanced-unified-alerts"></a>Birleştirilmiş uyarılar Gelişmiş

Yeni deneyimi Klasik birleşik deneyim kullanılamayan aşağıdaki özellikleri sağlar:

- **Abonelikler arasında uyarıları görüntüleyip**: şimdi görüntüleyin ve Uyarıları tek tek örneklerini tek bir görünümde birden çok aboneliğe yönetin.
- **Uyarıların durumunu yöneten**: uyarıları artık bunlar kapalı olarak onaylanır olup olmadığını belirten bir durum vardır.
- **Akıllı gruplarıyla uyarılar düzenlemek**: Akıllı grupları otomatik olarak Grup birlikte ilgili uyarıları yerine tek tek bir küme olarak yönetebilirsiniz.

### <a name="enable-enhanced-unified-alerts"></a>Gelişmiş birleşik uyarıları etkinleştir
Birleştirilmiş uyarılar deneyiminin yeni uyarılar sayfasında üst taraftaki başlığa seçerek etkinleştirin. Bu işlem, tetiklenen uyarılar, son 30 gün desteklenen hizmetler içeren bir uyarı deposu oluşturur. Yeni deneyime etkinleştirildikten sonra bu başlığını seçerek yeni ve eski bir deneyim arasında ileri ve geri geçiş yapabilirsiniz.

> [!NOTE]
>  Bu, yeni deneyimi başlangıçta etkinleştirilmesi için birkaç dakika sürebilir.

![Başlığı](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Yeni deneyimi etkinleştirdiğinizde, erişiminiz olan tüm abonelikler kaydedilir. Aboneliğin tümü etkindir; ancak, yeni deneyimi seçen kullanıcılar raporu görüntüleyebilirsiniz. Aboneliğe erişim diğer kullanıcılarla deneyimi ayrı olarak etkinleştirmeniz gerekir.

Yeni uyarı deneyimini Eylem grupları veya uyarı kurallarınızı Bildirimlerde yapılandırmasını etkilemez. Yalnızca görüntülemek ve Azure portalında uyarıları tetiklenme örneklerini yönetme şeklini değiştirir.

### <a name="smart-groups"></a>Akıllı grupları
Akıllı grupları, ilgili uyarıları tek bir birim yerine uyarıları ayrı ayrı olarak yönetmenize olanak tanıyarak Gürültüyü azaltma. Akıllı grupları ayrıntılarını görüntülemek ve Uyarılar için nasıl durumu benzer şekilde ayarlayın. Her uyarı, bir ve yalnızca bir akıllı grubunun bir üyesidir.

Tek bir sorunu temsil eden ilgili uyarıları birleştirmek için makine öğrenimini kullanarak akıllı grupları otomatik olarak oluşturulur. Bir uyarı oluşturulduğunda, algoritma yeni bir akıllı grubu veya benzer yapıya geçmiş desenleri ve benzer özellikleri gibi bilgileri temel alarak varolan akıllı grubuna ekler. 

Şu anda algoritması yalnızca bir abonelik içindeki aynı İzleyici hizmeti uyarıları dikkate alır. Akıllı grupları aracılığıyla bu birleştirme uyarı gürültüsünü en fazla %99 azaltabilir. Uyarılar, akıllı Grup ayrıntı sayfası grubunda bulunan nedeni görüntüleyebilirsiniz.

Akıllı bir grubun adı, ilk uyarı adıdır. Oluşturamaz veya akıllı bir grubu yeniden adlandırın.


### <a name="alert-states"></a>Uyarı durumları
Gelişmiş birleştirilmiş uyarılar, uyarı durumu kavramını sunar. Çözümleme işleminin neresinde olduğunu belirtmek için bir uyarının durumunu ayarlayabilirsiniz. Bir uyarı oluşturulduğunda durumuna sahip *yeni*. Bir uyarı ve kapattığınızda onayladığınızda durumunu değiştirebilirsiniz. Tüm durum değişiklikleri uyarı geçmişini içinde depolanır.

Şu uyarı durumlarından desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yalnızca algıladı ve henüz gözden. |
| Onaylanan | Bir yönetici, uyarıyı gözden geçirdi ve üzerinde çalışmaya başladı. |
| Kapalı | Sorun çözüldü. Bir uyarı kapatıldıktan sonra başka bir duruma değiştirerek yeniden açabilirsiniz. |

Bir uyarının durumunu izleme koşulu farklıdır. Ölçüm uyarı kuralları, bir koşulu için uyarı ayarlayabilirsiniz _çözümlenen_ zaman hata artık koşul. Uyarı durumu kullanıcı tarafından ayarlanan ve izleme koşulu bağımsızdır. Sistem izleme koşulu "Çözülmüş" için olsa da, uyarı durumu kullanıcı değiştirmediği kadar değiştirilmez.

#### <a name="change-the-state-of-an-alert-or-smart-group"></a>Bir uyarı veya akıllı grubunun durumunu değiştirin
Tek bir uyarının durumunu değiştirebilir veya birden çok uyarı akıllı bir grubun durumunu ayarlayarak birlikte yönetebilirsiniz.

Bir uyarının durumunu seçerek değiştirme **uyarı durumunu değiştir** uyarı için Ayrıntılar görünümünde. Veya seçerek akıllı bir grup için bir durum değişikliği **akıllı grubu durumunu değiştir** , ayrıntı görünümü'nde. Tek seferde birden çok öğe durumunu önce bunları bir liste görünümü ve ardından seçerek değiştirme **durumunu değiştir** sayfanın üstünde. 

Her iki durumda da, yeni bir durum, açılır menüden seçim yapın. Ardından, isteğe bağlı bir açıklama sağlayın. Tek bir öğe değiştiriyorsanız, akıllı grubundaki tüm uyarılar aynı değişiklikleri uygulamak için bir seçenek de.

![Durumu değiştir](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Uyarılar sayfası
Varsayılan uyarılar sayfasında belirli zaman aralığında oluşturulan uyarıların özetini sağlar. Her önem derecesi uyarıların her önem derecesi için her durumda toplam sayısını belirleme sütunlarla toplam uyarı görüntüler. Açmak için önem derecelerinin birini seçin [tüm uyarıları](#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

![Uyarılar sayfası](media/monitoring-overview-unified-alerts/alerts-page.png)

Bu görünümde, sayfanın üst kısmındaki açılan menüler, değerleri seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | En fazla beş Azure aboneliklerini seçin. Yalnızca seçilen Aboneliklerde uyarılar görünümünde dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
| Zaman aralığı | Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Başka bir sayfasını açmak için uyarılar sayfasında üstüne aşağıdaki değerleri seçin.

| Değer | Açıklama |
|:---|:---|
| Toplam uyarı | Seçilen ölçütlerle eşleşen uyarılar toplam sayısı. Bu değer ile filtre tüm uyarılar görünümünü açmak için seçin. |
| Akıllı grupları | Seçilen ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupları toplam sayısı. Tüm uyarılar Görünümü'nde akıllı grupları listesini açmak için bu değeri seçin.
| Toplam uyarı kuralı | Uyarı kuralları seçili abonelik ve kaynak grubunda toplam sayısı. Seçili abonelikte ve kaynak grubu üzerinde filtre kuralları görünümünü açmak için bu değeri seçin.


### <a name="all-alerts-page"></a>Tüm uyarılar sayfasında 
Tüm uyarılar sayfasında kullanarak, seçili zaman aralığında oluşturulan uyarıların bir listesini görüntüleyebilirsiniz. Tek tek uyarıların bir listesi veya uyarıları içeren Akıllı gruplarının bir listesini görüntüleyebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst tarafındaki başlık seçin.

![Tüm uyarılar sayfasında](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Sayfanın üst kısmındaki açılan menüler aşağıdaki değerleri belirleyerek görünüme filtre uygulayabilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | En fazla beş Azure aboneliklerini seçin. Yalnızca seçilen Aboneliklerde uyarılar görünümünde dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
| Kaynak türü | Bir veya daha fazla kaynak türlerini seçin. Yalnızca seçilen türdeki hedefleri olan uyarılar görünümünde dahil edilir. Bu sütun, yalnızca bir kaynak grubu belirttikten sonra kullanılabilir. |
| Kaynak | Bir kaynak seçin. Yalnızca bu kaynak bir hedef olarak uyarılarla Görünümü'nde dahil edilir. Bu sütun, yalnızca bir kaynak türünü belirttikten sonra kullanılabilir. |
| Severity | Bir uyarı önem derecesini seçin ya da seçin *tüm* her türlü önem derecesi, uyarı eklenecek. |
| İzleme koşulu | Bir izleme koşulu seçin ya da seçin *tüm* koşulları uyarıları eklemek için. |
| Uyarı durumu | Bir uyarı durumu veya seçin, *tüm* durumlarının uyarıları eklemek için. |
| İzleme hizmet | Bir hizmet veya seçin, *tüm* tüm hizmetleri dahil etmek için. Yalnızca hizmet hedefi olarak kullanan kurallar tarafından oluşturulan uyarıların dahil edilir. |
| Zaman aralığı | Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Seçin **sütunları** görüntülenecek sütunları seçmek için sayfanın üst kısmındaki. 

### <a name="alert-detail-page"></a>Uyarı Ayrıntı Sayfası
Bir uyarıyı seçtiğinizde, uyarı ayrıntısı sayfası görüntülenir. Bu uyarının ayrıntılar sağlar ve durumuna değiştirmenize olanak tanır.

![Uyarı ayrıntıları](media/monitoring-overview-unified-alerts/alert-detail.png)

Uyarı ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Temel Bileşenler | Özellikleri ve diğer uyarı hakkında önemli bilgi görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarı değişiklikleri listeler. Bu, şu anda durumu değişiklikleri sınırlıdır. |
| Akıllı grubu | Akıllı grubu uyarı hakkındaki bilgiler dahil edilir. *Uyarı sayısı* akıllı grubuna dahil edilen uyarıların sayısını ifade eder. Bu son 30 gün içinde oluşturulan akıllı grubundaki diğer uyarıları içerir.  Uyarıları Listesi sayfasında süresi filtre bağımsız olarak budur. Ayrıntılarını görüntülemek için bir uyarı seçin. |
| Diğer ayrıntılar | Daha fazla bağlamsal bilgi uyarı oluşturulan kaynak türü için genellikle belirli bir uyarı görüntüler. |


### <a name="smart-group-detail-page"></a>Akıllı Grup Ayrıntı Sayfası
Akıllı bir grubu seçtiğinizde akıllı Grup ayrıntı sayfası görüntülenir. Bu grubu oluşturmak için kullanılan ve durumuna değiştirmenize olanak tanır mantık dahil olmak üzere grubun akıllı hakkında ayrıntılar sağlar.
 
![Akıllı grubu ayrıntısı](media/monitoring-overview-unified-alerts/smart-group-detail.png)


Akıllı Grup ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Uyarılar | Akıllı gruba dahil bireysel uyarıları listeler. Kendi uyarı ayrıntısı sayfasını açmak için bir uyarı seçin. |
| Geçmiş | Akıllı grup için yapılan tüm değişiklikler tarafından gerçekleştirilen her eylemi listeler. Durum değişikliklerini ve uyarı üyelik değişiklikleri şu anda sınırlı budur. |

## <a name="next-steps"></a>Sonraki adımlar
- [Yeni uyarı deneyimi oluşturun, görüntüleyin ve Uyarıları yönetmek için kullanmayı öğrenin](monitor-alerts-unified-usage.md)
- [Günlük uyarıları uyarı deneyimi hakkında bilgi edinin](monitor-alerts-unified-log.md)
- [Uyarı deneyimi ölçüm Uyarıları hakkında bilgi edinin](monitoring-near-real-time-metric-alerts.md)
- [Etkinlik günlüğü uyarıları uyarı deneyimi hakkında bilgi edinin](monitoring-activity-log-alerts-new-experience.md)
