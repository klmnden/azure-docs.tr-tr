---
title: Microsoft Azure'da ve Azure İzleyici Klasik uyarılar genel bakış
description: Klasik uyarılar kullanımdan kalkacaktır. Uyarıları, Azure kaynak ölçümleri, olayları ve günlükleri izlemek ve belirttiğiniz koşulu karşılandığında size bildirilmesini sağlar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 7046a0c6ac84ad5f156098a26dcef2b8accd50af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987654"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft azure'da Klasik uyarılar nedir?

> [!NOTE]
> Bu makalede, eski Klasik ölçüm uyarısı oluşturmayı açıklar. Azure İzleyicisi'ni destekler [yeni neredeyse gerçek zamanlı ölçüm uyarıları ve yeni bir uyarı deneyimi](monitoring-overview-alerts.md). 
>

Uyarılar, veriler üzerinde koşulları yapılandırın ve son izleme verilerini koşulları eşleştiğinde bildirilmesi olanak tanır.

## <a name="old-and-new-alerting-capabilities"></a>Eski ve yeni uyarı verme özellikleri

Son Azure İzleyici'de, Application Insights, Log Analytics ve hizmetin sistem durumunu ayrı uyarı verme özellikleri vardı. Zaman içinde Azure geliştirdik ve kullanıcı arabirimi ve uyarı farklı yöntemleri. Birleştirme işlemi hala devam ediyor. Uyarılar

Azure portalında Klasik uyarıları kullanıcı ekran, yalnızca klasik uyarıları görüntüleyebilirsiniz. Bu ekrandan alma **Klasik uyarıları görüntüleyip** uyarılar ekranında düğmesi. 

 ![Azure portalında uyarı seçenekleri](./media/monitoring-overview-alerts-classic/monitor-alert-screen2.png) 

Yeni uyarılar kullanıcı deneyimi üzerinde Klasik uyarılar deneyiminin aşağıdaki faydaları vardır:
-   **Daha iyi bir bildirim sistemi** -tüm yeni uyarıları, bildirimleri ve Uyarıları birden çok yeniden kullanılabilir eylemler grupları adlı eylem grupları kullanın. Klasik ölçüm uyarısı ve eski Log Analytics uyarılarını Eylem grupları kullanmayın.
-   **Bir birleşik yazma deneyiminde** - tüm uyarı oluşturma için ölçümleri, günlükleri ve Etkinlik günlüğünü Log Analytics, Azure İzleyici arasında ve Application Insights, tek bir yerde.
-   **Görünüm, Azure portalında Log Analytics uyarılarını harekete** -Ayrıca bkz: aboneliğinizdeki bir Log Analytics uyarılarını harekete artık. Daha önce bunları ayrı bir portalda bulunuyordu.
-   **Tetiklenen uyarılar ve uyarı kuralları ayrımı** - uyarı kuralları (uyarı tetikleyen koşulu tanımı) ve tetiklenen uyarılar (uyarı kuralı Açmadığınızda örneği) Ayrıştırılan, işletimsel ve yapılandırma görünümleri ayrılmış şekilde.
-   **Daha iyi iş akışı** - yeni uyarılar hakkında uyarı için doğru öğelere bulmak daha basit hale getiren bir uyarı kuralı yapılandırma işlemi boyunca kullanıcı deneyimi kılavuzları yazma.
-   **Uyarı birleştirme akıllı** ve **uyarı durumu ayarlama** -yeni uyarılar, otomatik gruplandırma işlevi birlikte aşırı yükleme kullanıcı arabiriminde azaltmak için benzer uyarıların gösteren içerir. 

Yeni ölçüm uyarılarının Klasik ölçüm uyarılarını aşağıdaki avantajlara sahiptir:
-   **Geliştirilmiş gecikme**: yeni ölçüm uyarılarının dakikada bir kadar sık çalıştırılabilir. Eski ölçüm uyarıları olan 5 dakikada bir sıklığında her zaman çalışır. Yeni uyarıların bildirim ya da eylem (3-5 dakika) daha küçük occurance sorun gecikme artırma vardır. 5-15 dakika türüne bağlı olarak eski uyarılardır.  Günlük uyarıları genellikle sahip 10-15 dakika gecikme süresi nedeniyle günlükleri alma kadar süreceğine bağlıdır, ancak yeni işleme yöntemler bu süre azaltır. 
-   **Çok boyutlu ölçümler için destek**: ölçüm ilgi çekici bir segmentini izlemenize olanak sağlayan boyutlu ölçümler üzerinde sizi uyarabilir.
-   **Ölçüm koşullar hakkında daha fazla denetime**: daha zengin bir uyarı kuralları tanımlayabilirsiniz. Yeni uyarılar ölçüm maksimum, minimum, ortalama ve toplam değer izleme desteği sunar.
-   **Birden çok ölçümlerini izleme birleştirilmiş**: (şu anda en fazla iki ölçüm) birden çok ölçümleri tek bir kural ile izleyebilirsiniz. Her iki ölçüm, belirtilen zaman aralığı için ilgili kendi eşiklerini ihlal etmeniz durumunda bir uyarı tetiklenir.
-   **Daha iyi bir bildirim sistemi**: tüm yeni uyarılar kullanmak [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md), bildirimleri ve Uyarıları birden çok yeniden kullanılabilir eylemler grupları adlandırılır.  Klasik ölçüm uyarısı ve eski Log Analytics uyarılarını Eylem grupları kullanmayın. 
-   **Ölçümleri günlüklerinden** (genel Önizleme): günlük Log Analytics'e gönderilen veriler artık ayıklanır ve Azure İzleyici ölçümleri dönüştürülür ve ardından diğer ölçümler gibi uyarı. Bkz: [uyarılar (Klasik)](monitoring-overview-alerts-classic.md) Klasik uyarılar için belirli terminolojisi. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Azure İzleyici veri çubuğunda Klasik uyarılar
Klasik uyarılar kullanılabilir - ölçüm uyarıları ve etkinlik günlüğü uyarıları iki tür vardır.

