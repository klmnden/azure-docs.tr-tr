---
title: Uyarı ve bildirim Azure'da izlemeye genel bakış
description: Azure'da uyarı genel bakış. Uyarılar, Klasik uyarılar, uyarılar arabirimi.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: f044cf7e0b614d338ec9b294dfbf02c26c4351b1
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50413869"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft azure'da uyarılara genel bakış 

Bu makalede, hangi, avantajları, uyarılar ve bunları kullanmaya başlamak nasıl açıklar.  


## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft azure'da uyarılar nedir?
Uyarılar önemli olduğunda koşulları izleme verilerinizi bulunan proaktif olarak size bildirir. Sorunları tanımlamanıza ve sisteminizin kullanıcılar bunları fark önce ele olanak sağlar. 

Bu makalede, birleştirilmiş uyarı deneyimi artık Log Analytics ve Application Insights'ı içeren Azure İzleyici'de açıklanmaktadır. [Önceki uyarı deneyimi](monitoring-overview-alerts.md) ve uyarı türleri çağrılır **Klasik uyarılar**. Bu eski deneyimi ve eski uyarı türü tıklayarak görüntüleyebilirsiniz **Klasik uyarıları görüntüleyip** uyarı sayfanın üstünde.


## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagramda, uyarılar, akış ve genel koşulları temsil eder. 

![Uyarı akışı](media/monitoring-overview-alerts/Azure-Monitor-Alerts.svg)

Uyarı kuralları, uyarılar ve bir uyarı tetiklendiğinde gerçekleştirilecek eylemi ayrılır. 

- **Uyarı kuralı** -uyarı kuralı hedef ve uyarı ölçütlerini yakalar. Uyarı kuralı, etkin veya devre dışı durumda olabilir. Uyarılar yalnızca etkinleştirildiğinde kov. Bir uyarı kuralları önemli öznitelikleri şunlardır:
    - **Hedef kaynak** -tüm Azure kaynakları bir hedef olabilir. Hedef kaynak, uyarı vermek için kullanılabilir sinyaller ve kapsamını tanımlar. Örnek hedefleri: bir sanal makine, bir depolama hesabı, bir sanal makine ölçek kümesi, bir Log Analytics çalışma alanı veya bir Application Insights kaynağı. Belirli kaynaklar (örneğin, sanal makineler), bir uyarı kuralının hedefi olarak birden fazla kaynak belirtebilirsiniz.
    - **Sinyal** - hedef kaynak tarafından yayılan bildirir ve birçok türde olabilir. Ölçüm, etkinlik günlüğü, Application ınsights'ı ve günlük.
    - **Ölçüt** - ölçütüdür sinyal birleşimi ve mantıksal bir hedef kaynak üzerindeki uygulanır. Örnekler: 
         - CPU yüzdesi 70 > %
         - Sunucu yanıt süresi > 4 ms 
         - Bir günlük sorgu > 100 sonuç sayısı
- **Uyarı adı** – belirli bir adı kullanıcı tarafından yapılandırılan bir uyarı kuralı
- **Uyarı açıklaması** – kullanıcı tarafından yapılandırılan uyarı kuralı için bir açıklama
- **Önem derecesi** – uyarının önem derecesini uyarı kuralında belirtilen ölçütler karşılanıyorsa sonra. Önem derecesi 0'dan 4'e kadar değişebilir.
- **Eylem** - uyarı tetiklendiğinde gerçekleştirilecek özel bir eylem. Eylem grupları daha fazla bilgi için bkz.

## <a name="what-you-can-alert-on"></a>Üzerinde uyarabilir

Bölümünde anlatıldığı gibi ölçüm ve günlükleri üzerinde uyarabilir [veri kaynaklarını izleme](monitoring-data-sources.md). Bunlar dahil ancak bunlarla sınırlı değildir:
- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformu durumu
- Web sitesi kullanılabilirlik testleri



## <a name="manage-alerts"></a>Uyarıları yönetme
Çözümleme işleminin neresinde olduğunu belirtmek için bir uyarının durumunu ayarlayabilirsiniz. Uyarı kuralında belirtilen ölçütler karşılandığında bir uyarı oluşturulduğunda veya harekete, durumuna sahip *yeni*. Bir uyarı ve kapattığınızda onayladığınızda durumunu değiştirebilirsiniz. Tüm durum değişiklikleri uyarı geçmişini içinde depolanır.

