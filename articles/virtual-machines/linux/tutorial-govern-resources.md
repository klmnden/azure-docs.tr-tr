---
title: "Azure CLI ile Azure sanal makineleri yöneten | Microsoft Docs"
description: "Öğretici - Azure sanal makineleri RBAC uygulayarak yönetmek, ilkeler, kilitler ve Azure CLI ile etiketler"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: tomfitz
ms.openlocfilehash: ac6f7b0d32479e9e7e9945f83dc63a5847cba6a4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="virtual-machine-governance-with-azure-cli"></a>Azure CLI ile sanal makine Yönetimi

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yükleyip CLI yerel olarak kullanmak için bkz: [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="understand-scope"></a>Kapsam anlama

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

İşiniz bittiğinde, bu ayarları kolayca kaldırabilmeniz için Bu öğreticide, tüm yönetim ayarlarını bir kaynak grubuna uygulayın.

Bu kaynak grubu oluşturalım.

```azurecli-interactive
az group create --name myResourceGroup --location "East US"
```

Kaynak grubu şu anda boştur.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Kuruluşunuzdaki kullanıcıların bu kaynaklara erişim doğru düzeyde sahip olduğunuzdan emin olmak istersiniz. Sınırsız erişimi kullanıcılara vermek istediğiniz yoktur, ancak işlerini yapmak için emin olmanız gerekir. [Rol tabanlı erişim denetimi](../../active-directory/role-based-access-control-what-is.md) hangi kullanıcıların belirli eylemleri bir kapsamda tamamlamak için izni yönetmenizi sağlar.

Oluşturma ve rol atamalarını kaldırmak için kullanıcıların olmalıdır `Microsoft.Authorization/roleAssignments/*` erişim. Bu erişim sahibi veya kullanıcı erişimi yöneticisi rolleri aracılığıyla verilir.

Sanal makine çözümleri yönetmek için yaygın olarak gerekli erişim sağlayan üç kaynağa özel rollere vardır:

* [Sanal makine Katılımcısı](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor)
* [Ağ Katılımcısı](../../active-directory/role-based-access-built-in-roles.md#network-contributor)
* [Depolama hesabı katkıda bulunan](../../active-directory/role-based-access-built-in-roles.md#storage-account-contributor)

Tek tek kullanıcılara roller atama yerine genellikle daha kolay olur [bir Azure Active Directory grubu oluşturun](../../active-directory/active-directory-groups-create-azure-portal.md) benzer önlemler almak için gereken kullanıcılar için. Ardından, bu grup için uygun rolü atayın. Bu makalede basitleştirmek için bir Azure Active Directory grubu üyeleri olmadan oluşturun. Hala bu grubun bir kapsam için bir rol atayabilirsiniz. 

Aşağıdaki örnek adlı bir Azure Active Directory grubu oluşturur *VMDemoContributors* bir posta takma adı ile *vmDemoGroup*. Posta takma ad grubu için bir diğer ad olarak görev yapar.

```azurecli-interactive
adgroupId=$(az ad group create --display-name VMDemoContributors --mail-nickname vmDemoGroup --query objectId --output tsv)
```

Azure Active Directory yayılmasına grubu için komut istemi döndükten sonra bir dakika sürer. 20 veya 30 saniye bekledikten sonra kullanın [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create) yeni Azure Active Directory grubu kaynak grubu için sanal makine Katılımcısı rolüne atamak için komutu.  Bunu yayılmadan önce aşağıdaki komutu çalıştırırsanız, belirten bir hata alırsınız **asıl <guid> dizininde yok**. Komutu yeniden çalıştırmayı deneyin.

```azurecli-interactive
az role assignment create --assignee-object-id $adgroupId --role "Virtual Machine Contributor" --resource-group myResourceGroup
```

Genellikle, işlem için yineleme *ağ Katılımcısı* ve *depolama hesabı katkıda bulunan* dağıtılan kaynakları yönetmek için atanan kullanıcılar emin olmak için. Bu makalede, bu adımı atlayabilirsiniz.

## <a name="azure-policies"></a>Azure ilkeleri

[!INCLUDE [Resource Manager governance policy](../../../includes/resource-manager-governance-policy.md)]

### <a name="apply-policies"></a>İlkelerini uygula

Birkaç ilke tanımları aboneliğiniz zaten vardır. Mevcut ilke tanımları görmek için [az ilke tanım listesi](/cli/azure/policy/definition#az_policy_definition_list) komutu:

```azurecli-interactive
az policy definition list --query "[].[displayName, policyType, name]" --output table
```

Mevcut ilke tanımları bakın. İlke türü olan **yerleşik** veya **özel**. Ata istediğiniz bir koşul açıklayan olanları tanımlarında bakın. Bu makalede, ilkeler, ata:

* Tüm kaynaklar konumlarını sınırlayın.
* Sanal makineler için SKU'ları sınırlayın.
* Sanal makineler, yönetilen diskleri kullanmayın denetim.

Aşağıdaki örnekte, üç ilke tanımları görünen adını temel alarak alın. Kullandığınız [az ilke ataması oluşturma](/cli/azure/policy/assignment#az_policy_assignment_create) bu tanımları kaynak grubuna atamak için komutu. Bazı ilkeler için izin verilen değerleri belirtmek için parametre değerlerini sağlayın.

```azurecli-interactive
# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
locationDefinition=$(az policy definition list --query "[?displayName=='Allowed locations'].name | [0]" --output tsv)
skuDefinition=$(az policy definition list --query "[?displayName=='Allowed virtual machine SKUs'].name | [0]" --output tsv)
auditDefinition=$(az policy definition list --query "[?displayName=='Audit VMs that do not use managed disks'].name | [0]" --output tsv)

# Assign policy for allowed locations
az policy assignment create --name "Set permitted locations" \
  --resource-group myResourceGroup \
  --policy $locationDefinition \
  --params '{ 
      "listOfAllowedLocations": {
        "value": [
          "eastus", 
          "eastus2"
        ]
      }
    }'

# Assign policy for allowed SKUs
az policy assignment create --name "Set permitted VM SKUs" \
  --resource-group myResourceGroup \
  --policy $skuDefinition \
  --params '{ 
      "listOfAllowedSKUs": {
        "value": [
          "Standard_DS1_v2", 
          "Standard_E2s_v2"
        ]
      }
    }'

# Assign policy for auditing unmanaged disks
az policy assignment create --name "Audit unmanaged disks" \
  --resource-group myResourceGroup \
  --policy $auditDefinition
```

Önceki örnekte, bir ilke parametrelerini zaten biliyor varsayar. Parametreleri görüntülemek gereken durumlarda kullanın:

```azurecli-interactive
az policy definition show --name $locationDefinition --query parameters
```

## <a name="deploy-the-virtual-machine"></a>Sanal makine dağıtma

Çözümünüzü dağıtmak hazırsınız rolleri ve ilkeleri atadınız. Varsayılan boyutu, izin verilen SKU'lar biri Standard_DS1_v2 ' dir. Bir varsayılan konumda yoksa komut SSH anahtarları oluşturur.

```azurecli-interactive
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Dağıtımınız tamamlandıktan sonra çözüme daha fazla yönetim ayarlarını uygulayabilirsiniz.

## <a name="lock-resources"></a>Kaynakları kilitleme

[Kaynak kilitleri](../../azure-resource-manager/resource-group-lock-resources.md) yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzdaki kullanıcıların engelleme. Rol tabanlı erişim denetimi farklı olarak, tüm kullanıcılar ve roller bir kısıtlama kaynak kilitleri uygulayın. Kilit düzeyini ayarlayabilirsiniz *CanNotDelete* veya *salt okunur*.

Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/locks/*` eylemler. Yerleşik roller, yalnızca **sahibi** ve **kullanıcı erişimi Yöneticisi** bu eylemleri verilir.

Ağ güvenlik grubu ve sanal makine kilitlemek için kullanmak [az kilit oluşturmak](/cli/azure/lock#az_lock_create) komutu:

```azurecli-interactive
# Add CanNotDelete lock to the VM
az lock create --name LockVM \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVM \
  --resource-type Microsoft.Compute/virtualMachines

# Add CanNotDelete lock to the network security group
az lock create --name LockNSG \
  --lock-type CanNotDelete \
  --resource-group myResourceGroup \
  --resource-name myVMNSG \
  --resource-type Microsoft.Network/networkSecurityGroups
```

Kilitler sınamak için aşağıdaki komutu çalıştırarak deneyin:

```azurecli-interactive 
az group delete --name myResourceGroup
```

Silme işlemi nedeniyle kilit gerçekleştirilemiyor bildiren bir hata görürsünüz. Kaynak grubu, yalnızca özellikle kilitler kaldırırsanız silinebilir. Bu adım gösterilen [kaynakları temizlemek](#clean-up-resources).

## <a name="tag-resources"></a>Etiket kaynakları

Uyguladığınız [etiketleri](../../azure-resource-manager/resource-group-using-tags.md) Azure kaynaklarınızı mantıksal olarak kategorilere göre düzenlemek için. Her etiket bir ad ve değerden oluşur. Örneğin, "Ortam" adını ve "Üretim" değerini üretimdeki tüm kaynaklara uygulayabilirsiniz.

[!INCLUDE [Resource Manager governance tags CLI](../../../includes/resource-manager-governance-tags-cli.md)]

Bir sanal makineye etiketleri uygulamak üzere kullanmak [az kaynak etiketi](/cli/azure/resource#az_resource_tag) komutu. Kaynak üzerinde varolan etiketleri korunmaz.

```azurecli-interactive
az resource tag -n myVM \
  -g myResourceGroup \
  --tags Dept=IT Environment=Test Project=Documentation \
  --resource-type "Microsoft.Compute/virtualMachines"
```

### <a name="find-resources-by-tag"></a>Etikete göre kaynakları bulun

Bir etiketi ad ve değerli kaynakları bulmak için [az kaynak listesi](/cli/azure/resource#az_resource_list) komutu:

```azurecli-interactive
az resource list --tag Environment=Test --query [].name
```

Döndürülen değerlerin bir etiket değeri olan tüm sanal makineleri durdurma gibi yönetim görevleri için kullanabilirsiniz.

```azurecli-interactive
az vm stop --ids $(az resource list --tag Environment=Test --query "[?type=='Microsoft.Compute/virtualMachines'].id" --output tsv)
```

### <a name="view-costs-by-tag-values"></a>Görünüm maliyetler etiket değerlerine göre

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kilit kaldırılana kadar kilitli ağ güvenlik grubu silinemiyor. Kilidi kaldırmak için kilitler kimliklerini almak ve bunları sağlamak [az kilit silme](/cli/azure/lock#az_lock_delete) komutu:

```azurecli-interactive
vmlock=$(az lock show --name LockVM \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Compute/virtualMachines \
  --resource-name myVM --output tsv --query id)
nsglock=$(az lock show --name LockNSG \
  --resource-group myResourceGroup \
  --resource-type Microsoft.Network/networkSecurityGroups \
  --resource-name myVMNSG --output tsv --query id)
az lock delete --ids $vmlock $nsglock
```

Artık gerekli değilse, [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz. SSH oturumundan sanal makinenize çıkış yapın, ardından kaynakları aşağıda belirtildiği gibi silin:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kullanıcılar için rol atama
> * Standartlar zorunlu ilkelerini uygula
> * Kilitleri olan kritik kaynaklarını koruma
> * Faturalama ve Yönetim için etiket kaynaklar

Nasıl yüksek oranda kullanılabilir sanal makineler hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitoring.md)

