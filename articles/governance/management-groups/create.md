---
title: Azure kaynakları - Azure idare düzenlemek için Yönetim grupları oluşturma
description: Portal, Azure PowerShell ve Azure CLI kullanarak birden çok kaynağı yönetmek için Azure yönetim gruplarının nasıl oluşturulacağını öğrenin.
author: rthorn17
manager: rithorn
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2019
ms.author: rithorn
ms.topic: conceptual
ms.openlocfilehash: 928cb790bd97270870618534a73316bba5eeb070
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057447"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Kaynak kuruluşta ve Yönetim için Yönetim grupları oluşturma

Yönetim grupları Aboneliklerdeki erişim, ilke ve uyumluluk yardımcı kapsayıcıları yönetmek olan. İle kullanılabilen bir etkili ve verimli bir hiyerarşi oluşturmak için bu kapsayıcıları [Azure İlkesi](../policy/overview.md) ve [Azure rol tabanlı erişim denetimlerini](../../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi için bkz. [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](overview.md).

Dizinde oluşturulan ilk yönetim grubu tamamlanması 15 dakika sürebilir. Dizininiz için Azure Yönetim grupları hizmeti ayarlamak için ilk kez çalışan işlemler vardır. İşlem tamamlandığında, bir bildirim alırsınız.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="create-a-management-group"></a>Bir yönetim grubu oluşturma

Portal, PowerShell veya Azure CLI kullanarak, yönetim grubu oluşturabilirsiniz. Şu anda, Resource Manager şablonları, Yönetim grupları oluşturmak için kullanamazsınız.

### <a name="create-in-portal"></a>Portalda oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** > **Yönetim grupları**.

1. Ana sayfasında göze seçin **yeni yönetim grubu**.

   ![Yönetim gruplarıyla çalışma sayfası](./media/main.png)

1. Yönetim grubu kimliği alanını doldurun.

   - **Yönetim grubu Tanıtıcısı** bu yönetim grubunda komutları göndermek için kullanılan dizini benzersiz tanımlayıcısı. Bu tanımlayıcı, Azure sistem genelinde bu grubu tanımlamak için kullanılan oluşturulduktan sonra düzenlenebilir değildir. [Kök yönetim grubu](index.md#root-management-group-for-each-directory) Azure Active Directory kimliği olan bir kimliği ile otomatik olarak oluşturulur Diğer tüm Yönetim grupları için benzersiz bir kimliği atayın
   - Görünen ad alanı, Azure portalında görüntülenen addır. Yönetim oluştururken ayrı görünen ad isteğe bağlı bir alan olan Grup ve herhangi bir zamanda değiştirilebilir.  

   ![Yeni bir yönetim grubu oluşturmak için seçenekler bölmesi](./media/create_context_menu.png)  

1. **Kaydet**’i seçin.

### <a name="create-in-powershell"></a>PowerShell'de oluşturma

PowerShell içinde yeni AzManagementGroup cmdlet'i kullanın:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** oluşturulan bir benzersiz tanımlayıcısı. Bu kimliği, başka komutlarla bu Grup başvurmak için kullanılır ve daha sonra değiştirilemez.

Azure Portal'ın farklı bir ad göstermek için yönetim grubu istediğinizi varsa, eklersiniz **DisplayName** dizesiyle parametre. Örneğin, bir yönetim grubu, GroupName Contoso ve görünen adı "Contoso grup" ile oluşturmak istediyseniz, aşağıdaki cmdlet'i kullanın:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group' -ParentId '/providers/Microsoft.Management/managementGroups/ContosoTenant'
```

Kullanım **parentId** parametresi bu yönetim grubu için farklı bir yönetim altına oluşturulabilir.

### <a name="create-in-azure-cli"></a>Azure CLI ile oluşturma

Azure CLI, az kullandığınız hesabı yönetim grubu oluşturma komutu.

```azurecli-interactive
az account management-group create --name 'Contoso'
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için Yönetim grupları oluşturma](create.md)
- [Değiştirme, silme veya yönetim gruplarınızı yönetme](manage.md)
- [Azure PowerShell kaynakları modülündeki Yönetim gruplarını gözden geçirin](/powershell/module/az.resources#resources)
- [REST API Yönetim gruplarını gözden geçirin](/rest/api/resources/managementgroups)
- [Azure CLI'de Yönetim gruplarını gözden geçirin](/cli/azure/account/management-group)