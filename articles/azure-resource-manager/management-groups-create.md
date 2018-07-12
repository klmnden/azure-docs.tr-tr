---
title: Azure kaynaklarını düzenlemek için Yönetim Grup oluşturma | Microsoft Docs
description: Birden çok kaynağı yönetmek için Azure yönetim gruplarının nasıl oluşturulacağını öğrenin.
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
ms.openlocfilehash: f2b596b34aa18d20fa888ad40e82eccb90d5fd8c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38465780"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Kaynak kuruluşta ve Yönetim için Yönetim grupları oluşturma
Yönetim grupları Aboneliklerdeki erişim, ilke ve uyumluluk yardımcı kapsayıcıları yönetmek olan. İle kullanılabilen bir etkili ve verimli bir hiyerarşi oluşturmak için bu kapsayıcıları [Azure İlkesi](../azure-policy/azure-policy-introduction.md) ve [Azure rol tabanlı erişim denetimlerini](../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi için bkz. [kaynaklarınızı Azure yönetim gruplarıyla düzenleme ](management-groups-overview.md). 

Yönetim grubu özellik genel önizlemede kullanılabilir. Yönetim kullanmaya başlamak için gruplar, oturum açma [Azure portalında](https://portal.azure.com) veya kullanabilirsiniz [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), veya [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) için Yönetim grupları oluşturun.   

Dizinde oluşturulan ilk yönetim grubu tamamlanması 15 dakika sürebilir. Dizininiz için Azure Yönetim grupları hizmeti ayarlamak için ilk kez çalışan işlemler vardır. İşlem tamamlandığında, bir bildirim alırsınız.  

## <a name="how-to-create-a-management-group"></a>Bir yönetim grubu oluşturma
Portal, PowerShell veya Azure CLI kullanarak, yönetim grubu oluşturabilirsiniz.

### <a name="create-in-portal"></a>Portalda oluşturma

1. [Azure portalında](http://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** > **Yönetim grupları**.
3. Ana sayfasında göze seçin **yeni yönetim grubu.** 

    ![Ana Grup](media/management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim grubu Tanıtıcısı** bu yönetim grubunda komutları göndermek için kullanılan dizini benzersiz tanımlayıcısı. Bu Azure sistem genelinde bu grubu tanımlamak için kullanılan bu tanımlayıcıyı oluşturulduktan sonra düzenlenemez. 
    - Görünen ad alanı, Azure portalında görüntülenen addır. Yönetim oluştururken ayrı görünen ad isteğe bağlı bir alan olan Grup ve herhangi bir zamanda değiştirilebilir.  

    ![Oluştur](media/management-groups/create_context_menu.png)  
5.  Seçin **Kaydet**


### <a name="create-in-powershell"></a>PowerShell'de oluşturma
PowerShell içinde Ekle AzureRmManagementGroups cmdlet'leri kullanın.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
**GroupName** oluşturulan bir benzersiz tanımlayıcısı. Bu kimliği, başka komutlarla bu Grup başvurmak için kullanılır ve daha sonra değiştirilemez.

Azure Portal'ın farklı bir ad göstermek için yönetim grubu istediğinizi varsa, eklersiniz **DisplayName** dizesiyle parametre. Örneğin, bir yönetim grubu, GroupName Contoso ve görünen adı "Contoso grup" ile oluşturmak istediyseniz, aşağıdaki cmdlet'i kullanın: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Kullanım **parentId** parametresi bu yönetim grubu için farklı bir yönetim altına oluşturulabilir.  

### <a name="create-in-azure-cli"></a>Azure CLI ile oluşturma
Azure CLI, az kullandığınız hesabı yönetim grubu oluşturma komutu. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Sonraki adımlar 
Yönetim grupları hakkında daha fazla bilgi için bkz: 
- [Kaynaklarınızı Azure Yönetim grupları ile düzenleme ](management-groups-overview.md)
- [Değiştirme, silme veya yönetim gruplarınızı yönetme](management-groups-manage.md)
- [Azure Powershell modülünü yükleme](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Belirtimi gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI uzantısını yükleme](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
