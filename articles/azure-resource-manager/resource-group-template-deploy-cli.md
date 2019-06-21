---
title: Azure CLI ve şablon kaynakları dağıtma | Microsoft Docs
description: Kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure CLI kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: tomfitz
ms.openlocfilehash: 11d5b174dc21392df89def8e91847e8a0dd12562
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206533"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma

Bu makalede, Azure CLI'yı Resource Manager şablonları ile kaynakları Azure'a dağıtmak için nasıl kullanılacağını açıklanmaktadır. Dağıtma ile ilgili kavramları hakkında bilgi sahibi değilseniz ve Azure çözümlerinizi bkz [Azure Resource Manager'a genel bakış](resource-group-overview.md).  

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Azure CLI'yı yoksa, kullanabileceğiniz [Cloud Shell](#deploy-template-from-cloud-shell).

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınız için bir Azure aboneliğine veya abonelik içinde bir kaynak grubu hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubuna dağıtım hedefi. İlkeleri ve rol atamalarını abonelik üzerinden uygulamak için abonelik dağıtımları'ı kullanın. Abonelik dağıtımları da bir kaynak grubu oluşturun ve kaynakları dağıtmak için kullanın. Dağıtım kapsamını bağlı olarak, farklı komutlarını kullanın.

Dağıtmak için bir **kaynak grubu**, kullanın [az grubu dağıtım oluşturma](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create):

```azurecli
az group deployment create --resource-group <resource-group-name> --template-file <path-to-template>
```

Dağıtmak için bir **abonelik**, kullanın [az dağıtım oluşturma](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create):

```azurecli
az deployment create --location <location> --template-file <path-to-template>
```

Şu anda, yönetim grubu dağıtımları yalnızca REST API aracılığıyla desteklenir. Bkz: [kaynakları Resource Manager şablonları ve Resource Manager REST API'si ile dağıtma](resource-group-template-deploy-rest.md).

Bu makaledeki örneklerde, kaynak grubu dağıtımı kullanın. Abonelik dağıtımları hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](deploy-to-subscription.md).

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Kaynakları Azure'a dağıtırken:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. Bu, en fazla 90 karakter olabilir. Bu nokta ile bitemez.
3. Kaynak grubunu oluşturmak için gereken kaynakları tanımlayan şablonu dağıtma

Şablon dağıtımı özelleştirmenize olanak sağlayan parametreler ekleyebilir. Örneğin, belirli bir ortama (örneğin, geliştirme, test ve üretim gibi) için uygun değerler sağlayabilirsiniz. Örnek şablonu, depolama hesabı SKU'su için parametre tanımlar. 

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren bir ileti görürsünüz:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

Resource Manager şablonları, yerel makinenizde depolamak yerine dış bir konumda depolanması tercih edebilirsiniz. Şablonları bir kaynak denetim deposu (örneğin GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabında kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için **URI şablonu** parametresi. URI örnekte, github'dan örnek şablonu dağıtmak için kullanın.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

Yukarıdaki örnekte genel olarak erişilebilen bir URI şablonu için çoğu senaryo için şablonunuzu hassas verileri eklememelisiniz çünkü düşünülerek gerektirir. Hassas verileri (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Cloud Shell'de aşağıdaki komutları kullanın:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik olarak da bilinir *hatada geri alma*. Bir dağıtım başarısız olduğunda, dağıtım geçmişinden eski, başarılı bir dağıtım otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtım belirtmek için kullanın `--rollback-on-error` dağıtım komut parametresi. Bu işlevsellik, altyapı dağıtımınız için iyi bilinen bir duruma var ve bu duruma geri dönmek istiyorsanız kullanışlıdır. Uyarılar ve kısıtlamaları vardır:

- Yeniden dağıtma işlemi ile aynı parametreleri daha önce tam olarak çalıştırıldığı olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Kullanarak önceki dağıtım çalıştırma [tam modda](./deployment-modes.md#complete-mode). Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve herhangi bir kaynak yapılandırmaları önceki durumlarına ayarlanır. Tam olarak anladığınızdan emin olun [dağıtım modları](./deployment-modes.md).
- Yeniden dağıtma işlemi, yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özellik yalnızca kaynak grubu dağıtımlarında, abonelik düzeyinde dağıtımlar desteklenir. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](./deploy-to-subscription.md).

Bu seçeneği kullanmak için dağıtımlarınızı geçmişinde tanımlanan şekilde benzersiz adları olmalıdır. Benzersiz adlara sahip değilseniz, geçerli başarısız dağıtım geçmişini daha önce başarılı dağıtım üzerine yazılabilir. Bu gibi durumlarda, bu seçenek yalnızca kök düzey dağıtımlar kullanabilirsiniz. İç içe geçmiş şablon dağıtımları, yeniden dağıtım için kullanılamaz.

Son başarılı dağıtımı yeniden ekleyin `--rollback-on-error` parametre olarak bir bayrak.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error
```

Belirli bir dağıtımı yeniden dağıtmak için kullanın `--rollback-on-error` parametresi ve dağıtımın adını belirtin.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment02 \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS \
  --rollback-on-error ExampleDeployment01
```

Belirtilen dağıtım başarılı gerekir.

## <a name="parameters"></a>Parametreler

Parametre değerlerini geçirmek için satır içi parametre ya da bir parametre dosyası kullanabilirsiniz. Bu makalede Yukarıdaki örneklerde, satır içi parametreleri göster.

### <a name="inline-parameters"></a>Satır içi parametreleri

Satır içi parametreleri için değerler sağlayın. `parameters`. Örneğin, bir dize ve dizi için bir şablon geçirilecek bir Bash kabuğudur, kullanın:

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Ayrıca, dosya içeriğini Al ve bu içeriği satır içi parametresi olarak sağlayın.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Bir parametre değeri bir dosyadan alınırken, yapılandırma değerlerini sağlamak ihtiyacınız olduğunda yararlıdır. Örneğin, sağlayabilir [Linux sanal makinesi için cloud-init değerleri](../virtual-machines/linux/using-cloud-init.md).

ArrayContent.json biçimi şu şekildedir:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Parametre dosyaları

Betiğinizde değerleri satır içi olarak parametre geçirme yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolay. Parametre dosyasını yerel bir dosya olmalıdır. Dış parametre dosyaları, Azure CLI ile desteklenmez.

Parametre dosyasını şu biçimde olmalıdır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Parametreler bölümü (storageAccountType), şablonunuzda tanımlanan parametre eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyasını, parametre için bir değer içerir. Bu değer, şablona dağıtım sırasında otomatik olarak geçirilir. Birden fazla parametre dosyası oluşturun ve ardından uygun bir parametre dosyası senaryosu için geçirin. 

Önceki örneği kopyalayabilir ve adlı bir dosya kaydedin `storage.parameters.json`.

Bir yerel parametre dosyası geçirmek için kullanmak `@` storage.parameters.json adlı yerel bir dosya belirtmek için.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

### <a name="parameter-precedence"></a>Parametre önceliği

Satır içi parametreleri ve aynı dağıtım işlemi yerel parametre dosyasında kullanabilirsiniz. Örneğin, yerel dosyada bazı değerler belirtin ve diğer değerleri satır içi dağıtımı sırasında ekleyin. Hem yerel parametre dosyasında hem de satır içi parametre değerlerini sağlayın, satır içi değeri önceliklidir.

```azurecli
az group deployment create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters @demotemplate.parameters.json \
  --parameters exampleArray=@arrtest.json
```

## <a name="test-a-template-deployment"></a>Şablon dağıtımı test etme

Şablonu ve parametre değerleriniz tüm kaynakları gerçekten dağıtmadan test etmek için [az grubu dağıtımını doğrula](/cli/azure/group/deployment#az-group-deployment-validate). 

```azurecli-interactive
az group deployment validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Hata algılanırsa, komut test dağıtım hakkında bilgi döndürür. Özellikle dikkat **hata** değeri NULL'dur.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Hata algılanırsa, komutu bir hata iletisi döndürür. Örneğin, depolama hesabının SKU, yanlış bir değere geçirme şu hatayı döndürür:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Komut, şablonunuzun söz dizimi hatası varsa, şablon ayrıştırılamadı belirten bir hata döndürür. İleti satır numarasını ve ayrıştırma hatası konumunu gösterir.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Bu makaledeki örneklerde, varsayılan aboneliğinizde bir kaynak grubunda kaynak dağıtın. Farklı bir aboneliği kullanmak için bkz: [birden çok Azure aboneliklerini yönetme](/cli/azure/manage-azure-subscriptions-azure-cli).
- Kaynak grubunda var, ancak şablonunda tanımlanmayan kaynakları nasıl ele alınacağını belirtmek için bkz: [Azure Resource Manager dağıtım modları](deployment-modes.md).
- Şablonunuzda parametreleri tanımlayan anlamak için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
- Sık karşılaşılan dağıtım hataları çözümleme hakkında daha fazla ipucu için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
- Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-cli-sas-token.md).
- Güvenli bir şekilde, bir hizmetin ölçeğini birden fazla bölgeye toplamak için bkz: [Azure Deployment Manager](deployment-manager-overview.md).
