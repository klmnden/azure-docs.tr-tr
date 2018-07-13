---
title: Değiştirme, silmek veya yönetmek yönetim gruplarınızı - Azure | Microsoft Docs
description: Bakımı ve yönetimi Grup hiyerarşiniz güncelleştirmesi öğrenin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/09/2018
ms.author: rithorn
ms.openlocfilehash: 730f79ce0a70da92dbb6332ad824b17e6c2327ff
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488387"
---
# <a name="manage-your-resources-with-management-groups"></a>Yönetim gruplarıyla kaynaklarınızı yönetin

Yönetim grupları Aboneliklerdeki erişim, ilke ve uyumluluk yardımcı kapsayıcıları yönetmek olan. Değiştirme, silme ve kullanılabilir hiyerarşileri için bu kapsayıcıları yönetmek [Azure İlkesi](../azure-policy/azure-policy-introduction.md) ve [Azure rol tabanlı erişim denetimleri (RBAC)](../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure yönetim gruplarıyla düzenleme ](management-groups-overview.md).

Yönetim grubu özellik genel önizlemede kullanılabilir. Yönetim gruplarını kullanmaya başlamak için oturum açın [Azure portalında](https://portal.azure.com) veya [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), veya [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) için Yönetim gruplarınızı yönetme.

Bir yönetim grubuna değişiklik yapmak için yönetim grubunda bir sahibi veya katkıda bulunan rolü olmalıdır. Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi edinmek için [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Bir yönetim grubunun adını değiştirin

Portal, PowerShell veya Azure CLI kullanarak yönetim grubu adını değiştirebilirsiniz.

### <a name="change-the-name-in-the-portal"></a>Portalda adını değiştirin

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**  
3. Yeniden adlandırmak istediğiniz yönetim grubunu seçin.
4. Seçin **grubu Yeniden Adlandır** sayfanın üstündeki seçeneği.

    ![Grubu Yeniden Adlandır](media/management-groups/detail_action_small.png)
5. Menü açıldığında, görüntülenmesini istediğiniz yeni bir ad girin.

    ![Grubu Yeniden Adlandır](media/management-groups/rename_context.png)
6. **Kaydet**’i seçin.

### <a name="change-the-name-in-powershell"></a>PowerShell'de adını değiştirin

Görünen ad kullanımı güncelleştirilecek **güncelleştirme AzureRmManagementGroup**. Örneğin, "Contoso BT" "Contoso grubu" için bir yönetim grupları adını değiştirmek için aşağıdaki komutu çalıştırın: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Azure CLI'de adını değiştirin

Azure CLI için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name Contoso --display-name "Contoso Group" 
```

---

## <a name="delete-a-management-group"></a>Bir yönetim grubunu sil

Bir yönetim grubunu silmek için aşağıdaki gereksinimler karşılanmalıdır:

1. Alt Yönetim grupları veya yönetim grubundaki abonelikleri yoktur.
    - Bir yönetim grubu dışında bir aboneliği taşımak için bkz [abonelik başka bir yönetim grubuna Taşı](#Move-subscriptions-in-the-hierarchy).
    - Bir yönetim grubunu başka bir yönetim grubuna taşımak için bkz [Yönetim grupları hiyerarşide Taşı](#Move-management-groups-in-the-hierarchy).
2. Yönetim grubundaki yönetim grubu sahibi veya katkıda bulunan rolü yazma izinlerine sahip. Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Portalda Sil

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**  
3. Silmek istediğiniz yönetim grubunu seçin.
4. **Sil**’i seçin.
    - Simge devre dışı bırakılırsa, fare Seçici simgenin üzerine geldiğinizde nedenini gösterir.
    ![Grubu Sil](media/management-groups/delete.png)
5. Yönetim grubu silmek istediğiniz onaylayan açılan bir pencere yoktur.

    ![Grubu Sil](media/management-groups/delete_confirm.png)
6. Seçin **Evet**


### <a name="delete-in-powershell"></a>PowerShell'de Sil

Kullanım **Remove-AzureRmManagementGroup** yönetim grubunu silmek için PowerShell içinde komutu. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Azure CLI ile silme

Azure CLI ile komut az hesabı yönetim grubu silme kullanın.

```azurecli-interactive
az account management-group delete --name Contoso
```
---

## <a name="view-management-groups"></a>Yönetim grupları görüntüleyin

Doğrudan ya da devralınmış bir RBAC rolü sahip herhangi bir yönetim grubuna görüntüleyebilirsiniz.  

### <a name="view-in-the-portal"></a>Portalda görüntüle

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Yönetim grubu hiyerarşisi burada tüm yönetim gruplarında ve Aboneliklerde erişiminiz keşfedebilirsiniz yükleri sayfa. Grup adı seçtiğinizde hiyerarşide bir düzey aşağı açılır. Dosya Gezgini gibi Gezinti aynı çalışmaktadır. 
    ![Ana](media/management-groups/main.png)
4. Yönetim grubunun ayrıntılarını görmek için seçin **(Ayrıntılar)** yönetim grubunun başlığının yanındaki bağlantı. Bu bağlantı kullanılabilir değilse, bu yönetim grubunu görüntüleme izniniz yok.  

### <a name="view-in-powershell"></a>PowerShell'de görüntüle

Tüm grupları almak için Get-AzureRmManagementGroup komutunu kullanabilirsiniz.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Tek bir yönetim grubunun bilgi için - GroupName parametresini kullanın

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Azure CLI görünümünde

Tüm grupları almak için liste komutunu kullanın.  

```azurecli-interactive
az account management-group list
```
Tek bir yönetim grubunun bilgi show komutunu kullanın.

```azurecli-interactive
az account management-group show --name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Abonelikler hiyerarşide Taşı

Bir yönetim grubu oluşturmak için bir abonelik gruplamak nedenidir. Yalnızca yönetim gruplarında ve Aboneliklerde, başka bir yönetim grubunun alt yapılabilir. Bir yönetim grubuna taşıyan bir abonelik, tüm kullanıcı erişimini ve ilkeleri üst yönetim grubundan devralır.

Aboneliği taşımak için birkaç izinlere ihtiyacınız vardır:

- Alt abonelikte "Sahip" rolü.
- Yeni üst yönetim grubunda "Sahip" veya "Katılımcı" rolü.
- Eski üst yönetim grubunda "Sahip" veya "Katılımcı" rolü.

Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md).

### <a name="move-subscriptions-in-the-portal"></a>Portalda abonelikler taşıma

**Mevcut bir abonelik için Yönetim Grubu Ekle**

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**
3. Üst planlamanız durumunda yönetim grubunu seçin.
4. Sayfanın üst kısmında seçin **Abonelik Ekle**.
5. Doğru kimliğe sahip bir abonelik seçin

    ![Alt Öğeler](media/management-groups/add_context_sub.png)
1. "Kaydet" seçin

**Abonelik bir yönetim grubundan kaldırın.**

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Diğer bir deyişle, geçerli üst planlamanız durumunda yönetim grubunu seçin.  
4. Abonelik için satırın sonundaki üç nokta taşımak istediğiniz listeden seçin.

    ![Taşı](media/management-groups/move_small.png)
5. Seçin **Taşı**
6. Açılan menüden **üst yönetim grubu**.

    ![Taşı](media/management-groups/move_small_context.png)
7. Seçin **Kaydet**

### <a name="move-subscriptions-in-powershell"></a>PowerShell'de abonelikler taşıma

PowerShell'de bir aboneliği taşımak için AzureRmManagementGroupSubscription Ekle komutunu kullanabilirsiniz.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

Arasındaki bağlantıyı kaldırmak ve abonelik ve yönetim grubu kaldırma AzureRmManagementGroupSubscription komutunu kullanın.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Abonelikler Azure CLI'da taşıma

CLI'daki bir aboneliği taşımak için Ekle komutunu kullanabilirsiniz.

```azurecli-interactive
az account management-group subscription add --name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Abonelik yönetim grubundan kaldırmak için Aboneliği Kaldır komutu kullanın.  

```azurecli-interactive
az account management-group subscription remove --name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Yönetim grupları hiyerarşide Taşı  

Bir üst yönetim grubu Yönetim grupları, abonelikler, kaynak grupları ve kaynakları taşıma üst ile içeren tüm alt kaynaklar taşıdığınızda.   

### <a name="move-management-groups-in-the-portal"></a>Portalda Yönetim grupları Taşı

1. Oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**
3. Üst planlamanız durumunda yönetim grubunu seçin.
5. Sayfanın üst kısmında seçin **Yönetim Grubu Ekle**.
6. Açılır menüde, mevcut bir yönetim grubunu kullanın veya yeni bir istiyorsanız seçin.
    - Yeni'yi seçerek yeni bir yönetim grubu oluşturur.
    - Mevcut bir seçerek, bu yönetim grubuna taşıyabilirsiniz tüm yönetim grubunun bir açılır menü ile sunacaktır.  

    ![Taşıma](media/management-groups/add_context_MG.png)
7. Seçin **Kaydet**

### <a name="move-management-groups-in-powershell"></a>PowerShell'de Yönetim grupları Taşı

Güncelleştirme AzureRmManagementGroup komutu PowerShell'de farklı bir çalışma grubundaki bir yönetim grubuna taşımak için kullanın.  

```powershell
Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  

### <a name="move-management-groups-in-azure-cli"></a>Azure CLI içinde Yönetim grupları Taşı

Azure CLI ile bir yönetim grubuna taşımak için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name Contoso --parent "Contoso Tenant"
```

---

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi için bkz:

- [Kaynaklarınızı Azure Yönetim grupları ile düzenleme](management-groups-overview.md)
- [Azure kaynaklarını düzenlemek için Yönetim grupları oluşturma](management-groups-create.md)
- [Azure Powershell modülünü yükleme](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Belirtimi gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI uzantısını yükleme](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
