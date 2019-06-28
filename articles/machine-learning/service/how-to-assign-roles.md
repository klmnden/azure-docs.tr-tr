---
title: Bir Azure Machine Learning çalışma alanı rolleri yönetme
titleSuffix: Azure Machine Learning service
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure Machine Learning hizmeti çalışma alanına erişim öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 02/20/2019
ms.custom: seodec18
ms.openlocfilehash: e062fd73f2baeb4948430b13e0caa1f5c0b3f066
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341109"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Bir Azure Machine Learning çalışma alanı erişimi yönetme

Bu makalede, bir Azure Machine Learning çalışma alanına erişim yönetme konusunda bilgi edinin. [Rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Active Directory'de kullanıcıları kaynaklarına erişimi belirli rollere atanır. Azure, hem yerleşik roller hem de özel roller oluşturma olanağı sağlar.

## <a name="default-roles"></a>Varsayılan roller

Bir Azure Machine Learning çalışma alanı, bir Azure kaynağıdır. Yeni bir Azure Machine Learning çalışma alanı oluşturulduğunda, diğer Azure kaynakları gibi üç varsayılan rol ile gelir. Çalışma alanına kullanıcı ekleme ve bunları bu yerleşik rollerden birine atayın.

| Rol | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanındaki Eylemler salt okunur. Okuyucular listelemek ve varlıklar bir çalışma alanında görüntülemek, ancak oluşturabilir veya bu varlıkları güncelleştirin. |
| **Katılımcı** | (Uygunsa) Sil görüntülemek, oluşturmak, düzenlemek veya bir çalışma alanında varlıklar. Örneğin, Katkıda Bulunanlar bir deneme oluşturma, oluşturmak veya işlem kümesi ekleme, çalıştırma gönderin ve bir web hizmetini dağıtma. |
| **Sahip** | Tam erişim görüntülemek, oluşturmak, düzenlemek veya (uygunsa) silme yeteneği dahil olmak üzere çalışma alanında, bir çalışma alanında varlıklar. Ayrıca, rol atamalarını değiştirebilirsiniz. |

> [!IMPORTANT]
> Azure'da birden çok düzeyi için rol erişimini sınırlayabilirsiniz. Örneğin, bir çalışma alanı sahibi erişimi olan çalışma alanını içeren kaynak grubunu sahip erişimi olmayabilir. Daha fazla bilgi için [RBAC nasıl çalıştığını](/azure/role-based-access-control/overview#how-rbac-works).

Belirli yerleşik roller hakkında daha fazla bilgi için bkz. [Azure için yerleşik roller](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Çalışma alanı erişimi yönetme

Bir çalışma alanının sahibi değilseniz, ekleme ve çalışma alanı için rolleri kaldırın. Roller kullanıcılara da atayabilirsiniz. Nasıl erişimi yönetmek keşfetmek için aşağıdaki bağlantıları kullanın:
- [Azure portalı kullanıcı Arabirimi](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager şablonları](/azure/role-based-access-control/role-assignments-template)

Yüklediyseniz [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), roller kullanıcılara atamak için CLI komutunu kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

`user` Çalışma üst abonelik burada yer alan Azure Active Directory örneğinde mevcut bir kullanıcının e-posta adresi bir alandır. Bu komutu kullanmak nasıl bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluşturma

Yerleşik roller uzaksa, özel roller oluşturabilirsiniz. Özel roller olabilir okuma, yazma, silme ve bu çalışma alanında kaynak izinleri işlem. Rol kullanılabilir belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde yapabilirsiniz.

> [!NOTE]
> Bu kaynak içinde özel roller oluşturmanızı da o düzeyde kaynak sahibi olmalıdır.

Özel bir rol oluşturmak için ilk olarak izin ve rolü için kapsamı belirtir bir rol tanımı JSON dosyasını oluşturun. Aşağıdaki örnek, belirli bir çalışma alanı düzeyinde kapsama alınan "veri Bilimcisi" adlı özel bir rol tanımlar:

`data_scientist_role.json` :
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

Değiştirebileceğiniz `AssignableScopes` abonelik düzeyinde, kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde bu özel rolü kapsamını belirlemek için alan.

Bu özel rolü, aşağıdaki eylemler dışında çalışma alanındaki her şeyi yapabilirsiniz:

- Oluşturun veya bir işlem kaynağı güncelleştirin.
- Bir işlem kaynağı silemezsiniz.
- Ekleyemezsiniz, silin veya rol atamalarını değiştirme.
- Çalışma alanı silemezsiniz.

Bu özel rolü dağıtmak için aşağıdaki Azure CLI komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Dağıtımdan sonra bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Şimdi ekleyin ve Azure portalında bu rolü atayın. Veya, kullanarak bir kullanıcıya bu rolü atayabilirsiniz `az ml workspace share` CLI komutunu:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```


Daha fazla bilgi için [Azure kaynakları için özel roller](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Sonraki adımlar

- [Kurumsal Güvenliğe genel bakış](concept-enterprise-security.md)
- [Denemeleri ve sanal ağ içindeki çıkarımı/puanı güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
- [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)
