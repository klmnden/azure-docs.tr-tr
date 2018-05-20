---
title: Değiştirmek, silmek veya yönetim gruplarınızı - Azure yönetmek nasıl | Microsoft Docs
description: Korumak ve Yönetim Grup hiyerarşiniz güncelleştirmek hakkında bilgi edinin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/15/2018
ms.author: rithorn
ms.openlocfilehash: 822a2df113b848f07e616f155881f345028cee1d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-your-resources-with-management-groups"></a>Yönetim grupları ile kaynaklarınızı yönetmek 
Yönetim erişimi, ilke ve uyumluluk arasında birden çok abonelik yönetmenize yardımcı kapsayıcılardır gruplarıdır. Değiştirme, silme ve bu kapsayıcıları kullanılabilir hiyerarşileri yönetme [Azure ilke](../azure-policy/azure-policy-introduction.md) ve [Azure rol tabanlı erişim denetimlerini (RBAC)](../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi için bkz: [kaynaklarınızı Azure Yönetim grupları ile düzenleme ](management-groups-overview.md).

Yönetim grubu özelliğini genel önizleme olarak kullanılabilir. Management'ı kullanmaya başlamak için gruplar, oturum açma [Azure portal](https://portal.azure.com) veya kullanabilirsiniz [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), veya [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) için Yönetim gruplarını yönetin.

Bir yönetim grubuna değişiklik yapmak için yönetim grubu üzerinde bir sahibi veya katkıda bulunan rolü olmalıdır. Hangi izinlerin görmek için sahip, yönetim grubu seçin ve ardından **IAM**. RBAC rolleri hakkında daha fazla bilgi edinmek için [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="change-the-name-of-a-management-group"></a>Bir yönetim grubu adını değiştirin 
Portal, PowerShell veya Azure CLI kullanarak yönetim grubu adını değiştirebilirsiniz.

### <a name="change-the-name-in-the-portal"></a>Portalda adını değiştirin

1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**  
3. Yeniden adlandırmak istediğiniz yönetim grubunu seçin. 
4. Seçin **grubu Yeniden Adlandır** sayfanın üst kısmındaki seçeneği. 

    ![Grubu Yeniden Adlandır](media/management-groups/detail_action_small.png)
5. Menü açıldığında, görüntülenmesini istediğiniz yeni adını girin.

    ![Grubu Yeniden Adlandır](media/management-groups/rename_context.png) 
4. **Kaydet**’i seçin. 

### <a name="change-the-name-in-powershell"></a>PowerShell'de adını değiştirin

Görünen ad kullanımı güncelleştirmek için **güncelleştirme AzureRmManagementGroup**. Örneğin, "Contoso BT" "Contoso grubu" için bir yönetim grupları adını değiştirmek için aşağıdaki komutu çalıştırın: 

```azurepowershell-inetractive
C:\> Update-AzureRmManagementGroup -GroupName ContosoIt -DisplayName "Contoso Group"  
``` 

### <a name="change-the-name-in-azure-cli"></a>Azure CLI adını değiştirin

Azure CLI için güncelleştirme komutunu kullanın. 

```azure-cli
C:\> az account management-group update --group-name Contoso --display-name "Contoso Group" 
```

---
 
## <a name="delete-a-management-group"></a>Bir yönetim grubu Sil
Bir yönetim grubunu silmek için aşağıdaki gereksinimlerin karşılanması gerekir:
1. Alt Yönetim grupları veya yönetim grubu altında Abonelikleriniz yok. 
    - Bir yönetim grubu dışında bir aboneliği taşımak için bkz: [abonelik başka bir managemnt grubuna taşımak](#Move-subscriptions-in-the-hierarchy). 
    - Yönetim grubu başka bir yönetim grubuna taşımak için bkz: [Yönetim grupları hiyerarşide Taşı](#Move-management-groups-in-the-hierarchy). 
2. Yönetim grubunda yönetim grubu sahibi veya katkıda bulunan rolü yazma izinlerine sahip. Hangi izinlerin görmek için sahip, yönetim grubu seçin ve ardından **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz: [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Portalda Sil

1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları**  
3. Silmek istediğiniz yönetim grubunu seçin. 
    
    ![Grubu Sil](media/management-groups/delete.png)
4. **Sil**’i seçin. 
    - Simge devre dışıysa, fare Seçici simgenin üzerine gelerek veya onları nedenini gösterir. 
5. Yönetim grubu silmek istediğiniz onaylayan açılır penceresi yok. 

    ![Grubu Sil](media/management-groups/delete_confirm.png) 
6. Seçin **Evet** 


### <a name="delete-in-powershell"></a>PowerShell'de Sil

Kullanım **Kaldır AzureRmManagementGroup** Yönetim gruplarını silmek için PowerShell içinde komutu. 

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName Contoso
```

### <a name="delete-in-azure-cli"></a>Azure CLI ile silme
Azure CLI ile komut az hesabını yönetim grubu silmeyi kullanın. 

```azure-cli
C:\> az account management-group delete --group-name Contoso
```
---

## <a name="view-management-groups"></a>Yönetim grupları görüntüle
Bir doğrudan veya devralınan RBAC rolü sahip herhangi bir yönetim grubu görüntüleyebilirsiniz.  

### <a name="view-in-the-portal"></a>Portalında görüntüleyin
1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Yönetim grubu hiyerarşisi burada tüm Yönetim gruplarını ve abonelikleri erişiminiz keşfedebilirsiniz yükleri sayfa. Grup adı seçerek bir düzey hiyerarşide aşağı alır. Dosya Gezgini yaptığı gibi Gezinti aynı şekilde çalışır. 
    ![Ana](media/management-groups/main.png)
4. Yönetim grubu ayrıntılarını görmek için seçin **(ayrıntı)** yönetim grubunun başlığının yanındaki bağlantı. Bu bağlantıyı kullanılabilir değilse, bu yönetim grubu görüntüleme izniniz yok.  

### <a name="view-in-powershell"></a>PowerShell görünümünde
Tüm grupları almak için Get-AzureRmManagementGroup komutunu kullanın.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```
Bir tek bir yönetim grubunun bilgi için - GroupName parametresini kullanın

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName Contoso
```

### <a name="view-in-azure-cli"></a>Azure CLI görünümünde
Tüm grupları almak için liste komutunu kullanın.  

```azure-cli
az account management-group list
```
Bir tek bir yönetim grubunun bilgilerini Göster komutunu kullanın

```azurepowershell-interactive
az account management-group show --group-name Contoso
```
---

## <a name="move-subscriptions-in-the-hierarchy"></a>Abonelikleri hiyerarşisinde taşıyın
Bir yönetim grubu oluşturmak için bir abonelik birlikte paketlemektir nedenidir. Yalnızca yönetim grupları ve abonelikleri başka bir yönetim grubunun alt yapılabilir. Bir yönetim grubuna taşır bir abonelik tüm kullanıcı erişimi ve ilkeleri üst yönetim grubundan devralır. 

Aboneliği taşımak için bilmeniz gereken birkaç izinleri vardır: 
- Alt abonelik "Sahip" rolü.
- Yeni üst yönetim grubunda "Sahibi" veya "Katkıda" rolü. 
- Eski üst yönetim grubunda "Sahibi" veya "Katkıda" rolü.
Hangi izinlerin görmek için sahip, yönetim grubu seçin ve ardından **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz: [erişim ve izinleri ile RBAC yönetme](../role-based-access-control/overview.md). 

### <a name="move-subscriptions-in-the-portal"></a>Portalda abonelik taşıma

**Mevcut bir aboneliğe bir yönetim grubuna Ekle**
1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Üst olacak planlama yönetim grubunu seçin.      
5. Sayfanın en üstünde seçin **ekleme varolan**. 
6. Açılan menüde seçin **kaynak türü** olduğu taşımak için çalıştığınız öğenin **abonelik**.
7. Doğru kimliğiyle listesinde aboneliği seçin 

    ![Alt Öğeler](media/management-groups/add_context_2.png)
8. "Kaydet" seçin

**Bir abonelik bir yönetim grubundan Kaldır**
1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Geçerli üst planlama diğer bir deyişle yönetim grubunu seçin.  
4. Abonelik için satır sonunda elips taşımak istediğiniz listeden seçin.

    ![Taşı](media/management-groups/move_small.png)
5. Seçin **Taşı**
6. Açılır menüsünden seçin **üst yönetim grubu**.

    ![Taşı](media/management-groups/move_small_context.png)
7. Seçin **Kaydet**

### <a name="move-subscriptions-in-powershell"></a>PowerShell'de abonelikleri taşıma
PowerShell'de bir aboneliği taşımak için Add-AzureRmManagementGroupSubscription komutunu kullanın.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

Arasındaki bağlantıyı kaldırmak için ve abonelik ve yönetim grubu kaldırma AzureRmManagementGroupSubscription komutunu kullanın.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName Contoso -SubscriptionId 12345678-1234-1234-1234-123456789012
```

### <a name="move-subscriptions-in-azure-cli"></a>Azure CLI Aboneliklerde taşıma
CLI içinde bir aboneliği taşımak için Ekle komutunu kullanın. 

```azure-cli
C:\> az account management-group add --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

Abonelik yönetim grubundan kaldırmak için Aboneliği Kaldır komutunu kullanın.  

```azure-cli
C:\> az account management-group remove --group-name Contoso --subscription 12345678-1234-1234-1234-123456789012
```

---

## <a name="move-management-groups-in-the-hierarchy"></a>Yönetim grupları hiyerarşisinde taşıyın  
Bir üst yönetim grubu, Yönetim grupları, abonelikler, kaynak grupları ve kaynakları taşıma üst öğeye sahip dahil tüm alt kaynakları taşıdığınızda.   

### <a name="move-management-groups-in-the-portal"></a>Portalda Yönetim gruplarını taşıma

1. İçine oturum [Azure portalı](https://portal.azure.com)
2. Seçin **tüm hizmetleri** > **Yönetim grupları** 
3. Üst olacak planlama yönetim grubunu seçin.      
5. Sayfanın en üstünde seçin **ekleme varolan**.
6. Açılan menüde seçin **kaynak türü** olduğu taşımak için çalıştığınız öğenin **yönetim grubu**.
7. Yönetim grubu doğru kimliği ve adını seçin.

    ![Taşıma](media/management-groups/add_context.png)
8. Seçin **Kaydet**

### <a name="move-management-groups-in-powershell"></a>Yönetim gruplarını PowerShell'de taşıma
Farklı bir grup altında bir yönetim grubuna taşımak için PowerShell'de güncelleştirme AzureRmManagementGroup komutunu kullanın.  

```powershell
C:\> Update-AzureRmManagementGroup -GroupName Contoso  -ParentName ContosoIT
```  
### <a name="move-management-groups-in-azure-cli"></a>Azure CLI'daki taşıma Yönetim grupları
Azure CLI sahip bir yönetim grubu taşımak için güncelleştirme komutunu kullanın. 

```azure-cli
C:/> az account management-group udpate --group-name Contoso --parent-id "Contoso Tenant" 
``` 

---

## <a name="next-steps"></a>Sonraki adımlar 
Yönetim grupları hakkında daha fazla bilgi için bkz: 
- [Azure Yönetim grupları ile kaynaklarınızı düzenleme ](management-groups-overview.md)
- [Azure kaynaklarını düzenlemek için yönetim grubu oluşturma](management-groups-create.md)
- [Azure Powershell modülünü yükleyin](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Spec gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Azure CLI uzantısını yükleyin](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
