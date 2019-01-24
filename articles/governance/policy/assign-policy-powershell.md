---
title: Azure PowerShell ile uyumlu olmayan kaynakları belirlemek üzere bir ilke oluşturun
description: Uyumlu olmayan kaynakları belirlemek üzere bir Azure İlkesi ataması oluşturmak için Azure PowerShell kullanırsınız.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: b5f4306fc1627e679f8f59a92bae4124a48cbd42
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856477"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-using-azure-powershell"></a>Azure PowerShell kullanarak uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir. Bu hızlı başlangıçta, yönetilen disk kullanmayan sanal makineleri belirlemek üzere bir ilke ataması oluşturun. Tamamlandığında, ilke atamasıyla *uyumlu olmayan* sanal makineleri belirleyeceksiniz.

Azure PowerShell modülü, komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu kılavuzda, Az bir ilke ataması oluşturmak için nasıl kullanılacağı açıklanmaktadır. İlke, Azure ortamınızdaki uyumlu olmayan kaynakları belirler.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Henüz yapmadıysanız [ARMClient](https://github.com/projectkudu/ARMClient)’ı yükleyin. Bu, HTTP isteklerini Azure Resource Manager tabanlı API’lere gönderen bir araçtır.
- Başlamadan önce Azure PowerShell'in en son sürümünü yüklü olduğundan emin olun. Bkz: [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps) ayrıntılı bilgi için.
- Azure PowerShell kullanarak İlke Görüşleri kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısı kaydedildiğinde, aboneliğinizin bununla çalıştığından emin olunur. Bir kaynak sağlayıcısını kaydetmek için kayıt kaynak sağlayıcısı işlemi izni olmalıdır. Bu işlem, Katkıda Bulunan ve Sahip rolleriyle birlikte sunulur. Aşağıdaki komutu çalıştırarak kaynak sağlayıcısını kaydedin:

  ```azurepowershell-interactive
  Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Kaynak sağlayıcıları kaydetme ve görüntülemeyle ilgili daha fazla bilgi için bkz. [Kaynak Sağlayıcıları ve Türleri](../../azure-resource-manager/resource-manager-supported-services.md)

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturma ve atama *denetim Vm'leri yönetilen disklere sahip olmayan* tanımı. Bu ilke tanımı ilke tanımında ayarlanan koşullar ile uyumlu olmayan kaynakları belirler.

Yeni ilke ataması oluşturmak için aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
$rg = Get-AzResourceGroup -Name '<resourceGroupName>'
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }
New-AzPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit VMs without managed disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition
```

Yukarıdaki komutlarda aşağıdaki bilgiler kullanılmaktadır:

- **Ad** - Atamanın gerçek adı.  Bu örnekte *audit-vm-manageddisks* kullanıldı.
- **Görünen Ad** - Bu ilke atamasının görünen adı. Bu durumda, kullanmakta olduğunuz *yönetilen disk ataması olmayan denetim VM'ler*.
- **Tanım** - Bu, atamayı oluşturmak için kullandığınız ilke tanımıdır. Bu durumda, ilke tanımı kimliğidir *denetim yönetilen diskleri kullanmayan Vm'leri*.
- **Kapsam** - Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Bir abonelikten kaynak gruplarına kadar değişiklik gösterebilir. &lt;Kapsam&gt; yerine kaynak grubunuzun adını yazdığınızdan emin olun.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemeye hazırsınız.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Oluşturduğunuz ilke atamasıyla uyumlu olmayan kaynakları belirlemek için aşağıdaki bilgileri kullanın. Aşağıdaki komutları çalıştırın:

```azurepowershell-interactive
$policyAssignment = Get-AzPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs without managed disks Assignment' }
$policyAssignment.PolicyAssignmentId
```

İlke ataması kimlikleri hakkında daha fazla bilgi için bkz. [Get-AzPolicyAssignment](/powershell/module/az.resources/get-azpolicyassignment).

Daha sonra, JSON dosyasına çıkarılan uyumlu olmayan kaynakların kaynak kimliklerini almak için aşağıdaki komutu çalıştırın:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

Sonuçlarınız aşağıdaki örneğe benzer:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
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

Oluşturduğunuz atamayı kaldırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure ortamınızda uyumlu olmayan kaynakları belirlemek üzere bir ilke tanımı atadınız.

Yeni kaynakların uyumlu olduğunu doğrulamak için ilkeleri atama hakkında daha fazla bilgi için öğreticisiyle devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)