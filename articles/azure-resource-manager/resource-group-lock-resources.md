---
title: "Değişiklikleri önlemek için Azure kaynakları kilitleme | Microsoft Docs"
description: "Kullanıcının güncelleştirme veya tüm kullanıcılar ve roller için bir kilit uygulayarak kritik Azure kaynakları silmesini engeller."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 44c87b00f4fc63dbfd45a07d9a8cddc5eaf1a65c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Beklenmeyen değişiklikleri önlemek için kaynakları kilitleme 
Yönetici olarak, abonelik, kaynak grubu veya kaynak yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzda bulunan diğer kullanıcıların önlemek için kilitleme gerekebilir. Kilit düzeyini ayarlayabilirsiniz **CanNotDelete** veya **salt okunur**. 

* **CanNotDelete** yetkili kullanıcıların, hala okuyun ve kaynak değiştirebilirsiniz, ancak bir kaynağı silemezsiniz anlamına gelir. 
* **Salt okunur** yetkili kullanıcılar, bir kaynak okuyabilir, ancak silebilir veya kaynak güncelleştirme anlamına gelir. Bu kilit uygulama benzer tüm yetkili kullanıcılar tarafından verilen izinleri kısıtlamak için **okuyucu** rol. 

## <a name="how-locks-are-applied"></a>Kilitleri nasıl uygulanır

Bir üst kapsamda kilit uyguladığınızda, kapsamı içindeki tüm kaynakların aynı kilit devralır. Daha sonra eklediğiniz bile kaynakları kilidi üst devralır. Devralmada en kısıtlayıcı kilidi önceliklidir.

Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama uygulamak için yönetim kilitleri kullanın. Kullanıcılar ve roller için izinleri ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

Resource Manager kilitleri uygulamak gönderilen işlemleri oluşan yönetim düzeyi gerçekleşen işlemlerine `https://management.azure.com`. Kilitler nasıl kaynakları kendi işlevleri gerçekleştirmek kısıtlamaz. Kaynak kısıtlı değişir, ancak kaynak işlemlerinin sınırlı değildir. Örneğin, bir SQL veritabanı salt okunur kilit silme veya veritabanı değiştirme engeller, ancak bu, oluşturma, güncelleştirme veya silme verilerden veritabanındaki engellemez. Bu işlemler için gönderilmediği için veri hareketlerini izin verilen `https://management.azure.com`.

Uygulama **ReadOnly** gibi görünen bazı işlemler gerçekten ek eylemleri gerektirir okuma olduğundan beklenmeyen sonuçlara yol açabilir. Örneğin, yerleştirme bir **ReadOnly** kilit bir depolama hesabında anahtarları listeleme tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir. Başka bir örneğin yerleştirme bir **ReadOnly** kilit bir uygulama hizmeti kaynağına yazma erişimi bu etkileşimi gerektirmesi nedeniyle kaynak dosyalarını görüntüleme Visual Studio Sunucu Gezgini engeller.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kimin oluşturmak veya kuruluşunuzdaki kilitleri silmek mi
Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik roller, yalnızca **sahibi** ve **kullanıcı erişimi Yöneticisi** bu eylemleri verilir.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Şablon
Aşağıdaki örnek, bir depolama hesabı üzerinde bir kilit oluşturan bir şablonu gösterir. Depolama hesabı kilidi uygulanacağı parametre olarak sağlanır. Kilit adı ile kaynak adı birleştirerek oluşturulan **/Microsoft.Authorization/** ve bu durumda kilit adı **myLock**.

Sağlanan kaynak türü için belirli türüdür. Depolama için türü "Microsoft.Storage/storageaccounts/providers/locks" olarak ayarlayın.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="powershell"></a>PowerShell
Kilit kullanarak kaynakları Azure PowerShell ile dağıtılan [yeni AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) komutu.

Bir kaynağı kilitlemek için adını kaynak, kaynak türü ve kaynak grubu adını sağlayın.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite `
  -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu kilitlemek için kaynak grubu adını sağlayın.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete `
  -ResourceGroupName exampleresourcegroup
```

Kilit hakkında bilgi almak için kullanmak [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). Aboneliğinizdeki tüm kilitler almak için kullanın:

```powershell
Get-AzureRmResourceLock
```

Bir kaynak için tüm kilitleri almak için kullanın:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites `
  -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu için tüm kilitleri almak için kullanın:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Azure PowerShell diğer komutları gibi çalışma kilitlerini sağlar [kümesi AzureRmResourceLock](/powershell/module/azurerm.resources/set-azurermresourcelock) kilit, güncelleştirmeye ve [Kaldır AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock) kilit silmek için.

## <a name="azure-cli"></a>Azure CLI

Kilit kullanarak Azure CLI kaynaklarla dağıtılan [az kilit oluşturmak](/cli/azure/lock#create) komutu.

Bir kaynağı kilitlemek için adını kaynak, kaynak türü ve kaynak grubu adını sağlayın.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete \
  --resource-group exampleresourcegroup --resource-name examplesite \
  --resource-type Microsoft.Web/sites
```

Bir kaynak grubu kilitlemek için kaynak grubu adını sağlayın.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete \
  --resource-group exampleresourcegroup
```

Kilit hakkında bilgi almak için kullanmak [az kilit listesi](/cli/azure/lock#list). Aboneliğinizdeki tüm kilitler almak için kullanın:

```azurecli
az lock list
```

Bir kaynak için tüm kilitleri almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite \
  --namespace Microsoft.Web --resource-type sites --parent ""
```

Bir kaynak grubu için tüm kilitleri almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Azure CLI diğer komutları gibi çalışma kilitlerini sağlar [az kilit güncelleştirme](/cli/azure/lock#update) kilit, güncelleştirmeye ve [az kilit silmek](/cli/azure/lock#delete) kilit silmek için.

## <a name="rest-api"></a>REST API
Dağıtılan kaynaklarla kilitleyebilirsiniz [yönetim kilitleri için REST API](https://docs.microsoft.com/rest/api/resources/managementlocks). REST API oluşturmak ve kilitleri silin ve varolan kilitler hakkında bilgi almak etkinleştirir.

Kilit oluşturmak için çalıştırın:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. Kilit-ne olursa olsun kilidi aramak istediğiniz addır. API sürümü için kullanmak **2015-01-01**.

İstekte kilidi özelliklerini belirten bir JSON nesnesi içerir.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Sonraki adımlar
* Kaynak kilitleri ile çalışma hakkında daha fazla bilgi için bkz: [kilit aşağı bilgisayarınızı Azure kaynakları](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Mantıksal olarak kaynaklarınızı düzenleme hakkında bilgi edinmek için [etiketleri kullanarak kaynaklarınızı düzenleme](resource-group-using-tags.md)
* Kaynağın bulunduğu hangi kaynak grubunu değiştirmek için bkz: [kaynakları yeni kaynak grubuna taşıma](resource-group-move-resources.md)
* Özelleştirilmiş ilkeler aboneliğinizle arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Daha fazla bilgi için bkz. [Kaynakları yönetmek ve erişimi denetlemek için İlke kullanma](resource-manager-policy.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

