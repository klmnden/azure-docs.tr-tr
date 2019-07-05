---
title: Uyarı ve bildirim Azure'da izlemeye genel bakış
description: Azure'da uyarı genel bakış. Uyarılar, Klasik uyarılar, uyarılar arabirimi.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/28/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: c389f2ab9e67cbb1fd1a6a0c9ee274bca7d4c99d
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560432"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Microsoft azure'da uyarılara genel bakış 

Bu makalede, hangi, avantajları, uyarılar ve bunları kullanmaya başlamak nasıl açıklar.  




## <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft azure'da uyarılar nedir?
Uyarılar önemli olduğunda koşulları izleme verilerinizi bulunan proaktif olarak size bildirir. Sorunları tanımlamanıza ve sisteminizin kullanıcılar bunları fark önce ele olanak sağlar. 

Bu makalede, birleştirilmiş uyarı deneyimi Log Analytics ve Application Insights tarafından yönetiliyordu uyarıları artık içeren Azure İzleyici'de açıklanmaktadır. [Önceki uyarı deneyimi](alerts-classic.overview.md) ve uyarı türleri çağrılır **Klasik uyarılar**. Bu eski deneyimi ve eski uyarı türü tıklayarak görüntüleyebilirsiniz **Klasik uyarıları görüntüleyip** uyarı sayfanın üstünde. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagramda, uyarılar akışını temsil eder. 

![Uyarı akışı](media/alerts-overview/Azure-Monitor-Alerts.svg)

Uyarı kuralları, uyarılar ve bir uyarı tetiklendiğinde gerçekleştirilen eylemler ayrılır. 

**Uyarı kuralı** -uyarı kuralı hedef ve uyarı ölçütlerini yakalar. Uyarı kuralı, etkin veya devre dışı durumda olabilir. Uyarılar yalnızca etkinleştirildiğinde kov. 

Bir uyarı kuralının önemli öznitelikleri şunlardır:

**Hedef kaynak** - kapsamını tanımlar ve uyarı verme için kullanılabilir bildirir. Bir hedef herhangi bir Azure kaynak olabilir. Örnek hedefleri: bir sanal makine, bir depolama hesabı, bir sanal makine ölçek kümesi, bir Log Analytics çalışma alanı veya bir Application Insights kaynağı. Belirli kaynakları (sanal makineler gibi) birden fazla kaynak uyarı kuralının hedefi olarak belirtebilirsiniz.

**Sinyal** - hedef kaynak tarafından yayılan bildirir ve birçok türde olabilir. Ölçüm, etkinlik günlüğü, Application ınsights'ı ve günlük.

**Ölçüt** - ölçütüdür sinyal birleşimi ve mantıksal bir hedef kaynak üzerindeki uygulanır. Örnekler: 
   - CPU yüzdesi 70 > %
   - Sunucu yanıt süresi > 4 ms 
   - Bir günlük sorgu > 100 sonuç sayısı

**Uyarı adı** – belirli bir adı kullanıcı tarafından yapılandırılan bir uyarı kuralı

**Uyarı açıklaması** – kullanıcı tarafından yapılandırılan uyarı kuralı için bir açıklama

**Önem derecesi** – uyarının önem derecesini uyarı kuralında belirtilen ölçütler karşılanıyorsa sonra. Önem derecesi 0'dan 4'e kadar değişebilir.

**Eylem** - uyarı tetiklendiğinde gerçekleştirilecek özel bir eylem. Daha fazla bilgi için [Eylem grupları](../../azure-monitor/platform/action-groups.md).

## <a name="what-you-can-alert-on"></a>Üzerinde uyarabilir

Bölümünde anlatıldığı gibi ölçüm ve günlükleri üzerinde uyarabilir [veri kaynaklarını izleme](../../azure-monitor/platform/data-sources-reference.md). Bunlar dahil ancak bunlarla sınırlı değildir:
- Ölçüm değerleri
- Günlük arama sorguları
- Etkinlik günlüğü olayları
- Temel alınan Azure platformu durumu
- Web sitesi kullanılabilirlik testleri

Daha önce Azure İzleyici ölçümleri, Application Insights, Log Analytics ve hizmet durumu, ayrı bir uyarı verme özellikleri gerekiyordu. Azure, zaman içinde geliştirilmiş ve kullanıcı arabirimi ve uyarı farklı yöntemleri. Bu birleştirme işlemi hala devam ediyor. Sonuç olarak, yok yine de bazı uyarı verme özellikleri henüz yeni uyarılar sistemde.  

