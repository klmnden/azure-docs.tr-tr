---
title: Azure ilkesinde uyumluluk verilerini alma
description: Azure ilke değerlendirmeleri ve etkileri uyumluluk belirler. Uyumluluk ayrıntıları alma hakkında bilgi.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d36ecb18811901fb781e151c06badc0697c2d769
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655399"
---
# <a name="getting-compliance-data"></a>Uyumluluk verilerini alma

Azure ilkesinin en büyük avantajlarından biri olan Insight and sağlayan bir abonelikte kaynakları üzerinden denetimleri veya [yönetim grubu](../azure-resource-manager/management-groups-overview.md) aboneliklerin. Bu denetimin yanlış konumda oluşturulan kaynakları genel ve tutarlı etiketi kullanımı, zorlama önleme gibi birçok farklı yolla kullandı veya uygun denetim mevcut kaynakları için yapılandırmalar ve ayarlar. Her durumda, veriler, ortamınızın uyumluluk durumunu anlamak etkinleştirmek için ilke tarafından oluşturulur.

İlke ve girişimi atamaları tarafından oluşturulan uyumluluk bilgilerine erişmek için birkaç yolu vardır:

- Kullanarak [Azure portalı](#portal)
- Aracılığıyla [komut satırı](#command_line) komut dosyası oluşturma

Yöntemleri hakkında rapor bakarak önce uyumluluk bilgileri güncelleştirildiğinde ve sıklığı ve değerlendirme döngüsü tetikleyen olaylar bakalım.

## <a name="evaluation-triggers"></a>Değerlendirme Tetikleyicileri

Tamamlanan değerlendirme döngüsü sonuçlarını yansıtılır `Microsoft.PolicyInsights` kaynak sağlayıcısı aracılığıyla `PolicyStates` ve `PolicyEvents` işlemleri. Seçenekler ve Özellikler İlkesi Öngörüler REST API hakkında daha fazla bilgi için bkz: [İlkesi Öngörüler](/rest/api/policy-insights/).

Atanan ilkeleri ve girişimleri değerlendirmeleri çeşitli olayları sonucu olarak ortaya çıkar:

- Yeni bir ilke veya Initiative bir kapsamına atanır. Bu durumda, tanımlanmış kapsama uygulanacak ataması için yaklaşık 30 dakika sürer. Bu uygulandıktan sonra yeni atanan ilke veya Initiative karşı ve ilke tarafından kullanılan etkileri bağlı olarak, kapsamı içindeki kaynaklar için değerlendirme döngüsü başlar veya girişimi, kaynakları uyumlu veya uyumsuz olarak işaretlenir. Böylece zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir Beklenti tamamlayacak büyük ilke veya büyük kapsamını kaynaklara karşı değerlendirilen girişimi zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçlarını portal ve SDK'ları kullanılabilir.
- Bir ilke veya bir kapsama atanmış Initiative güncelleştirilir. Bu senaryo için zamanlama ve değerlendirme döngüsü yeni atama bir kapsam için aynı olur.
- Bir kaynak Resource Manager, REST, Azure CLI veya Azure PowerShell aracılığıyla atama bir kapsamla dağıtılır. Bu senaryoda, geçerli olay (ekleme, Denetim, reddetme, dağıtmak) ve uyumlu durum bilgisi kullanılabilir portal ve SDK'ları yaklaşık 15 dakika sonra.
- Standart uyumluluk değerlendirme döngüsü. Her 24 saatte bir kez atamaları otomatik olarak tekrar değerlendirilir. Böylece zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir Beklenti tamamlayacak büyük ilke veya büyük kapsamını kaynaklara karşı değerlendirilen girişimi zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçlarını portal ve SDK'ları kullanılabilir.

## <a name="how-compliance-works"></a>Uyumluluk nasıl çalışır?

Atama, ilke veya girişimi kuralları izlerseniz değil, uyumlu olmayan bir kaynak değildir. Aşağıdaki tabloda, sonuçta elde edilen uyumluluk durumu için koşulu değerlendirmesi etkileri çalışmak nasıl farklı ilke gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu Değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Kaynakları nasıl uyumsuz olarak işaretlenmiş daha iyi anlamak için yukarıda oluşturduğunuz ilke ataması örnek kullanalım.

Örneğin, ortak ağlara gösterilen bazı depolama hesapları (kırmızı ile vurgulanan) olan bir kaynak grubu – ContsoRG, olduğunu varsayalım.

![Ortak ağlara gösterilen depolama hesapları](media/policy-insights/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke atamasını oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Sonuç olarak durumlarına değiştirme üç uyumlu olmayan depolama hesaplarını denetimleri **uyumsuz.**

![Uyumlu olmayan depolama hesaplarını denetleniyor](media/policy-insights/resource-group03.png)

## <a name="portal"></a>Portal

Azure portalı Görselleştirme ve ortamınızdaki uyumluluk durumunu anlama bir grafik deneyimi gösterir. Üzerinde **İlkesi** sayfasında **genel bakış** seçeneği uyumluluk ilkeleri ve girişimleri üzerinde kullanılabilir kapsamları için ayrıntıları sağlar. Uyumluluk durumu ve atama başına sayısı ek olarak, son yedi gün boyunca uyumluluk gösteren bir grafik içerir. **Uyumluluk** sayfası (dışında grafik) aynı bilgilerin çoğunu içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

![İlke uyumluluk sayfası](media/policy-compliance/compliance-page.png)

Bir ilke veya girişimi için farklı kapsamlar atanabilir gibi tablodaki her atama ve bu kapsama atanmış tanım türü kapsamın unutmayın. Uyumlu olmayan ilkeleri ve her bir atama için uyumlu olmayan kaynaklar sayısını da sağlanır. Bir ilke veya tablosundaki Initiative tıklayarak belirli bu atama için Uyumluluk adresindeki daha derinlikli bir bakış sağlar.

![İlke uyumluluk ayrıntıları](media/policy-compliance/compliance-details.png)

Kaynak listesi üzerinde while **uyumsuz kaynakları** sekmesi mevcut kaynakların geçerli ataması, olaylar için değerlendirme durumunu yansıtır (ekleme, Denetim, reddetme, dağıtmak) bir kaynak oluşturmak için isteğiyle tetiklenen olan altında gösterilen **olayları** sekmesi.

![İlke uyumluluk olayları](media/policy-compliance/compliance-events.png)

Daha ayrıntılı bilgi toplama ve seçmek için istediğiniz olay satırındaki sağ **Göster etkinlik günlükleri**. Etkinlik günlüğü sayfasında açılır ve atama ve olayları ayrıntılarını gösteren arama önceden filtre uygulanmış. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

![İlke uyumluluk etkinlik günlüğü](media/policy-compliance/compliance-activitylog.png)

## <a name="command-line"></a>Komut Satırı

REST API kullanarak doğrudan portalda kullanılabilir aynı bilgi alınabilir (dahil olmak üzere [ARMClient](https://github.com/projectkudu/ARMClient)) veya Azure PowerShell REST API ile. REST API hakkında tam bilgi için bkz: [İlkesi Öngörüler](/rest/api/policy-insights/) başvuru. REST API başvuru sayfaları denemek üzere izin veren her bir işlemin üzerinde bir yeşil ' deneyin ' düğmesini sahip tarayıcıda sağ.

Aşağıdaki örnekler Azure PowerShell'de kullanmak için bu örnek kod ile kimlik doğrulama belirtecini oluşturun. Ardından $restUri sonra ayrıştırılabilir bir JSON nesnesi almak için örneklerde istenen dizesiyle değiştirin.

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Define the REST API to communicate with
# Use double quotes for $restUri as some endpoints take strings passed in single quotes
$restUri = "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04"

# Invoke the REST API
$response = Invoke-RestMethod -Uri $restUri -Method POST -Headers $authHeader

# View the response object (as JSON)
$response
```

### <a name="summarize-results"></a>Sonuçlarını özetler

REST API kullanarak, yönetim grubu, abonelik, kaynak grubu, kaynak, girişimi, ilke, abonelik düzeyi atama veya kaynak grubu düzeyi ataması özetleme gerçekleştirilebilir. Örneği, ilke InSight'ın kullanılarak abonelik düzeyinde özetleme [özetlemek için abonelik](/rest/api/policy-insights/policystates/summarizeforsubscription):

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

Çıktı abonelik özetler. Aşağıdaki örnek çıktıda özetlenen uyumluluk olan altında **value.results.nonCompliantResources** ve **value.results.nonCompliantPolicies**. Bu istek, daha fazla uyumlu olmayan sayılar ve her bir atama için tanım bilgisi yapılan her bir atama gibi ayrıntıları sağlar. Hiyerarşideki her ilke nesnesi sağlar bir **queryResultsUri** düzeyde ek ayrıntılar elde etmek için kullanılabilir.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Kaynaklar için sorgu

Yukarıda, örneğinde **value.policyAssignments.policyDefinitions.results.queryResultsUri** bize uyumlu olmayan tüm kaynaklar için belirli bir ilke tanımı almak için örnek URI ile sağlanan. Bakarak **$filter** değeri IsCompliant eşittir (eq) false olarak PolicyAssignmentId ilke tanımı, ardından Policydefinitionıd için belirtilir.
Policydefinitionıd birkaç ilke veya kapsamları çeşitli girişimi atamaları mevcut çünkü PolicyAssignmentId filtreye dahil olmak üzere nedenidir. PolicyAssignmentId ve Policydefinitionıd belirterek, sizi arıyoruz için sonuçlarında açık olabilir. Daha önce kullandık **son** PolicyStates için (yalnızca izin verilen değeri **policyStatesSummaryResource** özetlemek için abonelik işlecinde), hangi otomatik olarak ayarlar bir  **gelen** ve **için** zaman penceresi son 24 saat.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt kısaltma tek uyumlu olmayan bir kaynak gösterecek şekilde kesildikten (unutmayın @odata.count gerçekte 15'tir ve uyumlu olmayan kaynaklardan Yukarıdaki örnek sayısı ile eşleşen). Veri kaynağı, ilke (veya Initiative) ilgili çeşitli parçalarını ayrıntılı yanıt sağlar ve atama. Ayrıca hangi atama parametreler için ilke tanımı geçirilmiş gördüğünüzden dikkat edin.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Etkinlikleri görüntüleme

Bir kaynak oluşturulduğunda veya güncelleştirildiğinde, ilke değerlendirme sonucu üretilir. Sonuçları çağrılır _ilke olayları_. Abonelikle ilişkili son ilke olayları görüntülemek için aşağıdaki URI'ı kullanın.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

Sonuçlarınız aşağıdaki örneğe benzer:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz: [ilke olayları](/rest/api/policy-insights/policyevents) başvurusu makalesinde.

### <a name="azure-powershell-preview"></a>Azure PowerShell (Önizleme)

İlke henüz son, Azure PowerShell modülü ancak PowerShell Galerisi üzerinde şu anda kullanılabilir değil bir [Önizleme sürümü](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights).
PowerShellGet en az ise sürümü (sürüm öncesi öğeleri desteklemek için gereklidir) 1.6.0, önizleme sürümünü kullanarak indirebilirsiniz `Install-Module` (en son olduğundan emin olun [Azure PowerShell](/powershell/azure/install-azurerm-ps) yüklü):

```powershell
# Download preview from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights -AllowPrerelease

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

Önizleme modülü üç cmdlet vardır:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Örnek: durumu uyumlu olmayan kaynaklar en yüksek sayıda en üstteki atanan ilkesiyle için Özet alınıyor.

```powershell
PS > Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: en son kaynak değerlendirilmesi için durumu kayıt alma (varsayılan olarak azalan düzende zaman damgası tarafından).

```powershell
PS > Get-AzureRmPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Örnek: tüm uyumlu olmayan sanal ağ kaynakları için ayrıntıları alınıyor.

```powershell
PS > Get-AzureRmPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Örnek: belirli bir tarihten sonra oluştu uyumlu olmayan sanal ağ kaynaklarına ilgili olayları alınıyor.

```powershell
PS > Get-AzureRmPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

**PrincipalOid** alan, belirli bir kullanıcı Azure PowerShell cmdlet'iyle almak için kullanılabilir `Get-AzureRmADUser`. Değiştir **{principalOid}** yanıt veren önceki örnekten alın.

```powershell
PS > (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Varsa bir [günlük analizi](../log-analytics/log-analytics-overview.md) çalışma alanıyla `AzureActivity` çözüm aboneliğinize bağlı sonra basit Kusto sorgularını kullanarak değerlendirme döngüsü uyumsuzluk sonuçlarını da görüntüleyebilirsiniz ve `AzureActivity` tablo. Günlük analizi ile Ayrıntılar uyumsuzluk, bu da uyarıları uyumsuzluğun belirli bir kaynak, kaynak grubu veya bir eşik bile, birden fazla 10 son 24 saat içindeki gibi uyumlu olmayan öğelerin izlemek için yapılandırılabilir anlamına gelir.

![Günlük analizi kullanarak ilke uyumluluğu](media/policy-compliance/compliance-loganalytics.png)

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [ilke tanımı yapısını](policy-definition.md).
- Gözden geçirme [İlkesi etkilerini anlama](policy-effects.md).
- Bir yönetim grubu durumdayken İnceleme [kaynaklarınızı Azure Yönetim grupları ile düzenleme](../azure-resource-manager/management-groups-overview.md)