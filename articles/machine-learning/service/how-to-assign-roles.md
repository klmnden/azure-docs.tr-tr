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
ms.openlocfilehash: f6c74da2e9189c5dddb88cb80f04422f49cfaee2
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594262"
---
# <a name="manage-users-and-roles-in-an-azure-machine-learning-workspace"></a>Kullanıcıları ve rolleri bir Azure Machine Learning çalışma alanı yönetme

Bu makalede, bir Azure Machine Learning çalışma alanına kullanıcı ekleme ve bunları ile farklı roller atama öğrenin. Ayrıca özel roller oluşturmayı öğrenin.

## <a name="built-in-roles"></a>Yerleşik roller
Azure Machine Learning çalışma alanı, bir Azure kaynağıdır. Ve yeni bir Azure Machine Learning çalışma alanı oluşturulduğunda, tüm Azure kaynakları gibi üç varsayılan rol ile birlikte gelir. Çalışma alanına kullanıcı ekleme ve bunları bu yerleşik rollerden birine atayın.

- **Okuyucu**
    
    Bu rolü çalışma alanında salt okunur Eylemler sağlar. Bu role sahip listesi alabilirsiniz ve işlem hatları, vb. görünümü varlıklar denemeleri, çalıştırmaları, bilgi işlem, modeller, web Hizmetleri gibi bir çalışma alanında yayımladınız. Ancak, bu varlıkları güncelle izin verilmez.

- **Katılımcı**
    
    Bu rol, görüntülemek, oluşturmak, düzenlemek veya (uygunsa) silmek kullanıcılara bir çalışma alanında varlıklar. Örneğin, bir deneme oluşturma, oluşturabilir veya işlem kümesi eklemek bu role sahip bir model çalışma, bir kayıt gönderin ve bir web hizmetini dağıtma.

- **Sahip**
    
    Bu rol, görüntülemek, oluşturmak, düzenlemek veya (uygunsa) silmek izin verme çalışma alanına tam erişim sağlayan bir çalışma alanında varlıklar. Ayrıca, aynı zamanda ekleyin veya kullanıcıların kaldırabileceğiniz ve rol atamalarını değiştirme.

## <a name="add-or-remove-users"></a>Kullanıcı eklemek veya kaldırmak
Bir çalışma alanı sahibi, kullanıcılara ekleyebilir veya kullanıcılar, bu eylemi gerçekleştirmek için aşağıdaki yöntemlerden herhangi birini seçerek çalışma alanından kaldırın:
- [Azure portalı kullanıcı Arabirimi](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager şablonlarını](/azure/role-based-access-control/role-assignments-template).

Alternatif olarak, yüklediyseniz [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), çalışma alanına kullanıcı eklemek için kullanışlı bir CLI komutunu kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -n <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

Lütfen unutmayın `user` aynı Azure Active Directory'de Azure aboneliği çalışma alanının üst nerede yaşıyor mevcut bir kullanıcının e-posta adresi bir alandır. Bu komutu kullanmak nasıl bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -n my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluştur
Yerleşik roller gereksinimleriniz için yeterli değil, ayrıca özel roller oluşturabilirsiniz. Özel roller bulunabilir okuma, yazma veya çalışma alanını ve bu çalışma alanındaki işlem kaynağı izinlerini silme. Ve rol kullanılabilir belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde yapabilirsiniz. 

> [!NOTE]
> Bu kaynak içinde özel roller oluşturma için o düzeyde kaynak sahibi olmanız gerekir.

Özel bir rol oluşturmak için ilk izni ve rol için istediğiniz kapsam belirten bir rol tanımı JSON dosyasını oluşturun. Örneğin, belirli bir çalışma alanı düzeyinde kapsama alınan "veri Bilimcisi" adlı özel bir rol için bir rol tanımı dosyası aşağıdadır.

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

Bu rolü, aşağıdaki eylemler dışında çalışma alanındaki her şeyi sağlar:
- Oluşturun veya bir işlem kaynağı güncelleştirin.
- Bir işlem kaynağı silemezsiniz.
- Ekleyemezsiniz, kullanıcı silme veya herhangi bir kullanıcının rol atamalarını değiştirme.
- Çalışma alanı silemezsiniz.

Bu özel rolü dağıtmak için aşağıdaki Azure CLI komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Bu rol dağıtıldıktan sonra belirtilen çalışma alanında kullanılabilir hale gelir. Artık kullanıcı ekleme ve Azure portalında bu rolü atamanız. Bu rolü kullanarak ile kullanıcı ekleyebilirsiniz `az ml workspace share` CLI komutunu:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Ayrıca `AssignableScopes` bu özel rolü kapsamını abonelik düzeyinde kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde (gösterildiği gibi) belirlemek için alan.

Nasıl özel roller hakkında daha fazla bilgi için Azure'da çalışma, lütfen [bu belgeyi](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Sonraki adımlar

Bir çalışma alanı oluşturmak, eğitmek ve modeller Azure Machine Learning hizmeti ile dağıtmak için nasıl kullanılacağını öğrenmek için eksiksiz bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)