| **Kaynak İzleyicisi** | **Sinyal türü**  | **Açıklama** | 
|-------------|----------------|-------------|
| Hizmet durumu | Etkinlik günlüğü  | Desteklenmiyor. Bkz: [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).  |
| Application Insights | Web kullanılabilirlik testleri | Desteklenmiyor. Bkz: [Web test Uyarısı](../../azure-monitor/app/monitor-web-app-availability.md). Application Insights'a veri göndermek için izleme eklenmiş olan tüm Web sitelerinin kullanılabilir. Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentileri altında olduğunda bir bildirim alırsınız. |

## <a name="manage-alerts"></a>Uyarıları yönetme
Çözümleme işleminin neresinde olduğunu belirtmek için bir uyarının durumunu ayarlayabilirsiniz. Uyarı kuralında belirtilen ölçütler karşılandığında bir uyarı oluşturulduğunda veya harekete, durumuna sahip *yeni*. Bir uyarı ve kapattığınızda onayladığınızda durumunu değiştirebilirsiniz. Tüm durum değişiklikleri uyarı geçmişini içinde depolanır.

Şu uyarı durumlarından desteklenir.

| Eyalet | Açıklama |
|:---|:---|
| Yeni | Sorun yalnızca algıladı ve henüz gözden. |
| Onaylandı | Bir yönetici, uyarıyı gözden geçirdi ve üzerinde çalışmaya başladı. |
| Kapalı | Sorun çözüldü. Bir uyarı kapatıldıktan sonra başka bir duruma değiştirerek yeniden açabilirsiniz. |

