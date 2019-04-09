---
title: Uyumlu olmayan kaynakları düzeltme
description: Bu yöntem Azure İlkesi'nde ilkelerine uyumlu olmayan kaynakları düzeltme size kılavuzluk eder.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: fe06e7081e4e3691aeb054985f9f2f3f6dc7d19e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59283452"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Azure İlkesi ile uyumlu olmayan kaynakları Düzelt

İçin uyumlu olmayan kaynakları bir **Deployıfnotexists** İlkesi koyabilir ile uyumlu bir duruma **düzeltme**. Düzeltme çalıştırmak için ilke yönlendirerek gerçekleştirilir **Deployıfnotexists** atanan ilke mevcut kaynaklarınız üzerindeki etkisi. Bu makalede, anlama ve düzeltme İlkesi ile gerçekleştirmek için gerekli olan adımları gösterilmektedir.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="how-remediation-security-works"></a>Düzeltme güvenliği nasıl çalışır

İlke çalıştığında şablonu **Deployıfnotexists** ilke tanımı, mevcut bunu kullanarak bir [yönetilen kimliği](../../../active-directory/managed-identities-azure-resources/overview.md).
İlke, her atama için yönetilen bir kimlik oluşturur, ancak yönetilen kimlik vermek üzere hangi rolleri hakkında ayrıntılar olmalıdır. Yönetilen kimlik rolleri eksikse, ilke veya girişim ataması sırasında bu hata görüntülenir. Atama başlatıldıktan sonra portalı kullanırken, ilke otomatik olarak yönetilen kimlik listelenen rollere izin vermiş olursunuz.

![Yönetilen kimlik - eksik rol](../media/remediate-resources/missing-role.png)

