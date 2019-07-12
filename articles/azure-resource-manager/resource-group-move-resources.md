---
title: Azure kaynakları için yeni bir abonelik veya kaynak grubu taşıma | Microsoft Docs
description: Kaynakları yeni kaynak grubuna veya aboneliğe taşıma için Azure Resource Manager'ı kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 01ec8facf2771de9ec01b9470521340a59ee4d0d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721391"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Kaynakları yeni kaynak grubuna veya aboneliğe taşıma

Bu makalede, başka bir Azure aboneliğine veya başka bir kaynak grubuna aynı abonelik altında Azure kaynakları taşıma işlemini göstermektedir. Kaynakları taşıma için Azure portalı, Azure PowerShell, Azure CLI veya REST API'yi kullanabilirsiniz.

Kaynak grubu hem de hedef grubu taşıma işlemi sırasında kilitlenir. Yazma ve silme işlemleri taşıma işlemi tamamlanana kadar kaynak gruplarında engellenir. Bu kilit ekleyemez, güncelleştirme veya kaynak gruplarındaki kaynakları silin, ancak kaynakları dondurulmuş gelmez anlamına gelir. Örneğin, bir SQL Server ve veritabanı yeni bir kaynak grubuna taşırsanız, veritabanı kullanan bir uygulama kapalı kalma süresi olmadan karşılaşır. Bunu hala okuyabilir ve veritabanına yazma.

Bir kaynak taşıma yalnızca bu yeni kaynak grubuna veya aboneliğe taşınır. Bu kaynağın konumunu değiştirmez.

## <a name="checklist-before-moving-resources"></a>Kaynakları taşımadan önce Yapılacaklar listesi

Bir kaynağı taşımadan önce yapmanız gereken bazı önemli adımlar vardır. Bu koşulları doğrulayarak hataları önleyebilirsiniz.

1. Taşımak istediğiniz kaynakları taşıma işlemi desteklemesi gerekir. Kaynakları taşıma destekleyen bir listesi için bkz [taşıma işlemi Destek kaynakları için](move-support-resources.md).

1. Bazı hizmetler kaynakları taşırken belirli sınırlamalar ve gereksinimler vardır. Aşağıdaki hizmetlerden herhangi biri taşıma seçtiğiniz taşımadan önce bu kılavuzu kontrol edin.

   * [Uygulama Hizmetleri Taşıma Kılavuzu](./move-limitations/app-service-move-limitations.md)
   * [Azure DevOps hizmetleriyle Taşıma Kılavuzu](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Klasik dağıtım modeline Taşıma Kılavuzu](./move-limitations/classic-model-move-limitations.md) -Klasik işlem, Klasik depolama, Klasik sanal ağlar ve bulut Hizmetleri
   * [Kurtarma Hizmetleri Taşıma Kılavuzu](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Sanal makineleri Taşıma Kılavuzu](./move-limitations/virtual-machines-move-limitations.md)
   * [Sanal ağlar Taşıma Kılavuzu](./move-limitations/virtual-network-move-limitations.md)

1. Kaynak ve hedef abonelikler etkin olması gerekir. Hesabınız devre dışı bırakıldı, etkinleştirme yaşıyorsanız [bir Azure destek isteği oluşturma](../azure-supportability/how-to-create-azure-support-request.md). Seçin **abonelik yönetimi** sorun türü için.

1. Kaynak ve hedef abonelikler aynı içinde bulunmalıdır [Azure Active Directory kiracısı](../active-directory/develop/quickstart-create-new-tenant.md). Her iki aboneliğin aynı Kiracı Kimliğine sahip denetlemek için Azure PowerShell veya Azure CLI'yı kullanın.

   Azure PowerShell için şunu kullanın:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Azure CLI için şunu kullanın:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Kaynak ve hedef abonelikler için Kiracı kimlikleri aynı değilse, Kiracı kimlikleri karşılaştırmak için aşağıdaki yöntemleri kullanın:

   * [Azure aboneliğinin sahipliğini başka bir hesaba devretme](../billing/billing-subscription-transfer.md)
   * [Azure Active Directory'ye bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Hedef abonelik, taşınan kaynağın kaynak sağlayıcısına kayıtlı olmalıdır. Belirten bir hata alırsanız, **kaynak türü için abonelik kayıtlı değil**. Abonelik bu kaynak türü ile hiçbir zaman kullanılmış, ancak yeni bir abonelik için bir kaynak taşıma sırasında şu hatayla karşılaşabilirsiniz.

   PowerShell için kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Bir kaynak sağlayıcısını kaydetmek için kullanın:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Azure CLI için kayıt durumunu almak için aşağıdaki komutları kullanın:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Bir kaynak sağlayıcısını kaydetmek için kullanın:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Kaynakları taşıma hesabı, en az aşağıdaki izinlere sahip olmanız gerekir:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** kaynak kaynak grubu.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** hedef kaynak grubunda.

1. Kaynakları taşımadan önce kaynaklara Taşımakta olduğunuz abonelik için abonelik kotaları denetleyin. Kaynakları taşıma abonelik, sınırları aşamaz anlamına gelir, kota artışı isteği olup olmadığını gözden geçirmek gerekir. Limitler ve bir artış istemek nasıl bir listesi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).

