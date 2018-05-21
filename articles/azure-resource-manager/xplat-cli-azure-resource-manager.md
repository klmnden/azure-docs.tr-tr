---
title: Azure CLI ile kaynakları yönetme | Microsoft Docs
description: Azure kaynak ve grupları yönetmek için Azure komut satırı arabirimi (CLI) kullanın
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
ms.openlocfilehash: 90dd1b6b7e65178f6b339e4ac0bb781fb74a25a6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Azure kaynakları ve kaynak gruplarını yönetmek için Azure CLI kullanma

Bu makalede, Azure CLI ve Azure Resource Manager çözümlerinizi yönetmek öğrenin. Resource Manager ile bilmiyorsanız bkz [Resource Manager'a genel bakış](resource-group-overview.md). Bu makalede, yönetim görevlerine odaklanır. Yapacaklarınız:

1. Kaynak grubu oluşturma
2. Kaynak kaynak grubuna ekleyin
3. Bir etiket için kaynak ekleme
4. Adlarını veya etiket değerlerine göre kaynaklarını sorgulama
5. Uygulama ve kaynak üzerinde bir kilit kaldırın
6. Bir kaynak grubu Sil

Bu makalede, Resource Manager şablonu aboneliğinize dağıtma göstermez. Bu bilgi için bkz: [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](resource-group-template-deploy-cli.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yükleyip CLI yerel olarak kullanmak için bkz: [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

## <a name="set-subscription"></a>Set abonelik

Birden fazla aboneliğiniz varsa, farklı bir aboneliğe geçebilirsiniz. İlk olarak, hesabınız için tüm abonelikleri görelim.

```azurecli-interactive
az account list
```

Etkin ve devre dışı Aboneliklerin listesini döndürür.

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

Bir abonelik varsayılan olarak işaretlenmiş dikkat edin. Bu abonelik işlemleri için geçerli bağlamı ' dir. Farklı bir aboneliğe geçiş yapmak için abonelik ile ad **az hesabı kümesi** komutu.

```azurecli-interactive
az account set -s "Example Subscription Two"
```

Geçerli abonelik bağlamına göstermek için kullanın **az hesabı Göster** bir parametre olmadan:

```azurecli-interactive
az account show
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynağa aboneliğinize dağıtmadan önce kaynakları içeren bir kaynak grubu oluşturmanız gerekir.

Kaynak grubu oluşturmak için **az group create** komutunu kullanın. Komut kullanır **adı** parametresini kullanarak kaynak grubu için bir ad belirtin ve **konumu** konumunu belirtmek için parametre.

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

Kaynak grubu daha sonra alması gerekiyorsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az group show --name TestRG1
```

Tüm kaynak grupları, aboneliğinizde almak için kullanın:

```azurecli-interactive
az group list
```

## <a name="add-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna Ekle

Bir kaynak kaynak grubuna eklemek için kullanabileceğiniz **az kaynak oluşturmak** veya oluşturmakta kaynak türü için belirli bir komutunu (gibi **az depolama hesabı oluşturma**). Yeni kaynak için gerekli olan özellikler için parametreler içerdiğinden, bir kaynak türü için belirli bir komut kullanmayı daha kolay bulabilirsiniz. Kullanılacak **az kaynak oluşturma**, bunlar için istenmeden ayarlanacak tüm özellikler bilmeniz gerekir.

Ancak, yeni kaynak bir Resource Manager şablonu mevcut olmadığından kaynak komut dosyası aracılığıyla ekleme gelecekteki karışıklığa neden olabilir. Şablonlar, güvenilir ve art arda çözümünüzü dağıtmak etkinleştirir.

Aşağıdaki komut bir depolama hesabı oluşturur. Örnekte gösterilen adı kullanmak yerine, depolama hesabı için benzersiz bir ad sağlayın. Ad uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca rakamlar ve küçük harfler kullanın. Örnekte gösterilen adı kullanırsanız, bu ad zaten kullanımda olduğundan, bir hata alırsınız.

```azurecli-interactive
az storage account create -n myuniquestorage -g TestRG1 -l westus --sku Standard_LRS
```

Daha sonra bu kaynak alması gerekiyorsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az storage account show --name myuniquestorage --resource-group TestRG1
```

## <a name="add-a-tag"></a>Etiket ekleme

Etiketler kaynaklarınızı farklı özelliklere göre düzenlemek etkinleştirin. Örneğin, bazı kaynaklar aynı departmana ait farklı kaynak gruplarında olabilir. Aynı kategoriye ait olarak işaretlemek için bu kaynakları departmanı etiketini ve değer uygulayabilirsiniz. Veya, bir kaynak bir üretim veya test ortamında kullanılıp kullanılmadığını işaretleyebilirsiniz. Bu makalede, yalnızca bir kaynak etiketleri geçerli, ancak ortamınızda büyük olasılıkla tüm kaynaklarınıza etiketler uygulamak için anlamlıdır.

Aşağıdaki komutu, depolama hesabınıza iki etiketleri uygular:

```azurecli-interactive
az resource tag --tags Dept=IT Environment=Test -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

Etiketler tek bir nesne olarak güncelleştirilir. Etiketleri içeren bir kaynak için bir etiket eklemek için önce varolan etiketleri alın. Yeni etiket varolan etiketleri içeren nesneyi eklemek ve tüm etiketleri kaynağa yeniden uygulayın.

```azurecli-interactive
jsonrtag=$(az resource show -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g TestRG1 -n myuniquestorage --resource-type "Microsoft.Storage/storageAccounts"
```

## <a name="search-for-resources"></a>Kaynakları Ara

Kullanım **az kaynak listesi** farklı arama koşulu için kaynakları almak için komutu.

* Bir kaynak tarafından adını almak için sağlamanız **adı** parametre:

  ```azurecli-interactive
  az resource list -n myuniquestorage
  ```

* Tüm kaynaklar bir kaynak grubunda almak için sağlayın **kaynak grubu** parametre:

  ```azurecli-interactive
  az resource list --resource-group TestRG1
  ```

* Bir etiket adı ve değeri içeren tüm kaynakları almak için sağlayan **etiketi** parametre:

  ```azurecli-interactive
  az resource list --tag Dept=IT
  ```

* Belirli bir kaynak türü ile tüm kaynaklar sağlar **kaynak türü** parametre:

  ```azurecli-interactive
  az resource list --resource-type "Microsoft.Storage/storageAccounts"
  ```

## <a name="get-resource-id"></a>Kaynak Kimliği alma

Komutların çoğu kaynak kimliği, parametre olarak alın. Bir kaynak ve bir değişkeni deposunda Kimliği almak için kullanın:

```azurecli-interactive
webappID=$(az resource show -g exampleGroup -n exampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
```

## <a name="lock-a-resource"></a>Bir kaynak Kilitle

Kritik bir kaynak yanlışlıkla silinmiş veya değiştirilmiş emin olmak gerektiğinde bir kilidi kaynağı uygulayın. Ya da belirtebilirsiniz bir **CanNotDelete** veya **salt okunur**.

Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik roller bu eylemleri yalnızca sahip ve kullanıcı erişimi Yöneticisi verilir.

Kilit uygulamak için aşağıdaki komutu kullanın:

```azurecli-interactive
az lock create --lock-type CanNotDelete --resource-name myuniquestorage --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --name storagelock
```

Önceki örnekte kilitli kaynak kilit kaldırılana kadar silinemez. Kilidi kaldırmak için kullanın:

```azurecli-interactive
az lock delete --name storagelock --resource-group TestRG1 --resource-type Microsoft.Storage/storageAccounts --resource-name myuniquestorage
```

Ayar kilitleri hakkında daha fazla bilgi için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Kaynakları veya kaynak grubunu Kaldır
Bir kaynak veya kaynak grubu kaldırabilirsiniz. Bir kaynak grubu kaldırdığınızda, bu kaynak grubu içindeki tüm kaynaklar da kaldırın.

* Bir kaynak kaynak grubundan silmek için silmekte olduğunuz kaynak türü için silme komutunu kullanın. Komut kaynak siler, ancak kaynak grubu silmez.

  ```azurecli-interactive
  az storage account delete -n myuniquestorage -g TestRG1
  ```

* Bir kaynak grubu ve tüm kaynaklarını silmek için kullanın **az grubu Sil** komutu.

  ```azurecli-interactive
  az group delete -n TestRG1
  ```

Her iki komutlarında kaynağı veya kaynak grubunu kaldırmak istediğinizden onaylamanız istenir.

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablonları dağıtma hakkında bilgi edinmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy-cli.md).
* Yeni bir kaynak grubu mevcut kaynakları taşıyabilirsiniz. Örnekler için bkz: [yeni kaynak grubuna veya aboneliğe taşıma kaynaklara](resource-group-move-resources.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).