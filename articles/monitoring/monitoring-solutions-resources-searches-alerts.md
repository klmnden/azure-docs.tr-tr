---
title: Aramaları ve uyarılar yönetim çözümlerine kayıtlı | Microsoft Docs
description: Yönetim çözümleri genellikle Kaydedilmiş aramaları çözümü tarafından toplanan verileri çözümlemek için günlük analizi dahil edin.  Ayrıca kullanıcıya bildirmek için uyarılar tanımlayın olabilir veya otomatik olarak yanıt kritik bir sorun için adımları uygulayın.  Bu makalede, günlük yönetim çözümlerine dahil şekilde bir Resource Manager şablonunda kaydedilmiş aramaları ve Uyarıları analizi tanımlamak açıklar.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/18/2018
ms.author: bwren, vinagara
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c29d6cb0da2e394912a2584b0d3c3cedf13f054c
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304489"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Günlük analizi ekleme arar ve Uyarıları kaydedilen yönetim çözümü (Önizleme)

> [!NOTE]
> Bu, şu anda önizlemede olan yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.   


[Yönetim çözümleri](monitoring-solutions.md) genellikle içerecektir [kayıtlı aramalar](../log-analytics/log-analytics-log-searches.md) çözümü tarafından toplanan verileri çözümlemek için günlük analizi içinde.  Ayrıca tanımlayabilir [uyarıları](../log-analytics/log-analytics-alerts.md) kullanıcıya bildir veya otomatik olarak yanıt kritik bir sorun için adımları uygulayın.  Bu makalede nasıl günlük kayıtlı aramalar analizi tanımlayacağınızı açıklar ve Uyarıları gelen bir [kaynak yönetimi şablonu](../resource-manager-template-walkthrough.md) içinde eklenebilir şekilde [yönetim çözümleri](monitoring-solutions-creating.md).

> [!NOTE]
> Bu makaledeki örnekler parametreleri ve gerekli olduğunu veya yönetim çözümleri için ortak olduğunu ve açıklanan değişkenleri kullanma [tasarım ve yapı Azure Yönetimi çözümünde](monitoring-solutions-creating.md)  

## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [bir yönetim çözümü oluşturma](monitoring-solutions-creating.md) ve yapısı bir [Resource Manager şablonu](../resource-group-authoring-templates.md) ve çözüm dosya.


