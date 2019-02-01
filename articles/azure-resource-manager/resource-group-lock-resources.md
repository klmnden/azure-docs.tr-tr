---
title: Değişiklikleri önlemek için Azure kaynakları kilitleme | Microsoft Docs
description: Kullanıcının güncelleştiriliyor veya tüm kullanıcılar ve roller için bir kilit uygulayarak kritik Azure kaynakları silmesini engeller.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 209a5a9c213a48920452230b1d684fdf0738e87d
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496916"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Beklenmeyen değişiklikleri önlemek için kaynakları kilitleme 

Bir yönetici olarak bir aboneliğe, kaynak grubuna ya da kaynak yanlışlıkla kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcıların önlemek için kilit gerekebilir. Kilit düzeyini **CanNotDelete** veya **ReadOnly** olarak ayarlayabilirsiniz. Portalda, kilitler denir **Sil** ve **salt okunur** sırasıyla.

* **CanNotDelete** yetkili kullanıcılar hala okuyabilir ve bir kaynağı değiştirme, ancak bunlar bir kaynağı silemezsiniz anlamına gelir. 
* **Salt okunur** yetkili kullanıcılar, bir kaynak okuyabilir, ancak silebilir veya kaynak güncelleştirme anlamına gelir. Bu kilit uygulama tarafından verilen izinleri tüm yetkili kullanıcılara kısıtlama için benzer **okuyucu** rol. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-locks-are-applied"></a>Kilitleri nasıl uygulanır

Bir üst kapsamda bir kilit uyguladığınızda, ilgili kapsam içindeki tüm kaynaklar aynı kilit devralır. Kaynaklar daha sonra eklediğiniz kilit üst öğeden devralır. Devralmada en kısıtlayıcı kilit önceliklidir.

Rol tabanlı erişim denetimi, tüm kullanıcılar ve roller bir kısıtlama uygulamak yönetim kilitleri kullanın. Kullanıcılar ve roller için izinleri ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).

Resource Manager kilitleri uygulamak gönderilen operations oluşan yönetim düzlemi gerçekleşen işlemlere `https://management.azure.com`. Kilitler nasıl kaynakları kendi işlevleri gerçekleştiren kısıtlamaz. Kaynak değişiklikleri kısıtlıdır, ancak kaynak işlemleri sınırlı değildir. Örneğin, bir salt okunur kilidi SQL veritabanı, veritabanı silmesini veya engeller, ancak bu, oluşturma, güncelleştirme veya silme verileri veritabanındaki engellemez. Bu işlemler için gönderilmediği için veri işlem izin verilen `https://management.azure.com`.

Uygulama **salt okunur** gibi görünen bazı işlemleri operations gerçekten gerekli ek eylemler okunur beklenmeyen sonuçlara neden olabilir. Örneğin, yerleştirme bir **salt okunur** bir depolama hesabı üzerindeki kilidi anahtarları listeleme gelen tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir. Başka bir örnek için yerleştirme bir **salt okunur** bir App Service kaynak kilidi, o etkileşime yazma erişim gerektirdiğinden kaynak dosyalarını görüntüleme Visual Studio sunucu Gezgini'nde engeller.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kimler oluşturabilir ve kuruluşunuzdaki kilitlerini Sil
Yönetim kilitlerini Sil ya da oluşturmak için erişimi olmalıdır. `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik rollerden yalnızca **Sahip** ve **Kullanııcı Erişiimi Yöneticisi** bu eylemleri kullanabilir.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Şablon
Aşağıdaki örnek, bir app service planı, bir web sitesi ve bir kilit üzerinde web sitesi oluşturan bir şablon gösterir. Kaynak türü kilit kilitlenecek kaynağın kaynak türüdür ve **/providers/kilitleri**. Kilit adı kaynak adı ile birleştirerek oluşturulur **/Microsoft.Authorization/** ve kilit adı.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroup -Name sitegroup -Location southcentralus
New-AzResourceGroupDeployment -ResourceGroupName sitegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json -hostingPlanName plan0103
```

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli
az group create --name sitegroup --location southcentralus
az group deployment create --resource-group sitegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json --parameters hostingPlanName=plan0103
```

## <a name="powershell"></a>PowerShell
Kilit dağıtılan kaynakların Azure PowerShell ile kullanarak [yeni AzResourceLock](/powershell/module/az.resources/new-azresourcelock) komutu.

Bir kaynak kilitlemek için kaynak, kaynak türünü ve kaynak grubu adını adını sağlayın.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu kilitlemek için kaynak grubunun adını sağlayın.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Bir kilitleme hakkında bilgi almak için kullanın [Get-AzureRmResourceLock](/powershell/module/az.resources/get-azresourcelock). Aboneliğinizdeki tüm kilitleri almak için kullanın:

```azurepowershell-interactive
Get-AzResourceLock
```

Bir kaynak için tüm kilit almak için kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Bir kaynak grubu için tüm kilit almak için kullanın:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Kilit silmek için kullanın:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>Azure CLI

Kilit dağıtılan kaynaklar ile Azure CLI kullanarak [az lock oluşturma](/cli/azure/lock#az-lock-create) komutu.

Bir kaynak kilitlemek için kaynak, kaynak türünü ve kaynak grubu adını adını sağlayın.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Bir kaynak grubu kilitlemek için kaynak grubunun adını sağlayın.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Bir kilitleme hakkında bilgi almak için kullanın [az lock listesi](/cli/azure/lock#az-lock-list). Aboneliğinizdeki tüm kilitleri almak için kullanın:

```azurecli
az lock list
```

Bir kaynak için tüm kilit almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Bir kaynak grubu için tüm kilit almak için kullanın:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Kilit silmek için kullanın:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>REST API
Dağıtılan kaynaklarla kilitleyebilirsiniz [yönetim kilitleri için REST API](https://docs.microsoft.com/rest/api/resources/managementlocks). REST API oluşturma ve kilitleri silin ve mevcut kilitleri hakkında bilgi almak sağlar.

Kilit oluşturabilmeniz için çalıştırın:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Kapsam abonelik, kaynak grubu veya kaynak olabilir. Kilit adı, kilit çağırmak istediğiniz ' dir. Api-version, kullanın **2015-01-01**.

İstekte kilit özelliklerini belirten bir JSON nesnesi içerir.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Sonraki adımlar
* Kaynaklarınızı mantıksal olarak düzenleme hakkında bilgi edinmek için [kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md)
* Kaynağın bulunduğu kaynak grubunu değiştirmek için bkz [kaynakları yeni kaynak grubuna taşıma](resource-group-move-resources.md)
* Özelleştirilmiş ilkeler ile aboneliğinizin genelindeki kısıtlamaları ve kuralları uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

