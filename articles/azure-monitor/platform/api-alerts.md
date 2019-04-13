---
title: Log Analytics uyarı REST API kullanma
description: Log Analytics uyarı REST API oluşturma ve Log Analytics parçası olan Log Analytics'teki uyarılar, yönetmenizi sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve birkaç örnek ayrıntılarını sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2018
ms.author: bwren
ms.openlocfilehash: bee64909c7f3b295691ef1cb1840424aa7e3fe49
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549721"
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Oluşturma ve REST API ile Log analytics'teki uyarı kurallarını yönet
Log Analytics uyarı REST API oluşturma ve Log analytics'teki uyarılar yönetmenize olanak sağlar.  Bu makalede, farklı işlemler gerçekleştirmek için API ve birkaç örnek ayrıntılarını sağlar.

Log Analytics arama REST API, RESTful olduğu ve Azure Resource Manager REST API aracılığıyla erişilebilir. Bu belgede, API'yi kullanarak bir PowerShell komut satırı burada erişilen örnekler bulabilirsiniz [ARMClient](https://github.com/projectkudu/ARMClient), Azure Resource Manager API'si çağırma basitleştiren bir açık kaynak komut satırı aracı. PowerShell ile ARMClient ve Log Analytics arama API'sine erişmek için birçok seçenekten birini kullanılır. Bu araçlarla, Log Analytics çalışma alanları çağrı yapmak ve bunların içindeki arama komutları gerçekleştirmek için RESTful Azure Resource Manager API'si kullanabilir. API, birçok farklı şekilde, program aracılığıyla arama sonuçlarını kullanmanıza olanak sağlayan, arama sonuçları JSON biçiminde çıkarır.

## <a name="prerequisites"></a>Önkoşullar
Şu anda ile kayıtlı bir aramayı Log analytics'te uyarıları yalnızca oluşturulabilir.  Başvurabilirsiniz [Log Search REST API'sine](../../azure-monitor/log-query/log-query-overview.md) daha fazla bilgi için.

## <a name="schedules"></a>Zamanlamalar
Kayıtlı bir aramayı bir veya daha fazla zamanlama olabilir. Ne sıklıkta arama çalıştırma ve zaman aralığı üzerinde ölçütler tanımlanmıştır, zamanlamayı tanımlar.
Zamanlamaları aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Interval |Arama ne kadar sıklıkla çalışır. Birkaç dakika içinde ölçülür. |
| QueryTimeSpan |Zaman aralığı üzerinde ölçüt değerlendirme. Aralık değerinden büyük veya eşit olmalıdır. Birkaç dakika içinde ölçülür. |
| Sürüm |API sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

Örneğin, 15 dakikalık bir aralığı ve bir zaman aralığı 30 dakika ile olay sorgusu göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve doğru üzerinden çözmek ölçütleri devam bir uyarı tetiklenecek 30 dakikalık aralık.

### <a name="retrieving-schedules"></a>Zamanlamalar alınıyor
Kayıtlı arama için tüm zamanlamalar almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Get yöntemi, belirli bir zamanlama için kayıtlı bir aramayı almak için bir zamanlama kimliği ile kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Bir zamanlama için bir örnek yanıt aşağıdadır.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/sampleRG/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15,
            "Enabled": true,
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Bir zamanlama oluşturma
Put yöntemi yeni bir zamanlama oluşturmak için bir benzersiz zamanlama kimliği ile kullanın.  Kayıtlı aramalar farklı ile ilişkili olsalar bile iki zamanlamaları aynı Kimliğe sahip olduğunu unutmayın.  Log Analytics konsolda bir zamanlama oluşturmak, zamanlama kimliği için bir GUID oluşturulur

> [!NOTE]
> Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Bir zamanlama düzenleme
Bu zamanlamayı değiştirmek için aynı kayıtlı arama için mevcut bir zamanlama kimliği ile Put yöntemini kullanma; Aşağıdaki örnekte, zamanlama devre dışı bırakıldı. İstek gövdesi içermelidir *etag* zamanlamasını.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Enabled':'false' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Zamanlama siliniyor
Delete yöntemi bir zamanlama kimliği ile bir zamanlamayı silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Eylemler
Bir zamanlama birden fazla eylem olabilir. Posta gönderme veya bir runbook başlatma gibi gerçekleştirmek için bir veya daha fazla işlem bir eylem tanımlayabilir veya ne zaman bir aramanın sonuçları bazı ölçütlerle eşleşen belirleyen bir eşiği tanımlayabilir.  Eşiğine ulaşıldığında işlemleri gerçekleştirilir böylece bazı eylemler her ikisi de tanımlayın.

Tüm eylemler aşağıdaki tabloda özelliklere sahiptir.  Farklı uyarı türleri aşağıda açıklanan farklı ek özellikler vardır.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |Eylem türü.  Şu anda uyarı ve Web kancası olası değerler şunlardır. |
| `Name` |Uyarı görünen adı. |
| `Version` |API sürümü kullanılıyor.  Şu anda, bu her zaman 1 olarak ayarlanması gerekir. |

### <a name="retrieving-actions"></a>Eylemleri alınıyor

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için Eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Bir zamanlama için tüm eylemleri almak için Get yöntemini kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Get yöntemi, bir zamanlama için belirli bir eylemi almak için eylem kimliği ile kullanın.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Oluşturma veya düzenleme eylemleri
Yeni bir eylem oluşturmak için zamanlama için benzersiz olan bir eylem kimliği ile Put yöntemini kullanın.  Log Analytics konsolunda bir eylem oluşturduğunuzda, eylem kimliği için bir GUID değeridir.

> [!NOTE]
> Tüm kayıtlı aramalar, çizelgeler ve günlük analizi API'si ile oluşturulan eylem adını, küçük olmalıdır.

Bu zamanlamayı değiştirmek için var olan bir eylem kimliği aynı kayıtlı arama için Put yöntemini kullanın.  İstek gövdesi, zamanlama etag'i içermesi gerekir.

Aşağıdaki bölümlerde bu örnekler sağlanır, yeni bir eylem oluşturmak için istek biçimi eylem türüne göre değişir.

### <a name="deleting-actions"></a>Eylemler siliniyor

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için Eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Delete yöntemi eylem kimliği ile bir eylemi silmek için kullanın.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Uyarı eylemleri
Bir ve yalnızca bir uyarı eylemi bir zamanlamaya sahip olmalıdır.  Uyarı eylemleri bir veya daha fazla aşağıdaki tabloda bölümler var.  Her daha aşağıda ayrıntılı olarak açıklanmıştır.

| Section | Açıklama | Kullanım |
|:--- |:--- |:--- |
| Eşik |Eylem çalıştırıldığında ölçütleri.| Her uyarı için önce veya sonra Azure'a genişletilmiş olan gereklidir. |
| Severity |Uyarı tetiklendiğinde sınıflandırmak için kullanılan etiketi belirtin.| Her uyarı için önce veya sonra Azure'a genişletilmiş olan gereklidir. |
| Gizle |Uyarı bildirimleri durdurmak için seçenek. | Her uyarı için isteğe bağlı önce veya sonra Azure'a genişletildi. |
| Eylem Grupları |Azure burada gerekli Eylemler belirtilmiştir, e-postalar, SMSs, sesli aramalar, Web kancaları, Otomasyon runbook'ları, ITSM bağlayıcılar, vb. gibi - ActionGroup kimlikleri.| Uyarılar Azure'a genişletilmiş olan sonra gerekli|
| Eylemleri Özelleştirin|ActionGroup select eylemler için standart çıktı değiştirme| Her uyarı için isteğe bağlı kullanılabilir sonra uyarılar Azure'a genişletilir. |
| EmailNotification |Birden çok alıcıya e-posta gönderin. | Uyarılar Azure'a uzatıldıysa, gerekli değildir|
| Düzeltme |Tanımlanan sorunu düzeltme girişiminde bulunmak üzere Azure Otomasyonu'ndaki bir runbook'u başlatın. |Uyarılar Azure'a uzatıldıysa, gerekli değildir|
| Web kancası eylemleri | JSON olarak istenen hizmetine gelen uyarılar, veri gönderme |Uyarılar Azure'a uzatıldıysa, gerekli değildir|

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md).

#### <a name="thresholds"></a>Eşikler
Bir uyarı eylemi, yalnızca tek bir eşik değeri olması gerekir.  Kayıtlı arama sonuçlarını bu arama ile ilişkili bir eylem Eşikte eşleştiğinde, ardından bu eylemi diğer tüm işlemler çalıştırılır.  Böylece eşikleri içermeyen diğer tür Eylemler ile kullanılabilmesi için bir eylem yalnızca bir eşiği de içerebilir.