> [!IMPORTANT]
> Bir kaynak tarafından değiştirilirse **Deployıfnotexists** ilke ataması veya şablon kapsamı dışında ilke ataması kapsamı dışında kaynaklara erişir özellikleri, atamanın yönetilen kimlik olmalıdır[el ile erişim izni](#manually-configure-the-managed-identity) veya düzeltme dağıtımı başarısız olur.

## <a name="configure-policy-definition"></a>İlke tanımı'nı yapılandırma

Rol tanımlamak için ilk adımıdır, **Deployıfnotexists** , dahil şablon içeriği başarıyla dağıtmak için ilke tanımında gerekir. Altında **ayrıntıları** özelliği eklemek bir **roleDefinitionIds** özelliği. Bu özellik, ortamınızdaki eşleşen dizeler dizisidir. Tam bir örnek için bkz: [Deployıfnotexists örnek](../concepts/effects.md#deployifnotexists-example).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

**roleDefinitionIds** tam kaynak kimliğini kullanır ve kısa verdiği hızlı tepkilerden faydalanamamış **roleName** rolü. Ortamınızı 'Katkıda bulunan' rolü için kimliği almak için aşağıdaki kodu kullanın:

```azurecli-interactive
az role definition list --name 'Contributor'
```

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Yönetilen kimlik el ile yapılandırma

Portalı kullanarak bir atama oluştururken, ilkeyi hem yönetilen kimlik oluşturur ve tanımlanan rolleri verir **roleDefinitionIds**. Aşağıdaki durumlarda, yönetilen bir kimlik oluşturmak ve izinleri atamak için adımları el ile yapılması gerekir:

- (Örneğin, Azure PowerShell) SDK'sı kullanırken
- Atama kapsamı dışında bir kaynağa şablon tarafından değiştirildiğinde
- Atama kapsamı dışında bir kaynağa şablon tarafından ne zaman okuma

> [!NOTE]
> Şu anda bu özelliği destekleyen yalnızca SDK'ları şunlardır: Azure PowerShell ve .NET.

### <a name="create-managed-identity-with-powershell"></a>PowerShell ile yönetilen kimlik oluşturma

İlke ataması sırasında yönetilen bir kimlik oluşturmak için **konumu** tanımlanmalıdır ve **Assignıdentity** kullanılır. Aşağıdaki örnek, yerleşik ilke tanımı alır **dağıtmak, SQL veritabanı saydam veri şifrelemesi**, hedef kaynak grubu ayarlar ve ardından ataması oluşturulur.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

`$assignment` Değişkeni artık yönetilen kimlikle birlikte bir ilke ataması oluştururken, döndürülen değerlerin standart asıl kimliği içeriyor. Aracılığıyla erişilebilir `$assignment.Identity.PrincipalId`.

### <a name="grant-defined-roles-with-powershell"></a>PowerShell ile rol verme tanımlanan

Gerekli rolleri verilmeden önce yeni bir yönetilen kimlik Azure Active Directory aracılığıyla çoğaltma tamamlamanız gerekir. Çoğaltma tamamlandıktan sonra aşağıdaki örnekte ilke tanımında yinelenen `$policyDef` için **roleDefinitionIds** ve kullandığı [yeni AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) yeni yönetilen kimlik vermek için roller.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Portal üzerinden rolleri verme tanımlanan

Portalı kullanarak tanımlanmış rollere atamanın yönetilen kimlik vermek için iki yolu vardır **erişim denetimi (IAM)** veya ilke veya girişim ataması düzenleme ve tıklatarak **Kaydet**.

Rol atama için yönetilen kimlik eklemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin.

1. Yönetilen bir kimliğe sahip ataması bulun ve adına tıklayın.

1. Bulma **atama kimliği** düzenleme sayfası özelliği. Atama kimliği gibi bir şey olacaktır:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   Yönetilen kimlik adı olduğundan ataması kaynak kimliği son bölümüdür `2802056bfc094dfb95d4d7a5` Bu örnekte. Atama kaynak kimliği. Bu bölümü kopyalayın

1. Kaynak veya el ile eklenen rol tanımı gereken kaynakları üst kapsayıcının (kaynak grubu, abonelik, yönetim grubu) gidin.

1. Tıklayın **erişim denetimi (IAM)** tıklayın ve bağlantı Kaynaklar sayfasında **+ rol ataması Ekle** erişim denetimi sayfanın üstünde.

1. Eşleşen uygun rolü seçin bir **roleDefinitionIds** ilke tanımından. Bırakın **erişim Ata** 'Azure AD kullanıcı, Grup veya uygulama' varsayılan olarak ayarla. İçinde **seçin** kutusuna yapıştırın veya önceden bulunan ataması kaynak kimliği bölümünü yazın. Arama tamamlandığında, kimliği'ni seçin ve aynı ada sahip nesneye tıklayın **Kaydet**.

## <a name="create-a-remediation-task"></a>Düzeltme görev oluşturma

### <a name="create-a-remediation-task-through-portal"></a>Portal üzerinden düzeltme görev oluşturma

Değerlendirme, ilke atamasıyla sırasında **Deployıfnotexists** etkisi, uyumlu olmayan kaynakları olup olmadığını belirler. Uyumlu olmayan kaynakları bulunduğunda ayrıntıları sağlanır **düzeltme** sayfası. Uyumlu olmayan kaynakları olan ilkeleri listesinde birlikte tetiklemeye yönelik seçeneği olan bir **düzeltme görev**. Bir dağıtımın ne oluşturur, bu seçenek, **Deployıfnotexists** şablonu.

Oluşturmak için bir **düzeltme görev**, şu adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

   ![Tüm hizmetler ilkesinde arayın](../media/remediate-resources/search-policy.png)

1. Seçin **düzeltme** Azure İlkesi sayfasının sol tarafındaki.

   ![Düzeltme İlkesi sayfasında seçin](../media/remediate-resources/select-remediation.png)

1. Tüm **Deployıfnotexists** uyumlu olmayan kaynakları olan ilke atamaları eklenir **düzeltmeye yönelik ilkeler** sekmesi ve veri tablosu. Bir ilkeyle uyumlu olmayan kaynakları tıklayın. **Yeni bir düzeltme görev** sayfası açılır.

   > [!NOTE]
   > Açmak için alternatif bir yolu **düzeltme görev** sayfasıdır bulup ilkeden tıklayarak **Uyumluluk** sayfasında'a tıklayın **düzeltme Görevi Oluştur** düğmesi.

1. Üzerinde **yeni bir düzeltme görev** sayfasında, kaynakları kullanarak düzeltmek için filtre **kapsam** alt kaynakları burada ilkenin atandığı seçmek için üç nokta simgesini (aşağı ayrı kaynak dahil nesneler). Ayrıca, **konumları** daha da fazla filtrelemek için kaynakları açılır. Yalnızca kaynak tabloda listelenen düzeltilebilir.

   ![Düzelt - düzeltmek için kaynakları seçin](../media/remediate-resources/select-resources.png)

1. Kaynakları tıklayarak filtrelendi sonra düzeltme görevi Başlat **düzelt**. İlke uyumluluk sayfası açılacak **düzeltme görevleri** görevleri ilerleme durumunu göstermek için sekmesinde.

   ![Düzeltme - düzeltme görevlerin ilerlemesini](../media/remediate-resources/task-progress.png)

1. Tıklayarak **düzeltme görev** İlkesi uyumluluk sayfasından ilerleme durumu hakkında ayrıntılı bilgi edinmek için. Görev için kullanılan filtreleme düzeltilen kaynakların listesini birlikte gösterilir.

1. Gelen **düzeltme görev** sayfasında, ya da düzeltme görevin dağıtım görüntülemek için bir kaynak veya kaynak üzerinde sağ tıklayın. Satırın sonunda tıklayarak **ilgili olaylar** gibi bir hata iletisi ayrıntılarını görmek için.

   ![Düzeltme - kaynak görev bağlam menüsü](../media/remediate-resources/resource-task-context-menu.png)

Dağıtılan kaynakları aracılığıyla bir **düzeltme görev** eklenir **dağıtılan kaynakların** uyumluluk İlkesi sayfasının bir sekmesinde.

### <a name="create-a-remediation-task-through-azure-cli"></a>Azure CLI aracılığıyla bir düzeltme görev oluşturun

Oluşturmak için bir **düzeltme görev** Azure CLI ile kullanmak `az policy remediation` komutları. Değiştirin `{subscriptionId}` yerine abonelik Kimliğinizi ve `{myAssignmentId}` ile **Deployıfnotexists** ilke ataması kimliği.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme komutları ve örnekler için bkz. [az İlkesi düzeltme](/cli/azure/policy/remediation) komutları.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Azure PowerShell aracılığıyla bir düzeltme görev oluşturun

Oluşturmak için bir **düzeltme görev** Azure PowerShell ile kullanma `Start-AzPolicyRemediation` komutları. Değiştirin `{subscriptionId}` yerine abonelik Kimliğinizi ve `{myAssignmentId}` ile **Deployıfnotexists** ilke ataması kimliği.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Diğer düzeltme cmdlet ve örnekler için bkz. [Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights) modülü.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md)
- Gözden geçirme [İlkesi tanım yapısı](../concepts/definition-structure.md)
- Gözden geçirme [ilke etkilerini anlama](../concepts/effects.md)
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](programmatically-create.md)
- Bilgi edinmek için nasıl [uyumluluk verilerini al](getting-compliance-data.md)
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/overview.md) bölümünde yönetim gruplarını gözden geçirebilirsiniz