**Uyarı durumu** farklı ve bağımsız olarak **izleme koşulu**. Uyarı durumu, kullanıcı tarafından ayarlanır. İzleme koşulu, sistem tarafından ayarlanır. Bir uyarı tetiklendiğinde uyarı izleme koşulu kümesine *harekete*. Uyarıyı temizler ateşlenmesine neden altta yatan durumun izleme koşulu ayarlandığında *çözümlenen*. Uyarı durumu kullanıcı değiştirmediği kadar değiştirilmez. Bilgi [uyarılar ve akıllı grupları durumunu değiştirme](https://aka.ms/managing-alert-smart-group-states).

## <a name="smart-groups"></a>Akıllı gruplar 
Akıllı grupları Önizleme aşamasındadır. 

Yardımcı olan makine öğrenimi algoritmalarıyla, bağlı uyarılar toplamalarının uyarı gürültüsünü azaltmak ve trouble-shooting içinde yardımcı bunun akıllı gruplarıdır. [Akıllı grupları hakkında daha fazla bilgi](https://aka.ms/smart-groups) ve [akıllı gruplarınızı yönetmek nasıl](https://aka.ms/managing-smart-groups).


## <a name="alerts-experience"></a>Uyarı deneyimi 
Varsayılan uyarılar sayfasında belirli zaman aralığında oluşturulan uyarıların özetini sağlar. Her önem derecesi uyarıların her önem derecesi için her durumda toplam sayısını belirleme sütunlarla toplam uyarı görüntüler. Açmak için önem derecelerinin birini seçin [tüm uyarıları](#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

Alternatif olarak, [REST API'lerini kullanarak aboneliklerinizi üzerinde oluşturulan uyarı örneklerinin program aracılığıyla listeleme](#manage-your-alert-instances-programmatically).

Bırakmaz göster veya eski izleme [Klasik uyarılar](#classic-alerts). Abonelikleri değiştirin veya filtre parametreleri sayfası. 

![Uyarılar sayfası](media/alerts-overview/alerts-page.png)

Bu görünümde, sayfanın üst kısmındaki açılan menüler, değerleri seçerek filtreleyebilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarıları görüntülemek istediğiniz Azure aboneliklerini seçin. Tüm aboneliklerinizi seçmek isteğe bağlı olarak seçebilirsiniz. Seçili Aboneliklerde erişimi olmasını yalnızca uyarılar görünümünde dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
| Zaman aralığı | Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Başka bir sayfasını açmak için uyarılar sayfasında üstüne aşağıdaki değerleri seçin.

| Değer | Açıklama |
|:---|:---|
| Toplam uyarı | Seçilen ölçütlerle eşleşen uyarılar toplam sayısı. Bu değer ile filtre tüm uyarılar görünümünü açmak için seçin. |
| Akıllı gruplar | Seçilen ölçütlerle eşleşen uyarılardan oluşturulan akıllı grupları toplam sayısı. Tüm uyarılar Görünümü'nde akıllı grupları listesini açmak için bu değeri seçin.
| Toplam uyarı kuralı | Uyarı kuralları seçili abonelik ve kaynak grubunda toplam sayısı. Seçili abonelikte ve kaynak grubu üzerinde filtre kuralları görünümünü açmak için bu değeri seçin.


## <a name="manage-alert-rules"></a>Uyarı kurallarını yönet
Tıklayarak **uyarı kurallarını yönet** gösterilecek **kuralları** sayfası. **Kuralları** Azure aboneliklerinizde uyarı kurallarının tümünü yönetmek için tek bir yerdir. Bu, uyarı kurallarının tümünü listeler ve hedef kaynaklar, kaynak grupları, kural adı veya durum göre sıralanabilir. Uyarı kuralları ayrıca düzenlendi, etkin veya bu sayfadan devre dışı.  

 ![Uyarı kuralları](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Uyarıları izleme hizmeti bağımsız olarak tutarlı bir şekilde yazılabilir veya tür sinyal. Tüm uyarıları harekete ve ilgili ayrıntıları tek sayfasında kullanılabilir.
 
Aşağıdaki üç adımı ile yeni bir uyarı kuralı oluşturun:
1. Çekme _hedef_ uyarı.
1. Seçin _sinyal_ öğesinden hedef için kullanılabilir sinyaller.
1. Belirtin _mantıksal_ verilere sinyalden uygulanacak.
 
Bu basitleştirilmiş bir yazma işlemi artık izleme kaynağı veya bir Azure kaynağı seçmeden önce desteklenen sinyalleri bilmesini gerektirmez. Kullanılabilir sinyaller listesini otomatik olarak seçtiğiniz hedef kaynak göre filtrelenir. Ayrıca bu hedefte bağlı olarak, otomatik olarak uyarı kuralı mantığını tanımlama aracılığıyla kılavuzluk edilir.  

Uyarı kuralları oluşturma hakkında daha fazla bilgi [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).

Uyarılar, çeşitli Azure izleme hizmetleri arasında kullanılabilir. Hakkında bilgi ve bu hizmetlerin her biri kullanıldığı durumlar için bkz: [izleme Azure uygulamalarını ve kaynaklarını](../../azure-monitor/overview.md). 


## <a name="all-alerts-page"></a>Tüm uyarılar sayfasında 
Toplam uyarı tüm uyarılar sayfasında görmek için tıklayın. Burada, seçili zaman aralığında oluşturulan uyarıların bir listesini görüntüleyebilirsiniz. Tek tek uyarıların bir listesi veya uyarıları içeren Akıllı gruplarının bir listesini görüntüleyebilirsiniz. Görünümler arasında geçiş yapmak için sayfanın üst tarafındaki başlık seçin.

![Tüm uyarılar sayfasında](media/alerts-overview/all-alerts-page.png)

Sayfanın üst kısmındaki açılan menüler aşağıdaki değerleri belirleyerek görünüme filtre uygulayabilirsiniz.

| Sütun | Açıklama |
|:---|:---|
| Abonelik | Uyarıları görüntülemek istediğiniz Azure aboneliklerini seçin. Tüm aboneliklerinizi seçmek isteğe bağlı olarak seçebilirsiniz. Seçili Aboneliklerde erişimi olmasını yalnızca uyarılar görünümünde dahil edilir. |
| Kaynak grubu | Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
| Kaynak türü | Bir veya daha fazla kaynak türlerini seçin. Yalnızca seçilen türdeki hedefleri olan uyarılar görünümünde dahil edilir. Bu sütun, yalnızca bir kaynak grubu belirttikten sonra kullanılabilir. |
| Resource | Bir kaynak seçin. Yalnızca bu kaynak bir hedef olarak uyarılarla Görünümü'nde dahil edilir. Bu sütun, yalnızca bir kaynak türünü belirttikten sonra kullanılabilir. |
| Severity | Bir uyarı önem derecesini seçin ya da seçin *tüm* her türlü önem derecesi, uyarı eklenecek. |
| İzleme koşulu | Bir izleme koşulu seçin ya da seçin *tüm* koşulları uyarıları eklemek için. |
| Uyarı durumu | Bir uyarı durumu veya seçin, *tüm* durumlarının uyarıları eklemek için. |
| İzleme hizmeti | Bir hizmet veya seçin, *tüm* tüm hizmetleri dahil etmek için. Yalnızca hizmet hedefi olarak kullanan kurallar tarafından oluşturulan uyarıların dahil edilir. |
| Zaman aralığı | Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Seçin **sütunları** görüntülenecek sütunları seçmek için sayfanın üst kısmındaki. 

## <a name="alert-details-page"></a>Uyarı Ayrıntıları sayfası
Bir uyarıyı seçtiğinizde, uyarı ayrıntısı sayfası görüntülenir. Bu uyarının ayrıntılar sağlar ve durumuna değiştirmenize olanak tanır.

![Uyarı ayrıntısı](media/alerts-overview/alert-detail2.png)

Uyarı Ayrıntıları sayfası aşağıdaki bölümleri içerir.

| `Section` | Açıklama |
|:---|:---|
| Özet | Özellikleri ve diğer uyarı hakkında önemli bilgi görüntüler. |
| Geçmiş | Uyarı tarafından gerçekleştirilen her eylemi ve uyarı değişiklikleri listeler. Durum değişiklikleri şu anda sınırlı. |
| Tanılama | Akıllı grubu uyarı hakkındaki bilgiler dahil edilir. *Uyarı sayısı* akıllı grubuna dahil edilen uyarıların sayısını ifade eder. Uyarıları Listesi sayfasında süresi filtre bağımsız olarak son 30 gün içinde oluşturulan akıllı grubundaki diğer uyarıları içerir. Ayrıntılarını görüntülemek için bir uyarı seçin. |

## <a name="role-based-access-control-rbac-for-your-alert-instances"></a>Uyarı örnekleriniz için rol tabanlı erişim denetimi (RBAC)

Tüketim ve uyarı örneklerinin yönetim gerektirir ya da yerleşik RBAC rolleri kullanıcı [izleme katkıda bulunanı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) veya [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader). Bu roller, bir kaynak düzeyinde ayrıntılı atamaları için abonelik düzeyinde bir Azure Resource Manager kapsamda desteklenir. Örneğin, kullanıcının yalnızca 'ContosoVM1' sanal makinesi için 'izleme katkıda bulunanı' erişimi varsa, ardından o kullanma ve yalnızca 'ContosoVM1' üzerinde oluşturulan uyarıları yönetmek kullanabilirsiniz.

## <a name="manage-your-alert-instances-programmatically"></a>Uyarı örneklerinizin programlama yoluyla yönetme

Program aracılığıyla sorgulamak için üretilen uyarılar için aboneliğinizi karşı istediğiniz birçok senaryo vardır. Bu, Azure portalında dışında özel görünümlerini oluşturma veya modelleri ve eğilimlerini belirlemek için uyarıları çözümlemek için olabilir.

Aboneliklerinizi karşı kullanarak ya da oluşturulan uyarılar için sorgulama yapabilirsiniz [uyarı Yönetimi REST API'si](https://aka.ms/alert-management-api) kullanarak veya [uyarılar için Azure kaynak Graph REST API](https://docs.microsoft.com/rest/api/azureresourcegraph/resources/resources).

[Uyarılar için Azure kaynak Graph REST API](https://docs.microsoft.com/rest/api/azureresourcegraph/resources/resources) uygun ölçekte uyarı örnekleri için sorgu olanak tanır. Bu, birçok farklı abonelikler arasında oluşturulan uyarıları yönetmek için sahip olduğu senaryolar için önerilir. 

Aşağıdaki örnek istek API için bir abonelik içindeki uyarı sayısı döndürür:

```json
{
  "subscriptions": [
    <subscriptionId>
  ],
  "query": "where type =~ 'Microsoft.AlertsManagement/alerts' | summarize count()",
  "options": {
            "dataset":"alerts"
  }
}
```
Uyarılar için sorgulanabilir kendi ['temel'](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#essentials-fields) alanları.

[Uyarı Yönetimi REST API'si](https://aka.ms/alert-management-api) dahil olmak üzere belirli uyarılar hakkında daha fazla bilgi almak için kullanılan kendi ['uyarı bağlamı'](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) alanları.

## <a name="classic-alerts"></a>Klasik uyarılar 

Özellik Haziran 2018 tarihinden önce uyarı Azure İzleyici ölçümleri ve etkinlik günlüğüne "Uyarılar (Klasik)" olarak adlandırılır. 

Daha fazla bilgi için [Klasik uyarılar](./../../azure-monitor/platform/alerts-classic.overview.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı grupları hakkında daha fazla bilgi edinin](https://aka.ms/smart-groups)
- [Eylem grupları hakkında bilgi edinin](../../azure-monitor/platform/action-groups.md)
- [Uyarı örneklerinizi azure'da yönetme](https://aka.ms/managing-alert-instances)
- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)






