---
title: Azure CLI kullanarak Azure Resource Manager grupları yönetme | Microsoft Docs
description: Azure CLI, Azure Resource Manager gruplarınızı yönetmek için kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: c50a96b2598b89d5072a9441162d198163156c8d
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296282"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure CLI kullanarak Azure Resource Manager kaynak gruplarını yönetme

Azure CLI ile kullanmayı öğrenin [Azure Resource Manager](resource-group-overview.md) , Azure kaynak gruplarını yönetme. Azure kaynaklarını yönetmek için bkz: [Azure CLI kullanarak Azure kaynaklarınızı yönetme](./manage-resources-cli.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure portalını kullanarak Azure kaynak gruplarını yönetme](./manage-resources-portal.md)
- [Azure PowerShell kullanarak Azure kaynak gruplarını yönetme](./manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Bir kaynak grubu nedir

Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, kolayca dağıtabilir, güncelleştirme ve onları bir grup olarak silmek için aynı kaynak grubuna aynı yaşam döngüsünü paylaşan kaynakların ekleyin.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğinizde meta verilerin depolandığı belirlediniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturun

Aşağıdaki CLI betiği, bir kaynak grubu oluşturur ve sonra kaynak grubunu gösterir.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location $location
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listeleme

Aşağıdaki CLI betiği, aboneliğiniz altında kaynak grupları listeler.

```azurecli-interactive
az group list
```

Bir kaynak grubu almak için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silin

Aşağıdaki CLI betiği, bir kaynak grubunu siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Bkz: [kaynakları var olan bir kaynak grubuna dağıtma](./manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Bir kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturun ve kaynakları Resource Manager şablonu kullanarak grubuna dağıtın. Daha fazla bilgi için [kaynak grubu oluşturun ve kaynakları dağıtma](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik olarak da bilinir *hatada geri alma*. Daha fazla bilgi için [dağıtımı başarısız olduğunda yeniden](./resource-group-template-deploy-cli.md#redeploy-when-deployment-fails).

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneliğe taşıma

Kaynak grubunda başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için [kaynakları taşıma](./manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>Kilit kaynak grupları

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

Kaynak grubu silindi için aşağıdaki betiği bir kaynak grubu kilitler.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Aşağıdaki komut dosyası için bir kaynak grubu tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Aşağıdaki betiği bir kilit siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Etiket kaynak grupları

Varlıklarınızı mantıksal olarak düzenlemek için kaynak grupları ve kaynaklar için etiketler ekleyebilirsiniz. Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Kaynak grupları için şablonları dışarı aktarma

Kaynak grubunuz başarıyla ayarladıktan sonra kaynak grubu için Resource Manager şablonu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarma iki avantajı sunar:

- Şablon tüm eksiksiz altyapı içerdiğinden çözümün gelecekteki dağıtımlar otomatikleştirin.
- Şablon söz dizimi, JavaScript nesne gösterimi (çözümünüzü temsil eden JSON konumunda) bakarak öğrenin.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Komut dosyası şablonu konsolda görüntüler.  JSON dosyasını kopyalayın ve bir dosya olarak kaydedin.

Daha fazla bilgi için [şablonu Azure portalında tek ve birden çok kaynak dışarı aktarma](./export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure CLI kullanarak erişimini yönetme](../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).