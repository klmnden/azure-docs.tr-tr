---
title: Değiştirme, silme veya Azure - Azure idare yönetim gruplarınızı yönetme
description: Görüntüleme, tutmak, güncelleştirme ve Yönetim Grup hiyerarşiniz silme hakkında bilgi edinin.
author: rthorn17
ms.service: azure-resource-manager
ms.date: 02/20/2019
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 801a37496b36be1f98408c46807f5b10db2b0282
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622068"
---
# <a name="manage-your-resources-with-management-groups"></a>Yönetim gruplarıyla kaynaklarınızı yönetin

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve idare koşullarınızı bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır.

Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.  Yönetim grupları hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="change-the-name-of-a-management-group"></a>Bir yönetim grubunun adını değiştirin

Portal, PowerShell veya Azure CLI kullanarak yönetim grubu adını değiştirebilirsiniz.

### <a name="change-the-name-in-the-portal"></a>Portalda adını değiştirin

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Yeniden adlandırmak istediğiniz yönetim grubunu seçin.

1. Seçin **grubu Yeniden Adlandır** sayfanın üstündeki seçeneği.

   ![Grubu Yeniden Adlandır seçeneğini](./media/detail_action_small.png)

1. Menü açıldığında, görüntülenmesini istediğiniz yeni bir ad girin.

   ![Grubu yeniden adlandır bölmesi](./media/rename_context.png)

1. **Kaydet**’i seçin.

### <a name="change-the-name-in-powershell"></a>PowerShell'de adını değiştirin

Görünen ad kullanımı güncelleştirilecek **güncelleştirme AzManagementGroup**. Örneğin, bir yönetim değiştirmek için "Contoso BT" adından "Contoso grubuna" gruplarını görüntülemek, aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Update-AzManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Azure CLI'de adını değiştirin

Azure CLI için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Bir yönetim grubunu sil

Bir yönetim grubunu silmek için aşağıdaki gereksinimler karşılanmalıdır:

1. Alt Yönetim grupları veya yönetim grubundaki abonelikleri yoktur.

   - Bir yönetim grubu dışında bir aboneliği taşımak için bkz [abonelik başka bir yönetim grubuna Taşı](#move-subscriptions-in-the-hierarchy).

   - Bir yönetim grubunu başka bir yönetim grubuna taşımak için bkz [Yönetim grupları hiyerarşide Taşı](#move-management-groups-in-the-hierarchy).

1. Yönetim grubunda ("Sahip", "Katılımcı" veya "Yönetim grubu katkıda bulunan") yazma izinlerine sahip. Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Portalda Sil

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Silmek istediğiniz yönetim grubunu seçin.

1. Seçin **Sil**

    > [!TIP]
    > Simge devre dışı bırakılırsa, fare Seçici simgenin üzerine geldiğinizde nedenini gösterir.

   ![Grup seçeneği Sil](./media/delete.png)

1. Yönetim grubu silmek istediğiniz onaylayan açılan bir pencere yoktur.

   ![Grup onay penceresi Sil](./media/delete_confirm.png)

1. Seçin **Evet**.

### <a name="delete-in-powershell"></a>PowerShell'de Sil

Kullanım **Remove-AzManagementGroup** yönetim grubunu silmek için PowerShell içinde komutu.

```azurepowershell-interactive
Remove-AzManagementGroup -GroupName 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Azure CLI ile silme

Azure CLI ile komut az hesabı yönetim grubu silme kullanın.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Yönetim grupları görüntüleyin

Doğrudan ya da devralınmış bir RBAC rolü sahip herhangi bir yönetim grubuna görüntüleyebilirsiniz.  

### <a name="view-in-the-portal"></a>Portalda görüntüle

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Yönetim grubu hiyerarşisi sayfasını yükleyecektir. Bu, burada tüm Yönetim gruplarını keşfedebilirsiniz ve abonelikleri erişiminiz sayfasıdır. Grup adı seçtiğinizde hiyerarşide bir düzey aşağı açılır. Dosya Gezgini gibi Gezinti aynı çalışmaktadır.

1. Yönetim grubunun ayrıntılarını görmek için seçin **(Ayrıntılar)** yönetim grubunun başlığının yanındaki bağlantı. Bu bağlantı kullanılabilir değilse, bu yönetim grubunu görüntüleme izniniz yok.

   ![Ana](./media/main.png)

### <a name="view-in-powershell"></a>PowerShell'de görüntüle

Tüm grupları almak için Get-AzManagementGroup komutunu kullanabilirsiniz.  Bkz: [Az.Resources](/powershell/module/az.resources/Get-AzManagementGroup) yönetim tam listesi için modüllerin grup alma Powershell komutları.  

```azurepowershell-interactive
Get-AzManagementGroup
```

Tek bir yönetim grubunun bilgi için - GroupName parametresini kullanın

```azurepowershell-interactive
Get-AzManagementGroup -GroupName 'Contoso'
```

Belirli bir yönetim grubu ve hiyerarşisi altındaki tüm düzeyleri döndürmek için **-genişletin** ve **-Recurse** parametreleri.  

```azurepowershell-interactive
PS C:\> $response = Get-AzManagementGroup -GroupName TestGroupParent -Expand -Recurse
PS C:\> $response

Id                : /providers/Microsoft.Management/managementGroups/TestGroupParent
Type              : /providers/Microsoft.Management/managementGroups
Name              : TestGroupParent
TenantId          : 00000000-0000-0000-0000-000000000000
DisplayName       : TestGroupParent
UpdatedTime       : 2/1/2018 11:15:46 AM
UpdatedBy         : 00000000-0000-0000-0000-000000000000
ParentId          : /providers/Microsoft.Management/managementGroups/00000000-0000-0000-0000-000000000000
ParentName        : 00000000-0000-0000-0000-000000000000
ParentDisplayName : 00000000-0000-0000-0000-000000000000
Children          : {TestGroup1DisplayName, TestGroup2DisplayName}

PS C:\> $response.Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestGroup1
Name        : TestGroup1
DisplayName : TestGroup1DisplayName
Children    : {TestRecurseChild}

PS C:\> $response.Children[0].Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestRecurseChild
Name        : TestRecurseChild
DisplayName : TestRecurseChild
Children    :
```

### <a name="view-in-azure-cli"></a>Azure CLI görünümünde

Tüm grupları almak için liste komutunu kullanın.  

```azurecli-interactive
az account management-group list
```

Tek bir yönetim grubunun bilgi show komutunu kullanın.

```azurecli-interactive
az account management-group show --name 'Contoso'
```

Belirli bir yönetim grubu ve hiyerarşisi altındaki tüm düzeyleri döndürmek için **-genişletin** ve **-Recurse** parametreleri.

```azurecli-interactive
az account management-group show --name 'Contoso' -e -r
```

## <a name="move-subscriptions-in-the-hierarchy"></a>Abonelikler hiyerarşide Taşı

Bir yönetim grubu oluşturmak için bir abonelik gruplamak nedenidir. Yalnızca yönetim gruplarında ve Aboneliklerde, başka bir yönetim grubunun alt yapılabilir. Bir yönetim grubuna taşıyan bir abonelik, tüm kullanıcı erişimini ve ilkeleri üst yönetim grubundan devralır.

Aboneliği taşımak için aşağıdaki RBAC izinlerinin tümünün doğru olması gerekir:

- Alt abonelikte "Sahip" rolü.
- Hedef üst yönetim group.* "Sahip", "Katılımcı" veya "Yönetim grubu katılımcı" rolü
- Mevcut üst yönetim group.* "Sahip", "Katılımcı" veya "Yönetim grubu katılımcı" rolü

*: Hedefi ya da mevcut üst yönetim grubunu kök yönetim grubu olmadıkça. Kök yönetim grubundaki tüm Yönetim grupları ve abonelikler için nokta giriş varsayılan olduğundan, kullanıcıların bir öğeyi taşımak için izinlerine ihtiyacınız yoktur.

Hangi izinlerin yönetim grubu ve ardından Azure portalında, select olduğunu görmek için **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Portalda abonelikler taşıma

#### <a name="add-an-existing-subscription-to-a-management-group"></a>Mevcut bir abonelik için Yönetim Grubu Ekle

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Üst planlamanız durumunda yönetim grubunu seçin.

1. Sayfanın üst kısmında seçin **Abonelik Ekle**.

1. Doğru kimliğe sahip bir abonelik seçin

   ![Alt Öğeler](./media/add_context_sub.png)

1. "Kaydet"'i seçin.

#### <a name="remove-a-subscription-from-a-management-group"></a>Abonelik bir yönetim grubundan kaldırın.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Diğer bir deyişle, geçerli üst planlamanız durumunda yönetim grubunu seçin.  

1. Abonelik için satırın sonundaki üç nokta taşımak istediğiniz listeden seçin.

   ![Taşıma seçeneği](./media/move_small.png)

1. Seçin **taşıma**.

1. Açılan menüden **üst yönetim grubu**.

   ![Taşıma bölmesi](./media/move_small_context.png)

1. **Kaydet**’i seçin.

### <a name="move-subscriptions-in-powershell"></a>PowerShell'de abonelikler taşıma

PowerShell'de bir aboneliği taşımak için yeni AzManagementGroupSubscription komutunu kullanın.  

```azurepowershell-interactive
New-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Arasındaki bağlantıyı kaldırmak ve abonelik ve yönetim grubu kaldırma AzManagementGroupSubscription komutunu kullanın.

```azurepowershell-interactive
Remove-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Abonelikler Azure CLI'da taşıma

CLI'daki bir aboneliği taşımak için Ekle komutunu kullanabilirsiniz.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Abonelik yönetim grubundan kaldırmak için Aboneliği Kaldır komutu kullanın.  

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

## <a name="move-management-groups-in-the-hierarchy"></a>Yönetim grupları hiyerarşide Taşı  

Bir üst yönetim grubuna taşıdığınızda, bu grubun altında hiyerarşi ile taşır.

### <a name="move-management-groups-in-the-portal"></a>Portalda Yönetim grupları Taşı

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Üst planlamanız durumunda yönetim grubunu seçin.

1. Sayfanın üst kısmında seçin **Yönetim Grubu Ekle**.

1. Açılır menüde, mevcut bir yönetim grubunu kullanın veya yeni bir istiyorsanız seçin.

   - Yeni'yi seçerek yeni bir yönetim grubu oluşturur.
   - Mevcut bir seçerek, bu yönetim grubuna taşıyabilirsiniz tüm yönetim grubunun bir açılır menü ile sunacaktır.  

   ![Taşıma](./media/add_context_MG.png)

1. **Kaydet**’i seçin.

### <a name="move-management-groups-in-powershell"></a>PowerShell'de Yönetim grupları Taşı

Güncelleştirme AzManagementGroup komutu PowerShell'de farklı bir çalışma grubundaki bir yönetim grubuna taşımak için kullanın.

```azurepowershell-interactive
Update-AzManagementGroup -GroupName 'Contoso' -ParentId '/providers/Microsoft.Management/managementGroups/ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Azure CLI içinde Yönetim grupları Taşı

Azure CLI ile bir yönetim grubuna taşımak için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent-id '/providers/Microsoft.Management/managementGroups/ContosoIT'
```

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak yönetim gruplarını denetleme

Yönetim grupları [Azure Etkinlik Günlüğü](../../azure-monitor/platform/activity-logs-overview.md)'nde desteklenir. Bir yönetim grubuna diğer Azure kaynakları ile aynı merkezi konumda gerçekleşen tüm olayları sorgulayabilirsiniz.  Örneğin, belirli bir yönetim grubunda yapılan tüm Rol Atamalarını veya İlke Ataması değişikliklerini görebilirsiniz.

![Yönetim Gruplarıyla Etkinlik Günlükleri](media/al-mg.png)

Azure portalının dışında Yönetim Gruplarını sorgulamak istediğinizde, yönetim gruplarının hedef kapsamı **"/providers/Microsoft.Management/managementGroups/{yourMgID}"** gibi görünür.

## <a name="referencing-management-groups-from-other-resource-providers"></a>Diğer kaynak sağlayıcılarından başvuruda bulunan Yönetim grupları

Yönetim Grupları diğer kaynak sağlayıcısının eylemlerden başvururken aşağıdaki yol kapsamı olarak kullanın. Bu yol, PowerShell, Azure CLI ve REST API'leri kullanırken kullanılır.  

>"/ providers/Microsoft.Management/managementGroups/{yourMgID}"

PowerShell'de bir yönetim grubuna yeni bir rol ataması atarken bu yolu kullanarak, bir örnek verilmiştir

```powershell-interactive
New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"
```

Aynı kapsam yolu bir yönetim grubu, bir ilke tanımı alınırken kullanılır.

```http
GET https://management.azure.com/providers/Microsoft.Management/managementgroups/MyManagementGroup/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming?api-version=2018-05-01
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](/powershell/module/az.resources#resources)
- [REST API'de yönetim gruplarını gözden geçirme](/rest/api/resources/managementgroups)
- [Azure CLI'de yönetim gruplarını gözden geçirme](/cli/azure/account/management-group)