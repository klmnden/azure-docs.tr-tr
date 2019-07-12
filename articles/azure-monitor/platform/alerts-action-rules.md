---
title: Azure İzleyici uyarılar için Eylem kuralları
description: Azure İzleyici eylem kurallarında nedir ve nasıl yapılandırılacağı ve yönetileceğine anlama.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: df069ee398ea2937f03765b10576061b5e541390
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67656730"
---
# <a name="action-rules-preview"></a>Eylem kuralları (Önizleme)

Eylem kuralları tanımlayın veya herhangi bir Azure Resource Manager kapsamı (Azure aboneliği, kaynak grubu veya hedef kaynak) eylemlerini Gizle yardımcı olur. Uyarı örneklerinin belirli alt, dar Yardım üzerinde yapmak istediğiniz çeşitli filtreleri sahiptirler.

## <a name="why-and-when-should-you-use-action-rules"></a>Neden ve ne zaman eylem kurallarını kullanmalısınız?

### <a name="suppression-of-alerts"></a>Uyarı gizleme

Uyarıları oluşturan bildirimleri göstermemeye kullanışlı olduğu birçok senaryo vardır. Bu senaryolar aralığından planlı bakım penceresi sırasında gizleme saatlerde gizleme için. Örneğin, sorumlu takım **ContosoVM** gelecek hafta için Uyarı bildirimlerini bastır çünkü istediği **ContosoVM** planlı bakım yapılıyor. 

Takım, yapılandırılan her bir uyarı kuralı devre dışı bırakabilirsiniz ancak **ContosoVM** el ile (ve Bakım sonra yeniden etkinleştirme), basit bir işlem değildir. Eylem kuralları uyarı gizleme uygun ölçekte esnek bir şekilde gizleme yapılandırın olanağı tanımlamanıza yardımcı olmak. Önceki örnekte, takım bir eylem kural tanımlayabilirsiniz **ContosoVM** , hafta için tüm uyarı bildirimleri engeller.


### <a name="actions-at-scale"></a>Uygun ölçekte eylemleri

Uyarı kuralları, bir uyarı oluşturulduğunda tetikleyen bir eylem grubu tanımlamanıza yardımcı olsa da, müşteriler genellikle operations kapsamlarına arasında ortak bir eylem grubu sahiptir. Örneğin, kaynak grubu için sorumlu bir ekip **ContosoRG** aynı eylem grubu içinde tanımlanan tüm uyarı kuralları için büyük olasılıkla tanımlayacak **ContosoRG**. 

İşlem kuralları, bu işlemi basitleştireceksiniz yardımcı olur. Uygun ölçekte eylemleri tanımlayarak bir eylem grubu için yapılandırılmış kapsamda oluşturulan herhangi bir uyarı tetiklenebilir. Önceki örnekte, takım bir eylem kural tanımlayabilirsiniz **ContosoRG** aynı eylem grubu içinde oluşturulan tüm uyarılar için tetikleme.

> [!NOTE]
> İşlem kuralları, şu anda Azure hizmet durumu uyarıları için geçerli değildir.

## <a name="configuring-an-action-rule"></a>Bir eylem kuralı yapılandırma

Özellik seçerek erişebilirsiniz **işlemleri yönetmenizi** gelen **uyarılar** Azure İzleyici'de giriş sayfası. Ardından, **eylem kuralları (Önizleme)** . Kuralları seçerek erişebilirsiniz **eylem kuralları (Önizleme)** uyarılar için giriş sayfasının panosundan.

![Azure İzleyici giriş sayfasından eylemi kuralları](media/alerts-action-rules/action-rules-landing-page.png)

Seçin **+ yeni eylem kural**. 

![Yeni Eylem Kuralı Ekle](media/alerts-action-rules/action-rules-new-rule.png)

Alternatif olarak, bir uyarı kuralını yapılandırırken, bir eylem kuralı oluşturabilirsiniz.

![Yeni Eylem Kuralı Ekle](media/alerts-action-rules/action-rules-alert-rule.png)

Şimdi eylem kuralları oluşturmak için akış sayfası görmeniz gerekir. Aşağıdaki öğeler yapılandırın: 

