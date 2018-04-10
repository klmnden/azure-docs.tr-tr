---
title: Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için Azure CLI kullanma | Microsoft Docs
description: Uyumlu olmayan kaynakları belirlemek üzere bir Azure İlkesi ataması oluşturmak için PowerShell kullanın.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/30/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 356521c0d1771906e41c816d523d687fe8902acd
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Azure CLI ile Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturun

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir. Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile *uyumsuzdur*.

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını yönetmek veya bu kaynakları oluşturmak için kullanılır. Bu kılavuzda, Azure ortamınızda uyumlu olmayan kaynakları belirlemek ve bir ilke ataması oluşturmak için Azure CLI kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmak için, bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Ön koşullar

Azure CLI kullanarak İlke Görüşleri kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısı kaydedildiğinde, aboneliğinizin bununla çalıştığından emin olunur. Bir kaynak sağlayıcısını kaydetmek için, kaynak sağlayıcısı kaydetme işlemini gerçekleştirme iznine sahip olmanız gerekir. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

```
az provider register –-namespace 'Microsoft.PolicyInsights
```
Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [Kaynak Sağlayıcıları ve Türleri](../azure-resource-manager/resource-manager-supported-services.md)


## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup Yönetilen Diskleri Olmayan Sanal Makineleri Denetle tanımını atayacaksınız. Bu ilke tanımı, ilke tanımında ayarlanan koşullarla uyumlu olmayan kaynakları belirler.

İlke ataması oluşturmak için aşağıdaki komutu çalıştırın:

```
az policy assignment create --name 'Audit Virtual Machines without Managed Disks Assignment' --scope '<scope>' --policy '<policy definition ID>' --sku 'standard'
```

Yukarıdaki komutta aşağıdaki bilgiler kullanılmaktadır:

- **Ad** - Bu, ilke ataması için görünen addır. Bu durumda, *Yönetilen Disk Ataması Olmayan Sanal Makineleri Denetle* seçeneğini kullanıyorsunuz.
- **İlke** - Bu, atamayı oluşturmak için kullandığınız ilke tanımı kimliğidir. Bu durumda, *Yönetilen Diskleri Olmayan Sanal Makineleri Denetle* ilke tanımıdır. İlke tanımı kimliğini almak için şu komutu çalıştırın: `az policy definition show --name 'Audit Virtual Machines without Managed Disks Assignment'`
- **Kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir. &lt;Kapsam&gt; yerine kaynak grubunuzun adını yazdığınızdan emin olun.
- **Sku** - Bu komut, standart katman ile bir ilke ataması oluşturur. Standart katman, ölçekli yönetim, uyumluluk değerlendirmesi ve düzeltme elde etmenize olanak sağlar. Fiyatlandırma katmanları hakkında ek bilgi için bkz. [Azure İlkesi fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-policy).


## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Bu yeni atama altında uyumlu olmayan kaynakları görüntülemek için aşağıdaki komutları çalıştırarak ilke ataması kimliğini alın:

```
$policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Virtual Machines without Managed Disks"}
```

```
$policyAssignment.PolicyAssignmentId
```

İlke ataması kimlikleri hakkında daha fazla bilgi için bkz. [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment).

Daha sonra, JSON dosyasına çıkarılan uyumlu olmayan kaynakların kaynak kimliklerini almak için aşağıdaki komutu çalıştırın:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Sonuçlarınız aşağıdaki örneğe benzer:

```
{
"@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
"@odata.count": 3,
"value": [
{
    "@odata.id": null,
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
    },
    {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
         },
{
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
      "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
         }

]
}

```

Sonuçlar, Azure portalı görünümünde **Uyumlu olmayan kaynaklar** bölümünde gördüklerinize benzer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer kılavuzlar, bu hızlı başlangıcı temel alır. Sonraki kılavuzlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutu çalıştırarak oluşturduğunuz atamayı silin:

```azurecli
az policy assignment delete –name Audit Virtual Machines without Managed Disks Assignment --scope /subscriptions/ <subscriptionID> / <resourceGroupName>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı atadınız.

İlkeleri atama hakkında daha fazla bilgi edinmek ve **gelecekte** oluşturduğunuz kaynakların uyumlu olduğundan emin olmak için şu öğretici ile devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./create-manage-policy.md)