Eşikleri aşağıdaki tabloda özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| `Operator` |Eşik karşılaştırması için işleci. <br> gt büyük = <br> lt = kısa |
| `Value` |Eşik değeri. |

Örneğin, bir olay sorgusu ile 15 dakika, 30 dakikalık bir zaman aralığı ve bir eşik 10'dan büyük bir aralıkta göz önünde bulundurun. Bu durumda, sorgu 15 dakikada bir çalışır ve 30 dakikalık bir aralığı oluşturulan 10 olayları döndürdüyse bir uyarı tetiklenmesi.

Örnek yanıt yalnızca bir eşik ile bir eylem için aşağıda verilmiştir.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Put yöntemi yeni bir eşik eylem için bir zamanlama oluşturmak için bir benzersiz bir eylem kimliği ile kullanın.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Put yöntemi, bir zamanlama için bir eşik eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="severity"></a>Severity
Log Analytics, daha kolay yönetim ve Önceliklendirme izin vermek için kategoriler halinde uyarılarınızı sınıflandırmak sağlar. Tanımlanan uyarı önem derecesi: bilgilendirme, uyarı ve kritik. Bunlar Azure Uyarıları ' normalleştirilmiş önem derecesi ölçeğini eşlenir:

|Log Analytics önem düzeyi  |Azure Uyarıları önem düzeyi  |
|---------|---------|
|`critical` |Önem Derecesi 0|
|`warning` |Önem Derecesi 1|
|`informational` | Önem Derecesi 2|

Örnek yanıt yalnızca eşiğini ve önem derecesine sahip bir eylem için aşağıda verilmiştir. 

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Severity": "critical",
        "Version": 1    }

Önem derecesine sahip yeni bir eylem için bir zamanlama oluşturmak için benzersiz işlem Kimliğine sahip Put yöntemini kullanın.  

    $thresholdWithSevJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

Put yöntemi, bir zamanlama için bir önem derecesi eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $thresholdWithSevJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

#### <a name="suppress"></a>Gizle
Log Analytics uyarı eşiği karşılanmadığından veya aşıldığından her zaman ateşlenir sorgu tabanlı. Sorguda kapsanan mantıksal bağlı olarak, bu uyarı bir dizi aralıkları için tetiklendi neden olabilir ve bu nedenle bildirimleri de gönderilen sürekli olarak. Bu senaryoyu engellemek için bir kullanıcı bir görünürlüğe süre bildirim uyarı kuralı ikinci kez tetiklenir önce beklenecek Log Analytics söyleyen gösterme seçeneği ayarlayabilirsiniz. Dolayısıyla bastır 30 dakika boyunca; ayarlayın ardından uyarı ilk defa at ve yapılandırılmış bildirimler gönderin. Ancak daha sonra 30 uyarı kuralı için bildirimi yeniden kullanılmadan önce dakika bekleyin. Ara dönemde uyarı kuralı çalışmaya devam edecek - yalnızca bildirim için uyarı kuralı bu dönemde harekete kaç kez bakılmaksızın belirtilen zaman Log Analytics tarafından bastırılır.

Log Analytics uyarı kuralı kullanarak belirtilen özelliği Gizle *azaltma* değeri ve gizleme dönem kullanarak *Dakika Cinsiden Süre* değeri.

Aşağıdaki örnek yanıt için yalnızca bir eşik, önem derecesi, eylem ve özelliğini Gizle

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Throttling": {
          "DurationInMinutes": 30
        },
        "Severity": "critical",
        "Version": 1    }

Önem derecesine sahip yeni bir eylem için bir zamanlama oluşturmak için benzersiz işlem Kimliğine sahip Put yöntemini kullanın.  

    $AlertSuppressJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

Put yöntemi, bir zamanlama için bir önem derecesi eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AlertSuppressJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Throttling': { 'DurationInMinutes': 30 },'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myalert?api-version=2015-03-20 $AlertSuppressJson

#### <a name="action-groups"></a>Eylem Grupları
Azure'daki tüm uyarılar eylemlerini işleyen varsayılan bir mekanizma olarak eylem grubu kullanın. Eylem grubu ile bir kez eylemleri belirtin ve birden çok uyarı - eylem grubuna Azure genelinde ilişkilendirin. Tekrar tekrar aynı eylemleri tekrar tekrar bildirme gerek kalmadan. Eylem grupları, birden fazla eylem - e-posta, SMS, sesli arama, ITSM bağlantısı, Otomasyon Runbook'u, Web kancası URI ve benzeri destekler. 

