---
title: Microsoft Azure'da ve Azure İzleyici Klasik uyarılar genel bakış
description: Klasik uyarılar kullanımdan kalkacaktır. Uyarıları, Azure kaynak ölçümleri, olayları ve günlükleri izlemek ve belirttiğiniz koşulu karşılandığında size bildirilmesini sağlar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/29/2018
ms.author: robb
ms.openlocfilehash: 0d91e12de075ee6efebe39fd5ab582d4998046f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776650"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft azure'da Klasik uyarılar nedir?

> [!NOTE]
> Bu makalede, eski Klasik ölçüm uyarısı oluşturmayı açıklar. Azure İzleyicisi'ni destekler [yeni neredeyse gerçek zamanlı ölçüm uyarıları ve yeni bir uyarı deneyimi](../../azure-monitor/platform/alerts-overview.md). Klasik uyarılar [kullanımdan kaldırılması planlanan](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).  
>

Uyarılar, veriler üzerinde koşulları yapılandırın ve son izleme verilerini koşulları eşleştiğinde bildirilmesi olanak tanır.

## <a name="old-and-new-alerting-capabilities"></a>Eski ve yeni uyarı verme özellikleri

Azure İzleyici, Application Insights, Log Analytics ve hizmetin sistem durumunu ayrı olan geçmişte uyarı verme özellikleri. Zaman içinde Azure geliştirdik ve kullanıcı arabirimi ve uyarı farklı yöntemleri. Birleştirme işlemi hala devam ediyor. Uyarılar

Azure portalında Klasik uyarıları kullanıcı ekran, yalnızca klasik uyarıları görüntüleyebilirsiniz. Bu ekrandan alma **Klasik uyarıları görüntüleyip** uyarılar ekranında düğmesi. 

 ![Azure portalında uyarı seçenekleri](media/alerts-classic.overview/monitor-alert-screen2.png)

Yeni uyarılar kullanıcı deneyimi üzerinde Klasik uyarılar deneyiminin aşağıdaki faydaları vardır:
-   **Daha iyi bir bildirim sistemi** -tüm yeni uyarıları, bildirimleri ve Uyarıları birden çok yeniden kullanılabilir eylemler grupları adlı eylem grupları kullanın. Klasik ölçüm uyarısı ve eski Log Analytics uyarılarını Eylem grupları kullanmayın.
-   **Bir birleşik yazma deneyiminde** - tüm uyarı oluşturma için ölçümleri, günlükleri ve Etkinlik günlüğünü Log Analytics, Azure İzleyici arasında ve Application Insights, tek bir yerde.
-   **Görünüm, Azure portalında Log Analytics uyarılarını harekete** -Ayrıca bkz: aboneliğinizdeki bir Log Analytics uyarılarını harekete artık. Daha önce bunları ayrı bir portalda bulunuyordu.
-   **Tetiklenen uyarılar ve uyarı kuralları ayrımı** - uyarı kuralları (uyarı tetikleyen koşulu tanımı) ve tetiklenen uyarılar (uyarı kuralı Açmadığınızda örneği) Ayrıştırılan, işletimsel ve yapılandırma görünümleri ayrılmış şekilde.
-   **Daha iyi iş akışı** - yeni uyarılar hakkında uyarı için doğru öğelere bulmak daha basit hale getiren bir uyarı kuralı yapılandırma işlemi boyunca kullanıcı deneyimi kılavuzları yazma.
-   **Uyarı birleştirme akıllı** ve **uyarı durumu ayarlama** -yeni uyarılar, otomatik gruplandırma işlevi birlikte aşırı yükleme kullanıcı arabiriminde azaltmak için benzer uyarıların gösteren içerir. 