* **Klasik ölçüm uyarıları** -belirtilen bir ölçüm değerini atadığınız eşiği aştığında bu uyarı tetikler. Uyarı "(eşiği aşıldığında ve uyarı koşulu karşılandığında olduğunda) etkinleştirildiğinde" bir uyarı bir bildirim oluşturur. "(Eşiği yeniden çapraz ve koşulu artık karşılanmıyor olduğunda) çözüldüğünde" başka bir bildirim oluşturur.

* **Klasik etkinlik günlüğü uyarıları** -etkinlik günlüğü olayı eşleşme atadığınız ölçütleri Filtrele oluşturulduğunda tetikleyen akış günlük uyarısı. Yalnızca bir durum, bu uyarılar sahip "Uyarı alt filtre ölçütlerini yeni olaya yalnızca geçerlidir. bu yana, etkinleştirildi". Bu uyarılar, yeni bir hizmet durumu olay meydana geldiğinde veya bir kullanıcı veya uygulama "sanal makineyi silin." aboneliğinizde, örneğin, bir işlem gerçekleştirdiğinde bildirilmesi için kullanılabilir

Tanılama günlük verilerini Azure İzleyici kullanılabilir, verileri Log Analytics'e (OMS önceden) yönlendirmek ve Log Analytics sorgu uyarısını kullanın. Analizi şimdi kullandığı oturum [yöntemi yeni uyarı](monitoring-overview-unified-alerts.md) 

Aşağıdaki diyagramda, Azure İzleyici ve, kavramsal olarak, bu verileri nasıl uyarabilir veri kaynakları özetlenmektedir.

![Uyarılar açıklaması](./media/monitoring-overview-alerts-classic/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Uyarılar (Klasik)'in Taksonomisi
Azure Klasik uyarılar ve işlevlerini açıklamak için aşağıdaki terimler kullanılmaktadır:
* **Uyarı** -tanımıma ölçüt (bir veya daha fazla kural ya da koşulları) sağlandığında etkin hale gelir.
* **Etkin** -klasik bir uyarı tarafından tanımlanan ölçütler karşılandığında durumu.
* **Çözümlenen** -klasik bir uyarı tarafından tanımlanan ölçütler artık karşılandığında daha önce karşılanıp karşılanmadığını sonra durumu.
* **Bildirim** - etkin hale gelmesini klasik bir uyarı dışına göre gerçekleştirilecek eylem.
* **Eylem** -bir alıcı bir uyarı (örneğin, bir adresi gönderme veya bir Web kancası URL'si için posta) gönderilen belirli bir çağrı. Bildirimler, genellikle birden fazla eylem tetikleyebilirsiniz.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Azure İzleyici Klasik uyarıdan bir bildirim nasıl alabilirim?
Tarihsel olarak, farklı hizmetler Azure uyarılarından kendi yerleşik bildirim yöntemleri kullanılır. 

Azure İzleyicisi'nde gruplandırma yeniden kullanılabilir bir bildirim çağrılan oluşturulan *Eylem grupları*. Alıcıları bildirim için bir dizi eylem grupları belirtin ve eylem grubu başvuran bir uyarı etkinleştirildi dilediğiniz zaman tüm alıcılar bu bildirim alırsınız. Eylem grupları sayesinde alıcılar (örneğin, nöbet mühendisi listenize) gruplandırmasını yeniden arasında çok sayıda uyarı nesne. Eylem grupları bildirim e-posta adresleri, SMS sayıları ve diğer eylemler bir dizi ek olarak bir Web kancası URL'sine yayımlayarak destekler.  Daha fazla bilgi için [Eylem grupları](monitoring-action-groups.md). 

Eski Klasik etkinlik günlüğü uyarıları, Eylem grupları kullanın.

Ancak, eski ölçüm uyarıları Eylem grupları kullanmayın. Bunun yerine, aşağıdaki eylemleri de yapılandırabilirsiniz: 
- Hizmet Yöneticisi, ortak Yöneticiler veya belirttiğiniz ek e-posta adreslerine e-posta bildirimleri gönderin.
- Bu sayede ek Otomasyon eylemleri başlatmak bir Web kancası çağırın.

Web kancaları otomasyon ve düzeltme, örneğin, kullanarak sağlar:
    - Azure Otomasyonu Runbook
    - Azure İşlevi
    - Azure mantıksal uygulaması
    - Bir üçüncü taraf hizmeti

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları ve bunları kullanarak yapılandırma hakkında bilgi alın:

* Daha fazla bilgi edinin [ölçümleri](monitoring-overview-metrics.md)
* Yapılandırma [Azure portalında Klasik ölçüm uyarıları](insights-alerts-portal.md)
* Yapılandırma [Klasik ölçüm uyarılarını PowerShell](insights-alerts-powershell.md)
* Yapılandırma [Klasik ölçüm uyarılarını komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* Yapılandırma [Klasik ölçüm uyarılarını Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Daha fazla bilgi edinin [etkinlik günlüğü](monitoring-overview-activity-logs.md)
* Yapılandırma [Azure portal aracılığıyla etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları Resource Manager aracılığıyla](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
* Daha fazla bilgi edinin [Eylem grupları](monitoring-action-groups.md)
* Yapılandırma [yeni uyarılar](monitor-alerts-unified-usage.md)