Uyarılarını - Azure'a genişletilmiş kullanıcılar için bir zamanlama artık bir uyarı oluşturabilmek için eşik yanı sıra, geçirilen eylem grubu ayrıntıları olması gerekir. E-posta ayrıntıları, Web kancası URL'leri, Runbook Otomasyon ayrıntıları ve diğer eylemler olması gereken bir eylem grubu ilk önce bir uyarı; oluşturma tarafta tanımlanan bir izin oluşturabilir [Azure İzleyici'eylem grubundan](../../azure-monitor/platform/action-groups.md) portalı veya [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Bir uyarı eylem grubu ilişkisini eklemek için uyarı tanımında eylem grubu benzersiz Azure Resource Manager Kimliğini belirtin. Bir örnek resimde, aşağıda verilmiştir:

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini, zaten mevcut olan eylem grubu için bir zamanlama ilişkilendirmek için bir benzersiz bir eylem kimliği ile kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="customize-actions"></a>Eylemleri Özelleştirin
Varsayılan eylemler tarafından standart şablon ve bildirimler için biçim izleyin. Ancak Eylem grupları tarafından kontrol edilir olsa bile, kullanıcı bazı eylemler özelleştirebilirsiniz. Şu anda, e-posta konusu ve Web kancası yükü özelleştirme mümkündür.

##### <a name="customize-e-mail-subject-for-action-group"></a>Eylem grubu için e-posta konusu özelleştirme
Varsayılan olarak, uyarılar için e-posta konusu şöyledir: Uyarı bildirimini `<AlertName>` için `<WorkspaceName>`. Ancak, belirli sözcükleri ya da kolayca filtre kuralları, gelen kutunuzdaki görevlendirmek olanak tanımak için etiketleri - böylece bu, özelleştirilebilir. Özelleştirme e-posta üst bilgisi ayrıntıları aşağıdaki örnekte olduğu gibi ActionGroup ayrıntılarını göndermeniz gerekir.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini bir benzersiz bir eylem kimliği ile zaten mevcut olan eylem grubu için bir zamanlama özelleştirme ilişkilendirmek için kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

##### <a name="customize-webhook-payload-for-action-group"></a>Eylem grubu için Web kancası yükü özelleştirme
Varsayılan olarak, log analytics için eylem grubu aracılığıyla gönderilen Web kancası sabit yapısı vardır. Ancak, Web kancası uç noktası gereksinimlerini karşılamak için desteklenen belirli değişkenlerini kullanarak bir JSON yükü özelleştirebilirsiniz. Daha fazla bilgi için [günlük uyarı kuralları için Web kancası eylemi](../../azure-monitor/platform/alerts-log-webhook.md). 

ActionGroup ayrıntılarını birlikte göndermek ve Özelleştir Web kancası ayrıntıları gereken tüm Web kancası eylem grubu içinde; belirtilen URI uygulanabilir Aşağıdaki örnekte olduğu gibi.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a description here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ],
          "CustomWebhookPayload": "{\"field1\":\"value1\",\"field2\":\"value2\"}",
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Put yöntemini bir benzersiz bir eylem kimliği ile zaten mevcut olan eylem grubu için bir zamanlama özelleştirme ilişkilendirmek için kullanın.  Kullanım örneği bir gösterimi aşağıda verilmiştir.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Put yöntemi ile var olan bir eylem kimliği için bir zamanlama ilişkili bir eylem grubu değiştirmek için kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="email-notification"></a>E-posta Bildirimi
E-posta bildirimleri, bir veya daha fazla alıcıya e-posta gönderin.  Bunlar aşağıdaki tabloda özelliklerini içerir.

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için e-posta bildirimi gibi eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).
   

| Özellik | Açıklama |
|:--- |:--- |
| Alıcılar |E-posta adresleri listesi. |
| Özne |E-posta konusu. |
| Ek |Bu her zaman "None." değerine sahip şekilde ekleri şu anda, desteklenmez |