Yeni ölçüm uyarılarının Klasik ölçüm uyarılarını aşağıdaki avantajlara sahiptir:
-   **Geliştirilmiş gecikme**: Yeni ölçüm uyarılarının kadar sık dakikada bir çalıştırabilirsiniz. Eski ölçüm uyarıları olan 5 dakikada bir sıklığında her zaman çalışır. Yeni uyarıların daha küçük gecikme sorunu oluşum bildirim veya (3-5 dakika) eylemi için artan vardır. 5-15 dakika türüne bağlı olarak eski uyarılardır.  Günlük uyarıları genellikle 10 için 15 dakikalık gecikme nedeniyle günlükleri alma süresine sahip, ancak o zaman yeni işleme yöntemler azaltıyoruz. 
-   **Çok boyutlu ölçümler için destek**: Ölçüm ilgi çekici bir segmentini izlemenize olanak sağlayan boyutlu ölçümler üzerinde uyarı.
-   **Ölçüm koşullar hakkında daha fazla denetime**: Daha zengin bir uyarı kuralları tanımlayabilirsiniz. Yeni uyarılar ölçüm maksimum, minimum, ortalama ve toplam değer izleme desteği sunar.
-   **Birden çok ölçümlerini izleme birleştirilmiş**: (Şu anda en fazla iki ölçüm) birden çok ölçümleri tek bir kural ile izleyebilirsiniz. Her iki ölçüm, belirtilen zaman aralığı için ilgili kendi eşiklerini ihlal etmeniz durumunda bir uyarı tetiklenir.
-   **Daha iyi bir bildirim sistemi**: Tüm yeni uyarılar kullanmak [Eylem grupları](../../azure-monitor/platform/action-groups.md), bildirimleri ve Uyarıları birden çok yeniden kullanılabilir eylemler grupları adlandırılır.  Klasik ölçüm uyarısı ve eski Log Analytics uyarılarını Eylem grupları kullanmayın. 
-   **Ölçümleri günlüklerinden** (genel Önizleme): Günlük verileri log Analytics'e gidip artık olabilir ayıklanır ve Azure İzleyici ölçümleri dönüştürülür ve ardından diğer ölçümler gibi uyarı. Bkz: [uyarılar (Klasik)](alerts-classic.overview.md) Klasik uyarılar için belirli terminolojisi. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Azure İzleyici veri çubuğunda Klasik uyarılar
Klasik uyarılar kullanılabilir - ölçüm uyarıları ve etkinlik günlüğü uyarıları iki tür vardır.

* **Klasik ölçüm uyarıları** -belirtilen bir ölçüm değerini atadığınız eşiği aştığında bu uyarı tetikler. Bu eşik aşıldığında ve uyarı koşulu karşılandığında bir uyarı bir bildirim oluşturur. Bu noktada, uyarının "Etkinleştirildi" olarak kabul edilir. "- Diğer bir deyişle, eşiği yeniden çapraz ve koşulu artık karşılanmıyor çözüldüğünde" başka bir bildirim oluşturur.

* **Klasik etkinlik günlüğü uyarıları** -filtre ölçütlerinizle eşleşen etkinlik günlüğü olay girişe tetikleyen akış günlük uyarısı. Bu uyarılar sahip yalnızca bir durum "Etkinleştirildi". Uyarı altyapısı yalnızca yeni olaya filtre ölçütlerini de geçerlidir. Eski girişler bulmak için aramaz. Yeni bir hizmet durumu olay meydana geldiğinde veya bir kullanıcı veya uygulama aboneliğinizde Örneğin, "Delete sanal makine". bir işlem gerçekleştirdiğinde, bu uyarılar size bildirebilir

Tanılama günlük verilerini Azure İzleyici kullanılabilir, verileri Log Analytics'e (OMS önceden) yönlendirmek ve Log Analytics sorgu uyarısını kullanın. Analizi şimdi kullandığı oturum [yöntemi yeni uyarı](../../azure-monitor/platform/alerts-overview.md) 

Aşağıdaki diyagramda, Azure İzleyici ve, kavramsal olarak, bu verileri nasıl uyarabilir veri kaynakları özetlenmektedir.

![Uyarılar açıklaması](media/alerts-classic.overview/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Uyarılar (Klasik)'in Taksonomisi
Azure Klasik uyarılar ve işlevlerini açıklamak için aşağıdaki terimler kullanılmaktadır:
* **Uyarı** -tanımıma ölçüt (bir veya daha fazla kural ya da koşulları) sağlandığında etkin hale gelir.
* **Etkin** -klasik bir uyarı tarafından tanımlanan ölçütler karşılandığında durumu.
* **Çözümlenen** -klasik bir uyarı tarafından tanımlanan ölçütler artık karşılandığında daha önce karşılanıp karşılanmadığını sonra durumu.
* **Bildirim** - etkin hale gelmesini klasik bir uyarı dışına göre gerçekleştirilecek eylem.
* **Eylem** -bir alıcı bir uyarı (örneğin, bir adresi gönderme veya bir Web kancası URL'si için posta) gönderilen belirli bir çağrı. Bildirimler, genellikle birden fazla eylem tetikleyebilirsiniz.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Azure İzleyici Klasik uyarıdan bir bildirim nasıl alabilirim?
Tarihsel olarak, farklı hizmetler Azure uyarılarından kendi yerleşik bildirim yöntemleri kullanılır. 

Azure İzleyicisi'nde gruplandırma yeniden kullanılabilir bir bildirim çağrılan oluşturulan *Eylem grupları*. Eylem grupları, bir dizi için bir bildirim alıcıları belirtin. Eylem grubu başvuran bir uyarı etkinleştirildi dilediğiniz zaman tüm alıcılar bu bildirim alırsınız. Eylem grupları arasında çok sayıda uyarı nesne yeniden alıcılar (örneğin, nöbet mühendisi listenize) gruplandırmasını sağlar. Eylem grupları bildirim e-posta adresleri, SMS sayıları ve diğer eylemler bir dizi ek olarak bir Web kancası URL'sine yayımlayarak destekler.  Daha fazla bilgi için [Eylem grupları](../../azure-monitor/platform/action-groups.md). 

Eski Klasik etkinlik günlüğü uyarıları, Eylem grupları kullanın.

Ancak, eski ölçüm uyarıları Eylem grupları kullanmayın. Bunun yerine, aşağıdaki eylemleri de yapılandırabilirsiniz: 
- Hizmet Yöneticisi, diğer Yöneticiler veya belirttiğiniz ek e-posta adreslerine e-posta bildirimleri gönderin.
- Bu sayede ek Otomasyon eylemleri başlatmak bir Web kancası çağırın.

Web kancaları otomasyon ve düzeltme, örneğin, kullanarak sağlar:
- Azure Otomasyonu Runbook
- Azure İşlevi
- Azure mantıksal uygulaması
- Bir üçüncü taraf hizmeti

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları ve bunları kullanarak yapılandırma hakkında bilgi alın:

* Daha fazla bilgi edinin [ölçümleri](data-platform.md)
* Yapılandırma [Azure portalında Klasik ölçüm uyarıları](alerts-classic-portal.md)
* Yapılandırma [Klasik ölçüm uyarılarını PowerShell](alerts-classic-portal.md)
* Yapılandırma [Klasik ölçüm uyarılarını komut satırı arabirimi (CLI)](alerts-classic-portal.md)
* Yapılandırma [Klasik ölçüm uyarılarını Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Daha fazla bilgi edinin [etkinlik günlüğü](activity-logs-overview.md)
* Yapılandırma [Azure portal aracılığıyla etkinlik günlüğü uyarıları](activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları Resource Manager aracılığıyla](alerts-activity-log.md)
* Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](activity-log-alerts-webhook.md)
* Daha fazla bilgi edinin [Eylem grupları](action-groups.md)
* Yapılandırma [yeni uyarılar](alerts-metric.md)
