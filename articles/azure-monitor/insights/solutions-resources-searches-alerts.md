---
title: Yönetim çözümlerine kayıtlı aramalar | Microsoft Docs
description: Yönetim çözümleri genellikle çözüm tarafından toplanan verileri çözümlemek için Log analytics'te kayıtlı aramalar içerir. Kullanıcı bunu size bildirecek uyarılar da tanımlamanız veya otomatik olarak yanıt olarak kritik bir sorunu eylem. Bu makalede, Log Analytics yönetim çözümleri eklenmesi için bir Resource Manager şablonunda kayıtlı aramalar tanımlanacağını açıklar.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0975b23a8f96da6fc2dfcc8bd9ad046847a68aa9
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104842"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Log Analytics ekleme aramaları ve Uyarıları kaydedilen yönetim çözümü (Önizleme)

> [!IMPORTANT]
> Resource Manager şablonu kullanarak bir uyarı oluşturmak için ayrıntıları burada olan tanesi tarihi artık [Log Analytics uyarılarını, Azure İzleyici Genişletilmiş](../platform/alerts-extend.md). Resource Manager şablonu ile günlük uyarısı oluşturma hakkında daha fazla bilgi edinmek için bkz: [Azure kaynak şablonu kullanarak yönetme günlük uyarıları](../platform/alerts-log.md#managing-log-alerts-using-azure-resource-template).

> [!NOTE]
> Şu anda Önizleme aşamasında olan yönetim çözümleri oluşturmak için başlangıç belgeleri budur. Aşağıda açıklanan herhangi bir şema tabi bir değişikliktir.

[Yönetim çözümleri](solutions.md) genellikle içerecektir [kayıtlı aramalar](../../azure-monitor/log-query/log-query-overview.md) çözüm tarafından toplanan verileri çözümlemek için Log analytics'te. Da belirtebilirler [uyarılar](../../azure-monitor/platform/alerts-overview.md) kullanıcıya bildir veya otomatik olarak yanıt olarak kritik bir sorunu eylem. Bu makale, Log Analytics kayıtlı aramalar tanımlamayı açıklar ve uyarılar bir [kaynak yönetimi şablonu](../../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md) olarak eklenebilir böylece [yönetim çözümleri](solutions-creating.md).

> [!NOTE]
> Bu makaledeki örnekleri parametreler ve değişkenler gerekli olduğunu veya yönetim çözümleri için yaygın olduğunu ve açıklanan kullanmak [tasarım ve derleme Azure Yönetimi çözümünde](solutions-creating.md)

## <a name="prerequisites"></a>Önkoşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [yönetimi çözümü oluşturmak](solutions-creating.md) ve yapısı bir [Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) ve çözüm dosyası.


## <a name="log-analytics-workspace"></a>Log Analytics Çalışma Alanı
Log Analytics tüm kaynaklarda bulunan bir [çalışma](../../azure-monitor/platform/manage-access.md). Bölümünde anlatıldığı gibi [Log Analytics çalışma alanını ve Otomasyon hesabı](solutions.md#log-analytics-workspace-and-automation-account), çalışma yönetimi çözümünde dahil değildir, ancak çözüm yüklenmeden önce mevcut olması gerekir. Ardından, kullanılabilir durumda değilse, çözüm yükleme başarısız olur.

Çalışma alanı adına her bir Log Analytics kaynak adıdır. Bu çözüm ile gerçekleştirilir **çalışma** SavedSearch kaynağının aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Günlük analizi API'si sürümü
Bir Resource Manager şablonunda tanımlı tüm Log Analytics kaynakları bir özelliği olan **apiVersion** kullanması gereken kaynak API sürümünü tanımlar.

Aşağıdaki tabloda, bu örnekte kullanılan kaynak için API sürümü listeler.

| Kaynak türü | API sürümü | Sorgu |
|:---|:---|:---|
| savedSearches | 2017-03-15-Önizleme | Olay &#124; burada EventLevelName "Error" ==  |


## <a name="saved-searches"></a>Kayıtlı Aramalar
Dahil [kayıtlı aramalar](../../azure-monitor/log-query/log-query-overview.md) çözümünüz tarafından toplanan verileri sorgulamak için kullanıcıların bir çözümde. Kayıtlı aramalar altında görünen **kayıtlı aramalar** Azure portalında. Kayıtlı bir aramayı, her uyarı için de gereklidir.

[Log Analytics kayıtlı araması](../../azure-monitor/log-query/log-query-overview.md) kaynaklara sahip bir tür `Microsoft.OperationalInsights/workspaces/savedSearches` ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

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

Kayıtlı bir aramayı her bir özellik aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| category | Kayıtlı arama için kategori.  Konsolunda birlikte gruplanır, böylece aynı çözüm içindeki tüm kayıtlı aramalar genellikle tek bir kategori paylaşın. |
| DisplayName | Portalı'nda kayıtlı arama için görüntülenecek ad. |
| sorgu | Çalıştırılacak sorgu. |

> [!NOTE]
> JSON olarak yorumlanabilecek karakterler içeriyorsa, kaçış karakterleri sorguda kullanmanız gerekebilir. Örneğin, sorgunuz varsa **AzureActivity | OperationName:"Microsoft.Compute/virtualMachines/write"**, çözüm dosyasındaki yazılmalıdır **AzureActivity | OperationName: /\"Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Uyarılar
[Azure günlük uyarılarını](../../azure-monitor/platform/alerts-unified-log.md) düzenli aralıklarla belirtilen günlük sorguları çalıştıran Azure uyarı kuralları tarafından oluşturulur. Sorgu sonuçlarını belirtilen ölçütlerle eşleşen, bir uyarı kaydı oluşturulur ve bir veya daha fazla eylem kullanarak çalıştırılır [Eylem grupları](../../azure-monitor/platform/action-groups.md).

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren Azure'a genişletmek tüm Azure genel bulutunda örneğini Log Analytics çalışma alanı Uyarılardaki başladı. Daha fazla bilgi için [genişletmek uyarılar azure'a](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için Eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu - Azure Resource Manager şablonu](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Bir yönetim çözümüne uyarı kuralları aşağıdaki üç farklı kaynaklardan oluşur.

- **Kayıtlı arama.** Çalıştırılan günlük araması tanımlar. Birden çok uyarı kuralları tek kayıtlı bir aramayı paylaşabilirsiniz.
- **Zamanlama.** Günlük araması ne sıklıkta çalıştırılacağını tanımlar. Her uyarı kuralı bir ve yalnızca bir zamanlama var.
- **Uyarı eylemi.** Her uyarı kuralı bir eylem grubu kaynağı veya eylem kaynağı (eski) türüne sahip olan **uyarı** ne zaman bir uyarı kaydı oluşturulur ve uyarının önem derecesi ölçütlerini gibi uyarı ayrıntılarını tanımlar. [Eylem grubu](../../azure-monitor/platform/action-groups.md) kaynak, bir uyarı tetiklendiğinde - SMS, sesli arama gibi yapılandırılmış eylemler listesi olabilir e-posta, Web kancası, ITSM aracına, Otomasyon runbook'u, mantıksal uygulama, vs.

Eylem kaynağı (eski) isteğe bağlı olarak bir e-posta ve runbook yanıt tanımlarsınız.
- **Web kancası eylemi (eski).** Uyarı kuralını bir Web kancası çağırırsa sonra bir başka bir işlem kaynağı türünde gerektirir **Web kancası**.

Arama kaynakları, yukarıda açıklanan kaydedildi. Diğer kaynakları aşağıda açıklanmıştır.

### <a name="schedule-resource"></a>Zamanlama kaynak

Kayıtlı bir aramayı bir veya daha fazla zamanlama ayrı bir uyarı kuralı temsil eden her bir zamanlama olabilir. Ne sıklıkta arama çalıştırma ve verilerin alınacağı zaman aralığı, zamanlamayı tanımlar. Zamanlama kaynaklara sahip bir tür `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` ve aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

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
Zamanlama kaynakların özellikleri aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| enabled       | Evet | Oluşturulduğunda uyarının etkinleştirilip etkinleştirilmeyeceğini belirtir. |
| interval      | Evet | Ne sıklıkla sorgu dakikalar içinde çalışır. |
| QueryTimeSpan | Evet | Sürenin sonuçları değerlendirileceği üzerinden dakika cinsinden uzunluğu. |

Böylece zamanlama önce oluşturulan zamanlama kaynak kayıtlı arama üzerinde bağlı olmalıdır.
> [!NOTE]
> Zamanlama adı verilen bir çalışma alanında benzersiz olmalıdır; farklı kayıtlı aramalar ile ilişkili olsalar bile iki zamanlamaları aynı Kimliğe sahip olamaz. Ayrıca tüm kayıtlı aramalar, zamanlamalar ve günlük analizi API'si ile oluşturulan eylem adı küçük harfle olması gerekir.

### <a name="actions"></a>Eylemler
Bir zamanlama birden fazla eylem olabilir. Posta gönderme veya bir runbook başlatma gibi gerçekleştirmek için bir veya daha fazla işlem bir eylem tanımlayabilir veya ne zaman bir aramanın sonuçları bazı ölçütlerle eşleşen belirleyen bir eşiği tanımlayabilir. Eşiğine ulaşıldığında işlemleri gerçekleştirilir böylece bazı eylemler her ikisi de tanımlayın.
Eylemler [eylem grubu] kaynak veya kaynak eylemi kullanılarak tanımlanabilir.
> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren otomatik olarak Azure'a genişletmek tüm Azure genel bulutunda örneğini Log Analytics çalışma alanı Uyarılardaki başladı. Daha fazla bilgi için [genişletmek uyarılar azure'a](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için Eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu - Azure Resource Manager şablonu](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).
Eylem kaynağı tarafından belirtilen iki tür vardır **türü** özelliği. Bir zamanlama gerektiren **uyarı** uyarı kuralı ve bir uyarı oluşturulduğunda ne Eylemler gerçekleştirildikçe ayrıntılarını tanımlayan eylem. Eylem kaynaklara sahip bir tür `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.

Uyarı eylemleri aşağıdaki yapıya sahiptir. Kopyalayabilir ve bu kod parçacığı, çözüm dosyasına yapıştırın ve parametre adlarını değiştirmek için bu genel değişkenler ve parametreler içerir.

```json
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

Uyarı eylemi kaynakların özellikleri aşağıdaki tablolarda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Tür | Evet | Eylem türü.  Bu **uyarı** uyarı eylemleri için. |
| Ad | Evet | Uyarı görünen adı.  Bu uyarı kuralı için konsolunda görüntülenen addır. |
| Açıklama | Hayır | Uyarının isteğe bağlı bir açıklama. |
| Severity | Evet | Önem derecesi uyarı kaydını aşağıdaki değerleri:<br><br> **Kritik**<br>**Uyarı**<br>**Bilgilendirme**


#### <a name="threshold"></a>Eşik
Bu bölüm gereklidir. Uyarı eşiği için özellikleri tanımlar.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| İşleç | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt = büyüktür<br>lt = kısa** |
| Değer | Evet | Sonuçları Karşılaştırılacak değer. |

##### <a name="metricstrigger"></a>MetricsTrigger
Bu bölüm isteğe bağlıdır. Bu, bir ölçüm ölçüsü uyarı ekleyin.

> [!NOTE]
> Ölçüm ölçüsü uyarı şu anda genel Önizleme aşamasındadır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| TriggerCondition | Evet | Eşik ihlallerini veya aşağıdaki değerlerden ardışık ihlaller toplam sayısı için uygun olup olmadığını belirtir:<br><br>**Toplam<br>ardışık** |
| İşleç | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt = büyüktür<br>lt = kısa** |
| Değer | Evet | Uyarı tetiklemek için ölçütleri sağlanmalıdır. deneme sayısı. |


#### <a name="throttling"></a>Azaltma
Bu bölüm isteğe bağlıdır. Bazı uyarı oluşturulduktan sonra zaman miktarı için aynı kural uyarıları gizlemek istiyorsanız, bu bölüm içerir.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Dakika Cinsiden Süre | Azaltma eklenen öğe varsa Evet | Aynı uyarı kuralı birinden oluşturulduktan sonra uyarıları bastırmak için dakika sayısı. |

#### <a name="azure-action-group"></a>Azure eylem grubu
Azure'daki tüm uyarılar eylemlerini işleyen varsayılan bir mekanizma olarak eylem grubu kullanın. Eylem grubu ile bir kez eylemleri belirtin ve birden çok uyarı - eylem grubuna Azure genelinde ilişkilendirin. Tekrar tekrar aynı eylemleri tekrar tekrar bildirme gerek kalmadan. Eylem grupları, birden fazla eylem - e-posta, SMS, sesli arama, ITSM bağlantısı, Otomasyon Runbook'u, Web kancası URI ve benzeri destekler.

Kimin uyarılarını - Azure'a genişletilmiş kullanıcının için bir zamanlama artık bir uyarı oluşturabilmek için eşik yanı sıra, geçirilen eylem grubu ayrıntıları olması gerekir. E-posta ayrıntıları, Web kancası URL'leri, Runbook Otomasyon ayrıntıları ve diğer eylemler olması gereken bir eylem grubu ilk önce bir uyarı; oluşturma tarafta tanımlanan bir izin oluşturabilir [Azure İzleyici'eylem grubundan](../../azure-monitor/platform/action-groups.md) portalı veya [eylem grubu - kaynak şablonu](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| AzNsNotification | Evet | Uyarı ölçütleri karşılandığında gerekli eylemleri almak için uyarı ile ilişkilendirilecek Azure eylem grubu kaynak kimliği. |
| CustomEmailSubject | Hayır | Özel konu satırı ilişkili eylem grubunda belirtilen tüm adreslere gönderilen e-postası. |
| CustomWebhookPayload | Hayır | İlişkilendirilen eylem grubunda tanımlanan tüm Web kancası uç noktalarına gönderilecek özelleştirilmiş yükü. Biçim ne Web kancası bekliyor ve geçerli bir seri hale getirilmiş JSON olması gerektiğini bağlıdır. |

#### <a name="actions-for-oms-legacy"></a>Eylemler için OMS (eski)

Her zamanlama varsa **uyarı** eylem. Bu, uyarı ve isteğe bağlı olarak bildirim ve düzeltme eylemlerinin ayrıntılarını tanımlar. Bir bildirim, bir veya daha fazla adreslere bir e-posta gönderir. Bir düzeltme algılanan sorunu düzeltme girişiminde Azure Automation'da bir runbook başlatır.

> [!NOTE]
> 14 Mayıs 2018 tarihinden itibaren otomatik olarak Azure'a genişletmek tüm Azure genel bulutunda örneğini Log Analytics çalışma alanı Uyarılardaki başladı. Daha fazla bilgi için [genişletmek uyarılar azure'a](../../azure-monitor/platform/alerts-extend.md). Uyarıları Azure'a genişletme kullanıcılar için Eylemler artık Azure Eylem grupları içinde denetlenir. Bir çalışma alanı ve onun uyarılar Azure'a genişletilir, alma veya eylemleri kullanarak eklemek [eylem grubu - Azure Resource Manager şablonu](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Bu bölümde, isteğe bağlı bir veya daha fazla alıcıya e-posta göndermek için uyarı istiyorsanız bunu ekleyin.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Recipients | Evet | Virgülle ayrılmış bir uyarı aşağıdaki örnekte olduğu gibi oluşturulduğunda, bildirim göndermek için e-posta adresleri listesi.<br><br>**["recipient1\@contoso.com", "recipient2\@contoso.com"]** |
| Subject | Evet | E-posta konu satırı. |
| Attachment | Hayır | Ekleri şu anda desteklenmemektedir. Bu öğe dahil ise, olmalıdır **hiçbiri**. |

##### <a name="remediation"></a>Düzeltme
Bu bölüm isteğe bağlıdır ve uyarıya yanıt olarak başlatılması için bir runbook istiyorsanız bunu ekleyin. 

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| RunbookName | Evet | Başlamak için runbook'un adı. |
| WebhookUri | Evet | Runbook için bir Web kancası URI'si. |
| Expiry | Hayır | Tarih ve saat, düzeltme süresi dolar. |

##### <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir URL çağırma ve isteğe bağlı olarak gönderilmesi için bir yük sağlayarak bir işlem başlar. Azure Otomasyonu runbook'ları dışındaki işlemler çağırabilir Web kancaları için yöneliktir dışında düzeltme eylemlerinde benzerdir. İçin uzak işlem teslim edilecek bir yükü sağlama ek seçeneği de sağlanır.

Uyarınız bir Web kancasını çağıracak sonra türünde bir eylem kaynak gerekir **Web kancası** ek olarak **uyarı** eylem kaynağı.

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
Web kancası eylemi kaynakların özellikleri aşağıdaki tablolarda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| type | Evet | Eylem türü. Bu **Web kancası** Web kancası işlemleri için. |
| ad | Evet | Eylem görünen adı. Bu konsolunda görüntülenmez. |
| webhookUri | Evet | Web kancası için URI. |
| customPayload | Hayır | Web kancası'na gönderilecek özel yükü. Biçim, Web kancası bekleniyor üzerinde bağlıdır. |

## <a name="sample"></a>Örnek

Aşağıdaki kaynakları içeren bir çözümü bir örnek aşağıda verilmiştir:

- Kayıtlı arama
- Zamanlama
- Eylem grubu

Örnek kullanır [standart çözüm parametreleri]( solutions-solution-file.md#parameters) yaygın olarak kaynak tanımlarında runbook'a kod değerleri aksine bir çözümde kullanılan değişkenler.

```json
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
            "Description": "Sample alert. Fires when 3 error records found over hour interval.",
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
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
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
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
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

Şu parametre dosyası, bu çözüm için örnek değerler sağlar.

```json
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
* [Görünümler ekleme](solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynaklar ekleme](solutions-resources-automation.md) Yönetimi çözümünüz için.