## <a name="validate-move"></a>Taşıma doğrula

[Taşıma işlemi doğrulama](/rest/api/resources/resources/validatemoveresources) kaynakları taşımadan taşıma senaryonuza sınamanızı sağlar. Taşıma başarılı olur denetlemek için bu işlemi kullanın. Taşıma isteği gönderdiğinizde, doğrulama otomatik olarak çağrılır. Bu işlem, sonuçları önceden gerektiğinde kullanın. Bu işlemin çalıştırılması için ihtiyacınız vardır:

* Kaynak kaynak grubu adı
* Hedef kaynak grubunun kaynak kimliği
* Her bir kaynağın kaynak kimliği taşımak için
* [erişim belirteci](/rest/api/azure/#acquire-an-access-token) hesabınız için

Aşağıdaki isteği gönder:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

İstek gövdesi ile:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

İstek düzgün biçimlendirilmiş olup olmadığını, işlemi döndürür:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

Doğrulama isteği kabul edildi, ancak henüz taşıma işlemi başarılı olur, belirlenen taşınmadığından 202 durum kodunu gösterir. `location` Değeri uzun süre çalışan işlemin durumunu denetlemek için kullandığınız bir URL içerir.  

Durumu denetlemek için aşağıdaki isteği gönder:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

İşlemi hala devam ederken, 202 durum kodu almaya devam eder. Belirtilen saniye sayısı bekleyin `retry-after` yeniden denemeden önce değeri. Taşıma işlemi başarıyla doğrular, 204 durum kodu alırsınız. Taşıma doğrulaması başarısız olursa, bir hata iletisi gibi alırsınız:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Portalı kullanma

Kaynakları taşıma için bu kaynakları içeren kaynak grubunu seçin ve ardından **taşıma** düğmesi.

![kaynakları taşıma](./media/resource-group-move-resources/select-move.png)

Kaynakları yeni kaynak grubu veya yeni bir aboneliğe taşıyor seçin.

Taşınacak kaynaklar ve hedef kaynak grubunu seçin. Bu kaynaklar için betiklerini güncelleştirin ve seçmek gereken bildirimi **Tamam**. Önceki adımda düzenleme abonelik simgesini seçtiğinizde hedef abonelik de seçmeniz gerekir.

![hedef seçin](./media/resource-group-move-resources/select-destination.png)

İçinde **bildirimleri**, taşıma işleminin çalıştığını görürsünüz.

![taşıma durumu göster](./media/resource-group-move-resources/show-status.png)

Tamamlandığında, sonucunu bildirilir.

![taşıma sonucu göster](./media/resource-group-move-resources/show-result.png)

Bir hata alırsanız bkz [Azure kaynakları yeni kaynak grubuna veya aboneliğe taşıma sorun giderme](troubleshoot-move.md).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [taşıma AzResource](/powershell/module/az.resources/move-azresource) komutu. Aşağıdaki örnek, çeşitli kaynakları yeni kaynak grubuna taşımak gösterilmektedir.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Yeni bir aboneliğe taşımak için bir değer içerir. `DestinationSubscriptionId` parametresi.

Bir hata alırsanız bkz [Azure kaynakları yeni kaynak grubuna veya aboneliğe taşıma sorun giderme](troubleshoot-move.md).

## <a name="use-azure-cli"></a>Azure CLI kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [az kaynak taşıma](/cli/azure/resource?view=azure-cli-latest#az-resource-move) komutu. Kaynak taşımak için kaynak kimliklerini sağlayın. Aşağıdaki örnek, çeşitli kaynakları yeni kaynak grubuna taşımak gösterilmektedir. İçinde `--ids` parametresi, kaynak kimliklerini taşımak için boşlukla ayrılmış bir listesini sağlayın.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Yeni bir aboneliğe taşımak için sağlamak `--destination-subscription-id` parametresi.

Bir hata alırsanız bkz [Azure kaynakları yeni kaynak grubuna veya aboneliğe taşıma sorun giderme](troubleshoot-move.md).

## <a name="use-rest-api"></a>REST API’yi kullanma

Var olan kaynakları başka bir kaynak grubuna veya aboneliğe taşıma için kullanın [kaynakları taşıma](/rest/api/resources/Resources/MoveResources) işlemi.

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

İstek gövdesinde taşımak için hedef kaynak grubu ve kaynakları belirtin.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Bir hata alırsanız bkz [Azure kaynakları yeni kaynak grubuna veya aboneliğe taşıma sorun giderme](troubleshoot-move.md).

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma destekleyen bir listesi için bkz [taşıma işlemi Destek kaynakları için](move-support-resources.md).