Şu uyarı durumlarından desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yalnızca algıladı ve henüz gözden. |
| Onaylanan | Bir yönetici, uyarıyı gözden geçirdi ve üzerinde çalışmaya başladı. |
| Kapatıldı | Sorun çözüldü. Bir uyarı kapatıldıktan sonra başka bir duruma değiştirerek yeniden açabilirsiniz. |

Bir uyarının durumunu izleme koşulu farklıdır. Uyarı durumu kullanıcı tarafından ayarlanan ve izleme koşulu bağımsızdır. Tetiklenme uyarı için altta yatan durumun temizlediğinde, uyarı için izleme koşulu çözümlenen ayarlanır. Sistem izleme koşulu çözülmüş olsa da, uyarı durumu kullanıcı değiştirmediği kadar değiştirilmez. Bilgi [uyarılar ve akıllı grupları durumunu değiştirme](https://aka.ms/managing-alert-smart-group-states).

## <a name="smart-groups"></a>Akıllı gruplar 
Akıllı grupları Önizleme aşamasındadır. 

Yardımcı olan makine öğrenimi algoritmalarıyla bağlı uyarılar toplamalarının uyarı gürültüsünü azaltmak ve trouble-shooting içinde yardımcı bunun akıllı gruplarıdır. [Akıllı grupları hakkında daha fazla bilgi](https://aka.ms/smart-groups) ve [akıllı gruplarınızı yönetmek nasıl](https://aka.ms/managing-smart-groups).


## <a name="alerts-experience"></a>Uyarı deneyimi 
Varsayılan uyarılar sayfasında belirli zaman aralığında oluşturulan uyarıların özetini sağlar. Her önem derecesi uyarıların her önem derecesi için her durumda toplam sayısını belirleme sütunlarla toplam uyarı görüntüler. Açmak için önem derecelerinin birini seçin [tüm uyarıları](#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

Bırakmaz göster veya eski izleme [Klasik uyarılar](#classic-alerts). Abonelikleri değiştirin veya filtre parametreleri sayfası. 

![Uyarılar sayfası](media/monitoring-overview-alerts/alerts-page.png)

Bu görünümde, sayfanın üst kısmındaki açılan menüler, değerleri seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | En fazla beş Azure aboneliklerini seçin. Yalnızca seçilen Aboneliklerde uyarılar görünümünde dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
| Zaman aralığı | Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Başka bir sayfasını açmak için uyarılar sayfasında üstüne aşağıdaki değerleri seçin.

| Değer | Açıklama |
|:---|:---|
| Toplam uyarı sayısı | Seçilen ölçütlerle eşleşen uyarılar toplam sayısı. Bu değer ile filtre tüm uyarılar görünümünü açmak için seçin. |
| Akıllı gruplar | Seçilen ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupları toplam sayısı. Tüm uyarılar Görünümü'nde akıllı grupları listesini açmak için bu değeri seçin.
| Toplam uyarı kuralı sayısı | Uyarı kuralları seçili abonelik ve kaynak grubunda toplam sayısı. Seçili abonelikte ve kaynak grubu üzerinde filtre kuralları görünümünü açmak için bu değeri seçin.


## <a name="manage-alert-rules"></a>Uyarı kurallarını yönetin
Tıklayarak **uyarı kurallarını yönet** gösterilecek **kuralları** sayfası. **Kuralları** Azure aboneliklerinizde uyarı kurallarının tümünü yönetmek için tek bir yerdir. Bu, uyarı kurallarının tümünü listeler ve hedef kaynaklar, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları ayrıca düzenlendi, etkin veya bu sayfadan devre dışı.  

 ![Uyarı kuralları](./media/monitoring-overview-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya tür sinyal. Tüm uyarıları harekete ve ilgili ayrıntıları tek sayfasında kullanılabilir.
 
Aşağıdaki üç adımı ile yeni bir uyarı kuralı oluşturun:
1. Çekme _hedef_ uyarı.
1. Seçin _sinyal_ öğesinden hedef için kullanılabilir sinyaller.
1. Belirtin _mantıksal_ verilere sinyalden uygulanacak.
 
Bu basitleştirilmiş bir yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri bilmesini gerektirmez. Kullanılabilir sinyaller listesi otomatik olarak seçtiğiniz hedef kaynak göre filtrelenir ve uyarı kuralı mantığını tanımlama aracılığıyla yol gösterir.

Uyarı kuralları oluşturma hakkında daha fazla bilgi [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](monitor-alerts-unified-usage.md).

Uyarılar, çeşitli Azure izleme hizmetleri arasında kullanılabilir. Hakkında bilgi ve bu hizmetlerin her biri kullanıldığı durumlar için bkz: [izleme Azure uygulamalarını ve kaynaklarını](./monitoring-overview.md). Aşağıdaki tabloda, Azure genelinde kullanılabilir olan uyarı kuralları türlerinin bir listesini sağlar. Ayrıca, hangi uyarı deneyimi desteklenen özellikler listelenir.

Daha önce Azure İzleyici, Application Insights, Log Analytics ve hizmet durumu, ayrı bir uyarı verme özellikleri gerekiyordu. Zaman içinde Azure geliştirdik ve kullanıcı arabirimi ve uyarı farklı yöntemleri. Bu birleştirme işlemi hala devam ediyor. Sonuç olarak, yok yine de bazı uyarı verme özellikleri henüz yeni uyarılar sistemde.  

| **Kaynak İzleyicisi** | **Sinyal türü**  | **Açıklama** | 
|-------------|----------------|-------------|
| Hizmet durumu | Etkinlik günlüğü  | Desteklenmiyor. Bkz: [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Web kullanılabilirlik testleri | Desteklenmiyor. Bkz: [Web test Uyarısı](../application-insights/app-insights-monitor-web-app-availability.md). Application Insights'a veri göndermek için izleme eklenmiş olan tüm Web sitelerinin kullanılabilir. Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentileri altında olduğunda bir bildirim alırsınız. |


## <a name="all-alerts-page"></a>Tüm uyarılar sayfasında 
Toplam uyarı tüm uyarılar sayfasında görmek için tıklayın. Burada, seçili zaman aralığında oluşturulan uyarıların bir listesini görüntüleyebilirsiniz. Tek tek uyarıların bir listesi veya uyarıları içeren Akıllı gruplarının bir listesini görüntüleyebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst tarafındaki başlık seçin.

![Tüm uyarılar sayfasında](media/monitoring-overview-alerts/all-alerts-page.png)

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

## <a name="alert-detail-page"></a>Uyarı Ayrıntı Sayfası
Bir uyarıyı seçtiğinizde, uyarı ayrıntısı sayfası görüntülenir. Bu uyarının ayrıntılar sağlar ve durumuna değiştirmenize olanak tanır.

![Uyarı ayrıntıları](media/monitoring-overview-alerts/alert-detail2.png)

Uyarı ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Temel Bileşenler | Özellikleri ve diğer uyarı hakkında önemli bilgi görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarı değişiklikleri listeler. Bu, şu anda durumu değişiklikleri sınırlıdır. |
| Akıllı grubu | Akıllı grubu uyarı hakkındaki bilgiler dahil edilir. *Uyarı sayısı* akıllı grubuna dahil edilen uyarıların sayısını ifade eder. Bu son 30 gün içinde oluşturulan akıllı grubundaki diğer uyarıları içerir.  Uyarıları Listesi sayfasında süresi filtre bağımsız olarak budur. Ayrıntılarını görüntülemek için bir uyarı seçin. |
| Diğer ayrıntılar | Daha fazla bağlamsal bilgi uyarı oluşturulan kaynak türü için genellikle belirli bir uyarı görüntüler. |


## <a name="classic-alerts"></a>Klasik uyarılar 

Özellik Haziran 2018 tarihinden önce uyarı Azure İzleyici ölçümleri ve etkinlik günlüğüne "Uyarılar (Klasik)" olarak adlandırılır. 

Daha fazla bilgi için [Klasik uyarılar](./monitoring-overview-alerts-classic.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı grupları hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups)
- [Eylem grupları hakkında bilgi edinin](monitoring-action-groups.md)
- [Uyarı örneklerinizi azure'da yönetme](https://aka.ms/managing-alert-instances)
- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)