Örnek yanıt bir eşik ile bir e-posta bildirim eylemi için aşağıda verilmiştir.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Put yöntemi yeni bir e-posta eylemi için bir zamanlama oluşturmak için bir benzersiz bir eylem kimliği ile kullanın.  Aşağıdaki örnek, bir e-posta bildirimi bir eşik ile oluşturur, böylelikle kayıtlı arama sonuçlarını eşiği aşması durumunda e-posta gönderilir.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Put yöntemi, bir zamanlama için bir e-posta eylem değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Düzeltme eylemleri
Düzeltmeler, uyarı tarafından tanımlanan sorunu düzeltme girişiminde Azure Otomasyonu'ndaki bir runbook'u başlatın.  Bir düzeltme eylemi kullanılan runbook için bir Web kancası oluşturmanız ve ardından URI WebhookUri özelliğinde belirtmeniz gerekir.  Azure portalını kullanarak bu eylem oluşturduğunuzda, yeni bir Web kancası runbook için otomatik olarak oluşturulur.

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcıları için runbook kullanarak düzeltme gibi eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).

Düzeltmeleri özellikler aşağıdaki tabloda içerir.

| Özellik | Açıklama |
|:--- |:--- |
| RunbookName |Runbook'un adı. Bu Otomasyon çözümü Log Analytics çalışma alanınızda yapılandırılmış Otomasyon hesabını yayımlanan bir runbook'ta eşleşmelidir. |
| WebhookUri |Web kancası URI'si. |
| Süre Sonu |Web kancasının süresi ve sona erme tarihi.  Ardından bu Web kancasını bir sona erme yoksa, geçerli tarihe olabilir. |

Örnek yanıt için bir düzeltme eylemi bir eşik ile aşağıda verilmiştir.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Put yöntemi yeni bir düzeltme eylemi için bir zamanlama oluşturmak için bir benzersiz bir eylem kimliği ile kullanın.  Kayıtlı arama sonuçlarını eşiği aşması durumunda, runbook başlatıldığında bu nedenle aşağıdaki örnekte bir eşik ile bir düzeltme oluşturur.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Put yöntemi, bir zamanlama için bir düzeltme eylemi değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Örnek
Yeni bir e-posta Uyarı oluşturmak için tam bir örnek aşağıda verilmiştir.  Bu eşiği ve e-posta içeren bir eylem ile birlikte yeni bir zamanlama oluşturur.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"
    
    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson
    
    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

#### <a name="webhook-actions"></a>Web kancası eylemleri
Web kancası eylemleri, bir URL çağırma ve isteğe bağlı olarak gönderilmesi için bir yük sağlayarak bir işlem başlar.  Azure Otomasyonu runbook'ları dışındaki işlemler çağırabilir Web kancaları için yöneliktir dışında düzeltme eylemlerinde benzerdir.  İçin uzak işlem teslim edilecek bir yükü sağlama ek seçeneği de sağlanır.

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure genel bulutunda örneğini Log Analytics çalışma alanı içindeki tüm uyarılar otomatik olarak Azure'a genişletilir. Bir kullanıcı, gönüllü olarak azure'a genişletme uyarılar 14 Mayıs 2018'den önce başlatabilirsiniz. Daha fazla bilgi için [Log analytics'ten azure'a genişletme uyarılar](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcıları için Web kancası gibi eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu API](https://docs.microsoft.com/rest/api/monitor/actiongroups).


Web kancası eylemleri bir eşiği yoktur, ancak bunun yerine bir eşik ile bir uyarı eylemi olan bir zamanlamanın eklenmelidir.  

Web kancası eylemi ve bir eşik ile ilişkili bir uyarı eylemi için örnek yanıt aşağıdadır.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

##### <a name="create-or-edit-a-webhook-action"></a>Oluşturun veya bir Web kancası eylemi Düzenle
Put yöntemi yeni bir Web kancası eylemi için bir zamanlama oluşturmak için bir benzersiz bir eylem kimliği ile kullanın.  Kayıtlı arama sonuçlarını eşiği aşması durumunda, Web kancası tetiklenir, aşağıdaki örnek bir Web kancası eylemi ve bir uyarı eylemi ile bir eşiği oluşturur.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Put yöntemi, bir zamanlama için bir Web kancası eylemi değiştirmek için var olan bir eylem kimliği ile kullanın.  İstek gövdesi, eylemin etag içermesi gerekir.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction


## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük aramaları yapmak için REST API](../../azure-monitor/log-query/log-query-overview.md) Log analytics'te.
* Hakkında bilgi edinin [oturum uyarılar azure uyarıları](../../azure-monitor/platform/alerts-unified-log.md)