![Yeni Eylem kural oluşturma akış](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>`Scope`

Kapsam (Azure aboneliği, kaynak grubu veya hedef kaynak) seçin. Ayrıca çoklu tek bir abonelik kapsamlarda birleşimi seçim.

![Eylem Kural kapsamı](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filtre ölçütü

Ayrıca, bunları belirli bir alt kümesini uyarıları aşağı daraltmak için filtreleri tanımlayabilirsiniz. 

Kullanılabilir filtreleri şunlardır: 

* **Önem derecesi**: Bir veya daha fazla uyarı önem dereceleri seçmek için seçenek. **Önem derecesi = Sev1** Sev1 için tüm uyarılar için eylem kuralın geçerli olduğu anlamına gelir.
* **Hizmet izleme**: Kaynak izleme hizmetini temel alan filtre. Bu filtre, aynı zamanda çoklu seçim. Örneğin, **İzleyicisi hizmeti "Application Insights" =** Eylem kuralı tüm Application Insights tabanlı uyarılar için uygun olduğu anlamına gelir.
* **Kaynak türü**:  Belirli kaynak türüne göre filtre. Bu filtre, aynı zamanda çoklu seçim. Örneğin, **kaynak türü "Sanal makineler" =** Eylem kuralı tüm sanal makineler için geçerli olduğu anlamına gelir.
* **Uyarı kuralı kimliği**: Özel uyarı kuralları için uyarı kuralı Resource Manager Kimliğini kullanarak filtre seçeneği.
* **İzleme koşulu**:  Ya da uyarı örneği için bir filtre **Fired** veya **çözümlenmiş** izleme koşulu olarak.
* **Açıklama**: Uyarı kuralının bir parçası tanımlanan bir açıklama dizesi eşleştirin tanımlayan bir regex (normal ifade) ile eşleşir. Örneğin, **açıklama içeren 'prod'** "prod" dizesini içeren tüm uyarıları açıklamalarının içinde eşleşir.
* **Uyarı bağlamı (yükü)** : Bir dize eşleşmesini bir uyarının yükünün uyarı bağlamı alanlara göre tanımlayan bir regex eşleşme. Örneğin, **bağlam (yükü) içeren 'Bilgisayar-01' uyarı** olan yüklerini "Bilgisayar-01." dizesi içeren tüm uyarıları eşleşir

Bu filtreler, başka bir birlikte uygulanır. Örneğin ayarlarsanız **kaynak türü ' sanal makineleri =** ve **önem ' Sev0 =** , tüm filtreledikten sonra **Sev0** Vm'lerinizi yalnızca ilgili uyarılar. 

![Eylem kural filtreleri](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Gizleme veya eylem grubu yapılandırma

Ardından, uyarı gizleme veya eylem grubu desteği için Eylem kuralı yapılandırın. Her ikisini seçemezsiniz. Yapılandırma, filtreleri ve önceden tanımlanmış kapsamını eşleşen tüm uyarı örneklerinde çalışır.

#### <a name="suppression"></a>Gizleme

Seçerseniz **gizleme**, Eylemler ve bildirimleri bastırma süresince yapılandırın. Aşağıdaki seçeneklerden birini belirleyin:
* **Bugünden itibaren (her zaman)** : Süresiz olarak tüm bildirimleri engeller.
* **Zamanlanan tarihte**: Bildirimler, sınırlı bir süre içinde bastırır.
* **Bir yineleme ile**: Yinelenen bir günlük, haftalık veya aylık zamanlama bildirimlerinde bastırır.

![Eylem kural gizleme](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>eylem grubu

Seçerseniz **eylem grubu** geçiş ya da var olan bir eylem grubu ekleyin veya yeni bir tane oluşturun. 

> [!NOTE]
> Yalnızca bir eylem grubu Eylem kuralı ile ilişkilendirebilirsiniz.

![Eylem grubu seçerek yeni bir eylem kuralı oluşturma veya ekleme](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Eylem kural ayrıntıları

Son olarak, aşağıdaki eylemi kuralın ayrıntılarını yapılandırın:
* Ad
* Kaynak grubu içinde kaydedilir
* Açıklama 

## <a name="example-scenarios"></a>Örnek senaryolar

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Senaryo 1: Önem derecesine göre uyarıları gizleme

Contoso tüm Sev4 uyarılar abonelik içindeki tüm sanal makineler için bildirimleri bastır isteyen **ContosoSub** her hafta sonu.

**Çözüm:** Bir eylem kuralı oluşturun:
* Kapsam = **ContosoSub**
* Filtreler
    * Önem derecesi = **Sev4**
    * Kaynak türü = **sanal makineler**
* Haftalık olarak ayarlamak yineleme ile gizleme ve **Cumartesi** ve **Pazar** işaretli

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Senaryo 2: Uyarı bağlamı (yükü) tabanlı uyarı gizleme

Contoso tüm günlük için oluşturulan uyarılar için bildirimleri bastır isteyen **bilgisayar-01** içinde **ContosoSub** süresiz olarak olan giderek bakım yapmamanın.

**Çözüm:** Bir eylem kuralı oluşturun:
* Kapsam = **ContosoSub**
* Filtreler
    * Hizmet izleme = **Log Analytics**
    * Bağlam (yükü) içeren uyarı **bilgisayar-01**
* Gizleme kümesine **bugünden itibaren (her zaman)**

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Senaryo 3: Bir kaynak grubu tanımlanan eylem grubu

Contoso tanımlanmış [abonelik düzeyinde ölçüm Uyarısı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor). Ancak özellikle kaynak grubundan oluşturulan uyarılar için tetikleme eylemleri tanımlamak ister **ContosoRG**.

**Çözüm:** Bir eylem kuralı oluşturun:
* Kapsam = **ContosoRG**
* Filtre
* Eylem grubu kümesine **ContosoActionGroup**

> [!NOTE]
> *Eylem grupları Eylem kuralların tanımlanır ve uyarı kuralları bağımsız olarak, hiçbir yinelenenleri kaldırma ile çalışır.* Senaryoyu önceki sürümlerinde, bir eylem grubu için uyarı kuralı tanımlanır, eylem kuralda tanımlanan eylem grubu ile birlikte tetikler. 

## <a name="managing-your-action-rules"></a>Eylem kurallarınızı yönetme

Görüntüleyebilir ve eylem kurallarınızı listesi görünümünden yönetebilir:

![Eylem kuralları listesi görünümü](media/alerts-action-rules/action-rules-list-view.png)

Buradan, etkinleştirme, devre dışı bırakın veya bunları yanındaki onay kutusunu seçerek eylemi kurallarına uygun ölçekte silme. Bir eylem kuralı seçtiğinizde, alt yapılandırma sayfası açılır. Sayfa eylem kuralın tanımını güncelleştirin ve etkinleştirme veya devre dışı yardımcı olur.

## <a name="best-practices"></a>En iyi uygulamalar

Günlük ile oluşturduğunuz uyarıları [sonuç sayısı](alerts-unified-log.md) (birden fazla bilgisayara yayılabilir) tüm arama sonucunu kullanarak tek bir uyarı örneği oluşturma seçeneği. Bu senaryoda, bir eylem kuralı kullanıyorsa **uyarı bağlamı (yükü)** filtre, bunu görür uyarı örneğinde bir eşleşme var olduğu sürece. Senaryo 2'de açıklanan daha önce oluşturulan günlük uyarısı için arama sonuçlarını hem de içeriyorsa **bilgisayar-01** ve **bilgisayar-02**, tüm bildirim bastırılır. Oluşturulan bildirim yoktur **bilgisayar-02** hiç.

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Günlük uyarıları eylem kuralları ile en iyi şekilde kullanmak için günlüğü uyarıları oluşturma [ölçüm ölçüsü](alerts-unified-log.md) seçeneği. Uyarı ayrı örneklerde, tanımlanmış bir grup alana göre bu seçeneği tarafından oluşturulur. Senaryo 2'de için ayrı bir uyarı örneklerinin daha sonra oluşturulan **bilgisayar-01** ve **bilgisayar-02**. Eylem kuralı nedeniyle açıklanan senaryoda, yalnızca bildirim için **bilgisayar-01** bastırılır. Bildirim için **bilgisayar-02** yangın normal olarak devam eder.

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>SSS

### <a name="while-im-configuring-an-action-rule-id-like-to-see-all-the-possible-overlapping-action-rules-so-that-i-avoid-duplicate-notifications-is-it-possible-to-do-that"></a>Bir eylem kuralı yapılandırma, ancak birbirinin aynısı bildirimler kaçınabilirim eylem kuralları, çakışan tüm olası öğrenmek istiyorum. Bunu yapmak mümkün mü?

Bir eylem kuralı yapılandırırken bir kapsam tanımladıktan sonra aynı kapsamda (varsa) çakışan eylem kurallarının bir listesini görebilirsiniz. Bu çakışma şunlardan biri olabilir:

* Tam eşleşme: Örneğin, tanımlayacağınız eylem kural ve çakışan Eylem kuralı aynı abonelikte yararlanabilirsiniz.
* Alt küme: Örneğin, bir abonelikte, tanımlayacağınız Eylem kuralı olan ve abonelik içindeki kaynak grubu çakışan eylem kural etkindir.
* Üst küme: Örneğin, tanımlama eylemi bir kaynak grubu üzerinde kuralıdır ve kaynak grubu içeren abonelik üzerinde çakışan eylem kuralıdır.
* Kesişim: Tanımlama eylem kuralının konduğu gibi **VM1** ve **VM2**, ve çakışan eylem kuralının konduğu **VM2** ve **VM3**.

![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-overlapping.png)

### <a name="while-im-configuring-an-alert-rule-is-it-possible-to-know-if-there-are-already-action-rules-defined-that-might-act-on-the-alert-rule-im-defining"></a>Bir uyarı kuralı yapılandırma, ancak tanımlama uyarı kuralı varsa zaten işlem kuralları, tanımlanan bilmeniz mümkün hareket mi?

Uyarı, kural için hedef kaynak tanımladıktan sonra aynı kapsamda (varsa) seçerek davranan eylem kurallarının listesini görebilirsiniz **görünümü yapılandırılmış eylemleri** altında **eylemleri** bölümü. Bu liste, aşağıdaki senaryolar için kapsama göre doldurulur:

* Tam eşleşme: Örneğin, aynı abonelikte uyarı kuralı, tanımlama ve eylem kural vardır.
* Alt küme: Örneğin, tanımlama uyarı kuralı bir abonelikte olduğundan ve abonelik içindeki kaynak grubuna eylem kural etkindir.
* Üst küme: Örneğin, tanımlama uyarı kuralı olan bir kaynak grubu ve kaynak grubu içeren abonelik üzerinde eylem kuralıdır.
* Kesişim: Tanımlama uyarı kuralının konduğu gibi **VM1** ve **VM2**, ve eylem kuralının konduğu **VM2** ve **VM3**.
    
![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

### <a name="can-i-see-the-alerts-that-have-been-suppressed-by-an-action-rule"></a>Bir eylem kuralı tarafından gizlenen uyarılar görebilir miyim?

İçinde [uyarılar liste sayfası](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), adlı ek bir sütun seçebileceğiniz **gizleme durumu**. Bildirim uyarı örneği için engellendi, listeden durumu gösterebilir.

![Gizlenen bir uyarı örneği](media/alerts-action-rules/action-rules-suppressed-alerts.png)

### <a name="if-theres-an-action-rule-with-an-action-group-and-another-with-suppression-active-on-the-same-scope-what-happens"></a>Varsa bir eylem grubu ile bir eylem kuralı ve başka bir gizleme etkin aynı kapsamda ne olur?

Gizleme her zaman aynı kapsamına göre önceliklidir.

### <a name="what-happens-if-i-have-a-resource-thats-monitored-in-two-separate-action-rules-do-i-get-one-or-two-notifications-for-example-vm2-in-the-following-scenario"></a>İki ayrı bir eylem kurallarında izlenen bir kaynak varsa ne olur? Bir veya iki bildirim alabilir miyim? Örneğin, **VM2** aşağıdaki senaryoda:

      "action rule AR1 defined for VM1 and VM2 with action group AG1
      action rule AR2 defined for VM2 and VM3 with action group AG1"

VM1 ve VM3 her uyarı için bir kez eylem grubu AG1 tetiklenen. Her uyarı için **VM2**, eylem grubu AG1 tetiklenen iki kez çünkü eylemi kurallarına Eylemler kaldırmayın. 

### <a name="what-happens-if-i-have-a-resource-monitored-in-two-separate-action-rules-and-one-calls-for-action-while-another-for-suppression-for-example-vm2-in-the-following-scenario"></a>İki ayrı bir eylem kurallarında izlenen bir kaynak sahibim ve bir hatayla gizleme için başka bir eylem gerektirdiğinden ne olur? Örneğin, **VM2** aşağıdaki senaryoda:

      "action rule AR1 defined for VM1 and VM2 with action group AG1 
      action rule AR2 defined for VM2 and VM3 with suppression"

VM1'in üzerinde her uyarı için bir kez eylem grubu AG1 tetiklenen. Eylemler ve bildirimleri VM2 ve VM3 her uyarı için gizlenir. 

### <a name="what-happens-if-i-have-an-alert-rule-and-an-action-rule-defined-for-the-same-resource-calling-different-action-groups-for-example-vm1-in-the-following-scenario"></a>Bir uyarı kuralı ve farklı eylem grupları çağırma aynı kaynak için tanımlanan bir eylem kuralı varsa ne olur? Örneğin, **VM1** aşağıdaki senaryoda:

      "alert rule rule1 on VM1 with action group AG2
      action rule AR1 defined for VM1 with action group AG1" 
 
VM1'in üzerinde her uyarı için bir kez eylem grubu AG1 tetiklenen. Uyarı kuralı "bağlanma1" tetiklendiğinde, ayrıca AG2 ayrıca tetikler. Eylem grupları Eylem kuralların tanımlanır ve uyarı kuralları bağımsız olarak, hiçbir yinelenenleri kaldırma ile çalışır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki uyarılar hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
