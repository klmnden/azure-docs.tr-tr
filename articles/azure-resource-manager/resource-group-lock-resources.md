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
ms.date: 01/03/2018
ms.author: tomfitz
ms.openlocfilehash: e25de0366126ceee988eb253b66d18c9b8b62e1f
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
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
Aşağıdaki örnek bir uygulama hizmeti planı, bir web sitesi ve bir kilit üzerinde web sitesi oluşturur bir şablonu gösterir. Kaynak türü kilit kilitlemek için kaynak kaynak türüdür ve **/sağlayıcıları/kilitleri**. Kilit adı ile kaynak adı birleştirerek oluşturulan **/Microsoft.Authorization/** ve kilit adı.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "hostingPlanName": {
            "type": "string"
        }
    },
    "variables": {
        "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2016-09-01",
            "type": "Microsoft.Web/serverfarms",
            "name": "[parameters('hostingPlanName')]",
            "location": "[resourceGroup().location]",
            "sku": {
                "tier": "Free",
                "name": "f1",
                "capacity": 0
            },
            "properties": {
                "targetWorkerCount": 1
            }
        },
        {
            "apiVersion": "2016-08-01",
            "name": "[variables('siteName')]",
            "type": "Microsoft.Web/sites",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
            ],
            "properties": {
                "serverFarmId": "[parameters('hostingPlanName')]"
            }
        },
        {
            "type": "Microsoft.Web/sites/providers/locks",
            "apiVersion": "2016-09-01",
            "name": "[concat(variables('siteName'), '/Microsoft.Authorization/siteLock')]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
            ],
            "properties": {
                "level": "CanNotDelete",
                "notes": "Site should not be deleted."
            }
        }
    ]
}
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroup -Name sitegroup -Location southcentralus
New-AzureRmResourceGroupDeployment -ResourceGroupName sitegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json -hostingPlanName plan0103
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli
az group create --name sitegroup --location southcentralus
az group deployment create --resource-group sitegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json --parameters hostingPlanName=plan0103
```

## <a name="powershell"></a>PowerShell
Kilit kullanarak kaynakları Azure PowerShell ile dağıtılan [yeni AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock) komutu.

Bir kaynağı kilitlemek için adını kaynak, kaynak türü ve kaynak grubu adını sağlayın.

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu kilitlemek için kaynak grubu adını sağlayın.

```powershell
New-AzureRmResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Kilit hakkında bilgi almak için kullanmak [Get-AzureRmResourceLock](/powershell/module/azurerm.resources/get-azurermresourcelock). Aboneliğinizdeki tüm kilitler almak için kullanın:

```powershell
Get-AzureRmResourceLock
```

Bir kaynak için tüm kilitleri almak için kullanın:

```powershell
Get-AzureRmResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu için tüm kilitleri almak için kullanın:

```powershell
Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup
```

Kilit silmek için kullanın:

```powershell
$lockId = (Get-AzureRmResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzureRmResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure CLI

Kilit kullanarak Azure CLI kaynaklarla dağıtılan [az kilit oluşturmak](/cli/azure/lock#create) komutu.

Bir kaynağı kilitlemek için adını kaynak, kaynak türü ve kaynak grubu adını sağlayın.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Bir kaynak grubu kilitlemek için kaynak grubu adını sağlayın.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Kilit hakkında bilgi almak için kullanmak [az kilit listesi](/cli/azure/lock#list). Aboneliğinizdeki tüm kilitler almak için kullanın:

```azurecli
az lock list
```

Bir kaynak için tüm kilitleri almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Bir kaynak grubu için tüm kilitleri almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Kilit silmek için kullanın:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

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
* Mantıksal olarak kaynaklarınızı düzenleme hakkında bilgi edinmek için [etiketleri kullanarak kaynaklarınızı düzenleme](resource-group-using-tags.md)
* Kaynağın bulunduğu hangi kaynak grubunu değiştirmek için bkz: [kaynakları yeni kaynak grubuna taşıma](resource-group-move-resources.md)
* Özelleştirilmiş ilkeler aboneliğinizle arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

