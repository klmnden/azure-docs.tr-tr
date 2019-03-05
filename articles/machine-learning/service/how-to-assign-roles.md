---
title: Kullanıcıları ve rolleri bir Azure Machine Learning çalışma alanı yönetme
titleSuffix: Azure Machine Learning service
description: Kullanıcıları ve rolleri bir Azure Machine Learning hizmeti çalışma alanında yönetmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: hning86
ms.author: haining
ms.date: 2/20/2019
ms.custom: seodec18
ms.openlocfilehash: 623aaff3cba86e8e523a86e4adcb0a359c339c45
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57336473"
---
# <a name="manage-users-and-roles-in-an-azure-machine-learning-workspace"></a>Kullanıcıları ve rolleri bir Azure Machine Learning çalışma alanı yönetme

Bu makalede, bir Azure Machine Learning çalışma alanına kullanıcı ekleme öğrenin. Ayrıca farklı rollere kullanıcılar atamanız ve özel roller oluşturma konusunda bilgi edinin.

## <a name="built-in-roles"></a>Yerleşik roller
Bir Azure Machine Learning çalışma alanı, bir Azure kaynağıdır. Yeni bir Azure Machine Learning çalışma alanı oluşturulduğunda, diğer Azure kaynakları gibi üç varsayılan rol ile gelir. Çalışma alanına kullanıcı ekleme ve bunları bu yerleşik rollerden birine atayın.

- **Okuyucu**
    
    Bu rolü çalışma alanında salt okunur Eylemler sağlar. Okuyucular listelemek ve varlıklar bir çalışma alanında görüntülemek, ancak oluşturabilir veya bu varlıkları güncelleştirin.

- **Katılımcı**
    
    Bu rol, görüntülemek, oluşturmak, düzenlemek veya (uygunsa) silmek kullanıcılara bir çalışma alanında varlıklar. Örneğin, Katkıda Bulunanlar bir deneme oluşturma, oluşturmak veya işlem kümesi ekleme, çalıştırma gönderin ve bir web hizmetini dağıtma.

- **Sahip**
    
    Bu rol kullanıcılara tam erişim görüntülemek, oluşturmak, düzenlemek veya (uygunsa) silme yeteneği dahil olmak üzere çalışma alanına sağlar. bir çalışma alanında varlıklar. Ayrıca, ekleme veya kullanıcıları kaldırmak ve rol atamalarını değiştirme.

## <a name="add-or-remove-users"></a>Kullanıcı eklemek veya kaldırmak
Bir çalışma alanının sahibi değilseniz, ekleyin ve kullanıcılar, aşağıdaki yöntemlerden birini seçerek çalışma alanından kaldırın:
- [Azure portalı kullanıcı Arabirimi](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager şablonları](/azure/role-based-access-control/role-assignments-template)

Yüklediyseniz [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), çalışma alanına kullanıcı eklemek için bir CLI komutunu kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -n <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

`user` Çalışma üst abonelik burada yer alan Azure Active Directory örneğinde mevcut bir kullanıcının e-posta adresi bir alandır. Bu komutu kullanmak nasıl bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -n my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluştur
Yerleşik roller uzaksa, özel roller oluşturabilirsiniz. Özel roller olabilir okuma, yazma, silme ve bu çalışma alanında kaynak izinleri işlem. Rol kullanılabilir belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde yapabilirsiniz. 

> [!NOTE]
> Bu kaynak içinde özel roller oluşturmanızı da o düzeyde kaynak sahibi olmalıdır.

Özel bir rol oluşturmak için ilk olarak izin ve rol için istediğiniz kapsam belirten bir rol tanımı JSON dosyasını oluşturun. Örneğin, belirli bir çalışma alanı düzeyinde kapsama alınan "veri Bilimcisi" adlı özel bir rol için bir rol tanımı dosyası şu şekildedir:

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

Dağıtımdan sonra bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Şimdi ekleyin ve Azure portalında bu rolü atayın. Veya kullanarak bu role sahip bir kullanıcı ekleyebilirsiniz `az ml workspace share` CLI komutunu:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```


Azure'da özel roller hakkında daha fazla bilgi için bkz: [bu belgeyi](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Sonraki adımlar

Bir çalışma alanı oluşturmak, eğitmek ve modeller Azure Machine Learning hizmeti ile dağıtmak için nasıl kullanılacağını öğrenmek için eksiksiz bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)
