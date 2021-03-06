---
title: İlke uyumluluk verilerini alma
description: Azure İlkesi değerlendirmeleri ve etkileri uyumluluğunu belirler. Uyumluluk ayrıntıları almayı öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/01/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 428a1614889409300064420e1d3d4fbc0423a0ec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237523"
---
# <a name="get-compliance-data-of-azure-resources"></a>Azure kaynaklarınızın uyumluluk verilerini al

Azure İlkesi'nin en büyük avantajlarından biri olan içgörü ve denetimler sağlar bir Abonelikteki kaynakları üzerinden veya [yönetim grubu](../../management-groups/overview.md) abonelikler. Bu denetim, yanlış konumda oluşturulan kaynaklarını genel ve tutarlı etiket kullanım zorlamayı engelleyen gibi birçok farklı şekillerde uygulanabilecek veya yapılandırmaları ve ayarları denetim mevcut kaynakları için uygun. Her durumda, veriler Azure ortamınızın uyumluluk durumunu anlamak sağlamak için ilke tarafından oluşturulur.

Girişim atamaları ve ilke tarafından oluşturulan uyumluluk bilgileri erişmek için çeşitli yollar vardır:

- Kullanarak [Azure portalı](#portal)
- Aracılığıyla [komut satırı](#command-line) betik oluşturma

Uyumluluk üzerinde yöntemleri bakarak önce uyumluluk bilgilerini güncelleştirildiğinde ve sıklığı ve değerlendirme döngüsü tetikleyen olayları göz atalım.

> [!WARNING]
> Uyumluluk durumu olarak bildirildiğinden, **kayıtlı**, doğrulayın **Microsoft.policyınsights** kaynak sağlayıcısı kaydedildikten ve kullanıcı uygun rol tabanlı erişim denetimi ( Bölümünde anlatıldığı gibi RBAC) izinlerinin [Azure İlkesi'nde RBAC](../overview.md#rbac-permissions-in-azure-policy).

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="evaluation-triggers"></a>Değerlendirme Tetikleyicileri

Tamamlanan değerlendirme döngüsü sonuçlarını kullanılabilir `Microsoft.PolicyInsights` kaynak sağlayıcısı aracılığıyla `PolicyStates` ve `PolicyEvents` operations. Azure İlkesi Insights REST API işlemleri hakkında daha fazla bilgi için bkz. [Azure ilke görüşleri](/rest/api/policy-insights/).

Atanan ilkeleri ve girişimler değerlendirmeleri çeşitli olayları sonucu olarak ortaya çıkar:

- Yeni bir ilke veya girişim bir kapsama atanmış. Tanımlanan kapsamına uygulanacak ataması için yaklaşık 30 dakika sürer. Bunu uygulandıktan sonra yeni atanan ilke veya girişim ve ilke tarafından kullanılan etkilerine bağlı olarak, kapsamı içindeki kaynaklar için değerlendirme döngüsü başlatır veya girişim, kaynaklar uyumlu veya uyumsuz olarak işaretlenir. Bir ilke veya girişim kaynaklarının büyük bir kapsam karşı değerlendirilir zaman alabilir. Bu nedenle, değerlendirme döngüsü tamamlanır, önceden tanımlanmış hiçbir beklentisi yoktur. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.

- Bir ilke veya girişim bir kapsama atanmış güncelleştirilir. Bu senaryo için zamanlama ve değerlendirme döngüsü aynıdır bir kapsam için yeni bir atama.

- Bir kaynak atama Resource Manager, REST, Azure CLI veya Azure PowerShell aracılığıyla bir kapsamla dağıtılır. Bu senaryoda, geçerli olay (ekleme, Denetim, reddetme, dağıtım) ve tek tek kaynak için uyumlu durumu bilgileri kullanılabilir portal ve SDK'ları yaklaşık 15 dakika sonra. Bu olay, bir değerlendirme diğer kaynakların neden olmaz.

- Standart uyumluluk değerlendirme döngüsü. Her 24 saatte bir kez atamaları otomatik olarak yeniden hesaplanır. Bu yüzden zaman değerlendirme döngüsünü, önceden tanımlanmış hiçbir beklentisi tamamlayacak bir ilke veya girişim birçok kaynak zaman alabilir. İşlem tamamlandıktan sonra güncelleştirilmiş uyumluluk sonuçları portal ve SDK'ları kullanılabilir.

- [Konuk yapılandırma](../concepts/guest-configuration.md) kaynak sağlayıcısı tarafından yönetilen bir kaynağın uyumluluk ayrıntıları ile güncelleştirilir.

- İsteğe bağlı tarama

### <a name="on-demand-evaluation-scan"></a>İsteğe bağlı değerlendirme taraması

Bir abonelik veya kaynak grubu için bir değerlendirme taraması, REST API çağrısı ile başlatılabilir. Bu tarama zaman uyumsuz bir işlemdir. Bu nedenle, tarama yanıt tamamlanana kadar tarama başlatmak için REST uç noktasını beklemez. Bunun yerine, istenen değerlendirme durumunu sorgulamak için bir URI sağlar.

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourRG}` -Kaynak grubunuzun adıyla değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

Tarama, bir abonelik veya kaynak grubundaki kaynakların değerlendirme destekler. Bir tarama kapsama göre bir REST API'sini kullanmaya başlama **POST** komutunu aşağıdaki URI yapıları kullanarak:

- Abonelik

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

- Kaynak grubu

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

Çağrı döndürür bir **202 kabul edildi** durumu. Dahil edilen yanıtta başlığıdır bir **konumu** cfg aşağıdaki formatta özelliği:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2018-07-01-preview
```

`{ResourceContainerGUID}` İstenen kapsam için statik olarak oluşturulur. Bir kapsam bir isteğe bağlı tarama zaten çalışıyorsa, yeni bir tarama başlatıldı değil. Yeni istek aynı bunun yerine, sağlanan `{ResourceContainerGUID}` **konumu** durumu için URI. REST API **alma** komutunu **konumu** URI'sini döndürür bir **202 kabul edildi** değerlendirme devam ederken. Değerlendirme taraması tamamlandıktan sonra döndürür bir **200 Tamam** durumu. Tamamlanmış bir tarama durumuyla birlikte bir JSON yanıt gövdesidir:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>Uyumluluk nasıl çalışır?

Atama, bir kaynaktır **uyumlu** ilke veya girişim kuralların izleyin değil ise.
Aşağıdaki tabloda, farklı ilke efektler elde edilen uyumluluk durumu için Koşul değerlendirmesi ile çalışma gösterilmektedir:

| Kaynak durumu | Etki | İlke değerlendirmesi | Uyumluluk durumu |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Uyumlu değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | True | Uyumlu değil |
| Yeni | Audit, AuditIfNotExist\* | False | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

Örneğin, ortak ağlara sunulan bazı depolama hesapları (kırmızı renkte vurgulanmış) ile bir kaynak grubu – ContsoRG, olduğunu varsaymaktadır.

![Ortak ağlara maruz depolama hesapları](../media/getting-compliance-data/resource-group01.png)

Bu örnekte, güvenlik risklerini dikkatli olmanız gerekir. Bir ilke ataması oluşturduğunuza göre ContosoRG kaynak grubundaki tüm depolama hesapları için değerlendirilir. Üç uyumlu olmayan depolama hesapları, sonuç durumlarına değiştirme denetimleri **uyumlu.**

![Uyumlu olmayan depolama hesapları denetlendi](../media/getting-compliance-data/resource-group03.png)

Yanında **uyumlu** ve **uyumlu**, ilkeleri ve kaynakları diğer üç durumu vardır:

- **Çakışan**: İki veya daha fazla İlkesi ile çakışan kurallar yok. Örneğin, iki ilke aynı etiketi farklı değerlerle ekleme.
- **Başlatılmamış**: Değerlendirme döngüsü, ilke veya kaynak için başlatılmadı.
- **Kayıtlı**: Azure İlkesi kaynak sağlayıcısına kayıtlı olmayan veya oturum açmış hesabın uyumluluk verilerini okuma izni yok.

Azure İlkesi kullanan **türü** ve **adı** alan tanımında bir kaynak bir eşleşme olup olmadığını belirlemek için. Kaynak eşleştiğinde, geçerli olarak kabul edilir ve durumu ya da **uyumlu** veya **uyumlu**. Ya da **türü** veya **adı** tüm kaynakların uygun olarak kabul edilir ve değerlendirilir tanımındaki özelliktir.

Uyumluluk yüzdesi bölünmesiyle belirlenir **uyumlu** kaynaklar tarafından _toplam kaynakları_.
_Toplam kaynakları_ toplamı olarak tanımlanan **uyumlu**, **uyumlu**, ve **çakışan** kaynakları. Farklı kaynaklar toplamını genel uyumluluk sayılardır **uyumlu** toplamı tüm farklı kaynaklar tarafından ayrılmış. Aşağıdaki görüntüde, geçerli olan 20 farklı kaynak vardır ve yalnızca **uyumlu**. Genel kaynak uyumluluk % 95'inden (19 / 20) ' dir.

![İlke uyumluluğunu uyumluluk sayfası örneği](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

Azure portalında bir grafik deneyimi Görselleştirme ve anlama ortamınızın uyumluluk durumunu gösterir. Üzerinde **ilke** sayfasında **genel bakış** seçeneği kullanılabilir kapsamlarda uyumluluk ilkeleri ve girişimler için Ayrıntılar sağlar. Uyumluluk durumu ve başına atama sayısı ile birlikte, son yedi güne uyumluluk gösteren bir grafiği içerir. **Uyumluluk** sayfası (grafik dışında) aynı bilgilerin çoğunu içerir, ancak ek filtreleme ve sıralama seçenekleri sağlar.

![Azure İlkesi uyumluluk sayfası örneği](../media/getting-compliance-data/compliance-page.png)

Bir ilke veya girişim farklı kapsamlara atanabilir olduğundan, tablo, her atama ve tür tanımının atandığı için kapsamı içerir. Uyumlu olmayan kaynakları ve her atama için uyumlu olmayan ilkeler de sağlanır. Bir ilke veya girişim tabloda tıklayarak bu atama için Uyumluluk, daha kapsamlı bir bakış sağlar.

![Azure İlkesi uyumluluk Ayrıntıları sayfası örneği](../media/getting-compliance-data/compliance-details.png)

Kaynakları listesini **kaynak Uyumluluk** sekmesinde mevcut kaynaklar mevcut atamanın için değerlendirme durumunu gösterir. Varsayılanları sekmesi **uyumlu**, ancak filtrelenebilir.
Olaylar (ekleme, Denetim, reddetme, dağıtım) kaynak oluşturmak için istek tarafından tetiklenen altında gösterilen **olayları** sekmesi.

![Azure İlkesi uyumluluk olayları örneği](../media/getting-compliance-data/compliance-events.png)

Olay hakkında daha ayrıntılı bilgi toplamak ve seçmek için istediğiniz satıra sağ **etkinlik günlüklerini göster**. Etkinlik günlüğü sayfasında açılır ve atama ve olayların ayrıntılarını gösteren arama önceden filtre uygulanmış. Etkinlik günlüğü ek bağlam ve bu olaylar hakkında bilgi sağlar.

![Azure İlkesi uyumluluk etkinlik günlüğü örneği](../media/getting-compliance-data/compliance-activitylog.png)

### <a name="understand-non-compliance"></a>Uyumsuzluk anlama

<a name="change-history-preview"></a>

Ne zaman bir kaynakları belirlenir olmasını **uyumlu olmayan**, birçok olası nedeni vardır. Bir kaynağın nedenini belirlemek için **uyumlu olmayan** veya değişiklik sorumlu bulmak için bkz. [belirlemek uyumsuzluk](./determine-non-compliance.md).

## <a name="command-line"></a>Komut satırı

REST API ile aynı bilgileri portalda kullanılabilir alınabilir (dahil olmak üzere [ARMClient](https://github.com/projectkudu/ARMClient)) veya Azure PowerShell. REST API ile ilgili tüm ayrıntılar için bkz. [Azure ilke görüşleri](/rest/api/policy-insights/) başvuru. REST API başvuru sayfalarına deneyin olanak tanıyan her işlemi ''deneyin It bir yeşil düğmeyi sahip sağdaki tarayıcıdaki.

Azure PowerShell'de aşağıdaki örnekleri kullanmak için bir kimlik doğrulama belirteci ile bu kod örneği oluşturun. Ardından $restUri ardından ayrıştırılabilir bir JSON nesnesi almak için örnekler dizeyi değiştirin.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

$azContext = Get-AzContext
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

REST API ile kapsayıcı, tanımını veya atamasını özetleme gerçekleştirilebilir. İşte bir örnek kullanarak Azure İlkesi InSight'ın abonelik düzeyinde özetleme [özetlemek için abonelik](/rest/api/policy-insights/policystates/summarizeforsubscription):

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

Yukarıdaki örnekte **value.policyAssignments.policyDefinitions.results.queryResultsUri** belirli bir ilke tanımı için tüm uyumlu olmayan kaynaklar için örnek URI sağlar. Bakarak **$filter** değeri, IsCompliant eşittir (eq) false olarak PolicyAssignmentId ilke tanımı, ardından Policydefinitionıd için belirtilir. Birden çok ilke veya girişim atamaları farklı kapsamlarla Policydefinitionıd var olabilir çünkü PolicyAssignmentId filtreye dahil olmak üzere nedenidir. PolicyAssignmentId hem Policydefinitionıd belirterek, biz bekliyoruz sonuçlarında açık olabilir. Daha önce PolicyStates için kullandığımız **son**, otomatik olarak ayarlayan bir **gelen** ve **için** son 24 saatlik zaman penceresi.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Aşağıdaki örnek yanıt, tek bir uyumsuz kaynağı kısaltma için kırpılır. Ayrıntılı yanıt, veri kaynağı, ilke veya girişim ve atama hakkında birkaç bölümü ele alınmakta sahiptir. Ayrıca ilke tanımını atama parametreleri geçirilen gördüğünüzden dikkat edin.

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

İlke olaylarını sorgulama hakkında daha fazla bilgi için bkz. [Azure ilke olaylarını](/rest/api/policy-insights/policyevents) başvurusu makalesinde.

### <a name="azure-powershell"></a>Azure PowerShell

Azure ilkesi için Azure PowerShell modülü, PowerShell Galerisi'ndeki kullanılabilir [Az.PolicyInsights](https://www.powershellgallery.com/packages/Az.PolicyInsights).
PowerShellGet kullanarak modülü kullanarak yükleyebilirsiniz `Install-Module -Name Az.PolicyInsights` (en son sahip olduğunuzdan emin olun [Azure PowerShell](/powershell/azure/install-az-ps) yüklü):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name Az.PolicyInsights

# Import the downloaded module
Import-Module Az.PolicyInsights

# Login with Connect-AzAccount if not using Cloud Shell
Connect-AzAccount
```

Modülü aşağıdaki cmdlet'leri içerir:

- `Get-AzPolicyStateSummary`
- `Get-AzPolicyState`
- `Get-AzPolicyEvent`
- `Get-AzPolicyRemediation`
- `Remove-AzPolicyRemediation`
- `Start-AzPolicyRemediation`
- `Stop-AzPolicyRemediation`

Örnek: Durum için en üstteki atanan ilke ile uyumlu olmayan kaynakları en yüksek sayısını özeti alınıyor.

```azurepowershell-interactive
PS> Get-AzPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Örnek: En son kaynak değerlendirilmesi için durum kaydı alma (varsayılan değer azalan zaman damgası tarafından).

```azurepowershell-interactive
PS> Get-AzPolicyState -Top 1

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

Örnek: Tüm uyumlu sanal ağ kaynakları için Ayrıntılar alınıyor.

```azurepowershell-interactive
PS> Get-AzPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

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

Örnek: Belirli bir tarihten sonra oluştu uyumlu olmayan bir sanal ağ kaynakları ile ilgili olayları alınıyor.

```azurepowershell-interactive
PS> Get-AzPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

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

**PrincipalOid** alan, belirli bir kullanıcının Azure PowerShell cmdlet'iyle almak için kullanılabilir `Get-AzADUser`. Değiştirin **{principalOid}** yanıt veren önceki örnekten alın.

```azurepowershell-interactive
PS> (Get-AzADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Varsa bir [Log Analytics çalışma alanı](../../../log-analytics/log-analytics-overview.md) ile `AzureActivity` gelen [Activity Log Analytics çözümünü](../../../azure-monitor/platform/activity-log-collect.md) aboneliğinize bağlı, uyumsuzluk sonuçları değerlendirme döngüsü kullanarak da görüntüleyebilirsiniz Basit Kusto sorgu ve `AzureActivity` tablo. Azure İzleyici günlüklerine ayrıntılarla uyumsuzluk için izlemek için uyarılar yapılandırılabilir.


![Azure İzleyici günlüklerini kullanarak Azure ilke uyumluluğu](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md).
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md).
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları düzeltme](remediate-resources.md).
- Bir yönetim grubu olan gözden geçirme [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md).