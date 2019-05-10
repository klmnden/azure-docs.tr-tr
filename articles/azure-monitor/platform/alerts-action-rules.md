---
title: Azure İzleyici uyarılar için Eylem kuralları
description: Eylem kuralları nelerdir ve nasıl yapılandırılacağı ve yönetileceğine anlama.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: d27adadc9720dd2ad6a0dd133524bfaf32e63045
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65227976"
---
# <a name="action-rules-preview"></a>Eylem kuralları (Önizleme)

Bu makalede, Eylem kuralları nelerdir ve nasıl yapılandırılacağı ve yönetileceğine açıklanmaktadır.

## <a name="what-are-action-rules"></a>Eylem kuralları nelerdir?

Eylem kuralları eylemleri (veya eylemleri gizleme) tüm Resource Manager kapsam (abonelik, kaynak grubu, veya kaynak) tanımlamanızı sağlar. Belirli alt uygulamak istediğiniz uyarı örneklerinin daraltmak olanak tanıyan filtreler çeşitli sahiptirler. 

Eylem kuralları ile şunları yapabilirsiniz:

* Bakım pencereleri planladığınızdan veya hafta sonu/tatiller için her devre dışı bırakmak zorunda yerine tek tek uyarı kuralı eylemleri ve bildirimleri gösterme.
* Eylemler ve uygun ölçekte bildirimleri tanımlayın: Her uyarı kuralı için ayrı ayrı bir eylem grubu tanımlamak zorunda kalmak yerine, herhangi bir kapsamda oluşturulan uyarılar için tetiklemek için bir eylem grubu artık tanımlayabilirsiniz. Örneğin, Aboneliğim dahilindeki oluşturulan her uyarı için eylem grubu 'ContosoActionGroup' tetiklemesini sağlamak tercih edebilirsiniz.

## <a name="configuring-an-action-rule"></a>Bir eylem kuralı yapılandırma

Özellik seçerek erişebilirsiniz **işlemleri yönetmenizi** Azure İzleyici'de giriş sayfası uyarılar. Ardından **eylem kuralları (Önizleme)**. Seçerek erişebilirsiniz **eylem kuralları (Önizleme)** uyarılar için giriş sayfasının panosundan.

![Azure İzleyici giriş sayfasından eylemi kuralları](media/alerts-action-rules/action-rules-landing-page.png)

Seçin **+ yeni eylem kural**. 

![Yeni Eylem Kuralı Ekle](media/alerts-action-rules/action-rules-new-rule.png)

Alternatif olarak, bir uyarı kuralını yapılandırırken bir eylem kuralı oluşturmak de seçebilirsiniz.

![Yeni Eylem Kuralı Ekle](media/alerts-action-rules/action-rules-alert-rule.png)

Şimdi eylem kural oluşturma akışı açın görmeniz gerekir. Aşağıdaki öğeler yapılandırın: 

![Yeni Eylem kural oluşturma akış](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>`Scope`

İlk kapsamı, diğer bir deyişle, hedef kaynak, kaynak grubu veya abonelik seçin. Yukarıdakilerin tümü (içinde tek bir abonelik) birleşimi çoklu seçim özelliği de vardır. 

![Eylem Kural kapsamı](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filtre ölçütü

Ayrıca, belirli bir alt kümesini tanımlanmış kapsamda uyarıları aşağı daha da daraltmak için filtreleri tanımlayabilirsiniz. 

Kullanılabilir filtreleri şunlardır: 

* **Önem derecesi**: Bir veya daha fazla uyarı önem dereceleri seçin. Önem derecesi = Sev1 anlamına gelir eylem kural önem derecesi Sev1 olarak tüm uyarılar için geçerlidir.
* **Hizmet izleme**: Kaynak izleme hizmetini temel alan filtre. Ayrıca çoklu seçim yöntemdir. Örneğin, izleme hizmeti "Application Insights" = uyarılara dayalı olarak eylem kuralı tüm "Application Insights için" uygun olduğu anlamına gelir.
* **Kaynak türü**: Belirli kaynak türüne göre filtreleyin. Ayrıca çoklu seçim yöntemdir. Örneğin, kaynak türü = "Sanal makineler" anlamına gelir Eylem kuralı tüm sanal makineler için geçerlidir.
* **Uyarı kuralı kimliği**: Uyarı kuralı Resource Manager Kimliğini kullanarak özel uyarı kuralları için filtrelemenize olanak tanır.
* **İzleme koşulu**: "Fired" veya "Çözüldü" uyarı örneği için izleme koşulu filtreleyin.
* **Açıklama**: Uyarı kuralının bir parçası tanımlanan açıklama içinde eşleşen normal ifade.
* **Uyarı bağlamı (yükü)**: Normal ifade içinde eşleşen [uyarı bağlamı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) uyarı örneği alanları.

Bu filtreler birlikte bir başkasına uygulanır. Örneğin, 'Kaynak türü' ayarlarım, 'Sanal makineleri' ve 'Önem' = 'Sev0' tüm 'Sev0' uyarılar için yalnızca my Vm'lerde filtreledi sonra =. 

![Eylem kural filtreleri](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Gizleme veya eylem grubu yapılandırma

Sonraki eylem kural uyarı gizleme veya eylem grubu desteği için yapılandırın. Her ikisini seçemezsiniz. Yapılandırma, filtreleri ve önceden tanımlanmış kapsamını eşleşen tüm uyarı örneklerinde çalışır.

#### <a name="suppression"></a>Gizleme

Seçerseniz **gizleme**, Eylemler ve bildirimleri bastırma süresince yapılandırın. Aşağıdakilerden birini seçin:
* **Bugünden itibaren (her zaman)**: Süresiz olarak tüm bildirimleri engeller.
* **Zamanlanan tarihte**: Sınırlı bir süre içinde bildirimlerini bastır.
* **Bir yineleme ile**: Günlük, haftalık veya Aylık yinelenme zamanlamasına göre gizle.

![Eylem kural gizleme](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Eylem grubu

Seçerseniz **eylem grubu** geçiş ya da var olan bir eylem grubu ekleyin veya yeni bir tane oluşturun. 

> [!NOTE]
> Yalnızca bir eylem grubu Eylem kuralı ile ilişkilendirebilirsiniz.

![Eylem kural eylem grubu](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Eylem kural ayrıntıları

Son olarak, aşağıdaki eylemi kuralın ayrıntılarını Yapılandır
* Ad
* Kaynak grubu içinde kaydedilecek
* Açıklama 

## <a name="example-scenarios"></a>Örnek senaryolar

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Senaryo 1: Önem derecesine göre uyarıları gizleme

Contoso, tüm sanal makineler, abonelik 'ContosoSub' her hafta içindeki tüm Sev4 uyarılar için bildirimleri bastır ister.

**Çözüm:** Bir eylem kuralı oluşturun
* Kapsam 'ContosoSub' =
* Filtreler
    * Önem derecesi = 'Sev4'
    * Kaynak türü 'Sanal makineleri' =
* Haftalık yinelenme ile gizleme ayarlayın ve 'Cumartesi' ve 'Pazar' işaretli

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Senaryo 2: Uyarı bağlamı (yükü) tabanlı uyarı gizleme

Contoso isteyen tüm günlük 'İçin bilgisayar-01' oluşturulan uyarılar için bildirimleri bastır 'İçinde ContosoSub' süresiz olarak gibi bakım yapmamanın geçiyor.

**Çözüm:** Bir eylem kuralı oluşturun
* Kapsam 'ContosoSub' =
* Filtreler
    * Hizmet İzleme 'Log Analytics' =
    * Uyarı bağlamı (yükü) 'Bilgisayar-01' içeriyor
* Gizleme kümesine ' bugünden itibaren (her zaman)'

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Senaryo 3: Bir kaynak grubu tanımlanan eylem grubu

Contoso tanımlanmış [abonelik düzeyinde ölçüm Uyarısı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor), ancak bunların 'ContosoRG' kaynak grubu için ayrı ayrı uyarılar için tetikleme eylemleri tanımlamak ister.

**Çözüm:** Bir eylem kuralı oluşturun
* Scope = 'ContosoRG'
* Filtre
* 'ContosoActionGroup için' eylem grubu ayarlayın

## <a name="managing-your-action-rules"></a>Eylem kurallarınızı yönetme

Görüntüleyebilir ve aşağıda gösterildiği liste görünümü, eylem kurallarını yönetin.

![Eylem kuralları listesi görünümü](media/alerts-action-rules/action-rules-list-view.png)

Buradan, bunların yanındaki onay kutusunu seçerek etkinleştir/devre dışı bırakma/silme eylemi kurallarına uygun ölçekte kullanabilirsiniz. Herhangi bir eylem kural'ı tıklatarak, böylece tanımını güncelleştirin ve bu etkinleştir/devre kendi yapılandırma sayfası açılır.

## <a name="best-practices"></a>En iyi yöntemler

Günlük uyarıları ile oluşturulan ['sonuç sayısı'](alerts-unified-log.md) oluşturma seçeneği **tek bir uyarı örneği** kullanarak (örneğin, birden fazla bilgisayara olabilir) tüm arama sonuç. Bir eylem Kuralı 'Uyarı bağlamı (yükü)' filtresi kullanıyorsa bir eşleşme var olduğu sürece bu senaryoda, uyarı örneğinde davranır. Senaryo 2 'Bilgisayar-01' hem 'Bilgisayar-02' oluşturulan günlüğü uyarısı için arama sonuçlarını içeriyorsa, daha önce anlatıldığı gibi tüm bildirim bastırılır (diğer bir deyişle, 'Bilgisayar-02' için hiç oluşturulan bildirim yoktur).

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

En iyi yararlanarak günlük uyarıları için eylem kurallarla, biz ile günlüğü uyarıları oluşturma için öneri ['Ölçüm ölçüsü'](alerts-unified-log.md) seçeneği. Bu seçeneği kullanarak, ayrı bir uyarı örnekleri grubu tanımlanan alana göre oluşturulur. Ardından Senaryo 2'de, 'Bilgisayar-01' ve 'Bilgisayar-02' için ayrı bir uyarı örneği oluşturulur. Bildirim 'Bilgisayar-02' için normal olarak harekete olmaya devam eder ancak senaryoda açıklanan eylemi kural ile yalnızca bildirim için 'Bilgisayar-01' atlanması.

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>SSS

* S. Bir eylem kuralını yapılandırırken, böylece yinelenen bildirimler kaçınabilirim eylem çakışan tüm olası kuralları görmek istiyorum. Bunu yapmak mümkündür?

    A. Bir eylem kuralını yapılandırırken bir kapsam tanımlama sonra aynı kapsamda (varsa) çakışma eylem kurallarının bir listesini görebilirsiniz. Bu çakışma şunlardan biri olabilir:
    * Tam bir eşleşme: Örneğin, aynı abonelikte tanımladığınız eylem kural ve çakışan Eylem kuralı vardır.
    * Bir alt kümesi: Örneğin, tanımladığınız bir eylem kuralı bir abonelikte olduğundan ve abonelik içindeki kaynak grubu çakışan eylem kural etkindir.
    * Bir üst kümesidir: Örneğin, tanımladığınız eylemi bir kaynak grubu üzerinde kuralıdır ve kaynak grubu içeren abonelik üzerinde çakışan eylem kuralıdır.
    * Bir kesişime: Örneğin, tanımladığınız eylem 'VM1' ve 'VM2' kuralıdır ve çakışan eylem 'VM2' ve 'VM3' kuralıdır.

    ![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-overlapping.png)

* S. Bir uyarı kuralı yapılandırma olsa da mümkün olup olmadığını zaten işlem kuralları, tanımlanan bilmek tanımlama uyarı kuralı üzerinde hareket?

    A. Uyarı, kural için hedef kaynak tanımladıktan sonra aynı kapsamda (varsa) üzerinde davranan 'Actions' bölümünde 'Görünüm eylemleri yapılandırdığına' ı tıklatarak eylem kurallarının listesini görebilirsiniz. Bu liste, aşağıdaki senaryolar için kapsama göre doldurulur:
    * Tam bir eşleşme: Örneğin, tanımladığınız bir uyarı kuralı ve Eylem kuralı aynı abonelikte olan.
    * Bir alt kümesi: Örneğin, tanımladığınız bir uyarı kuralı bir abonelikte olduğundan ve abonelik içindeki kaynak grubuna eylem kural etkindir.
    * Bir üst kümesidir: Örneğin, tanımladığınız bir uyarı kuralı olan bir kaynak grubu ve kaynak grubu içeren abonelik üzerinde eylem kuralıdır.
    * Bir kesişime: Örneğin, tanımladığınız bir uyarı kuralı durumda 'VM1' ve 'VM2' ve 'VM2' ve 'VM3' eylem kuralıdır.
    
    ![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

* S. Bir eylem kuralı tarafından gizlenen uyarılar görebilir miyim?

    A. İçinde [uyarılar liste sayfası](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), çağrılan 'gizleme Status' seçilebilir ek bir sütun yok. Bildirim uyarı örneği için engellendi, listeden durumu gösterebilir.

    ![Gizlenen bir uyarı örneği](media/alerts-action-rules/action-rules-suppressed-alerts.png)

* S. Varsa bir eylem grubu ile bir eylem kuralı ve başka bir gizleme etkin aynı kapsamda ne olur?

    A. **Gizleme her zaman aynı kapsamına göre önceliklidir**.

* S. İki ayrı bir eylem kurallarında izlenen bir kaynak varsa ne olur? Bir veya iki bildirim alabilir miyim? Bu senaryoda örnek 'VM2':

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1'
      action rule 'AR2' defined for 'VM2' and 'VM3' with action group 'AG1'

    A. 'VM1' ve 'VM3' her uyarı için bir kez 'AG1' eylem grubu tetiklenen. Her uyarı için 'VM2' üzerinde 'AG1' eylem grubu iki kez tetiklenmesi (**eylem kuralları değil XML'deki yinelenen Eylemler**). 

* S. İki ayrı bir eylem kurallarında izlenen bir kaynak sahibim ve bir hatayla gizleme için başka bir eylem gerektirdiğinden ne olur? Örneğin, bu senaryoda ' VM2':

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1' 
      action rule 'AR2' defined for 'VM2' and 'VM3' with suppression

    A. 'VM1' her uyarı için bir kez 'AG1' eylem grubu tetiklenen. Eylemler ve 'VM2' ve 'VM3' her uyarı için bildirimleri gizlenir. 

* S. Bir uyarı kuralı ve farklı eylem grupları çağırma aynı kaynak için tanımlanan bir eylem kuralı varsa ne olur? Örneğin, bu senaryoda ' VM1':

      alert rule  'rule1' on          'VM1' with action group 'AG2'
      action rule 'AR1'   defined for 'VM1' with action group 'AG1' 
 
    A. 'VM1' her uyarı için bir kez 'AG1' eylem grubu tetiklenen. 'Bağlanma1' uyarı kuralı tetiklendiğinde, ayrıca 'AG2' de tetikler. **Eylem grupları tanımlanan eylem kuralların ve uyarı kuralları işletmek bağımsız olarak, hiçbir yinelenenleri kaldırma ile**. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki uyarılar hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