## <a name="log-analytics-workspace"></a>Log Analytics Çalışma Alanı
Günlük analizi tüm kaynaklarında bulunan bir [çalışma](../log-analytics/log-analytics-manage-access.md).  Bölümünde açıklandığı gibi [günlük analizi çalışma alanı ve Automation hesabı](monitoring-solutions.md#log-analytics-workspace-and-automation-account), çalışma alanı yönetimi çözümünde dahil değildir ancak çözüm yüklenmeden önce mevcut olması gerekir.  Kullanılabilir değilse, çözüm yükleme başarısız olur.

Her günlük analizi kaynak adına çalışma adıdır.  Bu çözümle yapılır **çalışma** SavedSearch kaynak aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Günlük analizi API sürümü
Resource Manager şablonunda tanımlanan tüm günlük analizi kaynaklarını özelliğine sahip **apiVersion** kaynak kullanması gereken API sürümü tanımlar.   

Bu örnekte kullanılan kaynak için API sürümü aşağıdaki tabloda listelenmektedir.

| Kaynak türü | API sürümü | Sorgu |
|:---|:---|:---|
| savedSearches | 2017-03-15-Önizleme | Olay &#124; burada EventLevelName "Error" ==  |


## <a name="saved-searches"></a>Kayıtlı Aramalar
Dahil [kayıtlı aramalar](../log-analytics/log-analytics-log-searches.md) çözümünüz tarafından toplanan sorgu veri kullanıcılarına izin vermek için bir çözüm içinde.  Kaydedilmiş aramaları altında görünen **Sık Kullanılanlar** OMS portalında ve **kayıtlı aramaları** Azure portalında.  Kaydedilmiş bir aramayı de her uyarı için gereklidir.   

[Günlük analizi kaydedilen arama](../log-analytics/log-analytics-log-searches.md) kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches` ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



Kaydedilmiş bir aramayı, her bir özellik aşağıdaki tabloda açıklanmıştır. 

| Özellik | Açıklama |
|:--- |:--- |
| category | Kayıtlı arama kategorisi.  Konsolunda birlikte gruplanır şekilde aynı çözümüne kaydedilmiş yapılan aramalar genellikle tek bir kategori paylaşır. |
| görünen adı | Portalı'nda kayıtlı arama için görüntülenecek adı. |
| sorgu | Çalıştırılacak sorgu. |

> [!NOTE]
> JSON olarak yorumlanabilecek karakterler içeriyorsa, sorguda kaçış karakterleri kullanmanız gerekebilir.  Örneğin, sorgu, **türü: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, çözüm dosyasındaki yazılmalıdır **türü: AzureActivity işlemadı:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Uyarılar
[Azure günlük uyarıları](../monitoring-and-diagnostics/monitor-alerts-unified-log.md) düzenli aralıklarla belirtilen günlük sorguları çalıştırmak Azure uyarı kuralları tarafından oluşturulur.  Sorgu sonuçlarını belirtilen ölçütlerle eşleşen, bir uyarı kaydı oluşturulur ve bir veya daha fazla eylemleri kullanarak çalışması [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md).  

> [!NOTE]
> 14 Mayıs 2018 başlayan bir çalışma alanındaki tüm uyarıları, Azure'da genişletmek otomatik olarak başlatılacak. Bir kullanıcı gönüllü 14 Mayıs 2018 önce Azure genişletme uyarıları başlatabilir. Daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure içine](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Uyarılar için Azure genişletmek kullanıcılar için Eylemler artık Azure eylem gruplarında denetlenir. Azure için genişletilmiş bir çalışma alanı ve onun uyarıları, almak veya eylemleri kullanarak eklemek [eylem Grup - Azure Resource Manager şablonu](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

Uyarı kurallarında bir yönetim çözümü, aşağıdaki üç farklı kaynakları yapılır.

- **Kayıtlı arama.**  Çalıştırılan günlük arama tanımlar.  Birden çok uyarı kurallarını, tek bir kayıtlı arama paylaşabilirsiniz.
- **Zamanlama.**  Günlük arama ne sıklıkta çalıştırmak tanımlar.  Her uyarı kuralı bir ve yalnızca bir zamanlaması vardır.
- **Uyarı eylem.**  Bir eylem grubu kaynağı veya eylem (eski) sahip kaynak türü, her bir uyarı kuralı sahip **uyarı** bir uyarı kaydı oluşturulduğunda ve uyarının önem derecesi ölçütlerini gibi uyarı ayrıntılarını tanımlar. [Eylem grubu](../monitoring-and-diagnostics/monitoring-action-groups.md) kaynak uyarı tetiklenir - sesli araması, SMS, gibi yapılacak yapılandırılmış eylemlerin bir listesini sahip e-posta, Web kancası, ITSM aracı, Otomasyon runbook'u, mantıksal uygulama, vb.
 
Eylem kaynak (eski) isteğe bağlı olarak bir posta ve runbook yanıt tanımlayacaksınız.
- **Web kancası eylem (eski).**  Uyarı kuralı bir Web kancası çağırır sonra başka bir işlem kaynak türüne sahip gerektirir **Web kancası**.    

Kaynaklar, yukarıda açıklanan arama kaydedildi.  Diğer kaynaklar aşağıda açıklanmıştır.


### <a name="schedule-resource"></a>Zamanlama kaynak

Kaydedilmiş bir aramayı her zamanlamayı ayrı bir uyarı kuralı temsil eden ile bir veya daha fazla zamanlama olabilir. Ne sıklıkta arama çalıştırma ve verilerin alınacağı zaman aralığı olan zamanlamayı tanımlar.  Zamanlama kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` ve aşağıdaki yapı ayarlanmıştır. Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



Zamanlama kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| enabled       | Evet | Oluşturulduğunda uyarının etkin olup olmadığını belirtir. |
| interval      | Evet | Ne sıklıkta sorgu dakika içinde çalışır. |
| QueryTimeSpan | Evet | Sonuçları değerlendirileceği üzerinden dakika cinsinden süre uzunluğu. |

Zamanlama önce oluşturulan zamanlama kaynak kayıtlı arama üzerinde bağımlı olmalıdır.

> [!NOTE]
> Zamanlama adı verilen bir çalışma alanında benzersiz olmalıdır; farklı Kaydedilmiş aramaları ile ilişkili olsalar bile, iki zamanlamaları aynı Kimliğe sahip olamaz. Ayrıca tüm kaydedilmiş aramaları, çizelgeler ve günlük analizi API ile oluşturulan eylemler için ad küçük olmalıdır.


### <a name="actions"></a>Eylemler
Bir zamanlama birden çok eylemler olabilir. Bir posta gönderme veya bir runbook'u başlatma gibi gerçekleştirmek için bir veya daha fazla işlem bir eylem tanımlayabilir veya ne zaman bir arama sonuçlarını bazı ölçütlere uyan belirleyen bir eşik tanımlayabilir.  Böylece eşiğine ulaşıldığında işlemleri gerçekleştirilen bazı eylemler her ikisi de tanımlayacaksınız.

Eylemler [eylem Grup] veya eylem kaynağına kullanılarak tanımlanabilir.

> [!NOTE]
> 14 Mayıs 2018 başlayan bir çalışma alanındaki tüm uyarıları, Azure'da genişletmek otomatik olarak başlatılacak. Bir kullanıcı gönüllü 14 Mayıs 2018 önce Azure genişletme uyarıları başlatabilir. Daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure içine](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Uyarılar için Azure genişletmek kullanıcılar için Eylemler artık Azure eylem gruplarında denetlenir. Azure için genişletilmiş bir çalışma alanı ve onun uyarıları, almak veya eylemleri kullanarak eklemek [eylem Grup - Azure Resource Manager şablonu](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).


Belirtilen eylem kaynak iki tür vardır **türü** özelliği.  Bir zamanlama gerektiren **uyarı** uyarı kuralı ve bir uyarı oluşturulduğunda, hangi eylemleri alınır ayrıntılarını tanımlayan eylem. Eylem kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

Uyarı eylemleri aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 


```
    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                  },
              },
      "AzNsNotification": {
        "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
        "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
        "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
        }
    }
```

Uyarı eylemi kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Tür | Evet | Eylem türü.  Bu **uyarı** uyarı eylemleri için. |
| Ad | Evet | Uyarı görünen adı.  Bu uyarı kuralı için konsolunda görüntülenen addır. |
| Açıklama | Hayır | Uyarı isteğe bağlı bir açıklama. |
| Severity | Evet | Aşağıdaki değerlerden uyarı kaydının önem derecesi:<br><br> **Kritik**<br>**Uyarı**<br>**Bilgilendirme**


#### <a name="threshold"></a>Eşik
Bu bölüm gereklidir.  Uyarı eşiği özelliklerini tanımlar.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| İşleç | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt büyük =<br>lt = küçüktür** |
| Değer | Evet | Sonuçları karşılaştırmak için değer. |

##### <a name="metricstrigger"></a>MetricsTrigger
Bu bölümde isteğe bağlıdır.  Bir ölçüm ölçüm uyarı içerir.

> [!NOTE]
> Ölçüm ölçüm uyarılar şu anda genel önizlemede. 

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| TriggerCondition | Evet | Eşik ihlallerini veya aşağıdaki değerlerden ardışık ihlallerini toplam sayısını olup olmadığını belirtir:<br><br>**Toplam<br>ardışık** |
| İşleç | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt büyük =<br>lt = küçüktür** |
| Değer | Evet | Sayısı uyarıyı tetikleyecek ölçütler karşılanması gerekir. |


#### <a name="throttling"></a>Azaltma
Bu bölümde isteğe bağlıdır.  Bazı süreyi bir uyarı oluşturulduktan sonra için aynı kural uyarıları gizlemek istiyorsanız, bu bölüm içerir.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Dakika Cinsiden Süre | Dahil edilen öğesi azaltma, Evet | Aynı uyarı kuralı birinden oluşturulduktan sonra uyarıları gizlemek için dakika sayısı. |


#### <a name="azure-action-group"></a>Azure eylem grubu
Azure, içindeki tüm uyarılar Eylemler işlemek için varsayılan bir mekanizma olarak eylem grubu kullanın. Eylem grubuyla eylemlerinizi bir kez belirtin ve sonra Azure arasında birden çok uyarı - eylem grubuna ilişkilendirebilirsiniz. Sürekli olarak aynı eylemleri tekrar tekrar bildirme gerek olmadan. Eylem grupları birden çok eylem - e-posta, SMS, sesli arama, ITSM bağlantı, Otomasyon Runbook'u, Web kancası URI ve benzeri destekler. 

Kimin uyarılarını Azure'da - genişletilmiş kullanıcının için bir zamanlama artık bir uyarı oluşturmak için eşik yanı sıra, geçirilen eylem grubu ayrıntıları olması gerekir. E-posta ayrıntıları, Web kancası URL'leri, Runbook Otomasyon Ayrıntılar ve diğer eylemleri olması gereken taraftaki ilk önce bir uyarı; oluşturma bir eylem grubu tanımlanmış bir oluşturabilir [eylem Azure İzleyicisi grubundan](../monitoring-and-diagnostics/monitoring-action-groups.md) portalında veya kullanım [eylem Grup - kaynak şablonu](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| AzNsNotification | Evet | Uyarı ölçütler karşılandığında gerekli eylemleri ayırdığınız için uyarı ilişkilendirilecek Azure eylem grubunun kaynak kimliği. |
| CustomEmailSubject | Hayır | İlişkili eylem grubunda belirtilen tüm adreslere gönderilen e-postaları özel konu satırı. |
| CustomWebhookPayload | Hayır | İlişkili eylem grubunda tanımlanan tüm Web kancası Uç noktalara gönderilmek üzere özelleştirilmiş yükü. Biçimi ne Web kancası beklediği ve geçerli bir seri hale getirilmiş JSON olmalıdır bağlıdır. |


#### <a name="actions-for-oms-legacy"></a>Eylemler için OMS (eski)

Her zamanlama varsa **uyarı** eylem.  Bu, uyarı ve isteğe bağlı olarak bildirim ve düzeltme eylemlerinin ayrıntılarını tanımlar.  Bir bildirim bir veya daha fazla adrese bir e-posta gönderir.  Bir düzeltme algılanan sorunu düzeltmek girişiminde Azure Otomasyonu'nda bir runbook başlatır.

> [!NOTE]
> 14 Mayıs 2018 başlayan bir çalışma alanındaki tüm uyarıları, Azure'da genişletmek otomatik olarak başlatılacak. Bir kullanıcı gönüllü 14 Mayıs 2018 önce Azure genişletme uyarıları başlatabilir. Daha fazla bilgi için bkz: [genişletmek uyarıları OMS Azure içine](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Uyarılar için Azure genişletmek kullanıcılar için Eylemler artık Azure eylem gruplarında denetlenir. Azure için genişletilmiş bir çalışma alanı ve onun uyarıları, almak veya eylemleri kullanarak eklemek [eylem Grup - Azure Resource Manager şablonu](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Bu bölümde isteğe bağlı bir veya daha fazla alıcıya posta göndermek için uyarı istiyorsanız ekleyin.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Alıcılar | Evet | Bir uyarı aşağıdaki örnekte olduğu gibi oluşturulduğunda, bildirim göndermek için e-posta adreslerini virgülle ayrılmış listesi.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Konu | Evet | Posta konu satırı. |
| Ek | Hayır | Ekleri şu anda desteklenmemektedir.  Bu öğe dahil ise olmalıdır **hiçbiri**. |


##### <a name="remediation"></a>Düzeltme
Bu bölümde isteğe yanıt olarak uyarı başlatmak üzere bir runbook'u istiyorsanız ekleyin. |

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| RunbookName | Evet | Başlatmak için runbook'un adı. |
| WebhookUri | Evet | Runbook için Web kancası URI'si. |
| Süre Sonu | Hayır | Tarih ve saat düzeltme süresi dolar. |

##### <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir URL çağırma ve isteğe bağlı olarak gönderilecek bir yükü sağlayarak bir işlem başlatın. Azure Otomasyon çalışma kitabı dışındaki işlemler çağırabilir Web kancası için amacı dışında düzeltme eylemleri benzerdir. Uzak işlem teslim edilecek bir yükü sağlama ek seçeneği de sağlar.

Uyarınız bir Web kancası çağırır sonra bir eylem kaynak türüne sahip gerekir **Web kancası** ek olarak **uyarı** eylem kaynak.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

Web kancası eylem kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| type | Evet | Eylem türü.  Bu **Web kancası** Web kancası eylemleri için. |
| ad | Evet | Eylem görünen adı.  Bu konsolunda görüntülenmez. |
| wehookUri | Evet | Web kancası için URI. |
| CustomPayload | Hayır | Web kancası için gönderilecek özel yükü. Web kancası bekleniyor üzerinde biçimi bağlıdır. |


## <a name="sample"></a>Örnek

Aşağıdaki kaynakları içermektedir içeren bir çözüm örneği aşağıda verilmiştir:

- Kayıtlı arama
- Zamanlama
- Eylem grubu

Örnek kullanır [standart çözüm parametreleri]( monitoring-solutions-solution-file.md#parameters) cmdlet'e kod değerleri kaynak tanımlarında aksine bir çözümde yaygın olarak kullanılacak değişkenleri.

```
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "actiongroup": {
            "type": "string",
            "metadata": {
              "Description": "List of action groups for alert actions separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
              "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",

          "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
              "GroupIds": [
                "[parameters('actiongroup')]"
              ],
              "CustomEmailSubject": "Sample alert"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
            "AzNsNotification": {
              "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
              "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
            }             
            }
          }
        ]
    }
```

Aşağıdaki parametre dosyası bu çözüm için örnek değerleri sağlar.
```
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "actiongroup": {
                "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
            }
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
* [Görünümler ekleme](monitoring-solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynakları eklemek](monitoring-solutions-resources-automation.md) Yönetimi çözümünüz için.

