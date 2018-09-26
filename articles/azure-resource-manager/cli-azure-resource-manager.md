---
title: Azure CLI ile kaynakları yönetme | Microsoft Docs
description: Azure kaynaklarını ve grupları yönetmek için Azure komut satırı arabirimi (CLI) kullanın
editor: ''
manager: timlt
documentationcenter: ''
author: tfitzmac
services: azure-resource-manager
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: dd111c33cbd348a05ed0f0c04f7325347612e54d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47107045"
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Azure kaynaklarını ve kaynak gruplarını yönetmek için Azure CLI kullanma

Bu makalede, çözümlerinizi Azure CLI ve Azure Resource Manager ile yönetme konusunda bilgi edinin. Resource Manager ile ilgili bilgi sahibi değilseniz bkz [Resource Manager'a genel bakış](resource-group-overview.md). Bu makalede, yönetim görevleri üzerinde odaklanır. Yapacaklarınız:

1. Kaynak grubu oluşturma
2. Bir kaynak kaynak grubuna ekleyin.
3. Kaynağa etiket ekleme
4. Adları veya etiket değerlerini temel alan kaynaklarını sorgulama
5. Uygula ve bir kaynak Kilidi Kaldır
6. Bir kaynak grubunu sil

Bu makalede, Resource Manager şablonu aboneliğinize dağıtmaya nasıl algılanacağını göstermez. Bu bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı için bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="set-subscription"></a>Abonelik kümesi

Birden fazla aboneliğiniz varsa, farklı bir aboneliğe geçebilirsiniz. İlk olarak, hesabınız için tüm abonelikleri görelim.

```azurecli-interactive
az account list
```

Etkin ve devre dışı aboneliklerinizin bir listesini döndürür.

```json
[
  {
    "cloudName": "AzureCloud",
    "id": "<guid>",
    "isDefault": true,
    "name": "Example Subscription One",
    "registeredProviders": [],
    "state": "Enabled",
    "tenantId": "<guid>",
    "user": {
      "name": "example@contoso.org",
      "type": "user"
    }
  },
  ...
]
```

Varsayılan olarak bir abonelik işaretlendiğine dikkat edin. Bu işlem için geçerli Bağlamınızı aboneliktir. Abonelik adı ile farklı bir aboneliğe geçmek için sağlamak **az hesabı kümesi** komutu.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Geçerli abonelik bağlamını göstermek için kullanın **az hesabı show** parametre olmadan:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm kaynakları aboneliğinize dağıtmadan önce kaynakları içerecek bir kaynak grubu oluşturmanız gerekir.

Kaynak grubu oluşturmak için **az group create** komutunu kullanın. Komut **adı** parametresini kullanarak kaynak grubu için bir ad belirtin ve **konumu** konumunu belirtmek için parametre.

```azurecli-interactive
az group create --name TestRG1 --location "South Central US"
```

Çıktı aşağıdaki biçimdedir:

```json
{
  "id": "/subscriptions/<subscription-id>/resourceGroups/TestRG1",
  "location": "southcentralus",
  "managedBy": null,
  "name": "TestRG1",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

Daha sonra kaynak grubunu almanız gerekiyorsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az group show --name TestRG1
```

Aboneliğinizdeki tüm kaynak gruplarını almak için kullanın:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna ekleyin.

Bir kaynak grubuna kaynak eklemek için kullanabileceğiniz **az kaynak oluşturma** komut veya oluşturduğunuz bir kaynak türü için belirli bir komut (gibi **az depolama hesabı oluşturma**). Yeni kaynak için gerekli olan özellikler için parametreler içerdiğinden kaynak türü için belirli bir komut kullanmayı daha kolay bulabilirsiniz. Kullanılacak **az kaynak oluşturma**, bunlar için istenmeden ayarlanacak tüm özellikler bilmeniz gerekir.

Ancak, yeni kaynak Resource Manager şablonunda var olmadığından kaynak betiği aracılığıyla ekleme gelecekteki karışıklığa neden olabilir. Şablonları çözümünüzü güvenilir ve sürekli olarak dağıtmanızı sağlar.

Aşağıdaki komut bir depolama hesabı oluşturur. Örnekte gösterilen adı kullanmak yerine, depolama hesabı için benzersiz bir ad belirtin. Ad 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlardan ve küçük harflerden oluşmalıdır. Örnekte gösterilen adı kullanırsanız, bu ad zaten kullanımda olduğundan, bir hata alırsınız.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Daha sonra bu kaynak almanız gerekiyorsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Etiket Ekle

Etiketleri kaynaklarınızı farklı özelliklere göre düzenlemenizi sağlar. Örneğin, çeşitli kaynaklar aynı departmana ait farklı kaynak gruplarında olabilir. Department etiketi ve değerini aynı kategoriye ait olarak işaretlemek için bu kaynaklara uygulayabilirsiniz. Veya, bir kaynak bir üretim ya da test ortamında kullanılıp kullanılmayacağını işaretleyebilirsiniz. Bu makalede, yalnızca bir kaynak için etiketler, ancak ortamınızda büyük olasılıkla tüm kaynaklarınıza etiketler mantıklıdır.

Aşağıdaki komutu, depolama hesabınıza iki etiket uygular:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

Etiketler, tek bir nesne olarak güncelleştirilir. Etiketler içeren bir kaynak için bir etiket eklemek için önce mevcut etiketleri alın. Mevcut etiketler içeren nesneye yeni etiketi ekleyin ve kaynağa tüm etiketleri yeniden uygulayın.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Kaynak Ara

Kullanım **az kaynak listesi** farklı arama koşulları için kaynakları almak için komutu.

* Ada göre kaynak almak için sağlamak **adı** parametresi:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Bir kaynak grubundaki tüm kaynakları almak için sağlamak **resource-group** parametresi:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Etiket adını ve değerini sahip tüm kaynakları almak için sağlayın **etiketi** parametresi:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Belirli bir kaynak türü ile tüm kaynakları sağlamak **kaynak türü** parametresi:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="get-resource-id"></a>Kaynak Kimliğini alın

Birçok komuta bir parametre olarak bir kaynak kimliği alın. Kaynak ve bir değişkene deposunda Kimliği almak için kullanın:

```azurecli-interactive
webappID=$(az resource show -g exampleGroup -n exampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
```

## <a name="lock-a-resource"></a>Bir kaynağı Kilitle

Kritik bir kaynak yanlışlıkla silinmiş veya değiştirilmiş emin olmanız gerekir, kilit kaynağına uygulayın. Belirtebilirsiniz bir **CanNotDelete** veya **salt okunur**.

Yönetim kilitlerini Sil ya da oluşturmak için erişimi olmalıdır. `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik rolleri, yalnızca sahip ve kullanıcı erişimi Yöneticisi, bu eylemlerin verilir.

Kilit uygulamak için aşağıdaki komutu kullanın:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

Önceki örnekte kilitli kaynak kilidi kaldırılana kadar silinemez. Kilidi kaldırmak için kullanın:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Ayar kilitleri hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Kaynaklar veya kaynak grubunu kaldırma
Bir kaynak veya kaynak grubunu kaldırabilirsiniz. Bir kaynak grubu kaldırdığınızda, o kaynak grubunun içindeki tüm kaynaklar da kaldırın.

* Kaynak grubundan kaynak silmek için delete komutu silmekte olduğunuz kaynak türü için kullanın. Komut kaynak siler ancak kaynak grubunu silmez.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* Kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için **az group delete** komutunu kullanın.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Her iki komutu için kaynak veya kaynak grubunu kaldırmak istediğinizden onaylamanız istenir.

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablon dağıtma hakkında bilgi edinmek için [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy-cli.md).
* Var olan kaynakları yeni kaynak grubuna taşıyabilirsiniz. Örnekler için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).