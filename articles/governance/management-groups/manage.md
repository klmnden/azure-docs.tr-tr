---
title: Değiştirme, silme veya Azure - Azure idare yönetim gruplarınızı yönetme
description: Görüntüleme, tutmak, güncelleştirme ve Yönetim Grup hiyerarşiniz silme hakkında bilgi edinin.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: dbfb6ecb9f29a82a8871922982a64dbefc338969
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342611"
---
# <a name="manage-your-resources-with-management-groups"></a>Yönetim gruplarıyla kaynaklarınızı yönetin

Yönetim grupları Aboneliklerdeki erişim, ilke ve uyumluluk yardımcı kapsayıcıları yönetmek olan. Değiştirme, silme ve kullanılabilir hiyerarşileri için bu kapsayıcıları yönetmek [Azure İlkesi](../policy/overview.md) ve [Azure rol tabanlı erişim denetimleri (RBAC)](../../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi edinmek için [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](overview.md).

Bir yönetim grubuna değişiklik yapmak için yönetim grubunda bir sahibi veya katkıda bulunan rolü olmalıdır. Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi edinmek için [erişim ve izinleri ile RBAC yönetme](../../role-based-access-control/overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

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

Görünen ad kullanımı güncelleştirilecek **güncelleştirme AzureRmManagementGroup**. Örneğin, "Contoso BT" "Contoso grubu" için bir yönetim grupları adını değiştirmek için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Azure CLI'de adını değiştirin

Azure CLI için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Bir yönetim grubunu sil

Bir yönetim grubunu silmek için aşağıdaki gereksinimler karşılanmalıdır:

1. Alt Yönetim grupları veya yönetim grubundaki abonelikleri yoktur.

   - Bir yönetim grubu dışında bir aboneliği taşımak için bkz [abonelik başka bir yönetim grubuna Taşı](#Move-subscriptions-in-the-hierarchy).

   - Bir yönetim grubunu başka bir yönetim grubuna taşımak için bkz [Yönetim grupları hiyerarşide Taşı](#Move-management-groups-in-the-hierarchy).

1. Yönetim grubundaki yönetim grubu sahibi veya katkıda bulunan rolü yazma izinlerine sahip. Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../../role-based-access-control/overview.md).  

### <a name="delete-in-the-portal"></a>Portalda Sil

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Silmek istediğiniz yönetim grubunu seçin.

1. Seçin **Sil**

   - Simge devre dışı bırakılırsa, fare Seçici simgenin üzerine geldiğinizde nedenini gösterir.

   ![Grup seçeneği Sil](./media/delete.png)

1. Yönetim grubu silmek istediğiniz onaylayan açılan bir pencere yoktur.

   ![Grup onay penceresi Sil](./media/delete_confirm.png)

1. Seçin **Evet**.

### <a name="delete-in-powershell"></a>PowerShell'de Sil

Kullanım **Remove-AzureRmManagementGroup** yönetim grubunu silmek için PowerShell içinde komutu.

```azurepowershell-interactive
Remove-AzureRmManagementGroup -GroupName 'Contoso'
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

1. Yönetim grubu hiyerarşisi burada tüm yönetim gruplarında ve Aboneliklerde erişiminiz keşfedebilirsiniz yükleri sayfa. Grup adı seçtiğinizde hiyerarşide bir düzey aşağı açılır. Dosya Gezgini gibi Gezinti aynı çalışmaktadır.

   ![Ana](./media/main.png)

1. Yönetim grubunun ayrıntılarını görmek için seçin **(Ayrıntılar)** yönetim grubunun başlığının yanındaki bağlantı. Bu bağlantı kullanılabilir değilse, bu yönetim grubunu görüntüleme izniniz yok.  

### <a name="view-in-powershell"></a>PowerShell'de görüntüle

Tüm grupları almak için Get-AzureRmManagementGroup komutunu kullanabilirsiniz.  

```azurepowershell-interactive
Get-AzureRmManagementGroup
```

Tek bir yönetim grubunun bilgi için - GroupName parametresini kullanın

```azurepowershell-interactive
Get-AzureRmManagementGroup -GroupName 'Contoso'
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

## <a name="move-subscriptions-in-the-hierarchy"></a>Abonelikler hiyerarşide Taşı

Bir yönetim grubu oluşturmak için bir abonelik gruplamak nedenidir. Yalnızca yönetim gruplarında ve Aboneliklerde, başka bir yönetim grubunun alt yapılabilir. Bir yönetim grubuna taşıyan bir abonelik, tüm kullanıcı erişimini ve ilkeleri üst yönetim grubundan devralır.

Aboneliği taşımak için birkaç izinlere ihtiyacınız vardır:

- Alt abonelikte "Sahip" rolü.
- Yeni üst yönetim grubunda "Sahip" veya "Katılımcı" rolü.
- Eski üst yönetim grubunda "Sahip" veya "Katılımcı" rolü.

Hangi izinlerin görmek için sahip, yönetim grubu seçip **IAM**. RBAC rolleri hakkında daha fazla bilgi için bkz. [erişim ve izinleri ile RBAC yönetme](../../role-based-access-control/overview.md).

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

PowerShell'de bir aboneliği taşımak için yeni AzureRmManagementGroupSubscription komutunu kullanın.  

```azurepowershell-interactive
New-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Arasındaki bağlantıyı kaldırmak ve abonelik ve yönetim grubu kaldırma AzureRmManagementGroupSubscription komutunu kullanın.

```azurepowershell-interactive
Remove-AzureRmManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
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

Bir üst yönetim grubu Yönetim grupları, abonelikler, kaynak grupları ve kaynakları taşıma üst ile içeren tüm alt kaynaklar taşıdığınızda.

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

Güncelleştirme AzureRmManagementGroup komutu PowerShell'de farklı bir çalışma grubundaki bir yönetim grubuna taşımak için kullanın.

```azurepowershell-interactive
Update-AzureRmManagementGroup -GroupName 'Contoso' -ParentName 'ContosoIT'
```  

### <a name="move-management-groups-in-azure-cli"></a>Azure CLI içinde Yönetim grupları Taşı

Azure CLI ile bir yönetim grubuna taşımak için güncelleştirme komutunu kullanın.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent 'Contoso Tenant'
```

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak yönetim gruplarını denetleme

Bu API yoluyla yönetim gruplarını izlemek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) kullanın. Şu anda PowerShell, CLI veya Azure portalını kullanarak yönetim grupları etkinliğini izlemek mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../../role-based-access-control/elevate-access-global-admin.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın.
1. Denetleyen kullanıcı olarak, yönetim grubu etkinliklerini görmek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) çağırın. Tüm yönetim grubu etkinliği için **Microsoft.Management** Kaynak Sağlayıcısına göre filtrelemek istersiniz.  Örnek:

```xml
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](https://aka.ms/mgPSdocs)
- [REST API'de yönetim gruplarını gözden geçirme](https://aka.ms/mgAPIdocs)
- [Azure CLI'de yönetim gruplarını gözden geçirme](https://aka.ms/mgclidoc)
