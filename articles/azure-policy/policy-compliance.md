---
title: Azure İlkesi'nde uyumluluk verilerini alma
description: Azure İlkesi değerlendirmeleri ve etkileri uyumluluğunu belirler. Uyumluluk ayrıntıları almayı öğrenin.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/29/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 6b310daec67f41ba589ce279e4a2dad427adb734
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818901"
---
# <a name="getting-compliance-data"></a>Uyumluluk verilerini alma

Azure İlkesi'nin en büyük avantajlarından biri olan içgörü ve denetimler sağlar bir Abonelikteki kaynakları üzerinden veya [yönetim grubu](../azure-resource-manager/management-groups-overview.md) abonelikler. Bu denetim, yanlış konumda oluşturulan kaynaklarını genel ve tutarlı etiket kullanım zorlamayı engelleyen gibi birçok farklı şekillerde uygulanabilecek veya yapılandırmaları ve ayarları denetim mevcut kaynakları için uygun. Her durumda, veriler sağlamak ortamınızın uyumluluk durumunu anlamak, ilke tarafından oluşturulur.

Girişim atamaları ve ilke tarafından oluşturulan uyumluluk bilgileri erişmek için çeşitli yollar vardır:

- Kullanarak [Azure portalı](#portal)
- Aracılığıyla [komut satırı](#command-line) betik oluşturma

Uyumluluk üzerinde yöntemleri bakarak önce uyumluluk bilgilerini güncelleştirildiğinde ve sıklığı ve değerlendirme döngüsü tetikleyen olayları göz atalım.

> [!WARNING]
> Uyumluluk durumu olarak bildirildiğinden, **'#yok'**, doğrulayın **Microsoft.policyınsights** kaynak sağlayıcısı kaydedildikten ve kullanıcının uygun rol tabanlı erişim denetimi (RBAC) sahip izinler açıklandığı [burada](azure-policy-introduction.md#rbac-permissions-in-azure-policy).

## <a name="evaluation-triggers"></a>Değerlendirme Tetikleyicileri

Tamamlanan değerlendirme döngüsü sonuçlarını yansıtılır `Microsoft.PolicyInsights` kaynak sağlayıcısı aracılığıyla `PolicyStates` ve `PolicyEvents` operations. İlke Insights REST API özellikleri ve seçenekleri hakkında daha fazla bilgi için bkz: [ilke görüşleri](/rest/api/policy-insights/).

Atanan ilkeleri ve girişimler değerlendirmeleri çeşitli olayları sonucu olarak ortaya çıkar:

- Yeni bir ilke veya girişim bir kapsama atanmış. Böyle bir durumda, tanımlanan kapsamına uygulanacak ataması için yaklaşık 30 dakika sürer. Bunu uygulandıktan sonra yeni atanan ilke veya girişim ve ilke tarafından kullanılan etkilerine bağlı olarak, kapsamı içindeki kaynaklar için değerlendirme döngüsü başlatır veya girişim, kaynaklar uyumlu veya uyumsuz olarak işaretlenir. Bu yüzden zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir beklentisi tamamlayacak bir ilke veya girişim kaynaklarının büyük bir kapsam karşı değerlendirilir zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.
- Bir ilke veya girişim bir kapsama atanmış güncelleştirilir. Bu senaryo için zamanlama ve değerlendirme döngüsü aynıdır bir kapsam için yeni bir atama.
- Bir kaynak atama Resource Manager, REST, Azure CLI veya Azure PowerShell aracılığıyla bir kapsamla dağıtılır. Bu senaryoda, geçerli olay (ekleme, Denetim, reddetme, dağıtım) ve tek tek kaynak için uyumlu durumu bilgileri kullanılabilir portal ve SDK'ları yaklaşık 15 dakika sonra. Bu olay, bir değerlendirme diğer kaynakların neden olmaz.
- Standart uyumluluk değerlendirme döngüsü. Her 24 saatte bir kez atamaları otomatik olarak tekrar değerlendirilir. Bu yüzden zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir beklentisi tamamlayacak bir ilke veya girişim kaynaklarının büyük bir kapsam karşı değerlendirilir zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.

## <a name="how-compliance-works"></a>Uyumluluk nasıl çalışır?

Atama, bir kaynak ilke veya girişim kuralları izleyin değil, uyumlu değil. Aşağıdaki tabloda, farklı ilke efektler elde edilen uyumluluk durumu için Koşul değerlendirmesi ile çalışma gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu Değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Örneğin, ortak ağlara sunulan bazı depolama hesapları (kırmızı renkte vurgulanmış) ile bir kaynak grubu – ContsoRG, olduğunu varsaymaktadır.

![Ortak ağlara maruz depolama hesapları](media/policy-insights/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke ataması oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Üç uyumlu olmayan depolama hesapları, sonuç durumlarına değiştirme denetimleri **uyumlu değil.**

![Uyumlu olmayan depolama hesapları denetlendi](media/policy-insights/resource-group03.png)

## <a name="portal"></a>Portal

Azure portalında bir grafik deneyimi Görselleştirme ve anlama ortamınızın uyumluluk durumunu gösterir. Üzerinde **ilke** sayfasında **genel bakış** seçeneği kullanılabilir kapsamlarda uyumluluk ilkeleri ve girişimler için Ayrıntılar sağlar. Uyumluluk durumu ve başına atama sayısı ek olarak, son yedi güne uyumluluk gösteren bir grafiği içerir. **Uyumluluk** sayfası (grafik dışında) aynı bilgilerin çoğunu içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

![İlke uyumluluk sayfası](media/policy-compliance/compliance-page.png)

Bir ilke veya girişim farklı kapsamlara atanabilir gibi tablodaki her atama ve bu kapsama atanmış tanım türü için kapsam unutmayın. Uyumlu olmayan ilkeler ve uyumlu olmayan kaynakları her atama için de sağlanır. Bir ilke veya girişim tabloda tıklayarak bu atama için Uyumluluk, daha kapsamlı bir bakış sağlar.

![İlke uyumluluk ayrıntıları](media/policy-compliance/compliance-details.png)

While kaynakların listesini **uyumlu olmayan kaynaklar** sekmesinde mevcut kaynakların geçerli atamanın, olayları için değerlendirme durumunu yansıtır (ekleme, Denetim, reddetme, dağıtım) kaynak oluşturmak için istek tarafından tetiklenen olan altında gösterilen **olayları** sekmesi.

![İlke uyumluluk olayları](media/policy-compliance/compliance-events.png)

Olay hakkında daha ayrıntılı bilgi toplamak ve seçmek için istediğiniz satıra sağ **etkinlik günlüklerini göster**. Etkinlik günlüğü sayfasında açılır ve atama ve olayların ayrıntılarını gösteren arama önceden filtre uygulanmış. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

![İlke uyumluluk etkinlik günlüğü](media/policy-compliance/compliance-activitylog.png)

## <a name="command-line"></a>Komut Satırı

REST API kullanarak doğrudan portalda aynı bilgileri alınabilir (dahil olmak üzere [ARMClient](https://github.com/projectkudu/ARMClient)) veya Azure PowerShell, REST API'si ile. REST API ile ilgili tüm ayrıntılar için bkz. [ilke görüşleri](/rest/api/policy-insights/) başvuru. REST API başvuru sayfalarına deneyin olanak tanıyan her işlemi ''deneyin It bir yeşil düğmeyi sahip sağdaki tarayıcıdaki.

Azure PowerShell'de aşağıdaki örnekleri kullanmak için bir kimlik doğrulama belirteci ile bu kod örneği oluşturun. Ardından $restUri örneklerde ardından ayrıştırılabilir bir JSON nesnesi almak için istediğiniz dizeyle değiştirin.

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

### <a name="summarize-results"></a>Sonuçlarını özetleme

REST API kullanarak, yönetim grubu, abonelik, kaynak grubu, kaynak, girişim, ilke, abonelik düzeyi atamasını veya kaynak grubu düzeyi atamasını özetleme gerçekleştirilebilir. İşte bir örnek ilke InSight'ın kullanarak abonelik düzeyinde özetleme [özetlemek için abonelik](/rest/api/policy-insights/policystates/summarizeforsubscription):

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

Çıkış abonelik özetler. Aşağıdaki örnek çıktıda özetlenen uyumluluk altındadır **value.results.nonCompliantResources** ve **value.results.nonCompliantPolicies**. Bu istek hakkında daha fazla ayrıntı, uyumlu olmayan sayılar ve her bir atama için tanım bilgisi yapılan her atama dahil olmak üzere sağlar. Hiyerarşideki her ilke nesnesi sağlayan bir **queryResultsUri** o seviyede ek bilgi almak için kullanılabilir.

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

Yukarıda, örneğinde **value.policyAssignments.policyDefinitions.results.queryResultsUri** tüm uyumlu olmayan kaynaklar için belirli bir ilke tanımı almak için örnek URI ile sağlanan. Bakarak **$filter** değeri, IsCompliant eşittir (eq) false olarak PolicyAssignmentId ilke tanımı, ardından Policydefinitionıd için belirtilir.
Birden çok ilke veya girişim atamaları kapsamları çeşitli Policydefinitionıd var olabilir çünkü PolicyAssignmentId filtreye dahil olmak üzere nedenidir. PolicyAssignmentId hem Policydefinitionıd belirterek, biz arıyoruz için sonuçları açık olabilir. Daha önce kullandığımız **son** PolicyStates için (değeri için izin verilen tek **policyStatesSummaryResource** özetlemek için abonelik işlecinde), otomatik olarak ayarlayan bir  **gelen** ve **için** son 24 saatlik zaman penceresi.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt uzatmamak için tek bir uyumsuz kaynağı gösterecek şekilde kesildikten (unutmayın @odata.count gerçekten 15'tir ve uyumlu olmayan kaynakları Yukarıdaki örnekteki sayısı ile eşleşir). Ayrıntılı yanıt birkaç bölümü ele alınmakta kaynak, ilke (veya girişim), ilgili verileri sağlar ve atama. Ayrıca ilke tanımını atama parametreleri geçirilen gördüğünüzden dikkat edin.

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

Bir kaynak oluşturulduğunda veya güncelleştirildiğinde, ilke değerlendirme sonucu üretilir. Sonuçları çağrılır _ilke olaylarını_. Abonelikle ilişkili son ilke olaylarını görüntülemek için aşağıdaki URI'ı kullanın.

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

İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz. [ilke olaylarını](/rest/api/policy-insights/policyevents) başvurusu makalesinde.

### <a name="azure-powershell"></a>Azure PowerShell

İlke için Azure PowerShell modülü, PowerShell Galerisi'nde kullanılabilir [AzureRM.PolicyInsights](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights). PowerShellGet kullanarak modülü kullanarak yükleyebilirsiniz `Install-Module -Name AzureRM.PolicyInsights` (en son sahip olduğunuzdan emin olun [Azure PowerShell](/powershell/azure/install-azurerm-ps) yüklü):

```powershell
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

Modül üç cmdlet vardır:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Örnek: durumu için en üstteki atanan ilke ile uyumlu olmayan kaynakları en yüksek sayısını özeti alınıyor.

```powershell
PS > Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: en son kaynak değerlendirilmesi için durum kaydı alma (varsayılan değer azalan zaman damgası tarafından).

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

Örnek: tüm uyumlu sanal ağ kaynakları için Ayrıntılar alınıyor.

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

Örnek: belirli bir tarihten sonra oluştu uyumlu olmayan bir sanal ağ kaynakları ile ilgili olayları alınıyor.

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

**PrincipalOid** alan, belirli bir kullanıcının Azure PowerShell cmdlet'iyle almak için kullanılabilir `Get-AzureRmADUser`. Değiştirin **{principalOid}** yanıt veren önceki örnekten alın.

```powershell
PS > (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Varsa bir [Log Analytics](../log-analytics/log-analytics-overview.md) çalışma alanıyla `AzureActivity` çözüm aboneliğinize bağlı, sonra da basit Kusto sorgu kullanarak değerlendirme döngüsü uyumsuzluk sonuçları görüntüleyebilirsiniz ve `AzureActivity` tablo. Log analytics'te uyumsuzluk ayrıntılarla Bu ayrıca uyarılar uyumsuzluğu belirli bir kaynağa, kaynak grubu veya hatta bir eşiği, son 24 saat içinde 10'dan fazla gibi uyumlu olmayan öğelerin izlemek üzere yapılandırılabilir anlamına gelir.

![Log Analytics kullanarak ilke uyumluluğu](media/policy-compliance/compliance-loganalytics.png)

## <a name="next-steps"></a>Sonraki adımlar

- [İlke tanım yapısını](policy-definition.md) gözden geçirin.
- [İlkenin etkilerini anlama](policy-effects.md) konusunu gözden geçirin.
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../azure-resource-manager/management-groups-overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz
