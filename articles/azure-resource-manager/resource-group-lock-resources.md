---
title: Değişiklikleri önlemek için Azure kaynakları kilitleme | Microsoft Docs
description: Kullanıcının güncelleştiriliyor veya tüm kullanıcılar ve roller için bir kilit uygulayarak kritik Azure kaynakları silmesini engeller.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: tomfitz
ms.openlocfilehash: 31d77b4ea6e7594cd3ed4dba264f9ea6db4ca290
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155219"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Beklenmeyen değişiklikleri önlemek için kaynakları kilitleme 

Bir yönetici olarak bir aboneliğe, kaynak grubuna ya da kaynak yanlışlıkla kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcıların önlemek için kilit gerekebilir. Kilit düzeyini **CanNotDelete** veya **ReadOnly** olarak ayarlayabilirsiniz. Portalda, kilitler denir **Sil** ve **salt okunur** sırasıyla.

* **CanNotDelete** yetkili kullanıcılar hala okuyabilir ve bir kaynağı değiştirme, ancak bunlar bir kaynağı silemezsiniz anlamına gelir. 
* **Salt okunur** yetkili kullanıcılar, bir kaynak okuyabilir, ancak silebilir veya kaynak güncelleştirme anlamına gelir. Bu kilit uygulama tarafından verilen izinleri tüm yetkili kullanıcılara kısıtlama için benzer **okuyucu** rol. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-locks-are-applied"></a>Kilitleri nasıl uygulanır

Bir üst kapsamda bir kilit uyguladığınızda, ilgili kapsam içindeki tüm kaynaklar aynı kilit devralır. Kaynaklar daha sonra eklediğiniz kilit üst öğeden devralır. Devralmada en kısıtlayıcı kilit önceliklidir.

Rol tabanlı erişim denetimi, tüm kullanıcılar ve roller bir kısıtlama uygulamak yönetim kilitleri kullanın. Kullanıcılar ve roller için izinleri ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).

Resource Manager kilitleri uygulamak gönderilen operations oluşan yönetim düzlemi gerçekleşen işlemlere `https://management.azure.com`. Kilitler nasıl kaynakları kendi işlevleri gerçekleştiren kısıtlama. Kaynak değişiklikleri kısıtlıdır, ancak kaynak işlemleri sınırlı değildir. Örneğin, bir salt okunur kilidi SQL veritabanı, veritabanı silmesini veya engeller. Oluşturma, güncelleştirme ya da veritabanı verilerini silme engellemez. Bu işlemlerin gönderildiği değildir çünkü veri hareketlerini verilen `https://management.azure.com`.

Uygulama **salt okunur** kaynağı değiştirme görülüyor bazı işlemler kilit tarafından engellenmiş Eylemler gerçekten gerektirdiğinden beklenmeyen sonuçlara neden olabilir. **Salt okunur** kaynak veya kaynak içeren kaynak grubunu, kilit uygulanabilir. Sık karşılaşılan örneklerden bazıları tarafından engellenen işlemlerin bir **salt okunur** kilit şunlardır:

* A **salt okunur** bir depolama hesabı üzerindeki kilidi anahtarları listeleme gelen tüm kullanıcıları engeller. Yazma işlemlerini listenin döndürülen anahtarları için kullanılabilir olmadığından anahtarları işlemi bir POST isteği gerçekleştirilir.

* A **salt okunur** bir App Service kaynak kilidi, o etkileşime yazma erişim gerektirdiğinden kaynak dosyalarını görüntüleme Visual Studio sunucu Gezgini'nde engeller.

* A **salt okunur** kilit bir sanal makine içeren bir kaynak grubundaki tüm kullanıcıların başlatılıyor veya sanal makinenin yeniden başlatılması engeller. Bu işlemler, bir POST isteği gerektirir.

## <a name="who-can-create-or-delete-locks"></a>Kimin oluşturabilir veya kilitlerini Sil
Yönetim kilitlerini Sil ya da oluşturmak için erişimi olmalıdır. `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik rollerden yalnızca **Sahip** ve **Kullanııcı Erişiimi Yöneticisi** bu eylemleri kullanabilir.

## <a name="managed-applications-and-locks"></a>Yönetilen uygulamaları ve kilitler

Azure Databricks gibi bazı Azure Hizmetleri kullanın [yönetilen uygulamaları](../managed-applications/overview.md) hizmeti uygulamak için. Bu durumda, hizmet, iki kaynak grubu oluşturur. Bir kaynak grubu hizmetine genel bir bakış içerir ve kilitli değil. Bir kaynak grubu, hizmet için altyapıyı içerir ve kilitli.

Altyapı kaynak grubunu silmek çalışırsanız, kaynak grubu kilitli olduğunu bildiren bir hata alırsınız. Altyapı kaynak grubu için kilit silmeye çalışırsanız, bir sistem uygulaması tarafından sahiplenildiğinden kilit silinemiyor bildiren bir hata alın.

Bunun yerine, altyapı kaynak grubu da siler hizmeti silin.

Yönetilen uygulamalar için dağıttığınız hizmeti seçin.

![Hizmet seçin](./media/resource-group-lock-resources/select-service.png)

Bildirim hizmeti için bir bağlantı içeren bir **yönetilen kaynak grubu**. Bu kaynak grubu, altyapı tutar ve kilitli. Doğrudan silinemez.

![Yönetilen grubunu Göster](./media/resource-group-lock-resources/show-managed-group.png)

Hizmeti kilitli altyapı kaynak grubu da dahil olmak üzere her şeyi silmek için seçin **Sil** hizmeti.

![Hizmeti Sil](./media/resource-group-lock-resources/delete-service.png)

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Şablon

Kilit dağıtmak için Resource Manager şablonu kullanarak, adını ve türünü kapsamına bağlı olarak kilit için farklı değerler kullanın.

Kilit uygulanırken bir **kaynak**, aşağıdaki biçimleri kullanın:

* adı: `{resourceName}/Microsoft.Authorization/{lockName}`
* tür- `{resourceProviderNamespace}/{resourceType}/providers/locks`

Kilit uygulanırken bir **kaynak grubu** veya **abonelik**, aşağıdaki biçimleri kullanın:

* adı: `{lockName}`
* tür- `Microsoft.Authorization/locks`

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

Bir kaynak grubu üzerinde bir kilit ayarlama örneği için bkz: [bir kaynak grubu oluşturun ve kilitler](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment).

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

Bir kilitleme hakkında bilgi almak için kullanın [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock). Aboneliğinizdeki tüm kilitleri almak için kullanın:

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

Kapsam abonelik, kaynak grubu veya kaynak olabilir. Kilit adı, kilit çağırmak istediğiniz ' dir. Api-version, kullanın **2016-09-01**.

İstekte kilit özelliklerini belirten bir JSON nesnesi içerir.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Sonraki adımlar
* Kaynaklarınızı mantıksal olarak düzenleme hakkında bilgi edinmek için [kaynaklarınızı düzenlemek için etiketleri kullanma](resource-group-using-tags.md)
* Özelleştirilmiş ilkeler ile aboneliğinizin genelindeki kısıtlamaları ve kuralları uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure İlkesi nedir?](../governance/policy/overview.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).

