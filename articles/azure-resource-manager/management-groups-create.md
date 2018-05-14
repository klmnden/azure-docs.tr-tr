---
title: Azure kaynaklarını düzenlemek için yönetim gruplar oluşturun | Microsoft Docs
description: Birden çok kaynakları yönetmek için Azure Yönetim grupları oluşturmayı öğrenin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: 1592e47509f2537bef9cbcefd3cf49618561edcc
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Kaynak kuruluştaki ve Yönetim için Yönetim grupları oluşturma
Yönetim erişimi, ilke ve uyumluluk arasında birden çok abonelik yönetmenize yardımcı kapsayıcılardır gruplarıdır. İle birlikte kullanılabilen bir etkili ve verimli hiyerarşisi oluşturmak için bu kapsayıcıları oluşturma [Azure ilke](../azure-policy/azure-policy-introduction.md) ve [Azure rol tabanlı erişim denetimlerini](../role-based-access-control/overview.md). Yönetim grupları hakkında daha fazla bilgi için bkz: [kaynaklarınızı Azure Yönetim grupları ile düzenleme ](management-groups-overview.md). 

Yönetim grubu özelliğini genel önizleme olarak kullanılabilir. Management'ı kullanmaya başlamak için gruplar, oturum açma [Azure portal](https://portal.azure.com) veya kullanabilirsiniz [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [Azure CLI](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), veya [REST API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) için Yönetim grupları oluşturun.   

Directory oluşturduğunuz ilk yönetim grubu tamamlanması 15 dakika sürebilir. Dizininiz için Azure içinde Yönetim grupları hizmeti ayarlamak için ilk kez çalıştırma işlemleri vardır. İşlem tamamlandığında, bir bildirim alırsınız.  

## <a name="how-to-create-a-management-group"></a>Bir yönetim grubu oluşturma
Portal, PowerShell veya Azure CLI kullanarak yönetim grubu oluşturabilirsiniz.

### <a name="create-in-portal"></a>Portalı'nda oluşturma

1. İçine oturum [Azure portal](http://portal.azure.com).
2. Seçin **tüm hizmetleri** > **Yönetim grupları**.
3. Ana sayfada seçin **yeni yönetim grubu.** 

    ![Ana grubu](media/management-groups/main.png) 
4.  Yönetim grubu kimliği alanını doldurun. 
    - **Yönetim grubu Tanıtıcısı** bu yönetim grubu komutlarını göndermek için kullanılan dizini benzersiz tanımlayıcısı değil. Bu grup tanımlamak için Azure sistem genelinde kullanıldığı şekilde bu tanımlayıcı oluşturulduktan sonra düzenlenemez. 
    - Görünen ad alanı Azure portalı içinde görüntülenen addır. Yönetim oluştururken, isteğe bağlı bir alan ayrı görünen adı olduğu grup ve herhangi bir zamanda değiştirilebilir.  

    ![Oluştur](media/management-groups/create_context_menu.png)  
5.  Seçin **Kaydet**


### <a name="create-in-powershell"></a>PowerShell'de oluşturma
PowerShell içinde Ekle AzureRmManagementGroups cmdlet'lerini kullanın.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
**GroupName** oluşturulan benzersiz tanımlayıcısı. Bu kimliği, başka komutlarla bu grubuna başvurmak için kullanılır ve daha sonra değiştirilemez.

Azure portalı içinde farklı bir ad göstermek için yönetim grubu istediğinizi varsa, eklediğiniz **DisplayName** dizesi parametresi. Örneğin, bir yönetim grubu, GroupName Contoso ve "Contoso grubu" görünen adını oluşturmak istemeniz durumunda, aşağıdaki cmdlet'i kullanın: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Kullanım **parentId** parametresi bu yönetim grubu için farklı bir yönetim altında oluşturulabilir.  

### <a name="create-in-azure-cli"></a>Azure CLI içinde oluşturma
Azure CLI üzerinde az kullandığınız hesap yönetimi-group komutu oluşturun. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Sonraki adımlar 
Yönetim grupları hakkında daha fazla bilgi için bkz: 
- [Azure Yönetim grupları ile kaynaklarınızı düzenleme ](management-groups-overview.md)
- [Değiştirme, silme veya Yönetim gruplarını yönet](management-groups-manage.md)
- [Azure Powershell modülünü yükleyin](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Spec gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Azure CLI uzantısını yükleyin](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
